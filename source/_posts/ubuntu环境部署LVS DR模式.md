---
title: ubuntu环境部署LVS DR模式
tags:
  - server
categories:
  - server
date: 2018-07-05 21:09:33
---
### 环境说明：
系统环境：ubuntu16.4
lvs服务器：172.18.10.246/172.18.10.245/172.18.10.244
真实服务器：172.18.10.246/172.18.10.245/172.18.10.244
VIP：172.18.10.246
部署目的：访问8080端口，可以将请求报文转发至172.18.10.245/172.18.10.244负载
### 安装LVS：
#### 更新ubuntu应用包：

```
apt-get update
apt-get upgrade
```
#### 安装LVS

```
#安装
apt-get -y install ipvsadm 
#查看版本
ipvsadm -v 
#查看是否已加载进内核模块中
lsmod |grep ip_vs 
#查看lvs规则
ipvsadm -L -n 
#查看链接情况
ipvsadm -L -c 
```

#### 配置维护LVS VIP服务
**说明：-A是添加一条虚拟服务器记录，即VIP。此处配置要选择算法**
##### 配置

```
#代理转发所有去往该IP TCP的报文，-s指定算法wrr为加权轮询算法，-p会话保持时间（默认值是300s）
ipvsadm -A -t 172.18.10.246:8080  -s wrr -p 300 
```
##### 维护
使用-E修改VIP设置
例：`ipvsadm -E -t 172.18.10.246:8080  -s wrr -p 40` #可修改会话保持时间与算法。

使用-D删除VIP设置
例：`ipvsadm -D -t 172.18.10.246:8080`    #删除该条VIP

#### 配置维护LVS RIP规则
**-a是添加VIP要对哪些真实服务器IP（即RIP）进行负载（默认是DR模式即-g）。**
##### 配置
```
#-r指定真实服务器IP，-g为选择DR模式（-m为NAT模式，-t为tunnel模式），-w选择权重。
ipvsadm -a -t 172.18.10.246:8080 -r 172.18.10.245:8080 -g -w 1   
ipvsadm -a -t 172.18.10.246:8080 -r 172.18.10.244:8080 -g -w 1   
```
##### 维护

```
#使用-e修改RIP规则的权重。
例：ipvsadm -e -t 172.18.10.246:8080 -r 172.18.10.245:8080 -g -w 4     
#使用-d删除RIP配置
例：ipvsadm -d -t 172.18.10.246:8080 -r 172.18.10.245:8080 -g -w 1
```
#### 在lvs上开启内核转发功能

```
echo 'net.ipv4.ip_forward=1' >>/etc/sysctl.conf
#查看配置
sysctl -p    
```
#### LVS测试

```
#查看lvs规则
ipvsadm -L -n       
#查看连接情况
ipvsadm -L -c  
```
测试成功

#### ipvsadm常用输出命令|状态查看命令
##### 查看lvs的连接状态命令： ipvsadm  -l  --stats

```
root@iZwz9394y9tahel8l6153sZ:/etc/nginx/conf.d# ipvsadm  -l  --stats
IP Virtual Server version 1.2.1 (size=4096)
Prot LocalAddress:Port               Conns   InPkts  OutPkts  InBytes OutBytes
  -> RemoteAddress:Port
TCP  172.18.10.246:http-alt             53      154        0     9576        0
  -> 172.18.10.244:http-alt             50      140        0     8736        0
  -> 172.18.10.245:http-alt              3       14        0      840        0
```
**说明：**

1. Conns    (connections scheduled)  已经转发过的连接数
2. InPkts   (incoming packets)       入包个数
3. OutPkts  (outgoing packets)       出包个数
4. InBytes  (incoming bytes)         入流量（字节）  
5. OutBytes (outgoing bytes)         出流量（字节）

##### 查看lvs速率  ：ipvsadm   -l  --rate

```
root@iZwz9394y9tahel8l6153sZ:/etc/nginx/conf.d# ipvsadm   -l  --rate
IP Virtual Server version 1.2.1 (size=4096)
Prot LocalAddress:Port                 CPS    InPPS   OutPPS    InBPS   OutBPS
  -> RemoteAddress:Port
TCP  172.18.10.246:http-alt              0        0        0        3        0
  -> 172.18.10.244:http-alt              0        0        0        0        0
  -> 172.18.10.245:http-alt              0        0        0        3        0
```

**说明：**

1. CPS      (current connection rate)   每秒连接数
2. InPPS    (current in packet rate)    每秒的入包个数
3. OutPPS   (current out packet rate)   每秒的出包个数
4. InBPS    (current in byte rate)      每秒入流量（字节）
5. OutBPS   (current out byte rate)      每秒入流量（字节）

