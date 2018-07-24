---
title: Laravel-debugbar 开发调试利器
tags:
  - PHP
url: 733.html
id: 733
categories:
  - Laravel
  - PHP
date: 2018-01-23 14:12:28
---

安装
--

1). 使用 Composer 安装该扩展包：

composer require barryvdh/laravel-debugbar

2). 安装完成后，修改 `config/app.php` 在 `providers` 数组内追加 Debugbar 的 Provider

'providers' => \[
    ...
    Barryvdh\\Debugbar\\ServiceProvider::class,\],

同时在 `aliases` 数组内追加如下内容 

'aliases' => \[
    ...
    'Debugbar' => Barryvdh\\Debugbar\\Facade::class,\]

3). 接下来运行以下命令生成此扩展包的配置文件 `config/debugbar.php`：

php artisan vendor:publish --provider="Barryvdh\\Debugbar\\ServiceProvider"

打开 `config/debugbar.php`，将 `enabled` 的值设置为：

'enabled' => env('APP_DEBUG', false),

修改完以后, Debugbar 分析器的启动状态将由 `.env`文件中 `APP_DEBUG` 值决定。

页面刷新后, 看到下图即表示运行成功。

![76ee0c9278f9c301a8b3ebd8f2850869.png](/ueditor/php/upload/image/20180123/1516687885848334.png)

到此, 此拓展包即安装成功 ![:beers:](/ueditor/php/upload/image/20180123/1516687886581962.png ":beers:")![:beers:](/ueditor/php/upload/image/20180123/1516687886581962.png ":beers:")![:beers:](/ueditor/php/upload/image/20180123/1516687886581962.png ":beers:")

  

注意：

使用Laravel Debugbar (Integrates PHP Debug Bar)调试信息报错。

Debugbar::warning(‘Watch out…‘);

报错信息如下：

Call to undefined method DebugBar\\DebugBar::warning()

 ![技术分享](/ueditor/php/upload/image/20180123/1516687922133634.png)

phpStorm工具只能导入包，在导入DebugBar的时候出的问题，导入的是其他的DebugBar的包，所以错误。将命名空间改成：

use Barryvdh\\Debugbar\\Facade as Debugbar;//命名空间引用