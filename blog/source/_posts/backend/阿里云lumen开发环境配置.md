---
title: 阿里云lumen开发环境配置
date: 2017-05-24 16:38:37
tags: php
categories: 后端开发
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

# 四、使用lumen中遇到的问题
## 4-1、nginx配置问题
查找lumen的root路径配置，发现只用定位到public文件夹就可以了
nginx配置中的index.xxx后缀文件配置也是挺烦的
我的阿里云nginx配置文件如下：

```
user www www;
worker_processes auto;

error_log /data/wwwlogs/error_nginx.log crit;
pid /var/run/nginx.pid;
worker_rlimit_nofile 51200;

events {
  use epoll;
  worker_connections 51200;
  multi_accept on;
}

http {
  include mime.types;
  default_type application/octet-stream;

  log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
              '$status $body_bytes_sent "$http_referer" '
                    '"$http_user_agent" "$http_x_forwarded_for"';
   
  access_log  logs/access.log  main;

  server_names_hash_bucket_size 128;
  client_header_buffer_size 32k;
  large_client_header_buffers 4 32k;
  client_max_body_size 1024m;
  client_body_buffer_size 10m;
  sendfile on;
  tcp_nopush on;
  keepalive_timeout 120;
  server_tokens off;
  tcp_nodelay on;

  fastcgi_connect_timeout 300;
  fastcgi_send_timeout 300;
  fastcgi_read_timeout 300;
  fastcgi_buffer_size 64k;
  fastcgi_buffers 4 64k;
  fastcgi_busy_buffers_size 128k;
  fastcgi_temp_file_write_size 128k;
  fastcgi_intercept_errors on;

  #Gzip Compression
  gzip on;
  gzip_buffers 16 8k;
  gzip_comp_level 6;
  gzip_http_version 1.1;
  gzip_min_length 256;
  gzip_proxied any;
  gzip_vary on;
  gzip_types
    text/xml application/xml application/atom+xml application/rss+xml application/xhtml+xml image/svg+xml
    text/javascript application/javascript application/x-javascript
    text/x-json application/json application/x-web-app-manifest+json
    text/css text/plain text/x-component
    font/opentype application/x-font-ttf application/vnd.ms-fontobject
    image/x-icon;
  gzip_disable "MSIE [1-6]\.(?!.*SV1)";

  #If you have a lot of static files to serve through Nginx then caching of the files' metadata (not the actual files' contents) can save some latency.
  open_file_cache max=1000 inactive=20s;
  open_file_cache_valid 30s;
  open_file_cache_min_uses 2;
  open_file_cache_errors on;

######################## default ############################
  server {
  listen 80;
  server_name _;
  root /data/lumen/public;
  index index.php index.html index.htm;
  access_log /data/wwwlogs/access_nginx.log main;
  location /nginx_status {
    stub_status on;
    access_log off;
    allow 127.0.0.1;
    deny all;
    }

  location / {
	  try_files $uri $uri/ /index.php?$query_string;
  }


  location ~ [^/]\.php(/|$) {
    #fastcgi_pass remote_php_ip:9000;
    fastcgi_pass unix:/dev/shm/php-cgi.sock;
    fastcgi_index index.php;
	fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
    include fastcgi.conf;
    }
  location ~ .*\.(gif|jpg|jpeg|png|bmp|swf|flv|mp4|ico)$ {
    expires 30d;
    access_log off;
    }
  location ~ .*\.(js|css)?$ {
    expires 7d;
    access_log off;
    }
  location ~ /\.ht {
    deny all;
    }
  }

########################## vhost #############################
  include vhost/*.conf;
}
```

## 4-2、.env环境配置
在lumen中，配置开发环境是通过.env文件进行的，出现的错误是无法连接到数据库，这个定位也花了好久时间，最终的错误是，我使用的阿里云多语言环境设置里，连接数据库只能通过localhost，而.env环境中写的是ip地址，127.0.0.1
修改127.0.0.1为localhost就可以了
