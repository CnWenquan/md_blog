---
title: 部署Python应用：Nginx+uWSGI
tags:
  - python
url: 838.html
id: 838
categories:
  - python
date: 2018-05-05 13:12:49
---

在PHP里，最方便的就是deployment了，只要把php文件丢到支持PHP的路径里面，然后访问那个路径就能使用了；无论给主机添加多少PHP应用，只要把目录改好就没你的事了，完全不用关心php-cgi运行得如何，deployment极为方便。

反观Python，部属起来真是头痛，常见的部署方法有：

◆fcgi：用spawn-fcgi或者框架自带的工具对各个project分别生成监听进程，然后和http服务互动。

◆wsgi：利用http服务的mod_wsgi模块来跑各个project。

无论哪种都很麻烦，apache的mod_wsgi配置起来很麻烦，内存占用还大，如果要加上nginx作为静态页面的服务器那就更麻烦了；我的应用基本上到后来都是是各个project各自为战，且不说管理上的混乱，这样对负载也是不利的，空闲的project和繁忙的project同样需要占用内存。

如果Python中能有个什么东西像php-cgi一样监听同一端口，进行统一管理和负载平衡，那真是能省下大量的部署功夫。偶然看到了uWSGI，才发现居然一直不知道有那么方便地统一部署工具。uWSGI，既不用wsgi协议也不用fcgi协议，而是自创了一个uwsgi的协议，据说该协议大约是fcgi协议的10倍那么快。

  

uWSGI的主要特点如下：

◆超快的性能。

◆低内存占用（实测为apache2的mod_wsgi的一半左右）。

◆多app管理。

◆详尽的日志功能（可以用来分析app性能和瓶颈）。

◆高度可定制（内存大小限制，服务一定次数后重启等）。

正式开工

uwsgi的文档虽然很多也很详细，这里是uwsgi的官方文档：http://projects.unbit.it/uwsgi/wiki/Doc。

1.安装uwsgi

ubuntu有uwsgi的ppa：

add-apt-repository ppa:stevecrozz/ppa       
apt-get update  
apt-get install uwsgi

  

2\. 用uwsgi代替mod_wsgi

[Nginx](http://developer.51cto.com/art/201004/194472.htm)的整体配置说来话长，这里不再多说，假设已经明白Nginx的基本配置，那么uwsgi就类似这么配置：

location / {  
  include uwsgi_params       
  uwsgi_pass 127.0.0.1:9090       
}

  

这就是把所有url传给9090端口的uwsgi协议程序来互动。再到project目录建立myapp.py，使得application调用框架的wsgi接口，比如web.py就是：

......  
app = web.application(urls, globals())       
appapplication = app.wsgifunc()

  

再比如django就是：

.......  
from django.core.handlers.wsgi import WSGIHandler       
application = WSGIHandler()

  

然后运行uwsgi监听9090，其中-w后跟模块名，也就是刚才配置的myapp

uwsgi -s :9090 -w myapp

  

运行网站发现已经部署完成了。

3.uwsgi的参数

以上是单个project的最简单化部署，uwsgi还是有很多令人称赞的功能的，例如：

并发4个线程：

uwsgi -s :9090 -w myapp -p 4

  

主控制线程+4个线程：

uwsgi -s :9090 -w myapp -M -p 4

  

执行超过30秒的client直接放弃：

uwsgi -s :9090 -w myapp -M -p 4 -t 30

  

限制内存空间128M：

uwsgi -s :9090 -w myapp -M -p 4 -t 30 --limit-as 128

  

服务超过10000个req自动respawn：

uwsgi -s :9090 -w myapp -M -p 4 -t 30 --limit-as 128 -R 10000

  

后台运行等：

uwsgi -s :9090 -w myapp -M -p 4 -t 30 --limit-as 128 -R 10000 -d uwsgi.log

  

4.为uwsgi配置多个站点

为了让多个站点共享一个uwsgi服务，必须把uwsgi运行成虚拟站点：去掉“-w myapp”加上”–vhost”：

uwsgi -s :9090 -M -p 4 -t 30 --limit-as 128 -R 10000 -d uwsgi.log --vhost

  

然后必须配置virtualenv，virtualenv是Python的一个很有用的虚拟环境工具，这样安装：

apt-get install Python-setuptools       
easy_install virtualenv

  

然后设置一个/多个app基准环境：

virtualenv /var/www/myenv

  

应用环境，在此环境下安装的软件仅在此环境下有效：

source /var/www/myenv/bin/activate       
pip install django       
pip install mako       
...

  

最后配置nginx，注意每个站点必须单独占用一个server，同一server不同location定向到不同的应用不知为何总是失败，估计也算是一个bug。

server {  
        listen       80;       
        server_name  app1.mydomain.com;       
        location / {       
                include uwsgi_params;       
                uwsgi_pass 127.0.0.1:9090;       
                uwsgi\_param UWSGI\_PYHOME /var/www/myenv;       
                uwsgi\_param UWSGI\_SCRIPT myapp1;       
                uwsgi\_param UWSGI\_CHDIR /var/www/myappdir1;       
        }       
    }  
    server {       
        listen       80;       
        server_name  app2.mydomain.com;       
        location / {       
                include uwsgi_params;       
                uwsgi_pass 127.0.0.1:9090;       
                uwsgi\_param UWSGI\_PYHOME /var/www/myenv;       
                uwsgi\_param UWSGI\_SCRIPT myapp2;       
                uwsgi\_param UWSGI\_CHDIR /var/www/myappdir2;       
        }       
    }

  

这样，重启nginx服务，两个站点就可以共用一个uwsgi服务了。

5.实战应用

最初的设置完毕以后，再添加的应用，只需要在Nginx里面进行少量修改，无需重启uwsgi，就能立刻部署完毕。uwsgi自带了基于django的监控uwsgi运行状态的工具，就拿它来部署好了：

server {  
    listen 80;       
    root   /var/www/django1.23;       
    index  index.html index.htm;       
    server_name uwsgiadmin.django.obmem.info;       
    access_log  /var/log/nginx/django.access.log;       
    location /media/ {       
        root /var/www/django1.23/adminmedia;       
        rewrite ^/media/(.*)$ /$1 break;       
    }  
    location / {       
        include uwsgi_params;       
        uwsgi_pass 127.0.0.1:9090;       
        uwsgi\_param UWSGI\_PYHOME /var/www/django1.23/vtenv;       
        uwsgi\_param UWSGI\_CHDIR /var/www/django1.23/uwsgiadmin;       
        uwsgi\_param UWSGI\_SCRIPT uwsgiadmin_wsgi;       
    }  
}

  

于是uwsgi的监控信息可以在http://uwsgiadmin.django.obmem.info看到（用户名密码都是admin）。再比如LBForum论坛程序的部署：根据安装说明安装完毕，再按部署说明修改完配置文件，然后只需修改nginx配置文件：

server {  
    listen 80;       
    root   /var/www/django1.23;       
    index  index.html index.htm;       
    server_name lbforum.django.obmem.info;       
    access_log  /var/log/nginx/django.access.log;       
    location / {       
        include uwsgi_params;       
        uwsgi_pass 127.0.0.1:9090;       
        uwsgi\_param UWSGI\_PYHOME /var/www/django1.23/vtenv;       
        uwsgi\_param UWSGI\_CHDIR /var/www/django1.23/LBForum/sites/default;       
        uwsgi\_param UWSGI\_SCRIPT lbforum_wsgi;       
    }  
}

  

后记

虽然写出来寥寥几行，配置的时候我可吃尽了uwsgi的苦头，有些想当然的用法完全不能成立，–no-site参数一加上去其他都好使LBForum怎么都部署不了，一开始多站点公用uwsgi怎么都成功不了等等。

Python世界很有趣，一直会发现有趣的东西，但是Python世界也很折腾人，大部分东西都是dev版本，文档缺失，各种兼容问题