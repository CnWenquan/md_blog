---
title: 使用Android Studio的错误整理
tags:
  - Android
url: 348.html
id: 348
categories:
  - Android
date: 2016-11-11 14:31:21
---

导入一些demo的时候，比较慢，会卡在进度条上，而且还影响整个studio不能进行操作，一般我都是先断网导入，然后再把这个gradle改成本地自己的（复制能运行的项目里的就好）....

![](/ueditor/php/upload/image/20161111/1478845756192216.png)

**1、android studio 导入项目时Error:SSL peer shut down incorrectly**

导入项目到studio，显示在下载1.1.0-rc4。

![](/ueditor/php/upload/image/20161111/1478845759516755.png)

报了这个错，网上搜了下说是

Android SDK Manager下载SDK报错：Download interrupted: SSL peer shut down incorrectly

还需要设置一些文件什么的，看着挺麻烦的

然后到这里看了下

![](/ueditor/php/upload/image/20161111/1478845760174070.png)

![](/ueditor/php/upload/image/20161111/1478845763947744.png)、

![](/ueditor/php/upload/image/20161111/1478845768267434.png)

对照着自己能正常运行的项目改了一下就好了，也可以在gradle里改

2、**android studio编译时Error:(6, 34) 错误: 程序包android.support.annotation不存在，或者是读取不到xxx版本的annotation**

![](/ueditor/php/upload/image/20161111/1478845773245454.png)

这个出现的原因应该比较少，纯粹个人手贱，鼓捣坏了。fx看了下，大部分是说依赖包的问题，或者是版本不兼容的问题之类的。那些去查就好了，这里只是针对我自己的。网速不好的话，不建议

把这两个删掉，然后重新下载就ok了。

![](/ueditor/php/upload/image/20161111/1478845776954481.jpg)

3、clean的时候出现如下错误

**Error:Execution failed for task ':app:clean'.> Unable to delete file: E:\\caibao\\dangkelanqiu\\Dunk\\app\\build\\intermediates\\exploded-aar\\com.android.support\\appcompat-v7\\23.1.1\\jars\\classes.jar**

或者是v7/appcompat/R$anim.class的错误

![](/ueditor/php/upload/image/20161111/1478845778161897.png)

解决方法：打开电脑的任务管理器，找到如下进程，结束掉，再次clean就ok了

![](/ueditor/php/upload/image/20161111/1478845780847316.png)

或者这样也可以

![](/ueditor/php/upload/image/20161111/1478845782465484.png)

4，**运行时，出现非零值退出值1的错误，**

![](/ueditor/php/upload/image/20161111/1478845784578292.png)

clean Project就好

如果是values 2的话，应该就是有重复导入的包了

**5、之前项目好好的，结果突然就报错了-**gradle升级了****

**Error:(1, 0) Plugin is too old, please update to a more recent version, or set ANDROID\_DAILY\_OVERRIDE environment variable to "aed79d567e57792ed352e708d2b7ca891ff897c6"**

**<a href="fixGradleElements">Fix plugin version and sync project</a><br><a href="openFile:E:\\xxx\\xxx\\xxxx\\xxx\\app\\build.gradle">Open File</a>**

**![](/ueditor/php/upload/image/20161111/1478845786936585.png)**

**![](/ueditor/php/upload/image/20161111/1478845788997675.png)**

classpath 'com.android.tools.build:gradle:2.0.0-alpha1'  //项目里的

(1)、classpath 'com.android.tools.build:gradle:2.0.+'        //改成这个

(2)、去这个网址看最新的插件版本[https://jcenter.bintray.com/com/android/tools/build/gradle/](https://jcenter.bintray.com/com/android/tools/build/gradle/)

把gradle的版本改成那个最新的就行了

完了之后还需要更新sdk,studio会提示的

**6.Error:Execution failed for task ':app:buildInfoDebugLoader'.  
\> Exception while doing past iteration backup : Source G:\\project\\***\\app\\build\\intermediates\\builds\\debug\\37215488613481\\classes.dex and destination G:\\project\\***\\app\\build\\intermediates\\builds\\debug\\37215488613481\\classes.dex must be different**

clean一下

**7.java.lang.RuntimeException: A TaskDescription's primary color should be opaque**

![](/ueditor/php/upload/image/20161111/1478845789799427.png)

![](/ueditor/php/upload/image/20161111/1478845791737935.png)

对比一下就可以发现是主题的颜色值格式设置的不对

**8.java.lang.NoSuchFieldError: No static field emojiView of type I in class Lcom/**/**R$id**

这个问题是由于主项目覆盖了库项目的xml所致，如果主项目中的某个xml里定义了一个跟库项目中的xml同名的xml，那么库中的xml将会被覆盖。这里的覆盖意思是，编译后只有主项目中的xml以及R.id，完全没有库项目的影子。所以才会有NoSuchFieldError的错误。

这个错误不会在编译的时候提示，算得上一个陷阱了。

解决方法很简单，删除主项目中的xml，整个项目只保留一份xml即可。  
其他参考：[http://blog.csdn.net/zgf1991/article/details/47106235](http://blog.csdn.net/zgf1991/article/details/47106235)

**9.Error:Execution failed for task ':app:transformClassesWithDexForDebug'.  
\> com.android.build.api.transform.TransformException: com.android.ide.common.process.ProcessException: java.util.concurrent.ExecutionException: com.android.dex.DexIndexOverflowException: method ID not in \[0, 0xffff\]: 65536**

方法数越界，可以参考[Multidex解决方法数越界](http://xybcoder.github.io/2016/04/27/Multidex%E8%A7%A3%E5%86%B3%E6%96%B9%E6%B3%95%E6%95%B0%E8%B6%8A%E7%95%8C/)