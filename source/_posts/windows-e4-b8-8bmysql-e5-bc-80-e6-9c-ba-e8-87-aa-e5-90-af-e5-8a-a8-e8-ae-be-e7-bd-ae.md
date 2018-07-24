---
title: Windows下Mysql开机自启动设置
tags:
  - mysql
url: 710.html
id: 710
categories:
  - 未分类
date: 2017-12-26 16:13:59
---

找到该路径：

C:\\Users\\wp\\AppData\\Roaming\\Microsoft\\Windows\\Start Menu\\Programs\\Startup

复制Mysql\\bin下mysqld.exe快捷方式到上面路径下

重启电脑测试

Mysql可以正常被调用