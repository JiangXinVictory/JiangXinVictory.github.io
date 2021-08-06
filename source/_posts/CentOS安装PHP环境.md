---
title: CentOS安装PHP环境
date: 2019-12-26 17:47:45
tags:
    - linux
    - PHP
---

CentOS 安装 Apache 服务器 & PHP 环境。
<!-- more -->

1.安装Apache服务器
```
$ yum -y install httpd
```
2.启动Apache服务器
```
$ systemctl start httpd.service
```
3.设置Apache开机启动
```
$ systemctl enable httpd.service
$ chkconfig --level 2345 httpd on
```
4.安装PHP
```
$ yum -y install php
```
5.改配置文件。顺利的话，可以访问了
6.安装数据库。因为centos7默认会安装mysql的其他分支数据库，不会安装mysql，所以我们需要这样操作
```
$ rpm -Uvh http://dev.mysql.com/get/mysql-community-release-el7-5.noarch.rpm

$ yum repolist enabled | grep "mysql.*-community.*"

$ yum -y install mysql-community-server

// 开机启动mysql
$ systemctl enable mysqld
$ chkconfig --level 2345 mysqld on

// 启动mysql
$ systemctl start mysqld

// 设置密码,需要先按下回车，因为是空密码
$ mysql_secure_installation

// 设置root访问数据库
$ GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'jiangxin' WITH GRANT OPTION;

$ FLUSH   PRIVILEGES;
```
