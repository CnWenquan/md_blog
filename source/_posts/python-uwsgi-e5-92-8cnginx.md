---
title: Python uWSGI和Nginx
tags:
  - python
url: 843.html
id: 843
categories:
  - python
date: 2018-05-13 18:40:13
---

    在谈论uWSGI之前，应该先了解下WSGI是什么。  

    **WSGI：**  

    WSGI，全称Web Server Gateway Interface,是为Python语言定义的Web服务器和Web应用程序或框架之间的一种简单而通用的接口。从名字上看这种东西是一个Gateway。而网关的作用是在协议之间进行转换。WSGI是作为Web服务器与Web应用程序或应用框架之间的一种低级别的接口，以提升可移植Web应用开发的共同点。WSGI是基于现存的CGI标准而涉及的。

    wsgi server（比如uWSGI）要和 django交互，uwsgi需要将过来的请求转给django处理，那么uWSGI和django的交互和调用就需要一个统一的规范，这个规范就是WSGI。  

  

* * *

    上面由WSGI协议引出了uWSGI，也可以把uWSGI看作一个应用程序，帮助我们实现WSGI协议，Http协议等，这样我们可以不在关注网络通信的底层实现，将更多精力放在处理HTTP请求数据上。  

    **uWSGI：**  

    uWSGI是一个Web服务器，它实现了WSGI协议、uwsgi、http等协议。Nginx中HttpUwsgiModule的作用是与uWSGI服务器进行交换。  

    利用uWSGI可以使我们的web应用得到更强的并发能力，uWSGI也可以返回静态文件，但是很笨拙，一般静态文件都交由Nginx，如果只需要与反向代理服务器进行交互则只需要接受socket，uWSGI与Nginx交互相当于两个进程见交互，一般使用的是sock文件或者指定端口接受socket。指定端口时再使用浏览器访问相应端口，uWSGI会提示skip，跳过该HTTP请求。  

* * *

    **Nginx：**

    Nginx是一款轻量级的Web 服务器/反向代理服务器及电子邮件（IMAP/POP3）代理服务器，并在一个BSD-like 协议下发行。其特点是占有内存少，并发能力强Nginx是一款轻量级的Web 服务器/反向代理服务器及电子邮件（IMAP/POP3）代理服务器，并在一个BSD-like 协议下发行。其特点是占有内存少，并发能力强。

    它具有反向代理服务器的所有特点，并且配置简单，支持对大于1kb的文件进行压缩，静态文件缓存，可以大大提高访问效率。Nginx接受来自客户端的Http请求发送给uWSGI，uWSGI处理请求并将关键信息传递给web应用(django，flask等)，应用返回Response经由uWSGI发送给Nginx，Nginx再发送给客户端。  

![20180115204429595.png](/ueditor/php/upload/image/20180513/1526208705654930.png "1526208705654930.png")

* * *

    上图讲的不是特别详细，特别是uWSGI到应用程序这一段路，在此做个补充。  

  

    **ewsgi：**  

    uWSGI应用通过uwsgi协议通往wsgi协议，wsgi协议规定了python中接受请求的格式及回调格式，例如用Python语言写一个符合WSGI的“Hello World”：

def app(environ, start_response):
    start_response('200 OK', \[('Content-Type', 'text/plain')\])
    yield "Hello world!\\n"

    代码意思：定义一个名为app的应用程序，接受两个参数，environ和start_response。     

    environ顾名思义，即环境变量，他会返回nginx的root以及uWSGI应用的配置，这样可以根据nginx的root配置到相应的路径下找对应的.py文件。

    start\_response是一个callable回调方法，接受两个必须的参数，status（Http返回状态）和respose\_headers(响应消息头例如（Current-Type）)  

    上面的代码反映了python可以根据不同的返回结果自定义http返回状态，这一点相对与php-cgi更灵活。这个方法一般是python的统一请求入口。  

  

* * *

    上面讲了python的统一请求入口，那python方法中如何定义方法呢？  

    uWSGI应用请求的文件会默认的把文件名置换为uwsgi\_file\_(文件名),so 只要监听到uwsgi\_file\_(文件名)就可以确定请求的类了：

import ewsgi

class App(ewsgi.WsgiAServer):
    ...
if \_\_name\_\_.startswith('uwsgi\_file\_'):
    application = App()