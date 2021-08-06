---
title: js获取地址栏参数
date: 2019-12-26 17:41:20
tags:
    - javaScript
---

js获取地址栏参数
<!-- more -->

```
function getParams (key) {
    var reg = new RegExp("(^|&)" + key + "=([^&]*)(&|$)")
    var result = window.location.search.substr(1).match(reg)
    return result ? decodeURIComponent(result[2]) : null
}

function parseURL (url) {
  var a =  document.createElement('a')
  a.href = url
  return {
    source: url,
    protocol: a.protocol.replace(':', ''),
    host: a.hostname,
    port: a.port,
    query: a.search,
    params: (function(){
      var ret = {},
        seg = a.search.replace(/^\?/, '').split('&'),
        len = seg.length, i = 0, s
      for (; i<len; i++) {
        if (!seg[i]) { continue }
        s = seg[i].split('=')
        ret[s[0]] = s[1]
      }
      return ret
    })(),
    file: (a.pathname.match(/\/([^\/?#]+)$/i) || [, ''])[1],
    hash: a.hash.replace('#', ''),
    path: a.pathname.replace(/^([^\/])/, '/$1'),
    relative: (a.href.match(/tps?:\/\/[^\/]+(.+)/) || [, ''])[1],
    segments: a.pathname.replace(/^\//, '').split('/')
  }
}
```
