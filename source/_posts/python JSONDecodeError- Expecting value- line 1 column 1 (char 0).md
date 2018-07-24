---
title: python JSONDecodeError- Expecting value- line 1 column 1 (char 0)
tags:
  - python
categories:
  - python
date: 2018-06-25 19:09:33
---

### 问题描述
今天在解析字符串中，使用json.loads解码字符串，脚本如下：

```
import json
string = "{u'lat': 61.190495, u'lng': -149.86884}"
dic = json.loads(string)
```

运行后报错

```
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/Library/Frameworks/Python.framework/Versions/3.6/lib/python3.6/json/__init__.py", line 354, in loads
    return _default_decoder.decode(s)
  File "/Library/Frameworks/Python.framework/Versions/3.6/lib/python3.6/json/decoder.py", line 339, in decode
    obj, end = self.raw_decode(s, idx=_w(s, 0).end())
  File "/Library/Frameworks/Python.framework/Versions/3.6/lib/python3.6/json/decoder.py", line 355, in raw_decode
    obj, end = self.scan_once(s, idx)
json.decoder.JSONDecodeError: Expecting property name enclosed in double quotes: line 1 column 2 (char 1)

```
### 解决方案：

#### 替换单引号：
类型的错误，就是由于JSON中，标准语法中，不支持单引号，属性或者属性值，都必须是双引号括起来的。

```
string = string.replace("'", '"')
dic = json.loads(string)
```
#### 替换引号前的u为空：
即去除引号前的u。这种解决编码问题，简单粗暴了点，至于其深入分析，未完待绪。同时也希望各位大神能够提供更好的解决办法。

```
string = string.replace("u", "") ＃ 这里比较简单，实际中需要用正则条件替换
dic = json.loads(string)
```




