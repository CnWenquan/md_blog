---
title: php---https Apache证书使用方法
url: 151.html
id: 151
categories:
  - 服务器
date: 2016-08-12 14:54:59
tags:
---

先看效果：

![1.png](/ueditor/php/upload/image/20160812/1470978621547384.png "1470978621547384.png")

效果还是不错滴

  

两种办法：

1、网上买证书与秘钥

2、自己生成，但会每次报证书错误！！！

  

拿到证书与秘钥后要把证书与文件放到Apache    config相应的目录下，打开apache ssl.cong文件，增加证书路径与秘钥路径。重启Apache服务器，测测结果是否出来了