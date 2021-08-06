---
title: XHR
date: 2019-12-26 17:32:43
tags:
    - javaScript
---

将 XMLHttpRequest 进行简易封装，处理一些简单的 GET POST 请求。
<!-- more -->

```
function ajax (type, url, data = {}) {
    // 处理post参数
    var arr = [];
    for (var i in data) {
        arr.push(i + '=' + data[i]);
    }
    var dataStr = arr.join('&');
    var request = new XMLHttpRequest();
    return new Promise(function (resolve, reject) {
        request.onreadystatechange = function () {
            if (request.readyState === 4) {
                if (request.status === 200) {
                    resolve(request.responseText);
                } else {
                    reject(request.status);
                }
            }
        };
        request.open(type, url, true);
        request.setRequestHeader("custom", JSON.stringify(header));
        request.setRequestHeader("Authorization", Authorization);
        request.setRequestHeader("Content-Type", "application/x-www-form-urlencoded;");
        request.send(dataStr);
    });
}
```
