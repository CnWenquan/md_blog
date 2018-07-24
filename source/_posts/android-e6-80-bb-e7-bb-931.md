---
title: Android 总结1
url: 85.html
id: 85
categories:
  - 未分类
date: 2016-07-02 17:08:57
tags:
---

1、**五种布局**：（全都继承与ViewGroup）

    FrameLayout、LinearLayout、AbsoluteLayout、RelativeLayout、Tablayout

2、**Activity生命周期**：

        onCreate()-->onStart()-->onResume()-->onPause()-->onStop()-->onDestroy()

    锁屏幕与解锁只会调用onPause(),而不会调用onStop()方法，开启屏幕后则调用onResume();

3、**通过Activity的XML标签改变任务栈的默认行为**

     四S：

       ①： standard：标准模式，每次启动Activity都会创建一个新的Activity实例，并且将其压入任务栈栈顶，而不管这个Activity是否已经存在。

        ②：singleTop：栈顶复用模式，这种模式下，如果新Activity已经位于任务栈的栈顶，那么此Activity不会被重新创建，所以它的启动就不会执行，但如果不在栈顶，作用同standard。

        ③：singleTask:栈内复用模式，创建这样的Activity的时候，系统会先确认它所需任务栈已经创建，否则先创建任务栈。然后放入Activity，如果栈中已经有一个Activity实例，那么这个Activity就会被调到栈顶，并且会把这个Activity之上的所有Activity清除掉。

        ④：singleInstance:一个Activity位于一个栈中，容不下其他的Activity。

**4、Fragment生命周期：**

    F:onAttach()->    F:onCreate()->    F:onCreateView()->    F:onViewCreated()->    A:onCreate()->    

    F:onActivityCreated()->    A:onStart()->    F:onStart()->    A:onResume()->    F:onResume()->

    F:onPause()->    A:onPause()->    F:onStop()->    A:onStop()->    F:onDestroyView()->    

    F:onDestroy()->    A:onDestroy()

**5、为什么在Service中创建子线程而不是Activity中？**

    这是因为Activity很难对Thread进行控制，当Activity被销毁之后，就没有任何其他的办法可以再重新获取到之前创建的子线程的实例。而且在一个Activity中创建的子线程，另一个Activity无法对其进行操作。但是Service就不同了，所有的Activity都可以与Service进行关联，然后可以很方便地操作其中的方法，即使Activity被销毁了，之后只要重新与Service建立关联，就又能够获取到原有的Service中Binder的实例。

6、Service的两种启动方式，有什么区别