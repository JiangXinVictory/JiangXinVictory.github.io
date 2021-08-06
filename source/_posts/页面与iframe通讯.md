---
title: 页面与iframe通讯
date: 2019-12-26 17:02:21
tags:
    - javaScript
---

分享一个父页面与iframe子页面相互通信的代码，之所以这样写，是因为iframe子页面需要父页面通过异步获取数据并传递给iframe
<!-- more -->

1.iframe中引入的js，最好所有的代码都放在listener中，这样可以保证父级页面传输数据的准确性
2.父级页面引入iframe后，向iframe发送消息，要放在iframe标签中的onload回调内，保证iframe能拿到消息
```
<iframe src="" id="iframe" name="iframe" onload="resize()" allowfullscreen></iframe>
```
3.父级页面向iframe发送消息，postMessage第二个参数为域名限制
```
function sendMessage (userId) {
    var iframe = document.getElementById("iframe").contentWindow
    iframe.postMessage({}, '*')
}
```
4.页面活iframe监听消息
```
window.addEventListener("message", function (msg) {
    console.log(msg)
})
```
5.iframe向父级页面发送消息， 第二个参数为域名限制
```
window.parent.postMessage({}, '*')
```
6.
```
top.events = {
    on: function (name, func) {
      if (!this.handles) {
        this.handles = {}
      }
      this.handles[name] = func
    },
    emit (name) {
      if (this.handles[name]) {
        // arguments是伪数组所以通过 call 来使用slice
        this.handles[name].apply(null, Array.prototype.slice.call(arguments, 1))
      }
    },
    destory: function (name) {
      if (this.handles && this.handles[name]) delete this.handles[name]
    }
}

top.events.on('test', function() {})

top.events.emit('test', param))

top.events.destory('test')
```