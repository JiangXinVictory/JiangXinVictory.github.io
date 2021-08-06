---
title: centOS安装nginx服务器
date: 2020-02-04 21:11:15
tags:
    - linux
---

centOS服务器如何安装nginx服务器及配置https服务
<!-- more -->

####一、先下载 nginx
```
1. 先安装所需依赖
yum install -y gcc-c++ pcre pcre-devel zlib zlib-devel openssl openssl-devel
2. 使用 wget 命令下载安装文件（可去nginx官网查看最新版本）
wget http://nginx.org/download/nginx-1.17.8.tar.gz
3. 解压文件
tar -zxvf nginx-1.17.8.tar.gz
4. 进入解压目录并执行配置
./configure --prefix=/usr/local/nginx --user=www --group=www --with-http_stub_status_module --with-http_ssl_module --with-ipv6
5. 安装
make
make install
6. 查找 nginx 安装目录，一般默认在 /usr/local/nginx
whereis nginx
7. 启动 nginx
./nginx
8. 若提示 Nginx: [error] open() ＂/usr/local/Nginx/logs/Nginx.pid" failed（2:No such file or directory）
/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
```
