---
title: >-
  Cannot get property 'buildToolsVer' on extra properties extension as it does
  not exist
tags:
  - Android
url: 449.html
id: 449
categories:
  - Android
date: 2017-01-18 11:01:16
---

下载网易MI demo导入AS出现上面的问题，点击openfile会定位到错误位置

这是因为引入的三方库的build.gradle中会根据我们项目build.gradle的ext｛｝中定义的数据进行配置。如果我们项目中没有定义，就会出现这个错误。

解决办法：

    打开项目的build.gradle。  
     新建一个ext{}，在里面定义三方库需要引用的变量。

ext {
    compileSdkVersion = 21
    buildToolsVersion = '22.0.1'
    minSdkVersion = 14
    targetSdkVersion = 23
    versionCode = 31
    versionName = '3.3.0'
    targetCompatibility = 1.7
    sourceCompatibility = 1.7
}

    然后点击try again，等待gradle完成配置