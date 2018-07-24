---
title: Chrome跨域请求
tags:
  - chrome
url: 404.html
id: 404
categories:
  - js/jquery/ajax
date: 2016-12-26 12:13:56
---

跨域指的是JavaScript通过`XMLHttpRequest`请求数据时，调用JavaScript的页面所在的域和被请求页面的域不一致。对于网站来说，浏览器出于安全考虑是不允许跨域。另外，对于域相同，但端口或协议不同时，浏览器也是禁止的。下表给出了进一步的说明：

URL

说明

是否允许请求

http://a.example.com/  
http://a.example.com/a.txt

同域下

允许

http://a.example.com/  
http://a.example.com/b/a.txt

同域下不同目录

允许

http://a.example.com/  
http://a.example.com:8080/a.txt

同域下不同端口

不允许

http://a.example.com/  
https://a.example.com/a.txt

同域下不同协议

不允许

http://a.example.com/  
http://b.example.com/a.txt

不同域下

不允许

http://a.example.com/  
http://a.foo.com/a.txt

不同域下

不允许

但这个规则如果同样限制Chrome扩展应用，就会使其能力大打折扣，所以Google允许Chrome扩展应用不必受限于跨域限制。但出于安全考虑，需要在Manifest的`permissions`属性中声明需要跨域的权限。

比如，如果我们想设计一款获取维基百科数据并显示在其他网页中的扩展，就要在Manifest中进行如下声明：

{
    ...
    "permissions": \[
        "*://*.wikipedia.org/*"
    \]}

这样Chrome就会允许你的扩展在任意页面请求维基百科上的内容了。

我们可以利用如下的代码发起异步请求：

function httpRequest(url, callback){
    var xhr = new XMLHttpRequest();
    xhr.open("GET", url, true);
    xhr.onreadystatechange = function() {
        if (xhr.readyState == 4) {
            callback(xhr.responseText);
        }
    }
    xhr.send();}

这样每次发起请求时，只要调用`httpRequest`函数，并传入要请求的URL和接收返回结果的函数就可以了。为什么要使用`callback`函数接收请求结果，而不直接用`return`将结果作为函数值返回呢？因为`XMLHttpRequest`不会阻塞下面代码的运行。