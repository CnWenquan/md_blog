---
title: LVS-NAT模式的配置详解
tags:
  - server
categories:
  - server
date: 2018-07-06 21:09:33
---
### LVS服务的简介：
> LVS是Linux Virtual Server的简写，意即Linux虚拟服务器，是一个虚拟的服务器集群系统，在1998年5月由章文嵩先生主导开发。LVS集群实现了IP负载均衡技术和基于内容请求分发技术。调度器通过将请求均衡地转移到不同的服务器上执行，且可以屏蔽掉后台故障的服务器，从而将一组服务器构成一个高性能的、高可用的服务器集群，而这样的结构对客户端来说是完全透明的，所以无需修改客户端和服务器端的程序。
> LVS服务器可以让客户端将LVS服务器作为一个连接的单点，仅仅通过连接LVS服务器便可以得到后端一整个服务器集群的处理与存储能力，这样能够大大提高系统的扩展性与可用性，同时也能够提供服务的安全性，单一入侵一台服务器并不会破坏其他与该服务器隔离的服务。

### LVS的模式
LVS可以支持如下三种模式：

* Virtual Server via Network Address Translation（VS/NAT）
通过网络地址转换，调度器重写请求报文的目标地址，根据预设的调度算法，将请求分派给后端的真实服务器；真实服务器的响应报文通过调度器时，报文的源地址被重写，再返回给客户，完成整个负载调度过程。

* Virtual Server via IP Tunneling（VS/TUN)
采用NAT技术时，由于请求和响应报文都必须经过调度器地址重写，当客户请求越来越多时，调度器的处理能力将成为瓶颈。为了解决这个问题，调度器把请求报 文通过IP隧道转发至真实服务器，而真实服务器将响应直接返回给客户，所以调度器只处理请求报文。由于一般网络服务应答比请求报文大许多，采用 VS/TUN技术后，集群系统的最大吞吐量可以提高10倍。

* Virtual Server via Direct Routing（VS/DR）
VS/DR通过改写请求报文的MAC地址，将请求发送到真实服务器，而真实服务器将响应直接返回给客户。同VS/TUN技术一样，VS/DR技术可极大地 提高集群系统的伸缩性。这种方法没有IP隧道的开销，对集群中的真实服务器也没有必须支持IP隧道协议的要求，但是要求调度器与真实服务器都有一块网卡连 在同一物理网段上。

鉴于ip安全的需求，这里我们需要分割LVS服务器与后端负载集群的ip地址，并且对于负载均衡集群的数目没有太大的需求，所以笔者选用了Virtual Server via Network Address Translation（VS/NAT) 模式。

### 系统环境
![](https://upload-images.jianshu.io/upload_images/8552201-a6fbabca490d6faf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/649)

系统平台：**Ubuntu 16.04 LTS**

**Load Balancer**: 双网卡

外网地址：39.108.55.227
内网地址：172.18.10.246
**Real Server**:

一共有两台台机器，ip地址分别为172.18.10.244/245。
### Load Balancer服务器的配置：
Load Balancer服务器的配置相对来说较为复杂，我们按部就班的走下这个流程：

* ipvsadm
LVS依赖于ipvsadm来进行配置，所以我们首先先安装ipvsadm：
```
sudo apt-get install ipvsadm
```
* NAT模式下的ip转发
之后我们需要配置NAT模式下的ip转发，让通过Load Balancer的ip包能够转发到真正提供服务的Real Server之上进行处理：

```
echo 1 >> /proc/sys/net/ipv4/ip_forward //在ipv4环境下可以进行ip转发
sudo iptables -t nat -F   //清除之前的iptable的规则
```

* LVS-NAT模式的配置
这里我们采取全盘转发的机制，也就是不指定端口的方式，这种用法可能相对比较少见，但由于NFS服务依赖与rpcbind选择端口，所以提供的端口是随机的，为了简单起见，我们采取全盘转发的模式。
```
sudo ipvsadm -A -t 39.108.55.227:80 -s wrr -p 30  //转发对应的ip,并且采取权重轮询的策略
sudo ipvsadm -a -t 39.108.55.227:80 -r 172.18.10.245:8080 -m -w 1
sudo ipvsadm -a -t 39.108.55.227:80 -r 172.18.10.244:8080 -m -w 1 //依次添加对应的Real Server服务器
```
下面是通过ipvsadm配置好的转发机制：

```
IP Virtual Server version 1.2.1 (size=4096)
Prot LocalAddress:Port Scheduler Flags
  -> RemoteAddress:Port           Forward Weight ActiveConn InActConn
TCP  39.108.55.227:80 wrr persistent 30
  -> 172.18.10.244:8080           Masq    1      0          0         
  -> 172.18.10.245:8080           Masq    1      0          0 
```

### Real Server服务器的配置：
Real Server这里需要配置路由，将默认的网关改为Load Balancer 服务器的内网ip地址，来实现路由转发的效果。（笔者这里多提一句：这里如果采用双网卡机制很容易出现，TCP连接问题，也就是syn消息收不到ack响应，所以尽量将无关的网卡禁用，以免路由配置的复杂。）

**route命令的配置：**

```
sudo route add -net 0.0.0.0 gw 172.18.10.246
```
这个命令需要在每台Real Server之上执行，否则其他Real Server没有办法接受到转发的ip数据包，会被Load Balance屏蔽，从而没有办法实现我们期待的负载均衡的结果。下图所示的是Real Server服务器路由转发配置的结果：

```
root@iZwz9bkpd716xn2z834h9vZ:/home/wenquan/data# route -n
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
0.0.0.0         172.18.10.246   0.0.0.0         UG    0      0        0 eth0
```
### 测试

```
root@iZwz9394y9tahel8l6153sZ:/etc/nginx# curl http://39.108.55.227/
222 
```


```
root@iZwz9394y9tahel8l6153sZ:/etc/nginx# ipvsadm -lc
IPVS connection entries
pro expire state       source             virtual            destination
TCP 00:46  NONE        172.18.10.246:0    39.108.55.227:http 172.18.10.245:http-alt
TCP 01:16  TIME_WAIT   172.18.10.246:44354 39.108.55.227:http 172.18.10.245:http-alt
```






