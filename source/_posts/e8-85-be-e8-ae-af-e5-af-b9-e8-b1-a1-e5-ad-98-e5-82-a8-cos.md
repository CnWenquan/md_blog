---
title: 腾讯对象存储--COS
tags:
  - Android
  - PHP
url: 330.html
id: 330
categories:
  - 服务器
date: 2016-11-04 15:32:47
---

玩儿了下阿里，当然也要试试腾讯了。虽然感觉腾讯云不如阿里，但人家的发展也算是很快的了。

* * *

php：

    1、准备工作：

①：使用的是PHP CI框架  

②：下载SDK  

[https://github.com/tencentyun/cos-php-sdk-v4](https://github.com/tencentyun/cos-php-sdk-v4)  

放到model目录下，（调用方便）

    2、修改conf文件：

    const APP_ID = 'XXXXXX';
    const SECRET_ID = 'XXXXXXXXXXXXX';
    const SECRET_KEY = 'XXXXXXXXXXXXXXXXXXXXX';

    3、直接上代码：

上传文件与列出文件&目录列表：

    Controller：

    //上传文件
    public function putVideo(){
            var\_dump($this->cos\_model->upload_file('video',"/data/wwwroot/default/www.wenquanpage.com/1.mp4",'1.mp4'));
    }
    
    //列出文件与目录
    public function get_list(){
         var\_dump($this->cos\_model->get\_folder\_list('video'));
    }

    Model：

<?php
defined('BASEPATH') OR exit('No direct script access allowed');
require\_once \_\_DIR__ . '/cos-php/include.php';
use qcloudcos\\Cosapi;


class Cos\_model extends CI\_Model{

    function __construct() {
        parent::__construct();
        header('Content-Type:text/html;charset=utf-8');
        $this->load->driver('cache');
        Cosapi::setTimeout(3600);
        Cosapi::setRegion('tj');
    }
    
    
    
    /*
     * 上传本地文件
     * $bucketName, bucket名字
     * $srcPath, 本地要上传文件的全路径
     * $dstPath, 文件在COS服务端的全路径
     * $bizAttr = null, 文件属性
     * $slicesize = null, 文件分片大小
     * $insertOnly = null   同名文件是否进行覆盖
     */
    public function upload\_file($bucketName,$file\_path,$filename){
        $result = Cosapi::upload($bucketName, $file_path,$filename);
        return $result;
    }
    
    /*
     * 列出目录中的文件和目录
     * $bucketName, bucket名称
     * $path, 目录的全路径
     * $num = 20, 要查询的目录/文件数量
     * $pattern = 'eListBoth', eListBoth：列举文件和目录；eListDirOnly：仅列举目录；eListFileOnly：仅列举文件
     * $order = 0, 默认正序(=0), 填1为反序
     * $context = null透传字段，查看第一页，则传空字符串。若需要翻页，需要将前一页返回值中的context透传到参数中。order用于指定翻页顺序。若order填0，则从当前页正序/往下翻页；若order填1，则从当前页倒序/往上翻页
     */
    public function get\_folder\_list($bucketName,$path='',$num=20,$pattern='eListBoth',$order=0,$context=null){
        $result = Cosapi::listFolder($bucketName, $path, $num, $pattern, $order, $context);
        return $result;
    }
    
    
}

* * *

Android实现上传：（话说腾讯的产品为啥这么多签名算法呢，能不能给开发者省点儿心！！！）

1、引入：

compile 'com.squareup.okhttp3:okhttp:3.4.1'
compile 'com.squareup.okio:okio:1.11.0'
compile 'com.qcloud:cos_api:4.2'

2、权限：

<uses-permission android:name="android.permission.INTERNET"/>
<uses-permission android:name="android.permission.ACCESS\_WIFI\_STATE"/>
<uses-permission android:name="android.permission.ACCESS\_NETWORK\_STATE"/>
<uses-permission android:name="android.permission.READ\_PHONE\_STATE"/>
<uses-permission android:name="android.permission.WAKE_LOCK"/>
<uses-permission android:name="android.permission.MOUNT\_UNMOUNT\_FILESYSTEMS" />
<uses-permission android:name="android.permission.WRITE\_EXTERNAL\_STORAGE" />
<uses-permission android:name="android.permission.READ\_EXTERNAL\_STORAGE"/>

3、代码：

package com.sparrow.costest;

import android.content.Context;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.util.Log;
import android.widget.Toast;

import com.tencent.cos.COSClient;
import com.tencent.cos.COSConfig;
import com.tencent.cos.common.COSEndPoint;
import com.tencent.cos.model.COSRequest;
import com.tencent.cos.model.COSResult;
import com.tencent.cos.model.PutObjectRequest;
import com.tencent.cos.model.PutObjectResult;
import com.tencent.cos.task.listener.ITaskListener;
import com.tencent.cos.task.listener.IUploadTaskListener;
import com.tencent.cos.utils.SHA1Utils;

import java.util.Random;


public class MainActivity extends AppCompatActivity {

    private static final String TAG = "MainActivity";
    private static  COSConfig cosConf;
    private static COSClient cosclient;
    private static Context context= this;
    String appid =  "1252275355";
    String secretId = "AKIDfaUL3Zuxxp8Kbi6RqA0kIenkbJsGoUKy";
    String secretKey = "lfbO0PbcTJy55wb5dS7HIAsOi3BAxukU";

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        initConf();

        upload_file();
    }

    private void initConf() {
        cosConf = new COSConfig();
        //设置园区
        cosConf.setEndPoint(COSEndPoint.COS_TJ);
        cosclient = new COSClient(context,appid,cosConf,null);
    }


    private void upload_file() {
        String bucket = "video";
        String cosPath = "明天会更好.mp3";
        String srcPath = "/data/wwwroot/default/www.wenquanpage.com/movie.webm";
        String sign = getSign(bucket,cosPath,srcPath);
        Log.i(TAG, "upload_file: "+sign);

//        UploadFileRequest uploadFileRequest = new UploadFileRequest(bucket,cosPath,srcPath);

        PutObjectRequest putObjectRequest = new PutObjectRequest();
        putObjectRequest.setBucket(bucket);
        putObjectRequest.setCosPath(cosPath);
        putObjectRequest.setSrcPath(srcPath);
        putObjectRequest.setSign(sign);
        putObjectRequest.setListener(new ITaskListener() {
            @Override
            public void onSuccess(COSRequest cosRequest, COSResult cosResult) {
                PutObjectResult result = (PutObjectResult) cosResult;
                if(result != null){
                    StringBuilder stringBuilder = new StringBuilder();
                    stringBuilder.append(" 上传结果： ret=" + result.code + "; msg =" +result.msg + "\\n");
                    stringBuilder.append(" access\_url= " + result.access\_url == null ? "null" :result.access_url + "\\n");
                    stringBuilder.append(" resource\_path= " + result.resource\_path == null ? "null" :result.resource_path + "\\n");
                    stringBuilder.append(" url= " + result.url == null ? "null" :result.url);
                    Log.w("TEST",stringBuilder.toString());
                }
            }

            @Override
            public void onFailed(COSRequest cosRequest, COSResult cosResult) {
                Log.w("TEST","上传出错： ret =" +cosResult.code + "; msg =" + cosResult.msg);
            }
        });

    }




    public String getSign(String bucket,String cosPath,String srcPath) {

        Credentials credentials = new Credentials(appid,secretId,secretKey);
        try {
            String sign = Sign.getPeriodEffectiveSign("void", "/data/wwwroot/default/www.wenquanpage.com/1.mp4",credentials,3600);
        } catch (AbstractCosException e) {
            e.printStackTrace();
        }

        return sign;
    }

}