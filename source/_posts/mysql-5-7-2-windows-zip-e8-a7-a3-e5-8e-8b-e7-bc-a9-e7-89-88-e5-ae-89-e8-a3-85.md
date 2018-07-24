---
title: mysql 5.7.2 windows zip解压缩版安装
tags:
  - mysql
url: 688.html
id: 688
categories:
  - MySql
date: 2017-11-20 17:27:56
---

    由于需要用到mysql服务，并且有些数据需要初始化，考虑两条路，一条是用php框架通过程序代码初始化，一条是直接把mysql打包好来用。从简易操作来考虑，选择了直接打包mysql。

    首先下载mysql5.7.20安装包

    将文件夹解压到适当路径，我需要做打包安装程序，so，我解压到了C盘根目录下

    配置my.ini文件：此版本没有带my-default.ini文件，直接在bin文件夹下新建文本文档，重命名为my.ini。

my.ini文件内容：

    \[client\]

        port = 3306

        character-set = utf8

    \[mysqld\]

        port = 3306

        character-set-server = utf8

        basedir = C:\\mysql

        datadir = C:\\mysql\\data

win10操作系统中以管理员打开命令提示符进入mysql\\bin目录下，执行:

mysqld --initialize-insecure --user=mysql

运行这句话，如果不出意外，会在mysql根目录下生成一个data文件夹

运行：mysqld --install

成功会显示：Servers Successfully installed

启动mysql服务：net start mysql

可以进入mysql服务查看：mysql -u root  

关闭mysql：net stop mysql