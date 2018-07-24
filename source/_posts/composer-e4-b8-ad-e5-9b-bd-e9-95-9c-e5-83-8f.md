---
title: composer 中国镜像
url: 579.html
id: 579
categories:
  - Laravel
  - PHP
date: 2017-08-10 14:56:28
tags:
---

国外的忒慢了！

    1、composer config -g repositories.packagist composer [http://packagist.phpcomposer.com](http://packagist.phpcomposer.com)  

    2、在composer.json文件中添加

    {

        "repositories":\[

            {"type":"composer","url":"[http://packagist.phpcomposer.com"},](http://packagist.phpcomposer.com"},)  

            {"packagist":false}  

        \]  

    }

问题报错：

    [https://getcomposer.org/doc/06-config.md#secure-http](https://getcomposer.org/doc/06-config.md#secure-http)  

只支持https访问请求！

解决办法：在app.php中配置

: {
    :},