---
title: python
date: 2017-04-10 15:30:46
tags: python
categories: python
---

# 缘由：我所了解的python

<!--more-->

# 一、python常用模块
## 1-1、requests


[更多请参考](http://docs.python-requests.org/en/latest/user/quickstart/)

## 1-2、lxml
[英文资料](http://lxml.de/tutorial.html#the-xml-function)

[中文参考](http://cuiqingcai.com/2621.html)

## 1-3、mysqlclient
【python于mysql的使用】

使用mysql-python包比较麻烦，其对python3的支持很不友好，在各种尝试和比较之后，选择使用mysqlclient包，这个包也是diagong所推荐的,其实mysqlclient包是mysql-python的一个分支

[mysqlclient使用参考](http://www.cnblogs.com/fnng/p/3565912.html)

## 1-4、系统自带模块
### 1-4-1、datetime模块
时间处理相关的模块，可[参考](http://www.wklken.me/posts/2015/03/03/python-base-datetime.html)

### 1-4-2、urlparse模块
网址信息分析截取模块，可[参考](https://my.oschina.net/guol/blog/95699)

### 1-4-3、re正则匹配模块
正则匹配模块，和perl脚本的正则匹配很像可[参考](http://www.runoob.com/python/python-reg-expressions.html)

正则匹配的[demo](http://www.codeceo.com/article/python-regular-expressions-re-module.html)

[What is the difference between .*? and .* regular expressions](http://stackoverflow.com/questions/3075130/what-is-the-difference-between-and-regular-expressions)

It is the difference between greedy and non-greedy quantifiers.

Consider the input 101000000000100.

Using 1.*1, * is greedy - it will match all the way to the end, and then backtrack until it can match 1, leaving you with 1010000000001.
.*? is non-greedy. * will match nothing, but then will try to match extra characters until it matches 1, eventually matching 101.

All quantifiers have a non-greedy mode: .*?, .+?, .{2,6}?, and even .??.

# 二、项目
## 2-1、美味不用等
### 2-1-1、background
周五下午要去吃海底捞，但是人太多要排队，生意太多火爆，所以借助美味不用等，这个应用来提前预约，然后用这个的人也多，而且不知道什么时候可以下预约的，所以需要自动通知提醒我

### 2-1-2、技术点一
爬虫，这个我也写了一些了，基本没什么难度，但是要找到数据这才是爬虫难的，好嘛，这个美味不用等，没有网页端，在微信中内嵌的html5

用charles抓包，分析了这个请求，然后发现是post的，我不知道怎么去分析post的参数在手机端上，所以借助了一个网站

[http请求在线分析网站](http://www.atool.org/httptest.php)

然后找出这个关键的参数是shopId，最后在去找所以的和美味不用等相关的接口，然后找到了这个字段的值

### 2-1-3、技术点二
爬取到数据之后，定时运行这个脚本就可以了，但是怎么通知我呢，采用邮件和短信通知都是可以的

### 2-1-4、赶着吃饭，粗糙的源码
```python
#-*- coding:utf-8 -*-
import http.client
import urllib
import requests
import json
import time

url = 'http://c-api.mwee.cn/wx_queue/shop/detail'
payload = {'shopId':'127113'}

host  = "106.ihuyi.com"
sms_send_uri = "/webservice/sms.php?method=Submit"
 
#用户名是登录ihuyi.com账号名（例如：cf_demo123）
account  = "C1869xxxx"
#密码 查看密码请登录用户中心->验证码、通知短信->帐户及签名设置->APIKEY
password = "7e46204054e54435141546xxxxx"
 
def send_sms(text, mobile):
    params = urllib.parse.urlencode({'account': account, 'password' : password, 'content': text, 'mobile':mobile,'format':'json' })
    headers = {"Content-type": "application/x-www-form-urlencoded", "Accept": "text/plain"}
    conn = http.client.HTTPConnection(host, port=80, timeout=30)
    conn.request("POST", sms_send_uri, params, headers)
    response = conn.getresponse()
    response_str = response.read().decode()
    conn.close()
    return response_str
 

if __name__ == '__main__':
 
    mobile = "手机号码"
    text = "您的验证码是：000000。请不要把验证码泄露给其他人。"

    r = requests.post(url,data=payload)
    binary = r.content.decode()
    data = json.loads(binary)
    out = data['data']['queueInfo']['queueList'][1]['name']
    print(out)
    if out == '中桌(晚市)':
        print(send_sms(text, mobile))
```
# 参考资料
## 模块参考资料
[系统自带库的官方文档](https://docs.python.org/2/library/index.html)

## 爬虫参考资料
### 参考书籍
《用python写网络爬虫》，可在脚本之家下载，使用微信关注，然后百度云下载
### 网络资源
[豆瓣电影top250爬虫](https://zhuanlan.zhihu.com/p/25228075)

[python学习资料](http://www.imooc.com/article/1451)

[python中文开发社区](http://bbs.pythontab.com/)

[python学习教程](http://docs.pythontab.com/learnpython/01/)

[github-spider-dmoe](https://github.com/lining0806/PythonSpiderNotes)

[python爬虫教程](http://www.lining0806.com/)

[python爬虫git版本](https://piaosanlang.gitbooks.io/spiders/02day/section2.3.html) 
