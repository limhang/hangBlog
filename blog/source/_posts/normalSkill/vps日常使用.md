---
title: vps日常使用
date: 2017-04-14 09:34:43
tags: vps使用指南
categories: 通用技能
---

# 缘由：在使用vps的过程中，也遇到了不少的坑，有些问题没有总结，导致一遍一遍的搜网页，在此一并记录

<!--more-->

# 一、centos6下python2.6升级到python2.7
## 我在centos6环境下使用的步骤
* 更新指令
```
yum -y update
yum groupinstall -y 'development tools'
```
	另外还需要安装 python 工具需要的额外软件包 SSL, bz2, zlib
```
yum install -y zlib-devel bzip2-devel openssl-devel xz-libs wget
```

* 源码安装Python 2.7.x
```
wget http://www.python.org/ftp/python/2.7.8/Python-2.7.8.tar.xz
xz -d Python-2.7.8.tar.xz
tar -xvf Python-2.7.8.tar
```

* 安装详情：
```
# 进入目录:
cd Python-2.7.8
# 运行配置 configure:
./configure --prefix=/usr/local
# 编译安装:
make
make altinstall
# 检查 Python 版本:
[root@dbmasterxxx ~]# python2.7 -V
Python 2.7.8
```

* 设置 PATH
```
export PATH="/usr/local/bin:$PATH"
or 
ln -s /usr/local/bin/python2.7  /usr/bin/python
# 检查
[root@dbmasterxxx ~]# python -V
Python 2.7.8
[root@dbmasterxxx ~]# which python 
/usr/bin/python
```

## 接下来这3步我没有用，我直接使用的虚拟环境
```
virtualenv caogao -p python2.7
```

* 安装 setuptools
```
#获取软件包
wget --no-check-certificate https://pypi.python.org/packages/source/s/setuptools/setuptools-1.4.2.tar.gz
# 解压:
tar -xvf setuptools-1.4.2.tar.gz
cd setuptools-1.4.2
# 使用 Python 2.7.8 安装 setuptools
python2.7 setup.py install
```

* 安装 PIP
```
curl  https://bootstrap.pypa.io/get-pip.py | python2.7 -
```

* 修复 yum 工具
此时yum应该是失效的，因为此时默认python版本已经是2.7了。而yum需要的是2.6 所以：
```
[root@dbmasterxxx ~]# which yum 
/usr/bin/yum
#修改 yum中的python 
将第一行  #!/usr/bin/python  改为 #!/usr/bin/python2.6
此时yum就ok啦
```

参考：[centos6.5下python2.6升级python2.7](https://ruiaylin.github.io/2014/12/12/python%20update/)

# 二、centos6环境下开启定时任务，后台执行
## python自动化脚本运行方案

引子：一直想用vps自动运行我的python脚本，但是有几个问题我一直都懒得去查和解决。。但是做coderhelper数据抓取的时候，我总不能每天都定点去抓数据吧，所以其实还是出于我很懒的原因，才有这个小文章的。

### Q-0
我写的脚本是运行在python3.5上的，所以系统自带的2.7的版本我是运行不了的，而且我习惯使用python的虚拟环境，不想污染自己的开发环境，那么我必须启动一个脚本然后打开运行环境，然后运行我的python脚本，在运行完成后，在退出

解决：我尝试使用python去写脚本，运行之后，发现没什么作用，搜索之后，发现网上关于这个的解决也比较少，大多是说source应该替换为‘.’，后来我又使用了shell脚本，但是效果是一样的，后来我意识到，其实是起作用了的，只是打开了activate环境，然后马上就关闭了而已，不会出现自己终端运行出现的(caogao)，这种现象。

### Q-1
怎样让脚本自动运行在vps上，关键词是crontab

第一步，使用crontab -e命令，可能出现的错是“Error detected while processing /root/.vimrc:”----解决：


```
[root@~]# vim ~/.bashrc
 
export EDITOR=/usr/bin/vim
  
[root@~]# source ~/.bashrc
```

第二步，编辑要定时执行的脚本，eg：


```
每天凌晨3:00执行备份程序：0 3 * * * /root/backup.sh

每周日8点30分执行日志清理程序：30 8 * * 7 /root/clear.sh

```

第三步，让root用户生效


```
crontab -u root /var/spool/cron/root
```

第四步，重启crontab服务


```
service crond restart

```

### Q2
能不能给个参考的定时python任务啊，当然：
```sh
  #!/bin/sh
  cd /root/python_blog
  source ./bin/activate
  cd /root/python_blog/python_goose/hang/sis
  python list.py
  deactivate
```

### Q3
我用的是服务器在美国洛杉矶，所以系统时间上是有时间差的
比如：我在定时器中使用20 07 换算到中国大陆就是下午7点20，如果是15 22 换算到中国大陆就是上午十点15分

## 参考
[第三步和第四步](http://blog.csdn.net/a1264716408/article/details/52523645)

[第一步和第二步](https://www.vpser.net/manage/crontab.html)


# 三、centos6.5环境下vsftpd安装与配置
* 以root身份在终端输入yum install vsftpd
* 添加jacob上传用户，以root身份在终端输入useradd jacob,并通过passwd jacob设置密码，用于上传;
* 修改vsftpd配置文件，通过命令cd /etc/vsftpd/ && vim user_list打开文件，在该文件最下方添加jacob用户， 在该目录下通过命令vim vsftpd.conf, 将userlist_enable的值改为NO
* 通过命令重启vsftpd,并通过命令service iptables stop和setenforce 0关闭防火墙

参考：[centos6.5下vsftpd安装](http://jingyan.baidu.com/article/636f38bb4923fbd6b8461016.html)

# 四、vps使用小技巧
## 4-1、nohup + & 指令做到后台运行程序
这个需求来自于我下载vagrant box，我在vps上下载好了box，然后通过bypy上传到百度云，但是box很大，我需要后台运行上传的任务。具体的指令如下：
```
nohup bypy upload xxx.box &
```

现在开始讲解一下这里的2个指令，第一个是&，这个指令是让程序在后台运行，但是如果关闭了终端，则任务还是会结束。
在指令最前面加上nohup，则当我关闭了vps的终端，程序还是可以在后台运行，具体进程查看可以使用top指令，查看后台任务也可以使用jobs。

这里扩展一下查看进程和任务的命令。jobs,top,ps[process status]

**info:***
[如何在后台运行 Linux 命令并且将进程脱离终端
](https://linux.cn/article-7918-1.html)

[ps查看器](http://linuxtools-rst.readthedocs.io/zh_CN/latest/tool/ps.html)