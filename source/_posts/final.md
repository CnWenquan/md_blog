---
title: final
url: 249.html
id: 249
categories:
  - Java
date: 2016-09-26 17:01:12
tags:
---

(1) final修饰变量(成员变量或局部变量),则成为常量，只能赋值一次

       final typevariableName;//变量名一般大写

修饰成员变量时，定义时同时给出初始值，而修饰局部变量时不做要求。

(2)final修饰方法，则该方法不能被子类重写

    finalreturnTypemethodName(paramList)

   {

      …

   }

(3)final修饰类，则类不能被继承

     final classfinalClassName{

         …

     }