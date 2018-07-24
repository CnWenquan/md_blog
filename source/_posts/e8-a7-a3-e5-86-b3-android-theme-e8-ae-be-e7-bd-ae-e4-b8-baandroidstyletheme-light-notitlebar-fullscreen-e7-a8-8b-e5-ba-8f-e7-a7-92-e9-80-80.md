---
title: '解决 android Theme 设置为Android:style/Theme.Light.NoTitleBar.Fullscreen 程序秒退'
tags:
  - Android
url: 441.html
id: 441
categories:
  - Android
date: 2017-01-13 18:09:48
---

问题描述：

在Android Studio 的Manifest添加全屏去除ActionBar的Theme会出现闪退情况：

android:theme="@android:style/Theme.Light.NoTitleBar.Fullscreen"

删除样式，闪退问题解决。

  

解决办法：

全屏模式与Android Studio自动添加的****Activity有冲突，去掉继承的类名就OK了。