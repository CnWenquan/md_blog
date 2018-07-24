---
title: 使用Nginx+Gunicorn+virtualenv+supervisor来部署django项目
tags:
  - python
url: 813.html
id: 813
categories:
  - python
date: 2018-05-01 22:05:37
---

安装Virtualenv
------------

终端下输入 `sudo pip install virtualenv`  
`virtualenv`可以创建一个拥有自己安装目录的环境, 这个环境不与其他虚拟环境共享库, 能够方便的管理`python`版本和管理`python`库。 下面是创建python3的虚拟环境

#创建ENV目录
root@dev:~# cd ~ && mkdir ENV && cd ENV
root@dev:~/ENV# ls
#创建python3虚拟环境
root@dev:~/ENV# virtualenv -p /usr/bin/python3 python3
Running virtualenv with interpreter /usr/bin/python3
Using base prefix '/usr'
New python executable in /root/ENV/python3/bin/python3
Also creating executable in /root/ENV/python3/bin/python
Installing setuptools, pip, wheel...done.

看到类似这样的输出说明虚拟环境我们已经创建成功，下面是使他生效:

root@dev:~/ENV# source python3/bin/activate
(python3) root@dev:~/ENV#

注意，在虚拟环境生效之后前面会有`(python3)`的提示，来指示现在所生效的虚拟环境名。  
在创建虚拟环境的时候，如果加上`--system-site-packages`的话会集成系统现有的包。  
这样，虚拟环境就生效了，我们就可以使用pip来安装包而不会和系统的包发生冲突。  
关闭可以使用如下命令来退出现在的虚拟环境:

(python3) root@dev:~/ENV# deactivate
root@dev:~/ENV#

可以看到，在退出之后前面的`(python3)`就会消失掉。 接下来就是安装`gunicorn`和`django`等包，直接使用pip安装即可。注意是在之前创建的虚拟环境生效的前提下来安装。下面会介绍gunicorn。  
`pip install django gunicorn`

配置gunicorn
----------

Gunicorn (独角兽)是一个高效的[Python](http://www.lylinux.org/tag/python) WSGI Server,通常用它来运行 wsgi application(由我们自己编写遵循WSGI application的编写规范) 或者 wsgi framework(如Django,Paster),地位相当于Java中的Tomcat。  
在我们要部署的`Django`项目中执行如下命令来尝试运行:  
`gunicorn DjangoBlog.wsgi:application -b 0.0.0.0:6666` `DjangoBlog`为要运行的Django项目名，如果有类似如下输出则说明成功执行: ![tryrun](/ueditor/php/upload/image/20180501/1525183509173557.jpg)  
上面的命令是一个简单的测试，为了真正在生产环境下使用Gunicorn，我们还需要增加一些配置。我们把这些配置文件写成一个bash脚本，保存为`bin/gunicorn_start`:

#!/bin/bash
NAME="DjangoBlog"
DJANGODIR=/root/test/DjangoBlog #Django project directory
SOCKFILE=/root/test/DjangoBlog/run/gunicorn.sock # we will communicte using this unix socket
USER=root # the user to run as
GROUP=root # the group to run as
NUM_WORKERS=2 # how many worker processes should Gunicorn spawn
DJANGO\_SETTINGS\_MODULE=DjangoBlog.settings # which settings file should Django use
DJANGO\_WSGI\_MODULE=DjangoBlog.wsgi # WSGI module name

echo "Starting $NAME as \`whoami\`

"# Activate the virtual environment
cd $DJANGODIR
source /root/ENV/python3/bin/activateexport 
DJANGO\_SETTINGS\_MODULE=$DJANGO\_SETTINGS\_MODULE
export PYTHONPATH=$DJANGODIR:$PYTHONPATH# Create the run directory if it doesn't exist
RUNDIR=$(dirname $SOCKFILE)
test -d $RUNDIR || mkdir -p $RUNDIR
# Start your Django Unicorn
# Programs meant to be run under supervisor should not daemonize themselves (do not use --daemon)
#gunicorn目录(刚刚创建的虚拟环境的bin目录中)
exec /root/ENV/python3/bin/gunicorn  ${DJANGO\_WSGI\_MODULE}:application \
--name $NAME \\--workers $NUM_WORKERS \
--user=$USER --group=$GROUP \
--bind=unix:$SOCKFILE \
--log-level=debug \
--log-file=-

添加可执行权限： `sudo chmod u+x bin/gunicorn_start` 然后就可以尝试执行这个脚本了: `./bin/gunicorn_start`  
如果看到类似下面对输出说明已经成功了: ![rungunicorn](/ueditor/php/upload/image/20180501/1525183512226786.jpg)  
如果你在上面的过程中设置了自定义的参数的话，需要将`gunicorn_start`脚本中对应的参数改过来。其中，worker的数量推荐设置为2 * CPUs + 1，这样的话，在任何时候都有一半的worker在做IO.

安装Supervisor
------------

Superviosr是一个进程监管的工具。简而言之，Superviosr可以保证你的程序在服务器开机时自动启动以及程序意外终止时重新启动。通过下面的命令即可安装:  
`sudo aptitude install supervisor`  
Superviosr通过配置文件来设置被监管的程序。一般配置文件都放置在/etc/supervisor/conf.d路径下面。此处我们创建一个名为djaogoblog.conf的配置文件，内容如下：

\[program:djangoblog\]
command = /root/test/DjangoBlog/bin/django_start
user = root
autostart=true
autorestart=true

redirect_stderr = true
stdout_logfile = /root/logs/blog/robot.log
stderr_logfile=/root/logs/blog/err.log

日志目录还需要手动创建下: `mkdir -p /root/logs/blog`  
设置好上面的文件以后，我们可以通过supervisorctl工具来启用这些设置了:

sudo supervisorctl update
sudo supervisorctl reload

配置Nginx 为我们的Django应用创建一个配置文件`/etc/nginx/sites-available/djangoblog.conf`. 文件内容如下：
---------------------------------------------------------------------------------

upstream djangoblog_server {
    server unix:/root/test/DjangoBlog/run/gunicorn.sock fail_timeout=0;}server {

    server_name blog.lylinux.org;
    root /root/test/DjangoBlog;

    listen 80;

    keepalive_timeout 70;
    #access\_log /var/log/nginx/django\_access.log;
    #error\_log /var/log/nginx/django\_error.log;

    location /static/ {
          expires max;
          alias /root/test/DjangoBlogcollectedstatic/;
    }

    location / {
         proxy\_set\_header X-Forwarded-For $proxy\_add\_x\_forwarded\_for;


        if (!-f $request_filename) {
            proxy\_pass http://djangoblog\_server;
            break;
        }
    }}

使其生效:

  sudo ln -s  
   /etc/nginx/sites-available/djangoblog.conf  
    /etc/nginx/sites-enabled/djangoblog.conf  
    #重启nginx
    sudo service nginx restart

至此，全部完成～～