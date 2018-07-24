---
title: ubuntu gitlab安装
tags:
  - gitlab
url: 624.html
id: 624
categories:
  - Linux
date: 2017-10-27 00:01:44
---

1.安装依赖包，运行命令

sudo apt-get install curl openssh-server ca-certificates postfix1

执行完成后，出现邮件配置，选择Internet那一项（不带Smarthost的）

2.利用清华大学的镜像（[https://mirror.tuna.tsinghua.edu.cn/help/gitlab-ce/](https://mirror.tuna.tsinghua.edu.cn/help/gitlab-ce/)）来进行主程序的安装。  
首先信任 GitLab 的 GPG 公钥:

curl https://packages.gitlab.com/gpg.key 2> /dev/null | sudo apt-key add - &>/dev/null1

利用root用户[1](http://blog.csdn.net/discoverer100/article/details/51814171#fn:how_to_exe_as_root "See footnote")（不是sudo，而是root），vi打开文件/etc/apt/sources.list.d/gitlab-ce.list，加入下面一行：

deb https://mirrors.tuna.tsinghua.edu.cn/gitlab-ce/ubuntu xenial main1

安装 gitlab-ce:

sudo apt-get update
sudo apt-get install gitlab-ce12

3.执行命令

sudo gitlab-ctl reconfigure1

4.打开 sshd 和 postfix 服务

service sshd start service postfix start 12

5.了使 GitLab 社区版的 Web 界面可以通过网络进行访问，我们需要允许 80 端口通过防火墙，这个端口是 GitLab 社区版的默认端口。为此需要运行下面的命令

sudo iptables -A INPUT -p tcp -m tcp --dport 80 -j ACCEPT 1

6.检查GitLab是否安装好并且已经正确运行，输入下面的命令

sudo gitlab-ctl status1

如果得到类似下面的结果，则说明GitLab运行正常

![QQ截图20171026235910.png](/ueditor/php/upload/image/20171026/1509033564701843.png "1509033564701843.png")

7、最后结果：

![QQ截图20171027000033.png](/ueditor/php/upload/image/20171027/1509033606832472.png "1509033606832472.png")