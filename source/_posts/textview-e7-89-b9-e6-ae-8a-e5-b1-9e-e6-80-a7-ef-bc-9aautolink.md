---
title: Day03
url: 33.html
id: 33
categories:
  - Android课程总结
date: 2016-05-20 18:01:06
tags:
---

Android第三天总结
------------

**摘要**：TextView特殊属性、像素尺寸的区别及算法、ImageView的特殊属性、RadioGroup监听器、checkBox的监听器、Spinner二级联动

TextView特殊属性：autoLink  

web、phone、map、email、all、none  

**dp、sp、dip、px的区别及换算**

dp:    dememsion    independent    pixels

sp:    scaled    independent    pixels

dpi:    dots    per    inch    ---->(每英寸屏幕上的像素点的个数)

px:    =dp*像素密度比值

px     =dp*（当前屏幕的密度/160）

    3.7''    320*480px  

ImageView的特殊属性：  

fitXY、matrix  

fitCenter（既放大、有缩小、无裁切）

CenterInside（只缩小，不放大，无裁切）

Center（无放大、无缩小、有裁切）

CenterCrop（有放大、有缩小、有裁切）

  

RadioGrop的监听器：

RadioGrop.onCheckedChangeListener：返回点击的checkId，根据CheckId返回选项内容

  

  

checkBox的监听器：

    compoundButton.OnCheckedChangeListener  

  

Spinner二级联动：

1.  初始化省的适配器，并设置适配器
    
2.  初始化市的适配器，并设置适配器
    

1.  执行省Spinner的监听器
    
    在省的监听器中：
    
    ①：获取被选择的item的pid
    
    ②：给适配器中添加新的数据源
    
    ③：让适配器通知已经设置了该适配器的view进行刷新