---
title: Laravel Ide helper
tags:
  - laravel
url: 575.html
id: 575
categories:
  - Laravel
  - PHP
date: 2017-08-09 18:45:55
---

从名字可以看出是一款编辑器辅助扩展包，不经意之间发现的，功能也是特别强大，结合PhpStorm，安装办法：

    1、

composer require barryvdh/laravel-ide-helper

    2、在config/app.php文件中添加注册服务提供者：

Barryvdh\\LaravelIdeHelper\\IdeHelperServiceProvider::class,

    3、运行php artisan命令：

        php artisan clear-compiled

        php artisan optimize

        php artisan ide-helper:generate

    4、效果图：

![WX20170809-184342@2x.png](/ueditor/php/upload/image/20170809/1502275342830388.png "1502275342830388.png")

注意：

    安装过程中遇到个问题：

![WX20170809-184509@2x.png](/ueditor/php/upload/image/20170809/1502275410937755.png "1502275410937755.png")

查找资料说问题出在没有在app.php中添加注册者，但反复检查我市添加的，于是各种试，终于，功夫不负有心人！！执行了下清除缓存再次运行就成功了：

php artisan config:cache