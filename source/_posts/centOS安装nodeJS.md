---
title: centOS安装nodeJS
date: 2020-02-04 21:30:03
tags:
    - NodeJS
---

centOS如何安装NodeJS
<!-- more -->

####一、下载最新版 nodejs 安装文件
```
1. 可在 https://nodejs.org/en/download/ 找到最新版本下载地址
2. 在 root 目录下创建 node 文件夹
3. wget https://nodejs.org/dist/v12.14.1/node-v12.14.1-linux-x64.tar.xz
```

####二、解压文件
```
1. xz -d node-v12.14.1-linux-x64.tar.xz
2. tar -xf node-v12.14.1-linux-x64.tar
3. cd node-v12.14.1-linux-x64
```

####三、创建快捷方式
```
1. ln -s ~/node/node-v12.14.1-linux-x64/bin/node /usr/bin/node
2. ln -s ~/node/node-v12.14.1-linux-x64/bin/npm /usr/bin/npm
3. 若后面安装hexo时，出现 command not found，也需要使用该方法进行配置
```

####四、查看是否成功
```
1. 执行 node -v
2. 执行 npm -v
```

