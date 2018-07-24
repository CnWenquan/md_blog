---
title: Apache ab 压力并发测试工具
tags:
  - linux
url: 779.html
id: 779
categories:
  - Linux
date: 2018-03-17 19:59:30
---

当你使用PHP(或其他[编程语言](http://cpro.baidu.com/cpro/ui/uijs.php?adclass=0&app_id=0&c=news&cf=46&ch=0&di=128&fv=19&is_app=0&jk=1d4988a9148e80f1&k=%B1%E0%B3%CC%D3%EF%D1%D4&k0=%B1%E0%B3%CC%D3%EF%D1%D4&kdi0=0&luki=5&mcpm=0&n=10&p=baidu&q=97086099_cpr&rb=0&rs=1&seller_id=1&sid=f1808e14a988491d&ssp2=1&stid=0&t=tpclicked3_hc&td=1691595&tu=u1691595&u=http:%2F%2Fwww.365mini.com%2Fpage%2Fapache-benchmark.htm&urlid=0))完成一个web程序的开发，并且web程序在Apache服务器上正常运行的时候，你有没有考虑过对你的Apache服务器及部署在其上的web程序进行一些压力测试呢？毕竟，真金不怕火炼，只要配置优化合理、[程序代码](http://cpro.baidu.com/cpro/ui/uijs.php?adclass=0&app_id=0&c=news&cf=46&ch=0&di=128&fv=19&is_app=0&jk=1d4988a9148e80f1&k=%B3%CC%D0%F2%B4%FA%C2%EB&k0=%B3%CC%D0%F2%B4%FA%C2%EB&kdi0=0&luki=4&mcpm=0&n=10&p=baidu&q=97086099_cpr&rb=0&rs=1&seller_id=1&sid=f1808e14a988491d&ssp2=1&stid=0&t=tpclicked3_hc&td=1691595&tu=u1691595&u=http:%2F%2Fwww.365mini.com%2Fpage%2Fapache-benchmark.htm&urlid=0)运行性能良好、硬件撑得住，通过压力测试我们就能看到一个喜人的成绩。话又说回来，即使测试结果差强人意或者糟糕透了，我们也可以好好反思并找到「问题出在哪里」。说了这么多废话，其实就是想告诉你一句：对服务器以及网站程序进行压力测试还是很有必要的。

要对Apache及其上的web程序进行压力测试其实非常简单，我们也不用再额外下载安装什么[测试工具](http://cpro.baidu.com/cpro/ui/uijs.php?adclass=0&app_id=0&c=news&cf=46&ch=0&di=128&fv=19&is_app=0&jk=1d4988a9148e80f1&k=%B2%E2%CA%D4%B9%A4%BE%DF&k0=%B2%E2%CA%D4%B9%A4%BE%DF&kdi0=0&luki=9&mcpm=0&n=10&p=baidu&q=97086099_cpr&rb=0&rs=1&seller_id=1&sid=f1808e14a988491d&ssp2=1&stid=0&t=tpclicked3_hc&td=1691595&tu=u1691595&u=http:%2F%2Fwww.365mini.com%2Fpage%2Fapache-benchmark.htm&urlid=0)，因为Apache HTTP Server已经给我们准备了一个压力测试工具——ab。

**ab**，即Apache Benchmark，只要我们安装了Apache，就能够在Apache的安装目录中找到它。它的居住地址是`Apache安装目录/bin/ab.exe`。现在，我们就来看看如何使用ab.exe来进行压力测试。

在使用之前我们先来学习一下ab的用法。首先，我们输入cmd打开DOS窗口，然后将更改当前工作目录为：`Apache安装目录/bin/`。然后键入帮助命令`ab -help`(或者`ab /?`、`ab -h`)，我们就可以看到如下的用法介绍界面。

常用命令 : ab -n 1000 -c 10 127.0.0.1/index.php //使用绝对路径! 1000次访问并发量为 10 

![](/ueditor/php/upload/image/20180317/1521287918922057.png)

ab的用法介绍

通过上面的用法介绍可以得知，ab的使用方法就是输入如下DOS命令：

ab \[可选的参数选项\] 需要进行压力测试的url

此外，我们再根据上面的用法介绍界面来详细了解每个参数选项的作用。

*   -n
    
*   即requests，用于指定压力测试总共的执行次数。
    
*   -c
    
*   即concurrency，用于指定[压力测试](http://cpro.baidu.com/cpro/ui/uijs.php?adclass=0&app_id=0&c=news&cf=46&ch=0&di=128&fv=19&is_app=0&jk=1d4988a9148e80f1&k=%D1%B9%C1%A6%B2%E2%CA%D4&k0=%D1%B9%C1%A6%B2%E2%CA%D4&kdi0=0&luki=10&mcpm=0&n=10&p=baidu&q=97086099_cpr&rb=0&rs=1&seller_id=1&sid=f1808e14a988491d&ssp2=1&stid=0&t=tpclicked3_hc&td=1691595&tu=u1691595&u=http:%2F%2Fwww.365mini.com%2Fpage%2Fapache-benchmark.htm&urlid=0)的并发数。
    
*   -t
    
*   即timelimit，等待响应的最大时间(单位：秒)。
    
*   -b
    
*   即windowsize，TCP发送/接收的缓冲大小(单位：字节)。
    
*   -p
    
*   即postfile，发送POST请求时需要上传的文件，此外还必须设置`-T`参数。
    
*   -u
    
*   即putfile，发送PUT请求时需要上传的文件，此外还必须设置`-T`参数。
    
*   -T
    
*   即content-type，用于设置Content-Type请求头信息，例如：`application/x-www-form-urlencoded`，默认值为`text/plain`。
    
*   -v
    
*   即verbosity，指定打印帮助信息的冗余级别。
    
*   -w
    
*   以HTML表格形式打印结果。
    
*   -i
    
*   使用HEAD请求代替GET请求。
    
*   -x
    
*   插入字符串作为table标签的属性。
    
*   -y
    
*   插入字符串作为tr标签的属性。
    
*   -z
    
*   插入字符串作为td标签的属性。
    
*   -C
    
*   添加cookie信息，例如："Apache=1234"(可以重复该参数选项以添加多个)。
    
*   -H
    
*   添加任意的请求头，例如："Accept-Encoding: gzip"，请求头将会添加在现有的多个请求头之后(可以重复该参数选项以添加多个)。
    
*   -A
    
*   添加一个基本的网络认证信息，用户名和密码之间用英文冒号隔开。
    
*   -P
    
*   添加一个基本的代理认证信息，用户名和密码之间用英文冒号隔开。
    
*   -X
    
*   指定使用的[代理服务器](http://cpro.baidu.com/cpro/ui/uijs.php?adclass=0&app_id=0&c=news&cf=46&ch=0&di=128&fv=19&is_app=0&jk=1d4988a9148e80f1&k=%B4%FA%C0%ED%B7%FE%CE%F1%C6%F7&k0=%B4%FA%C0%ED%B7%FE%CE%F1%C6%F7&kdi0=0&luki=1&mcpm=0&n=10&p=baidu&q=97086099_cpr&rb=0&rs=1&seller_id=1&sid=f1808e14a988491d&ssp2=1&stid=0&t=tpclicked3_hc&td=1691595&tu=u1691595&u=http:%2F%2Fwww.365mini.com%2Fpage%2Fapache-benchmark.htm&urlid=0)和端口号，例如:"126.10.10.3:88"。
    
*   -V
    
*   打印版本号并退出。
    
*   -k
    
*   使用HTTP的KeepAlive特性。
    
*   -k
    
*   使用HTTP的KeepAlive特性。
    
*   -d
    
*   不显示百分比。
    
*   -S
    
*   不显示预估和警告信息。
    
*   -g
    
*   输出结果信息到gnuplot格式的文件中。
    
*   -e
    
*   输出结果信息到CSV格式的文件中。
    
*   -r
    
*   指定接收到错误信息时不退出程序。
    
*   -h
    
*   显示用法信息，其实就是`ab -help`。
    

虽然ab可以配置的参数选项比较多，但是，一般情况下我们只需要使用形如`ab -n 数字 -c 数字 url路径`的命令即可。譬如，我们对位于本地Apache服务器上、URL为localhost/index.[php](http://cpro.baidu.com/cpro/ui/uijs.php?adclass=0&app_id=0&c=news&cf=46&ch=0&di=128&fv=19&is_app=0&jk=1d4988a9148e80f1&k=php&k0=php&kdi0=0&luki=2&mcpm=0&n=10&p=baidu&q=97086099_cpr&rb=0&rs=1&seller_id=1&sid=f1808e14a988491d&ssp2=1&stid=0&t=tpclicked3_hc&td=1691595&tu=u1691595&u=http:%2F%2Fwww.365mini.com%2Fpage%2Fapache-benchmark.htm&urlid=0)的页面进行[压力测试](http://cpro.baidu.com/cpro/ui/uijs.php?adclass=0&app_id=0&c=news&cf=46&ch=0&di=128&fv=19&is_app=0&jk=1d4988a9148e80f1&k=%D1%B9%C1%A6%B2%E2%CA%D4&k0=%D1%B9%C1%A6%B2%E2%CA%D4&kdi0=0&luki=10&mcpm=0&n=10&p=baidu&q=97086099_cpr&rb=0&rs=1&seller_id=1&sid=f1808e14a988491d&ssp2=1&stid=0&t=tpclicked3_hc&td=1691595&tu=u1691595&u=http:%2F%2Fwww.365mini.com%2Fpage%2Fapache-benchmark.htm&urlid=0)。测试总次数为1000，并发数为100(相当于100个用户同时访问，他们总共访问1000次)。我们输入DOS命令`ab -n 1000 -c 100 localhost/index.php`，打印结果如下：

F:\\Apache2.2\\bin>ab -n 1000 -c 100 localhost/index.php
This is ApacheBench, Version 2.3 <$Revision: 655654 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking localhost (be patient)
Completed 100 requests
Completed 200 requests
Completed 300 requests
Completed 400 requests
Completed 500 requests
Completed 600 requests
Completed 700 requests
Completed 800 requests
Completed 900 requests
Completed 1000 requests
Finished 1000 requests


Server Software:        Apache/2.2.25 (服务器软件名称及版本信息)
Server Hostname:        localhost (服务器主机名)
Server Port:            80 (服务器端口)
Document Path:          /index.php (供测试的URL路径)
Document Length:        10 bytes (供测试的URL返回的文档大小)
Concurrency Level:      100 (并发数)
Time taken for tests:   0.247 seconds (压力测试消耗的总时间)
Complete requests:      1000 (压力测试的总次数)
Failed requests:        0 (失败的请求数)
Write errors:           0 (网络连接写入错误数)
Total transferred:      198000 bytes (传输的总数据量)
HTML transferred:       10000 bytes (HTML文档的总数据量)
Requests per second:    4048.34 \[#/sec\] (mean) (平均每秒的请求数)
Time per request:       24.701 \[ms\] (mean) (所有并发用户(这里是100)都请求一次的平均时间)
Time per request:       0.247 \[ms\] (mean, across all concurrent requests) (单个用户请求一次的平均时间)
Transfer rate:          782.78 \[Kbytes/sec\] received (传输速率，单位：KB/s)
Connection Times (ms)
              min  mean\[+/-sd\] median   max
Connect:        0    0   0.3      0       1
Processing:     6   23   4.2     24      30
Waiting:        5   20   5.3     21      29
Total:          6   23   4.2     24      30

Percentage of the requests served within a certain time (ms)
  50%     24
  66%     25
  75%     26
  80%     26
  90%     27
  95%     27
  98%     28
  99%     29
 100%     30 (longest request)

在上面的测试中，我们设置的[压力测试](http://cpro.baidu.com/cpro/ui/uijs.php?adclass=0&app_id=0&c=news&cf=46&ch=0&di=128&fv=19&is_app=0&jk=1d4988a9148e80f1&k=%D1%B9%C1%A6%B2%E2%CA%D4&k0=%D1%B9%C1%A6%B2%E2%CA%D4&kdi0=0&luki=10&mcpm=0&n=10&p=baidu&q=97086099_cpr&rb=0&rs=1&seller_id=1&sid=f1808e14a988491d&ssp2=1&stid=0&t=tpclicked3_hc&td=1691595&tu=u1691595&u=http:%2F%2Fwww.365mini.com%2Fpage%2Fapache-benchmark.htm&urlid=0)总次数以及并发数并没有让服务器感觉到什么「压力」，现在我们再来看一个「压力山大」的执行命令：`ab -n 100000 -c 1000 localhost/index.php`,这个时候apache就直接罢工——拒绝访问了：

F:\\Apache2.2\\bin>ab -n 100000 -c 1000 localhost/index.phpThis is ApacheBench, Version 2.3 <$Revision: 655654 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking localhost (be patient)  
Test aborted after 10 failures
apr\_socket\_connect(): 由于目标计算机积极拒绝，无法连接。   (730061)

注意：在上面的压力测试中，Apache使用的是默认配置，并没有经过任何优化措施处理。实际上，Apache在经过配置优化后，只要服务器硬件够用，Apache服务器是能够撑起1000的并发量的。

测试 redis 的并发

  

<?php    $redis = new redis();    $redis->connect('127.0.0.1', 6379);    $time = date("Ymd H:i:s");    $redis->rpush('list1', $time);   
    echo "队列总长度：".$redis->lLen('list1');    echo "<br/>";    
   /* 
    while (true) { //出对
        $get = $redis->lpop('list');
        if ($get) {
            echo '出队列--' . $get;
            echo '<br/>';
        } else {
            echo '出队完成';
            return false;
        }
    
    }*/