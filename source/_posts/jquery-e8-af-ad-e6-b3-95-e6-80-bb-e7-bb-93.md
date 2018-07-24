---
title: jQuery 语法总结（一）
url: 196.html
id: 196
categories:
  - js/jquery/ajax
date: 2016-08-20 22:50:31
tags:
---

1、隐藏函数hide()的运用  

    $(this).hide();    //隐藏当前的<html>标签内容

    $("#test").hide();    //隐藏id为test标签内容

    $(".test").hide();    //隐藏class为test标签内容

    $("p").hide();    //隐藏p标签内容

  

2、文档就绪文件：

$(document).ready(function(){
    --- jQuery functions go here ----
});

这是为了防止文档在完全加载（就绪）之前运行 jQuery 代码。

  

3、jQuery选择器：

$("p.info")    //选取所有class = "info"的<p>标签元素

$("p#demo")    //选取所有id = "demo"的<p>标签元素

$("\[href\]")    //选取所有带有href属性的元素

$("\[href = '#'\]")    //选取所有带有 href 值等于 "#" 的元素。

$("\[href != '#'\]")    //选取所有带有 href 值不等于 "#" 的元素。

$("\[href = '.jpg'\]")    //选取所有带有href值以'.jpg'结尾的元素。

$("p").css("background","red");    //把所有<p>标签的背景颜色改为红色

  

4、jQuery实现一个button的click事件的呆毛：

<html>
<head>
<script type="text/javascript" src="/jquery/jquery.js"></script>
<script type="text/javascript">
$(document).ready(function(){
$("button.hide").click(function(){
$("p").hide();
});

$("button.show").click(function(){
$("p").show();
});
});
</script>
</head>

<body>
<h2>This is a heading</h2>
<p>This is a paragraph.</p>
<p>This is another paragraph.</p>
<button class = "hide" type="button">Click me</button>
<button class = "show" type="button">show</button>
</body>
</html>

下面是 jQuery 中事件方法的一些例子：

Event 函数

绑定函数至

$(document).ready(function)

将函数绑定到文档的就绪事件（当文档完成加载时）

$(selector).click(function)

触发或将函数绑定到被选元素的点击事件

$(selector).dblclick(function)

触发或将函数绑定到被选元素的双击事件

$(selector).focus(function)

触发或将函数绑定到被选元素的获得焦点事件

$(selector).mouseover(function)

触发或将函数绑定到被选元素的鼠标悬停事件

  

5、jQuery切换显示与隐藏呆毛：

<script>     
$(document).ready(function(){    
$(".flip").click(function(){    
$(".panel").toggle();    
});    
});    
</script>    
<p class="flip">点击这里，隐藏/显示面板</p>    
<div class="panel"><p>一寸光阴一寸金，因此，我们为您提供快捷易懂的学习内容。</p>    
<p>在这里，您可以通过一种易懂的便利的模式获得您需要的任何知识。</p></div>    
</div>

  

6、淡入淡出：

    ①、淡入：

$("button").click(function(){
  $("#div1").fadeIn();
  $("#div2").fadeIn("slow");
  $("#div3").fadeIn(3000);
});

    ②、淡出：

$("button").click(function(){
  $("#div1").fadeOut();
  $("#div2").fadeOut("slow");
  $("#div3").fadeOut(3000);
});

    ③、切换淡入淡出：

$("button").click(function(){
  $("#div1").fadeToggle();
  $("#div2").fadeToggle("slow");
  $("#div3").fadeToggle(3000);
});

    ④、设置不透明度：

$("button").click(function(){
  $("#div1").fadeTo("slow",0.15);
  $("#div2").fadeTo("slow",0.4);
  $("#div3").fadeTo("slow",0.7);
});