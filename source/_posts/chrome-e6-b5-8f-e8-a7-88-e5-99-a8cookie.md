---
title: Chrome浏览器Cookie
tags:
  - chrome
  - cookie
url: 413.html
id: 413
categories:
  - js/jquery/ajax
date: 2016-12-26 18:16:08
---

Cookies是浏览器记录在本地的用户数据，如用户的登录信息。Chrome为扩展提供了Cookies API用以管理Cookies。

要管理Cookies，需要在Manifest中声明`cookies`权限，同时也要声明所需管理Cookies所在的域：

"permissions": \[
    "cookies",
    "*://*.google.com"\]

如果想要管理所有的Cookies可以声明如下权限：

"permissions": \[
    "cookies",
    "<all_urls>"\]

请注意，除非必要，否则请不要如此声明权限，这会提示此扩展可以访问所有网络资源，给用户带来不安。

Chrome定义的`Cookie`对象包含如下属性：`name`（名称）、`value`（值）、`domain`（域）、`hostOnly`（是否只允许完全匹配domain的请求访问）、`path`（路径）、`secure`（是否只允许安全连接调用）、`httpOnly`（是否禁止客户端调用）、`session`（是否是session cookie）、`expirationDate`（过期时间）和`storeId`（包含此cookie的cookie store的id）。

读Cookies。Chrome提供了`get`和`getAll`两个方法读取Cookies，`get`方法可以读取指定`name`、`url`和`storeId`的Cookie，其中`storeId`可以不指定，但是`name`和`url`必须指定。如果在同一URL中包含多个`name`相同的Cookies，则会返回`path`最长的那个，如果有多个Cookies的`path`长度相同，则返回创建最早的那个。

chrome.cookies.get({
    url: 'https://github.com',
    name: 'dotcom_user'}, function(cookie){
    console.log(cookie.value);});

这里需要注意一点，如果`cookie`的`secure`属性值为`true`，那么通过`get`获取时`url`应该是https协议。

`getAll`方法与get方法不同，它可以获取所有符合条件的Cookies，支持的匹配条件包括`url`、`name`、`domain`、`path`、`secure`、`session`和`storeId`中的任意一个或多个，如果一个都不指定，则返回所有此扩展有权访问到的Cookies。比如下面的代码就可以获取到所有可以读取的Cookies：

chrome.cookies.getAll({}, function(cookies){
    console.log(cookies);});

设置Cookie。`set`方法可以设置Cookie：

chrome.cookies.set({
    'url':'http://github.com/test_cookie',
    'name':'TEST',
    'value':'foo',
    'secure':false,
    'httpOnly':false}, function(cookie){
    console.log(cookie);});

如果创建成功，则回调函数会获取到创建后的`cookie`对象，否则会得到`null`。`url`是必须指定的，其他的属性可选。另外扩展对URL必须有访问权限，否则会设置失败。如果不指定`expirationDate`属性，则所创建的Cookie将在浏览器关闭后被删除。

![enter image description here](http://www.ituring.com.cn/download/01YwDeKDy9Bf.small)[\[+\]查看原图](http://www.ituring.com.cn/download/01YwDeKDy9Bf.big)  
_设置Cookies_

删除Cookie。`remove`方法可以删除指定`url`、`name`和`storeId`的Cookie。

chrome.cookies.remove({
    url: 'http://www.google.com',
    name: '_ga'}, function(result){
    console.log(result);});

同样，扩展首先要具有对URL的访问权限，否则删除操作会失败。

除非你清楚在做什么，不要轻易删除用户的Cookies，否则你可能会收到大量用户抱怨气愤的邮件。

`getAllCookieStores`方法用来获取全部的cookie store，cookie store包含一个`id`属性和一个`tabIds`属性，`id`的属性值为这个cookie store的`id`，`tabIds`为包含共享这个cookie store所有tab的`id`的数组。有关tab的内容将在后面的章节讲解。

`onChanged`事件用来监控cookie的设置和删除行为：

chrome.cookies.onChanged.addListener(function(changeInfo){
    console.log(changeInfo);});

`changeInfo`包含三个属性：`removed`，是否是删除行为；`cookie`，被设置或删除的`cookie`对象；`cause`，Cookie变化的原因，可能的值包括`evicted`、`expired`、`explicit`、`expired_overwrite`和`overwrite`。

再次提醒，Cookies是用户的敏感数据，在进行操作时一定倍加小心，并要让用户有知情权，必要时一定要先得到用户的确认。