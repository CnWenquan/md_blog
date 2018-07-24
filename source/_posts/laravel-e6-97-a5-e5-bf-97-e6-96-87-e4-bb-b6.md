---
title: laravel 日志文件
tags:
  - laravel
url: 581.html
id: 581
categories:
  - Laravel
  - PHP
date: 2017-08-10 15:02:43
---

安装：

composer require rap2hpoutre/laravel-log-viewer

Add Service Provider to `config/app.php` in `providers` section

Rap2hpoutre\\LaravelLogViewer\\LaravelLogViewerServiceProvider::class,

Add a route in your web routes file:

Route::get('logs', '\\Rap2hpoutre\\LaravelLogViewer\\LogViewerController@index');

  

测试：

public function logTest(){  
 \\Log::info('info Log Test');  
 \\Log::warning("warning Log Test");  
 \\Log::error("error Log Test");  
}

  

效果：

![WX20170810-150356@2x.png](/ueditor/php/upload/image/20170810/1502348540516640.png "1502348540516640.png")