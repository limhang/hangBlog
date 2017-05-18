---
title: shadowsocks配置须知
date: 2017-05-18 10:08:03
tags: vps使用指南
categories: 通用技能
keywords: 翻墙 shadowsocks 配置 config
description: shadowsocks基本配置，多人共享shadowsocks
---

# 缘由：记录下shadowsocks的基本配置，方便今后查找

# 一、VPS上安装shadowsocks
```
pip install shadowsocks
```

# 二、配置文件配置
```js
{
	"server":"138.128.198.147",
	"timeout":600,
	"method":"aes-256-cfb",
	"port_password":
	{
		"9000":"hehe",
		"9001":"haha",
		"9002":"xiaozuo",
		"9003":"zhanyuan",
		"9004":"minghang"
	},
	"_comment":
	{
		"9000":"hehe",
		"9001":"haha",
		"9002":"xiaozuo",
		"9003":"zhanyuan",
		"9004":"minghang"
	}
}
```

# 三、启动服务
将配置文件保存为/etc/shadowsocks/config.json，然后按配置文件启动shadowsocks：
```
ssserver -c /etc/shadowsocks/config.json -d start
```

停止shadowsocks服务：
```
ssserver -d stop
```

