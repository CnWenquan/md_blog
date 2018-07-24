---
title: 'Laravel  The only supported ciphers are AES-128-CBC and AES-256-CBC #9080'
tags:
  - laravel
url: 565.html
id: 565
categories:
  - Laravel
  - PHP
date: 2017-08-01 16:48:59
---

每个人遇到这个问题解决办法都不相同，我的解决办法是：  

1、首先查看config/app.php中是否存在参数：

'cipher' =\> 'AES-256-CBC',  

2、如果存在的话，查看.dev文件是否存在，不存在，复制一份.dev.example文件，mv .dev.example .dev

3、我的APP_KEY不存在，故要生成一下：

php artisan key:generate

重新刷新界面，运行正常。