---
title: php move_uploaded_file与unlik函数
url: 526.html
id: 526
categories:
  - PHP
date: 2017-06-14 16:14:54
tags:
---

move\_uploaded\_file()函数将上传文件移动到新位置。若成功，则返回true，否则返回false。  

  

语法：    move\_uploaded\_file(file,newloc)    //file=>要移动的文件,newloc=>文件的新位置

  

* * *

unlink()函数删除文件。若成功，则返回true，失败则返回false。  

  

语法：    unlink(filename,context)    //filename=>规定要删除的文件，context=>（可选）规定文件句柄的环境。