---
title: CGI与FastCGI
tags:
  - linux
url: 486.html
id: 486
categories:
  - Linux
date: 2017-04-19 15:33:08
---

    最早的web服务器简单地响应浏览器发来的HTTP请求，并将存储在服务器上的HTML文件返回给浏览器，也就是静态html。事物总是不断发展，网站也越来越复杂，所以出现动态技术。但是服务器并不能直接运行php，asp这样的文件，自己不能做，就外包给别人吧，但是要与第三方做个约定，我给你什么，然后你给我什么，就是我把参数发送给你，然后我接收你的处理结过给客户端。那这个约定就是common gateway interface，简称cgi。这个协议可以用vb，c，php，python来实现。cgi只是接口协议，根本不是什么语言。下图是流程。  

![](/ueditor/php/upload/image/20170419/1492584576399825.gif)

    Web服务器将根据CGI程序的类型决定数据向CDI程序的传送方式，一般来讲是通过标准输入/输出流和环境变量来与CGI程序间传递数据。如下图所示：  

![](/ueditor/php/upload/image/20170419/1492584896743651.gif)

    CGI程序通过标准输入（STDIN）和标准输出（STDOUT）来进行输入输出。此外CGI程序还通过环境变量来得到输入，操作系统提供了许多环境变量，他们定义了程序的执行环境，应用程序可以存取他们。web服务器和CGI接口又另外设置了一些环境变量，用来向CGI程序传递一些重要的参数。CGI的GET方法还通过环境变量QUERY-STRING向CGI程序传递Form中的数据。下面是一些常用的CGI环境变量。  

变量名

描述

CONTENT_TYPE

这个环境变量的值指示所传递来的信息的MIME类型。目前，环境变量CONTENT_TYPE一般都是：application/x-www-form-urlencoded,他表示数据来自于HTML表单。

CONTENT_LENGTH

如果服务器与CGI程序信息的传递方式是POST，这个环境变量即使从标准输入STDIN中可以读到的有效数据的字节数。这个环境变量在读取所输入的数据时必须使用。

HTTP_COOKIE

客户机内的 COOKIE 内容。

HTTP\_USER\_AGENT

提供包含了版本数或其他专有数据的客户浏览器信息。

PATH_INFO

这个环境变量的值表示紧接在CGI程序名之后的其他路径信息。它常常作为CGI程序的参数出现。

QUERY_STRING

如果服务器与CGI程序信息的传递方式是GET，这个环境变量的值即使所传递的信息。这个信息经跟在CGI程序名的后面，两者中间用一个问号'?'分隔。

REMOTE_ADDR

这个环境变量的值是发送请求的客户机的IP地址，例如上面的192.168.1.67。这个值总是存在的。而且它是Web客户机需要提供给Web服务器的唯一标识，可以在CGI程序中用它来区分不同的Web客户机。

REMOTE_HOST

这个环境变量的值包含发送CGI请求的客户机的主机名。如果不支持你想查询，则无需定义此环境变量。

REQUEST_METHOD

提供脚本被调用的方法。对于使用 HTTP/1.0 协议的脚本，仅 GET 和 POST 有意义。

SCRIPT_FILENAME

CGI脚本的完整路径

SCRIPT_NAME

CGI脚本的的名称

SERVER_NAME

这是你的 WEB 服务器的主机名、别名或IP地址。

SERVER_SOFTWARE

这个环境变量的值包含了调用CGI程序的HTTP服务器的名称和版本号。例如，上面的值为Apache/2.2.14(Unix)

    CGI工作原理：每当客户请求CGI的时候，WEB服务器就请求操作系统生成一个新的CGI解释器进程（如php-cgi.exe），CGI的一个进程则处理完一个请求后退出，下一个请求来时再创建新进程。当然，这样在访问量很少没有并发的情况也行。可是当访问量增大，并发存在，这种方式就不适合了。于是就有了fastcgi。  

    FastCGI像是一个常驻（long-live）型的CGI，它可以一直执行着，只要激活后，不会每次都要花费时间去fork一次（这是CGI最为人诟病的fork-and-execute）模式。  

      

* * *

    FastCGI工作流程：  

    1、Web Server启动时载入FastCGI进程管理器  

    2、FastCGI进程管理器自身初始化，启动多个CGI解释器进程（可见多个php-cgi）并等待来自Web Server的连接。  

    3、当客户端请求到到Web Server时，FastCGI进程管理器选择并连接到一个CGI解释器。Web Server将CGI环境变量和标准输入发送到FastCGI子进程php-cgi。  

    4、FastCGI子进程完成处理后将标准输出和错误信息从同一连接返回Web Server。但FastCGI子进程关闭连接时，请求便告处理完成。FasrCGI子进程接着等待并处理来自FastCGI进程管理器的下一个连接。在CGI模式中，php-cgi在此便退出了。  

  

* * *

PHP-FPM与Spawn-FCGI  

    Spawn-FCGI是一个通用的FastCGI管理服务器，他是lighttpd中的一部分，很多人都用lighttpd的Spawn-FCGI进行FastCGI模式下的管理工作。但是有缺点，于是PHP-fpm就是针对于PHP的，FastCGI的一种实现，他负责管理一个进程池，来处理来自Web服务器的请求。目前，PHP-fpm是内置于PHP的。  

  

* * *

有人测试nginx+php-fpm在高并发情况下可能会达到Apache+mod_php5的5~10倍，现在nginx+php-fpm使用的人越来越多。

