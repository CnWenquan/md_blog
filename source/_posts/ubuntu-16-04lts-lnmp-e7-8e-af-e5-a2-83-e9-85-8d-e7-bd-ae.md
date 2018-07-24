---
title: Ubuntu 16.04LTS LNMP环境配置
tags:
  - ubuntu
url: 622.html
id: 622
categories:
  - 未分类
date: 2017-10-26 22:31:47
---

### 1.安装mysql

`sudo` `apt` `install` `mysql-server`

### 2.安装nginx和php

`#添加nginx和php的ppa源`

`sudo` `apt-add-repository ppa:nginx``/stable`

`sudo` `apt-add-repository ppa:ondrej``/php`

`sudo` `apt update`

`sudo` `apt` `install` `nginx`

  

 安装好nginx，打开浏览器输入 http://localhost    看到 Welcome to nginx！ 说明安装成功了。

 安装php 7.0 和 安装其他版本也是一样的。

### 3.安装_PHP_FastCGI管理器

`sudo` `apt` `install` `php7.0-fpm`

###  4.修改配置文件

`sudo` `vim` `/etc/php/7``.0``/fpm/pool``.d``/www``.conf`

`#nginx 和fastcgi通信有2种方式，一种是TCP方式，还有种是UNIX Socket方式`

`#默认是socket方式`

`listen =` `/run/php/php7``.0-fpm.sock`

`#TCP方式`

`#listen = 127.0.0.1:9000`

`#可以用如下方式检查下配置文件是否有错误`

`sudo` `php-fpm7.0 -t `

`#修改重启下 php-fpm7.0`

`sudo` `service php-fpm7.0 restart`

`#修改nginx配置文件`

`sudo` `vim` `/etc/nginx/sites-enabled/default`

`root` `/var/www``;`

`# Add index.php to the list if you are using PHP`

`index index.php index.html index.htm index.nginx-debian.html;`

`#找到   location ~ \.php$  { 修改里面`

`#socket 方式 必须和上面socket的listen路径一样`

`fastcgi_pass unix:``/run/php/php7``.0-fpm.sock;`

`#TCP方式`

`#fastcgi_pass 127.0.0.1:9000;`

`#不管用那种方式，通信方式一定要对应。`

`#修改重启下 nginx`

`sudo` `service nginx restart`

`#nginx 检查配置文件命令是`

`sudo` `nginx -t`

到此配置文件基本ok了，我们在/var/www目录下，新建个index.php测试下看看

`<?php`

`phpinfo();`

`?>`