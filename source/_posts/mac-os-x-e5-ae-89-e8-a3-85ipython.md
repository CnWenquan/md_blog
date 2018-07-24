---
title: Mac OS X安装ipython
tags:
  - python
url: 855.html
id: 855
categories:
  - python
date: 2018-05-31 14:21:53
---

#第一步：安装ipython
pip3 install ipython
#第二步：找到python3安装路径
> which python3
/usr/local/bin/python3
> ls -l /usr/local/bin/python3
lrwxr-xr-x  1 root  wheel  69 10 29 23:02 /usr/local/bin/python3 -> /Library/Frameworks/Python.framework/Versions/3.5/bin/python3
#第三步：设置环境变量，编辑.zshrc文件
添加这一行（把加黑部分换成你的Python目录）
export PATH=/Library/Frameworks/Python.framework/Versions/3.5/bin:$PATH
#第四步：使配置文件生效
> source ~/.zshrc