---
title: python中动态导入模块
tags:
  - python
url: 651.html
id: 651
categories:
  - python
date: 2017-11-02 16:47:34
---

如果导入的模块不存在，Python解释器会报 ImportError 错误：

>>> import something
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ImportError: No module named something

有的时候，两个不同的模块提供了相同的功能，比如 **StringIO **和 **cStringIO **都提供了StringIO这个功能。

这是因为Python是动态语言，解释执行，因此Python代码运行速度慢。

如果要提高Python代码的运行速度，最简单的方法是把某些关键函数用C语言重写，这样就能大大提高执行速度。

同样的功能，StringIO 是纯Python代码编写的，而 cStringIO 部分函数是C写的，因此 cStringIO 运行速度更快。

利用ImportError错误，我们经常在Python中动态导入模块：

try:
    from cStringIO import StringIO
except ImportError:
    from StringIO import StringIO

上述代码先尝试从cStringIO导入，如果失败了（比如cStringIO没有被安装），再尝试从StringIO导入。这样，如果cStringIO模块存在，则我们将获得更快的运行速度，如果cStringIO不存在，则顶多代码运行速度会变慢，但不会影响代码的正常执行。

**try **的作用是捕获错误，并在捕获到指定错误时执行 **except **语句。