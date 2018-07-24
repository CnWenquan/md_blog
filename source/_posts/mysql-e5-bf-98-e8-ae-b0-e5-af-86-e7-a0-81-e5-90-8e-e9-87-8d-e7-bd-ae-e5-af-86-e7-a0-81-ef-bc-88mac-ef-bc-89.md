---
title: Mysql忘记密码后重置密码（Mac）
tags:
  - mysql
url: 472.html
id: 472
categories:
  - MySql
date: 2017-03-30 11:20:27
---

1.  关闭mysql服务器  
         sudo /usr/local/mysql/support-files/mysql.server stop  
         也可以在系统偏好里有个MySQL里关闭。
    
2.  cd /usr/local/mysql/bin    进入目录
    
3.  sudo su 获取权限
    
4.   ./mysqld_safe      --skip-grant-tables & 重启服务器
    
5.  重开个终端，
    

配置短命令：

alias mysql=/usr/local/mysql/bin/mysql

alias mysqladmin=/usr/local/mysql/bin/mysqladmin

1.  输入mysql 进入mysql命令模式
    
2.  use mysql进入mysql数据库
    
3.  flush      privileges;大概就是获取权限，要不他不让你改。
    
4.  set      password for 'root'@'localhost'=password('新密码'); 完成修改