---
title: Day17
url: 60.html
id: 60
categories:
  - Android课程总结
date: 2016-05-26 20:51:28
tags:
---

Android 第17天总结
--------------

**摘要**：EventBus实现过程、LruCache、二次采样生成图片缩略图

**EventBus实现过程（导入eventBus包）：**

1、定义一个eventBus类，一个接收数据的属性并添加其get与set方法  

2、定义一个有参（传数据），一个无参的构造方法

3、在需要抛出数据的地方，调用eventBus:

EventBus.getDefault().post(new MyEvent(bytes));
    4、给需要调用的上下文注册EventBus
    5、定义一个方法，接收EventBus的数据：

@Subscribe
public void getData(MyEvent event){

   byte\[\] result = event.getData();
   String jsonString = new String(result);
   List<Map<String,String>> list = loadNetworkData(jsonString);
   if (CurPage==1)
      totalList.clear();
   totalList.addAll(list);
   adapter.notifyDataSetChanged();
}

  

**LruCache：最近最少使用**  

1、LruCache分为：强引用、软引用、弱引用、虚引用  

        强引用的特点：OOM（内存容易溢出） 

2、LruCache使用方法：

        1、定义一个LruCache子类  

        2、定义一个构造方法，参数为最大缓存大小  

        3、两个方法比较重要：sizeOf---》计算大小、entryRemoved（）---》最适宜移除的缓存文件  

        4、使用的时候跟Map集合特别像，有相应的键值对，只要

lruCache.put(URL_STRING\[flag\],bm);

**         3、三层缓存策略：  
**

**                1、现在内存的强引用中查找，找到返回  
**

**                2、在SDCard的私有Cache中找，找到返回，并在强引用中缓存一份  
**

**                3、在网络中加载，返回数据，并在强引用与SDCard中缓存一份  
**

**二次采样：**

**        目的：减小内存的消耗、并且图片不失真  
**

**        步骤：  
**

        第一次采样：只采集边界信息，不采集图片的像素信息  

        第二次采样：按照特定的options配置，即采边又采集图片的像素信息。  

    private Bitmap createThumbnail(byte\[\] data,int newWidth,int newHeight){
        //二次采样,只能缩不能放
        BitmapFactory.Options opts = new BitmapFactory.Options();

        //第一次采样
        BitmapFactory.decodeByteArray(data,0,data.length,opts);


        opts.inJustDecodeBounds = true;
        //第二次采样
        opts.inPreferredConfig = Bitmap.Config.RGB_565;//
        int width = opts.outWidth;//输出的高度
        int height = opts.outHeight;//输出的宽度

        //计算缩放比例

        int widthRatio = 0;
        int heightRatio = 0;
        int sampleSize = 0;
        if (newWidth!=0&&newHeight==0){
            sampleSize = (int) (width / (float)newWidth);
        }else if(newHeight!=0&&newWidth==0){
            sampleSize = (int) (height / (float)newHeight);
        }


//        int sampleSize = (int) (width / (float)newWidth);

        opts.inSampleSize = sampleSize;
        opts.inJustDecodeBounds = false;//是否只采集图像的边界信息
        return BitmapFactory.decodeByteArray(data,0,data.length,opts);
    }