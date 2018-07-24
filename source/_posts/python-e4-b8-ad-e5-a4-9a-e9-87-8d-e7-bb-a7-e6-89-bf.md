---
title: python中多重继承
tags:
  - python
url: 666.html
id: 666
categories:
  - python
date: 2017-11-02 19:09:33
---

除了从一个父类继承外，Python允许从多个父类继承，称为多重继承。

多重继承的继承链就不是一棵树了，它像这样：

class A(object):
    def \_\_init\_\_(self, a):
        print 'init A...'
        self.a = a

class B(A):
    def \_\_init\_\_(self, a):
        super(B, self).\_\_init\_\_(a)
        print 'init B...'

class C(A):
    def \_\_init\_\_(self, a):
        super(C, self).\_\_init\_\_(a)
        print 'init C...'

class D(B, C):
    def \_\_init\_\_(self, a):
        super(D, self).\_\_init\_\_(a)
        print 'init D...'

看下图:

[![](/ueditor/php/upload/image/20171102/1509620926368433.jpg)](http://img.mukewang.com/54daf037000142d207580552.jpg)

像这样，**D **同时继承自**B**和**C**，也就是**D**拥有了**A、B、C**的全部功能。多重继承通过**super()**调用**\_\_init\_\_()**方法时，**A**虽然被继承了两次，但**\_\_init\_\_()**只调用一次：

>>> d = D('d')
init A...
init C...
init B...
init D...

**多重继承的目的**是从两种继承树中分别选择并继承出子类，以便组合功能使用。

举个例子，Python的网络服务器有**TCPServer、UDPServer、UnixStreamServer、UnixDatagramServer**，而服务器运行模式有 **多进程ForkingMixin **和 **多线程ThreadingMixin**两种。

要创建多进程模式的**TCPServer**：

class MyTCPServer(TCPServer, ForkingMixin)
    pass

要创建多线程模式的**UDPServer**：

class MyUDPServer(UDPServer, ThreadingMixin):
    pass

如果没有多重继承，要实现上述所有可能的组合需要 4x2=8 个子类。