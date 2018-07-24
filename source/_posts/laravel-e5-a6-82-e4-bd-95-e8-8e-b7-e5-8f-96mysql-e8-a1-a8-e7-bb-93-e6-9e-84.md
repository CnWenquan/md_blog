---
title: Laravel如何获取mysql表结构
tags:
  - laravel
url: 765.html
id: 765
categories:
  - Laravel
  - PHP
date: 2018-03-07 18:13:04
---

有时候我们会需要查看我们数据库表的结构，在laravel中我们没有这样的函数（我没有找到），所以我们只能通过原始表达式来查询，分为两种方式

### 1、获取表完整结构，包括字段，描述等等

$info = DB::select("select * from information\_schema.columns where table\_name = 'person_info'");

### 2、如果你只想获取到表的结构和类型，可以使用如下几种方法

$info = DB::select("desc person_info");
$info = DB::select("describe person_info");
$info = DB::select("show columns from person_info");