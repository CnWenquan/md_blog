---
title: 阿里大鱼--短信使用
tags:
  - Android
  - PHP
url: 335.html
id: 335
categories:
  - 服务器
date: 2016-11-07 17:52:01
---

相对于其他的短信提供商，阿里大鱼的价格更低廉

PHP：

    准备：

        框架：ThinkPhp 3.2  

        SDK：[https://www.alidayu.com/center/application/sdk](https://www.alidayu.com/center/application/sdk)  

    导入：

        放到ThinkPHP的 Library->Vendor下

        在控制器中vendor('aliy\_message\_code.TopSdk');

    demo：

        vendor('aliy\_message\_code.TopSdk');  
            vendor('aliy\_message\_code.top.TopClient');  
            vendor('aliy\_message\_code.top.request.AlibabaAliqinFcSmsNumSendRequest');  
            $c = new \\TopClient();  
            $c->appkey = 'XXX';  
            $c->secretKey = 'XXXXXXXX';  
            $req = new \\AlibabaAliqinFcSmsNumSendRequest();  
            $req->setExtend("123456");  
            $req->setSmsType("normal");  
            $req->setSmsFreeSignName("短信测试");  
            $req->setSmsParam("{\\"name\\":\\"1234\\"}");  
            $req->setRecNum("18210364952");  
            $req->setSmsTemplateCode("SMS_25565091");  
            $resp = $c->execute($req);  
            var_dump($resp);exit;

    结果：  

object(SimpleXMLElement)#9 (2) {  \["result"\]=>  object(SimpleXMLElement)#8 (3) {    \["err\_code"\]=>    string(1) "0"    \["model"\]=>    string(26) "104448059484^1106036210284"    \["success"\]=>    string(4) "true"  }  \["request\_id"\]=>  string(13) "147581meoqmcm" }

  

* * *

Android：（建议现在后台请求到，Android调用接口获取）