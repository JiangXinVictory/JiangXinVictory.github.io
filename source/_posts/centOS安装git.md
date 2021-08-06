---
title: centOS安装git
date: 2020-02-05 09:44:05
tags:
    - linux
---

centOS如何安装git
<!-- more -->

####一、安装git
```
yum install git 
```
####二、安装依赖库
```
yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel
yum install gcc-c++ perl-ExtUtils-MakeMaker
```
####三、查看git版本号，确定是否安装成功
```
git  --version
```
####四、如需clone项目，需要配置ssh publickey
1. 没有生成过
```
ssh-keygen -t rsa
```
2. 已生成，通常保存在 ``/root/.ssh/id_rsa.pub``
