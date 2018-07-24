---
title: json rpc远程调用服务
url: 400.html
id: 400
categories:
  - js/jquery/ajax
date: 2016-12-19 18:04:10
tags:
---

json rpc(remote protocol call)是一种以json为协议的远程调用服务，具有开发调试简单，多平台通用的特性。

  

Git地址：

[https://github.com/datagraph/jquery-jsonrpc](https://github.com/datagraph/jquery-jsonrpc)  

  

  
代码：

$(document).ready(function(){
                    $.jsonRPC.setup({
                        endPoint: 'http://10.110.1.82:4000/jsonrpc',
                        namespace: ''
                    });
                    $('#get_data').click(function(){
//                        $.jsonRPC.request('gettask', {
//                            params: {"remote": "", "local": ""},
//                            jsonrpc: '2.0',
//                            id: 0,
//                            success: function(result) {
//                                console.log(result);
//                            },
//                            error: function(result) {
//                              console.log(result);
//                            }
//                          });


                        $.jsonRPC.batchRequest(\[
                            {
                                method:'gettask',
                                params:{"remote": "", "local": ""},
                                jsonrpc:'2.0',
                                id:0
                            },{
                                method:'user',
                                params:{"username":"", "password":""},
                                id:1
                            }
                        \],{
                            success:function(result){
                                console.log(result);
                            }
                        });
                    });

//            });
            });