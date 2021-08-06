---
title: formData遍历
date: 2019-12-26 16:58:11
tags:
    - javaScript
---

遍历formData类型数据，查看全部的key=>value
<!-- more -->

```
for(var i of formData.entries()){
    console.log(i[0]+ ', '+ i[1])
}
```
