---
title: centos6.9安装python3环境
date: 2017-06-07 13:19:24
tags: python
categories: python
---

# 缘由：阿里云多语言版本无python3开发环境，安装python3开发环境

<!--more-->

# 1、安装编译python的组件gcc等

```
yum install gcc
yum groupinstall "Development tools"
```

# 2、再安装依赖包

```
yum install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel     readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel
```

# 3、下载Python3.5的源码包并编译

```
wget https://www.python.org/ftp/python/3.5.2/Python-3.5.2.tgz #其中3.5.2可修改成自己想要的版本
tar xf Python-3.5.2.tgz
cd Python-3.5.2
./configure --prefix=/usr/local --enable-shared  #把python3安装在此目录，这里注意下，后面有详解
make
make install
```

# 4、创建Python3.5软连接

```
ln –s /usr/local/bin/python3 /usr/bin/python3   #记住这些目录很重要
```

# 5、出现错误
error while loading shared libraries: xxxxxxxx: cannot open shared object file: No such file or directory（xxxx为文件名）

解决办法：我是采用修改bash_profile

```
export LD_LIBRARY_PATH=/usr/local/lib/
```

# 6、参考
[python3安装--简书](http://www.jianshu.com/p/4cdc00388457)