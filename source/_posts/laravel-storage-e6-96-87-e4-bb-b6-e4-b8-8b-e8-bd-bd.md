---
title: laravel storage文件下载
tags:
  - laravel
url: 626.html
id: 626
categories:
  - Laravel
  - PHP
date: 2017-10-27 19:00:54
---

    遇到个问题，需要下载文件从服务器，地址在storage目录下，但每次通过storage_path()都是绝对路径，客户端不可能通过绝对路径下载，于是各种找资料：

    ![WX20171027-185809@2x.png](/ueditor/php/upload/image/20171027/1509101866390844.png "1509101866390844.png")

仿着写了下，比较坑的是第一次使用的是post请求。。。。老半天返回的是一个二进制的结果。。

改为get测试，正常，可以直接下

if (!$file_path = $request->file_path){  
 return error('文件路径缺失');  
}  
$path = Backup::getPath();  
$file_path = $path.'/'.$file_path;  
return response()->download($file_path);

小记：以后下载文件用这种方式挺强大的！！！