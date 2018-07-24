---
title: Mysql Match Against全文检索
url: 324.html
id: 324
categories:
  - MySql
date: 2016-11-02 18:11:37
tags:
---

好文章就该抓下来  

* * *

mysql中模糊搜索通常用like '%keyword%' 来进行模糊搜索，例如

select * from table where title like '%唠吧小站%'

MySQL 4.x以上提供了全文检索支持 MATCH ……AGAINST 模式（不区分大小写）
-----------------------------------------------

### 建立全文索引的表的存储引擎类型必须为MyISAM

问题是match   against对中文模糊搜索支持不是太好

新建一个utf8 MyISAM类型的表并建立一个全文索引  ：

1.  CREATE TABLE articles (
    
2.      id INT UNSIGNED AUTO_INCREMENT NOT NULL PRIMARY KEY,
    
3.      title VARCHAR(200),
    
4.      body TEXT,
    
5.      FULLTEXT (title,body)
    
6.  ) ENGINE=MyISAM  DEFAULT >
    

其中FULLTEXT(title, body) 给title和body这两列建立全文索引，之后检索的时候注意必须同时指定这两列。

给这个表添加点测试数据

1.  INSERT INTO articles (title,body) VALUES
    
2.      ('MySQL Tutorial','DBMS stands for DataBase ...'),
    
3.      ('How To Use MySQL Well','After you went through a ...'),
    
4.      ('Optimizing MySQL','In this tutorial we will show ...'),
    
5.      ('1001 MySQL Tricks','1. Never run mysqld as root. 2. ...'),
    
6.      ('MySQL vs. YourSQL','In the following database comparison ...'),
    
7.      ('MySQL Security','When configured properly, MySQL ...');
    

3\. 全文检索测试

1.  SELECT * FROM articles   WHERE MATCH (title,body) AGAINST ('database'); 
    

注意 MATCH (title,body) 里面的值必须是前面建立全文索引的两个字段不能少。

mysql 默认支持全文检索的字符长度是4，可以用SHOW VARIABLES LIKE 'ft\_min\_word\_len' 来查看指定的字符长度，也可以在mysql配置文件my.ini 更改最小字符长度，方法是在my.ini 增加一行 比如：ft\_min\_word\_len = 2，改完后重启mysql即可。  

另外，MySQL还会计算一个词的权值，以决定是否出现在结果集中，具体如下：

mysql在集和查询中的对每个合适的词都会先计算它们的权重，一个出现在多个文档中的词将有较低的权重(可能甚至有一个零权重)，因为在这个特定的集中，它有较低的语义值。否则，如果词是较少的，它将得到一个较高的权重，mysql默认的阀值是50%，上面‘you’在每个文档都出现，因此是100%，只有低于50%的才会出现在结果集中。

全文检索语法
------

SELECT * FROM articles WHERE MATCH (title,body)     AGAINST ('+apple -banana' IN BOOLEAN MODE);   + 表示AND，即必须包含。- 表示NOT，即不包含。

SELECT * FROM articles WHERE MATCH (title,body)     AGAINST ('apple banana' IN BOOLEAN MODE);   apple和banana之间是空格，空格表示OR，即至少包含apple、banana中的一个。

SELECT * FROM articles WHERE MATCH (title,body)     AGAINST ('+apple banana' IN BOOLEAN MODE);   必须包含apple，但是如果同时也包含banana则会获得更高的权重。

SELECT * FROM articles WHERE MATCH (title,body)     AGAINST ('+apple ~banana' IN BOOLEAN MODE);   ~ 是我们熟悉的异或运算符。返回的记录必须包含apple，但是如果同时也包含banana会降低权重。但是它没有 +apple -banana 严格，因为后者如果包含banana压根就不返回。

SELECT * FROM articles WHERE MATCH (title,body)     AGAINST ('+apple +(>banana <orange)' IN BOOLEAN MODE);   返回同时包含apple和banana或者同时包含apple和orange的记录。但是同时包含apple和banana的记录的权重高于同时包含apple和orange的记录。

  

转载地址：http://www.lao8.org/article\_1602/MATCH\_AGAINST_mysql