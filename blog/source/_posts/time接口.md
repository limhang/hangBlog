---
title: time接口
date: 2017-06-12 17:23:08
tags: interface
categories: interface
---

# 缘由：时间管理应用的接口

<!--more-->

# 一、登录注册模块接口
注册接口
```
url:http://time.coderhelper.cn/v1_0/person/user/register
请求方式：post
参数：username password
```

登录接口
```
url:http://time.coderhelper.cn/v1_0/person/user/login
请求方式：post
参数：username password
```

# 二、时间管理模块接口
```
http://time.coderhelper.cn/v1_0/time/user/create?token=7ff6dccc65f9a80cbdffa0d5cf6369ec&timeType=2&duration=20&timedesc=xxxx&loadTime=2017
```

```
CREATE TABLE manager(
   id INT NOT NULL AUTO_INCREMENT,
   timeType INT NOT NULL,
   duration VARCHAR(50) NOT NULL,
   user_id VARCHAR(50) NOT NULL,
   timedesc VARCHAR(200) NOT NULL,
   loadTime VARCHAR(100) NOT NULL,
   PRIMARY KEY ( id )
)ENGINE=InnoDB DEFAULT CHARSET=utf8;
```