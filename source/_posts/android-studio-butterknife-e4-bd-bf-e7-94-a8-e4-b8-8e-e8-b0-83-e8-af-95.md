---
title: Android Studio ButterKnife使用与调试
url: 305.html
id: 305
categories:
  - Android
date: 2016-10-21 18:44:47
tags:
---

一、安装（以Mac为例）：

    点击Config-》plugins到达下面图片，第一个就是ButterKnife，点击右侧的Install并重启

![79D18691-DCBC-4601-8855-FF0D500A8258.png](/ueditor/php/upload/image/20161021/1477045557244061.png "1477045557244061.png")

  

设置好以后再项目里切换到Android模式，右键app，有一个列表，选择Open Module Settings，

![BDBD9A82-3375-4EDE-A82A-8BF0F9AD81F3.png](/ueditor/php/upload/image/20161021/1477045869287020.png "1477045869287020.png")

  

在界面中点击dependencies，Mac中在下方有个加号，Windows版本在右边，点开后有个Library Dependences，在输入框输入ButterKnife，选择第一个，然后点击OK

![B6FE979B-DA0D-4EEC-9EE5-DF2BB6001DCA.png](/ueditor/php/upload/image/20161021/1477046068163392.png "1477046068163392.png")

  

安装结束！！！

* * *

调试空指针：

    第一步，在gradle(project)的dependencies中添加

classpath 'com.neenbedankt.gradle.plugins:android-apt:1.8'

![717E889D-4217-493F-B7A1-30C06A4C24E3.png](/ueditor/php/upload/image/20161021/1477046313407597.png "1477046313407597.png")

第二步：在build.gradle(Module)中添加两个位置：

顶部：

apply plugin: 'android-apt'

  
dependenties中添加：

apt 'com.jakewharton:butterknife-compiler:8.4.0'