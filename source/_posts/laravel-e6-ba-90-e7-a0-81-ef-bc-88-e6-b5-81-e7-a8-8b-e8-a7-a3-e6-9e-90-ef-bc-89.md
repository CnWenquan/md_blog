---
title: laravel源码（流程解析）
tags:
  - laravel
url: 735.html
id: 735
categories:
  - Laravel
  - PHP
date: 2018-01-23 16:35:07
---

解析index.php
===========

> 首先我们来看index.php这个人口文件

//为Laravel应用引入了由 Composer 提供的类加载器，这样Laravel应用便无需再手动加载任 何的类。关于Composer，我有时间，专门写一篇文章require \_\_DIR\_\_.'/../bootstrap/autoload.php';//启动整个项目$app = require\_once \_\_DIR__.'/../bootstrap/app.php';12345

接着我们来看看app.php这个文件都做了什么

解析app.php
=========

$app = new Illuminate\\Foundation\\Application(
    realpath(\_\_DIR\_\_.'/../')
);$app->singleton(
    Illuminate\\Contracts\\Http\\Kernel::class,    App\\Http\\Kernel::class);

$app->singleton(    Illuminate\\Contracts\\Console\\Kernel::class,    App\\Console\\Kernel::class);

$app->singleton( Illuminate\\Contracts\\Debug\\ExceptionHandler::class,    App\\Exceptions\\Handler::class);return $app;1234567891011121314151617181920

> 首先实例化了Application这个类，接下来我们来看看Application这个类

Application类解读
==============

> 首先我们看一下Application类的构造函数

    public function __construct($basePath = null)
    {       //绑定Appliation和Container到instance容器
        $this->registerBaseBindings();       //注册基础的Service Provider
        $this->registerBaseServiceProviders();       //注册别名,把一系列key和对应的类名，接口名加入alias属性数组
        $this->registerCoreContainerAliases();       //注册该应用程序的路径
        if ($basePath) {            $this->setBasePath($basePath);
        }
    }12345678910111213141516

registerBaseServiceProviders这个方法注册两个基本的服务提供者EventServiceProvider和RoutingServiceProvider，关于服务提供者，都继承了Illuminate\\Support\\ServiceProvider这个类，它接受一个 `Application` 对象作为构造函数参数，存储在实例变量 `$app` 中。这个抽象类要求你在你的提供者上定义至少一个方法：`register`。在 `register` 方法中，你应该 只将事物绑定至 [服务容器](https://laravel-china.org/docs/5.1/container) 之中。永远不要试图在`register` 方法中注册任何事件侦听器、路由或任何其它功能。

EventServiceProvider解析
======================

首先我们看一下Illuminate\\Events\\EventServiceProvider的register方法

    public function register()
    {
        $this->app->singleton('events', function ($app) {
            return (new Dispatcher($app))->setQueueResolver(function () use ($app) {
                return $app->make('Illuminate\\Contracts\\Queue\\Factory');
            });
        });
    }12345678

> 上面方法中将`Illuminate\Events\Dispatcher`注册了一个单例绑定到了容器中，键名为 `events`，它负责实现事件的调度。

RoutingServiceProvider解析
========================

再来我们看一下Illuminate\\Routing\\RoutingServiceProvider的register方法

public function register()
{    $this->registerRouter();    $this->registerUrlGenerator();    $this->registerRedirector();    $this->registerPsrRequest();    $this->registerPsrResponse();    $this->registerResponseFactory();
}1234567891011121314

把路由相关的服务(Router， RouterGenerator, Redirector, ResponseFactory)注入到了app主体中

* * *

回到app.php
=========

接着app.php又注册了三个单例到容器,分别是http处理核心类（App\\Http\\Kernel）,cli处理核心类（App\\Console\\Kernel）,和异常处理（App\\Exceptions\\Handler）.

* * *

回到index.php
===========

//在app.php中 Http\\Kernel已经被注册到容器 所以可以通过make调用实例化$kernel = $app->make(Illuminate\\Contracts\\Http\\Kernel::class);

调用了\\Illuminate\\Foundation\\Http\\Kernel::handle来处理请求
$response = $kernel->handle(
    $request = Illuminate\\Http\\Request::capture()
);1234567

解析App\\Http\\Kernel
===================

<?php

namespace App\\Http;

use Illuminate\\Foundation\\Http\\Kernel as HttpKernel;

class Kernel extends HttpKernel{
    /**
     * 注册应用程序的全局HTTP中间件。
     */
    protected $middleware = \[
        \\Illuminate\\Foundation\\Http\\Middleware\\CheckForMaintenanceMode::class,        \\App\\Http\\Middleware\\EncryptCookies::class,        \\Illuminate\\Cookie\\Middleware\\AddQueuedCookiesToResponse::class,        \\Illuminate\\Session\\Middleware\\StartSession::class,        \\Illuminate\\View\\Middleware\\ShareErrorsFromSession::class,        \\App\\Http\\Middleware\\VerifyCsrfToken::class,    \];

    /**
     * 注册应用程序路由中间件。
     */
    protected $routeMiddleware = \[
        'authenticate' => \\App\\Http\\Middleware\\Authenticate::class,
        'auth.basic' => \\Illuminate\\Auth\\Middleware\\AuthenticateWithBasicAuth::class,
        'guest' => \\App\\Http\\Middleware\\RedirectIfAuthenticated::class,
        'search' => \\App\\Http\\Middleware\\FilterSearchCondition::class,
        'role' => \\Zizaco\\Entrust\\Middleware\\EntrustRole::class,
        'permission' => \\Zizaco\\Entrust\\Middleware\\EntrustPermission::class,
        'ability' => \\Zizaco\\Entrust\\Middleware\\EntrustAbility::class,
        'authorize' => \\App\\Http\\Middleware\\Authorize::class,
        'wxAuthenticate' => \\App\\Http\\Middleware\\WxAuthenticate::class,
        'wxActivityStat' => \\App\\Http\\Middleware\\WxActivityStat::class,
        'wxFollowRequired' => \\App\\Http\\Middleware\\WxFollowRequired::class,    \];}123456789101112131415161718192021222324252627282930313233343536373839

该类继承了Illuminate\\Foundation\\Http\\Kernel，我们主要看构造函数和handle方法

__construct方法
-------------

    public function __construct(Application $app, Router $router)
    {
        $this->app = $app;        $this->router = $router;        foreach ($this->routeMiddleware as $key => $middleware) {            $router->middleware($key, $middleware);
        }
    }123456789

> 看构造函数主要功能是注册中间件

handle方法
--------

public function handle($request){...$response = $this->sendRequestThroughRouter($request);
...
}protected function sendRequestThroughRouter($request){//將request类注册到容器$this->app->instance('request', $request);

Facade::clearResolvedInstance('request');//完成http请求环境的初始化 下面详解$this->bootstrap();//运行中间件（关于中间件下回详解）最后dispatchToRouter解析路由 执行并返回路由结果return (new Pipeline($this->app))
->send($request)
->through($this->app->shouldSkipMiddleware() ? \[\] : $this->middleware)
->then($this->dispatchToRouter());
}public function bootstrap()
    {
        if (! $this->app->hasBeenBootstrapped()) {            $this->app->bootstrapWith($this->bootstrappers());
        }
    }protected function bootstrappers()
    {
        return $this->bootstrappers;
    }123456789101112131415161718192021222324252627282930313233343536

Illuminate\\Foundation\\Application

    /**
     * Run the given array of bootstrap classes.
     *
     * @param  array  $bootstrappers
     * @return void
     */
    public function bootstrapWith(array $bootstrappers)
    {
        $this->hasBeenBootstrapped = true;        foreach ($bootstrappers as $bootstrapper) {            $this\['events'\]->fire('bootstrapping: '.$bootstrapper, \[$this\]);            $this->make($bootstrapper)->bootstrap($this);            $this\['events'\]->fire('bootstrapped: '.$bootstrapper, \[$this\]);
        }
    }123456789101112131415161718

> 由此可见，最终\\Illuminate\\Foundation\\Http\\Kernel::bootstrap中通过Application::bootstrapWith完成http请求环境的初始化,具体则是:   
> •标记Application为已经引导启动状态   
> •依次启动\\Illuminate\\Foundation\\Http\\Kernel::$bootstrappers中的项目   
> 1\. Illuminate\\Foundation\\Bootstrap\\DetectEnvironment解析.env文件,设置应用程序的环境   
> 2\. Illuminate\\Foundation\\Bootstrap\\LoadConfiguration：加载应用程序的配置文件、绑定配置类到容器   
> 3\. Illuminate\\Foundation\\Bootstrap\\ConfigureLogging：绑定日志处理类   
> 4\. Illuminate\\Foundation\\Bootstrap\\HandleExceptions：异常处理   
> 5\. Illuminate\\Foundation\\Bootstrap\\RegisterFacades：Facade模式的应用   
> 6\. Illuminate\\Foundation\\Bootstrap\\RegisterProviders：将服务注册到容器(执行register完成注入)   
> 7\. Illuminate\\Foundation\\Bootstrap\\BootProviders：运行每个服务中的boot方法

回到index.php
===========

//运行Illuminate\\Http\\Response父类的send方法$response->send();//在 HTTP 响应被发送到浏览器之后，调用中间件的terminate()方法$kernel->terminate($request, $response);123456

解析Symfony\\Component\\HttpFoundation\\Response的send方法

public function send(){//设置请求头$this->sendHeaders();//输出内容$this->sendContent();if (function\_exists('fastcgi\_finish_request')) {
fastcgi\_finish\_request();
} elseif ('cli' !== PHP_SAPI) {static::closeOutputBuffers(0, true);
}return $this;
}123456789101112131415

> 至此，larvel解析告一段落，基本流程已经走完

总结
==

1、引入了由 Composer 提供的类加载器   
2、基础启动

*   绑定Appliation和Container到instance容器
    
*   注册基础的Service Provider
    
*   注册别名
    
*   注册该应用程序的路径
    

3、 注册http处理核心类（App\\Http\\Kernel）、cli处理核心类（App\\Console\\Kernel）和异常处理（App\\Exceptions\\Handler）

*   注册中间件
    
*   完成http请求环境的初始化
    
*   运行中间件
    

4、运行Illuminate\\Http\\Response父类的send方法响应请求   
5、在 HTTP 响应被发送到浏览器之后，调用中间件的terminate()方法