---
title: mysql大量数据数据迁移方案
tags:
  - mysql
url: 770.html
id: 770
categories:
  - MySql
date: 2018-03-07 18:25:04
---

### 导出数据库数据

导出我们使用`mysqldump`即可，我们可以导出一个数据库

mysqldump -uroot -ptedsadasda ibm > ibm.sql

如果想导出所有数据库或者多个数据库，可以参考

# 导出所有数据库mysqldump -uroot -proot --all-databases >/tmp/all.sql# 导出两个数据库mysqldump -uroot -proot --databases db1 db2 >/tmp/user.sql

如果你只想迁移一个或者多个表，可以使用

mysqldump -uroot -proot --databases db1 --tables a1 a2  >/tmp/db1.sql

### 数据多个主机之间的迁移

迁移使用`scp`或者`sftp`均可以，我们这里讲一下`scp`，一般情况下，我们在服务器使用如下命令即可把我们备份的数据库传到目标主机，类似`ssh`

scp /root/ibm.sql root@172.19.2.75:/root/

如果想反过来操作，把文件从远程主机`copy`到当前系统，也很简单。

scp root@172.19.2.75:/root/ibm.sql /root/ibm.sql

### 异地备份

其实我们完全可以考虑`mysqldump`异地导出到我们的服务器，因为我发现`scp`传输大量数据真的太慢了

mysqldump -uroot -pdsdsdsd -h41.93.52.20 ibm > ibm.sql

这样直接即可导出到我们服务器

### 数据库还原

数据库还原非常简单，使用`mysql -u root -p [dbname] < backup.sq`即可，如：

mysql -u root -p < /root/ibm.sql