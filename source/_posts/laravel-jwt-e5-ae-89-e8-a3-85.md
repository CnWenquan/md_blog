---
title: laravel   JWT安装
tags:
  - laravel
url: 567.html
id: 567
categories:
  - Laravel
  - PHP
date: 2017-08-02 14:28:47
---

详细地址参照：[http://laravelacademy.org/post/3640.html](http://laravelacademy.org/post/3640.html)  

问题：在生成token时报错（Non-static method Tymon\\JWTAuth\\JWTAuth::attempt() should not be called statically）

解决办法：

You need to

use JWTAuth;

instead of

use Tymon\\JWTAuth\\JWTAuth;

Because you have already added the `JWTAuth` alias to 'app.php'.

Take a look at the [creating tokens](https://github.com/tymondesigns/jwt-auth/wiki/Creating-Tokens) examples.

  

也即在app.php中配置了JWTAuth的门面，直接use JWTAuth即可引入。

  

测试：

  

生成token  

public function test(){  
 if (Auth::attempt(\['email' =\> 'xiaoyuanline@huaying.com', 'password' =\> 'secret'\])) {  
 // Authentication passed...  
 return JWTAuth::fromUser(Auth::user());  
 }else{  
 return '未登陆成功';  
 }}

验证：

blog.dev?token={生成的token}

  public function index(){  
  
 $user = JWTAuth::parseToken()->authenticate();  
//        $res = $user->can('manage_user');  
 dd($user);  
 }

可以打印出token，ok