---
title: js选择上传图片并展示
date: 2019-12-26 17:24:34
tags:
    - javaScript
---

前端选择图片并展示选择的图片
<!-- more -->

```
function showImage (input) {
  if (input.files && input.files[0]) {
    let reader = new FileReader()
    reader.onload = function (e) {
      let imgSource = e.target.result
    }
    reader.readAsDataURL(input.files[0])
  }
}
```
