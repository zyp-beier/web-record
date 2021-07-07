# vue3 & vue2 差异

脚手架 Vite

## 一, 全局api
### 1. 全局 Vue API 已更改为使用应用程序实例 
vue2使用全局api 如 Vue.component, Vue.mixin, Vue.use等，缺点是会导致所创建的根实例将共享相同的全局配置（从相同的 Vue 构造函数创建的每个根实例都共享同一套全局环境。这样就导致一个问题，只要某一个根实例对 全局 API 和 全局配置做了变动，就会影响由相同 Vue 构造函数创建的其他根实例。）   
vue3调用createApp返回一个应用实例，拥有全局API的一个子集，任何全局改变 Vue 行为的 API 现在都会移动到应用实例上  
### 2. 全局和内部 API 已经被重构为可 tree-shaking   
编译阶段利用ES6 Module判断哪些模块已经加载    
判断那些模块和变量未被使用或者引用，进而删除对应代码   
在Vue2中，全局 API 如 Vue.nextTick() 是不支持 tree-shake 的，不管它们实际是否被使用，都会被包含在最终的打包产物中。   
而Vue3源码引入tree shaking特性，将全局 API 进行分块。如果你不使用其某些功能，它们将不会包含在你的基础包中

涉及的API有：   
Vue.nextTick   
Vue.observable (replaced by Vue.reactive)   
Vue.version   
Vue.compile (only in full builds)   
Vue.set (only in compat builds)   
Vue.delete   
### 3. 组件挂载   
vue3使用createApp()初始化后，应用实例 app 可用 app.mount() 挂载根组件实例 
```
import { createApp } from 'vue'
import App from './App.vue'

const app = createApp(App)  
```
App单文件组件经过编译后得到一个render函数
createApp会返回一个app对象，里面包含一个mount函数
mount函数是被重写过的
1. 处理传入的容器并生成节点；
2. 判断传入的组件是不是函数组件，组件里有没有render函数，template属性，没有就用容器的innerHTML作为组件的template；
3. 清空容器内容
4. 运行缓存的mount函数实现挂载组件；

## 二, 模板指令   
+ 组件上 v-model 用法更改，替换 v-bind.sync   
 vue2默认会利用名为 value 的 prop 和名为 input 的事件  
 prop：value -> modelValue；   
 event：input -> update:modelValue   
 model选项,允许组件自定义用于 v-model 的 prop 和事件   
.sync (对某一个 prop 进行“双向绑定”,是update:myPropName 事件的简写)    
v-bind 的 .sync 修饰符和组件的 model 选项已移除，可用 v-model 作为代替  
vue3 
 可以将一个 argument 传递给 v-model：   
 `<ChildComponent v-model:title="pageTitle" />`   
 等价于   
 `
 <ChildComponent :title="pageTitle" @update:title="pageTitle = $event" />`   
可使用多个model
 + 可以在template元素上添加 key
 + 同一节点v-if 比 v-for 优先级更高
 + v-bind="object" 现在排序敏感（vue绑定相同property，单独的 property 总是会覆盖 object 中的绑定。vue3按顺序决定如何合并）
 + 移除 v-on.native 修饰符    
 Vue 2 如果想要在一个组件的根元素上直接监听一个原生事件，需要使用v-on 的 .native 修饰符   
 Vue3 现在将所有未在组件emits 选项中定义的事件作为原生事件添加到子组件的根元素中（除非子组件选项中设置了 inheritAttrs: false）。
 (强烈建议组件中使用的所有通过emit触发的event都在emits中声明)
 + v-for 中的 ref 不再注册 ref 数组   
 在 v-for 语句中使用ref属性时，会生成refs数组插入$refs属性中。由于当存在嵌套的v-for时，这种处理方式会变得复杂且低效。
 在 v-for 语句中使用ref属性 将不再会自动在$refs中创建数组。而是，将 ref 绑定到一个 function 中，在 function 中可以灵活处理ref。    
## 三, 组件
+ 只能使用普通函数创建功能组件   
在 Vue 2 中，函数式组件有两个主要应用场景：   
作为性能优化，因为它们的初始化速度比有状态组件快得多   
返回多个根节点   
然而Vue 3对有状态组件的性能进行了提升，与函数式组件的性能相差无几。此外，有状态组件现在还包括返回多个根节点的能力。   
+ 函数式组件单文件组件：functional 移除,并将 props 的所有引用重命名为 $props，将 attrs 重命名为 $attrs。
```
<template>
  <component
    v-bind:is="`h${$props.level}`"
    v-bind="$attrs"
  />
</template>

<script>
export default {
  props: ['level']
}
</script>
```
函数写法： 所有的函数式组件都是用普通函数创建的，换句话说，不需要定义 { functional: true } 组件选项。
它们将接收两个参数：props 和 context
```
import { h } from 'vue'

const DynamicHeading = (props, context) => {
  return h(`h${props.level}`, context.attrs, context.slots)
}

DynamicHeading.props = ['level']

export default DynamicHeading
```

+ 异步组件需要 defineAsyncComponent 方法来创建    
异步组件的导入需要使用辅助函数defineAsyncComponent来进行显式声明,带选项异步组件，component 选项重命名为 loader
```
import { defineAsyncComponent } from 'vue'
const child = defineAsyncComponent(() => import('@/components/async-component-child.vue'))
```
带选项的
```
const asyncPageWithOptions  = defineAsyncComponent({
  loader: () => import('./NextPage.vue'),
  delay: 200,
  timeout: 3000,
  error: ErrorComponent,
  loading: LoadingComponent
})
```
+ （新增）组件事件需要在 emits 选项中声明（）   
强烈建议使用 emits 记录每个组件所触发的所有事件。
因为移除了 v-on.native 修饰符。任何未声明 emits 的事件监听器都会被算入组件的 $attrs 并绑定在组件的根节点上。   
如果emit的是原生的事件（如，click）,就会存在两次触发。   
一次来自于$emit的触发；   
一次来自于根元素原生事件监听器的触发；   
## 四, 渲染函数
+ 渲染函数API    
h是全局导入，而不是作为参数传递给渲染函数  
在 2.x 中，render 函数会自动接收 h 函数作为参数    
在 3.x 中，h 函数需要全局导入。由于 render 函数不再接收任何参数，它将主要在 setup() 函数内部使用。可以访问在作用域中声明的响应式状态和函数，以及传递给 setup() 的参数
```
import { h, reactive } from 'vue'

export default {
  setup(props, { slots, attrs, emit }) {
    const state = reactive({
      count: 0
    })

    function increment() {
      state.count++
    }

    // 返回render函数
    return () =>
      h(
        'div',
        {
          onClick: increment
        },
        state.count
      )
  }
}
```
+ ~~插槽统一~~

+ 移除$listeners整合到 $attrs   
包含了父作用域中的(不含emits的) v-on 事件监听器。它可以通过 v-on="$listeners" 传入内部组件

```
{{$attrs}}
<grand-son v-bind="$attrs"></grand-son>
```
+ $attrs包含class&style

## 五, 自定义元素
+ 自定义元素检测在编译时执行   
自定义元素交互   
Vue 2中，通过 Vue.config.ignoredElements 配置自定义元素
```
Vue.config.ignoredElements = ['plastic-button']
```
Vue 3 通过app.config.isCustomElement   
```
const app = Vue.createApp({})
app.config.isCustomElement = tag => tag === 'plastic-button'
```
+ Vue 3.x 对 is做了新的限制
当在 Vue 保留的 component标签上使用is时，它的行为将与 Vue 2.x 中的一致   
当在不同组件标签上使用is时，is会被当做一个不同的prop;   
当在普通的 HTML 元素上使用is，is将会被当做元素的属性。   
新增了v-is，专门来实现在普通的 HTML 元素渲染组件。
## 六, 其他
+ destroyed 生命周期选项被重命名为 unmounted
+ beforeDestroy 生命周期选项被重命名为 beforeUnmount
+ Props 的默认值函数不能访问this    
替代方案：    
把组件接收到的原始 prop 作为参数传递给默认函数；   
inject API 可以在默认函数中使用。    
```
import { inject } from 'vue'

export default {
  props: {
    theme: {
      default (props) {
        // `props` 是传递给组件的原始值。
        // 也可以使用 `inject` 来访问注入的属性
        return inject('theme', 'default-theme')
      }
    }
  }
}
```
+ 自定义指令 API 与组件生命周期一致
```
const MyDirective = {
  created(el, binding, vnode, prevVnode) {}, // 新增
  beforeMount() {},
  mounted() {},
  beforeUpdate() {}, // 新增
  updated() {},
  beforeUnmount() {}, // 新增
  unmounted() {}
}    
```
绑定组件的实例从 Vue 2.x 的vnode.context移到了binding.instance中
+ data 选项应始终被声明为一个函数   
data 组件选项声明不再接收 js 对象，只接受函数形式的声明。
当合并来自 mixin 或 extend 的多个 data 返回值时，data现在变为浅拷贝形式(只合并根级属性)。
+ (非兼容)过渡的 class 名更改(过渡类名 v-enter 修改为 v-enter-from、过渡类名 v-leave 修改为 v-leave-from。)
+ transition-group 不再需要设置根元素(<transition-group> 不再默认渲染根元素，但仍可以使用 tag prop创建一个根元素。)
+ （非兼容）侦听数组（当侦听一个数组时，只有当数组被替换时才会触发回调。如果你需要在数组改变时触发回调，必须指定 deep 选项。）
+ 已挂载的应用不会取代它所挂载的元素（在 Vue 2.x 中，当挂载一个具有 template 的应用时，被渲染的内容会替换我们要挂载的目标元素。在 Vue 3.x 中，被渲染的应用会作为子元素插入，从而替换目标元素的 innerHTML）
+ （非兼容）生命周期 hook: 事件前缀改为 vnode-（监听子组件和第三方组件的生命周期）
## 移除API
+ （非兼容）不再支持使用数字 (即键码) 作为 v-on 修饰符
+ 过滤器  （如果需要使用全局过滤器Vue 3.x 提供了globalProperties。我们可以借助globalProperties来注册全局过滤， 全局过滤器里面定义的只能是method。）
+ 内联模板 （inline-template attribute移除）
+ $children（建议使用 $refs）
+ propsData 选项之前用于在创建 Vue 实例的过程中传入 prop，现在它被移除了。如果想为 Vue 3 应用的根组件传入 prop，使用 createApp 的第二个参数。
+ 全局函数 set 和 delete 以及实例方法 $set 和 $delete。基于代理的变化检测不再需要它们了。
## 响应原理的变化
Vue3 之前使用了 ES5 的一个 API Object.defineProperty Vue3 中使用了 ES6 的 Proxy，都是对需要侦测的数据进行 变化侦测 ，添加 getter 和 setter ，这样就可以知道数据何时被读取和修改。

vue2 使用一个Observer 类将data所有属性都转化为 getter/setter 的形式




## vue3新特性
### 1. 组合式 API
与选项API最大的区别的是逻辑的关注点
选项API
这种碎片化使得理解和维护复杂组件变得困难，在处理单个逻辑关注点时，我们必须不断地上下翻找相关代码的选项块。
组合式API将同一个逻辑关注点相关代码收集在一起
```
import { useRouter } from 'vue-router'
import { reactive, onMounted, toRefs } from 'vue'

setup (props) {
  const state = reactive({
    userInfo: {}
  })

  const getUserInfo = async () => {
    state.userInfo = await GET_USER_INFO(props.id)
  }

  onMounted(getUserInfo) // 在 `mounted` 时调用 `getUserInfo`

  return {
    ...toRefs(state),
    goBack,
    getUserInfo
  }
}
```

### 2. Teleport
允许我们控制在 DOM 中哪个父节点下渲染了 HTML
```
<teleport to="body">
  <div v-if="modalOpen" class="modal">
    <div>
      I'm a teleported modal! 
      (My parent is "body")
      <button @click="modalOpen = false">
        Close
      </button>
    </div>
  </div>
</teleport>
```
### 3. 片段
### 4. 触发组件选项 （emits 1.更好的记录已发出的事件，2.验证抛出的事件）

## 官方支持的库
Vue Router   
Vue Router 4.0 提供了 Vue 3 支持   
Vue CLI   
v4.5.0   
Vuex   
Vuex 4.0 提供了 Vue 3 支持，其 API 与 3.x 基本相同

![vue2迁移vue3](./img/img.jpeg)