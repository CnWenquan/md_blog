---
title: Laravel添加自定义辅助函数
tags:
  - laravel
url: 562.html
id: 562
categories:
  - Laravel
  - PHP
date: 2017-08-01 11:51:59
---

1、在app目录下新建一个文件helpers.php  

2、在composer.json文件的autoload字典中添加"files":\["app/helpers.php"\]

"autoload": {
    "classmap": \[
        "database/seeds",
        "database/factories"
    \],
    "psr-4": {
        "App\\\": "app/"
    },
    "files":\["app/helpers.php"\]
},

3、执行命令：composer dumpautoload OK搞定  

  

4、测试：在Helpers中写个方法：

<?php  
/**  
 \* Created by PhpStorm. \* User: wenquan \* Date: 2017/8/1 \* Time: 上午11:43 */  
function test(){  
 return 'test';  
}

  

在任意控制器中调用测试：

return test();

返回“test”