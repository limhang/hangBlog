---
title: python开发环境配置(window&mac)
date: 2017-04-10 15:56:03
tags: 环境安装与配置
categories: python
---

# 缘由：现在用的mac电脑上系统自带的是2.7版本的python，想用python3.5，需要自行安装，电脑上可以同时有2.7和3.5的版本2个版本的兼容可以使用，虚拟环境的方式（virtualenv）【python三大神器virtualenv, fabric, pip】。

<!--more-->

# 一、python在mac上的安装与配置
## 1-1、安装操作
### 1-1-1、使用homebrew安装python ->brew install python3

遇到的问题：

1、

	【Error: The `brew link` step did not complete successfully
	The formula built, but is not symlinked into /usr/local
	Could not symlink .
	/usr/local/opt is not writable.
	You can try again using:【brew link gdbm】
  
  
 参考信息：
 
 	【Following Alex' answer I was able to resolve this issue; 
 	seems this to be an issue non specific to the packages being 
 	installed but of the permissions of homebrew folders.
	sudo chown -R `whoami`:admin /usr/local/bin】


FROM:

	http://stackoverflow.com/questions/26647412/homebrew-could-not-symlink-usr-local-bin-is-not-writable

我的尝试：

	sudo chown -R `whoami`:admin /usr/local/bin
	
	
2、

	【python3 You must `brew link pkg-config gdbm` before python3 can be installed】
	
参考信息：

	http://www.dongcoder.com/detail-293244.html
	
我的尝试：
输入：【brew link pkg-config gdbm】

出现：/usr/local/share/man/man3 is not writable.

输入：【sudo chown -R het /usr/local/share/man/man3/】

出现：/usr/local/opt is not writable.

输入：【sudo chown -R het /usr/local/opt/】

出现：Error: Could not symlink .

输入：brew link gdbm

出现：Linking /usr/local/Cellar/gdbm/1.12... 12 symlinks created

输入：brew install python3
等待安装。。。
心得：【查找Google，按照英文提示尝试】

3、

	【Error: Your Xcode (8.0) is outdated.Please update to Xcode 8.2 (or delete it).Xcode can be updated from the App Store.】
	
参考信息：

	【It was an overly strict move, and after some debate the decision has been vastly loosened in 12aad5c136. It will be made generally available with the 1.0.4 release, which will be cut fairly soon. For now there are at least three workarounds:
	Probably the easiest: set the TRAVIS environment variable:
	export TRAVIS=1
	Manually check out the master branch;
	Manually check out the 1.0.2 tag.
	Sorry for the inconvenience.】
	
我的尝试：
	
	export TRAVIS=1

结果：出现问题4

4、

	【Error: Permission denied - /usr/local/etc/openssl/misc/c_hash
 	Warning: Bottle installation failed: building from source.】
 	
 参考信息：
 
 	FROM:---http://www.jianshu.com/p/d96feb47b05b

 我的尝试：

	【chmod -R 755 /usr/local/etc/openssl/misc】

结果：
还是有蛮多问题的，但是基本都是按照提示来处理就好，大多出现权限不够等等，如果755还是不可以，可以尝试777安装homebrew和python3基本耗时2个钟头，安装记录都在这里了，这个习惯需要坚持，这个习惯很好@——@

## 1-2、虚拟开发环境，隔离版本
1.安装Virtualenv，pip3 install virtualenv

tip：在安装的时候，应该存在pip2和pip3，python2和python3，但是修改路径不是很好，所以我采用添加版本号的方式，不加的话，mac系统会逐层查找，所有会找到系统自带的python2

[参考信息](https://segmentfault.com/a/1190000006118856)

## 1-3、虚拟开发环境使用
创建虚拟环境：virtualenv caogao ##创建一个caogao的文件夹

也可以指定虚拟环境的版本 virtualenv caogao --python=python3.6

激活虚拟环境：source ./bin/activate

关闭虚拟环境：deactivate

# 二、python在window上的安装与配置
## 2-1、安装配置
* 1、官网下载最新版的python，http://www.python.org/download/【python3.5自带pip】
* 2、修改python的环境配置，有2种方法，方法一：寻常的修改方法，我的计算机，属性...
其实方法一还是挺简单的，直接在环境变量中的系统变量里面设置path为安装路径就可以了
方法二，使用cmd操作，path=%path%;C:\Python ，其中 C:\Python 是Python的安装目录。注意：需要进入到C:\Python目录下操作，在cmd中进入的操作是【D:】
* 3、在cmd中使用python --version，查看是否有安装好
* 4、使用pip安装文件，pip install aiohttp，安装第三包
【pip install aiohttp】包的时候容易报错
【小坑】：在pip install的过程中，一直出现

***
Retrying (Retry(total=4, connect=None, read=None, redirect=None)) after connection broken by 'ReadTimeoutError("HTTPSConnectionPool(host='pypi.python.org', port=443): Read timed out. (read timeout=15)",)': /simple/netifaces/

the above should download, then fail as we don't have python-dev .. but it can't download it to start with

times-out

ReadTimeoutError: HTTPSConnectionPool(host='pypi.python.org', port=443): Read timed out.
***

其实就是外网被墙的原因
【解法】：开始谷歌翻墙插件就好了。。。多用谷歌参考：https://github.com/pypa/pip/issues/1805
【小坑】：在pip install 过程中，在python下的Scripts文件下运行，因为pip不是python的解释型下运行的

## 2-2、在git bash中使用python

在git bash中使用python，需要在前面加上winpty python，才可以

在git bash中用pip安装插件，也需要在script下进行(最好的办法就是在环境变量中添加D:\python\Scripts;)，但是需要加上winpty，eg:winpty pip3 install virtualenv

但是这个都是很大，所以很慢，可以使用镜像来解决，
winpty pip3 install -i url virtualenv

可使用:https://pypi.tuna.tsinghua.edu.cn/simple

[参考1](http://blog.csdn.net/lambert310/article/details/52412059)

在git bash中使用pip 的virtualenv，使用source venv/scripts/activate
[参考2](http://blog.csdn.net/xie_0723/article/details/51958243)
