---
title: python之导入模块
tags:
  - python
url: 649.html
id: 649
categories:
  - python
date: 2017-11-02 16:40:05
---

要使用一个模块，我们必须首先导入该模块。Python使用import语句导入一个模块。例如，导入系统自带的模块 math：

import math

你可以认为math就是一个指向已导入模块的变量，通过该变量，我们可以访问math模块中所定义的所有公开的函数、变量和类：

>>> math.pow(2, 0.5) # pow是函数
1.4142135623730951

>>> math.pi # pi是变量
3.141592653589793

如果我们只希望导入用到的math模块的某几个函数，而不是所有函数，可以用下面的语句：

from math import pow, sin, log

这样，可以直接引用 pow, sin, log 这3个函数，但math的其他函数没有导入进来：

>>> pow(2, 10)
1024.0
>>> sin(3.14)
0.0015926529164868282

如果遇到名字冲突怎么办？比如math模块有一个log函数，logging模块也有一个log函数，如果同时使用，如何解决名字冲突？

如果使用import导入模块名，由于必须通过模块名引用函数名，因此不存在冲突：

import math, logging
print math.log(10)   # 调用的是math的log函数logging.log(10, 'something')   # 调用的是logging的log函数

如果使用from...import导入log函数，势必引起冲突。这时，可以给函数起个“别名”来避免冲突：

from math import log
from logging import log as logger   # logging的log现在变成了loggerprint log(10)   # 调用的是math的loglogger(10, 'import from logging')   # 调用的是logging的log