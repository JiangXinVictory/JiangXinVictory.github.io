---
title: 常用sql语句
date: 2019-12-26 17:44:00
tags:
    - linux
    - sql
---

数据库&数据表增删改查
<!-- more -->

1.进入数据库
```
$ mysql -u root -p
```
2.查看数据库
```
$ show databases;
```
3.创建数据库
```
$ create database ****;
```
4.使用数据库
```
$ use ****
```
5.查看数据表
```
$ show tables;
```
6.创建数据表
```
$ create table **** (**** int not null auto_increment primary key,**** varchar(20),**** varchar(20));
```
7.查看表结构
```
$ desc ****
```
8.写入表数据
```
$ insert into **** (****,****) values(‘****’,’****');
```
9.删除数据库
```
$ drop database ****;
```