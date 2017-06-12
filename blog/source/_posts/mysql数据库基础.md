---
title: mysql数据库基础
date: 2017-06-12 16:26:41
tags: mysql
categories: mysql
---

# 缘由：做个记录，方便查看，都是很简单的东西，久了不写就忘

<!--more-->

# 一、创建数据库，给用户赋予权限
创建数据库
```
create database time;
```
给用户权限
```
grant all privileges on *.* to jacob@localhost identified by 'password' with grant option;
```

# 二、创建表
```
CREATE TABLE users(
   id INT NOT NULL AUTO_INCREMENT,
   status INT NOT NULL,
   vip_level INT NOT NULL,
   username VARCHAR(50) NOT NULL,
   password VARCHAR(50) NOT NULL,
   publish_time VARCHAR(80) NOT NULL,
   token VARCHAR(200) NOT NULL,
   PRIMARY KEY ( id )
)ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

