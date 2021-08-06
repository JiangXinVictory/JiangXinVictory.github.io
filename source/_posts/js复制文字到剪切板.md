---
title: js复制文字到剪切板
date: 2019-12-26 17:17:50
tags:
    - javaScript
---

js实现复制文本到剪切板
<!-- more -->

1.这里只记录外链引入使用方法，npm方式更为简单，主要是记录这个库的名字
```
<script src="//wx.china.cn/wxscene/js/clipboard-1.7.1.js"></script>

<div v-for="(item, index) in textArr">
    <div class="doc_top">{{item}}</div>
    <div :id="'doc_text_' + index" style="font-size: 0;display: inline-block;opacity: 0;">{{item}}</div> <!—防止复制后文字有选中效果，所以从这个隐藏标签中选文字-->
    <div class="doc_bottom" :class="'btn' + index" @click="copy(index)" data-clipboard-action="copy" :data-clipboard-target="'#doc_text_' + index"></div> <!—按钮，需要动态绑定class，target为上边的文字容器id-->
</div>

copy(index) {
    let str = 'btn' + index; // 按钮的class名，通过元素标签动态绑定的
    let clipboard = new Clipboard('.' + str);
    clipboard.on('success', function(e) {
        // 复制成功
    });
}
```
