vueRouter
### 简单使用
```
<div id="app">
  <p>
    <!-- 使用 router-link 组件来导航 -->
    <!-- 通过传入 `to` 属性指定链接 -->
    <!-- <router-link> 默认会被渲染成一个 `<a>` 标签 -->
    <router-link to="/foo">Go to Foo</router-link>
    <router-link to="/bar">Go to Bar</router-link>
  </p>
  <!-- 路由出口 -->
  <!-- 路由匹配到的组件将渲染在这里 -->
  <router-view></router-view>
</div>
```
当使用路由参数时，例如从 /user/foo 导航到 /user/bar，原来的组件实例会被复用。因为两个路由都渲染同个组件，比起销毁再创建，复用则显得更加高效。不过，这也意味着组件的生命周期钩子不会再被调用。
复用组件时，想对路由参数的变化作出响应的话，你可以简单地 watch (监测变化) $route 对象：
```
const User = {
  template: '...',
  watch: {
    $route(to, from) {
      // 对路由变化作出响应...
    }
  }
}
```
或者使用beforeRouteUpdate导航守卫：
```
const User =  {
  template: '...',
  beforeRouteUpdate(to, form, next) {
    // react to route changes...
    // don't forget to call next()
  }
}
```
### 编程时导航
除了使用router-link创建a标签来定义导航链接之外，还可以借助router.push的实例方法，它们是相等的。   
`#router.push(location, onComplete?, onAbort?)`   
注意：在Vue实例内部，你可以通过$router访问路由实例。因此你可以调用 this.$router.push。
想要导航到不同的URL，则使用router.push方法。这个方法会向history栈添加一个新的记录，所以，当用户点击浏览器后退按钮时，则回到之前的URL
```
// 字符串
router.push('/user')
// 对象形式
router.push({path: '/user'})
// 命名路由
router.push({name: 'user', params: {id: '123'}})
// 带查询参数，变成 /register?plan=private
router.push({ path: 'register', query: { plan: 'private' }})
```
注意：如果提供了path，params会被忽略，上述例子中的query并不属于这种情况。取而代之的是下面例子的做法，需要提供路由的name或手写完整的带有参数的path
```
const userId = '123'
router.push({ name: 'user', params: { userId }}) // -> /user/123
router.push({ path: `/user/${userId}` }) // -> /user/123
// 这里的 params 不生效
router.push({ path: '/user', params: { userId }}) // -> /user
```
同样的规则也适用于 router-link 组件的 to 属性

router.replace(location, onComplete?, onAbort?)
跟 router.push 很像，唯一的不同就是，它不会向 history 添加新记录，而是跟它的方法名一样 —— 替换掉当前的history记录。

router.go(n)
这个方法的参数是一个整数，意思是在 history 记录中向前或者后退多少步，类似 window.history.go(n)
```
// 在浏览器记录中前进一步，等同于 history.forward()
router.go(1)

// 后退一步记录，等同于 history.back()
router.go(-1)

// 前进 3 步记录
router.go(3)

// 如果 history 记录不够用，那就默默地失败呗
router.go(-100)
router.go(100)
```

### 命名路由
通过名称来标识一个路由有时候更方便一些。你可以在创建 Router 实例的时候，在routes配置中给某个路由设置名称。
```
const router = new VueRouter({
  routes: [
    {
      path: '/user/:userId',
      name: 'user',
      component: User
    }
  ]
})
```

```
<router-link :to="{ name: 'user', params: { userId: 123 }}">User</router-link>

router.push({ name: 'user', params: { userId: 123 } })
```
这两种方式都会把路由导航到 /user/123 路径。

### 命名视图
有时候想同时展示多个视图，命名视图就派上用场了。可以在界面中拥有多个单独命名的视图，而不是只有一个单独的出口。如果 router-view 没有设置名字，那么默认为 default。
```
<router-view class="view one"></router-view>
<router-view class="view two" name="a"></router-view>
<router-view class="view three" name="b"></router-view>
```
一个视图使用一个组件渲染，因此对于同个路由，多个视图就需要多个组件。确保正确使用 components 配置 (带上 s)：
```
const router = new VueRouter({
  routes: [
    {
      path: '/',
      components: {
        default: Foo,
        a: Bar,
        b: Baz
      }
    }
  ]
})
```
### 路由组件传参
1. params
```
const User = {
  template: '<div>User {{ $route.params.id }}</div>'
}
const router = new VueRouter({
  routes: [{ path: '/user/:id', component: User }]
})
```
2. props
```
const User = {
  props: ['id'],
  template: '<div>User {{ id }}</div>'
}
const router = new VueRouter({
  routes: [
    { path: '/user/:id', component: User, props: true },

    // 对于包含命名视图的路由，你必须分别为每个命名视图添加 `props` 选项：
    {
      path: '/user/:id',
      components: { default: User, sidebar: Sidebar },
      props: { default: true, sidebar: false }
    }
  ]
})
```
+ 布尔模式
如果 props 被设置为 true，route.params将会被设置为组件属性。
+ 对象模式
如果 props 是一个对象，它会被按原样设置为组件属性。当props是静态的时候有用。
```
routes: [
    {
      path: '/promotion/from-newsletter',
      component: Promotion,
      props: { newsletterPopup: false }
    }
  ]
```
+ 函数模式
创建一个函数返回props,这样便可以将参数转换成另一种类型，将静态值与基于路由的值结合。
```
routes: [
  {
    path: '/search',
    component: SearchUser,
    props: route => ({ query: route.query.q })
  }
]
```
`URL /search?q=vue 会将 {query: 'vue'} 作为属性传递给 SearchUser 组件。`   
尽可能保持props函数为无状态的，因为它只会在路由发生变化时起作用
### HTML5 History 模式
vue-router默认hash模式 —— 使用 URL 的hash来模拟一个完整的URL，于是当URL改变时，页面不会重新加载。
如果不想要很丑的hash，可以用路由的history模式，这种模式充分利用history.pushState API来完成URL跳转而无须重新加载页面。
```
const router = new VueRouter({
  mode: 'history',
  routes: [...]
})
```
使用history模式时，URL就像正常的url。
不过这种模式需要后台配置支持。因为我们的应用是个单页客户端应用，如果后台没有正确的配置就会返回404。要在服务端增加一个覆盖所有情况的候选资源：如果URL匹配不到任何静态资源，则应该返回同一个index.html 页面，这个页面就是app依赖的页面。
### 导航守卫
vue-router提供的导航守卫主要用来通过跳转或取消的方式守卫导航。有多种机会植入路由导航过程中：全局的, 单个路由独享的, 或者组件级的。
参数或查询的改变并不会触发进入/离开的导航守卫。可以通过观察$route对象来应对这些变化，或使用beforeRouteUpdate 的组件内守卫。
1. 全局前置守卫   
使用router.beforeEach注册一个全局前置守卫：
```
const router = new VueRouter({ ... })
router.beforeEach((to, from, next) => {
  // ...
})
```
当一个导航触发时，全局前置守卫按照创建顺序调用。守卫是异步解析执行，此时导航在所有守卫resolve完之前一直处于等待中。
每个守卫方法接收三个参数：
+ to: Route: 即将要进入的目标 路由对象
+ from: Route: 当前导航正要离开的路由
+ next: Function: 一定要调用该方法来resolve这个钩子。执行效果依赖next方法的调用参数。
  next(): 进行管道中的下一个钩子。如果全部钩子执行完了，则导航的状态就是 confirmed (确认的)。   
  next(false): 中断当前的导航。如果浏览器的URL改变了(可能是用户手动或者浏览器后退按钮)，那么URL地址会重置到from路由对应的地址。   
  next('/') 或者 next({ path: '/' }): 跳转到一个不同的地址。当前的导航被中断，然后进行一个新的导航。你可以向 next 传递任意位置对象，且允许设置诸如 replace: true、name: 'home' 之类的选项以及任何用在router-link的to prop 或 router.push 中的选项。   
  next(error): 如果传入next的参数是一个Error实例，则导航会被终止且该错误会被传递给router.onError()注册过的回调。
  确保 next 函数在任何给定的导航守卫中都被严格调用一次。它可以出现多于一次，但是只能在所有的逻辑路径都不重叠的情况下，否则钩子永远都不会被解析或报错。这里有一个在用户未能验证身份时重定向到 /login 的示例：
```
router.beforeEach((to, from, next) => {
  if (to.name !== 'Login' && !isAuthenticated) next({ name: 'Login' })
  else next()
})
```
2. 全局解析守卫   
可以用router.beforeResolve注册一个全局守卫。这和 router.beforeEach 类似，区别是在导航被确认之前，同时在所有组件内守卫和异步路由组件被解析之后，解析守卫就被调用。

3. 全局后置钩子   
和守卫不同的是，这些钩子不会接受next函数也不会改变导航本身：
```
router.afterEach((to, from) => {
  // ...
})
```
4. 路由独享的守卫   
beforeEnter
```
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      beforeEnter: (to, from, next) => {}
    }
  ]
})
```
与全局前置守卫的方法参数是一样的。   
5. 组件内的守卫   
beforeRouteEnter   
beforeRouteUpdate   
beforeRouteLeave   
```
const Foo = {
  template: `...`,
  beforeRouteEnter(to, from, next) {
    // 在渲染该组件的对应路由被 confirm 前调用
    // 不！能！获取组件实例 `this`
    // 因为当守卫执行前，组件实例还没被创建
  },
  beforeRouteUpdate(to, from, next) {
    // 在当前路由改变，但是该组件被复用时调用
    // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
    // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
    // 可以访问组件实例 `this`
  },
  beforeRouteLeave(to, from, next) {
    // 导航离开该组件的对应路由时调用
    // 可以访问组件实例 `this`
  }
}
```
beforeRouteEnter守卫不能访问this，因为守卫在导航确认前被调用，因此即将登场的新组件还没被创建。   
不过，可以通过传一个回调给next来访问组件实例。在导航被确认的时候执行回调，并且把组件实例作为回调方法的参数。
```
beforeRouteEnter (to, from, next) {
  next(vm => {
    // 通过 `vm` 访问组件实例
  })
}
```
beforeRouteEnter是支持给next传递回调的唯一守卫。对于 beforeRouteUpdate 和 beforeRouteLeave 来说，this 已经可用了，所以不支持传递回调，因为没有必要了。

这个离开守卫通常用来禁止用户在还未保存修改前突然离开。该导航可以通过 next(false) 来取消。
```
beforeRouteLeave (to, from, next) {
  const answer = window.confirm('Do you really want to leave? you have unsaved changes!')
  if (answer) {
    next()
  } else {
    next(false)
  }
}
```
### 完整的导航解析流程
钩子函数种类有:全局守卫、路由守卫、组件守卫
1. 导航被触发。
2. 在失活的 组件里调用beforeRouteLeave守卫
3. 调用全局的beforeEach守卫
4. 在重用的组件里调用beforeRouteUpdate守卫
5. 在路由配置里调用beforeRouteEnter
6. 解析异步组件路由
7. 在被激活的组件里调用beforeRouteEnter
8. 调用全局的beforeResolve守卫
9. 导航被确认
10. 调用全局的afterEach钩子
11. 触发dom更新
12. 调用beforeRouteEnter守卫中传给next（）回调函数，创建好的组件实例会作为回调函数的参数传入

### 路由元信息
定义路由的时候可以配置 meta 字段：
```
{
  path: 'bar',
  component: Bar,
  // a meta field
  meta: { requiresAuth: true }
}
```
一个路由匹配到的所有路由记录会暴露为 $route 对象 (还有在导航守卫中的路由对象) 的 $route.matched数组。因此，需要遍历 $route.matched来检查路由记录中的meta字段。
```
router.beforeEach((to, from, next) => {
  if (to.matched.some(record => record.meta.requiresAuth)) {
    // this route requires auth, check if logged in
    // if not, redirect to login page.
    if (!auth.loggedIn()) {
      next({
        path: '/login',
        query: { redirect: to.fullPath }
      })
    } else {
      next()
    }
  } else {
    next() // 确保一定要调用 next()
  }
})
```
### 滚动行为
当切换到新路由时，想要页面滚到顶部，或者是保持原先的滚动位置，就像重新加载页面那样。 vue-router能做到，它让你可以自定义路由切换时页面如何滚动。
注意: 这个功能只在支持 history.pushState 的浏览器中可用。
当创建一个 Router 实例，你可以提供一个 scrollBehavior 方法：
```
const router = new VueRouter({
  routes: [...],
  scrollBehavior (to, from, savedPosition) {
    // return 期望滚动到哪个的位置
  }
})
```
scrollBehavior 方法接收 to 和 from 路由对象。第三个参数 savedPosition 当且仅当 popstate 导航 (通过浏览器的 前进/后退 按钮触发) 时才可用。
这个方法返回滚动位置的对象信息，长这样：
```
{ x: number, y: number }
{ selector: string, offset? : { x: number, y: number }}
如果返回一个falsy的值，或者是一个空对象，那么不会发生滚动。
```
举例：
```
scrollBehavior (to, from, savedPosition) {
  return { x: 0, y: 0 }
}
```
对于所有路由导航，简单地让页面滚动到顶部。
如果模拟“滚动到锚点”的行为：
```
scrollBehavior (to, from, savedPosition) {
  if (to.hash) {
    return {
      selector: to.hash
    }
  }
}
```
我们还可以利用路由元信息更细颗粒度地控制滚动