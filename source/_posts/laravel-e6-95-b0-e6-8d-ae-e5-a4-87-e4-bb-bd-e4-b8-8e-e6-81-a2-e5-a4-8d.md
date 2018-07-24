---
title: laravel 数据备份与恢复
tags:
  - laravel
url: 599.html
id: 599
categories:
  - Laravel
  - PHP
date: 2017-09-02 18:07:33
---

    需求是可以自动备份与手动备份，还可以恢复，网页版的！！！

    composer上下载量与点赞最多的一个扩展包作者说处于安全性考虑，没有开放restore功能，于是只好放弃，继续找，大部分只支持laravel4与5.1、5.2.

    找到一个叫cornford/backup的扩展包，可以支持最新版laravel，还有恢复功能，

[https://devhub.io/zh/repos/bradcornford-Backup](https://devhub.io/zh/repos/bradcornford-Backup)  

    参照安装教程与配置，设置app/backup.php文件如下

<?php  
  
return array(  
  
   /*  
   |--------------------------------------------------------------------------  
   | Enable Backup  
   |--------------------------------------------------------------------------  
   |  
   | Enable database backup.  
   |  
   */  
   'enabled' => true,  
  
   /*  
   |--------------------------------------------------------------------------  
   | Path  
   |--------------------------------------------------------------------------  
   |  
   | A database backup path, absolute path, or path relative from public  
   | directory, a trailing slash is required.  
   |  
   */  
   'path' => '../storage/app/backup/',  
  
   /*  
   |--------------------------------------------------------------------------  
   | Filename  
   |--------------------------------------------------------------------------  
   |  
   | A database export filename to use when exporting databases.  
   |  
   */  
   'filename' => 'backup-' . date('Ymd-His'),  
  
   /*  
   |--------------------------------------------------------------------------  
   | Enable Compression  
   |--------------------------------------------------------------------------  
   |  
   | Enable backup compression using gzip. Requires gzencode/gzdecode.  
   |  
   */  
   'compress' => true,  
  
   /*  
   |--------------------------------------------------------------------------  
   | Database Engine Processors  
   |--------------------------------------------------------------------------  
   |  
   | Set the database engines processor location, trailing slash is required.  
   |  
   */  
   'processors' => array(  
      'mysql' => array(  
         'export' => '/usr/local/Cellar/mysql/5.7.19/bin/', //mysqldump执行文件位置  
         'restore' => '/usr/local/Cellar/mysql/5.7.19/bin/'  //mysql执行文件位置  
      )  
   ),  
  
);  

  

**注意1：**

**    backup.php文件中processors的mysql=>export指定mysqldump执行文件位置，mysql=>restore指定mysql执行文件位置。**

**注意2:**

**    由于在config／app.php文件中声明了Backup的全局参数，由于我用的是phpStorm编辑器，故在引入Backup时它提示的引入类的path不代表已声明的Backup，直接 use  Backup就可以。**

**注意3:**

**    在恢复备份时，根据教程的意思是可以直接写保存的文件名就可以到配置路径恢复，但回报文件不存在的错误，处理办法是先Backup::getPath(),然后拿到filepath后跟filename拼接，再次请求就可以了。**