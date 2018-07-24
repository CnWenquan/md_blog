---
title: Nginx中文域名配置
tags:
  - nginx
url: 427.html
id: 427
categories:
  - 服务器
date: 2017-01-04 16:21:50
---

Nginx虚拟主机上绑定一个带中文域名，比如linuxeye.中国，浏览器不能跳转。

#### why?

因为操作系统的核心都是英文组成，DNS服务器的解析也是由英文代码交换，所以DNS服务器上并不支持直接的中文域名解析，所有中文域名的解析都需要转成punycode码，然后由DNS解析punycode码。其实目前所说和各种浏览器完美支持中文域名，只是浏览器软里面主动加入了中文域名自动转码，不需要原来的再次安装中文域名转码控件来完成整个流程。

中文域名变为Punycode编码即可，在线转码地址：[http://tools.jb51.net/punycode/](https://blog.linuxeye.com/wp-content/themes/begin/inc/go.php?url=http://tools.jb51.net/punycode/)，如下图：

![](/ueditor/php/upload/image/20170104/1483518013322882.png)

#### Punycode？

Punycode是一个根据RFC 3492标准而制定的编码系统,主要用於把域名从地方语言所采用的Unicode编码转换成为可用於DNS系统的编码。 "中文域名"后缀不被标准的解析服务器支持，需转化为punycode码进行解析。

#### 修改Nginx配置

1.  server {
    
2.  listen 80;
    
3.  server_name blog.linuxeye.com linuxeye.xn--fiqs8s;
    
4.  ...
    
5.  }
    

保存，重载nginx：

1.  service nginx reload