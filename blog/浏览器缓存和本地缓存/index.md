## 本地存储和vuex
随着Web应用程序的出现，直接在客户端存储用户信息的需求也随之出现。这背后的想法是合理的：与特定用户相关的信息应该保存在用户的机器上。无论是登录信息、个人偏好，还是其他数据，Web应用程序提供者都需要有办法把它们保存在客户端。对该问题的第一个解决方案就是cookie。今天，cookie只是在客户端存储数据的一个选项。
### cookie
HTTP cookie也叫作cookie，最初用于在客户端存储会话信息。这个规范要求服务器在响应HTTP请求时，通过发送Set-Cookie HTTP头部包含会话信息。例如，下面是包含这个头部的一个HTTP响应：
```
Content-type: text/html
Set-Cookie: name=value
other-header: other-header-value
```
这个HTTP响应会设置一个名为"name"，值为"value"的cookie。名和值在发送时都会经过URL编码。浏览器会存储这些会话信息，并在之后的每个请求中都会通过HTTP头部cookie再将它们发回服务器,比如
```
Cookie: name=value
other-header: other-header-value
```
这些发送回服务器的额外信息可用于唯一标识发送请求的客户端。
客户端通过读取Cookies，得知你的相关信息，就可以做出相应的动作，如在页面显示欢迎你的标语，或者让你不用输入ID、密码就直接登录等等。
### Web Storage
Web Storage的目的是解决通过客户端存储不需要频繁发送回服务器的数据时使用cookie的问题Web Storage定义了两个对象：localStorage和sessionStorage。localStorage是永久存储机制，sessionStorage是跨会话的存储机制。这两种浏览器存储API提供了在浏览器中不受页面刷新影响而存储数据的两种方式

### sessionStorage对象
sessionStorage对象只存储会话数据，这意味着数据只会存储到浏览器关闭。这跟浏览器关闭时会消失的会话cookie类似。存储在sessionStorage中的数据不受页面刷新影响，可以在浏览器崩溃并重启后恢复,并且只能由最初存储数据的页面使用，在多页应用程序中的用处有限。
### 使用方法
```
// 使用方法存储数据
sessionStorage.setItem('name', 'baobei')

// 使用属性存储数据
sessionStorage.book = 'Javascript'
```

```
// 使用方法取得数据
let name = sessionStorage.getItem('name')

// 使用属性获取数据
let book = sessionStorage.book
```
所有现代浏览器在实现存储写入时都使用了同步阻塞方式，因此数据会被立即提交到存储
要从sessionStorage中删除数据，可以使用delete操作符直接删除对象属性，也可以使用removeItem()方法。
```
// 使用delete删除值
delete sessionStorage.name

// 使用方法删除值
sessionStorage.removeItem('book')
```
sessionStorage对象应该主要用于存储只在会话期间有效的小块数据。如果需要跨会话持久存储数据，可以使用localStorage
### localStorage对象
要访问同一个localStorage对象，页面必须来自同一个域（子域不可以）、在相同的端口上使用相同的协议
localStorage对象操作数据的方法和sessionStorage方法类似,这里就不展开写了
两种存储方法的区别在于，存储在localStorage中的数据会保留到通过JavaScript删除或者用户清除浏览器缓存。localStorage数据不受页面刷新影响，也不会因关闭窗口、标签页或重新启动浏览器而丢失

总结： 
1. 数据存储方式
cookie在浏览器和服务器之间通过header来回传递，cookie的数据还有路径（path）的概念，可以限制cookie只属于某个路径下，sessionStorage和localStorage不会自动把数据发送给服务器，仅在本地保存
2. 大小限制
cookie数据不能超过4k，session Storage和local Storage能达到5M
3. 有效时间
sessionStorage：仅在当前浏览器窗口关闭之前有效；
localStorage：始终有效，窗口或浏览器关闭也一直保存，因此用作持久数据；
cookie：只在设置的cookie过期时间之前有效，即使窗口关闭或浏览器关闭
4. 作用域不同
sessionStorage: 不在不同的浏览器窗口中共享即使是同一个页面；
localStorage: 在所有同源窗口中都是共享的，也就是说只要浏览器不关闭，数据依然存在
cookie：在所有同源窗口中都是共享的，也就是说只要浏览器不关闭，数据依然存在

除了上面说到的几种方法之外还有一个浏览器中存储结构化数据的一个方案那就是IndexedDB，IndexedDB是类似于MySQL或WebSQL Database的数据库。与传统数据库最大的区别在于，IndexedDB使用对象存储而不是表格保存数据。IndexedDB的设计几乎完全是异步的，为此，大多数操作以请求的形式执行，这些请求会异步执行，产生成功的结果或错误。绝大多数IndexedDB操作要求添加onerror和onsuccess事件处理程序来确定输出。这里只是简单的做了解具体请查看官方文档https://developer.mozilla.org/zh-CN/docs/Web/API/IndexedDB_API

### 本地存储与vuex的区别
1. 实质的区别
vuex存的是状态，存储在内存，webStorage是浏览器提供的接口，存的是文件，以文件的形式存储在本地
2. 应用场景
vuex用于组件之间的传值，webStorage主要用于页面之间的传值
3. 永久性
当刷新页面时，vuex存储的值为丢失，webStorage不会