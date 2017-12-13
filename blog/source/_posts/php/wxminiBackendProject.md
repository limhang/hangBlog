---
title: laravel后端api实现教程
date: 2017-12-8 10:44:53
tags: [php, laravel, 教程, api]
categories: php
---

## 缘由：
写这篇文章，主要是想记录常见的api后台接口实现步骤，以后按照该文档，step by step就可以实现我其他项目的api接口了。其作用等同于前端的组件化方案【所以必须尽可能详尽】。

<!--more-->

开始前工作

* laravel/homestead.box文件
* virtualbox和vagrant软件
* phpstorm软件和xdebug配置 -- 【已写了详细文档】

## 一、在虚拟机中搭建laravel应用
### 1-1、laravel应用安装
在虚拟机中，执行`composer create-project --prefer-dist laravel/laravel wxminibackend`命令，生成一个wxminibackend的文件夹【就是我们的应用】，在浏览器中输入，我们配置好的网址，如果出现laravel官方页面，则配置成功。

**warning:如果配置了xdebug，需要重新启动phpstorm**

### 1-2、mvc整体框架
在应用搭建过程中，我们选择主流的方案，mvc，由于我们是写api接口，所以不会涉及到view，所以重点在于controller和model。

### 1-3、model搭建
#### a、数据库设置
laravel应用中，数据库设置项位于.env中，我们自定义数据库名称，管理员账号和密码，这里设置数据库名称为`wxminicode`

#### b、使用cli生成数据库表的模板
执行`php artisan make:model wxuser -m`命令，生成`wxuser表`，我们可以使用`php artisan help make:model`命令查看刚刚`-m`参数是什么意思。其实`-m`就是migrations的意思，生成一个数据表模板，为实际生成数据库表，做准备。

#### c、完善刚刚生成的数据库模板
在b中，我们生成了`wxminibackend/database/migrations/time_wxusers`文件，其中已经有3个列的数据项，`created_at`,`updated_at `,`id`,其中`increments`函数是控制id的，`timestamps`函数是控制另外两个时间项的。
由于，我们项目需要`userId`，`userName`，`vip`这三个数据项，所以添加代码如下：

```php
public function up()
{
    Schema::create('w_x_users', function (Blueprint $table) {
        $table->increments('id');
        $table->timestamps();
        $table->string('userId');
        $table->string('userName');
        $table->boolean('vip');
    });
}
```

#### d、生成数据表
由于，我们使用了新的数据库，wxminicode，但是用户homestead可能没有权限新建这个数据库，所以我们需要在虚拟机中，手动创建该数据库。
执行`php artisan migrate`命令，则在数据库中，生成WXUser表。

#### e、数据字段安全
对于数据库安全，可以设置过滤条件，将下面代码添加到wxuser.php中
```
protected $fillable = ['userName', 'userId', 'vip'];
```

#### f、重新归档文件位置
将wxuser.php统一放到app\model下

### 1-4、路由搭建
路由就是用户输入请求的接口，路由的设计，也就是整个后端设计的体现。
```
//上传用户个人信息，将数据处理部分，下发给控制器 -- 【联动UserController中的insertUser方法】
Route::post('user','UserController@insertUser');
```


### 1-5、控制器设计
执行`php artisan make:controller UserController`，生成UserController，以便处理user路由下发的逻辑。
在`app/Http/Controllers/UserController.php`中添加下面代码

```php
use App\Model\wxuser;
class UserController extends Controller
{
    public function insertUser(Request $request)
    {
        $info = wxuser::create($request->all());
        return response()->json($info,200);
    }
}
```


## 二、参考
useful information about laravel framework

### 2-1、service container & service provider [practice]

[how to register & use laravel service provider](https://code.tutsplus.com/tutorials/how-to-register-use-laravel-service-providers--cms-28966)

Through this article you could understand what is service container and what is service provider.
Read it step by step you can register service provider and use it accurately.

### 2-2、routers [Built a react APP with a laravel restful backend]

[part 1,laravel 5.5API ](https://code.tutsplus.com/tutorials/build-a-react-app-with-laravel-restful-backend-part-1-laravel-5-api--cms-29442)


Through this article you will create your own restful API .








