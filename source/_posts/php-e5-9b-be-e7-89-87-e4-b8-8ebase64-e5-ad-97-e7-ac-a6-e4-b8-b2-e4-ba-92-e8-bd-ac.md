---
title: php 图片与Base64字符串互转
tags:
  - PHP
url: 353.html
id: 353
categories:
  - PHP
  - 未分类
date: 2016-11-11 17:02:46
---

1、base64转图片：

$base64 = "**********************************************************";
$url = explode(',' $base64);
$a = file\_put\_contents('./test.png', base64_decode($url\[1\]));//返回的是字节数
print_r($a);

2、图片转base64:

$str = file\_get\_contents('./test.png');
echo base64_encode($str);