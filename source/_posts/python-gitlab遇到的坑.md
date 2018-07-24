---
title: Python-gitlab遇到的坑
tags:
  - python
categories:
  - python
date: 2018-06-26 21:09:33
---

### 说明
在做App版本控制的时候，计划采用gitlab控制版本迭代。流程大概是：
在gitlab上创建android/ios仓库，master放源码，version放版本信息，新增apk包后version分支添加tag，tag为最新版本号，并且在项目上创建webHook钩子，自动调用远程接口，接口中监听最新的tag即版本号。
#### 安装python-gitlab
python-gitlab is compatible with Python 2.7 and 3.4+.
Use pip to install the latest stable version of python-gitlab:

```
$ sudo pip install --upgrade python-gitlab
```

git直接安装方式：

```
$ git clone https://github.com/python-gitlab/python-gitlab
$ cd python-gitlab
$ sudo python setup.py install
```
#### 开始使用：

```
import gitlab

# private token or personal token authentication
gl = gitlab.Gitlab('http://10.0.0.1', private_token='JVNSESs8EwWRx5yDxM5q')

# oauth token authentication
gl = gitlab.Gitlab('http://10.0.0.1', oauth_token='my_long_token_here')

# username/password authentication (for GitLab << 10.2)
gl = gitlab.Gitlab('http://10.0.0.1', email='jdoe', password='s3cr3t')

# anonymous gitlab instance, read-only for public resources
gl = gitlab.Gitlab('http://10.0.0.1')

# make an API request to create the gl.user object. This is mandatory if you
# use the username/password authentication.
gl.auth()
```
多种方式初始化gitlab对象，建议使用token方式
#### Demo：

```
# list all the projects
projects = gl.projects.list()
for project in projects:
    print(project)

# get the group with id == 2
group = gl.groups.get(2)
for group in groups:
    print()

# create a new user
user_data = {'email': 'jen@foo.com', 'username': 'jen', 'name': 'Jen'}
user = gl.users.create(user_data)
print(user)
```

#### 问题：
从上面给的Demo跟初始化gitlab对象都挺方便的，问题出在哪儿呢？我遇到的坑是，V3-》V4版本过度期，本来V3根V4有一个过渡使用的过程，第一次测试使用V3正常返回，但过了个周末V3完全drop掉了，测试报错，返回一个二进制的html页面源码，解开看是gitlab登录界面，查看本地pip安装的gitlab扩展源码与新装gitlab对比，发现新装gitlab扩展缺少V3模块，于是做了个替换，测试成功。。。暂时先用着旧版的！！！


