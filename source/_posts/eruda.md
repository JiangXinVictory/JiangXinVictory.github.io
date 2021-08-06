---
title: eruda移动端页面调试工具
date: 2019-08-15 09:48:20
tags:
    - JavaScript
    - tools
---

> 分享一个适用于移动端页面调试的小工具，方便大家在移动端项目或微信h5页面上线后，能快速定位问题所在。
<!-- more -->

### 工具名称
[eruda.js](https://www.npmjs.com/package/eruda)

### 使用方法

在页面 head 标签内引入如下代码：
```
<script src="https://cdn.bootcss.com/eruda/1.5.4/eruda.js"></script>
<script>
    eruda.init()
</script>
```
当我们在控制台输出内容时，点击页面右下角的工具按钮，就可以打开控制台了。

{% asset_img eruda.png '' %}
``


