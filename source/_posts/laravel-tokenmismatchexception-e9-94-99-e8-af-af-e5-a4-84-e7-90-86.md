---
title: laravel TokenMismatchException错误处理
tags:
  - laravel
  - PHP
url: 609.html
id: 609
categories:
  - Laravel
  - PHP
date: 2017-09-28 17:26:13
---

    在处理xml读取时，抛出TokenMismatchException异常，原来是为了post、put提交请求时一种安全验证机制，但作为一个api后台，不存在表单直接提交的情况，so，简单粗暴解决办法，直接在kannel中隐藏该判断：

![图片.png](/ueditor/php/upload/image/20170928/1506590712688269.png "1506590712688269.png")

并且刷新配置：

php artisan config:cache