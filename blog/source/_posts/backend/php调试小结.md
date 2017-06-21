---
title: php调试小结
date: 2017-06-13 10:52:53
tags: php
categories: 后端开发
---

# 缘由：排除一切不可能的，剩下的即使再不可能，那也是真相——Sherlock Holmes

<!--more-->

# 一、php初级调试技巧
使用nginx+php开发服务器，遇到500错误，是非常头痛的，良好的调试技巧，可以快速定位。

## 1-1、php.ini中设置display_errors
* 设置display_errors = On
* 重启php-fpm，使设置生效，service php-fpm restart
上述的设置在开发环境中，是很有效的，可以直接将错误信息显示在浏览器上，但是生成环境千万不能这样做，这样会让黑客探测到很多服务器的信息，很容易被黑

## 1-2、php.ini中设置log_errors
* 设置log_errors = On	
* error_log 设置错误信息存放文件，例如："/usr/local/php56/var/log/php-error.log"
* 设置刚刚log文件的读写属性，我在这里被坑过一次，chmod 777 log
* 重启php-fpm，使设置生效，service php-fpm restart
上述的设置可以在生成环境中使用，出现问题可以直接到指定文件夹中去找错误报告

## 1-3、php-fpm.conf中设置
主要设置2个地方，一个是catch_workers_output = yes 一个是error_log = log/php-fpm.log也就是错误的位置

