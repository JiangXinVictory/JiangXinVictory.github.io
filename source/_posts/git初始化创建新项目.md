---
title: git初始化创建新项目
date: 2019-12-26 16:33:59
tags:
    - git
---

记录一下git代码管理工具的日常使用
<!-- more -->

```
git init
git add .
git commit -m 'first commit'
git remote add origin ssh...(远程仓库地址)
git push origin master
git checkout -b dev // 创建dev分支
git push --set -u origin dev // 提交dev分支
git merge --no-ff dev // 合并dev分支到当前分支
```
