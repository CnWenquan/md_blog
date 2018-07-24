---
title: java 迭代器Iterator、枚举enum
url: 381.html
id: 381
categories:
  - Java
date: 2016-12-02 15:17:57
tags:
---

Iterator  

一、功能：迭代器，主要是遍历集合

二、使用：

    ①初始化对象：Iterator it = 集合.iterator();

    ②遍历：

    while(it.hasNext()){//判断是否有下一个  

        it.next();//移动指针，向下遍历一个元素  

    }

    it.remove();//只能删一次

三、使用增强for循环可以代替迭代器

* * *

enum

枚举格式：

    enum 枚举名{

        常量1，常量2，...常量N;     

    }

使用：

    enum Color{

        RED,WHITE,BLACK;  

    }

1、初始化

Color c1 = Color.RED;

3、可以用在switch中