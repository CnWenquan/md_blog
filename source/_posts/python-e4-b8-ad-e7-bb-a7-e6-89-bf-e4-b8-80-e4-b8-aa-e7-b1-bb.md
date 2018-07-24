---
title: python中继承一个类
tags:
  - python
url: 664.html
id: 664
categories:
  - python
date: 2017-11-02 18:42:13
---

如果已经定义了**Person**类，需要定义新的**Student**和**Teacher**类时，可以直接从Person类继承：

class Person(object):
    def \_\_init\_\_(self, name, gender):
        self.name = name
        self.gender = gender

定义**Student**类时，只需要把额外的属性加上，例如score：

class Student(Person):
    def \_\_init\_\_(self, name, gender, score):        super(Student, self).\_\_init\_\_(name, gender)
        self.score = score

一定要用 super(Student, self).\_\_init\_\_(name, gender) 去初始化父类，否则，继承自**Person**的**Student**将没有**name**和**gender**。

函数**super(Student, self)**将返回当前类继承的父类，即** Person **，然后调用**\_\_init\_\_()**方法，注意self参数已在super()中传入，在\_\_init\_\_()中将隐式传递，不需要写出（也不能写）。