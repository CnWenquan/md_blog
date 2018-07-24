---
title: python中__slots__
tags:
  - python
url: 676.html
id: 676
categories:
  - python
date: 2017-11-03 13:34:53
---

由于Python是动态语言，任何实例在运行期都可以动态地添加属性。

如果要限制添加的属性，例如，**Student**类只允许添加 **name、gender**和**score **这3个属性，就可以利用Python的一个特殊的**\_\_slots\_\_**来实现。

顾名思义，**\_\_slots\_\_**是指一个类允许的属性列表：

class Student(object):
    \_\_slots\_\_ = ('name', 'gender', 'score')
    def \_\_init\_\_(self, name, gender, score):
        self.name = name
        self.gender = gender
        self.score = score

现在，对实例进行操作：

>>> s = Student('Bob', 'male', 59)
>>> s.name = 'Tim' # OK
>>> s.score = 99 # OK
>>> s.grade = 'A'
Traceback (most recent call last):
  ...
AttributeError: 'Student' object has no attribute 'grade'

**\_\_slots\_\_**的目的是限制当前类所能拥有的属性，如果不需要添加任意动态的属性，使用**\_\_slots\_\_**也能节省内存。