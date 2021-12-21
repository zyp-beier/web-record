
### generator函数与async/await

理解async函数就要先理解generator函数，因为async就是Generator函数的语法糖   
#### Generator 函数
Generator 函数是 ES6 提供的一种异步编程解决方案，可以先理解为一个状态机，封装了多个内部状态，执行Generator函数返回一个遍历器对象，通过遍历器对象，可以依次遍历 Generator 函数内部的每一个状态   
语法上，Generator 函数是一个普通函数，但是有两个特征。
一是，function关键字与函数名之间有一个星号；
二是，函数体内部使用yield表达式，定义不同的内部状态（yield在英语里的意思就是“产出”）；
```
function* helloGenerator() {
  yield 'hello'
  yield 'Generator'
  return 'ending'
}

let Generator = helloGenerator（）
```

调用Generator函数后并不执行，返回的也不是函数运行结果而是一个指向内部状态的指针对象，也就是遍历器对象（Iterator Object）。
必须调用遍历器对象的next方法，使得指针移向下一个状态。
```
console.log(Generator.next())  //  {value: 'hello', done: false}
console.log(Generator.next())  //  {value: 'Generator', done: false}
console.log(Generator.next())  //  {value: 'ending', done: true}
```
第一次调用next方法，Generator函数开始执行，直到遇到yield表达式为止。next方法返回一个对象，value属性就是当前yield表达式的值hello，done属性的值false，表示遍历还没有结束。
第二次调用next方法，Generator 函数从上次yield表达式停下的地方，一直执行到下一个yield表达式
继续调用next方法直到done属性值为true或者执行到return语句（如果没有return语句就执行到函数结束），表示遍历已经结束
如果再次调用next方法，此时Generator函数已经运行完毕，next方法返回对象的value属性为undefined，done属性为true。以后再调用next方法，返回的都是这个值
#### yield 表达式
可以理解为暂停的标志,遇到yield表达式，就暂停执行后面的操作，并将紧跟在yield后面的那个表达式的值，作为返回的对象的value属性值。yield表达式与return语句都能返回紧跟在语句后面的那个表达式的值。区别在于每次遇到yield，函数暂停执行，下一次再从该位置继续向后执行，而return语句不具备位置记忆的功能。一个函数里面，只能执行一次return语句，但是可以执行多次yield表达式。从另一个角度看，也可以说 Generator 生成了一系列的值，这也就是它的名称的来历（英语中，generator 这个词是“生成器”的意思）。另外需要注意，yield表达式只能用在 Generator 函数里面，用在其他地方都会报错。

#### next方法的参数
next方法可以带一个参数，该参数就会被当作上一个yield表达式的返回值
```
function* foo(x) {
  let y = yield  x + 1
  let k = yield y + 2
  yield k / 2
  return k
}

let a = foo(1)

console.log(a.next())   //  {value: 2, done: false}
console.log(a.next(3))  //  {value: 5, done: false}
console.log(a.next(8))  //  {value: 4, done: false}
console.log(a.next())   //  {value: 8, done: true}
```
第一次运行next方法时，返回1+1的值2；第二次调用next方法，将上一次yield表达式的值设为3，y等于3，返回y + 2的值5；第三次调用next方法，将上一次yield表达式的值设为8，k等于8，返回k/2的值4   
注意，由于next方法的参数表示上一个yield表达式的返回值，所以在第一次使用next方法时，传递参数是无效的。

### next()、throw()、return()
除了next方法还有throw()、return()两个方法，这三个方法本质上是同一件事，可以放在一起理解。它们的作用都是让 Generator 函数恢复执行，并且使用不同的语句替换yield表达式。
next()是将yield表达式替换成一个值。
throw()是将yield表达式替换成一个throw语句。
```
const g = function* (x, y) {
  let result = yield x + y;
  return result;
};

const gen = g(1, 2);

gen.throw(new Error('出错了')); // Uncaught Error: 出错了
// 相当于将 let result = yield x + y 替换成 let result = throw(new Error('出错了'));
```
return()是将yield表达式替换成一个return语句。   
```
gen.return(2); // {value: 2, done: true}
// 相当于将 let result = yield x + y替换成 let result = return 2;

```
#### yield* 表达式
ES6提供了yield*表达式，用来在一个Generator函数里面执行另一个Generator函数。


#### async/await

ES7 中引入了 async/await,async 是一个通过异步执行并隐式返回 Promise 作为结果的函数。async 函数的实现原理，就是将 Generator函数和自动执行器，包装在一个函数里。

根据阮一峰老师的介绍，async函数就是Generator函数的语法糖,并对Generator函数进行了改进。

上面代码async函数就是将Generator函数的星号（*）替换成async，将yield替换成await，仅此而已
async函数对 Generator 函数的改进，体现在以下四点
1. 内置执行器
Generator 函数的执行必须靠执行器，需要调用next方法，才能真正执行，得到最后结果。
2. 更好的语义
async和await，比起星号和yield，语义更加清楚。async表示函数里有异步操作，await表示紧跟在后面的表达式需要等待结果。
3. 更广的适用性
co模块约定，yield命令后面只能是 Thunk 函数或 Promise 对象，而async函数的await命令后面，可以是 Promise 对象和原始类型的值（数值、字符串和布尔值，但这时会自动转成立即 resolved 的 Promise 对象）
4. 返回值是promise
async函数的返回值是Promise对象，比Generator函数的返回值是Iterator对象方便多了。可以用then方法指定下一步的操作。
async函数完全可以看作多个异步操作，包装成的一个 Promise 对象，而await命令就是内部then命令的语法糖。
