---
title: Mysql left join数据去重
tags:
  - mysql
url: 853.html
id: 853
categories:
  - MySql
date: 2018-05-30 19:36:31
---

1、查询去重后总数，不包含Null的情况：  

[``COUNT(DISTINCT `expr`,[`expr`...])``](file:///Users/wenquan%201/Library/Application%20Support/Dash/DocSets/MySQL/MySQL.docset/Contents/Resources/Documents/functions.html#function_count)

Returns a count of the number of rows with different non-`NULL` `expr` values.

If there are no matching rows, [`COUNT(DISTINCT)`](file:///Users/wenquan%201/Library/Application%20Support/Dash/DocSets/MySQL/MySQL.docset/Contents/Resources/Documents/functions.html#function_count) returns `0`.

mysql> SELECT COUNT(DISTINCT results) FROM student;

In MySQL, you can obtain the number of distinct expression combinations that do not contain `NULL` by giving a list of expressions. In standard SQL, you would have to do a concatenation of all expressions inside [`COUNT(DISTINCT ...)`](file:///Users/wenquan%201/Library/Application%20Support/Dash/DocSets/MySQL/MySQL.docset/Contents/Resources/Documents/functions.html#function_count).

  

2、查询数据去重：

select distinct(st.id) from student st join course cs on st.id = cs.user_id