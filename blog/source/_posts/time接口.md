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
添加时间项目接口
```
url:http://time.coderhelper.cn/v1_0/time/user/timecreate
请求方式：post
```

| 参数 | 说明 | 类型 |
| :--- | :----: | ----: |
| duration | 时长 | string |
| timedesc    | 详细说明      | string    |
| timeType |时间分类|string|
| token |令牌指定用户|string|
|loadTime|上传当天时间|string|