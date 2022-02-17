### 响应式原理
整体思路是数据劫持+观察者模式（发布者-订阅者模式）  
vue会遍历data选项中的所有属性，并使用object.defineProperty劫持属性的getter/setter,数组则是通过重写数组方法来实现。当页面使用对应属性时，每个属性都拥有自己的 dep 属性，存放他所依赖的 watcher（依赖收集），当属性变化后会通知自己对应的 watcher 去更新(派发更新)。

### -------nextTick原理
主要思路就是采用微任务优先的方式调用异步方法去执行 nextTick 包装的方法    

下次DOM更新循环结束之后执行延迟回调，再修改数据之后使用$nextTick,则可以在回调中使用更新后的dom，

Vue在更新DOM时是异步执行的。只要侦听到数据变化，Vue将开启1个队列，并缓冲在同一事件循环中发生的所有数据变更。如果同一个watcher被多次触发，只会被推入到队列中-次。这种在缓冲时去除重复数据对于避免不必要的计算和DOM操作是非常重要的。nextTick方法会在队列中加入一个回调函数，确保该函数在前面的dom操作完成后才调用；
nextTick主要使用了宏任务和微任务。 根据执行环境分别尝试采用Promise、MutationObserver、setImmediate，如果以上都不行则采用setTimeout定义了一个异步方法，多次调用nextTick会将方法存入队列中，通过这个异步方法清空当前队列

### 使用object.defineProperty()来进行数据挟持有什么缺点？
无法拦截对象的新增属性以及通过下标的方式修改数组。更精确的来说对于数组，大部分操作无法拦截到，只是vue内部通过重写函数的方式解决了这个问题。
vue3的proxy它就可以完美的监听到任何数据的改变。唯一的缺点就是兼容性的问题

### vue中封装了哪些数组，如何实现页面更新
+ push
+ pop
+ shift
+ unshift
+ splice
+ sort
+ reverse
重写了数组的原生方法，首先获取到数组的Observer对象，如果有新的值，就调用observeArray继续对新的值观察变化，然后手动调用notify，通知渲染watcher，执行update。


### 生命周期
+ create： 实例被创建
beforeCreate： 实例创建前，data和method中的数据还没有被初始化
created： 实例创建完成，data、计算属性、方法已经初始化， 未挂载到dom

+ mount阶段：实例被挂载到真实的dom节点
beforeMount：可以发起服务端请求，取数据，render 函数首次被调用
mounted: 实例挂载到DOM上，此时可以操作dom但不会保证所有的子组件也都被挂载完成，如果希望等待整个视图都渲染完毕，可以在mounted 内部使用 vm.$nextTick

+ update阶段： data数据变化时，触发组件的重新渲染
beforeUpdate: 数据发生改变后，DOM 被更新之前
updated：数据更改且DOM重新渲染之后
+ destroy阶段： 实例被销毁
beforeDestroy：实例销毁前，此时可以手动销毁一些方法
destroyed:销毁后

### 父子组件生命周期顺序
加载渲染过程   
父beforeCreate->父created->父beforeMount->子beforeCreate->子created->子beforeMount->子mounted->父mounted

更新过程
父beforeUpdate->子beforeUpdate->子updated->父updated

销毁过程
父beforeDestroy->子beforeDestroy->子destroyed->父destroyed

### template到render的过程
template -> ast -> render

1. 调用parse方法将template转化为ast（抽象语法树）
2. 对静态节点做优化 分析出哪些是静态节点，给其打个标记，后续更新渲染可以直接跳过静态节点做优化
3. 生成代码

### data里属性值发生变化，视图会立即同步执行重新渲染吗？
不会立即同步执行重新渲染，vue在更新dom时是异步执行的，只要侦听到数据的变化，vue将开启一个队列，并缓冲在同一事件循环中发生的所有数据变更。
如果同一个watcher被多次触发，只会被推入到队列中一次避免了不必要的计算和dom操作，然后在下一个事件循环tick中，vue刷新队列执行实际工作。

### computed、watch、methods区别
computed:
+ 支持缓存，依赖的数据发生变化才会重新计算
+ 不支持异步，有异步操作，无法监听数据的变化
+ computed的值会默认走缓存，计算属性是基于它们的响应式依赖进行缓存的，也就是基于data声明过，或者父组件传递过来的props中的数据进行计算的
+ 如果一个属性是由其他属性计算而来的，这个是属性依赖其他的属性，一般会使用computed
watch：
+ 不支持缓存，数据变化时就会执行响应的操作
+ 支持异步监听
+ 监听数据必须是data中声明的或者父组件传递过来的props中的数据
method：
+ 可以讲同一个函数定义为一个method或一个计算属性，对于最终结果，两种方式是相同的
不同点
+ 计算属性是基于他们的依赖进行缓存的，只有相关依赖发生变化的才会重新求值。methods调用总会执行函数

### 组件传值都有哪些方法
父子传值 
1. props $emit
2. $parent $children 获取当前组件的父组件和当前组件的子组件
+ $attrs $listeners A>B>C
+ provide inject
+ $refs获取组件实例
+ $emit $on兄弟组件数据传递
+ vux 状态管理

### keep-alive的实现
作用：实现组件缓存，保持这些组件的状态，以避免反复渲染导致的性能问题。 需要缓存组件 频繁切换，不需要重复渲染
场景：tabs标签页 后台导航，vue性能优化
原理：Vue.js内部将DOM节点抽象成了一个个的VNode节点，keep-alive组件的缓存也是基于VNode节点的而不是直接存储DOM结构。它将满足条件（pruneCache与pruneCache）的组件在cache对象中缓存起来，在需要重新渲染的时候再将vnode节点从cache对象中取出并渲染。

### 路由原理 history 和 hash 两种路由方式的特点
hash 模式

location.hash的值实际就是URL中#后面的东西，特点在于：hash虽然出现URL中，但不会被包含在HTTP请求中，对后端完全没有影响，因此改变 hash 不会重新加载页面。

可以为 hash 的改变添加监听事件

window.addEventListener("hashchange", funcRef, false);

每一次改变 hash（window.location.hash），都会在浏览器的访问历史中增加一个记录利用 hash 的以上特点，就可以来实现前端路由“更新视图但不重新请求页面”的功能了

特点：兼容性好但是不美观

history 模式
利用了HTML5 HistoryInterface中新增的pushState()和replaceState()方法。
这两个方法应用于浏览器的历史记录站，在当前已有的 back、forward、go 的基础之上，它们提供了对历史记录进行修改的功能。这两个方法有个共同的特点：当调用他们修改浏览器历史记录栈后，虽然当前URL改变了，但浏览器不会刷新页面，这就为单页应用前端路由“更新视图但不重新请求页面”提供了基础。

特点：虽然美观，但是刷新会出现 404 需要后端进行配置


### MVC和MVVM的区别
MVC：
Model(模型)-View(视图)-Control(控制器)的缩写，一种软件设计规范

Model（模型）：用于处理数据逻辑的部分，通常模型对象负责在数据库中存取数据
View（视图）: 应用程序中处理数据显示的部分。通常视图是依据模型数据创建的
Controller: 处理用户交互的部分。通常控制器负责从视图读取数据，控制用户输入，并向模型发送数据

MVC 的思想：一句话描述就是 Controller 负责将 Model 的数据用 View 显示出来，换句话说就是在 Controller 里面把 Model 的数据赋值给 View。

MVVM:
ViewModel:  一是将【模型】转化成【视图】，即将后端传递的数据转化成所看到的页面。实现的方式是：数据绑定。二是将【视图】转化成【模型】，即将所看到的页面转化成后端的数据。实现的方式是：DOM 事件监听。

### 虚拟dom有什么优缺点
优点：    
1. 保证性能的下限： 虚拟dom需要适配任何上层api可能产生的操作，所以性能并不是最优的，但是比起粗暴的dom操作性能要好很多；
2. 无需手动操作dom；
3. 跨平台
缺点：
1. 无法进行极致的优化
2. 首次渲染大量dom时，由于多了一层虚拟dom的计算，会比innerHTML插入慢。


### v-for 为什么要加 key
如果不使用 key，Vue 会使用一种最大限度减少动态元素并且尽可能的尝试就地修改/复用相同类型元素的算法。key 是为 Vue 中 vnode 的唯一标记，通过这个 key，diff 操作可以更准确、更快速

###  Vue 事件绑定原理
原生事件绑定是通过 addEventListener 绑定给真实元素的，组件事件绑定是通过 Vue 自定义的$on 实现的；

### vue-router 路由钩子函数是什么 执行顺序是什么
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

### Vuex的理解及使用场景
 vuex 是专门为 vue 提供的全局状态管理系统，用于多个组件中数据共享、数据缓存等。（无法持久化、内部核心原理是通过创造一个全局实例 new Vue）vuex的状态存储是响应式的。组件从 store 中读取状态的时候，store中的状态发生变化，组件也会相应地更新，改变store中的状态的唯一途径就是显式地提交 (commit) mutation

Vuex主要包括以下几个核心模块：
+ State：定义了应用状态的数据结构，可以在这里设置默认的初始状态。

+ Getter：在 store 中定义“getter”（可以认为是 store 的计算属性），就像计算属性一样，getter 的返回值会根据它的依赖被缓存起来， 且只有当它的依赖值发生了改变才会被重新计算 
+ Getter：允许组件从 Store 中获取数据，mapGetters 辅助函数仅仅是将 store 中的 getter 映射到局部计算属性。

+ Mutation：是唯一更改 store 中状态的方法，且必须是同步函数 
+ Action：用于提交 mutation，而不是直接变更状态，可以包含任意异步操作 
+ Module：允许将单一的 Store 拆分为多个 store 且同时保存在单一的状态树中

### Vuex 页面刷新数据丢失怎么解决
需要做 vuex 数据持久化 一般使用本地存储的方案来保存数据 可以自己设计存储方案 也可以使用第三方插件
推荐使用 vuex-persist 插件，它就是为 Vuex 持久化存储而生的一个插件。不需要你手动存取 storage ，而是直接将状态保存至 cookie 或者 localStorage 中

### vue 中使用了哪些设计模式
1.工厂模式 - 传入参数即可创建实例
虚拟 DOM 根据参数的不同返回基础标签的 Vnode 和组件 Vnode
2.单例模式 - 整个程序有且仅有一个实例
vuex 和 vue-router 的插件注册方法 install 判断如果系统存在实例就直接返回掉
3.发布-订阅模式 (vue 事件机制)
4.观察者模式 (响应式数据原理)
5.装饰模式: (@装饰器的用法)
6.策略模式 策略模式指对象有某个行为,但是在不同的场景中,该行为有不同的实现方案-比如选项的合并策略

### vue-loader是什么？使用它的用途有哪些？
vue文件的一个加载器，将template/js/style转换成js模块。
用途：js可以写es6、style样式可以scss或less、template可以加jade等



### vue和react的区别
主要是涉及思路不同
