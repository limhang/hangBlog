---
title: 制作centos69box
date: 2017-12-13 10:44:53
tags: [virtualbox, vagrant]
categories: 通用技能
---

# 缘由：制作centos69box，方便以后日常使用，集成各种开发环境

<!--more-->

# 一、准备工作
## 1-1、资源
centos69box，网上下载最简洁的centos69资源【已上传到百度云】

## 1-2、技能
* shell命令 -- 【待充电】
* vagrant配置命令【参考官网】

以上2个技能，在`1-3`和`1-4`中都会有详解

## 1-3、shell技能快速点亮
### 1-3-1、基本知识
#### a、shell脚本添加注释
sh里没有多行注释，只能每一行加一个#号

#### b、获取当前执行环境的路径
```sh
currentPath=$(cd `dirname $0`; pwd);
```

#### c、判断一个文件是否存在
```sh
#!/bin/bash
file="/etc/hosts"
if [ -f "$file" ]
then
	echo "$file found."
else
	echo "$file not found."
fi
```

#### d、导入一个文件内容到另外一个文件，or逐行read
```sh
# usage : [a.sh xx.txt]
touch load.txt
while IFS='' read -r line || [[ -n "$line" ]]; do
    # echo "Text read from file: $line"
    echo "Text read from file: $line"
    echo "$line" >> load.txt
done < "$1"
```

## 1-4、vagrant配置相关操作，查看之前文档

## 二、github库补充文档
### 2-1、步骤`e、python-virtualenv integrated && mysql configure`补充
### 2-1-1、python-virtualenv搭建
在进行到这一步的时候，我们已经安装好了python3.5。在这个基础上，我们安装`virtualenv`，用它来做python开发环境的隔离。
执行`sudo pip3 install -i https://pypi.tuna.tsinghua.edu.cn/simple virtualenv`，如果报错找不到`pip3`，那么执行`sudo ln -s /usr/local/bin/pip3 /usr/bin/pip3`，如果报错`有某个库文件找不到`，那么执行`sudo ln -s /usr/local/lib/lib_name_wanted /usr/lib/lib_name_wanted`，对于vagrant虚拟机，需要在执行上述软链命令后，重启vagrant配置`vagrant reload --provision`；如果是购买的vps，应该不用重启的操作。

上述操作进行后，已经将python的virtualenv扩展包，安装好了。执行`virtualenv python35 --python=python3.5`命令，就可以创建名为`python35`的文件夹，该文件将的python开发环境被指定为python3.5。开始虚拟环境`source ./bin/activate`，关闭虚拟环境`deactivate`。

### 2-1-2、mysql配置
在执行到该步骤的时候，已经安装好了mysql，但是还没有进行配置。
执行`mysql_secure_installation`，回车，输入y，然后填写密码，就修改了root的账户信息。

### 2-2、步骤`f`步骤
f -- make the python scratch workflow work well. include python-virtualenv-lib && scratch-demo-script

f步骤，主要是搭建一个python爬虫开发环境，在进行到该步骤时，我们已经安装好了mysql，python3.5，virtualenv。其实一切都已经准备就绪。我们只需要将这些工具软件，串起来。为了方便测试，我写了一个拉钩的简单[爬虫](https://github.com/limhang/lagouzhaopingou/blob/master/python/lagou_sql.py)，该爬虫脚本，可以将数据信息写入，数据库(库名自己填)，表名为lagou。该脚本依赖一个库，这个库是链接mysql和python，我之前的文章多次提到这个库。**但是，我发现我之前的文章写的不对，以后就看这篇文章了**，**执行`pip install -i https://pypi.tuna.tsinghua.edu.cn/simple pymysql`命令就可以了**，一切其实很简单的。


## 三、最终结果
代码、配置文件，以及所有需要的资源都在我的[github](https://github.com/limhang/centos69)上
