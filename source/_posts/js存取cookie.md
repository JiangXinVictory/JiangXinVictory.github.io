---
title: js存取cookie
date: 2019-12-26 17:35:12
tags:
    - javaScript
---

原生js对获取cookie，存储cookie，删除cookie
<!-- more -->

```
function setCookie (name, value, days) {
  let exp = new Date()
  exp.setTime(exp.getTime() + days * 24 * 60 * 60 * 1000)
  document.cookie = name + "=" + escape (value) + ";expires = " + exp.toGMTString()
}
function getCookie (name) {
  let arr, reg = new RegExp("(^| )" + name + "=([^;]*)(;|$)")
  if (arr = document.cookie.match(reg)) {
    return unescape(arr[2])
  } else {
    return null
  }
}
function delCookie (name) {
  let exp = new Date()
  exp.setTime(exp.getTime() - 1)
  let cVal = getCookie(name)
  if (cVal != null) document.cookie = name + "=" + cVal + ";expires="+exp.toGMTString()
}
```
