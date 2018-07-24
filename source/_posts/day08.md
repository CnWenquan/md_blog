---
title: Day08
url: 49.html
id: 49
categories:
  - Android课程总结
date: 2016-05-21 16:22:35
tags:
---

Android 08 总结
-------------

**摘要**：ListView优化、AsyncTask异步任务基本用法

**ListView的优化：**

1.  Layout\_height可以设置成march\_parent而非wrap\_content，warp\_content的用法是自适应调整高度，会重复试几次，march_parent是根据屏幕高度，只渲染一次，这样节省了资源消耗
    
2.  采用ViewHolder，当ViewHolder为空的时候，给item贴标签，当ViewHolder不为空的时候撕标签，这样节省了每次渲染item消耗的资源
    
3.  convertView复用的好处：
    
    节省了布局填充器填充布局的时间
    
    节省了new ViewHolder()所耗费的内存空间，此外还节省了对item中每个UI控件findViewById()，也就是控件初始化的时间。
    
4.  setTag    ：撕标签
    
5.  Inflater.inflate(三个参数):三个参数有好处，具体啥好处，谁用谁知道  
    

**AsyncTask：异步任务**

A:    定义异步任务子类，重写其中四个方法  

onPreExecute方法：    事前方法  

doInBackGround方法：    进行中方法

onPostExecute:    事后方法

onProgressUpdate方法：   。。。

B：    定义一个set方法  

    new MyTask().execute(urlString);

  

C:    最后把异步任务关闭：

    myTask.cancel();