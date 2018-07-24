---
title: Linux基本操作命令
url: 254.html
id: 254
categories:
  - Linux
  - 未分类
date: 2016-09-27 09:44:48
tags:
---

**Linux****终端介绍**

**tty****控制台终端   pts虚拟终端**

**tty1-tty6  **

**如是一开始进入的是图形图面：tty1  就是图形界面。**

**图形界面切换到字符界面终端： Ctrl+Shift+Alt+F2-6 **

**字符切换到图形界面：Alt+F1**

**pts****虚拟终端**

**ctrl+shift+T **

**Alt+1**

**放大：ctrl+shift\+ +**

**缩小：ctrl+ -**

**pts(pseudo-terminal slave)** **伪终端 ：通过ssh相关工具连接上后，显示出来终端为：pts**

**Shell****提示符：**

**\[root@xuegod63 ~\]#   **

**\[****用户@主机名 当前目录名称 \] 提示符   root为# 普通用户为：$**

**Bash Shell****基本语法**

**总结Linux命令输入规律：**

**如何输入命令：**

**命令 \[选项\]（\[参数\]）\[选项的值\]（\[参数的值\]）　　**

**空格作为分割**

**常见选项（参数）：-h  --help   ； 特点：选项样子为： -字母  或 --加单词**

**基本命令：**

**ls** **命令**

**作用：查看当前目录下有哪些文件**

**语法：ls 目录 ，如果不加目录查看当前目录**

**参数：**

**-l** **显示详细信息　　　系统里面的快捷方式#ll**

**-a** **显示隐藏文件　**

**-d** **查看目录(不查看里面的内容)**

**-h** **增强可读性**

**Linux****前期是一个积沙成塔。**

**例：**

**\[root@xuegod63 ~\]# ls -l /etc/passwd**

**-rw-r--r-- 1 root root 1753 Jan 21 20:45 /etc/passwd**

**\[root@xuegod63 ~\]# ls /etc/passwd**

**/etc/passwd**

**\[root@xuegod63 ~\]# ls -d /etc/**

**/etc/**

**\[root@xuegod63 ~\]# ls -dl /etc/**

**drwxr-xr-x. 115 root root 12288 Jan 21 20:45 /etc/**

**\[root@xuegod63 ~\]# ls -l /etc/passwd**

**-rw-r--r-- 1 root root 1753 Jan 21 20:45 /etc/passwd**

**\[root@xuegod63 ~\]# ls -lh /etc/passwd**

**-rw-r--r-- 1 root root 1.8K Jan 21 20:45 /etc/passwd**

**例：查看隐藏文件，**

**ls -a**

**.** **当前目录**

**..** **　父目录(上层目录、上级目录)**

**pwd**

**作用：查看当前目录**

**语法：pwd  没有参数**

**例：**

**pwd**

**\[root@xuegod63 ~\]# ll -d /etc/**

**drwxr-xr-x. 115 root root 12288 Jan 21 20:45 /etc/**

**cd**

**作用：切换目录**

**语法：cd 目录名**

**例：**

**#cd** **回到用户自己的主目录**

**#cd ~** **回到用户自己的主目录**

**#cd -** **回到上一个目录**

**#cd /etc////sysconfig/** **和 cd /etc/sysconfig 一样**

**cd /etc/** **和 cd /etc  一样**

**查看系统和BIOS硬件时间：  指的是bios时间**

**\[root@xuegod63 sysconfig\]# hwclock**

**Wed 21 Jan 2015 08:58:11 PM CST  -0.942319 seconds**

**Linux****如何获得帮助**

**方法1：加参数-h 或--help**

**\[root@mk ~\]# shutdown --help**

**\[root@mk ~\]# fdisk -h**

**\[root@mk ~\]# man  find**

**关机命令：**

**shutdown****、 init 、reboot  **

**shutdown**

**作用：关机，重启，定时关机**

**语法：shutdown  \[选项\]**

**有如下选项：**

**-r     =>** **重新启动计算机**

**-h    =>** **关机**

**-h** **时间  =>定时关机**

**-c    =>** **取消之前的定时关机  或ctrl+c**

**例：**

**shutdown -h +10  #10****分钟后关机**

**如果是你想定时在某一具体时间关机时使用命令：**

**shutdown -h 10:01**

**立即关机：shutdown -h now**

**shutdow -r ****用法和 shutdown -h  一样。  
shutdown -r now  立即重启  
shutdown -r +10    #10 分钟以后重启  
shutdown -r  22：22     # 22:22 以后重启**

**init** **命令**

**作用：切换系统运行级别**

**语法：init  0-6**

**例：**

**init 0****命令用于立即将系统运行级别切换为0，即关机；**

**init 6****命令用于将系统运行级别切换为6，即重新启动。**

**Linux 7****个启动级别：**

**0** **系统停机模式，系统默认运行级别不能设置为0，否则不能正常启动，机器关闭。  
1 单用户模式，root权限，用于系统维护，禁止远程登陆，就像Windows下的安全模式登录。  
2 多用户模式，没有NFS网络支持。  
3 完整的多用户文本模式，有NFS，登陆后进入控制台命令行模式。  
4 系统未使用，保留一般不用，在一些特殊情况下可以用它来做一些事情。例如在笔记本电脑的电池用尽时，可以切换到这个模式来做一些设置。  
5 图形化模式，登陆后进入图形GUI模式，X Window系统。  
6 重启模式，默认运行级别不能设为6，否则不能正常启动。运行init 6机器就会重启。**

**runlevel**

**作用：查看当前运行的级别**

**语法：runlevel**

![clip_image001.png](/ueditor/php/upload/image/20160927/1474940502171149.png "1474940502171149.png")

• **创建一个方便快捷实验环境快照**

**配置好静态IP地址，关闭iptables防火墙，关闭selinux**

** \[root@xuegod63 ~\]# setup**

**\[root@xuegod63 ~\]# /etc/init.d/NetworkManager stop**

**Stopping NetworkManager daemon:                            \[FAILED\]**

**\[root@xuegod63 ~\]# chkconfig NetworkManager off**

**\[root@xuegod63 ~\]# /etc/init.d/network restart**

**\[root@xuegod63 ~\]# cat !$**

**cat /etc/selinux/config**

**\# This file controls the state of SELinux on the system.**

**\# SELINUX= can take one of these three values:**

**#     enforcing - SELinux security policy is enforced.**

**#     permissive - SELinux prints warnings instead of enforcing.**

**#     disabled - No SELinux policy is loaded.**

**SELINUX=disabled**

**#SELINUX=enforcing**

**\# SELINUXTYPE= can take one of these two values:**

**#     targeted - Targeted processes are protected,**

**#     mls - Multi Level Security protection.**

**SELINUXTYPE=targeted**

**\[root@xuegod63 ~\]# iptables -F**

**\[root@xuegod63 ~\]# /etc/init.d/iptables save**

**iptables: Saving firewall rules to /etc/sysconfig/iptables:\[  OK  \]**

• **设置服务器在来电后自动开机**

**Linux****基本文件管理**

**本节所讲内容：**

• **Linux****系统目录结构**

• **相对/绝对路径**

• **创建/复制/删除文件**

• **rm -rf /** **意外事故**

• **查看文件内容**

• **恢复Linux被删除的文件**

**Linux****系统目录结构**

**倒置树型结构**

**/** **通常称为根分区。所有的文件和目录皆由此开始。只有root用户对此目录拥有写权限。**

**---/etc** **配置文件  包含所有应用程序的配置文件，也包含启动、关闭某个特定程序的脚本，例如，**

**/etc/passwd****，/etc/init.d/network等。**

**---/boot** **存放Linux系统启动时需要加载的文件。 (一般在另外一个磁盘分区里面保存) Kernel、grub等文件都存放在此。**

**---/home** **普通用户所有数据存放在这个目录下 **

**---/var** **是一个可增长的目录，包含很经常变的文件。例如，/var/log（系统日志）、/var/lib （包文件） 、**

**---/root** **管理员所有数据。  root用户的家目录**

**---/tmp** **临时文件存储位置**

**---/usr  usr****表示的是unix software source**

**---/bin** **命令  此目录包含二进制可执行文件。**

**---/sbin** **系统命令 ，此目录中的命令主要供系统管理员使用，以进行系统维护。例如，iptables、reboot、fdisk等。**

**/mnt -** **挂载目录  挂载点，系统管理员可用于临时挂载文件系统。**

**---/dev** **包含设备文件。在Linux中，一切都被看做文件。终端设备、USB、磁盘等等都被看做文件，如/dev/sda。**

**绝对路径/相对路径：**

**cd /etc/sysconfig/network-scripts/**

**相对路径：**

**\[root@xuegod63 ~\]# cd /etc**

**\[root@xuegod63 etc\]# cd sysconfig/network-scripts/**

**.** **表示当前目录**

**..** **表示上级目录**

**创建/查看/复制/删除 文件和文件夹**

**创建文件和文件夹**

**touch**

**作用：常用来创建空文件**

**语法： touch 文件名**

**\[root@xuegod63 /\]# touch a.txt**

**\[root@xuegod63 /\]# ls a.txt**

**a.txt**

**mkdir**

**作用：创建目录**

**语法：mkdir 目录名**

**例： mkdir /tmp/test**

**-p** **连同父目录一起创建**

**#mkdir /PATH/TO/DIRNAME**

**#mkdir -p**

**/a/b/c/d**

**例1：**

**\[root@xuegod63 ~\]#   mkdir -p  /a/b/c/d**

**\[root@xuegod63 ~\]# cd !$    #!$** **表示上一个命令的最后一个参数**

**cd /a/b/c/d**

**\[root@xuegod63 d\]# cd ../../**

**\[root@xuegod63 b\]# pwd**

**/a/b**

**查看文件方法：**

**cat**

**作用：查看文件内容**

**语法：cat 文件名**

**例：**

**cat /etc/passwd**

**more****　　查看文件名字**

**例：**

**more  /etc/passwd**

**q****　退出**

**less****　　查看文件名字**

**q****　退出**

**使用光标键可以向上翻页**

**linux****中more与less的区别**

**more:****不支持后退，但几乎不需要加参数，空格键是向下翻页，Enter键是向下翻一行，在不需要后退的情况下比较方便。**

**less****：支持前后翻滚，既可以向上翻页（pageup按键），也可以向下翻页（pagedown按键）。，空格键是向下翻页，Enter键是向下翻一行**

**head****　从第一行开始，查看文件，默认显示前10行**

**-n****　数字    显示多少行**

**例：**

**head /etc/passwd**

**head -n 3 /etc/passwd**

**tail** **从第后一行开始，查看文件，默认显示最后10行**

**-n** **显示多少行**

**-f** **动态显示数据（不关闭）　　常用来查看日志**

**\[root@mk tmp\]# tail -f /var/log/messages**

**\[root@mk ~\]# service network restart **

**查看谁修改了/etc/resolv.conf中DNS服务器地址。**

**![clip_image003.png](/ueditor/php/upload/image/20160927/1474940530876140.png "1474940530876140.png")**

**复制文件：**

**cp**

**作用：复制文件**

**语法：cp 源文件  目标文件**

**\[root@xuegod63 ~\]# cp /etc/passwd /tmp/**

**\[root@xuegod63 ~\]# ls /tmp/passwd**

**/tmp/passwd**

**拷贝目录：**

**\[root@xuegod63 ~\]# cp /etc/ /tmp/**

**cp: omitting****（忽略） directory `/etc/'**

**-r** **包含子目录和文件  。**

**\[root@xuegod63 ~\]# cp -r /etc/ /tmp/**

**ctrl+C**

**互动：**

**touch /tmp/test11**

**mkdir /tmp/test11  #****这个命令还可以创建成功吗？**

**注：同一目录下文件和文件名不一样。**

**删除文件和文件夹**

**rm  **

**作用：删除文件或目录**

**语法： rm -rf  文件或目录名**

**\[root@xuegod63 ~\]# rm -rf  /tmp/test**

**\[root@xuegod63 ~\]# rm -rf  /tmp/test11**

**-r** **递归删除（可以删除目录和目录里面的东西）**

**-f** **强行删除**

**重命名：**

**mv **

**作用：重命名**

**语法：mv 源：文件或目录名    目标：文件或目录名**

**\[root@xuegod63 ~\]# mkdir yanhuo**

**\[root@xuegod63 ~\]# mv yanhuo xiaoli**

**\[root@xuegod63 ~\]# ls yanhuo**

**工作经验：  慎用删除命令，勤用mv命令**