---
title: nginx配置ssl证书
url: 479.html
id: 479
categories:
  - Linux
date: 2017-04-11 18:07:00
tags:
---

前面一篇博客写了chrome浏览器认可的Let's Encrypt证书生成办法，现在补充下已有（已购买）证书的安装办法。

    1、在nginx的config目录下新建一个ssl文件夹

    2、把*.crt,*.key文件上传到ssl文件夹

    3、在需要证书的域名中配置ssl

server {
  listen 80;
  server_name XXX;
}

server{
   listen 443;   #ssl监听端口号
   server_name dcp.huayingjuhe.com;
   ssl on;      #打开ssl
   ssl\_certificate ssl\_keys/1__.XXX\_bundle.crt; #ssl\_keys代表的是存放证书文件的文件夹名称
   ssl\_certificate\_key ssl\_keys/2\_\_.XXX.com.key;
   ssl\_session\_timeout 5m;
   ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
   ssl_ciphers ALL:!ADH:!EXPORT56:-RC4+RSA:+HIGH:+MEDIUM:!EXP;
   ssl\_prefer\_server_ciphers on;

  access\_log /data/wwwlogs/dcp.XXX.com\_nginx.log combined;
  index index.html index.htm index.php;
  include /usr/local/nginx/conf/rewrite/wordpress.conf;
  root /data/wwwroot/website/dcp/webroot;

  #error_page 404 = /404.html;
  #error_page 502 = /502.html;

  location ~ \[^/\]\\.php(/|$) {
    #fastcgi\_pass remote\_php_ip:9000;
    fastcgi_pass unix:/dev/shm/php-cgi.sock;
    fastcgi_index index.php;
    include fastcgi.conf;
  }
  location ~ .*\\.(gif|jpg|jpeg|png|bmp|swf|flv|mp4|ico)$ {
    expires 30d;
    access_log off;
  }
  location ~ .*\\.(js|css)?$ {
    expires 7d;
    access_log off;
  }
  location ~ /\\.ht {
    deny all;
  }

}