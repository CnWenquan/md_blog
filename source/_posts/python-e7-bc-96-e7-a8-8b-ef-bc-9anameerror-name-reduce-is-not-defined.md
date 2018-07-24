---
title: 'Python编程：NameError: name ''reduce'' is not defined'
tags:
  - python
url: 783.html
id: 783
categories:
  - python
date: 2018-03-27 11:04:09
---

使用 reduce() 测试的时候报错：reduce 未定义！

解决
--

> 引用stackoverflow的回答：   
> \- 你使用的是python3   
> \- 参考的是python2的指南

from functools import reduce  # py3print(reduce(lambda x, y: x + y, \[ 1, 2, 3\]))"""Output：
6
"""

reduce函数在python3的内建函数移除了，放入了functools模块