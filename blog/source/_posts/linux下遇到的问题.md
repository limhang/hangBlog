---
title: linux下遇到的问题
date: 2017-06-29 18:22:32
tags: linux
categories: linux
---

问题一、
问题描述：给centos6.8配置virtualenv的时候，使用【pip3 install -i https://pypi.tuna.tsinghua.edu.cn/simple virtualenv】指令，出现错误：
```
python3.5: error while loading shared libraries: libpython3.5m.so.1.0: cannot open shared object file: No such file or directory
```

解决办法：一般这个路径找不到的问题，都是库的配置有问题
* 使用find指令找到库
```
find / -name "libpython3.5m.so.1.0"
```
* 配置$LD_LIBRARY_PATH
```
export LD_LIBRARY_PATH=/usr/local/lib/
```

/usr/local/lib/ 路径就是我的库所在位置

[参考](https://stackoverflow.com/questions/39298681/anaconda-python-virtualdev-cant-find-libpython3-5m-so-1-0-on-windows-subsystem)