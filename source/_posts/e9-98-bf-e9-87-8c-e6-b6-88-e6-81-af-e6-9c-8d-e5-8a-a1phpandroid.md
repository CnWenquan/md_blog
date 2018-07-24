---
title: 阿里消息服务php
tags:
  - PHP
url: 343.html
id: 343
categories:
  - CI
  - PHP
date: 2016-11-10 15:32:18
---

概念：

阿里云消息服务(Message Service)是一种高效、可靠、安全、便捷、可弹性扩展的分布式消息服务。MNS能够帮助应用开发者在他们应用的分布式组件上自由的传递数据、通知消息，构建松耦合系统。

消息服务提供了两种模型：

*   队列模型
    
*   主题模型
    

两种功能模型的区别：

队列模型支持一对一发送和接收消息；主题模型支持一对多发布和订阅消息，并且支持多种消息推送方式；

由于一对多的主题模型是收费的，鄙人囊中羞涩，只测试了队列模式。

* * *

PHP：  

  准备：CI框架、

    php-sdk:

        [http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/32381/cn_zh/1478166644841/aliyun-mns-php-sdk-1.3.2.zip?spm=5176.doc32381.2.1.uSbo16&file=aliyun-mns-php-sdk-1.3.2.zip](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/32381/cn_zh/1478166644841/aliyun-mns-php-sdk-1.3.2.zip?spm=5176.doc32381.2.1.uSbo16&file=aliyun-mns-php-sdk-1.3.2.zip)

  代码：  

<?php

defined('BASEPATH') OR exit('No direct script access allowed');

require\_once (dirname(\_\_FILE__) . '/php\_mns\_sdk/mns-autoloader.php');

use AliyunMNS\\Client;
use AliyunMNS\\Requests\\SendMessageRequest;
use AliyunMNS\\Requests\\CreateQueueRequest;
use AliyunMNS\\Exception\\MnsException;

class AliaMnsTest extends CI_Controller {

    function __construct() {
        parent::__construct();
        header('Content-Type:text/html;charset=utf-8');
        $this->endPoint = 'http://**********.mns.cn-beijing.aliyuncs.com/';
        $this->accessId = '**************';
        $this->accessKey = '***************';
        $this->client = new Client($this->endPoint, $this->accessId, $this->accessKey);
        $this->queueName = 'ceshi';
        $this->queue = $this->client->getQueueRef($this->queueName,FALSE);
    }

    /*
     * 创建队列
     */

    public function createQueue() {
        // 1. 首先初始化一个client
//        $this->client = new Client($this->endPoint, $this->accessId, $this->accessKey);
        // 2. 生成一个CreateQueueRequest对象。CreateQueueRequest还可以接受一个QueueAttributes参数，用来初始化生成的queue的属性。
        // 2.1 对于queue的属性，请参考help.aliyun.com/document_detail/27476.html
        $request = new CreateQueueRequest($this->queueName);
        try {
            $res = $this->client->createQueue($request);
            // 2.2 CreateQueue成功
            echo "QueueCreated! \\n";
        } catch (MnsException $e) {
            // 2.3 可能因为网络错误，或者Queue已经存在等原因导致CreateQueue失败，这里CatchException并做对应的处理
            echo "CreateQueueFailed: " . $e . "\\n";
            echo "MNSErrorCode: " . $e->getMnsErrorCode() . "\\n";
            return;
        }
    }

    /*
     * 发送消息
     */

    public function sendMessage() {
        // 1. 首先获取Queue的实例
        // 1.1 PHP SDK默认会对发送的消息做Base64 Encode，对接收到的消息做Base64 Decode。
        // 1.2 如果不希望SDK做这样的Base64操作，可以在getQueueRef的时候，传入参数$base64=FALSE。即$queue = $this->client->getQueueRef($queueName, FALSE);
//        $this->client->getQueueRef($this->queueName, FALSE);
//        $this->queue = $this->client->getQueueRef($this->queueName,FALSE);
        $messageBody = "test";
        // 2. 生成一个SendMessageRequest对象
        // 2.1 SendMessageRequest对象本身也包含了DelaySeconds和Priority属性可以设置。
        // 2.2 对于Message的属性，请参考help.aliyun.com/document_detail/27477.html
        $request = new SendMessageRequest($messageBody);
        try {
            $res = $this->queue->sendMessage($request);
            // 3. 消息发送成功
            echo "MessageSent! \\n";
        } catch (MnsException $e) {
            // 4. 可能因为网络错误，或MessageBody过大等原因造成发送消息失败，这里CatchException并做对应的处理。
            echo "SendMessage Failed: " . $e . "\\n";
            echo "MNSErrorCode: " . $e->getMnsErrorCode() . "\\n";
            return;
        }
    }
    
    /*
     * 获取消息
     */
    public function getMessage() {
        $receiptHandle = NULL;
        try {
            // 1. 直接调用receiveMessage函数
            // 1.1 receiveMessage函数接受waitSeconds参数，无特殊情况这里都是建议设置为30
            // 1.2 waitSeconds非0表示这次receiveMessage是一次http long polling，如果queue内刚好没有message，那么这次request会在server端等到queue内有消息才返回。最长等待时间为waitSeconds的值，最大为30。
            $res = $this->queue->receiveMessage(30);
            echo "ReceiveMessage Succeed! \\n";
            //返回消息正文
            var_dump($res\['messageBody'\]);
            // 2. 获取ReceiptHandle，这是一个有时效性的Handle，可以用来设置Message的各种属性和删除Message。具体的解释请参考：help.aliyun.com/document_detail/27477.html 页面里的ReceiptHandle
            $receiptHandle = $res->getReceiptHandle();
        } catch (MnsException $e) {
            // 3. 像前面的CreateQueue和SendMessage一样，我们认为ReceiveMessage也是有可能出错的，所以这里加上CatchException并做对应的处理。
            echo "ReceiveMessage Failed: " . $e . "\\n";
            echo "MNSErrorCode: " . $e->getMnsErrorCode() . "\\n";
            return;
        }
        // 这里是用户自己的处理消息的逻辑。Sample里就直接略过这一步了。
        // 如果这里发生了程序崩溃或卡住等异常情况，对应的Message会在VisibilityTimeout之后重新可见，从而可以被其他进程处理，避免消息丢失。
        // 4. 现在消息已经处理完了。我们可以从队列里删除这条消息了。
        try {
            // 5. 直接调用deleteMessage即可。
            $res = $this->queue->deleteMessage($receiptHandle);
            echo "DeleteMessage Succeed! \\n";
        } catch (MnsException $e) {
            // 6. 这里CatchException并做异常处理
            // 6.1 如果是receiptHandle已经过期，那么ErrorCode是MessageNotExist，表示通过这个receiptHandle已经找不到对应的消息。
            // 6.2 为了保证receiptHandle不过期，VisibilityTimeout的设置需要保证足够消息处理完成。并且在消息处理过程中，也可以调用changeMessageVisibility这个函数来延长消息的VisibilityTimeout时间。
            echo "DeleteMessage Failed: " . $e . "\\n";
            echo "MNSErrorCode: " . $e->getMnsErrorCode() . "\\n";
            return;
        }
    }
    
    /*
     * 删除队列
     */
    public function deleteQueues(){
        try {
            $this->client->deleteQueue($this->queueName);
            echo "DeleteQueue Succeed! \\n";
        } catch (MnsException $e) {
            echo "DeleteQueue Failed: " . $e;
            return;
        }
    }

}

建议封装成CI_Model类，创建有参构造方法，根据对象调用相应的方法.

* * *