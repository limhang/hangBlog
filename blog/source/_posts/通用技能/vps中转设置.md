---
title: vps中转设置
date: 2017-04-14 09:34:43
tags: vps使用指南 hasRemove HLevel1
categories: 通用技能
---

# 缘由：做一个阿里云中转ss流量，方案采用iptables

<!--more-->

# 一、开启iptables中转功能
打开/etc/sysctl.conf，添加下面代码
```
net.ipv4.ip_forward=1
```

下面的这个步骤被我第一次写文章时省略了，结果坑了自己好久
```
sysctl -p
```

# 二、设置iptables
终端输入，其中39.108.82.4为本机的ip地址，阿里云需要填写的是内网地址，参考步骤4
```
iptables -t nat -A PREROUTING -p tcp --dport 8381 -j DNAT --to-destination 138.128.198.147:8381

iptables -t nat -A POSTROUTING -p tcp -d 138.128.198.147 --dport 8381 -j SNAT --to-source 39.108.82.4

iptables -t nat -A PREROUTING -p udp --dport 8381 -j DNAT --to-destination 138.128.198.147:8381

iptables -t nat -A POSTROUTING -p udp -d 138.128.198.147 --dport 8381 -j SNAT --to-source 39.108.82.4
```

下面的这个步骤也被我第一次写文章时省略了，结果坑了自己好久
```
service iptables save ## 将上一步的配置写入iptabels中
service iptables restart
```
# 三、设置链接配置
本机ss设置中，ip设置为阿里云主机，端口密码还是和之前一样配置

# 四、注意查看阿里云主机的网卡绑定ip地址

```
ifconfig 查看 inet 后面的地址
```