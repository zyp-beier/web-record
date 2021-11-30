## 自我介绍
下午好，我叫**，今天来应聘贵公司的前端工程师岗位。我从事前端开发两年多，在上家公司主要从事小程序商城 后台管理系统等项目开发以及组内技术分享，头脑风暴等活动。平常喜欢逛一些技术社区丰富自己的技术，像思否，掘金之类的网站。 我的性格比较温和，跟同事朋友相处时比较外向，在工作中代码开发时我喜欢全心全意的投入，对于工作我总抱着认真负责的态度。

### 数据类型
栈： 原始数据类型 string、number、null、undefined、 Boolean、 symbol(es6新增)、bigint(es6新增)
堆： 对象、数组、函数

原始数据类型直接存储在栈（stack）中的简单数据段，占据空间小、大小固定、属于被频繁使用的数据

引用数据类型存储在堆（heap）中的对象,占据空间大、大小不固定。
### 数据类型检测的方式？
1. typeof
其中 数组、对象、null、都会被判断为object其他判断都正确
2. instanceof
instanceof可以正确的判断对象的类型，其内部运行机制是判断在原型链中能否找到该类型的原型
instanceof只能判断正确引用数据类型，不能判断基本数据类型。instanceof运算符可以用来测试一个对象在其原型链中是否存在一个构造函数的prototype属性。
3. constructor

```
console.log((2).constructor)
```
有两个作用，一是判断数据的类型，二是对象实例通过 constructor 对象访问它的构造函数。需要注意，如果创建一个对象来改变它的原型，constructor就不能用来判断数据类型了：
```
function Fn(){};
 
Fn.prototype = new Array();
 
var f = new Fn();
 
console.log(f.constructor===Fn);    // false
console.log(f.constructor===Array); // true
```
4. Object.prototype.toString.call()
Object.prototype.toString.call()使用Object对象的原型方法toString来判断数据类型
```
var a = Object.prototype.toString;
 
console.log(a.call(2));
console.log(a.call(true));
console.log(a.call('str'));
console.log(a.call([]));
console.log(a.call(function(){}));
console.log(a.call({}));
console.log(a.call(undefined));
console.log(a.call(null));
```
### null 和undefined的区别
 undefined代表为定义
 null代表是空值

 使用typeof进行判断的时候， null会返回object。使用双等号对两个值进行比较时返回true，三个等号返回false
### intanceof操作符的实现原理及实现
intanceof用于判断构造函数的prototype属性是否出现在对象的原型链中的任何位置
### 为什么0.1 + 0.2 !== 0.3 ？
计算机是通过二进制的方式存储数据的，所及实际上是计算两个数的二进制的和，这两个数的二进制是无限循环的数，
### typeof NaN的结果是什么？
number
NaN是一个特殊值，它和自身不相等是唯一一个非自反（NaN===NaN不成立）的值，而NaN !== NaN为true
### object.assign和扩展运算符是浅拷贝
### 箭头函数与普通函数的区别？
1. 箭头函数都是匿名函数
2. 箭头函数不能用于构造函数，不能使用new
3. this的指向不同
普通函数 this指向调用它的对象，如果用做构造函数，this指向创建的对象实例。
箭头函数没有原型所以本身没有this，但可以捕获所在上下文的this使用。

### 事件循环
js是单线程的，为了防止一个函数执行时间过长阻塞后面的代码，所以会先将同步的代码压入执行栈中，依稀执行，将异步代码推进异步队列，异步队列又分为宏任务队列和微任务队列，因为宏任务队列执行的时间较长，所以微任务队列要优先于宏任务队列。微任务队列的代表就是promise.then, MutationObserver,宏任务的话就是setImmediate setTimeout setInterval

### 事件冒泡、捕获
event.stopPropagation() 或者 ie下的方法 event.cancelBubble = true; //阻止事件冒泡

### 防抖节流
防抖的原理是事件被触发n秒后在执行回调，如果在这n秒内又被触发，则重新计时
应用场景： 
resize/scroll 触发统计事件
文本输入验证，不用用户输一个文字请求一次，随着用户的输入验证一次就可以

节流的核心是让一个函数不要执行的太频繁，减少一些过快的调用来节流，也就是在一段固定的时间内只触发一次回调函数，即便在这段时间内某个事件多次被触发也只触发回调一次。
应用场景：
DOM 元素的拖拽功能实现
计算鼠标移动的距离
搜索联想

### 数组去重
1. 双循环去重
```
const arr = [1, 1, 2, 3, 3, 4, 5, 5, 6, 7, 7, 8, 9, 9, 10]
function unique() {
  
  let arr1 = [arr[0]]
    for(let i = 0; i < arr.length; i++ ) {
      let flag = true
      for(let j = 0; j < arr1.length; j++) {
        if (arr[i] === arr1[j]) {
          flag = false;
          break
        }
      }
      if (flag) {
        arr1.push(arr[i])
      }
    }
    return arr1
}
```
2. index of 去重1
```
function unique() {
  
  let arr1 = []
  for (let i = 0; i < arr.length; i++) {
    if (arr1.indexOf(arr[i]) === -1 ) {
      arr1.push(arr[i])
    }
  }
    return arr1
}
```
3. indexof 去重2
```
function unique() {
  let arr1 = []
  return arr.filter((item, index) => {
      return arr.indexOf(item) === index
  })
}
```
4. 相邻元素去重
这种方法首先调用了数组的排序方法sort()，然后根据排序后的结果进行遍历及相邻元素比对，如果相等则跳过改元素，直到遍历结束

```
function unique(arr) {
  arr = arr.sort()
  let res = []
  for (let i = 0; i < arr.length; i++) {
      if (arr[i] !== arr[i-1]) {
          res.push(arr[i])
      }
  }
  return res
}
```
1. 利用es6 set去重
```
function unique (arr) {
  return Array.from(new Set(arr))
}
```

### 小程序开发用户授权流程
1. 使用wx.authorize 向用户发起授权请求

### new 一个函数发生了什么
```
function Func(){
}
let func= new Func();
```
1. 创建一个空对象
let obj = new Object()

2. 链接到原型
把obj的proto指向构造函数的原型对象prototype，此时便建立了obj对象的原型链：
obj>Func.prototype>Object.prototype>null
obj._proto_ = Func.prototype
3. 绑定this值（让Func中的this指向obj，并执行Fun的函数体）
let result = Func.call(obj)
4. 返回新对象（判断Func的返回值类型： 如果无返回值或者返回一个非对象的值，则将obj作为新对象返回；否则会将result作为新对象返回）



数组总结 跨域