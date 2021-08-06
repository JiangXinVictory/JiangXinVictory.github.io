---
title: CentOS安装shadowsocks
date: 2019-12-26 16:45:17
tags:
    - linux
---
很多IT行业的新人，在工作或者学习中，经常需要查看国外的一些技术网站，但是由于一些限制很多网站都无法访问，本文简要概述如何通过自建ss来访问这些网站，希望大家在学习研究的时候，少走一些弯路，禁止用于非法途径，谢谢配合。。。
<!-- more -->

1.使用 root 用户登录
```
wget --no-check-certificate -O shadowsocks-go.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-go.sh
chmod +x shadowsocks-go.sh
./shadowsocks-go.sh 2>&1 | tee shadowsocks-go.log
```
2.安装完成后，脚本提示如下
```
Congratulations, Shadowsocks-go server install completed!
Your Server IP        :your_server_ip
Your Server Port      :your_server_port
Your Password         :your_password
Your Encryption Method:your_encryption_method

Welcome to visit:https://teddysun.com/392.html
Enjoy it!
```
3.配置文件路径：/etc/shadowsocks/config.json
```
{
    "server":"000.000.000.000”,//如果为阿里云专网用户，需要输入私网IP
    "port_password":{
            "8888": "text"
        },
    "local_port":1080,
    "method":"aes-256-cfb",
    "timeout":600
}
```
4.如果是阿里云专网用户，需要在阿里云后台添加安全组规则，添加端口号，双入口配置: TCP和UDP
```
启动：/etc/init.d/shadowsocks start
停止：/etc/init.d/shadowsocks stop
重启：/etc/init.d/shadowsocks restart
状态：/etc/init.d/shadowsocks status
```
5.卸载方法
```
./shadowsocks-go.sh uninstall
```

