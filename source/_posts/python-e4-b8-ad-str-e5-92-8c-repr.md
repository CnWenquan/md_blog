---
title: python中 __str__和__repr__
tags:
  - python
url: 670.html
id: 670
categories:
  - python
date: 2017-11-03 10:56:04
---

如果要把一个类的实例变成**str**，就需要实现特殊方法\_\_str\_\_()：

class Person(object):
    def \_\_init\_\_(self, name, gender):
        self.name = name
        self.gender = gender
    def \_\_str\_\_(self):
        return '(Person: %s, %s)' % (self.name, self.gender)

现在，在交互式命令行下用 **print **试试：

>>> p = Person('Bob', 'male')
>>> print p
(Person: Bob, male)

但是，如果直接敲变量**p**：

>>> p
<main.Person object at 0x10c941890>

似乎\_\_str\_\_() 不会被调用。

因为 Python 定义了**\_\_str\_\_()**和**\_\_repr\_\_()**两种方法，\_\_str\_\_()用于显示给用户，而**\_\_repr\_\_()**用于显示给开发人员。

有一个偷懒的定义\_\_repr\_\_的方法：

class Person(object):
    def \_\_init\_\_(self, name, gender):
        self.name = name
        self.gender = gender
    def \_\_str\_\_(self):
        return '(Person: %s, %s)' % (self.name, self.gender)
    \_\_repr\_\_ = \_\_str\_\_