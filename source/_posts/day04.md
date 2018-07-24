---
title: Day04
url: 37.html
id: 37
categories:
  - 未分类
date: 2016-05-20 18:53:13
tags:
---

Android第四天总结
------------

**摘要**：多选框的全选和反选、Spinner监听器、ListView监听器与适配器、自定义适配器的流程、Glide加载网络图片、利用Intent实现拨打电话及发送短信等功能。

**多选框的全选和反选：**

全选框的单击事件：  

    isChecked()  

    setChecked(boolean)  

Spinner的监听器：  

AdapterView.OnItemSelectedListener        Spinner的item条目被选中监听器Spinner不能使用OnItemClickListener

  

LIstView  

A.适配器：ArrayAdapter、SimpleAdapter

B.监听器：OnItemClickListener

**ListView自定义适配器：**

1：继承于BaseAdapter的子类，实现其中的四个方法：  

    getCount()  

    getItem()  

    getItemId()  

    getView()  

2：内部类--》ViewHolder（view持有者，其中定义item布局文件的所有UI控件）

3：getView()------利用convertView复用的原理来实现ListView优化。

    ①：定义ViewHolder    mHolder;  

    ②：判断convertView是否为null；  

    ③：convertView初始化：利用布局填充器填充item布局  

    ④：实例化ViewHolder：作用是将item布局中所有 的UI控件进行初始化findViewByID  

    ⑤：给convertView设置标签：将ViewHolder对象作为标签设置在convertView上  

    ⑥：从convertView上撕标签  

    ⑦：给每个UI赋值  

    ⑧：返回convertView  

**Glide加载网络图片：**

Glide.with(context).load(url).into(ImageView)  

利用Intent实现拨打电话及发短信：  

Intent intent = new Intent();  

intent.setAction(Intent.ACTION_CALL)

intent.setData(uri.pause("tel:"+phone));

startActivity(intent);