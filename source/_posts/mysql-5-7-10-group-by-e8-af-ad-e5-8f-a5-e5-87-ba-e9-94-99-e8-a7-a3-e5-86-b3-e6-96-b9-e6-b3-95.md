---
title: mysql 5.7.10 group by语句出错解决方法
tags:
  - mysql
url: 615.html
id: 615
categories:
  - MySql
date: 2017-10-17 14:54:09
---

    先把mysql结构展示出来:

![图片.png](/ueditor/php/upload/image/20171017/1508220178944665.png "1508220178944665.png")

    需求是统计影片项目id，并且查出相应的名称：

     sql语句：

    "select name,count(project\_ids),project\_ids from showtime\_schedules group by project\_ids"

    多么简单的一句语句，一运行打脸了:

this is incompatible with sql\_mode=only\_full\_group\_by

    字面意思是sql\_model=only\_full\_group\_by限制了，导致在以往mysql中能正常查询的sql，在5.7不能用了。

解决方法  
    经过使用修改sql_mode测试，发现可以在不修改sql的情况下，不再报错了。

SET sql_mode = '';  
      SELECT * FROM \`sdb\_login\_log\` group by ip;

如果不想修改sql_mode的话，但还是要输出表的所有 字段，可以修改sql达到。  
       方法1（返回结果同上SQL）  
SELECT ip,max(id) id,max(group\_level) group\_level,max(uid) uid,max(username) username,max(last\_login)  last\_login FROM \`sdb\_login\_log\` group by ip;  
方法2（返回结果和上SQL不同）：  
SELECT ip,id,group\_level,uid,username,last\_login FROM \`sdb\_login\_log\` group by ip,id,group\_level,uid,username,last\_login;

方法2的返回结果不一样，所以 不能采用。  
升级MYSQL5.7是为了更好的性能，据我猜测，MYSQL5.7对sql_mode的限制，也是为了更好的查询性能。所以如果在开发阶段，如果要使用mysql5.7版本建议大家阅读一下5.7的文档，然后将自己的sql写的更严谨，尽量少用select * ，只查出自己想要的数据列即可。

方法3：我只需要project_ids与name两个字段，故我把两个字段加到group中也是不影响结果的：

"select name,count(project\_ids),project\_ids from showtime\_schedules group by project\_ids,name"