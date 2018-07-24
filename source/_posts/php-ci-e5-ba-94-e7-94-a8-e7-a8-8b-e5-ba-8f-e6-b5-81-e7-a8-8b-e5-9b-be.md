---
title: php ci应用程序流程图
url: 156.html
id: 156
categories:
  - CI
date: 2016-08-15 09:42:32
tags:
---

下图说明了整个系统的数据流程：

![CodeIgniter 程序流程](/ueditor/php/upload/image/20160815/1471225112765927.png)

入口文件（index.php）--->Routing(路由规则)--->Security(安全验证)--->Application（控制器）--->View（视图）--->Caching(缓存)--->index.php  

1.  index.php 文件作为前端控制器，初始化运行 CodeIgniter 所需的基本资源；
    
2.  Router 检查 HTTP 请求，以确定如何处理该请求；
    
3.  如果存在缓存文件，将直接输出到浏览器，不用走下面正常的系统流程；
    
4.  在加载应用程序控制器之前，对 HTTP 请求以及任何用户提交的数据进行安全检查；
    
5.  控制器加载模型、核心类库、辅助函数以及其他所有处理请求所需的资源；
    
6.  最后一步，渲染视图并发送至浏览器，如果开启了缓存，视图被会先缓存起来用于 后续的请求。