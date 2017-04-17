---
title: 最强工作环境virtualBox
date: 2017-04-17 10:44:53
tags: virtualBox
categories: 通用技能
---

# 缘由：一款可移动的工作环境，一款可备份的工作环境，今后换电脑，不用再折腾各种环境软件配置了

<!--more-->

# 一、安装准备以及手动安装
## 1-1、安装准备
* 下载 VirtualBox
* 下载  Vagrant
* 下载需要安装的virtualbox，接下来以laravel/homestead为例
在这一步中，由于防火长城的原因，导致使用vagrant box add xxx会非常的慢，所以我一般使用手动安装导入。具体的思路就是在自己的vps上下载下来box,wget url，然后使用bypy上传到百度云，然后下从百度云上拉下来。到此基本的需要软件就弄好了

## 1-2、手动安装box
新建文件metadata.json
```js
{
    "name": "laravel/homestead",
    "versions": [{
        "version": "0.4.0",
        "providers": [{
            "name": "virtualbox",
            "url": "file:///Users/het/Desktop/virtualbox/virtualbox.box"
        }]
    }]
}
```

Then run 
```
vagrant box add metadata.json
```

This will install the box with a version and can be confirmed by:

```
$ vagrant box list
laravel/homestead               (virtualbox, 2.1.0)
```
**注意：**这里需要自己就确定版本
[官方vagrant指令](https://www.vagrantup.com/docs/cli/box.html)
[手动安装参考](http://stackoverflow.com/questions/34946837/box-laravel-homestead-could-not-be-found)
# 二、homestead使用步骤
## 2-1、下载配置github上的Homestead
```
cd ~
git clone https://github.com/laravel/homestead.git Homestead

cd Homestead

// 检出所需要的版本...
git checkout v4.0.5

// Mac / Linux...
bash init.sh

// Windows...
init.bat
```
可以查看init.sh代码，知道在mac下，其实就是将3个文件拷贝到**../homestead下**

## 2-2、更多的详细步骤
[详细步骤可见](http://d.laravel-china.org/docs/5.4/homestead)，接下来主要说说这里面没有将明白的地方。

## 2-3、一些问题
### 2-3-1、ip设置
在Homestead.yaml中将ip进行设置，如果是在本地进行调试，可以将ip设置为127.0.0.1

### 2-3-2、ssh密钥设置
在Homestead文件中运行
```
ssh-keygen -t rsa -C "you@homestead"
```
然后直接几个回车就好

### 2-3-3、相关配置
配置共享文件夹
```
folders:
    - map: ~/Code //这个路径是本地的目录路径，这里更新可以直接同步虚拟环境中
      to: /home/vagrant/Code //这个路径是虚拟环境中的路径（vagrant ssh可进入环境）
```

配置nginx站点
```
sites:
    - map: homestead.app
      to: /home/vagrant/Code/Laravel/public
```

这些东西如果更改的话，**warning：**一定要运行：
```
vagrant reload --provision
```
# 参考
[laravel开发环境配置](http://d.laravel-china.org/docs/5.4/homestead)

[homestead离线安装指南](https://quericy.me/blog/827/)
