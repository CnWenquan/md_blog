---
title: uwsgi -- unavailable modifier requested
tags:
  - python
categories:
  - python
date: 2018-06-26 20:09:33
---

**centos:**`yum install uwsgi-plugin-python3`
**ubuntu:**`apt-get install uwsgi-plugin-python3`
同时启动文件需要增加一个配置项
`--plugins=python3`
测试直接在ini配置文件中无效，只能在命令行里加入。


