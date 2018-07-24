---
title: python中方法也是属性
tags:
  - python
url: 660.html
id: 660
categories:
  - 未分类
date: 2017-11-02 18:25:32
---

我们在**class**中定义的实例方法其实也是属性，它实际上是一个函数对象：

class Person(object):
    def \_\_init\_\_(self, name, score):
        self.name = name
        self.score = score
    def get_grade(self):
        return 'A'

p1 = Person('Bob', 90)
print p1.get\_grade# => <bound method Person.get\_grade of <\_\_main\_\_.Person object at 0x109e58510>>print p1.get_grade()# => A

也就是说，**p1.get_grade **返回的是一个函数对象，但这个函数是一个绑定到实例的函数，**p1.get_grade() **才是方法调用。

因为方法也是一个属性，所以，它也可以动态地添加到实例上，只是需要用 types.MethodType() 把一个函数变为一个方法：

import types
def fn\_get\_grade(self):
    if self.score >= 80:
        return 'A'
    if self.score >= 60:
        return 'B'
    return 'C'

class Person(object):
    def \_\_init\_\_(self, name, score):
        self.name = name
        self.score = score

p1 = Person('Bob', 90)p1.get\_grade = types.MethodType(fn\_get\_grade, p1, Person)print p1.get\_grade()# => Ap2 = Person('Alice', 65)
print p2.get\_grade()# ERROR: AttributeError: 'Person' object has no attribute 'get\_grade'
# 因为p2实例并没有绑定get_grade

给一个实例动态添加方法并不常见，直接在class中定义要更直观。