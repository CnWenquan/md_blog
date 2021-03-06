---
title: PHP优化语法
tags:
  - php
categories:
  - php
date: 2018-06-13 19:09:33
---

## 1、字符串
### 1.1 少用正则表达式
能用PHP内部字符串操作函数的情况下，尽量用他们，不要用正则表达式，因为其效率高于正则。
str_replace函数要比preg_replace快得多，strtr函数又比str_replace来的快。
有没有你漏掉的好用的函数？
例如：strpbrk()、strncasecmp()、strpos()、strrpos()、stripos()、strripos()。
### 1.2 字符替换
如果需要转换的全是单个字符，用字符串作为strtr()函数完成替换，而不是数组：

```php
$addr = strtr($addr,"abcd","efgh") //建议
$addr = strtr($addr,['a':'b']) //不建议
```
效率提升10倍
str_replace字符替换比正则替换preg_replace快，但strtr比str_replace又快1/4.
另外，不要做无谓的替换，即使没有替换，str_replace也会为其参数分配内存。很慢！
用strpos先查找（非常快），看是否需要替换，如果需要，再替换。
如果需要替换，效率几乎相等，差别在0.1%左右。
如果不需要替换：用strpos快200%。
### 1.3 压缩大的字符串
使用gzcompress()和gzuncompress()对容量大的字符串进行压缩和解压，再存入和取出数据库。这种内置的函数使用gzip算法，能压缩字符串90%。
### 1.4 echo输出
echo字符串用逗号代替点连接符更快些。
虽然，echo是一种语言结构，不是真正的函数。
但是，它可以把逗号隔开的多个字符串当作“函数”参数传入，所以速度会更快。

```php
echo $str1,$str2 //速度快
echo $str1.$str2 //速度稍慢
```
### 1.5 尽量用单引号
PHP引擎允许使用单引号和双引号来封装字符串变量，但是它们的数独是有很大的差别的！
使用双引号的字符串会告诉PHP引擎，首先取读取字符串内容，查找其中的变量，并改为变量对应的值。
一般来说字符串是没有变量的，使用双引号会导致性能不佳。
最好使用字符串连接，而不是双引号字符串。

```php
$output = "This is a plain string";//不好的实践
$output = 'This is a plain string'; //好的实践

$type = "mixed"; //不好的实践
$output = "This is a $type string";

$type = 'mixed'; //好的实践
$output = 'This is a '.$type.' string';
```
### 1.6 使用isset代替strlen
在校验字符串长度时，我们第一想法会使用strlen()函数。
此函数执行起来相当快，因为它不做任何计算，只返回在zval结构（C的内置数据结构，用于存储PHP变量）中存储的已知字符串长度。

但是，由于strlen()是函数，多多少少会有些慢，因为函数调用会经过诸多步骤，如字母小写化、哈希查找，会跟随被调用的函数一起执行。

在某些情况下，可以使用isset()技巧加速执行你的代码。例如：

```php
if(strlen($foo)<5){
    echo "Foo is too short";
}

if(!isset($foo{5})){
    echo "Foo is too short";
}
```

### 1.7 用split分割字符串
在分割字符串时，split()要比explode()快


```php
split()

0.001813 - 0.002271 seconds (avg 0.002042 seconds)

explode()

0.001678 - 0.003626 seconds (avg 0.002652 seconds)
```
### 1.8 echo效率高于print
因为echo没有返回值，print返回一个整型。
注意：echo输出大字符串的时候，如果没有调整就会严重影响性能。
打开Apache的mod_deflate进行压缩，或者打开ob_start将内容放进缓冲区，可以改善性能问题。
## 2 语句
### 2.1 最好不用@
用@掩盖错误会降低脚本运行速度，并且在后台有很多额外操作。
用@比起不用，效率差距3倍。特别不要在循环中使用@。
在5次循环的测试中，即使时先用error_reporting(0)关掉错误，循环完后再打开，都比用@快。
### 2.2 避免使用魔术方法
对于__开头的函数就命名为魔术函数，它们都在特定的条件下触发。
这些魔术函数包括：__construct()、__get()、__call()、__autoload()等等。

以__autoload()为例，如果不能将类名与实际的磁盘文件对应起来，将不得不做大量的文件存在判断。

而判断文件存在需要磁盘I/O操作，众所周知，磁盘I/O操作的效率很低，因此这才是使得autoload机制效率降低的原因。

因此，在系统设计时，需要定义一套清晰的、将类名与实际磁盘文件映射的机制。

这个规则越简单越明确，__autoload()机制的效率就越高。

autoload机制并不是天然的效率低，只有滥用autoload、设计不好的自动装载函数，才会导致起效率的降低。

所以说，尽量避免使用__autoload 等魔术方法，有待商榷。
### 2.3 别再循环里用函数

例如：
`for($x = 0;$x<count($array);$x++){}`
这种写法在每次循环的时候都会调用count()函数，效率大大降低，建议这样：

```php
$len = count($array)
for($x =0;$x<$len;$x++){
}
```
让函数在循环外面一次获得循环次数。
### 2.4 使用三元运算符
在简单的判断语句中，三元运算符？：更简洁高效。
### 2.5 使用选择分支语句
switch、case好于使用多个if、else if语句，并且代码更加容易阅读和维护。
### 2.6 屏蔽敏感信息
使用error_reporting() 函数来预防潜在的敏感信息显示给用户。
理想的错误报告应该被完全禁用在php.ini文件里。
如果用的是共享虚拟主机，php.ini不能修改，最好添加error_reporting()函数。
放在每个脚本文件的第一行，或者用require_once()来加载，能有效的保护敏感的SQL查询和路径，在出错时不被显示。
### 2.7 不实用段标签
不要使用开始标志的缩写形式，你正在使用这样的符号吗，应该用完整的<?php 开始标签。
当然，如果是输出变量，用= $value ?>这种方式是鼓励的，可以是代码更加简洁。
### 2.8 纯PHP代码不加结束标记
如果文件内容是纯 PHP 代码，最好在文件末尾删除 PHP 结束标记?>。
这可以避免在 PHP 结束标记之后万一意外加入了空格或者换行符，会导致 PHP 开始输出这些空白，而脚本中此时并无输出的意图。
### 2.9 永远不要使用register_globals和magic quotes
这是两个很古老的功能，在当时（十年前）也许是一个好方法，但现在看来并非如此。
老版本的PHP在安装时会默认打开这两个功能，这会引起安全漏洞、编程错误及其他的问题。
如只有用户输入了数据时才会创建变量等。
PHP5.4.0开始这两个功能都被舍弃了，所以每个程序员都应该避免使用。
如果你过去的程序有使用这两项功能，那就尽快将其剔除吧。



