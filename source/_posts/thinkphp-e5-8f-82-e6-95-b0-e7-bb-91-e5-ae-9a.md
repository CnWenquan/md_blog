---
title: thinkphp 参数绑定
url: 137.html
id: 137
categories:
  - PHP
date: 2016-08-02 11:10:51
tags:
---

     参数绑定是指绑定一个参数到预处理的SQL语句中的对应命名占位符或问号占位符指定的变量，并且可以提高SQL处理的效率，需要数据库驱动类的支持，目前只有`PDO和Sqlsrv驱动`支持参数绑定功能。

    TP中的参数绑定其实实现的就是Mysql的占位符功能，这样可以防止sql注入，对数据安全方面更有帮助。  

      

    手动绑定  

    参数手动绑定需要调用连贯操作的bind方法，例如：  

$Model = M('User');
$where\['name'\] = ':name';
$list = $Model->where($where)->bind(':name',I('name'))->select();

    可以支持指定绑定变量的类型参数，例如：

$Model = M('User');
$where\['id'\] = ':id';
$list = $Model->where($where)->bind(':id',I('id'),\\PDO::PARAM_INT)->select();

    也可以批量绑定，例如：  

$Model = M('User');
$where\['id'\]    =    ':id';
$where\['name'\]  =    ':name';
$bind\[':id'\]    =    array(I('id'),\\PDO::PARAM_INT);
$bind\[':name'\]  =    array(I('name'),\\PDO::PARAM_STR);
$list = $Model->where($where)->bind($bind)->select();

自动绑定  

    对于某些操作的情况（例如模型的写入和更新方法），可以支持参数的自动绑定，例如： 首先需要开启DB\_BIND\_PARAM配置参数：

'DB\_BIND\_PARAM'    =>    true

    然后，我们在使用

$Model = M('User');
$Model->name = 'thinkphp';
$Model->email = 'thinkphp@qq.com';
$Model->add();

    会自动对写入的数据进行参数绑定操作。其操作等效于：

$Model = M('User');
$Model->name = ':name';
$Model->email = ':email';
$bind\[':name'\]    = 'thinkphp';
$bind\[':email'\]   = 'thinkphp@qq.com';
$Model->bind($bind)->add();