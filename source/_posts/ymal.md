---
title: YMAL
tags:
  - ymal
url: 754.html
id: 754
categories:
  - 服务器
date: 2018-02-12 17:05:50
---

一 简介
====

### 定位

`YMAL`是专门用来编写配置文件的语言，简洁又方便。

### 基本语法规则

*   大小写敏感
    
*   使用缩进表示层级关系
    
*   缩进时不允许使用`Tab`，只允许使用空格
    
*   缩进的空格数目不重要，只要相同层级元素对齐即可
    

### 注释

`#`号 表示注释，从该符号一直到行结尾都会被解析器忽略

### 支持的数据结构

1.  对象：键值对的集合，又称映射（mapping）/哈希（hash）/字典（dictionary）
    
2.  数组：一组按次序排列的值，又称为序列（sequence）/列表（list）
    
3.  纯量（scalars）：单个的/不可再分的值
    

二 对象
====

对象是一组键值对

animal: pets

转为JavaScript格式如下：

{animal: 'pets'}

Ymal也允许另一种写法，将所有键值对写成一个行内对象

hash: {name: Steven, foo: bar}

转为JavaScript格式如下：

{hash: {name: 'Steven', foo: 'bar'}}

三 数组
====

一组连词线开头的行，构成一个数组

- Cat
- Dog
- Goldfish

转为JavaScript格式如下：

\['Cat', 'Dog', 'Goldfish'\]

数组也可以采用行内表示法

animal: \[Cat, Dog\]

转为JavaScript格式如下：  
{animal: \['Cat', 'Dog'\]}

四 符合结构
======

对象和数组可以结合使用，形成复合结构

languages:  - Ruby
  - Perl
  - Pythonwebsites:  Ruby: ruby-lang.org
  Python: python.org
  Perl: use.perl.org

转为JavaScript格式如下：

{languages: \['Ruby', 'Perl', 'Python'\]},
{websites:
  {
    Ruby: 'ruby-lang.org',
    Python: 'python.org',
    Perl: 'use.perl.org'
  }
}

五 纯量
====

纯量是最基本的、不可再分的值。以下数据类型都属于 JavaScript 的纯量

- 字符串
- 布尔值
- 整数
- 浮点数
- null- 时间
- 日期

六 字符串
=====

字符串是最常见，也是最复杂的一种数据类型。

str: 这是一行字符串

转为JavaScript格式如下：

{str: "这是一行字符串"}

如果字符串之中包含特殊字符或者空格，需要使用引号

str: '内容 : 这是一行字符串'

转为JavaScript格式如下：

{str: '内容 : 这是一行字符串'}

七 引用
====

锚点`&`和别名`*`， 可以用来引用

defaults: &defaults
  adapter: postgres
  host: localhost

development:
  database: myapp_development
  <<: *defaultstest:
  database: myapp_test
  <<: *defaults

等同于

defaults:
  adapter:  postgres
  host:     localhost

development:
  database: myapp_development
  adapter:  postgres
  host:     localhosttest:
  database: myapp_test
  adapter:  postgres
  host:     localhost

解释

*   `&` 用来建立锚点
    
*   `<<` 表示合并到当前数据
    
*   `*` 用来引用锚点
    

一个简单的例子：

- &showell Steven
- Clark
- Brian
- Oren
- *showell

转为 JavaScript 代码如下。

\[ 'Steve', 'Clark', 'Brian', 'Oren', 'Steve' \]