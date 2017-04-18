---
title: nginx配置信息
date: 2017-04-18 15:07:18
tags: nginx配置
categories: nginx
---

# 缘由：nginx配置信息示例，如下，方便以后查看

<!--more-->

# 代码如下：
```
# For more information on configuration, see:
#   * Official English Documentation: http://nginx.org/en/docs/
#   * Official Russian Documentation: http://nginx.org/ru/docs/

user nginx;
worker_processes auto;
error_log /var/log/nginx/error.log;
pid /var/run/nginx.pid;

# Load dynamic modules. See /usr/share/nginx/README.dynamic.
include /usr/share/nginx/modules/*.conf;

events {
    worker_connections  1024;
}

   http {
        server {
            listen       80;
            server_name  localhost;
            root  /usr/share/nginx/html;
            autoindex on;
            autoindex_exact_size off;
            autoindex_localtime on;

			include /etc/nginx/mime.types;
			#告诉客户端 不缓存 静态资源文件	
			add_header Cache-Control no-cache;
			location ~.*\.(js|css|html|png|jpg)$
			{
				    #expires    3d;
			}
            location / {
                index  index.html index.htm index.php;
                try_files $uri $uri/ /index.php$is_args$args;
            }

            location ~ \.php {
                fastcgi_pass   unix:/var/run/php-fpm/php-fpm.sock;
                #fastcgi_pass   127.0.0.1:9000;
                fastcgi_index  index.php;
                fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
                #fastcgi_param  PATH_INFO $fastcgi_script_name;
                include        fastcgi_params;
            }
        }
        server {
            listen       80;
            server_name  api.lmhang.com;
            root  /usr/share/nginx/html/lumen/public/index.php;
            autoindex on;
            autoindex_exact_size off;
            autoindex_localtime on;

            include /etc/nginx/mime.types;
            #告诉客户端 不缓存 静态资源文件   
            add_header Cache-Control no-cache;
            location ~.*\.(js|css|html|png|jpg)$
            {
                    #expires    3d;
            }
            location / {
                index  index.html index.htm index.php;
                try_files $uri $uri/ /index.php$is_args$args;
            }

            location ~ \.php {
                fastcgi_pass   unix:/var/run/php-fpm/php-fpm.sock;
                #fastcgi_pass   127.0.0.1:8500;
                fastcgi_index  index.php;
                fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
                #fastcgi_param  PATH_INFO $fastcgi_script_name;
                include        fastcgi_params;
            }
        }
    }

```

# nginx相关操作
nginx -s reload  ：修改配置后重新加载生效
nginx -s reopen  ：重新打开日志文件
nginx -t -c /path/to/nginx.conf 测试nginx配置文件是否正确

关闭nginx：
nginx -s stop  :快速停止nginx
         quit  ：完整有序的停止nginx

其他的停止nginx 方式：

ps -ef | grep nginx

kill -QUIT 主进程号     ：从容停止Nginx
kill -TERM 主进程号     ：快速停止Nginx
pkill -9 nginx          ：强制停止Nginx



启动nginx:
nginx -c /path/to/nginx.conf

平滑重启nginx：
kill -HUP 主进程号