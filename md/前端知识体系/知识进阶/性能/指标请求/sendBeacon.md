[toc]

# 背景
这个方法主要用于满足统计和诊断代码的需要，这些代码通常尝试在卸载（unload）文档之前向web服务器发送数据。过早的发送数据可能导致错过收集数据的机会。然而，对于开发者来说保证在文档卸载期间发送数据一直是一个困难。因为用户代理通常会忽略在 unload (en-US) 事件处理器中产生的异步 XMLHttpRequest。
| 技术 | 作用 | 缺点 |
|------|-----|------|
| 同步XMLHttpRequest | 迫使用户代理延迟卸载文档 | 影响下个页面载入性能 |
| 卸载事件中创建一个图片元素并设置它的src | 绝大多数用户代理会延迟卸载以保证图片的载入，所以数据可以在卸载事件中发送 | 1.编码模式不好；2.不可靠；3.影响下个页面载入性能 |
| 创建几秒钟的no-op循环 | 占用js线程来延迟卸载 | 1.编码模式不好；2.不可靠；3.影响下个页面载入性能 |

主要原则：**阻塞**
主要缺点：**影响用户体验**

# sendBeacon

navigator.sendBeacon() 方法可用于通过HTTP将少量数据异步传输到Web服务器

## 语义

> beacon：  灯塔，信号浮标；指引

sendBeacon如其语义，发送锚点、指标。

## 用法
请求的method为POST，keep-alive为true，最大数据由用户代理决定，Chrome40的限制为65535。
### 语法

navigator.sendBeacon(url, data);

### 参数

- url
  - url 参数表明 data 将要被发送到的网络地址。
- data
  - data 参数是将要发送的 ArrayBufferView 或 Blob, DOMString 或者 FormData 类型的数据。
  - 
### 返回值

当用户代理成功把数据加入传输队列时，sendBeacon() 方法将会返回 true，否则返回 false
### 接口返回内容
本API不提供响应回调，鼓励服务器省略针对此类请求的响应正文（例如使用响应204 No Content）。

## 注意
应尽量避免使用unload，而应该用`visibilitychange`替代。当页面过渡到后台状态（例如，当用户切换到其他应用程序，进入主屏幕等）或正在卸载时，此事件是唯一可以保证在移动设备上触发的事件。开发人员应避免依赖unload事件，因为事件不会在页面处于后台状态（即visibilityState等于hidden）并且移动操作系统终止该过程时触发 。

例子：
```js
  // emit non-blocking beacon to record client-side event
  function reportEvent(event) {
    var data = JSON.stringify({
      event: event,
      time: performance.now()
    });
    navigator.sendBeacon('/collector', data);
  }

  // emit non-blocking beacon with session analytics as the page
  // transitions to background state (Page Visibility API)
  document.addEventListener('visibilitychange', function() {
    if (document.visibilityState === 'hidden') {
      var sessionData = buildSessionReport();
      navigator.sendBeacon('/collector', sessionData);
    }
  });
```
## sendBeacon的优点
- **数据可靠**
- **语义更明确、代码更简洁**
- **不会延迟页面的卸载或影响下一个页面的加载**
- **不竞争** 进行优先级排序，不与时间紧迫的操作和更高优先级的网络请求竞争
- **不阻塞** 不阻塞js线程，不阻塞请求和其他时间处理

## 限制（不做也不打算做的）
- 不提供用于脱机存储或交付的特殊处理。信标请求与其他任何请求一样，可以与[ SERVICE-WORKERS ]组合以在必要时提供离线功能
- 无意提供后台同步或传输功能。用户代理限制了可接受的最大有效负载大小，以确保信标请求能够快速，及时地完成。
- 不提供自定义请求方法，提供自定义请求标头或更改请求和响应的其他 处理属性的功能。需要此类请求的非默认设置的应用程序应使用[ FETCH ] API，并将keep-alive标志设置为 true。

# 参考链接

- [mdn sendBeacon](https://developer.mozilla.org/zh-CN/docs/Web/API/Navigator/sendBeacon)
- [w3c sendBeacon](https://w3c.github.io/beacon/#sendbeacon-method)