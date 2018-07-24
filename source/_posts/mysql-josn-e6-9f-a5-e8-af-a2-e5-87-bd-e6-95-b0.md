---
title: mysql josn查询函数
tags:
  - mysql
url: 747.html
id: 747
categories:
  - MySql
date: 2018-02-12 14:05:09
---

新版 Mysql 中加入了对 JSON Document 的支持，可以创建 JSON 类型的字段，并有一套函数支持对JSON的查询、修改等操作，下面就实际体验一下

创建带有 JSON 字段的表
--------------

比如一个‘文章’表，字段包括

id、标题 title、标签 tags

一篇文章会有多个标签，tags 就可以设为 JSON 类型

建表语句如下：

CREATE TABLE \`article\` (

 \`id\` mediumint(8) unsigned NOT NULL AUTO_INCREMENT,\`title\` varchar(200) NOT NULL,

 \`tags\` json DEFAULT NULL,PRIMARY KEY (\`id\`)) ENGINE=InnoDB;

插入数据
----

插入一条带有 JSON 内容的数据，执行 insert 语句：

INSERT INTO \`article\` (\`title\`, \`tags\`)VALUES ('体验 Mysql JSON','\["Mysql", "Database"\]');

这里插入的是一个 JOSN 数组 \[“Mysql”, “Database”\]

查询 article 表中的所有内容，可以看到新插入的这条数据

![Mysql里的JSON系列操作函数](/ueditor/php/upload/image/20180212/1518415419158446.jpg)

  

  

查询
--

使用 JSON 函数做两个简单的查询

1. 查找带有标签”Mysql”的所有文章

SELECT * FROM \`article\`WHERE JSON_CONTAINS(tags, '\["Mysql"\]');

2. 查找标签中以”Data”开头的文章

SELECT * FROM \`article\`WHERE JSON_SEARCH(tags, 'one', 'Data%') IS NOT NULL;

JSON_SEARCH 函数中3个参数的含义：

1 要查找的文档

2 查找的范围，有两个选项，’one’ 查找第一个符合条件的，’all’查找所有符合条件的

3 查找的条件

JSON Path
---------

JSON Path 用来定位文档中的目标字段，例如

SELECT JSON_EXTRACT('{"id": 1, "name": "mysql"}','$.name');

结果为：mysql

JSON_EXTRACT() 是JSON提取函数，$.name 就是一个 JSON path，表示定位文档的 name 字段

JSON path 是以 $ 开头，下面看几个更多的示例

{"num": 123,"arr": \[1, 2\],"obj": {"a": 3,"b": 4}

}

$.num //结果：123$.arr //结果：\[1, 2\]$.arr\[1\] //结果：1$.obj.a //结果：3$**.b //结果：4

使用 JSON path 的查询示例

SELECTtags->"$\[0\]" as 'tag'FROM \`article\`;

更新数据
----

例如想给文章添加一个 “dev” tag，更新的条件是已经包含 “Mysql” 标签，并且还没有 “dev” 标签的数据

更新语句如下：

UPDATE \`article\`SET tags = JSON\_MERGE(tags, '\["dev"\]')WHEREJSON\_SEARCH(tags, 'one', 'dev') IS NULLANDJSON_SEARCH(tags, 'one', 'Mysql') IS NOT NULL;

  

  

![Mysql里的JSON系列操作函数 - 1](/ueditor/php/upload/image/20180212/1518415420309294.jpg)

  

  

可以看到成功添加了 “dev” 标签

再比如想把 “Mysql” 这个标签更新为 “Mysql 5.7.13″，更新语句如下：

UPDATE \`article\` set tags = JSON_SET(tags, ‘$\[0\]’, ‘Mysql 5.7.13’) ;

上面体验了 JSON\_MERGE 和 JSON\_SET，用于修改JSON的函数还有很多，例如：

JSON_INSERT (doc, path, val\[, path, val\]…)

插入数据

JSON_REPLACE (doc, path, val\[, path, val\]…)

替换数据

JSON\_ARRAY\_APPEND (doc, path, val\[, path, val\]…)

向数组尾部追加数据

JSON_REMOVE (doc, path\[, path\]…)

从指定位置移除数据

* * *

进一步探索：

需求是需要查询一个json串里面的某个字段模糊匹配

{

    'city':'XXX',  

    'address':'xxxxxxx'  

}

sql语句混合使用：

SELECT * FROM \`fdm\_facility\` WHERE json\_search(json_extract(info,'$.city'),'all','%北京%') IS NOT NULL ;

php 框架laravel中的使用：

$Facility->where('info->city','like','%北京%');

框架中太简单，会产生严重的依赖性！！！不过我喜欢。