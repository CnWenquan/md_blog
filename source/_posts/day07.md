---
title: Day07
url: 47.html
id: 47
categories:
  - Android课程总结
date: 2016-05-21 15:00:56
tags:
---

Android07    总结
---------------

**摘要**：Intent属性、GridView、具有返回值的Activity页面跳转

**Intent属性：**

1.  ComponentName：    实现页面跳转，多用于App之间的跳转
    
2.  Action：    Intent属性中最重要的一个，任何页面的跳转都离不开它
    
3.  Category：    配合Action实现页面跳转
    
4.  Data：    实现页面跳转的时候传值。比如拨打电话或发送短信时，可以把电话号码放入Data中
    
5.  Extra：    实现页面跳转的时候传值。比如发送短信的时候可以将短信内容放入Extra中传递
    
6.  Type：    打开文件时指定文件类型。便于调用正确的App，一般与Data连用
    
7.  Flags：    启动模式，但比写在注册表中的启动模式更灵活  
    

**GridView:**

<GridView
        android:id="@+id/gridView_main"
        android:layout\_width="match\_parent"
        android:layout\_height="match\_parent"
        android:numColumns="auto_fit"
        android:stretchMode="columnWidth"
        android:columnWidth="90dp" 
        android:verticalSpacing="5dp"
        android:horizontalSpacing="5dp">
    </GridView>

在item布局中注意定义ImageView的布局高度或者其父控件的高度。

  

GridView的监听器：

    GridView.OnItemCheckListener

**具有返回值的Activity页面跳转:**

1.   在请求页面定义一个Intent，intent中定义setClass(),然后startActivityForResult(Intent，requestCode)
    
2.   在第二个页面getIntent(),在Intent或Bundle中赋值，然后setResult(resultCode,Intent);
    
3.  在请求页重写onActivityResult方法
    

protected void onActivityResult(int requestCode, int resultCode,
	                Intent data) {
		super.onActivityResult(requestCode, resultCode, data);
		if (requestCode == 10 && resultCode == 10) {
			String dateinfo = data.getExtras()
			                .getString("dateinfo");
			editText\_main\_date.setText(dateinfo);
		}
		if (requestCode == 20 && resultCode == 20) {
			int imgid = data.getIntExtra("imgid",
			                R.drawable.ic_launcher);
			imageView\_main\_face.setImageResource(imgid);
		}
		
	}