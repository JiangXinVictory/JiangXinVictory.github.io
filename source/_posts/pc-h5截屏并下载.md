---
title: pc-h5截屏并下载
date: 2019-12-26 17:22:19
tags:
    - javaScript
---

PC端使用canvas实现截屏并保存图片到本地
<!-- more -->

1.依赖 html2canvas
```
<a class="down" href="" download="downImg">下载</a>

shots () {
  //创建一个新的canvas
  let canvas2 = document.createElement("canvas")
  let _canvas = document.querySelector('.scrollBar’) // 需要截图的容器
  let w = parseInt(window.getComputedStyle(_canvas).width)
  let h = parseInt(window.getComputedStyle(_canvas).height)
  //将canvas画布放大若干倍，然后盛放在较小的容器内，就显得不模糊了
  canvas2.width = w * 2
  canvas2.height = h * 2
  canvas2.style.width = w + "px"
  canvas2.style.height = h + "px"
  //可以按照自己的需求，对context的参数修改,translate指的是偏移量
  //  var context = canvas.getContext("2d")
  //  context.translate(0,0)
  let context = canvas2.getContext("2d")
  context.scale(2,2)
  html2canvas(document.querySelector('.scrollBar'),{canvas:canvas2}).then(function(canvas) {
    document.body.appendChild(canvas)
    //canvas转换成url，然后利用a标签的download属性，直接下载，绕过上传服务器再下载，需要点击a标签下载
    document.querySelector(".down").setAttribute('href',canvas.toDataURL())
  })
}
```
