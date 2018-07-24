---
title: python中编写无参数decorator（装饰类）
tags:
  - python
url: 642.html
id: 642
categories:
  - python
date: 2017-11-02 11:28:06
---

Python的 decorator 本质上就是一个高阶函数，它接收一个函数作为参数，然后，返回一个新函数。

使用 decorator 用Python提供的@语法，这样可以避免手动编写f = decorate(f) 这样的代码。

**考察一个@log的定义：**

def log(f):
    def fn(x):
        print 'call ' + f.\_\_name\_\_ + '()...'
        return f(x)
    return fn

对于阶乘函数，@log工作得很好：

@log
def factorial(n):
    return reduce(lambda x,y: x*y, range(1, n+1))
print factorial(10)

**结果：**

call factorial()...
3628800

但是，对于参数不是一个的函数，调用将报错：

@log
def add(x, y):
    return x + y
print add(1, 2)

**结果：**

Traceback (most recent call last):
  File "test.py", line 15, in <module>
    print add(1,2)
TypeError: fn() takes exactly 1 argument (2 given)

因为 add() 函数需要传入两个参数，但是 @log 写死了只含一个参数的返回函数。

要让 @log 自适应任何参数定义的函数，可以利用Python的 *args 和 **kw，保证任意个数的参数总是能正常调用：

def log(f):
    def fn(\*args, \*\*kw):
        print 'call ' + f.\_\_name\_\_ + '()...'
        return f(\*args, \*\*kw)
    return fn

现在，对于任意函数，@log都能正常工作。

  

def log(f):
    def fn(x):
        print 'call ' + f.\_\_name\_\_ + '()...'
        return f(x)
    return fn

发现对于被装饰的函数，log打印的语句是不能变的（除了函数名）。

如果有的函数非常重要，希望打印出'\[INFO\] call xxx()...'，有的函数不太重要，希望打印出'\[DEBUG\] call xxx()...'，这时，log函数本身就需要传入'INFO'或'DEBUG'这样的参数，类似这样：

@log('DEBUG')
def my_func():
    pass

把上面的定义翻译成高阶函数的调用，就是：

my\_func = log('DEBUG')(my\_func)

上面的语句看上去还是比较绕，再展开一下：

log_decorator = log('DEBUG')
my\_func = log\_decorator(my_func)

上面的语句又相当于：

log_decorator = log('DEBUG')
@log_decorator
def my_func():
    pass

所以，带参数的log函数首先返回一个decorator函数，再让这个decorator函数接收my_func并返回新函数：

def log(prefix):
    def log_decorator(f):
        def wrapper(\*args, \*\*kw):
            print '\[%s\] %s()...' % (prefix, f.\_\_name\_\_)
            return f(\*args, \*\*kw)
        return wrapper
    return log_decorator

@log('DEBUG')
def test():
    pass
print test()

**执行结果：**

\[DEBUG\] test()...
None

对于这种3层嵌套的decorator定义，你可以先把它拆开：

# 标准decorator:def log_decorator(f):
    def wrapper(\*args, \*\*kw):
        print '\[%s\] %s()...' % (prefix, f.\_\_name\_\_)
        return f(\*args, \*\*kw)
    return wrapper
return log_decorator# 返回decorator:def log(prefix):
    return log_decorator(f)

拆开以后会发现，调用会失败，因为在3层嵌套的decorator定义中，最内层的wrapper引用了最外层的参数prefix，所以，把一个闭包拆成普通的函数调用会比较困难。不支持闭包的编程语言要实现同样的功能就需要更多的代码。

@decorator可以动态实现函数功能的增加，但是，经过@decorator“改造”后的函数，和原函数相比，除了功能多一点外，有没有其它不同的地方？

在没有decorator的情况下，打印函数名：

def f1(x):
    pass
print f1.\_\_name\_\_

**输出： **f1

有decorator的情况下，再打印函数名：

def log(f):
    def wrapper(\*args, \*\*kw):
        print 'call...'
        return f(\*args, \*\*kw)
    return wrapper
@log
def f2(x):
    pass
print f2.\_\_name\_\_

**输出：** wrapper

可见，由于decorator返回的新函数函数名已经不是'f2'，而是@log内部定义的'wrapper'。这对于那些依赖函数名的代码就会失效。decorator还改变了函数的\_\_doc\_\_等其它属性。如果要让调用者看不出一个函数经过了@decorator的“改造”，就需要把原函数的一些属性复制到新函数中：

def log(f):
    def wrapper(\*args, \*\*kw):
        print 'call...'
        return f(\*args, \*\*kw)
    wrapper.\_\_name\_\_ = f.\_\_name\_\_
    wrapper.\_\_doc\_\_ = f.\_\_doc\_\_
    return wrapper

这样写decorator很不方便，因为我们也很难把原函数的所有必要属性都一个一个复制到新函数上，所以Python内置的functools可以用来自动化完成这个“复制”的任务：

import functools
def log(f):
    @functools.wraps(f)
    def wrapper(\*args, \*\*kw):
        print 'call...'
        return f(\*args, \*\*kw)
    return wrapper

最后需要指出，由于我们把原函数签名改成了(\*args, \*\*kw)，因此，无法获得原函数的原始参数信息。即便我们采用固定参数来装饰只有一个参数的函数：

def log(f):
    @functools.wraps(f)
    def wrapper(x):
        print 'call...'
        return f(x)
    return wrapper

也可能改变原函数的参数名，因为新函数的参数名始终是 'x'，原函数定义的参数名不一定叫 'x'。