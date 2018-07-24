---
title: Android中视图的绘制流程
tags:
  - Android
url: 493.html
id: 493
categories:
  - Android
date: 2017-05-04 17:45:56
---

好久没看Android啦，今天看到一篇不错的博客，就想分享给大家！  

  

一、前言
====

    Android中Activity是作为应用程序的载体存在的，它代表着一个完整的用户界面，提供了一个窗口来绘制各种视图，当Activity启动时，我们会通过setContentView方法来设置一个内容视图，这个内容视图就是用户看到的界面，在Android中View存在的两种形式：一种是单一的View控件 ，另一种就是可以包含其他View的ViewGroup容器，前面的内容视图就是以ViewGroup的形式存在的，先看一个Android的UI管理系统的层级管理图：

    ![](/ueditor/php/upload/image/20170504/1493891123511890.png)

    PhoneWindow是Android系统中最基本的窗口系统，每个Activity都会创建一个。PhoneWindow是Activity和View系统交互的接口。DecorView本质上是一个FrameLayout，是Activity中所有View的祖先。

二、绘制的整体流程
=========

    当应用启动时会启动一个主Activity，Android系统会根据Activity的布局来对它进行绘制。绘制会从根视图ViewRoot的performTraversals()方法开始，从上到下遍历整个视图树，每个View控件负责绘制自己，而VewGroup还需要负责通知自己的子View进行绘制操作，视图绘制的过程可以分为三个步骤，分别是测量（Measure）、布局（layout）和绘制（Draw），以下我们看一下performTraversals()的关键代码，performTraversals()在ViewRootImpl类中，ViewRootImpl类在\\sdk\\sources\\android-23\\android\\view包下面，是一个隐藏类，在as中看不到 直接启动连击两次Shift键搜索，代码：           

private void performTraversals() {
                    ...                    int childWidthMeasureSpec = getRootMeasureSpec(mWidth, lp.width);                    int childHeightMeasureSpec = getRootMeasureSpec(mHeight, lp.height);                   //执行测量流程
                    performMeasure(childWidthMeasureSpec, childHeightMeasureSpec);
                    ...                    //执行布局流程
                    performLayout(lp, desiredWindowWidth, desiredWindowHeight);
                    ...      
                    //执行绘制流程                    performDraw();
            
    }

三、MeasureSpec
=============

    MeasureSpec表示的是一个32位的整型值，它的高2位表示测量模式SpecMode，低30位表示某种测量模式下的规则大小SpecSize。MeasureSpec是View类的一个静态内部类，用来说明应该如何测量这个View，代码如下：

public static class MeasureSpec {    private static final int MODE\_SHIFT = 30;    private static final int MODE\_MASK  = 0x3 << MODE_SHIFT;    // 不指定测量模式
    public static final int UNSPECIFIED = 0 << MODE_SHIFT;    // 精确测量模式
    public static final int EXACTLY     = 1 << MODE_SHIFT;    //最大值测量模式
    public static final int AT\_MOST     = 2 << MODE\_SHIFT;    //根据指定的大小和模式创建一个MeasureSpec
    public static int makeMeasureSpec(int size, int mode) {        if (sUseBrokenMakeMeasureSpec) {            return size + mode;
        } else {            return (size & ~MODE\_MASK) | (mode & MODE\_MASK);
        }
    }

     
 
    //微调某个MeasureSpec的大小
    static int adjust(int measureSpec, int delta) {        final int mode = getMode(measureSpec);        int size = getSize(measureSpec);        if (mode == UNSPECIFIED) {            // No need to adjust size for UNSPECIFIED mode.
            return makeMeasureSpec(size, UNSPECIFIED);
        }        size += delta;        if (size < 0) {
            Log.e(VIEW\_LOG\_TAG, "MeasureSpec.adjust: new size would be negative! (" + size +") spec: " + toString(measureSpec) + " delta: " + delta);            size = 0;
        }        return makeMeasureSpec(size, mode);
    }
}

我们需要关注的三种测量模式，在后面的Measure阶段会用到

*   UNSPECIFIED：不指定测量模式，父视图没有限制子视图的大小，子视图可以是想要的任何尺寸，通常用于系统内部，应用开发中很少使用到。
    
*   EXACTLY：精确测量模式，当该视图的layout\_width或者layout\_height指定为具体数值或者match_parent时生效，表示父视图已经决定了子视图的精确大小，这种模式下View的测量值就是SpecSize值
    
*   AT\_MOST：最大值模式，当该视图的layout\_width或者parent\_height指定为wrap\_content时生效，此时子视图的尺寸可以是不超过父视图允许的最大尺寸的任何尺寸。
    

    对DecorView而言，它的MeasureSpec由窗口尺寸和其自身的LayoutParams共同决定；对于普通的View，它的MeasureSpec由父视图的MeasureSpec由父视图的MeasureSpec和其自身的LayoutParams共同决定。

四、Measure
=========

    Measure操作用来计算View的实际大小，由前面的分析可知，页面的测量流程是从performMeasure方法开始的，核心代码：

private void performMeasure(int childWidthMeasureSpec, int childHeightMeasureSpec) {
    Trace.traceBegin(Trace.TRACE\_TAG\_VIEW, "measure");    try {
        mView.measure(childWidthMeasureSpec, childHeightMeasureSpec);
    } finally {
        Trace.traceEnd(Trace.TRACE\_TAG\_VIEW);
    }
}

    具体的测量操作是分发给ViewGroup的，由ViewGroup在它的measureChild方法中传递给子View。ViewGroup通过遍历自身所有的子View，并逐个调用子View的measure方法实现测量操作。

//遍历测量ViewGroup中所有的Viewprotected void measureChildren(int widthMeasureSpec, int heightMeasureSpec) {    final int size = mChildrenCount;    final View\[\] children = mChildren;    for (int i = 0; i < size; ++i) {        final View child = children\[i\];
        //当View的可见性处于GONE状态时，不对其进行测量
        if ((child.mViewFlags & VISIBILITY_MASK) != GONE) {
            measureChild(child, widthMeasureSpec, heightMeasureSpec);
        }
    }
}

//测量某个指定的Viewprotected void measureChild(View child, int parentWidthMeasureSpec,        int parentHeightMeasureSpec) {    final LayoutParams lp = child.getLayoutParams();
    //根据父容器的MeasureSpec和子View的LayoutParams等信息计算子View的MeasureSpec
    final int childWidthMeasureSpec = getChildMeasureSpec(parentWidthMeasureSpec,
            mPaddingLeft + mPaddingRight, lp.width);    final int childHeightMeasureSpec = getChildMeasureSpec(parentHeightMeasureSpec,
            mPaddingTop + mPaddingBottom, lp.height);

    child.measure(childWidthMeasureSpec, childHeightMeasureSpec);
}

    再看看View（ViewGroup）的measure方法，最终的测量是通过回调onMeasure方法实现的，这个通常由View的特定子类自己实现，开发者也可以通过重写这个方法实现自定义View。

public final void measure(int widthMeasureSpec, int heightMeasureSpec) {
        ...
        onMeasure(widthMeasureSpec, heightMeasureSpec);
        ...  
}

//如果需要自定义测量过程，则子类可以重写这个方法protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
    //setMeasuredDimension方法用于设置View的测量宽高    getDefaultSize(getSuggestedMinimumHeight(),getSuggestedMinimumWidth());
}

//如果View没有重写onMeasure方法，则默认会直接调用getDefaultSize，来获得View的宽高public static int getDefaultSize(int size, int measureSpec) {    int result = size;    int specMode = MeasureSpec.getMode(measureSpec);    int specSize = MeasureSpec.getSize(measureSpec);    switch (specMode) {    case MeasureSpec.UNSPECIFIED:
        result = size;        break;    case MeasureSpec.AT_MOST:
    case MeasureSpec.EXACTLY:
        result = specSize;        break;
    }    return result;
}

五、layout
========

    Layout过程用来确定View在父容器中的布局位置，它是由父容器获取子View的位置参数后，调用子View的layout方法并将位置参数传入实现的，ViewRootImpl的performLayout代码：

private void performLayout(WindowManager.LayoutParams lp, int desiredWindowWidth,int desiredWindowHeight) {
        ...
        host.layout(0, 0, host.getMeasuredWidth(), host.getMeasuredHeight());
        ...          
}

public final void layout(int l, int t, int r, int b) {    
     super.layout(l, t, r, b);
    
}

//空方法，子类如果是ViewGroup类型，则重写这个方法，实现ViewGroup中所有View控件布局流程protected void onLayout(boolean changed, int l, int t, int r, int b) {
    
}

六、Draw
======

    Draw操作用来将控件绘制出来，绘制的流程从performDraw方法开始，核心代码：

private void performDraw() {
    draw(fullRedrawNeeded);    
    ...
}

private void draw(boolean fullRedrawNeeded) {
     ...
     if (!drawSoftware(surface, mAttachInfo, xOffset, yOffset, scalingRequired, dirty)) {
            return;
      }
      ...
}

private boolean drawSoftware(Surface surface, AttachInfo attachInfo, int xoff, int yoff,        boolean scalingRequired, Rect dirty) {

        mView.draw(canvas);

             
}

       可以看到最终调用到每个View的draw方法绘制每个具体的View，绘制基本上可以分为六个步骤，代码如下：

protected void onDraw(Canvas canvas) {    //第一：  绘制View的背景
    drawBackground(canvas);
    ...    //第二：如果需要的话，保存canvas的图层，为fading做准备
    int saveCount = canvas.getSaveCount();
    ...
    canvas.saveLayer(left, top, right, top + length, null, flags);
    ...    //第三：绘制View的内容
    onDraw(canvas);
    ...    //第四：绘制View的子View
    dispatchDraw(canvas);

    ...    //第五：如果需要的话，绘制View的fading边缘并恢复图层
    ...
    canvas.restoreToCount(saveCount);    //第六：绘制View的装饰
    onDrawScrollBars(canvas);

}