---
title: Lumen配合WorkerMan实现长链接
date: 2017-07-19 11:23:22
tags: lumen
categories: lumen
---

# 缘由：Lumen框架下长链接的使用

<!--more-->

# 一、所用工具
* 后端相关：
```
lumen框架
redis数据库
workerman(gatewayworker&&gatewayclient)
事件通知，广播
```
* 前端相关：
```
dva
react
```

# 二、流程图



# 三、lumen框架下redis使用（lumen5.4）
## 3-1、安装依赖
```
"require": {
        "php": ">=5.6.4",
        "laravel/lumen-framework": "5.4.*",
        "vlucas/phpdotenv": "~2.2",
        "illuminate/redis": "^5.4",
        "predis/predis": "1.1.*"
    },
```
执行composer install

## 3-2、修改配置项
在bootstrap/app.php
文件中把
```
$app->withFacades();

$app->withEloquent();
```
注释取消
并且注册redis类
代码如下
```
$app->register(Illuminate\Redis\RedisServiceProvider::class);
```

在.env下，修改配置如下：
```
CACHE_DRIVER=redis
```

在config文件夹下，查看database.php是否有如下代码：
```
'redis' => [  
  
        'cluster' => env('REDIS_CLUSTER', false),  
  
        'default' => [  
            'host'     => env('REDIS_HOST', '127.0.0.1'),  
            'port'     => env('REDIS_PORT', 6379),  
            'database' => env('REDIS_DATABASE', 0),  
            'password' => env('REDIS_PASSWORD', null),  
        ],  
```

## 3-3、基本使用
```
use Illuminate\Support\Facades\Cache;

Cache::put('roomId', $roomId, 60);
Cache::put('userName',$userName,60);
```
详细见参考资料3-3-2

## 3-4、参考资料
1、[lumen5.4下redis使用配置](http://www.jianshu.com/p/6f543adac732)
2、[lumen官方cache使用](http://d.laravel-china.org/docs/5.4/cache)
3、[lumen缓存使用介绍](http://blog.gxxsite.com/lumen-advance-use-cache/)


# 四、GatewayWorker&&GatewayClient使用
## 4-1、基本介绍




# 五、lumen框架的事件和广播


# 六、reactJS中使用webSocket

# 七、项目代码仓库
```
https://github.com/limhang/LandLord
```
