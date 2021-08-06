---
title: canvas绘制文字并实现文字换行效果
date: 2020-03-15 14:45:12
tags:
    - html
    - javaScript
---

个人简单记录一下在小程序内，使用canvas生成分享海报，实现分享文字超长时换行效果，时间紧迫先记录代码，后续补充
<!-- more -->

#### 注意
一、需要注意使用 drawImage 方法时的参数，sx, sy 在绘制完整图片时，应该为0，不要与 x, y 搞混！

```
  // 绘制文字，画板，背景图（根据需求是否需要该参数）
  fillText (ctx, bg) {
    let row = '这里是需要绘制的标题文字内容'
    let rowLength = row.split('').length // 绘制文字内容的长度
    let rowArr = [] // 分割后的数组
    let subIndex = 0
    // 默认字体大小24
    // 因业务需求，超出一行后的文字，需要换行，并且字体变小，所以修改字体大小为18后，重新换算需要多少行
    ctx.font = 'italic 24px MicrosoftYaHei'
    if (ctx.measureText(row).width > 228) {
      ctx.font = 'italic 18px MicrosoftYaHei'
      for (let i = 0; i < rowLength; i++) {
        if (ctx.measureText(row.substring(subIndex, i)).width > 228) {
          console.log(i)
          rowArr.push(row.substring(subIndex, i - 1))
          subIndex = i - 1
        }
      }
      if (subIndex !== rowLength) {
        rowArr.push(row.substring(subIndex))
      }
    } else {
      rowArr.push(row)
    }
    for (let i = 0; i < rowArr.length; i++) {
      if (rowArr[i]) {
        ctx.fillText(rowArr[i], 78, 153 + i * 28)
      }
    }
  }
```
