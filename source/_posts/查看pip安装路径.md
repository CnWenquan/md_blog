---
title: 查看pip安装路径
tags:
  - python
categories:
  - python
date: 2018-06-26 19:09:33
---

对于安装的packages的话，例如json包的信息，可以使用：
`pip show json`
它会显示这个包的信息，包括安装地址：

```
➜  md_blog pip3 show pip
Name: pip
Version: 10.0.1
Summary: The PyPA recommended tool for installing Python packages.
Home-page: https://pip.pypa.io/
Author: The pip developers
Author-email: python-virtualenv@groups.google.com
License: MIT
Location: /Library/Frameworks/Python.framework/Versions/3.6/lib/python3.6/site-packages
Requires: 
Required-by: 
```

