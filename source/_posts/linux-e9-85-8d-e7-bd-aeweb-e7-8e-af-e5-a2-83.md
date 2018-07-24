---
title: linux 配置WEB环境
tags:
  - linux
url: 477.html
id: 477
categories:
  - 服务器
date: 2017-04-07 16:21:19
---

1、准备一台虚拟机，要是虚拟机已经安装nginx，那么会对新的web环境产生影响

    解决办法①、格式化网盘

                  ②、yum remove nginx

2、下载tgz文件：

            [    http://123.56.227.138/OneinStack/oneinstack.tgz](http://123.56.227.138/OneinStack/oneinstack.tgz)

3、参照配置文档进行配置：

        [http://123.56.227.138/OneinStack/OneinStack一键PHP JAVA安装工具《专业版》V1.8.pdf](http://123.56.227.138/OneinStack/oneinstack.tgz)

4、重启服务器：

    service nginx restart

![图片.png](/ueditor/php/upload/image/20170407/1491553244630230.png "1491553244630230.png")