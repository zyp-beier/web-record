### promise
引用类型
可以通过new操作符来实例化

promise在新建后回立即执行

Promise.prototype.then()
Promise.prototype.catch()
Promise.prototype.finally()
Promise.all() 接收一个数组，只有所有的Promise实例状态都变成fulfilled,p的状态才会变成fulfilled，返回一个数组
Promise.race([p1,p2,p3]) 有一个实例率先改变状态，p的状态就跟着改变，那个率先改变的 Promise 实例的返回值，就传递给p的回调函数
Promise.allSettled() 用来确定一组异步操作是否都结束了（不管成功或失败）。包含了”fulfilled“和”rejected“两种情况。
Promise.any() 接受一组 Promise 实例作为参数，只要参数实例有一个变成fulfilled状态，包装实例就会变成fulfilled状态；如果所有参数实例都变成rejected状态，包装实例就会变成rejected状态，Promise.any()跟Promise.race()方法很像，只有一点不同，就是Promise.any()不会因为某个 Promise 变成rejected状态而结束，必须等到所有参数 Promise 变成rejected状态才会结束。
Promise.resolve()将现有对象转为 Promise 对象
Promise.reject()
Promise.try()