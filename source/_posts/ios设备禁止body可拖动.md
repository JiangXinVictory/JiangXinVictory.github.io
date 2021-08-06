---
title: ios设备禁止body可拖动
date: 2019-12-26 17:39:33
tags:
    - javaScript
---

ios自带safari浏览器下，h5页面滚动到页面边缘后，继续拖动手指会导致页面整体拖动，下面介绍一下怎么阻止这种情况的发生
<!-- more -->

```
var body = $('body')
  window.addEventListener('touchmove', function (body) {
    let x = body.touches[0].pageX
    let y = body.touches[0].pageY
    if (x > y) {
      body.preventDefault()
}
}, {passive:false})
```
