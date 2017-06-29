---
title: nginx配置常见问题
date: 2017-06-23 09:52:52
tags: nginx
categories: 后端开发
---

# 缘由：Nginx配置问题，location，error_log

<!--more-->

# 问题一、location配置问题
问题描述：想用nginx做反向代理，例如【/api/xxx/yyy】检测到/api后，通过location修改proxy_pass，但是location正则一直没法匹配上，一直匹配到【location /】

解决办法：应该是location正则匹配的优先级的问题，采用【location ~ /api/[^\/]+/[^\/]+】匹配

[参考资料](https://blog.coding.net/blog/tips-in-configuring-Nginx-location)


# 问题二、ip:port无法生效
问题描述：需要改变ip的默认port，例如：www.hang.com:81，在nginx中，将listen：81，但是还是无法正常访问

解决办法：之后想到可能是防火墙的原因，修改/etc/sysconfig/iptables，在iptables中添加81端口，然后重启iptables，命令：service iptables restart，这样修改之后还是不可以，这个时候想到可能是阿里云那边的安全限制，果然是这样，修改阿里云的esc下的安全组设置就可以了

[参考资料](https://stackoverflow.com/questions/5009324/node-js-nginx-what-now)