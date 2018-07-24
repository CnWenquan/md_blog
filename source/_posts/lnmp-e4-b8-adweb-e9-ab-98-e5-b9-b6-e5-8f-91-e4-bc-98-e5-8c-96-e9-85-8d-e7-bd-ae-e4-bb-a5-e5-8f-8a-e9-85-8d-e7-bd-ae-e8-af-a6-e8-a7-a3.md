---
title: LNMP中web高并发优化配置以及配置详解
tags:
  - linux
url: 768.html
id: 768
categories:
  - 服务器
date: 2018-03-07 18:22:29
---

### 一、nginx配置优化(nginx.cnf)

`nginx.cnf`为`nginx`的配置文件，我们可以在这里优化我们的`nginx`服务器，ubuntu中文件位置为`/etc/nginx/nginx.conf`

#### 1.1、worker_processes 2;

`nginx`进程数，建议按照`cpu`数目来指定，一般为它的倍数 (如2个四核的cpu计为8，可以设置为8或者16)，不过不是越大越好。

#### 1.2、worker\_cpu\_affinity 00000001 00000010 00000100 00001000 00010000 00100000 01000000 10000000;

为每个进程分配cpu，上例中将8个进程分配到8个cpu，当然可以写多个，或者将一个进程分配到多个cpu。

#### 1.3、worker\_rlimit\_nofile 65535;

这个指令是指当一个`nginx`进程打开的最多文件描述符数目，理论值应该是最多打开文件数`ulimit -n`与`nginx`进程数相除，但是`nginx`分配请求并不 是那么均匀，所以最好与`ulimit -n`的值保持一致,具体数量在命令行输入`ulimit -n`或者`ulimit -a`即可查看。

现在`linux 4.4`内核下开启文件打开数为`65535`，`worker_rlimit_nofile`就相应应该填写`65535`。

#### 1.4、use epoll;

`epoll`是多路复用`IO(I/O Multiplexing)`中的一种方式,仅用于`linux2.6`以上内核,可以大大提高nginx的性能

#### 1.5、worker_connections 65535;

每个进程允许的最多连接数， 理论上每台`nginx`服务器的最大连接数为`worker_processes*worker_connections`。

#### 1.6、keepalive_timeout 60;

`keepalive`超时时间，当`keepalive_timeout`时间大于0时，即启用`Keep-Alive`时，一个`tcp`连接的生命周期。

#### 1.7、client\_header\_buffer_size 4k;

客户端请求头部的缓冲区大小，这个可以根据你的系统分页大小来设置，一般一个请求头的大小不会超过1k，不过由于一般系统分页都要大于1k，所以这里 设置为分页大小。 

#### 1.8、open\_file\_cache max=65535 inactive=60s;

这个将为打开文件指定缓存，默认是没有启用的，`max`指定缓存数量，建议和打开文件数一致，`inactive`是指经过多长时间文件没被请求就删除缓存，即缓存多少时间。

#### 1.9、open\_file\_cache_valid 30s;

这个是指多长时间检查一次缓存的有效信息，如果发现30s内没有用过一次的删除。

#### 1.9.1、open\_file\_cache\_min\_uses 1;

`open_file_cache`指令中的`inactive`参数时间内文件的最少使用次数，如果超过这个数字，文件描述符一直是在缓存中打开的，如上例，如果有一个 文件在`inactive`时间内一次没被使用，它将被移除。

### 二、php配置优化(php-fpm.cnf)

#### 2.1、pm (类型string)

设置进程管理器如何管理子进程. 可用值: `static`, `dynamic`. 必须设置.

*   `static` – 子进程的数量是固定的 (`pm.max_children`).
    
*   `dynamic` – 子进程的数量在下面配置的基础上动态设置: `pm.max_children`, `pm.start_servers`, `pm.min_spare_servers`, `pm.max_spare_servers`.
    

#### 2.2、pm.max_children (类型int)

子进程的数量，`pm`设置为`static`时表示创建的，`pm`设置为`dynamic`时表示最大可创建的,必须设置。4G内存一般可以设置为200

#### 2.3、pm.start_servers (类型int)

设置启动时创建的子进程数目，仅在`pm`设置为`dynamic`时使用. 默认值: `min_spare_servers+(max_spare_servers-min_spare_servers)/2`.

#### 2.4、pm.min\_spare\_servers (类型int)

设置空闲服务进程的最低数目，仅在`pm`设置为`dynamic`时使用，必须设置。

#### 2.5、pm.max\_spare\_servers (类型int)

设置空闲服务进程的最大数目，仅在`pm`设置为`dynamic`时使用，必须设置。

#### 2.6、pm.max_requests (类型int)

设置每个子进程重生之前服务的请求数，对于可能存在内存泄漏的第三方模块来说是非常有用的，如果设置为`0`则一直接受请求；等同于`PHP_FCGI_MAX_REQUESTS`环境变量，默认值: 0。

### 三、mysql配置优化(my.cnf)

参考[MySQL数据库高并发优化配置](https://www.cnblogs.com/musings/p/5913157.html)

#### 3.1、key\_buffer\_size

这对MyISAM表来说非常重要。如果只是使用MyISAM表，可以把它设置为可用内存的 30-40%。合理的值取决于索引大小、数据量以及负载 — 记住，MyISAM表会使用操作系统的缓存来缓存数据，因此需要留出部分内存给它们，很多情况下数据比索引大多了。尽管如此，需要总是检查是否所有的 key\_buffer 都被利用了 — .MYI 文件只有 1GB，而 key\_buffer 却设置为 4GB 的情况是非常少的。这么做太浪费了。如果你很少使用MyISAM表，那么也保留低于 16-32MB 的 key\_buffer\_size 以适应给予磁盘的临时表索引所需。具体参考[优化mysql之key\_buffer\_size设置](http://www.jb51.net/article/85310.htm)

#### 3.2、innodb\_buffer\_pool_size

这对`Innodb`表来说非常重要。`Innodb`相比`MyISAM`表对缓冲更为敏感。`MyISAM`可以在默认的`key_buffer_size` 设置下运行的可以，然而`Innodb`在默认的`innodb_buffer_pool_size`设置下却跟蜗牛似的。由于`Innodb`把数据和索引都缓存起来，无需留给操作系统太多的内存，因此如果只需要用`Innodb`的话则可以设置它高达 70-80% 的可用内存。一些应用于`key_buffer`的规则有 — 如果你的数据量不大，并且不会暴增，那么无需把`innodb_buffer_pool_size`设置的太大了。

#### 3.3、innodb\_additional\_pool_size

这个选项对性能影响并不太多，至少在有差不多足够内存可分配的操作系统上是这样。不过如果你仍然想设置为`20MB`(或者更大)，因此就需要看一下`Innodb`其他需要分配的内存有多少。

#### 3.4、innodb\_log\_file_size

在高写入负载尤其是大数据集的情况下很重要。这个值越大则性能相对越高，但是要注意到可能会增加恢复时间。我经常设置为 `64-512MB`，跟据服务器大小而异。

#### 3.5、innodb\_log\_buffer_size

默认的设置在中等强度写入负载以及较短事务的情况下，服务器性能还可 以。如果存在更新操作峰值或者负载较大，就应该考虑加大它的值了。如果它的值设置太高了，可能会浪费内存 — 它每秒都会刷新一次，因此无需设置超过1秒所需的内存空间。通常`8-16MB`就足够了。越小的系统它的值越小。

#### 3.6、innodb\_flush\_logs\_at\_trx_commit

是否为`Innodb`比`MyISAM`慢1000倍而头大？看来也许你忘了修改这个参数了。默认值是 1，这意味着每次提交的更新事务（或者每个事务之外的语句）都会刷新到磁盘中，而这相当耗费资源，尤其是没有电池备用缓存时。很多应用程序，尤其是从 MyISAM转变过来的那些，把它的值设置为 2 就可以了，也就是不把日志刷新到磁盘上，而只刷新到操作系统的缓存上。日志仍然会每秒刷新到磁盘中去，因此通常不会丢失每秒1-2次更新的消耗。如果设置 为 0 就快很多了，不过也相对不安全了 — MySQL服务器崩溃时就会丢失一些事务。设置为 2 指挥丢失刷新到操作系统缓存的那部分事务。

#### 3.7、table_cache

打开一个表的开销可能很大。例如`MyISAM`把`MYI`文件头标志该表正在使用中。你肯定不希望这种操作太频繁，所以通常要加大缓存数量，使得足以最大限度 地缓存打开的表。它需要用到操作系统的资源以及内存，对当前的硬件配置来说当然不是什么问题了。如果你有200多个表的话，那么设置为 `1024`也许比较合适（每个线程都需要打开表），如果连接数比较大那么就加大它的值。我曾经见过设置为`100,000`的情况。

#### 3.8、thread_cache

线程的创建和销毁的开销可能很大，因为每个线程的连接/断开都需要。我通常至少设置为`16`。如果应用程序中有大量的跳跃并发连接并且 `Threads_Created`的值也比较大，那么我就会加大它的值。它的目的是在通常的操作中无需创建新线程。

#### 3.9、query_cache

如果你的应用程序有大量读，而且没有应用程序级别的缓存，那么这很有用。不要把它设置太大了，因为想要维护它也需要不少开销，这会导致`MySQL`变慢。通常设置为 `32-512Mb`。设置完之后最好是跟踪一段时间，查看是否运行良好。在一定的负载压力下，如果缓存命中率太低了，就启用它。

#### 3.9.1、sort\_buffer\_size

如果你只有一些简单的查询，那么就无需增加它的值了，尽管你有`64GB`的内存。搞不好也许会降低性能。

#### 3.9.2、mysqld参数描述以及建议

![](blob:http://blog.wenquanpage.com/8b5b0381-4d41-4bf5-bd8a-21f12c8a5825)

#### 3.9.3、不用长连接，程序查询完毕后`close`数据库

#### 3.9.4、主从同步

change master to master\_host='192.168.1.199', master\_user='apk\_service', master\_password='123456', master\_log\_file='mysql-bin.000042', master\_log\_pos=366;