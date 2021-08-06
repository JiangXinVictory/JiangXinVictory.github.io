---
title: CSS3中实现块级元素width以内容为主的尺寸计算方式
date: 2020-02-28 16:51:20
tags:
---

MDN: <a href="https://developer.mozilla.org/zh-CN/docs/Web/CSS/width" target="_blank">css-width</a>

```
width 属性用于设置元素的宽度。width 默认设置内容区域的宽度，但如果 box-sizing 属性被设置为 border-box，就转而设置边框区域的宽度。
width 属性也指定为：
    1. 下面关键字值之一：min-content（元素内容固有的最小宽度），max-content（元素内容固有的（intrinsic）合适宽度），fit-content（取以下两种值中的较大值：固有的最小宽度、固有首选宽度（max-content）和可用宽度（available）两者中的较小值），auto。
    2. 一个长度值 <length> 或者百分比值 <percentage>。
```

