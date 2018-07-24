---
title: linux 配置固定IP地址
tags:
  - linux
url: 850.html
id: 850
categories:
  - Linux
date: 2018-05-26 23:50:51
---

1、ifconfig etj0 192.168.0.100 netmask 255.255.255.0

#临时设置eth0网卡的IP地址与子网掩码

2、CentOS下使用setup工具：红毛专有图形化工具setup设置IP地址

选择网络配置-》进行address、网关、子网掩码配置

3、Ubuntu下配置：

①：配置接口信息：

vim /etc/network/interfaces

auto enp2s0
#iface enp2s0 inet dhcp

iface enp2s0 inet static #设置静态IP
address 192.168.0.110    #内网IP地址
netmask 255.255.255.0    #子网掩码
gateway 192.168.0.1      #网关

②：配置DNS  

vim /etc/resolv.conf

nameserver 192.168.0.1

  

三：重启网络服务：

/etc/init.d/networking restart