---
title: Android Studio Git的使用
url: 230.html
id: 230
categories:
  - Android
date: 2016-09-09 10:52:08
tags:
---

分几个步骤：  

  

    1、准备，下载git到你的电脑上,下载地址: _**[http://git-scm.com/downloads](http://git-scm.com/downloads) **_安装,直接下一步下一步就好,记得下载位置就好,配置的时候要用。

  

    2、在studio中设置git插件：File->Setting->Version Control->Git,添加您刚刚安装的git地址目录,例如: **_C:\\Program Files\\Git\\bin\\git.exe 。_**

**_    3、点击file->new->project from version control->git,会有以下一个弹出框。  
_**

**_    ![blob.png](/ueditor/php/upload/image/20160909/1473388806975182.png "1473388806975182.png")_**

**_    在url中填写你要连的git项目地址，点击右边Test按钮，如果没问题，会有如下这个弹框（success）  
_**

**_    ![blob.png](/ueditor/php/upload/image/20160909/1473388913807594.png "1473388913807594.png")_**

**_    中间框是本地项目路径，最后一个框是项目名称。  
_**

    点击clone会弹出选择是在new Windows还是this Windows。（建议选择new Windows）

    4、建好项目后这时空空如也，建议在本地先把框架搭好，然后复制到建好的空项目中，然后在AS中选择project视图，点击向上的VCS，选择要上传的文件，就ok了。  

![blob.png](/ueditor/php/upload/image/20160909/1473389472226208.png "1473389472226208.png")