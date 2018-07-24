---
title: Laravel使用vaptcha做手势验证
tags:
  - php
    - laravel
categories:
  - php、laravel
date: 2018-06-19 19:09:33
---

## 准备工作：
[官网](https://www.vaptcha.com)获取vid与key
## 后端开发接口：
#### 安装
使用composer安装：
`composer require Vaptcha/vaptcha-sdk;`
#### 创建控制器与路由：
创建路由：

```
Route::get('vaptcha/challenge', 'VaptchaController@getChallenge');
Route::get('vaptcha/downtime', 'VaptchaController@getDownTime');
```
创建控制器：

```
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use Vaptcha\Vaptcha;

class VaptchaController extends Controller
{
    //

    private $vaptcha;

    public function __construct(){
        $this->vaptcha = new Vaptcha('vid', 'key'); // 这里替换成前面获取到的vid与key
    }

    public function response($status, $msg, $data){
        return response()->json([
            'status' =>  $status,
            'msg' => $msg,
            'data' => $data
        ], $status);
    }

    public function responseSuccess($data){
        return $this->response(200, 'success', $data);
    }

    public function getChallenge(Request $request){
        $data = $this->vaptcha->getChallenge($request->sceneid);
        return $this->responseSuccess($data);
    }

    public function getDownTime(Request $request) {
        $data = $this->vaptcha->downTime($request->data);
        return $data;
    }
}

```
访问接口：

```
{
    "status": 200,
    "msg": "success",
    "data": {
        "vid": "5b28a46fa485e50410181691",
        "challenge": "bd0e97f9aeb749e288efb68ef2cac69d00bfcjdjciifgwwy4"
    }
}
```

## 前端使用vaptcha：

```
<style>
    .vaptcha-init-main {
        display: table;
        width: 100%;
        height: 100%
        background-color: #EEEEEE;
    }
    .vaptcha-init-loading {
        display: table-cell;
        vertical-align: middle;
        text-align: center
    }
    .vaptcha-init-loading>a {
        display: inline-block;
        width: 18px;
        height: 18px;
        border: none;
    }
    .vaptcha-init-loading>a img {
        vertical-align: middle
    }
    .vaptcha-init-loading .vaptcha-text {
        font-family: sans-serif;
        font-size: 12px;
        color: #CCCCCC;
        vertical-align: middle
    }
</style>
<!-- 点击式按钮建议宽度不低于200px,高度介于36px与46px  -->
<!-- 嵌入式仅需设置宽度，高度根据宽度自适应，最小宽度为200px -->
<div id="vaptcha_container" style="width:300px;height:36px;">
   <!--vaptcha_container是用来引入Vaptcha的容器，下面代码为预加载动画，仅供参考-->
   <div class="vaptcha-init-main">
      <div class="vaptcha-init-loading">
          <a href="/" target="_blank"><img src="https://cdn.vaptcha.com/vaptcha-loading.gif"/></a>
          <span class="vaptcha-text">Vaptcha启动中...</span>
      </div>
  </div>
</div>
<script src="https://cdn.vaptcha.com/v.js"></script>
<!-- 引入jquery -->
<script src="/js/jquery.min.js"></script> 
<script type="text/javascript">
//这里使用到验证场景，传过去的参数即为对应的编号，比如之前登录的对应的编号即为01
    $.get('http://test.wq/vaptcha/challenge?sceneid=01', function(response){
        console.log(response);
        var options={
            vid: response.data.vid, //验证单元id, string, 必填
            challenge: response.data.challenge, //验证流水号, string, 必填
            container:"#vaptcha_container",//验证码容器, HTMLElement或者selector, 必填
            type:"click", //必填，表示点击式验证模式,
            effect:'float', //验证图显示方式, string, 可选择float, popup, 默认float
            https:false, //协议类型, boolean, 可选true, false
            color:"#57ABFF", //按钮颜色, string
            outage:"/api/vaptcha/downtime", //服务器端配置的宕机模式接口地址
            success:function(token,challenge){//验证成功回调函数, 参数token, challenge 为string, 必填
                //todo:执行人机验证成功后的操作
            },
            fail:function(){//验证失败回调函数  
                //todo:执行人机验证失败后的操作
            }
        }

        var obj;
        window.vaptcha(options,function(vaptcha_obj){
            obj = vaptcha_obj;
            obj.init();
        });
});
</script>
```

成功后的效果：
![](/../images/15293980842481.jpg)












