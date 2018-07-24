---
title: phpspider网络爬虫
url: 377.html
id: 377
categories:
  - PHP
date: 2016-11-28 18:28:14
tags:
---

互联网上廉价的信息不计其数，要想获取到免费的午餐，就得掌握获取的技术！

1、火车头：

        火车头是一个很好用的爬虫工具，可以说只要能看懂html标签，知道正则就可以上手了，操作简单，可以直接连接数据库，生成sqllite文件。但也有局限性（也许我没研究透），有些页面需要ajax请求，没法儿实现，不能多进程，不能代理ip，不能设置访问浏览器。。。优缺点明显，适合简单的网页爬取。

2、php爬虫框架：phpspider

        码代码的时候正在爬起点中文网的免费小说，目前已经爬了近3000本小说，还是不错的。具体的也是按照开发文档实现的，没啥好装13的，直接上代码、截图：

文档地址：

[https://doc.phpspider.org/callback.html](https://doc.phpspider.org/callback.html)  

git源码地址：

1.  https://github.com/owner888/phpspider
    

代码实现：

<?php
ini\_set("memory\_limit", "1024M");
require dirname(\_\_FILE\_\_).'/../core/init.php';

/* Do NOT delete this comment */
/* 不要删除这段注释 */

$configs = array(
    'name' => '免费小说_飞卢小说网',
    //'log_show' => true,
    'tasknum' => 1,
    //'save\_running\_state' => true,
    'domains' => array(
        'qidian.com',
        'f.qidian.com',
        'book.qidian.com'
    ),
    'scan_urls' => array(
        'http://f.qidian.com/all?size=-1&sign=-1&tag=-1&chanId=-1&subCateId=-1&orderId=&update=-1&page=1&month=-1&style=1&action=1'
    ),
    'list\_url\_regexes' => array(
        "http://f.qidian.com/all?size=-1&sign=-1&tag=-1&chanId=-1&subCateId=-1&orderId=&update=-1&page=\\d+&month=-1&style=1&action=1"
    ),
    'content\_url\_regexes' => array(
        "http://book.qidian.com/info/\\d+",
    ),
    'max_try' => 5,
    //'export' => array(
        //'type' => 'csv',
        //'file' => PATH_DATA.'/qiushibaike.csv',
    //),
    //'export' => array(
        //'type'  => 'sql',
        //'file'  => PATH_DATA.'/qiushibaike.sql',
        //'table' => 'content',
    //),
    'export' => array(
        'type' => 'db', 
        'table' => 'quanshu',
    ),
    'fields' => array(
        array(
            'name' => "name",
            'selector' => "//div\[contains(@class,'book-info')\]//h1//em",
            'required' => true,
        ),
        array(
            'name' => "author",
            'selector' => "//div\[contains(@class,'book-info')\]//h1//a\[1\]",
            'required' => true,
        ),
        array(
            'name' => "introduce",
            'selector' => "//div\[contains(@class,'book-intro')\]//p",
            'required' => true,
        ),
        array(
            'name' => "create_time",
            'selector' => "//div\[contains(@class,'book-intro')\]//p",
            'required' => true,
        ),
        array(
            'name' => "url",
            'selector' => "//div\[contains(@class,'book-intro')\]//p",
            'required' => true,
        )
    ),
);

$spider = new phpspider($configs);


$spider->on\_extract\_field = function($fieldname, $data, $page) 
{
    
    
    if ($fieldname == 'create_time') 
    {
        // 用当前采集时间戳作为发布时间
        $data = time();
    }
    else if ($fieldname == 'name'||$fieldname == 'author'||$fieldname == 'introduce'){
        $encode=mb\_detect\_encoding($data,array('ASCII','UTF-8','GB2312','GBK'));
        if ($encode == 'GBK'||$encode == 'GB2312'){
            $data = iconv('GBK','UTF-8',$data);
        }
    }
    else if($fieldname == 'url'){
        $data = $page\['url'\];
    }  
    return $data;
};

$spider->start();

数据库配置文件：

<?php

$GLOBALS\['config'\]\['db'\] = array(
    'host'  => '127.0.0.1',
    'port'  => 3306,
    'user'  => 'root',
    'pass'  => 'KOtXA2iV',
    'name'  => 'spider',
);

$GLOBALS\['config'\]\['redis'\] = array(
    'host'      => '127.0.0.1',
    'port'      => 6379,
    'pass'      => '',
    'prefix'    => 'phpspider',
    'timeout'   => 30,
);

include "inc_mimetype.php";

截图：

![QQ截图20161128182328.png](/ueditor/php/upload/image/20161128/1480328735275386.png "1480328735275386.png")

  

日志：

![QQ截图20161128182315.png](/ueditor/php/upload/image/20161128/1480328751585818.png "1480328751585818.png")

  

数据库：

![QQ截图20161128182806.png](/ueditor/php/upload/image/20161128/1480328808364704.png "1480328808364704.png")