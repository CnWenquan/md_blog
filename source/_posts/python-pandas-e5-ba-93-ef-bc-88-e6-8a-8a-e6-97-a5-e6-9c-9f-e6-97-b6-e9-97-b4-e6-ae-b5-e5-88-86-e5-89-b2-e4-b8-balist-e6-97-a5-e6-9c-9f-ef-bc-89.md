---
title: python pandas库（把日期时间段分割为list日期）
tags:
  - python
url: 857.html
id: 857
categories:
  - python
date: 2018-05-31 14:50:29
---

import pandas as pd from datetime 
import datetime 
def datelist(beginDate, endDate): 
  # beginDate, endDate是形如‘20160601’的字符串或datetime格式 
  date\_l=\[datetime.strftime(x,'%Y-%m-%d') for x in list(pd.date\_range(start=beginDate, end=endDate))\] 
return date_l