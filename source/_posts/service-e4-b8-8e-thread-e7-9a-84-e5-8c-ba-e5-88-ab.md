---
title: Service 与 Thread 的区别
url: 102.html
id: 102
categories:
  - Android课程总结
date: 2016-07-12 20:46:53
tags:
---

**Thread**：Thread是程序执行的最小单元，它是分配CPU的基本单位。可以用 Thread 来执行一些异步的操作。

**Service**：Service 是android的一种机制，当它运行的时候如果是Local Service，那么对应的 Service 是运行在主进程的 main 线程上的。如：onCreate，onStart 这些函数在被系统调用的时候都是在主进程的 main 线程上运行的。如果是Remote Service，那么对应的 Service 则是运行在独立进程的 main 线程上。  

Service 生命周期：**onCreate　　onStart　　onDestroy　　onBind**