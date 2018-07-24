---
title: Linux 定时任务创建（实现每秒执行）
url: 122.html
id: 122
categories:
  - PHP
date: 2016-07-23 17:48:41
tags:
---

linux crontab 命令，最小的执行时间是一分钟。如需要在小于一分钟内重复执行，可以有两个方法实现。

  

**1.使用延时来实现每N秒执行**

创建一个php做执行动作，非常简单，就是把当前时间写入log。

  
  

<?php  
file\_put\_contents('/home/fdipzone/php/crontab/run.log', date('Y-m-d H:i:s')."\\r\\n", FILE_APPEND);  
?>

crontab -e 输入以下语句，然后 :wq 保存退出。

  

1.  * * * * * php /home/fdipzone/php/crontab/tolog.php  
    
2.  * * * * * sleep 10; php /home/fdipzone/php/crontab/tolog.php  
    
3.  * * * * * sleep 20; php /home/fdipzone/php/crontab/tolog.php  
    
4.  * * * * * sleep 30; php /home/fdipzone/php/crontab/tolog.php  
    
5.  * * * * * sleep 40; php /home/fdipzone/php/crontab/tolog.php  
    
6.  * * * * * sleep 50; php /home/fdipzone/php/crontab/tolog.php  
    

  

  

使用 tail -f 查看执行情况，可以见到log每10秒被写入一条记录。

  

1.  fdipzone@ubuntu:~$ tail -f /home/fdipzone/php/crontab/run.log  
    
2.  2014-03-31 21:47:01  
    
3.  2014-03-31 21:47:11  
    
4.  2014-03-31 21:47:21  
    
5.  2014-03-31 21:47:31  
    
6.  2014-03-31 21:47:41  
    
7.  2014-03-31 21:47:51  
    
8.  2014-03-31 21:48:01  
    

  

**原理：**通过延时方法 sleep N  来实现每N秒执行。

**注意：**

60必须能整除间隔的秒数（没有余数），例如间隔的秒数是2，4，6，10，12等。

如果间隔的秒数太少，例如2秒执行一次，这样就需要在crontab 加入60/2=30条语句。不建议使用此方法，可以使用下面介绍的第二种方法。

  

**2.编写shell脚本实现**

**crontab.sh**

1.  #!/bin/bash  
    

3.  step=2 #间隔的秒数，不能大于60  
    

5.  for (( i = 0; i < 60; i=(i+step) )); do  
    
6.      $(php '/home/fdipzone/php/crontab/tolog.php')  
    
7.      sleep $step  
    
8.  done  
    

10.  exit 0  
    

  

crontab -e 输入以下语句，然后:wq 保存退出。

*   # m h  dom mon dow   command  
    
*   * * * * * /home/fdipzone/php/crontab/crontab.sh
    

  

使用 tail -f 查看执行情况，可以见到log每2秒被写入一条记录。

*   fdipzone@ubuntu:~/php/crontab$ tail -f run.log  
    
*   2014-03-31 22:23:01  
    
*   2014-03-31 22:23:03  
    
*   2014-03-31 22:23:06  
    
*   2014-03-31 22:23:08  
    
*   2014-03-31 22:23:10  
    
*   2014-03-31 22:23:12  
    
*   2014-03-31 22:23:14  
    
*   2014-03-31 22:23:16  
    
*   2014-03-31 22:23:18  
    
*   2014-03-31 22:23:20  
    
*   2014-03-31 22:23:22  
    
*   2014-03-31 22:23:25  
    
*   2014-03-31 22:23:27  
    
*   2014-03-31 22:23:29  
    
*   2014-03-31 22:23:31  
    
*   2014-03-31 22:23:33  
    
*   2014-03-31 22:23:35