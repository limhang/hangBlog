---
title: urlStore接口
date: 2017-06-21 12:21:21
tags: api
categories: api
---

# 一、urlStore用户模块接口
## 1-1、添加注册接口
```
url:http://urlapi.coderhelper.cn/v1_0/person/user/register
请求方式：post
```

| 参数 | 说明 | 类型 |
| :--- | :----: | ----: |
| username | 账户 | string |
| password | 密码 | string |


## 1-2、添加登录接口
```
url:http://urlapi.coderhelper.cn/v1_0/person/user/login
请求参数：post
```

|参数|说明|类型|
|:---|:---:|---:|
|username|账户|string|
|password|密码|string|

# 二、url内容发布接口
## 2-1、发布url接口
```
url:http://urlapi.coderhelper.cn/v1_0/url/user/urlcreate
请求参数：post
```

|参数|说明|类型|
|:---|:---:|---:|
|token|令牌|string|
|url|需要保持的网址|string|
|category|分类|string|
|tag|标签|string|
|detail|详细描述|string|

```json
{"code":200,"info":"","datas":{"category":"test","tag":"web","detail":"justFortest","userId":"7393ee8db55db7ea99070676e0029972","id":1}}
```

## 2-2、查询url接口
