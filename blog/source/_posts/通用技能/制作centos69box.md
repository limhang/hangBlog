---
title: 制作centos69box
date: 2017-12-13 10:44:53
tags: [virtualbox vagrant]
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

## 二、最终结果
代码、配置文件，以及所有需要的资源都在我的[github](https://github.com/limhang/centos69)上



