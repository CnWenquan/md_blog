---
title: mysql 方法与函数（1）
tags:
  - mysql
url: 877.html
id: 877
categories:
  - MySql
date: 2018-06-05 15:44:46
---

**两个数相除取余数**：MOD(N,M) 返回N除M的余数

    mysql> SELECT MOD(29,9);
    +-----------+
    | mod(29,9) |
    +-----------+
    | 2 |
    +-----------+
    1 rows in set (0.00 sec)
    

**修改查询返回值类型：**

    mysql> SELECT 38.8, CAST(38.8 AS CHAR);
    -> 38.8, '38.8'
    

**等于“=、\\<=>、\\<>、!=、\\<=、\\<、>=、>”特殊用法：**

    #(a,b) = (x,y) is equivalent to:
    (a = x) AND (b = y)
    
    #(a,b) <=> (x,y) is equivalent to:
    (a <=> x) AND (b <=> y)
    
    #(a,b) <> (x,y) and (a,b) != (x,y) are equivalent to:
    (a <> x) OR (b <> y)
    

**NULL-safe equal : [`<=>`](file:///Users/wenquan%201/Library/Application%20Support/Dash/DocSets/MySQL/MySQL.docset/Contents/Resources/Documents/functions.html#operator_equal-to)**

    mysql> SELECT 1 <=> 1, NULL <=> NULL, 1 <=> NULL;
    -> 1, 1, 0
    mysql> SELECT 1 = 1, NULL = NULL, 1 = NULL;
    -> 1, NULL, NULL
    

**布尔值判断：**

    # is boolean value
    mysql> SELECT 1 IS TRUE, 0 IS FALSE, NULL IS UNKNOWN;
    -> 1, 1, 1
    
    # is not boolean value
    mysql> SELECT 1 IS NOT UNKNOWN, 0 IS NOT UNKNOWN, NULL IS NOT UNKNOWN;
    -> 1, 1, 0
    
    # is null
    mysql> SELECT 1 IS NULL, 0 IS NULL, NULL IS NULL;
    -> 0, 0, 1
    

**expr between min and max:( min \\<= expr and expr \\<= max)**

    mysql> SELECT 2 BETWEEN 1 AND 3, 2 BETWEEN 3 and 1;
    -> 1, 0
    mysql> SELECT 1 BETWEEN 2 AND 3;
    -> 0
    mysql> SELECT 'b' BETWEEN 'a' AND 'c';
    -> 1
    mysql> SELECT 2 BETWEEN 2 AND '3';
    -> 1
    mysql> SELECT 2 BETWEEN 2 AND 'x-3';
    -> 0
    

**coalesce(value….)：返回在列表中的第一个非空的值，或者没有非空值返回NULL**

    mysql> SELECT GREATEST(2,0);
    -> 2
    mysql> SELECT GREATEST(34.0,3.0,5.0,767.0);
    -> 767.0
    mysql> SELECT GREATEST('B','A','C');
    -> 'C'