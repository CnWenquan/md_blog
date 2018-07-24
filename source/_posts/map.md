---
title: Map
url: 245.html
id: 245
categories:
  - Java
date: 2016-09-26 16:14:27
tags:
---

一、特点：实现Map接口的集合类用来存储“键-值”映射对

二、键的存储特点：

①Map实现类中存储的“键-值”映射对是通过键来唯一标识，Map底层的“键”是用Set来存放的。

②所以存入Map中的映射对的“键”对应的类必须重写hashcode()和equals()方法。

三、Map.Entry

以一个键值对为一个对象

四、Map的常用方法

◦Object  put(Object key, Object value);　//将指定的“键-值”对存入Map中

◦Object  get(Object key);　　//返回指定键所映射的值

◦Object  remove(Object key);　//根据指定的键把此“键-值”对从Map中移除。

◦boolean  containsKey(Object key);   //判断此Map是否包含指定键的“键-值”对。

◦boolean  containsValue(Object value);　    //判断此Map是否包含指定值的“键-值”对。

◦boolean  isEmpty();　　//判断此Map中是否有元素。

◦int  size();　  //获得些Map中“键-值”对的数量。

◦Set  keySet(); 　  //返回此Map中包含的键的Set集。

◦Collection values();　  //返回此Map中包含的值的Collection集。

◦void  clear();　  //清空Map中的所有“键-值”对。