## 自我介绍
下午好，我叫**，今天来应聘贵公司的前端工程师岗位。我从事前端开发两年多，在上家公司主要从事后台管理系统等项目开发以及组内技术分享，头脑风暴等活动。平常喜欢逛一些技术社区丰富自己的技术，像思否，掘金之类的网站。 我的性格比较温和，跟同事朋友相处时比较外向，在工作中代码开发时我喜欢全心全意的投入，对于工作我总抱着认真负责的态度。

### 数据类型
栈： 原始数据类型 string、number、null、undefined、 Boolean、 symbol(es6新增)、bigint(es6新增)
堆： 对象、数组、函数

原始数据类型直接存储在栈（stack）中的简单数据段，占据空间小、大小固定、属于被频繁使用的数据

引用数据类型存储在堆（heap）中的对象,占据空间大、大小不固定，引用地址存储在栈中，本身内容存储在堆中。
### 数据类型检测的方式？
1. typeof（判断基础类型，除了null会返回object） 引用类型也将返回object
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
js是单线程的，为了防止一个函数执行时间过长阻塞后面的代码，所以会先将同步的代码压入执行栈中，依次执行，将异步代码推进异步队列，异步队列又分为宏任务队列和微任务队列，因为宏任务队列执行的时间较长，所以微任务队列要优先于宏任务队列。微任务队列的代表就是promise.then, MutationObserver,宏任务的话就是setImmediate setTimeout setInterval
执行栈的执行方式是先进后出
任务队列的执行方式是先进先出

### 事件冒泡、捕获
event.stopPropagation() 或者 ie下的方法 event.cancelBubble = true; //阻止事件冒泡
### 小程序开发用户授权流程
1. 使用wx.authorize 向用户发起授权请求

### new 一个函数发生了什么
```
function Func(){
}
let func= new Func();
```
1. 创建一个新对象
2. 新对象内部的[[Prototype]]指针被赋值为构造函数的prototype属性
3. 将构造函数的作用域赋给新对象（因此 this 就指向了这个新对象）
4. 执行构造函数内部的代码（给新对象添加属性）
5. 如果构造函数返回非空对象，则返回该对象；否则，返回刚创建的新对象。


暂时性死区（Temporal Dead Zone ）
用MDN上的定义

> let bindings are created at the top of the (block) scope containing the declaration, commonly referred to as “hoisting”. Unlike variables declared with var, which will start with the value undefined, let variables are not initialized until their definition is evaluated. Accessing the variable before the initialization results in a ReferenceError. The variable is in a “temporal dead zone” from the start of the block until the initialization is processed.

大概意思便是let同样存在变量提示（hoisting），只是形式与var不同，var定义的变量将会被赋予undefined的初始值，而let在被显式赋值之前不会被赋予初始值，并且在赋值之前读写变量都会导致 ReferenceError 的报错。从代码块(block)起始到变量求值(包括赋值)以前的这块区域，称为该变量的暂时性死区。



let personName, personAge;

let person = {
  name: 'aa',
  age: 12
};

({name: personName, age: personAge} = person)

console.log(personName, personAge)







let person = {
  name: '13',
  age: 24,
  job: {
    title: 'jobjob'
  }
};

let person1 = {};   // 必须加分号

({name: person1.name1, age: person1.age1, job: person1.job1} = person)

console.log(person)
console.log(person1)

person.job.title = 'aaaaaa'
console.log(person)
console.log(person1)


小程序原理

http状态码

vue和react的区别

cdn原理


登陆流程

跨域 线上是怎么搞的