---
title: Python3 'dict' object has no attribute 'has_key'
tags:
  - python
url: 792.html
id: 792
categories:
  - python
date: 2018-04-17 11:18:15
---

has_key方法在python2中是可以使用的，在python3中删除了。

比如：

if dict.has_key(word):

改为：

if word in dict: