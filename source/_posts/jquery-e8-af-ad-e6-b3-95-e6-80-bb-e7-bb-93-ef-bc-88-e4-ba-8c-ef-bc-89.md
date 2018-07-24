---
title: jQuery 语法总结（二）
url: 198.html
id: 198
categories:
  - js/jquery/ajax
date: 2016-08-21 10:32:51
tags:
---

1、滑动效果：  

    ①、划出：

$("#flip").click(function(){
  $("#panel").slideDown();
});

    ②、划入：  

$("#flip").click(function(){
  $("#panel").slideUp();
});

    ③、划入划出切换：  

$("#flip").click(function(){
  $("#panel").slideToggle();
});

  

2、jQuery方法链接：  

下面的代码把css，slideUp,slideDown链接一起。"p1"元素首先会变为红色，然后向上滑动，最后向下滑动。  

$("#p1").css("color","red")
    .slideUp(2000)
    .slideDown(2000);

  

3、获取内容（先copy一份呆毛）：

<!DOCTYPE html>
<html>
<head>
<script src="/jquery/jquery-1.11.1.min.js"></script>
<script>
$(document).ready(function(){
  $("#btn1").click(function(){
    alert("Text: " + $("#test").text());
  });
  $("#btn2").click(function(){
    alert("HTML: " + $("#test").html());
  });
  $("#btn3").click(function(){
      alert("Value: " + $("#test").val());
  });
  $("btn4").click(function(){
      alert("attr: " + $("#w3s").attr("href"));
  });
});
</script>
</head>

<body>
<p id="test">这是段落中的<b>粗体</b>文本。</p>
<p>姓名：<input type="text" id="test" value="米老鼠"></p>
<p><a href="http://www.w3school.com.cn" id="w3s">W3School.com.cn</a></p>
<button id="btn1">显示文本</button>
<button id="btn2">显示 HTML</button>
<button id="btn3">显示 Value</button>
<button id="btn4">显示 属性</button>
</body>

</html>

    ①：获取文本  

$("#test").text();

    ②：获取Html  

$("#test").html();

    ③：获取input的Value值：  

$("test").val();

    ④：获取属性值：  

$("test").attr("href");

  

4、有获取就有设置：（copy一份呆毛）

<!DOCTYPE html>
<html>
<head>
<script src="/jquery/jquery-1.11.1.min.js"></script>
<script>
$(document).ready(function(){
  $("#btn1").click(function(){
    $("#test1").text("Hello world!");
  });
  $("#btn2").click(function(){
    $("#test2").html("<b>Hello world!</b>");
  });
  $("#btn3").click(function(){
    $("#test3").val("Dolly Duck");
  });
  $("#btn4").click(function(){
    $("w3c").attr("href","http://www.wenquanpage.com");
  });
});
</script>
</head>

<body>
<p id="test1">这是段落。</p>
<p id="test2">这是另一个段落。</p>
<p>Input field: <input type="text" id="test3" value="Mickey Mouse"></p>
<p><a href="http://www.w3school.com.cn" id="w3s">W3School.com.cn</a></p>
<button id="btn1">设置文本</button>
<button id="btn2">设置 HTML</button>
<button id="btn3">设置值</button>
<button id="btn4">改变href值</button>
</body>
</html>

设置text、html、val、attr与获取它们的值得方法与第三条中的方法一一对应。

  

5、添加元素：  

 ①：append方法：在被选元素的结尾插入（它是一种在所选标签内添加内容的方法）  

$("p").append("Some appended text.");

 ②：prepend方法在被选元素的开头插入内容（它与append大致一样，它在开头插入，append在结尾插入，仅此而已）  

$("p").prepend("Some prepended text.");

 ③：after()和before()方法：

   after方法在被选元素之后插入内容，before方法在被选元素之前插入内容。只是在被选元素外进行操作，不会改变被选元素内的内容。

$("img").after("Some text after");

$("img").before("Some text before");

6、删除元素  

  ①：remove方法：删除被选元素和其子元素，过滤需要删除的元素

$("#div1").remove();

下面的例子是删除掉class为italic中所有的<p>元素
$("p").remove(".italic");

  ②：empty方法：删除被选元素的子元素

$("#div1").empty();