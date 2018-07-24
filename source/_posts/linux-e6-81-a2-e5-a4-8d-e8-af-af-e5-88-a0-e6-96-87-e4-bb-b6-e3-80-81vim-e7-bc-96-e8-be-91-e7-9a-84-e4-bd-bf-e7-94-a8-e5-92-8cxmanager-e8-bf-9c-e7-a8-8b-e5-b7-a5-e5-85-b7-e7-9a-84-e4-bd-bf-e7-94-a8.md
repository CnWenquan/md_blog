---
title: Linux恢复误删文件、vim编辑的使用和Xmanager远程工具的使用
url: 258.html
id: 258
categories:
  - Linux
date: 2016-09-27 17:52:34
tags:
---

**初学者了解：**

**恢复Linux下被误删除的文件**

**\[root@xuegod63 ~\]# mount /dev/cdrom /mnt/**

**分一个区：sda4 **

**查找：extundelete**

**分一个区：sda4 **

**\[root@localhost ~\]# mkdir /tmp/sda4**

**\[root@localhost ~\]# mkfs.ext4 /dev/sda4  ****格式化**

**\[root@xuegod63 Desktop\]# echo $?**

**0**

**\[root@localhost ~\]# mount /dev/sda4 /tmp/sda4/**

** \[root@localhost ~\]# df -Th  #****查看磁盘相关信息**

**\[root@xuegod63 Desktop\]# mount /dev/sda4 /tmp/sda4**

**\[root@xuegod63 Desktop\]# df -h**

**Filesystem            Size  Used Avail Use% Mounted on**

**/dev/sda2             9.7G  3.8G  5.5G  41% /**

**tmpfs                 569M  264K  569M   1% /dev/shm**

**/dev/sda1             194M   28M  157M  15% /boot**

**/dev/sr0              3.4G  3.4G     0 100% /mnt**

**/dev/sda4            1012M   34M  928M   4% /tmp/sda4**

**复制一些用于测试删除的文件：**

**\[root@localhost ~\]# cp /etc/passwd /tmp/sda4/**

**\[root@localhost ~\]# cp /etc/hosts /tmp/sda4/**

**\[root@localhost ~\]# vim /tmp/sda4/a.txt #vim****编辑a.txt 随便写入一些数据。**

**\[root@localhost ~\]# mkdir -p  /tmp/sda4/a/b/c**

**\[root@localhost ~\]# cp /tmp/sda4/a.txt /tmp/sda4/a**

**\[root@localhost ~\]# cp /tmp/sda4/a.txt /tmp/sda4/a/b/**

**删除：**

**\[root@xuegod63 ~\]# mkdir /tmp/back**

**\[root@xuegod63 ~\]# cp -r /tmp/sda4/* /tmp/back**

**\[root@xuegod63 ~\]# cd /tmp/sda4/**

**\[root@localhost ~\]# rm -rf passwd hosts a   a.txt**

**\[root@localhost ~\]# ls /tmp/sda4/**

**lost+found**

**卸载需要恢复文件的分区：  inode**

**\[root@xuegod63 sda4\]# cd**

**\[root@xuegod63 ~\]# umount /tmp/sda4/**

**安装软件 extundelete**

**\[root@xuegod63 ~\]# tar jxvf extundelete-0.2.4.tar.bz2**

**\[root@xuegod63 ~\]# cd extundelete-0.2.4**

**\[root@xuegod63 extundelete-0.2.4\]# ./configure**

**Configuring extundelete 0.2.4**

**configure: error: Can't find ext2fs library   #****报错**

**\[root@xuegod63 ~\]# mount /dev/cdrom  /mnt/**

**\[root@xuegod63 extundelete-0.2.4\]# rpm -ivh /mnt/Packages/e2fsprogs-devel-1.41.12-11.el6.x86_64.rpm**

**\[root@xuegod63 extundelete-0.2.4\]#**

**\[root@xuegod63 extundelete-0.2.4\]# ./configure  #****检查安装环境并生成Makefile**

**\[root@xuegod63 extundelete-0.2.4\]# make #****编译**

**\[root@xuegod63 extundelete-0.2.4\]# make install  #****安装**

**开始恢复：**

**方法1：**

**通过inode结点查看被删除的文件名字：**

**\[root@xuegod63 ~\]# mkdir test**

**\[root@xuegod63 ~\]# cd test/**

**\[root@localhost ~\]# extundelete /dev/sda4 --inode 2**

**。。。**

**lost+found                                        11**

**passwd                                            12             Deleted**

**hosts                                             13             Deleted**

**a                                                 8193           Deleted**

**a.txt                                             15             Deleted**

**通过inode节点来恢复：**

**\[root@localhost ~\]# extundelete  /dev/sda4 --restore-inode  12**

**NOTICE: Extended attributes are not restored.**

**Loading filesystem metadata ... 16 groups loaded.**

**Loading journal descriptors ... 32 descriptors loaded.**

** \[root@localhost ~\]# ls RECOVERED_FILES/       #****恢复成功**

**file.12**

**\[root@xuegod63 test\]# diff /etc/passwd /root/test/RECOVERED_FILES/file.12**

**方法二，通过文件名恢复**

**恢复某个文件：**

**\[root@xuegod63 test\]# rm -rf RECOVERED_FILES/**

**\[root@localhost ~\]\# extundelete /dev/sda4 --restore-file  passwd**

**恢复某个目录****，如目录a下的所有文件：**

**\[root@localhost ~\]# extundelete /dev/sda4 --restore-directory   a   #****恢复目录a ，不用**

**恢复所有的文件**

**\[root@localhost ~\]# extundelete  /dev/sda4 --restore-all      **

**使用心得：**

**空目录或空文件恢复不成功。**

  

**vi** **安装**

**\[root@xuegod63 ~\]# rpm -qf \`which vi\`**

**vim-minimal-7.2.411-1.6.el6.x86_64**

**root@xuegod63 ~\]# rpm -ivh /mnt/Packages/vim-minimal-7.2.411-1.6.el6.x86_64.rpm**

**vim****安装：**

**\[root@xuegod63 ~\]# rpm -qf \`which vim\`**

**vim-enhanced-7.2.411-1.6.el6.x86_64**

**\[root@xuegod63 ~\]# rpm -ivh /mnt/Packages/vim-enhanced-7.2.411-1.6.el6.x86_64.rpm**

**vim** **编辑器模式切换:**

**命令模式 、命令行模式、编辑模式**

**命令模式：**

 **字符操作**

 **字符操作**

**i** **当前插入**

**I** **行首插入**

**a** **当前字符之后插入**

**A** **行尾插入**

**esc** **退出当前模式**

**o** **下一行插入**

**O** **上一行插入**

**x** **向后删除一个字符**

**X** **向前删除一个字符**

**u** **撤销一步 **

 **行操作**

**home****键或^ 行首**

**$****行尾      end键**

**dd** **删除一行 Ndd**

**yy** **复制一行 Nyy 复制N行**

**p** **将复制行粘贴 P上粘**

 **词操作**

**dw** **删除一个词，删除时要将光标移动到这个词的行首。 另外，如果光标不在行首，则删除光标之后的字母。**

**yw** **复制一个词**

**块操作**

 **大D 或d+$删至行尾 d+^ 删至行首**

**y+$** **复制至尾 y+^ 复制至首**

**v** **模式**

 **进入v模式 移动光标选择区域、**

**编程的时候需要进行多行注释：  
**** 1****、注释：ctrl+v 进入列编辑模式**

**2****向下或向上移动光标**

**3****把需要注释的行的开头标记起来**

**4****然后按大写的I**

**5****再插入注释符,比如"#"。**

**6****再按Esc,就会全部注释了。**

**删除多行注释：****  
2****、删除：再按ctrl+v 进入列编辑模式；向下或向上移动光标 ；选中注释部分,然后按d, 就会删除注释符号。**

**VIM****命令行模式操作**

**:w** **保存 save  
:q 没有进行任何修改，退出 quit  
:q! 修改了，不保存，强制退出  
:wq 保存并退出**

**：wq! 强制保存并退出。**

**保存：**

**ZZ** **。    改一个字符： r   再对应文字**

**替换  
:%s/this/that 每一行的第一个this被替换成that    
:%s/this/that/g 将文本中所有的this替换成that**

**:1,5 s/old/new/g   #****替换第一行到第5行中匹配内容。**

**:set nu/nonu   #****显示行号和取消行号**

**/** **正向查找  ：****/target     n** **往下查找，N 往上查找**

 **去消高亮显示：  noh  或 随便查找一组没有的字符**

**: !ls** **调用系统命令**

**编辑文目录：**

**如果不小心打开目录，直接退出就可以了。**

**vim****中定位到某行：**

**gg** **定位到行首**

**G** **定位到最后一行，行首**

**vim****打开多个文件：**

**vim****打开多个文件：**

**#vim -O file1 file2   ctrl+ww** **在两个文件间切换。**

**自定义vim**

**#vim ~/.vimrc**

**输入：**

**set nu**

**set history=1000**

**\[root@xuegod63 ~\]# gedit**

**实战：**

**在windows中编辑好的汉字文本文档，上传到Linux下打开乱码。**

**rpm -ivh /mnt/Packages/lrzsz-0.12.20-27.1.el6.x86_64.rpm**

**通过iconv命令转码**

**输入/输出格式规范：  
-f, --from-code=名称 原始文本编码  
-o, --output=FILE 输出文件  
-l, --list 列举所有已知的字符集  
\[root@xuegod63 test\]# iconv -f gb2312 c.txt -o ssss.txt**

**\[root@xuegod63 test\]# cat ssss.txt**

**#!/bin/bash**

**echo "****学神IT"**

**echo "****学神IT"**

**echo "****学神IT"**

**echo "****学神IT"**

**echo "****学神IT"**

**解决将公司服务器上脚本导到windows上打开串行的问题**

**这是因为windows和linux处理回车不同。**

**![](/ueditor/php/upload/image/20160927/1474969914164943.jpg "1474969914164943.jpg")**

**\[root@xuegod63 test\]# unix2dos b.txt**

**unix2dos: converting file b.txt to DOS format ...**