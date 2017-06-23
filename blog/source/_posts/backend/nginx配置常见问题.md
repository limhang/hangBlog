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
