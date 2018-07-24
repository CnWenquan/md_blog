---
title: 基于Redis的消息队列php-resque
tags:
  - redis
url: 560.html
id: 560
categories:
  - PHP
date: 2017-07-30 00:53:59
---

    目前工作中需要用到消息队列通知下线各个渠道商，暂定用redis实现消息队列。于是百度、谷歌了半天最终决定试试php-resque

    我一直使用的是phpredis扩展. php-resque使用Redis Client是 Credis, https://github.com/colinmollenhour/credis ,而Credis的API完全是基于phpredis来封装的,即API一样,前者是C扩展,后者是PHP实现.

    下面具体介绍一下PHP-Resque的安装与使用.

**PHP-Resque安装**

1.环境需求

*   PHP 5.3+
    
*   Redis 2.2+
    
*   Optional but Recommended: Composer
    

2.安装Composer

$ curl -sS https://getcomposer.org/installer | php
$ mv composer.phar /usr/local/bin/composer

3.安装PHP-Resque

git clone git://github.com/chrisboulton/php-resque.gitcd php-resque
composer install

**php-resque的队列实现**

所有的消息队列都差不多,有生产者,消费者,还有Task Server.只不过名称不同而已.

生产者:接受用户提交信息,组成消息队列中的一个任务,并将其加入消息队列服务中.

消费者:一般为一个守护进程,从消息队列服务器中取出任务,对任务做出相应的处理.PHP-Resque中的消息者为守护进程+Job Class .即由守护进程从消息队列中取出任务,具体的任务中在任务的生产过程中已经指定了由哪个Job Class来做具体的处理.

Task Server: 消息队列服务器,本处指redis服务.

**生成任务(创建一个Job)**

  

// Required if redis is located elsewhere
Resque::setBackend('localhost:6379');
 
$args = array(    'name' => 'Chris');
Resque::enqueue('default', 'My_Job', $args);

其中第一行为连接redis服务

第二行的数组是任务的内容,可随意编写.通过Resque::enqueue 序列化后加入到消息队列中.

最后一行的第一个参数表示 消息队列的名称(可随意标记,比如 email,log等),第二个参数表示取出任务后,由My_Job这个类来处理此条任务.

同时需要说明的是: 在PHP-Resque中也没有数字标识的优化级,但可以通过消息队列名称来实现优化级,具体下面会讲到.

**定义Job Class**

class My_Job
{
    public function setUp()
    {        // ... Set up environment for this job    }
 
    public function perform()
    {        // .. Run job    }
 
    public function tearDown()
    {        // ... Remove environment for this job    }
}

从上面的代码中可以看出,这是一个非常简单的PHP类文件, 方法perform是必须的,主要用来处理任务.其他两个方法可有可无,根据你自己的应用环境看是否需要.

Demo目录中的job.php (修改过)

<?php
    class PHP_Job{        
        public function perform()
        {                
            sleep(5);                
            // fwrite(STDOUT, 'Hello!');
            $p = $this->args\['name'\];                
            echo 'ttime '.$p;                
            // print\_r(json\_decode($p));
            echo "string";
        }
}?>

从上面的例子可以看出,如果想在Job Class中读取任务的具体内容,可直接使用 $this->args\['name'\]来进行读取.按上面的例子来说,它将输出 Chris

**测试消息队列**

_(1)创建任务_

\[root@myfc demo\]# cd php-resque/demo
\[root@myfc demo\]# php queue.php PHP_Job
Queued job bcd1cb6f39d72b9b786716f81b757370

queue.php代码如下:  

<?php
    if(empty($argv\[1\])) {
        die('Specify the name of a job to add. e.g, php queue.php PHP_Job');
    }
 
require \_\_DIR\_\_ . '/init.php';
date\_default\_timezone_set('GMT');
Resque::setBackend('127.0.0.1:6379');
 
$args = array(        
    'time' => time(),        
    'array' => array(                
        'test' => 'test',
        ),
);
 
$jobId = Resque::enqueue('default', $argv\[1\], $args, true);
echo "Queued job ".$jobId."\\n\\n";

此时查看Redis,会发现多了三条数据

resque:queues,  default
resque:queue:default, {"class":"PHP_Job","args":\[{"time":1368167779,"array":{"test":"test"}}\],"id":"875234d18af92212a7d60056daeae910"}
resque:job:875234d18af92212a7d60056daeae910:status, {"status":1,"updated":1368167779,"started":1368167779}

  

注意: “,”逗号前是键名,后面是键值

在开启守护进程以前,请确保PHP\_Job存在,PHP\_Job为PHP类名,而非PHP文件名. 如果PHP_Job不存在,则此条任务会处理失败.(因为根本不存在处理它的文件)

_(2)处理任务_

在demo目录中,守护进程是由目录下的resque.php来进行,代码如下:

<?php
date\_default\_timezone_set('GMT');
require 'bad_job.php';
require 'job.php';
require 'long_job.php';
require 'MtSend_Job.php'; 
require '../bin/resque';

Job Class需要放到此文件头部引入,当然也可以使用其他方式引入. 上面代码中job.php即是类PHP_Job文件

**开启守护进程参数说明:**

在上面的代码中,最后一行引入的../bin/resque 其实也是一个php cli程序,代码如下:

#!/usr/bin/env php<?php 
// Find and initialize Composer$files = array(
  \_\_DIR\_\_ . '/../../vendor/autoload.php',
  \_\_DIR\_\_ . '/../../../autoload.php',
  \_\_DIR\_\_ . '/../../../../autoload.php',
  \_\_DIR\_\_ . '/../vendor/autoload.php',); 
$found = false;foreach ($files as $file) {        if (file\_exists($file)) {                require\_once $file;                break;
        }
} 
if (!class\_exists('Composer\\Autoload\\ClassLoader', false)) {        die(                'You need to set up the project dependencies using the following commands:' . PHP\_EOL .
                'curl -s http://getcomposer.org/installer | php' . PHP_EOL .
                'php composer.phar install' . PHP_EOL
        );
} 
$QUEUE = getenv('QUEUE');if(empty($QUEUE)) {        die("Set QUEUE env var containing the list of queues to work.\\n");
} 
$REDIS\_BACKEND = getenv('REDIS\_BACKEND');$REDIS\_BACKEND\_DB = getenv('REDIS\_BACKEND\_DB');if(!empty($REDIS\_BACKEND)) {        if (empty($REDIS\_BACKEND_DB))
                Resque::setBackend($REDIS_BACKEND);        else
                Resque::setBackend($REDIS\_BACKEND, $REDIS\_BACKEND_DB);
} 
$logLevel = 0;$LOGGING = getenv('LOGGING');$VERBOSE = getenv('VERBOSE');$VVERBOSE = getenv('VVERBOSE');if(!empty($LOGGING) || !empty($VERBOSE)) {        $logLevel = Resque\_Worker::LOG\_NORMAL;
}else if(!empty($VVERBOSE)) {        $logLevel = Resque\_Worker::LOG\_VERBOSE;
} 
$APP\_INCLUDE = getenv('APP\_INCLUDE');if($APP\_INCLUDE) {        if(!file\_exists($APP\_INCLUDE)) {                die('APP\_INCLUDE ('.$APP_INCLUDE.") does not exist.\\n");
        } 
        require\_once $APP\_INCLUDE;
} 
$interval = 5;$INTERVAL = getenv('INTERVAL');if(!empty($INTERVAL)) {        $interval = $INTERVAL;
} 
$count = 1;$COUNT = getenv('COUNT');if(!empty($COUNT) && $COUNT > 1) {        $count = $COUNT;
} 
$PREFIX = getenv('PREFIX');if(!empty($PREFIX)) {    fwrite(STDOUT, '*** Prefix set to '.$PREFIX."\\n");
    Resque_Redis::prefix($PREFIX);
} 
if($count > 1) {        for($i = 0; $i < $count; ++$i) {                $pid = Resque::fork();                if($pid == -1) {                        die("Could not fork worker ".$i."\\n");
                }                // Child, start the worker
                else if(!$pid) {                        $queues = explode(',', $QUEUE);                        $worker = new Resque_Worker($queues);                        $worker->logLevel = $logLevel;                        fwrite(STDOUT, '*** Starting worker '.$worker."\\n");                        $worker->work($interval);                        break;
                }
        }
}// Start a single workerelse {        $queues = explode(',', $QUEUE);        $worker = new Resque_Worker($queues);        $worker->logLevel = $logLevel; 
        $PIDFILE = getenv('PIDFILE');        if ($PIDFILE) {                file\_put\_contents($PIDFILE, getmypid()) or                        die('Could not write PID information to ' . $PIDFILE);
        } 
        fwrite(STDOUT, '*** Starting worker '.$worker."\\n");        $worker->work($interval);
}

从上面的代码可以看出,在命令行启动守护进程时,可以传递一些环境变量.而 ../bin/resque通过getenv函数来获取环境变量.

支持的环境变量有:

========================================================================================================

QUEUE – 這个是必要的，会決定 worker 要執行什麼任務，重要的在前，例如 QUEUE=default,notify,mail,log 。也可以設定為 QUEUE=* 表示執行所有任務。

APP\_INCLUDE – 这也可以说是必要的，因為 Resque 的 Job 都是写成PHP类，那 worker 執行的時候當然要把物件的檔案引入進來。可以設成 APP\_INCLUDE=require.php 再在 require.php 中引入所有 Job 的 Class 文件即可。

COUNT – 設定 worker 數量，預設是1 COUNT=5 。

REDIS_BACKEND – 設定 Redis 的 ip, port。如果沒設定，預設是連 localhost:6379 。

LOGGING, VERBOSE – 設定 log， VERBOSE=1 即可。

VVERBOSE – 比較詳細的 log， VVERBOSE=1 debug 的時候可以開出來看。

INTERVAL – worker 檢查 queue 的間隔，預設是五秒 INTERVAL=5 。

PIDFILE – 如果你是開單 worker，可以指定 PIDFILE 把 pid 寫入，例如 PIDFILE=/var/run/resque.pid 。

=============================================================================

启动守护进程:

QUEUE=* APP_INCLUDE=require.php COUNT=5 VVERBOSE=1 php resque.php

QUEUE=* 表明处理所有的消息队列,此时则没有优化级别.如果需要优化级别,可以按先后顺序列出,如 QUEUE=mail1,mail2,mail3

APP_INCLUDE=require.php 文件中,可以引入其他的一些Job Class或其他辅助类,当然也可以直接在 demo/resque.php中引入

启动守护进程后:

\[root@myfc demo\]# QUEUE=*  COUNT=5 VVERBOSE=1 php resque.php
#!/usr/bin/env php*** Starting worker myfc:22143:*
*** Starting worker myfc:22144:*
** \[07:25:09 2013-05-10\] Registered signals*** Starting worker myfc:22145:*
** \[07:25:09 2013-05-10\] Registered signals** \[07:25:09 2013-05-10\] Registered signals*** Starting worker myfc:22146:*
** \[07:25:09 2013-05-10\] Registered signals*** Starting worker myfc:22148:*
** \[07:25:09 2013-05-10\] Registered signals
\[root@myfc demo\]# ** \[07:25:09 2013-05-10\] Checking default** \[07:25:09 2013-05-10\] Found job on default** \[07:25:09 2013-05-10\] got (Job{default} | ID: 875234d18af92212a7d60056daeae910 | PHP\_Job | \[{"time":1368167779,"array":{"test":"test"}}\])** \[07:25:09 2013-05-10\] Forked 22163 at 2013-05-10 07:25:09** \[07:25:09 2013-05-10\] Processing default since 2013-05-10 07:25:09** \[07:25:09 2013-05-10\] Checking default** \[07:25:09 2013-05-10\] Sleeping for 5** \[07:25:09 2013-05-10\] Checking default** \[07:25:09 2013-05-10\] Sleeping for 5** \[07:25:09 2013-05-10\] Checking default** \[07:25:09 2013-05-10\] Sleeping for 5** \[07:25:09 2013-05-10\] Checking default** \[07:25:09 2013-05-10\] Sleeping for 5ttime 1368167779string** \[07:25:14 2013-05-10\] done (Job{default} | ID: 875234d18af92212a7d60056daeae910 | PHP\_Job | \[{"time":1368167779,"array":{"test":"test"}}\])** \[07:26:29 2013-05-10\] Checking default** \[07:26:29 2013-05-10\] Checking default** \[07:26:29 2013-05-10\] Sleeping for 5** \[07:26:29 2013-05-10\] Sleeping for 5** \[07:26:34 2013-05-10\] Checking default** \[07:26:34 2013-05-10\] Checking default** \[07:26:34 2013-05-10\] Sleeping for 5** \[07:26:34 2013-05-10\] Sleeping for 5** \[07:26:34 2013-05-10\] Checking default** \[07:26:34 2013-05-10\] Sleeping for 5** \[07:26:34 2013-05-10\] Checking default** \[07:26:34 2013-05-10\] Checking default** \[07:26:34 2013-05-10\] Sleeping for 5** \[07:26:34 2013-05-10\] Sleeping for 5** \[07:26:39 2013-05-10\] Checking default** \[07:26:39 2013-05-10\] Checking default** \[07:26:39 2013-05-10\] Sleeping for 5** \[07:26:39 2013-05-10\] Sleeping for 5** \[07:26:39 2013-05-10\] Checking default** \[07:26:39 2013-05-10\] Sleeping for 5** \[07:26:39 2013-05-10\] Checking default** \[07:26:39 2013-05-10\] Checking default** \[07:26:39 2013-05-10\] Sleeping for 5** \[07:26:39 2013-05-10\] Sleeping for 5

可以看出,已经成功取出任务,并使用PHP_Job处理.然后守护进程一直在运行,等待处理新的任务

此时看一下进程文件:

\[root@myfc sop\]# ps -aux|grep php
root     22143  0.0  0.0 310700 11496 pts/2    S    15:25   0:00 php resque.php
root     22144  0.0  0.0 310700 11580 pts/2    S    15:25   0:00 php resque.php
root     22145  0.0  0.0 310700 11496 pts/2    S    15:25   0:00 php resque.php
root     22146  0.0  0.0 310700 11496 pts/2    S    15:25   0:00 php resque.php
root     22148  0.0  0.0 310700 11496 pts/2    S    15:25   0:00 php resque.php

**查看任务的状态**

在生成任务的时候,使用 `$token = Resque::enqueue('default', 'My_Job', $args, true); 生成一个任务,第四个参数设置为true,会返回任务的JobID,通过JobID可以查看任务的状态.`

  

$status = new Resque\_Job\_Status($token);
echo $status->get(); // Outputs the status

在`类文件Resque_Job_Status中定义了所有类型`任务状态

Resque\_Job\_Status::STATUS_WAITING - 等待被取出处理
Resque\_Job\_Status::STATUS_RUNNING - 任务正在被处理
Resque\_Job\_Status::STATUS_FAILED - 任务处理失败
Resque\_Job\_Status::STATUS_COMPLETE - 任务处理完成