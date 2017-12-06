---
title: laravel开发环境配置
date: 2017-12-6 10:44:53
tags: [php, laravel, homestead]
categories: php
---

# 缘由：laravel环境配置，准备工作都在这里了
<!--more-->

软件工程中，学习一门技能，主要分2部分
* 一是。搭建环境，学习语言；
* 二是。熟悉框架，使用框架。
做好上述2步，基本就算会了这门技能，提升主要体现在对框架源码的认知高度，对语言实现运行时的理解。概括为，入门体现在会用该技能解决现实中某种问题；提升体现在知道为什么这样做怎样做到的。

本文主要总结归纳php-laravel，这门技能【我之前使用过的】 -- `环境搭建部分`

## 一、vagrant，virtualbox，homestead.box
### 1-1、准备工作
下载vagrant，virtualbox，homestead.box，这三个文件，在我的U盘中，都长期有备份。
virtualbox和vagrant的使用，可参考之前写过的[vagrant&virtualbox的文章](http://blog.coderhelper.cn/%E9%80%9A%E7%94%A8%E6%8A%80%E8%83%BD/varant&virtualBox.html)

### 1-2、安装laravel官网，配置homestead.box
我们进入[laravel官网](https://laravel.com/docs/5.5/homestead),查看homestead.box的配置。
为了以后方便，这里指定接下来使用的box版本号和homestead库分支号。`homestead.box-v5.0.1`,`Homestead库分支-v7.0.1`

* 1.fork下homestead库，这里面有vagrant软件配置项【针对homestead】

我已经fork了一份，以后可以做完常用配置文件，修改保存，[地址](https://github.com/limhang/homestead)

* 2.使用homestead库配置homestead.box

进入homestead库文件夹，切换好branch dev 【这个分支，是我自己使用的】
```
git checkout dev
```

运行脚本，生成Homestead.yaml,aliases,after.sh，这3个文件
```
// Mac / Linux...
bash init.sh

// Windows...
init.bat
```

* 3.配置Homestead.yaml文件

a、设置Homestead.yaml中的Provider配置项
```
provider: virtualbox
```

b、配置共享文件夹
设置Homestead.yaml中的folders配置项
```
folders:
    - map: ~/code
      to: /home/vagrant/code
```

c、配置nginx站点
设置Homestead.yaml中的sites配置项
```
sites:
    - map: hang.com
      to: /home/vagrant/code/Laravel/public
```

设置本地hosts文件
```
192.168.10.10  hang.com
```

* 4.加载Vagrant Box
在homestead文件夹中，运行`vagrant up`
这里一般会出现二个错误，1.提示找不到laravel/homestead的box;2.还是提示找不到laravel/homestead的box

对于问题一
```
在我们使用vagrant box add xx_name file:///xx的时候，我们需要将xx_name 填为laravel/homestead，
因为在scripts/homestead.rb文件中有名称约束
```

对于问题二
我们需要修改scripts/homestead.rb文件中的约束
```
  # Configure The Box
  config.vm.define settings["name"] ||= "homestead-7"
  config.vm.box = settings["box"] ||= "laravel/homestead"
	#config.vm.box_version = settings["version"] ||= ">= 4.0.0"
  config.vm.hostname = settings["hostname"] ||= "homestead"
```
由于我们本地使用的version是0，所以应该将box的版本约束去掉

* 5.一些问题
a、进行到4步之后，可能遇到一些问题，一般是由于网络因素，所以设置终端走代理，小飞机开全局

b、我们如果修改homestead中配置项的话，需要执行`vagrant reload --provision`命令


## 二、配置laravel开发环境【虚拟机中】
### 2-1、修改虚拟机中的composer镜像
在虚拟机终端执行下面代码，配置镜像
```
composer config -g repo.packagist composer https://packagist.phpcomposer.com
```

### 2-2、开始体验laravel
正常情况下，你只需要在浏览器中，输入`hang.com`就可以了。
如果有一些文件夹权限的问题，按照错误信息，修改文件夹权限就可以了




