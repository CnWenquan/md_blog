---
title: Webstorm/Phpstorm设置ftp连接，直接导入编辑器
url: 549.html
id: 549
categories:
  - PHP
date: 2017-07-24 14:57:10
tags:
---

Phpstorm除了能直接打开localhost文件之外，还可以连接FTP，除了完成正常的数据传递任务之外，还可以进行本地文件与服务端文件的异同比较，同一文件自动匹配目录上传，下载，这些功能是平常IDE，FTP软件中少见的，而且是很耗工作时间的一个操作。换句话说，在Webstorm/Phpstorm中操作ftp能找到原来版本控制的感觉。唯一的缺点是：上传，下载的打开链接要稍费时间，适合的场景在于单文件的编辑，这个如果网速够快一般可以忽略，而且就个人体验，虽然链接的速度稍慢，传输的速度并不慢。

1\. 设置： 设置的入口有两处，    

a. Tools->Deployment->configruation   

![](/ueditor/php/upload/image/20170724/1500879409301618.jpg)

b. File->Settings->Deployment->configruation

![](/ueditor/php/upload/image/20170724/1500879409747424.jpg)

单击左上角加号新增一个FTP服务连接 然后配置FTP host,username,pwd等信息，配置好之后，可以Test FTP connection测试FTP是否连接成功。 然后点击Root path三点，如果有FTP服务端的目录读取出来，那就没问题，如果读取不出来，则在下面的Advanced options中选择Passive mode(被动模式)。 然后在Web server root URL中填写域名信息或者空间商提供的免费三级域名。 然后在Mappings 选择映射目录分别为本地，发布或web路径。

![](/ueditor/php/upload/image/20170724/1500879409792658.jpg)

![](/ueditor/php/upload/image/20170724/1500879410520614.jpg)

2\. 出口：    

在与ftp连接的项目文件右击就能发现deployment，分别有四个选项(这个选项只有在连接成功后才能显示)：    

![](/ueditor/php/upload/image/20170724/1500879410469030.jpg)

1). upload to ...         上传到服务端    

2). Download from ...     从服务端拉    

3). Compare with Deployed Version on ...     本地版本与服务端版本进行比较    

![](/ueditor/php/upload/image/20170724/1500879410610754.jpg)

4). Sync with Deployed to ...     比较之外还可以直接进行操作.（3,4的差别在于，3只能浏览不能操作，4可以操作）。

![](/ueditor/php/upload/image/20170724/1500879410870900.jpg)

3\. 显示扩展面板     Tools->Deployment->configruation->Browse Remote host

![](/ueditor/php/upload/image/20170724/1500879410551342.jpg)