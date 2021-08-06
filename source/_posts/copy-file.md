---
title: webpack将指定文件打包到dist目录或指定目录
date: 2019-08-09 17:22:35
tags: 
    - webpack
---

如何使用webpack将项目中的某个目录或某个文件，打包到指定的目录下（dist目录）？
<!-- more -->

> 因公司需要对网站进行百度收录统计，所以出现了一个需求，需要将百度收录认证文件放到网站根目录。运维放上去后，发现并未生效，经检查后发现，网站根目录指向的是项目打包目录（dist）。所以需要将这个文件放入前端项目代码中，在进行打包的时候，放入dist目录中。

### 插件 [CopyWebpackPlugin](https://www.webpackjs.com/plugins/copy-webpack-plugin/)
将文件或目录复制到构建目录下。

文件目录如图：
{% asset_img before-copy.png '' %}

想把图片通过 yarn run build-dev 打包时，放入 dist 目录，配置如下：

打开 webpack 配置文件 webpack-dev-conf.js
{% asset_img copy.png %}

然后在 Terminal 输入 yarn run build-dev，打包结束后，查看 dist 目录
{% asset_img copy-after.png %}

