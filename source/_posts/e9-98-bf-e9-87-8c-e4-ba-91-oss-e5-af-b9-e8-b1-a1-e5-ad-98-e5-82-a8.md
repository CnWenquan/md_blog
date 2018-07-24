---
title: 阿里云-OSS对象存储
url: 322.html
id: 322
categories:
  - 服务器
date: 2016-11-02 16:13:04
tags:
---

产品概述
====

阿里云对象存储服务（Object Storage Service，简称 OSS），是阿里云提供的海量、安全、低成本、高可靠的云存储服务。您可以通过调用 API，在任何应用、任何时间、任何地点上传和下载数据，也可以通过 Web 控制台对数据进行简单的管理。OSS 适合存放任意类型的文件，适合各种网站、开发企业及开发者使用。

  

典型使用场景
======

OSS主要应用于以下场景。

图片和音视频等应用的海量存储
--------------

OSS可用于图片、音视频、日志等海量文件的存储。各种终端设备、Web网站程序、移动应用可以直接向OSS写入或读取数据。OSS支持流式写入和文件写入两种方式，如下图所示：

![](/ueditor/php/upload/image/20161102/1478074214210290.png)

网页或者移动应用的静态和动态资源分离
------------------

利用BGP带宽，OSS可以实现超低延时的数据直接下载。也可以配合阿里云CDN加速服务，为图片、音视频、移动应用的更新分发提供最佳体验，如下图所示：

![](/ueditor/php/upload/image/20161102/1478074214369180.png)

云端数据处理
------

上传文件到OSS后，可以配合媒体转码服务(MTS)和图片处理服务（IMG）进行云端的数据处理，如下图所示：

![](/ueditor/php/upload/image/20161102/1478074215985497.png)

* * *

以上是阿里云官网公布的信息

PHP的使用：（直接上测试了的例子吧）

<?php
defined('BASEPATH') OR exit('No direct script access allowed');

use OSS\\OssClient;
use OSS\\Core\\OssException;

class Oss\_model extends CI\_Model{

    function __construct() {
        parent::__construct();
        header('Content-Type:text/html;charset=utf-8');
        $this->load->driver('cache');
    }
    
    /*
     * 获取ossClient对象
     */
    function get_ossClient(){
        $app_id = 'XXXXXXXX';
        $app_key = 'XXXXXXXXXXXX';
        $endpoint ='http://oss-cn-shanghai-internal.aliyuncs.com';
        try {
            $ossClient = new OssClient($app\_id, $app\_key, $endpoint);
            return $ossClient;
        } catch (OssException $e) {
            return $e->getMessage();
        }
    }
    
    /*
     * 创建存储空间
     */
    function create_ossClient($ossClient,$bucket){
        try {
            $ossClient->createBucket($bucket);
            return 'OK';
        } catch (OssException $e) {
            return $e->getMessage();
        }
    }
    

    /**
     * 上传字符串作为object的内容
     *
     * @param OssClient $ossClient OSSClient实例
     * @param string $bucket 存储空间名称
     * @return null
     */
    function putObject($ossClient, $bucket,$file\_name,$file\_path)
    {
        $content = file\_get\_contents($file_path);
        try{
            $ossClient->putObject($bucket, $file_name, $content);
        } catch(OssException $e) {
            printf(\_\_FUNCTION\_\_ . ": FAILED\\n");
            printf($e->getMessage() . "\\n");
            return;
        }
        print(\_\_FUNCTION\_\_ . ": OK" . "\\n");
    }
    
    /**
    * 上传指定的本地文件内容
    *
    * @param OssClient $ossClient OSSClient实例
    * @param string $bucket 存储空间名称
    * @return null
    */
   function uploadFile($ossClient, $bucket,$file\_name,$file\_path)
   {
       try{
           $ossClient->uploadFile($bucket, $file\_name, $file\_path);
       } catch(OssException $e) {
           printf(\_\_FUNCTION\_\_ . ": FAILED\\n");
           printf($e->getMessage() . "\\n");
           return;
       }
       print(\_\_FUNCTION\_\_ . ": OK" . "\\n");
   }
   
   
   /**
    * get\_object\_to\_local\_file
    *
    * 获取object
    * 将object下载到指定的文件
    *
    * @param OssClient $ossClient OSSClient实例
    * @param string $bucket 存储空间名称
    * @return null
    */
   function getFileToLocalFile($ossClient, $bucket,$file_name,$localfile)
   {
       $options = array(
           OssClient::OSS\_FILE\_DOWNLOAD => $localfile,
       );
       try{
           $ossClient->getObject($bucket, $file_name, $options);
       } catch(OssException $e) {
           printf(\_\_FUNCTION\_\_ . ": FAILED\\n");
           printf($e->getMessage() . "\\n");
           return;
       }
       print(\_\_FUNCTION\_\_ . ": OK, please check localfile: 'upload-test-object-name.txt'" . "\\n");
   }
   
   /**
    * 获取object的内容
    *
    * @param OssClient $ossClient OSSClient实例
    * @param string $bucket 存储空间名称
    * @return null
    */
   function getFileToMemcache($ossClient, $bucket,$file_name)
   {
       try{
           $content = $ossClient->getObject($bucket, $file_name);
       } catch(OssException $e) {
           printf(\_\_FUNCTION\_\_ . ": FAILED\\n");
           printf($e->getMessage() . "\\n");
           return;
       }
       print(\_\_FUNCTION\_\_ . ": OK" . "\\n");
   }
   
   /**
    * 获取object的内容
    *范围下载
    * @param OssClient $ossClient OSSClient实例
    * @param string $bucket 存储空间名称
    * @return null
    */
   function getFilesBy($ossClient, $bucket,$file_name)
   {
       try{
           $options = array(OssClient::OSS_RANGE => '0-100');
           $content = $ossClient->getObject($bucket, $file_name, $options);
       } catch(OssException $e) {
           printf(\_\_FUNCTION\_\_ . ": FAILED\\n");
           printf($e->getMessage() . "\\n");
           return;
       }
       print(\_\_FUNCTION\_\_ . ": OK" . "\\n");
   }
   
   /**
    * 如果文件在指定的时间之后修改过，则下载文件
    *
    * @param OssClient $ossClient OSSClient实例
    * @param string $bucket 存储空间名称
    * @return null
    */
   function getFilesBySearch($ossClient, $bucket,$file_name,$search= array())
   {
       try{
           $options = array(
               OssClient::OSS_HEADERS => $search,
       );
           $content = $ossClient->getObject($bucket,$file_name , $options);
       } catch(OssException $e) {
           printf(\_\_FUNCTION\_\_ . ": FAILED\\n");
           printf($e->getMessage() . "\\n");
           return;
       }
       print(\_\_FUNCTION\_\_ . ": OK" . "\\n");
   }
   
   
   /**
    * 列出Bucket内所有目录和文件， 根据返回的nextMarker循环调用listObjects接口得到所有文件和目录
    *
    * @param OssClient $ossClient OssClient实例
    * @param string $bucket 存储空间名称
    * @return null
    */
   function listAllFiles($ossClient, $bucket)
   {
       //构造dir下的文件和虚拟目录
//       for ($i = 0; $i < 100; $i += 1) {
//           $ossClient->putObject($bucket, "dir/obj" . strval($i), "hi");
//           $ossClient->createObjectDir($bucket, "dir/obj" . strval($i));
//       }
       $prefix = '';
       $delimiter = '';
       $nextMarker = '';
       $maxkeys = 30;
       while (true) {
           $options = array(
               'delimiter' => $delimiter,
               'prefix' => $prefix,
               'max-keys' => $maxkeys,
               'marker' => $nextMarker,
           );
           var_dump($options);
           try {
               $listObjectInfo = $ossClient->listObjects($bucket, $options);
           } catch (OssException $e) {
               printf(\_\_FUNCTION\_\_ . ": FAILED\\n");
               printf($e->getMessage() . "\\n");
               return;
           }
           // 得到nextMarker，从上一次listObjects读到的最后一个文件的下一个文件开始继续获取文件列表
           $nextMarker = $listObjectInfo->getNextMarker();
           $listObject = $listObjectInfo->getObjectList();
           $listPrefix = $listObjectInfo->getPrefixList();
           var_dump($listObject);
           var_dump($listPrefix);
           if ($nextMarker === '') {
               break;
           }
       }
   }
   
   /**
    * 判断object是否存在
    *
    * @param OssClient $ossClient OSSClient实例
    * @param string $bucket bucket名字
    * @return null
    */
   function doesObjectExist($ossClient, $bucket,$file_name)
   {
       try{
           $exist = $ossClient->doesObjectExist($bucket, $file_name);
       } catch(OssException $e) {
           printf(\_\_FUNCTION\_\_ . ": FAILED\\n");
           printf($e->getMessage() . "\\n");
           return;
       }
       print(\_\_FUNCTION\_\_ . ": OK" . "\\n");
       var_dump($exist);
   }
   
   
   /**
    * 创建虚拟目录
    *
    * @param OssClient $ossClient OSSClient实例
    * @param string $bucket 存储空间名称
    * @return null
    */
   function createObjectDir($ossClient, $bucket,$dir_name) {
       try{
           $ossClient->createObjectDir($bucket, $dir_name);
       } catch(OssException $e) {
           printf(\_\_FUNCTION\_\_ . ": FAILED\\n");
           printf($e->getMessage() . "\\n");
           return;
       }
       print(\_\_FUNCTION\_\_ . ": OK" . "\\n");
   }
   
   /**
    * 删除object
    *
    * @param OssClient $ossClient OSSClient实例
    * @param string $bucket bucket名字
    * @return null
    */
   function deleteFile($ossClient, $bucket,$file_name)
   {
       try{
           $ossClient->deleteObject($bucket, $file_name);
       } catch(OssException $e) {
           printf(\_\_FUNCTION\_\_ . ": FAILED\\n");
           printf($e->getMessage() . "\\n");
           return;
       }
       print(\_\_FUNCTION\_\_ . ": OK" . "\\n");
   }
   
    /**
    * 批量删除object
    *
    * @param OssClient $ossClient OSSClient实例
    * @param string $bucket bucket名字
    * @return null
    */
    function deleteFiles($ossClient, $bucket,$files= array())
    {
       try{
           $ossClient->deleteObjects($bucket, $files);
       } catch(OssException $e) {
           printf(\_\_FUNCTION\_\_ . ": FAILED\\n");
           printf($e->getMessage() . "\\n");
           return;
       }
       print(\_\_FUNCTION\_\_ . ": OK" . "\\n");
    }
    
    /**
    * 拷贝object
    *
    * @param OssClient $ossClient OSSClient实例
    * @param string $bucket bucket名字
    * @return null
    */
   function copyFile($ossClient, $from\_bucket,$from\_file,$to\_bucket,$to\_file)
   {
       try{
           $ossClient->copyObject($from\_bucket, $from\_file, $to\_bucket, $to\_file);
       } catch(OssException $e) {
           printf(\_\_FUNCTION\_\_ . ": FAILED\\n");
           printf($e->getMessage() . "\\n");
           return;
       }
       print(\_\_FUNCTION\_\_ . ": OK" . "\\n");
   }
    
}

* * *

  

Android端调试：

1、引入：

compile 'com.squareup.okhttp3:okhttp:3.4.1'
compile 'com.squareup.okio:okio:1.11.0'
compile 'com.aliyun.dpa:oss-android-sdk:2.3.0'

2、权限：

<uses-permission android:name="android.permission.INTERNET"></uses-permission>
<uses-permission android:name="android.permission.ACCESS\_NETWORK\_STATE"></uses-permission>
<uses-permission android:name="android.permission.ACCESS\_WIFI\_STATE"></uses-permission>
<uses-permission android:name="android.permission.WRITE\_EXTERNAL\_STORAGE"></uses-permission>

3、混淆设置：

在Android Studio自带的proguard-rules.pro文件中

-keep class com.alibaba.sdk.android.oss.** { *; }
-dontwarn okio.**
-dontwarn org.apache.commons.codec.binary.**

4、敲代码：

package com.example.a123.aliyunosstest;

import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.util.Log;

import com.alibaba.sdk.android.oss.ClientException;
import com.alibaba.sdk.android.oss.OSS;
import com.alibaba.sdk.android.oss.OSSClient;
import com.alibaba.sdk.android.oss.ServiceException;
import com.alibaba.sdk.android.oss.callback.OSSCompletedCallback;
import com.alibaba.sdk.android.oss.common.auth.OSSCredentialProvider;
import com.alibaba.sdk.android.oss.common.auth.OSSPlainTextAKSKCredentialProvider;
import com.alibaba.sdk.android.oss.internal.OSSAsyncTask;
import com.alibaba.sdk.android.oss.model.ListObjectsRequest;
import com.alibaba.sdk.android.oss.model.ListObjectsResult;

public class MainActivity extends AppCompatActivity {

    public static OSS oss;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        initOss();
        getFileLists();
    }

    private void initOss() {
        String endpoint = "http://oss-cn-shanghai.aliyuncs.com";
        OSSCredentialProvider credentialProvider = new OSSPlainTextAKSKCredentialProvider("XXXXX","XXXXXXXXXXXXX");
        oss = new OSSClient(getApplicationContext(),endpoint,credentialProvider);
    }

    public void getFileLists() {
        ListObjectsRequest listObjects = new ListObjectsRequest("wenquan-picture");

        listObjects.setPrefix("file");

        OSSAsyncTask task = oss.asyncListObjects(listObjects, new OSSCompletedCallback<ListObjectsRequest, ListObjectsResult>() {
            @Override
            public void onSuccess(ListObjectsRequest listObjectsRequest, ListObjectsResult listObjectsResult) {
                Log.i("AyncListObjects","Success!");
                for (int i=0;i<listObjectsResult.getObjectSummaries().size();i++){
                    Log.d("AyncListObjects", "--->>object: " + listObjectsResult.getObjectSummaries().get(i).getKey() + " "
                            + listObjectsResult.getObjectSummaries().get(i).getETag() + " "
                            + listObjectsResult.getObjectSummaries().get(i).getLastModified());
                }
            }

            @Override
            public void onFailure(ListObjectsRequest listObjectsRequest, ClientException e, ServiceException e1) {
                // 请求异常
                if (e != null) {
                    // 本地异常如网络异常等
                    e.printStackTrace();
                }
                if (e != null) {
                    // 服务异常
                    Log.e("ErrorCode", e1.getErrorCode());
                    Log.e("RequestId", e1.getRequestId());
                    Log.e("HostId", e1.getHostId());
                    Log.e("RawMessage", e1.getRawMessage());
                }
            }
        });

        task.waitUntilFinished();
    }
}