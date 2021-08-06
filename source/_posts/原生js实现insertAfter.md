---
title: 原生js实现insertAfter
date: 2019-12-26 17:05:34
tags:
    - javaScript
---

使用原生js实现将某个节点插入到目标节点后边
<!-- more -->

```
function insertAfter (dom, target) {
    // 获取目标节点的父级标签
    var parent = target.parentNode
    // 如果目标节点正好是最后一个节点，使用appendChild，否则找到它的下一个节点，再使用insertBefore()
    if(parent.lastChild === target) {
        parent.appendChild(dom)
    } else {
        parent.insertBefore(dom, target.nextSibling)
    }
}
```
