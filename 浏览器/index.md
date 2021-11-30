### 浏览器缓存
 cookie、WebStorage（localStorage（本地缓存）和sessionStorage（会话缓存））、indexDB
 共同点： 保存在浏览器且同源的
 不同： 
 1. cookie数据始终在同源的http请求中携带（即使不需要），即cookie在浏览器和服务器之间来回传递。cookie的数据还有路径（path）的概念，可以限制cookie只属于某个路径下，sessionStorage和localStorage不会自动把数据发送给服务器，仅在本地保存
2. 存储大小限制也不同
cookie数据不能超过4k，session Storage和local Storage能达到5M
session Storage:仅在当前浏览器窗口关闭之前有效
local Storage始终有效，窗口或浏览器关闭也一直保存，因此用作持久数据
cookie： 只在设置的cookie过期时间之前有效，即使窗口关闭或浏览器关闭
3. 作用域不同
sessionStorage: 不在不同的浏览器窗口中共享即使是同一个页面；
localStorage: 在所有同源窗口中都是共享的，也就是说只要浏览器不关闭，数据依然存在
cookie：在所有同源窗口中都是共享的，也就是说只要浏览器不关闭，数据依然存在
没有什么能够主转换

### 浏览器缓存和本地缓存的区别
1. 实质的区别
vuex存的是状态，存储在内存，localStorage是浏览器提供的接口，存的是文件，以文件的形式存储在本地
2. 应用场景
vuex用于组件之间的传值，localStorage主要用于页面之间的传值
3. 永久性
当刷新页面时，vuex存储的值为丢失，localStorage不会

### 缓存方法
保存数据： localStorage.setItem(key, value)
读取数据： localStorage.getItem(key, value)
删除单个数据： localStorage.removeItem(key, value)
删除所有数据： localStorage.clear()
得到某个索引的key：localStorage.key(index)
