---
title: Android友盟第三方登录+单点登录
tags:
  - Android
url: 444.html
id: 444
categories:
  - Android
date: 2017-01-17 16:54:51
---

准备工作就不在此赘述了，官网教程很详细。

我重点关注的是怎样获取用户的信息，所以就省去了好多功能！

首先得有个微信，qq，新浪的log界面，于是就有了第一个Activity，上面展示的就是微信与qq

![16625342074929846.png](/ueditor/php/upload/image/20170117/1484642407349641.png "1484642407349641.png")

其次，点击后会有一个登陆的按钮，然后就是progressBar等待授权后获取用户的信息，如果成功，则返回数据到TextView中，整套流程结束。

![303967292021333822.png](/ueditor/php/upload/image/20170117/1484642537863315.png "1484642537863315.png")![294350469141827727.png](/ueditor/php/upload/image/20170117/1484642549948044.png "1484642549948044.png")

于是，有了第二个Activity

代码地址：

[https://git.oschina.net/wenquanpage/UmengGetUserInfo](https://git.oschina.net/wenquanpage/UmengGetUserInfo)  

遇到的坑：

1、缺少资源xxx：

    没有把res文件完全引入！！！

2、

ResContainer.getResourceId(context,"drawable",list.get(position).mIcon)

这个方法报错，查了下是因为方法的参数类型发生了改变，我参考的旧的demo，只好把sdk也跟着换成旧的，待继续研究

  

* * *

单点登录：  

    还没有想到很好的方法，目前大概的思路就是除了登录注册外，其他的所有接口都带一个token，这样没有即时响应的效果，待完善。