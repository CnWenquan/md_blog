---
title: Mac安装sshfs
date: 2018-06-08 11:46:31
tags: server
categories:
    - server
---

sshfs类似windows的samba，可以同步远程服务器的文件挂载到本地。

1. 首先mac上安装了[homebrew](https://link.jianshu.com/?t=http://brew.sh/index_zh-cn.html)

2. 安装sshfs的依赖fuse

`brew install Caskroom/cask/osxfuse`
3. 安装sshfs

`brew install sshfs`

### 使用
`sshfs -C -o reconnect user@hostname:remote_dir local_dir`

注：
```
user 远程连接用户名
hostname 远程连接的主机名
remote_dir 远程目录
local_dir 本地目录
```

### 相关问题

当mac休眠或重启后，发现挂载失败，进入目录会提示：

```
$ls local_dir 
ls: local_dir: Input/output error
```

这时候的操作应该是先杀死sshfs进程，然后取消挂载，然后再重新挂载！！！

```
$pkill -9 sshfs
$umount local_dir
```




