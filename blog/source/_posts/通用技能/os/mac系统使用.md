---
title: mac系统使用
date: 2017-04-10 12:12:22
tags: 操作系统
categories: 
- 通用技能 
- 操作系统
---

# 缘由：mac系统使用，该文档主要记录日常中使用的各种小技巧

<!--more-->

# 一、mac终端使用
## 1-1、自定义外观
依据自己的审美，设置自己的外观

* 颜色：我选择黑底白字
* 字体sf mono regular 13
* 透明度：85%
* 背景图片：依据自己口味，

![图片01](http://ok2nitkry.bkt.clouddn.com/mac%E7%BB%88%E7%AB%AF%E4%BD%BF%E7%94%A801.png)

## 1-2、常用操作
* 进入全屏模式：cmd + ctrl + f
* 缩放到低栏：cmd + m

## 1-3、自定义快捷操作
最能体现终端功效的，莫过于定义**.bash_profile**文件

查找自己电脑是否有此文件，终端输入cd，到根目录，然后输入ls -a，查看是否有.bash_profile文件，没有自己创建，设置自定义alias命令，保存后，输入source ~/.bash_profile，运行文件，然后才能生效。

我自己的配置文件在github上的软件配置文件夹中。

## 1-4、终端开始翻墙的功能
对于程序员来说，终端是每天必备的利器。可是shadowsocks即使配置全局代理，终端还是不管用，接下来介绍一种简单实用的方法：
如果是默认的 bash，则写入 ~/.bash_profile ，如果是 zsh，则写在 ~/.zshrc

```
alias setproxy="export ALL_PROXY=socks5://127.0.0.1:1080" 
alias unsetproxy="unset ALL_PROXY"
```
上面的端口(1080)和具体的地址，自己可以使用浏览器访问谷歌，然后自己查看就可以了

# 二、文件操作相关
## 2-1、强写ReadOnly文件
有些文件系统设置为只读属性，这个时候chmod模式改写后，还是不行，这时，可以尝试使用vim打开，然后命令模式下输入：

```
:w !sudo tee % > /dev/null
```
[参考](http://stackoverflow.com/questions/8253362/etc-apt-sources-list-e212-cant-open-file-for-writing)

## 2-2、修改文件默认打开的方式
右键点击需要修改的文件，点击显示简介，然后点击打开方式，最后设置完成。

# 三、软件操作相关
## 3-1、设置xx软件直接在终端打开
以sublime为例子

```
alias subl=\''/Applications/Sublime Text 2.app/Contents/SharedSupport/bin/subl'\'
```
**alias为linux的个人偏好设置文件，silicongo中有用到过**

# 四、mac软件使用推荐
* **macdown** 编写markdown的一款软件
* **charles** 抓包工具，设置相关配置后，可抓取https请求