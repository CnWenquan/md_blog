---
title: python中 __cmp__
tags:
  - python
url: 672.html
id: 672
categories:
  - python
date: 2017-11-03 11:18:48
---

对 **int**、**str **等内置数据类型排序时，Python的 **sorted() **按照默认的比较函数 **cmp **排序，但是，如果对一组 **Student **类的实例排序时，就必须提供我们自己的特殊方法\_\_cmp\_\_()：

class Student(object):
    def \_\_init\_\_(self, name, score):
        self.name = name
        self.score = score
    def \_\_str\_\_(self):
        return '(%s: %s)' % (self.name, self.score)
    \_\_repr\_\_ = \_\_str\_\_

    def \_\_cmp\_\_(self, s):
        if self.name < s.name:
            return -1
        elif self.name > s.name:
            return 1
        else:
            return 0

上述 Student 类实现了\_\_cmp\_\_()方法，**\_\_cmp\_\_**用实例自身**self**和传入的实例 **s **进行比较，如果**self**应该排在前面，就返回 -1，如果**s**应该排在前面，就返回1，如果两者相当，返回 0。

Student类实现了按name进行排序：

>>> L = \[Student('Tim', 99), Student('Bob', 88), Student('Alice', 77)\]
>>> print sorted(L)
\[(Alice: 77), (Bob: 88), (Tim: 99)\]

**注意**: 如果list不仅仅包含 Student 类，则 \_\_cmp\_\_ 可能会报错：

L = \[Student('Tim', 99), Student('Bob', 88), 100, 'Hello'\]
print sorted(L)