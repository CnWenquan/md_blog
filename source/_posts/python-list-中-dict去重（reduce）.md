---
title: python list 中 dict去重（reduce）
date: 2018-06-08 11:46:31
tags: python
categories:
  - python
---

### 需求是list中套dict，用set直接去重报unhashable，因为dict是不确定的，不可被hash ***

### reduce处理：

```python
def reduce(function, iterable, initializer=None):
    it = iter(iterable)
    if initializer is None:
        value = next(it)
    else:
        value = initializer
    for element in it:
        value = function(value, element)
    return value
```

### 使用上面的方法定义一个function，传入iterable就可以循环比较iterable数据了

```python
url_list = [
        {'path': ['jy5', 'xhr', 'compose', 'init.do'], 'host': 'cwebmail.mail.163.com', 'method': 'POST', 'query': ['cType', 'sid']},
        {'path': ['jy5', 'xhr', 'user', 'refresh.do'], 'host': 'cwebmail.mail.163.com', 'method': 'POST', 'query': ['sid']},
        {'path': ['jy5', 'xhr', 'compose', 'compose.do'], 'host': 'cwebmail.mail.163.com', 'method': 'POST', 'query': ['action', 'sid']},
        {'path': ['jy5', 'data', 'analytics.s'], 'host': 'cwebmail.mail.163.com', 'method': 'GET', 'query': ['product', 'uid', 'host', 'fun', 'data', '_']},
        {'path': ['jy5', 'swf', 'upload2.swf'], 'host': 'cwebmail.mail.163.com', 'method': 'GET', 'query': []},
        {'path': ['jy5', 'data', 'analytics.s'], 'host': 'cwebmail.mail.163.com', 'method': 'GET', 'query': ['product', 'uid', 'host', 'fun', 'data', '_']},
        {'path': ['jy5', 'xhr', 'user', 'refresh.do'], 'host': 'cwebmail.mail.163.com', 'method': 'POST', 'query': ['sid']},
        {'path': ['jy4-app', 'xhr', 'dropbox', 'account', 'check.do'], 'host': 'jy4-app.mail.163.com', 'method': 'POST', 'query': ['utoken', 'sid']}
]
```

### 转换方法：

```python
f = lambda x,y:x if y in x else x + [y]
url_list = reduce(f, [[], ] + url_list)
```

### 处理结果：

```python
url_list = [
        {'path': ['jy5', 'xhr', 'compose', 'init.do'], 'host': 'cwebmail.mail.163.com', 'method': 'POST', 'query': ['cType', 'sid']},
        {'path': ['jy5', 'xhr', 'user', 'refresh.do'], 'host': 'cwebmail.mail.163.com', 'method': 'POST', 'query': ['sid']},
        {'path': ['jy5', 'xhr', 'compose', 'compose.do'], 'host': 'cwebmail.mail.163.com', 'method': 'POST', 'query': ['action', 'sid']},
        {'path': ['jy5', 'data', 'analytics.s'], 'host': 'cwebmail.mail.163.com', 'method': 'GET', 'query': ['product', 'uid', 'host', 'fun', 'data', '_']},
        {'path': ['jy5', 'swf', 'upload2.swf'], 'host': 'cwebmail.mail.163.com', 'method': 'GET', 'query': []},
        {'path': ['jy4-app', 'xhr', 'dropbox', 'account', 'check.do'], 'host': 'jy4-app.mail.163.com', 'method': 'POST', 'query': ['utoken', 'sid']}
]   

```