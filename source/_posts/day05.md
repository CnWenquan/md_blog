---
title: Day05
url: 40.html
id: 40
categories:
  - Android课程总结
date: 2016-05-20 20:27:18
tags:
---

Android05 总结
------------

**摘要**：制作糗事百科的过程

**一、自定义适配器 BaseAdapter**

1：继承于BaseAdapter的子类，实现其中的四个方法：

    getCount()

    getItem()

    getItemId()

    getView()

2：内部类–》ViewHolder（view持有者，其中定义item布局文件的所有UI控件）

3：getView()——利用convertView复用的原理来实现ListView优化。

    ①：定义ViewHolder    mHolder;

    ②：判断convertView是否为null；

    ③：convertView初始化：利用布局填充器填充item布局

    ④：实例化ViewHolder：作用是将item布局中所有 的UI控件进行初始化findViewByID

    ⑤：给convertView设置标签：将ViewHolder对象作为标签设置在convertView上

    ⑥：从convertView上撕标签

    ⑦：给每个UI赋值

    ⑧：返回convertView

**二、在自定义适配器中定义ListView刷新的方法**

数据源.clear();

数据源.addAll(list);

notifyDatasetChanged();

**三、ListView的滚动监听器，实现ListView数据分页加载**

AbsListView.OnScrollListener()  

OnScroll():判断是否滑到了最后一条

OnScrollStateChanger():如果已经滑到最后一条，并且状态为0，则加载下一页内容