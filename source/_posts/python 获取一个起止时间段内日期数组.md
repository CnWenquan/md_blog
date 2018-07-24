---
title: python 获取一个起止时间段内日期数组
tags:
  - python
categories:
  - python
date: 2018-06-27 21:09:33
---


```
import datetime
   ...: 
   ...: start = datetime.datetime.strptime("21-06-2014", "%d-%m-%Y")
   ...: end = datetime.datetime.strptime("07-07-2014", "%d-%m-%Y")
   ...: date_generated = [start + datetime.timedelta(days=x) for x in range(0, (
   ...: end-start).days)]
   ...: 
   ...: for date in date_generated:
   ...:     print(date.strftime("%d-%m-%Y"))
```

**注意：**
    忌用pandas，pandas包太大，大材小用
    

