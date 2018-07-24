---
title: ajaxFileUpload插件只能上传一次问题解决
url: 515.html
id: 515
categories:
  - js/jquery/ajax
date: 2017-06-06 10:22:22
tags:
---

用ajaxFileUpload插件做一个读取Excel文件上传到服务器的功能，通过change方法触发，但二次上传file信息为空，上传信息拿不到。

读取ajaxFileUpload源码发现问题所在，创建from的时候没有保留原file的file信息。

* * *

解决办法：  

在ajaxFileUpload.js中找到createUploadForm这一段。

var oldElement = $('#' + fileElementId);  
var newElement = $(oldElement).clone();  
$(oldElement).attr('id', fileId);  
$(oldElement).before(newElement);  
$(oldElement).appendTo(form);

替换为：

var oldElement = jQuery('#' + fileElementId);  
var newElement = oldElement.clone(true);  
newElement\[0\].files=oldElement\[0\].files;  
oldElement.attr('id', fileId);  
oldElement.before(newElement);  
oldElement.appendTo(form);

测试问题解决了。