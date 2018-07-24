---
title: composer sabre/xml一些操作修改
tags:
  - laravel
  - PHP
url: 606.html
id: 606
categories:
  - Laravel
  - 未分类
date: 2017-09-27 15:01:52
---

    安装就不多说啦：把官网的安装教程爬过来：

Installing with Composer
------------------------

If Composer is not yet on your system, [follow the instructions on getcomposer.org](https://getcomposer.org/doc/00-intro.md#installation-nix) to install it.

From your project directory, the easiest is then to simply call:

composer require sabre/xml

After running this, sabre/xml should be installed, and you can load it in by including the autoloader:

include 'vendor/autoload.php';

    修改：需要在头部xml标签中加入 version与encoding，但它的默认版本只支持version的显示，查询源代码，发现它留了修改encoding的口子，于是改源码：

![图片.png](/ueditor/php/upload/image/20170927/1506495958667328.png "1506495958667328.png")

现在就支持encoding的显示了