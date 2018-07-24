---
title: sabre_xml php读写xml工具类
tags:
  - xml
url: 573.html
id: 573
categories:
  - Laravel
  - PHP
date: 2017-08-09 15:59:00
---

    应公司业务需求，要能接收xml请求，并且返回xml结果，在网上找php的扩展包，大部分都是xml的arrarToXml方法或者XmlToArray，从功能实现上来说，已经满足需求了，但不漂亮！  

    本身依赖laravel框架开发，再加上composer扩展包的加持，（对我的赶脚是越来越像Android的开发模式啦，题外话），那就在composer中找找有木有扩展包，根据套路说，应该接下来说能找到！搜索 Array transfer xml,有一条下载量特别大，star最多的，同时满足write跟read的，就它了--》sabre/xml。  

    **install:**  

        composer require sabre/xml 2.0  

   ** write:**  

public function writeTest(){  
 $service = new Service();  
 $ds = '{http://www.w3.org/2000/09/xmldsig&#35;}';  
 $service->namespaceMap\['http://www.w3.org/2000/09/xmldsig#'\] = 'ds';  
 echo $service->write( 'OnlineTicketingServiceReply', \[  
  
 'RefundTicketReply'=>\[  
 'attributes'=>\['Status' =\> '返回状态码','ErrorCode'=>'错误代码','ErrorMessage'=>'错误描述','Id'=>'ID_RefundTicketReply'\],  
 'value'=>\[  
 'attributes'=>\['OrderCode'=>'订单编号'\],  
 'name'=>'Order',  
 'value'=>\[  
 \[ 'name'=>'Status',  
 'value'=>'状态'  
 \],  
 \[ 'name'=>'RefundTime',  
 'value'=>'退票时间'  
 \]  
  
 \] \], \],  
 $ds.'Signature'=>\[\]  
  
 \]);}  

结果：

* * *

![WX20170809-160004@2x.png](/ueditor/php/upload/image/20170809/1502265513285742.png "1502265513285742.png")

* * *

**read:**public function readTest(){

 $xml = '<?xml version="1.0"?>  
<OnlineTicketingServiceReply xmlns:ds="http://www.w3.org/2000/09/xmldsig#">  
 <RefundTicketReply Status="&#x8FD4;&#x56DE;&#x72B6;&#x6001;&#x7801;" ErrorCode="&#x9519;&#x8BEF;&#x4EE3;&#x7801;" ErrorMessage="&#x9519;&#x8BEF;&#x63CF;&#x8FF0;" Id="ID_RefundTicketReply"> <Order OrderCode="&#x8BA2;&#x5355;&#x7F16;&#x53F7;"> <Status>状态</Status> <RefundTime>退票时间</RefundTime> </Order> </RefundTicketReply> <ds:Signature></ds:Signature>  
</OnlineTicketingServiceReply>';  
 $reader = new Service();  
 return $reader->parse($xml);  
 }

    结果：  

* * *

![WX20170809-155838@2x.png](/ueditor/php/upload/image/20170809/1502265432982572.png "1502265432982572.png")