---
title: ubuntu下使用pip3.6安装模块总是报ssl module in Python is not available的错误
tags:
  - python
categories:
  - python
date: 2018-06-26 19:09:33
---

刚刚安装python3.6,在使用pip3.6安装模块的时候出现此错误。
从错误提示来看是缺少了ssl模块，解决办法：

```
1.安装ssl  
sudo apt-get install openssl  
  
sudo apt-get install libssl-dev  
  
2. 修改Moudles/Setup  (该目录在python的解压目录下)  
  
vim Modules/Setup  
#修改结果如下：  
# Socket module helper for socket(2)  
_socket socketmodule.c timemodule.c  
  
# Socket module helper for SSL support; you must comment out the other  
# socket line above, and possibly edit the SSL variable:  
#SSL=/usr/local/ssl  
_ssl _ssl.c \  
-DUSE_SSL -I$(SSL)/include -I$(SSL)/include/openssl \  
-L$(SSL)/lib -lssl -lcrypto  
  
  
3.重新安装一次  
  
./configure --prefix=/usr/local/python  
  
make  
  
sudo -H make install  
  
  
4.python3  
  
>>>import ssl  #检测成功！  
>>>   
```



