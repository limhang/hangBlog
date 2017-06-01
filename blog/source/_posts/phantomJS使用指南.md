---
title: phantomJS使用指南
date: 2017-05-31 11:36:45
tags:
categories:
---

# 缘由：phantomjs初体验

<!--more-->

# 一、安装指南

## 1-1、依赖库的安装
```
yum install fontconfig freetype freetype-devel fontconfig-devel libstdc++
```

## 1-2、下载安装包
需要自行结合自己系统版本，32位还是64位
例子：
```
wget https://bitbucket.org/ariya/phantomjs/downloads/phantomjs-1.9.8-linux-x86_64.tar.bz2
```

查看自己系统的版本：
```
cat /proc/version
```

## 1-3、初始化
* 创建文件夹
```
mkdir -p /opt/phantomjs
```

* 解压下载包
```
tar -xjvf ~/downloads/phantomjs-1.9.8-linux-x86_64.tar.bz2 -C /opt/phantomjs/
```

* 全局使用
```
ln -s /opt/phantomjs/bin/phantomjs /usr/bin/phantomjs
```

## 1-4、测试是否安装成功
```
phantomjs /opt/phantomjs/examples/hello.js
```

# 二、基本使用
