---
title: Android ANR概念（应用程序无响应）
url: 91.html
id: 91
categories:
  - Android
date: 2016-07-09 10:29:25
tags:
---

    在Android上，如果长时间应用程序无响应，或不够灵敏，系统会向用户显示一个对话框，这个对话框称作应用程序无响应对话框。用户可以选择让程序继续运行，但是，他们在使用你的应用程序时，并不希望每次都处理这个对话框。因此，在程序里对响应性能的设计很重要，这样，系统不会显示ANR给用户。

    不同的组件发生 ANR 的时间不一样,主线程(Activity、Service)是 5 秒,BroadCastReceiver 是 10 秒

     

    解决方案：

    将所有耗时的操作，比如访问网络，Socket通信，查询大量SQL语句，复杂逻辑计算等都放在子线程中去，然后通过Handler.sendMessage、runonUIThread、AsyncTask等方式更新UI。无论如何都要确保用户界面操作的流畅度。如果耗时操作让用户等待，可以在界面上设置进度条。