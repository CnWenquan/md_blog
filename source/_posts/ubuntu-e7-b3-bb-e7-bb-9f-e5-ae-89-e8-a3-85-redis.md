---
title: Ubuntu 系统安装 Redis
tags:
  - redis
url: 699.html
id: 699
categories:
  - 服务器
date: 2017-12-20 22:00:37
---

$sudo apt-get update
$sudo apt-get install redis-server

### 启动 Redis

$ redis-server

### 查看 redis 是否启动？

$ redis-cli

以上命令将打开以下终端：

redis 127.0.0.1:6379>

127.0.0.1 是本机 IP ，6379 是 redis 服务端口。现在我们输入 PING 命令。

redis 127.0.0.1:6379> ping
PONG

以上说明我们已经成功安装了redis。