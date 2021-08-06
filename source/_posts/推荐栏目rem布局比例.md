---
title: rem布局
date: 2019-12-26 17:38:26
tags:
    - javaScript
---

rem是CSS3新增的一个相对单位（root em，根em），这个单位引起了广泛关注。这个单位与em有什么区别呢？区别在于使用rem为元素设定字体大小时，仍然是相对大小，但相对的只是HTML根元素。这个单位可谓集相对大小和绝对大小的优点于一身，通过它既可以做到只修改根元素就成比例地调整所有字体大小，又可以避免字体大小逐层复合的连锁反应。目前，除了IE8及更早版本外，所有浏览器均已支持rem。对于不支持它的浏览器，应对方法也很简单，就是多写一个绝对单位的声明。这些浏览器会忽略用rem设定的字体大小。
<!-- more -->

```
// 移动端，设备宽度限制最大640
function resize () {
  let fontSize = window.innerWidth >= 640 ? 32 : window.innerWidth / 20
  document.getElementsByTagName('html')[0].style.fontSize = fontSize + 'px'
  fontSize = null
}
resize()
window.onresize = resize
```
