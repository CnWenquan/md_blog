---
title: mysql中insert ignore into和replace into的使用
tags:
  - mysql
url: 692.html
id: 692
categories:
  - MySql
date: 2017-11-28 11:00:30
---

    需要从其他网站平台获取数据，以免重复获取，使用了replace into，没有多想就去获取数据了，并在获取的数据上做了相应的业务逻辑，第二次测试的时候再次获取了数据，没想到数据被替换了。。。于是找各种替换方式，最后替换为：insert ignore into，赶脚有必要总结下二者的区别。

    replace into表示插入替代数据，需求表中有PrimaryKey,或者unique索引，如果数据库已经存在数据，则用新数据替换，如果没有数据效果则和install into一样；

    insert ignore表示，如果数据库中已经存在相同的记录，则忽略当前新数据