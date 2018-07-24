---
title: >-
  在django中执行manage.py migrate命令后出现django.db.utils.OperationalError: (1366,
  "Incorrect string value
tags:
  - mysql
url: 810.html
id: 810
categories:
  - MySql
date: 2018-05-01 21:57:57
---

首先要保证代码头添加了

# -*- coding: utf-8 -*-  

并且字符串前要有加unicode标识如 **u'你好'**  

如果还是报错，可以去数据库排查一下编码问题

进入mysql命令行

mysql> show variables like '%char%';    

+--------------------------+-----------------------------------------------------------+  

| Variable_name            | Value                                                     |  

+--------------------------+-----------------------------------------------------------+  

| character\_set\_client     | utf8                                                      |  

| character\_set\_connection | utf8                                                      |  

| character\_set\_database   | latin1                                                    |  

| character\_set\_filesystem | binary                                                    |  

| character\_set\_results    | utf8                                                      |  

| character\_set\_server     | latin1                                                    |  

| character\_set\_system     | utf8                                                      |  

| character\_sets\_dir       | /usr/local/mysql-5.7.21-macos10.13-x86_64/share/charsets/ |  

如上，其中的编码有latin1的所以需要修改latin1为utf8，以下是mac中的修改办法

vim /etc/mysql/my.cnf  

添加如下内容

\[client\]   

default-character-set =utf8  

\[mysql\]   

default-character-set =utf8  

\[mysqld\]  

collation-server = utf8\_unicode\_ci  

init_connect ='SET NAMES utf8'  

character\_set\_server=utf8  

然后重启mysql服务即可