---
title: MySQL 5.7 使用原生JSON类型
tags:
  - mysql
url: 402.html
id: 402
categories:
  - MySql
date: 2016-12-23 12:14:32
---

首先回顾一下JSON的语法规则：

数据在键值对中，
数据由逗号分隔，
花括号保存对象，
方括号保存数组。

按照最简单的形式，可以用下面的JSON表示：

{"NAME": "Brett", "email": "brett@xxx.com"}

**如何在MySQL中使用JSON类型：**

新建user表，设置lastlogininfo列为JSON类型。

mysql> CREATE TABLE user(id INT PRIMARY KEY, name VARCHAR(20) , lastlogininfo JSON);
Query OK, 0 rows affected (0.27 sec)

向user表插入普通数据与json数据。mysql会对插入的数据进行JSON格式检查，确保其符合JSON格式，若插的是不合法的数据，会出现Invalid JSON text错误。

mysql> INSERT INTO user VALUES(1 ,"lucy",'{"time":"2015-01-01 13:00:00","ip":"
192.168.1.1","result":"fail"}');
Query OK, 1 row affected (0.05 sec)

mysql> INSERT INTO user VALUES(2 ,"bobo",'{"time":"2015-10-07 06:44:00","ip":"
192.168.1.0","result":"success"}');
Query OK, 1 row affected (0.04 sec)

  

查询name为'lucy'的最后登陆信息。

mysql> SELECT lastlogininfo FROM user WHERE name = 'lucy';
+------------------------------------------------------------------------+
| lastlogininfo  |
+------------------------------------------------------------------------+
| {"ip": "192.168.1.1", "time": "2015-01-01 13:00:00", "result": "fail"} |
+------------------------------------------------------------------------+1 row in set (0.00 sec)

查询最后登陆时间在2015-10-02后的用户。JSON数据使用->操作符，其表达式为：该json列->'$.键'与JSON_EXTRACT(json列 , '$.键')等效使用。如果传入的不是一个有效的键，则返回Empty set。该表达式可以用于SELECT查询列表 ，WHERE/HAVING , ORDER/GROUP BY中，但它不能用于设置值。

表达式 ： json列->'$.键'

mysql>  SELECT * FROM user WHERE lastlogininfo ->'$.time' > '2015-10-02';
+-----------------------------------------------------------------------+
| id | name | lastlogininfo|
+-----------------------------------------------------------------------+
|  2 | bobo | {"ip": "192.168.1.0", "time": "2015-10-07 06:44:00", "result": "success"} |
+-----------------------------------------------------------------------+1 row in set (0.00 sec)