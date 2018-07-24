---
title: php与js删除array中的空值
url: 537.html
id: 537
categories:
  - js/jquery/ajax
date: 2017-06-30 15:53:36
tags:
---

php中删除空值的方法：array_filter($array);  

<?php
    $array = ('a' => "abc", 'b' => "bcd",'c' =>"cde",'d' =>"def",'e'=>"");
    array_filter($array);
    echo "<pre>";
    print_r($array);
?>
结果：
Array (
     \[a\] => abc
     \[b\] => bcd
     \[c\] => cde
    \[d\] => def
   )

js中也想删除怎么办呢，感觉语言是互通的，于是查找资料发现确实js也有 Array filter()方法  

  

给的例子是筛选出大于某个数的数组：

var ages = \[32, 33, 16, 40\];

function checkAdult(age) {
    return age >= 18;
}

  
function myFunction() {
    
  document.getElementById("demo").innerHTML = ages.filter(checkAdult);
}

稍加改动就成为过滤空值的方法：  

var ages = \[32, 33, ,16, 40\];

function checkAdult(age) {
    return age != '';
}

  
function myFunction() {
    
  document.getElementById("demo").innerHTML = ages.filter(checkAdult);
}

so eassy!