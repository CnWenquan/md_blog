---
title: Day06
url: 44.html
id: 44
categories:
  - Android课程总结
  - 未分类
date: 2016-05-21 10:49:58
tags:
---

Android 06 总结  

----------------

**摘要**：ListView增加尾视图、Item多种布局的自定义适配器、Activity生命周期

**ListView增加尾视图（Loading）  
**

。。。。。。  

**Item多种布局的自定义适配器：**

自定义适配器中增加了getItemViewType()、getViewTypeCount()两个方法：

A:getItemViewType()

public int getItemViewType(int position) {
        int sex = Integer.parseInt(list.get(position).get("sex").toString());
        switch (sex) {
        case 0:
            return 0;
        default:
            return 1;
        }
    }

B:getViewTypeCount()

public int getViewTypeCount() {
        return 2;
    }

        C:根据获取到的type类型创建填充器与内部类

int type = getItemViewType(position);

    static class ViewHolder0 {
        private TextView textView\_item\_username;
        private TextView textView\_item\_number;
        private ImageView imageView\_item\_headpic;
        private Button button\_item\_link;
    }

    static class ViewHolder1 {
        private TextView textView\_item\_username;
        private TextView textView\_item\_number;
        private ImageView imageView\_item\_headpic;
    }

**Activity生命周期：**

1.  **onCreate()    创建**
    
2.  **onStart()    开始**
    
3.  **onResume()    活跃**
    
4.  **onPause()    暂停**
    
5.  **onStop()    停止**
    
6.  **onDestroy()    销毁**
    
7.  **onRestart()    重新开始**
    

强制横屏或竖屏：

**    android:screenOrientation="" \[portrait | landscape\]**

当横竖屏切换生命周期不发生改变的方法

    android:configChanges="orientation|screensize|keyboardHidden"

Activity启动模式：

1.  Standard:    标准启动模式，每次激活Activity时都会创建Activity，并放入栈内
    
2.  SingleTop:    如果在任务栈中正好有该Activity，且位于栈顶，则重用该Activity，不创建新的；如果任务栈中有该Activity，但不在栈顶，则重新在该栈中创建Activity
    
3.  SingleTask:    如果该栈中已经有该栈，则不创建新的Activity，并且把该栈上的任务弹出栈
    
4.  SingleInstance:    只能被实例化一次，且整个栈中只有一个