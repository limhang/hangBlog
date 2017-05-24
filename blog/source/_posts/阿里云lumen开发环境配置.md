---
title: 阿里云lumen开发环境配置
date: 2017-05-24 16:38:37
tags: php
categories: 后端开发
keywords: php lumen 后端开发 api接口
description: php lumen 开发环境配置
---

# 缘由：阿里云做活动，9元半年vps，就体验了下，在国内用阿里云还是有保障的，今天记录下lumen开发环境的配置

<!--more-->

# 一、安装composer
* 安装composer的4行代码如下：
```
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"

php -r "if (hash_file('SHA384', 'composer-setup.php') === '669656bab3166a7aff8a7506b8cb2d1c292f042046c5a994c43155c0be6190fa0355160742ab2e1c88d40d5be660b410') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"

php composer-setup.php

php -r "unlink('composer-setup.php');"
```

* 将composer做成全局的
```
sudo mv composer.phar /usr/local/bin/composer
```

# 二、安装lumen
* 配置Composer中国镜像，国内用户都懂得。
```
composer config -g repo.packagist composer https://packagist.phpcomposer.com
```

* 建立一个目录为lumen的项目
```
composer create-project --prefer-dist laravel/lumen lumen
```

# 三、安装中出现的问题
```
[Symfony\Component\Process\Exception\RuntimeException]                                   
The Process class relies on proc_open, which is not available on your PHP installation. 
```

解决办法：
```
1.找到php.ini文件，搜索disable_functions
2.删除搜索到的这一行
3.[参考的资料](https://stackoverflow.com/questions/19911737/laravel4-composer-install-got-proc-open-not-available-error)
```
