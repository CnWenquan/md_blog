---
title: ubuntu redis配置文件
tags:
  - reids
url: 701.html
id: 701
categories:
  - redis
date: 2017-12-20 22:27:39
---

ubuntu redis安装路径：

/etc/redis  
  
该路径下有个config文件，把该文件非空行、非注释行内容写入新文件中：

cat redis.conf | grep -v "#" | grep -v "^$" > redis-6380.conf

设置新的端口、日志文件地址：

![图片.png](/ueditor/php/upload/image/20171220/1513779829311326.png "1513779829311326.png")

启动redis-server 6380配置：

redis-server redis-6380.conf  
查看启动结果：

 ps -ef | grep redis-server | grep 6380  

  

![图片.png](/ueditor/php/upload/image/20171220/1513779924421150.png "1513779924421150.png")

  

启动成功！

最后查看日志：

![图片.png](/ueditor/php/upload/image/20171220/1513780249702274.png "1513780249702274.png")