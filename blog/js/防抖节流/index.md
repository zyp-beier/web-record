### 防抖和节流
### 防抖（延迟执行，重新计时）
防抖的原理是事件被触发n秒后在执行回调，如果在这n秒内又被触发，则重新计时，也就是当事件被触发，先执行一个n秒的定时器，n秒后再去执行真正的回调，如果n秒内事件被频繁的触发，那么每次都是重新的计时，它和节流的不同在于如果某段时间内事件以间隔小于n秒的频率执行，那么这段时间回调只会触发一次。节流则是按照200ms或者300ms定时触发，而不仅仅是一次。
基本防抖实现
```
function debounce(func, wait) {
    if (typeof func !== 'function') {
      throw new Error('need a function')
    }

    wait = +wait || 0
    let timer = null
    return function() {
      if (timer) {
        clearTimeout(timer)
      }
      timer = setTimeout(func, wait)
    }
  }
  const handleThrottling = debounce(() => {
      console.log(123)
  }, 2000)
```
它是一个闭包，核心就是定时器的设置，第一次进入timer不存在，就设置一个延迟wait毫秒的定时器，如果timer已经存在则先清除定时器再 重新设置延迟。如上，如果在延迟之内又触发了一个事件，则计时器重新开始计时

应用场景： 
resize/scroll 触发统计事件
文本输入验证，不用用户输一个文字请求一次，随着用户的输入验证一次就可以

### 节流
节流的核心是让一个函数不要执行的太频繁，减少一些过快的触发来节流，也就是在一段固定的时间内只触发一次回调函数，即便在这段时间内某个事件多次被触发也只触发回调一次。

应用场景：
DOM 元素的拖拽功能实现
计算鼠标移动的距离
搜索联想

为什么适合节流不适合防抖？
试想如果我们正在拖拽一个元素，拖拽的时候没反应等我们放开元素n秒后，元素一下子蹦过来了，那这肯定不是我们想要的。

基本节流实现
```
<input type="button" value='click me' id = 'button' onclick="handleThrottling()">

const handleThrottling = Throttling(() => {
    console.log('执行了')
  }, 2000)

  function Throttling (func, gapTime) {
    if (typeof func !== 'function') {
      throw new Error('need a function')
    }
    gapTime = +gapTime || 0
    let lastTime = 0
    return function() {
      let now = +new Date()
      if (now - lastTime > gapTime || !lastTime) {
        // 当前时间-上次执行时间大于间隔秒数（也就是说两次触发时间小与间隔时间）或者第一次执行时，执行真正的回调
        func()
        // 当前时间赋值给上次执行时间
        lastTime = now
      }
    }
  }
```

总结
根据业务场景，正确合理的使用防抖（debounce）和节流（throttle）能够优化性能和提升用户体验，这两者的核心区别在于持续的触发事件时，防抖只会在最后一次事件后延迟再执行，而节流则是每隔n秒触发一次