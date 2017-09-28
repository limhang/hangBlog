---
title: 最强工作环境virtualBox
date: 2017-04-17 10:44:53
tags: virtualBox
categories: 通用技能
---

# 缘由：一款可移动的工作环境，一款可备份的工作环境，今后换电脑，不用再折腾各种环境软件配置了

<!--more-->

# 一、vagrant初级使用指南
## 1-1、vagrant与virtualbox简介
这里有一些概念，我们需要提前弄明白，不然困惑会一直存在。什么是virtualbox，它是一款开源的虚拟机，可在其上运行各种操作系统；什么是virtualbox，它是一款应用软件，通过它用户可以操作各种操作系统。
[虚拟机的解释wiki](https://zh.wikipedia.org/wiki/%E8%99%9B%E6%93%AC%E6%A9%9F%E5%99%A8)
[virtualbox和vagrant介绍](https://www.taniarascia.com/what-are-vagrant-and-virtualbox-and-how-do-i-use-them/)

## 1-2、安装准备
* 下载 VirtualBox
* 下载  Vagrant
* 下载需要安装的virtualbox，接下来以laravel/homestead为例
在这一步中，由于防火长城的原因，导致使用vagrant box add xxx会非常的慢，所以我一般使用手动安装导入。具体的思路就是在自己的vps上下载下来box,wget url，然后使用bypy上传到百度云，然后下从百度云上拉下来。到此基本的需要软件就弄好了

## 1-3、使用示例
* 下载my.box
* 将my.box文件添加到vagrant中，【vagrant box add xx_name file:///User/het/xxxx/my.box】
* 创建一个文件夹vagrant_laravel，进入文件夹，执行【vagrant init】，生成了Vagrantfile
* 配置Vagrantfile文件，将config.vm.box改为xx_name，保存关闭
* 执行vagrant up，开启改虚拟机，然后执行vagrant ssh进入该虚拟机

上述步骤就是最基本的vagrant使用指南，当然，当我们修改了虚拟机，需要保存备份做快照，怎么办呢，

* 执行【vagrant package】,打包当前的虚拟环境，在当前目录生成.box文件
* 对该.box文件执行上述基本操作

[how to sava and share box](https://stackoverflow.com/questions/22181325/saving-and-sharing-changes-made-to-vagrant-box)                                                                        
[官方参考资料](https://www.vagrantup.com/intro/index.html)

## 常用指令
* vagrant box add name file -- 添加box到vagrant，file为本地路径file:///xxx
* vagrant box list -- 列出所有vagrant添加的box
* vagrant global-status --prune -- 列出所有vagrant运行的虚拟机，可以看到该虚拟机的id和name
* vagrant destroy [name/id] -- 删除vagrant运行的虚拟机和global-status指令结合起来使用
* vagrant box remove name -- 删除名字叫name的box，和vagrant box add结合使用


# 二、vagrant进阶使用
## 2-1、vagrant基本使用的弊端
xxx的进阶使用，是不是很熟悉，一个东西之所以有进阶高阶使用，要不是因为它不够优雅简洁，要不是就有局限性，换句话就是做的不够做的不好，想明白vagrant基本使用中，哪里做的不够做的不好，就明白怎样进阶使用了。

* 使用虚拟机略显麻烦，我们需要使用vagrant ssh然后编辑代码，这样我们没法使用sublime或者ws之类的工具了，有没有一种办法可以编辑本地文件，然后自动同步到虚拟机呢？
* 使用虚拟机，无法较为方面的模拟网站开发，域名打开麻烦

ok，我们列举了一些不足，那么怎样进阶使用呢，下面以laravel/homestead项目为例，剖析

## 2-2、homestead是如何做的
我们进入laravel官网，它们提供了一个vagrant的配置文件，Vagrantfile文件是用ruby语法编写的

```ruby
# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'json'
require 'yaml'

VAGRANTFILE_API_VERSION ||= "2"
confDir = $confDir ||= File.expand_path(File.dirname(__FILE__))

homesteadYamlPath = confDir + "/Homestead.yaml"
homesteadJsonPath = confDir + "/Homestead.json"
afterScriptPath = confDir + "/after.sh"
aliasesPath = confDir + "/aliases"

require File.expand_path(File.dirname(__FILE__) + '/scripts/homestead.rb')

Vagrant.require_version '>= 1.9.0'

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
    if File.exist? aliasesPath then
        config.vm.provision "file", source: aliasesPath, destination: "/tmp/bash_aliases"
        config.vm.provision "shell" do |s|
            s.inline = "awk '{ sub(\"\r$\", \"\"); print }' /tmp/bash_aliases > /home/vagrant/.bash_aliases"
        end
    end

    if File.exist? homesteadYamlPath then
        settings = YAML::load(File.read(homesteadYamlPath))
    elsif File.exist? homesteadJsonPath then
        settings = JSON.parse(File.read(homesteadJsonPath))
    else
        abort "Homestead settings file not found in #{confDir}"
    end

    Homestead.configure(config, settings)

    if File.exist? afterScriptPath then
        config.vm.provision "shell", path: afterScriptPath, privileged: false
    end

    if defined? VagrantPlugins::HostsUpdater
        config.hostsupdater.aliases = settings['sites'].map { |site| site['map'] }
    end
end
```
在阅读上述代码之前，我们需要弄明白几个知识点yaml是一个文件格式，可被多种语法识别类似于json，require语法注意下。
其实核心的内容很短，想要完全弄明白还是需要参考vagrant官方，这里大概说下
```
if File.exist? aliasesPath
```
这里是将本地的alias文件复制到虚拟机上

```
if File.exist? homesteadYamlPath
```
这里是将配置vagrant配置项作用到虚拟机上，开始的时候我这里不明白，原来是看漏了一句代码
```
require File.expand_path(File.dirname(__FILE__) + '/scripts/homestead.rb')
```
上述代码是导入一个ruby函数，然后将yaml文件的配置项导入到虚拟机

```
if File.exist? afterScriptPath then
```
这里是开启虚拟机之前的一个运行脚本

```json
---
ip: "192.168.10.10"
memory: 2048
cpus: 1
provider: virtualbox

authorize: ~/.ssh/id_rsa.pub

keys:
    - ~/.ssh/id_rsa

folders:
    - map: ~/Code
      to: /home/vagrant/Code

sites:
    - map: homestead.app
      to: /home/vagrant/Code/public

databases:
    - homestead

# blackfire:
#     - id: foo
#       token: bar
#       client-id: foo
#       client-token: bar

# ports:
#     - send: 50000
#       to: 5000
#     - send: 7777
#       to: 777
#       protocol: udp
```
这个算是homestead对于虚拟机的配置了

**结论：**laravel/homestead算是良心之作，核心功能我们都需要，也没加太多私货，以其当模板，我们能省不少功夫。

## 2-3、homestead部分详细说明
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


