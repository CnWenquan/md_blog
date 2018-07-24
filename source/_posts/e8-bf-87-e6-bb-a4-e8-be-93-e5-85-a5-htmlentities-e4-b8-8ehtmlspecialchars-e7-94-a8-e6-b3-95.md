---
title: 过滤输入  htmlentities与htmlspecialchars用法
tags:
  - PHP
url: 467.html
id: 467
categories:
  - PHP
date: 2017-03-07 12:31:13
---

过滤输入(即来自所列数据源中的任何数据)是指，转义或删除不安全的字符。在数据到达应用的存储层之前，一定要过滤输入数据。这是第一道防线。假如网站的评论表单接收html，默认情况下  

访客可以毫无阻拦地在评论中加入恶意的<script>标签，如下标示：

<p>
    我的测试
</p>
<script>
    alert(123)
</script>

  上面例子。如果不过滤这个评论，恶意代码会存入数据库，然后再网站的标记中渲染。

  HTML

  我们可以使用htmlentities或者htmlspecialchars函数来过滤html，把特殊字符转换成对应的html实体。

  htmlentities这个函数转换所有含有对应“html实体”的特殊字符，比如货币表示符号欧元英镑等、版权符号等，htmlspecialchars 只是把某些特殊的字符转义了， & " ' < >

  这2个函数比较傻，默认是不会转义单引号的

$str='<a href="test.html">\\'测试页面\\'</a><script>alert(213)</script>'; 

//并没有转义单引号
echo $str;
echo "<hr/>".PHP_EOL;
echo htmlentities($str);
echo "<hr/>".PHP_EOL;
echo htmlspecialchars($str);

需要设置第2个参数 ENT_QUOTES，具体可以看php手册

echo htmlentities($str,ENT_QUOTES,'UTF-8'); //单引号也转义
echo "<hr/>".PHP_EOL;
echo htmlspecialchars($str,ENT_QUOTES,'UTF-8');//单引号也转义

 以上例子并不能区别出htmlentities和htmlspecialchars ，下面换上一些特殊的字符，如欧元等。htmlentities将会对此转义，htmlspecialchars却不会

echo htmlentities('€ <>"').PHP_EOL;
echo "<hr/>".PHP_EOL;
echo htmlspecialchars('€ <>"').PHP_EOL; //€没有转义

结论：做一般表单提交的时候完全可以用strip_tags去除html标签，如果涉及到富文本编辑器需要保留html标签，可以用htmlspecialchars对提交数据进行过滤。