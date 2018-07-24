---
title: Swig 介绍
tags:
  - swig
url: 618.html
id: 618
categories:
  - 服务器
date: 2017-10-22 21:32:04
---

`    C` 和 `C++` 被公认为（理当如此）创建高性能代码的首选平台。对开发人员的一个常见要求是向脚本语言接口公开 `C/C++` 代码，这正是 Simplified Wrapper and Interface Generator (SWIG) 的用武之地。SWIG 允许您向广泛的脚本语言公开 `C/C++` 代码，包括 Ruby、Perl、Tcl 和 Python。本文使用 Ruby 作为公开 `C/C++` 功能的首选脚本接口。要理解本文，您必须具备 `C/C++` 与 Ruby 方面的相应知识。

SWIG 是一款不错的工具，可适合多种场景，其中包括：

*   向 `C/C++` 代码提供一个脚本接口，使用户更容易使用
    
*   向您的 Ruby 代码添加扩展或将现有的模块替换为高性能的替代模块
    
*   提供使用脚本环境对代码执行单元和集成测试的能力
    
*   使用 TK 开发一个图形用户接口并将它与 `C/C++` 后端集成
    

此外，与 GNU Debugger 每次都需触发相比，SWIG 要容易调试得多。

  

        SWIG 接口文件语法是 `C` 的一个超集。SWIG 通过一个定制 `C` 预处理器处理它的输入文件。此外，接口文件中的 SWIG 操作通过一个百分比符号 (`%`) 后跟的特殊的指令（`%module`、`%constant` 等）来控制。SWIG 接口还允许您定义以 `%{` 开头和以 `%}` 结束的信息块。`%{` 和 `%}` 之间的所有内容会原封不动地复制到生成的包装器文件中。

  

        SWIG 接口文件必须以 `%module` 声明开头，例如 `%module _module-name_`，其中 _module-name_ 是目标语言扩展模块的名称。如果目标语言是 Ruby，这类似于创建一个 Ruby 模块。可以提供命令行选项 `–module _module-name-modified_` 来改写模块名称