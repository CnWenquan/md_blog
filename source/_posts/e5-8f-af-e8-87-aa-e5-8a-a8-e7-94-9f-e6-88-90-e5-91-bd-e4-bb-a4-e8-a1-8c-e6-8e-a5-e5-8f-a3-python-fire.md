---
title: 可自动生成命令行接口 Python Fire
tags:
  - python
url: 774.html
id: 774
categories:
  - python
date: 2018-03-09 17:48:53
---

Python Fire 是 Google 开源的一个可从任何 Python 代码自动生成命令行接口（CLI）的库。

  

简易示例：

import fire


class Calculator(object):
    """A simple calculator class."""

    def double(self, number):
        return 2 * number


if \_\_name\_\_ == '\_\_main\_\_':
    fire.Fire(Calculator)

  

然后，你可以运行：

![](blob:http://blog.wenquanpage.com/fa26edb4-256a-4091-a16b-5f64827631b2)