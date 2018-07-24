---
title: windows下执行laravel框架定时任务
tags:
  - laravel
url: 712.html
id: 712
categories:
  - Laravel
date: 2017-12-26 16:28:54
---

1、设置php、mysql全局变量（先决条件）  

2、在laravel中添加command任务（位置：/app/Console/Kerner.php，其他方法自行百度去）：

protected function schedule(Schedule $schedule)  
{  
  $schedule->call(function (){  
  $now = Carbon::now();  
  $six_hours = Carbon::today()->addHours($_ENV\['BACKUP_HOUR'\]);  
  $six\_hours\_minute = Carbon::today()->addHours($_ENV\['BACKUP_HOUR'\])->addMinutes(1);  
  if ($now->between($six_hours,$six\_hours\_minute)){  
  $filename='Auto_'.date("Y-m-d_H-i-s").".sql"; //生成sql文件名  
  header("Content-disposition:filename=".$filename);  
  header("Content-type:application/octetstream");  
  header("Pragma:no-cache");  
  header("Expires:0");  
  $filepath = $_ENV\['BACKUP_FILE'\].$filename; //文件保存路径  
  exec("mysqldump  -uroot --default-character-set=utf8 school-line -n > ".$filepath); //所有表结构和数据  
  \\Log::info('yes ------------> '.date('H:i:s'));  
  }else{  
 \\Log::info('now------------->'.$now);  
  \\Log::info('hours------------->'.$six_hours);  
  \\Log::info('minutes------------->'.$six\_hours\_minute);  
  \\Log::info('no ------------> '.date('H:i:s'));  
  }  
 })->everyMinute();  
  
}

3、执行下php artisan schedule:run测试下，如果输出一下内容算正常

Running scheduled command: Closure

  

4、创建windows的bat文件，添加一下内容并保存

D:

cd D:\\workspace\\services\\webserver\\www\\school-line

php artisan schedule:run

  

5、打开windows定时任务

![image.png](/ueditor/php/upload/image/20171226/1514276245611039.png "1514276245611039.png")

![image.png](/ueditor/php/upload/image/20171226/1514276271445494.png "1514276271445494.png")

设置任务触发条件跟操作后保存重启电脑

  

6、查看laravel中的打印日志：

如果每隔一分钟执行一次，说明正常设置

  

注意：

设置windows定时任务后，非SYSTEM账户执行定时任务，会在执行时弹出一下命令框再关闭，所以设置时需要指定SYSTEM执行。

![image.png](/ueditor/php/upload/image/20171226/1514276861721724.png "1514276861721724.png")

![image.png](/ueditor/php/upload/image/20171226/1514276880691680.png "1514276880691680.png")

![image.png](/ueditor/php/upload/image/20171226/1514276907334346.png "1514276907334346.png")