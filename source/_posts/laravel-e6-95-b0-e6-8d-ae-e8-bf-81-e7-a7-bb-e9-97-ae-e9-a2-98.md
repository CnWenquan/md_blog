---
title: laravel数据迁移问题
tags:
  - laravel
url: 554.html
id: 554
categories:
  - Laravel
  - PHP
date: 2017-07-26 18:40:43
---

新建table  

    php artisan make:migration create\_tasks\_table

执行迁移：

    php artisan migrate

出现报错：

    ![QQ截图20170726183726.png](/ueditor/php/upload/image/20170726/1501065357668358.png "1501065357668358.png")

猜测是跟mysql 配置有关（允许最大length）

解决办法：

  

一、在 /app/Providers/AppServiceProvider.php中，添加两句话：

use Illuminate\\Support\\Facades\\Schema;

public function boot()  
{  
 Schema::defaultStringLength(191);  
}

![QQ截图20170726183915.png](/ueditor/php/upload/image/20170726/1501065540533242.png "1501065540533242.png")

  

执行composer update

  

二、修改CreateUsersTable类：

  

public function up()  
{  
 if(!Schema::hasTable('users')) {  
 Schema::create('users', function (Blueprint $table) {  
 $table->increments('id');  
 $table->string('name');  
 $table->string('email')->unique();  
 $table->string('password');  
 $table->rememberToken();  
 $table->timestamps();  
 });  
 }  
}