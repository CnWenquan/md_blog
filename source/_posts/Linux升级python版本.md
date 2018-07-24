---
title: Linux升级Python版本
tags:
  - python
categories:
  - python
date: 2018-06-26 20:09:33
---

#### 说明
在ubuntu里，zlib叫zlib1g,相应的zlib-devel叫zlib1g.dev。默认的安装源里没有zlib1g.dev。要在packages.ubuntu.com上找。
`$sudo apt-get install ruby`
然后再装zlib1g-dev就可以了
`$sudo apt-get install zlib1g-dev`
#### 安装必备的软件包
**centos:**`yum -y groupinstall development zlib zlib-devel`
**ubuntu:**`sudo apt-get install zlib1g-dev`

## 以升级到Python3.6为例

### 下载Python3.6

```
wget https://www.python.org/ftp/python/3.6.3/Python-3.6.3.tgz
cd Python-3.6.3
./configure
make
make install
```
### 检查是否成功

```
which python3
python3 -V
```
### 创建软链接

```
cd /usr/bin
mv python python.backup
ln -s /usr/local/bin/python3 /usr/bin/python
```
### 检测

```
➜  md_blog python3.6
Python 3.6.3 (v3.6.3:2c5fed86e0, Oct  3 2017, 00:32:08) 
[GCC 4.2.1 (Apple Inc. build 5666) (dot 3)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> 
```
安装成功


