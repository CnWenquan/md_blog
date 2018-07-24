---
title: Centos 下编译安装fileinfo扩展
tags:
  - php扩展
url: 763.html
id: 763
categories:
  - 服务器
date: 2018-03-06 11:25:47
---

1.检查当前环境
--------

`php -i|grep fileinfo`   
若出现  
`fileinfo`  
`fileinfo support => enabled`  
则代表`fileinfo`扩展可用,否则不可用。

2.安装
----

### 2-1:下载

`wget -O php-5.6.9.tar.gz http://cn2.php.net/get/php-5.6.9.tar.gz/from/this/mirror`

### 2-2:解压

`tar -zxvf php-5.6.9.tar.gz`

2-3:进入对应php版本扩展目录(我这是5.6.9版本)
-----------------------------

`cd /var/php5.6.9/php-5.6.9/ext/fileinfo`

2-4：编译&&安装
----------

/usr/local/php/bin/phpize
./configure -with-php-config=/usr/local/php/bin/php-config
make && make install
vim /usr/local/php/etc/php.ini

2-5：修改php.ini
-------------

加入:`extension=fileinfo.so`