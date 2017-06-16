---
title: 我所了解的git
date: 2017-04-09 10:20:38
tags: git
categories: 通用技能
---

# 缘由：git相关资料，我自己写的比较杂乱，在此合并一处，以后git看这里就够了

<!--more-->

# 一、git基本使用
## 1-1、常用操作
* 查看分支：git branch

* 创建分支：git branch <name>

* 远端拉取分支到本地：git checkout -b master origin/master

* 切换分支：git checkout <name>

* 创建本地分支：git checkout -b [branch-name]

* 将创建的本地分支上传到远端服务器：git push origin [branch-name]

* 删除本地分支：git branch -D [branch-name]

* 删除远端分支：git push origin :[branch-name]  --注意origin后面接一个空格

* 创建+切换分支：git checkout -b <name>

* 合并某分支到当前分支：git merge <name>

* 删除分支：git branch -d <name>

* 关联邮箱：git config --global user.email "你的邮件地址"

* 关联用户名：git config --global user.name "你的Github用户名"

## 1-2、版本回退与放弃修改
* 放弃本地所有修改：git checkout .

* 回退到指定版本：git reset --hard commit_id

## 1-3、推送远端操作
* 拉取更新操作：git pull

* 本地代码更新或有新文件加入：git add .

* 查看git的状态：git status

* 提交修改的文件到远端：git commit -m "xxx(备注)"

* 推送到远端服务器：git push

# 二、提升git clone的办法
## 2-1、使用浅复制
```
	git clone --depth=1 [url] 
```

这样的操作，不会记录历史的版本，只会下载最新的版本，所有下载下来的内容会小的多

## 2-2、使用终端代理的方法
[git-clone太慢怎么办](http://www.tuicool.com/articles/a2m6fau)

# 三、遇到的问题
## 3-1、vps的ssh中使用git遇到的坑
### 3-1-1、一些没注意到的知识
git的clone其实是分2种的，网上很多的说法也不靠谱，最靠谱的还是需要自己去理解其原理，追本溯源，方得始终。

git的clone操作有ssh模式和https模式，在vps操作下其实使用的是ssh，故clone的时候需要选中ssh模式的外链

[github的ssh模式和https模式](https://help.github.com/articles/which-remote-url-should-i-use/)

### 3-1-2、在vps中使用git提交不上去
一、修改.git中的config文件，将https改为ssh

二、查看.ssh文件夹下面有没有id_rsh.pub文件，就是ssh加密的公钥文件，git使用ssh加密上传的，这里我目前也不太懂，有空自己搭建一个git就知道了

[.ssh配置相关](https://help.github.com/articles/connecting-to-github-with-ssh/)

在有官方的参考文件后，我还是一直遇到坑，就是一直报我提交的公钥有问题，自己没有办法解决，还是网上找的，有时候遇到问题自己还是先搜搜吧，就是应为vim复制的问题

使用cat命令打印到终端，然后在复制就好了，orz

【cat ~/.ssh/id_rsa.pub】
