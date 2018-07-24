---
title: php websocket实现实时通信
tags:
  - PHP
url: 465.html
id: 465
categories:
  - PHP
date: 2017-03-06 17:26:35
---

    要实现网页间的实时通信，在不刷新界面的情况下最难的不是客户端给服务端发送请求，而是服务端给客户端推送，目前的本办法由轮询和服务器的pending，都是无谓的消耗。自从h5新加了websocket技术，这才是实时通信的王道。  

    Websocket简介：  
        Websocket不是一门技术，而是一种全新的协议。它应用TCP的Socket（套接字），为网络应用定义了一个新的重要的能力：客户端和服务器端的双全工传输和双向通信。TCP协议支持长连接，Http协议支持短连接。顾名思义长连接可以长时间保持通信，短链接则需要定时的刷新。  

    创建服务端socket：  
        

// 创建一个 TCP socket, 此函数的可选值在官方文档中写得十分详细，这里不再提了
$this->master = socket\_create(AF\_INET, SOCK\_STREAM, SOL\_TCP);
// 设置IP和端口重用,在重启服务器后能重新使用此端口;
socket\_set\_option($this->master, SOL\_SOCKET, SO\_REUSEADDR, 1);
// 将IP和端口绑定在服务器socket上;
socket_bind($this->master, $host, $port);
// listen函数使主动连接套接口变为被连接套接口，使得此 socket 能被其他 socket 访问，从而实现服务器功能。后面的参数则是自定义的待处理socket的最大数目，并发高的情况下，这个值可以设置大一点，虽然它也受系统环境的约束。
socket\_listen($this->master, self::LISTEN\_SOCKET_NUM);

    服务器端逻辑：  
    ①获取全部的socket资源  

$sockets = array_column($this->sockets, 'resource'); // 获取到全部的 socket 资源

    ②：循环全部的socket并进行判断，如果是可读的服务器socket，则处理连接逻辑，如果可读的是其他已连接socket，则读取其数据，并处理应答逻辑。  

$sockets = array_column($this->sockets, 'resource'); // 获取到全部的 socket 资源
$read\_num = socket\_select($sockets, $write, $except, NULL);

foreach ($sockets as $socket) {
        // 如果可读的是服务器 socket, 则处理连接逻辑;            
        if ($socket == $this->master) {
            socket_accept($this->master);
            // socket_accept() 接受 请求 “正在 listen 的 socket（像我们的服务器 socket ）” 的连接, 并一个客户端 socket, 错误时返回 false;
             self::connect($client);
             continue;
            }
        // 如果可读的是其他已连接 socket ,则读取其数据,并处理应答逻辑
        } else {
            // 函数 socket_recv() 从 socket 中接受长度为 len 字节的数据，并保存在 $buffer 中。
            $bytes = @socket_recv($socket, $buffer, 2048, 0);

            if ($bytes < 9) {
                // 当客户端忽然中断时，服务器会接收到一个 8 字节长度的消息（由于其数据帧机制，8字节的消息我们认为它是客户端异常中断消息），服务器处理下线逻辑，并将其封装为消息广播出去
                $recv_msg = $this->disconnect($socket);
            } else {
                // 如果此客户端还未握手，执行握手逻辑
                if (!$this->sockets\[(int)$socket\]\['handshake'\]) {
                    self::handShake($socket, $buffer);
                    continue;
                } else {
                    $recv_msg = self::parse($buffer);
                }
            }

            // 广播消息
            $this->broadcast($msg);
        }
    }
}

        客户端：  

前端主要就通过JS调用websocket的监听：

// 创建一个 websocket 连接
var ws = new WebSocket("ws://127.0.0.1:8080");

// websocket 创建成功事件
ws.onopen = function () {
};

// websocket 接收到消息事件
ws.onmessage = function (e) {
    var msg = JSON.parse(e.data);
}

// websocket 错误事件
ws.onerror = function () {
};

demo地址：https://github.com/zhenbianshu/websocket