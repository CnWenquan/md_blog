---
title: js与flash交互
url: 174.html
id: 174
categories:
  - PHP
  - 未分类
date: 2016-08-17 14:46:45
tags:
---

内容大纲：

　　1\. 在页面中嵌入SWF文件

　　2\. JS与SWF的通讯方式

　　3\. 常见问题与解决方法

**第一部分：在页面中嵌入SWF文件**

　　既然是**Flash与JS**的交互那肯定与他们的共同载体Web页面脱不了干系，也就是我们第一节的内容；

　　我们先来看一看Flash文件是以什么样的方式出现在HTML页面当中的。  
  
 

　　第一种：古老且不符合W3C标准的嵌入方式：

1

<object classid="clsid:d27cdb6e-ae6d-11cf-96b8-444553540000" codebase="[http://fpdownload.macromedia.com/pub/shockwave/cabs/flash/swflash.cab](http://fpdownload.macromedia.com/pub/shockwave/cabs/flash/swflash.cab)#version=7,0,0,0" width="550" height="400" id=“demo1" align="middle">

2

    <param name="allowScriptAccess" value="sameDomain" />

3

    <param name="movie" value="mymovie.swf" />

4

    <param name="quality" value="high" />

5

    <param name="bgcolor" value="#ffffff" />

6

    <embed src="mymovie.swf" quality="high" bgcolor="#ffffff" width="550"  height="400" name=“demo1" align="middle" allowScriptAccess="sameDomain"  type="application/x-shockwave-flash" pluginspage="[http://www.macromedia.com/go/getflashplayer](http://www.macromedia.com/go/getflashplayer)" />

7

</object>

　　把以上代码拷贝到你的html文档里面，修改一下flash文件的路径，如：<param name="movie" value="xxx/xxx.swf" />和 <embed src="xxx/xxx.swf"  就可以在你的站点里面显示Flash的内容了，根据需要你可能还会修改一下 width、height、bgColor 等待属性。

  

　　但是，当我们第一眼看到上班的代码时，是不是很晕啊，这是什么啊，这么长看上去好像有很多没有语意，而且有很多冗余的代码。的确这里面有很多让人不爽的地方，下面我们一一道来：

　　　　1）冗余的代码：

　　　　　　我们看到 width，height，quality，bgcolor align等等这些属性分别在 Object标签、param标签 和 embed 标签里面重复定义了两次同样的值。  
 

　　　　2）codebase 这个是什么意思，我需要么？

　　　　　　 Microsoft Internet Explorer 3.0 版和更高版本使用 CODEBASE 属性检测用户的计算机上安装了哪个版本的 Macromedia Flash Player ActiveX 控件。如果用户的版本比 CODEBASE 指定的版本旧, 那么 Internet Explorer 可自动从 CODEBASE 指定的位置下载并安装新版本的 Macromedia Flash Player。 -[来源](http://www.adobe.com/cn/support/flashplayer/ts/documents/tn_12685.htm)；  
　　　　　　通过上面的解释，我们知道它是来判断当前Flash文件需要的FlashPlayer的运行版本的。但是有更好的方法么？   
 

　　　　3）为什么还要在object里套一层 embed标签。

　　　　　　 OBJECT标签是用于windows平台的IE浏览器的，而EMBED是用于windows和Macintosh平台下的Netscape Navigator浏览器以及Macintosh平台下的IE浏览器。windows平台的IE利用Activex控件来播放flash而其它的浏览器则 使用Netscape插件技术来播放flash。  
　　　　　　也就是说embed是来告诉非 IE的浏览器 渲染当前的Flash文件所需要的信息。虽然embed解决了非IE浏览器渲染Flash文件的问题，但是它无法通过W3C的验证。  
　　　　为了解决这些不爽，我们找到了下面这种更好的解决方案。  
  
  

　　第二种嵌入方式：**符合W3C 标准的嵌入方式**：

1

<object width="550" height="400" type="application/x-shockwave-flash" data="swf/fp9.swf" id="test2" style="visibility: visible;">

2

    <param name="movie" value="swf/fp9.swf" />

3

    <param name="allowScriptAccess" value="always" />

4

    <param name="wmode" value="transparent" />

5

    <param name="allowFullscreen" value="true" />

6

    <param name="quality" value="high" />

7

    <param name="flashvars" value="text=这是flashvars传入的数据啊" />

8

</object>

　　相比上一段代码，这段的确减轻了我们眼睛的负担，没有了那么多多冗余代码，也符合W3C的标准了。细心的同学估计也看到了 flash文件的路径又被重复定义了两次，跟上面的情况一样，data是满足非IE， <param name="move" src='' />满足IE浏览器。

　　综合上面的两种方式，虽然都解决了在页面中嵌入Flash的问题，而且第二中方法看起来更优雅，冗余代码很少，但是它们都有一个共同的问题：**没有FlashPlayer检测；**

　　也就是说如果当前的浏览器里面没有安装Flash Player 或者 安装的浏览器版本不能满足要显示的Flash文件的最低的版本要求，就会导致浏览器不能正确的把Flash呈现出来。而此时屏幕前面的用户既没有看到正确 的内容，也没有得到错误提示（当然，如果他压根没有安装FlashPlayer,浏览器会自动检测，并提示他安装FlashPlayer插件。）。那么我 们怎么解决这个问题呢？为了解决这些问题，我们继续寻找。。。  
  
 

**　　第三种豪华版嵌入方式登场：swfobject**

　　我们先来了解一下SwfObject是个什么东西。

　　swfobject 是一个js类库，它封装了许多方法，用这些方法可以帮助你检查客户端的FlashPlayer环境和动、静态嵌入flash文件到页面中。  
　　方法简单，快捷。下面是它的优点：

*   　该模块中的JS脚本能够自动检测客户端机器上各种主流浏览器对Flash插件的支持情况。使得插入Flash媒体资源尽量简捷、安全。
    
*   　而且它非常符合搜索引擎优化的原则。能够避免您的HTML、XHTML中出现object、embed等非标准标签，从而符合更加标准。  
      
     
    

　　再来看看swfobjet的一个具体的例子：

1

<script type="text/javascript" src="swfobject.js"></script> 

2

<script type="text/javascript">

3

    swfobject.embedSWF("test.swf", "swfid", "300", "120", "9.0.0"); 

4

</script>

5

  

6

<div id="swfid">

　　是不是比上面的两种方法简单了许多。如果你只是嵌入一个Flash动画文件的画，上面的四句话都搞定了。

　　这样是不是很爽，代码量少了很多。。。

　　下面我们介绍一下SwfObject嵌入flash的详细方法：

01

<script type="text/javascript" src="swfobject.js"></script> 

02

<script type="text/javascript" language="javascript">

03

varflashvars= {

04

    nameA:"hello",

05

    nameB:"world"

06

}

07

varparams= {

08

    menu: "false",

09

    allowFullscreen: "false",

10

    allowScriptAccess: "always",

11

    bgcolor: "#ffffff",

12

    wmode: "window"

13

}

14

varattributes= {

15

    id:"test"

16

}

17

swfobject.embedSWF("swf/test.swf", "<span style="color: #ff9900;">altContent", "100%", "100%", "9.0.0", "swf/expressInstall.swf",flashvars,params,attributes);

18

</script>

19

  

20

<!\-\- 要被flash文件替换的标签位置 -->

21

<div id="<span style="color: #ff9900;">altContent">

22

    <h1>Flashvars Demo</h1><!-- 此处为该flash的相关名称或功能描述 -->

23

    

<a href="[http://www.adobe.com/go/getflashplayer](http://www.adobe.com/go/getflashplayer)">安装最新的FlashPlayer

24

  

25

<!\-\- /要被flash文件替换的标签位置 -->

　　flashvars　　是一个json对象，为flash传递一些初始化信息；  
　　params　　　设置flash的一些渲染模式和背景颜色，缩放模式，右键菜单、是否允许flash访问js里面的方法等  
　　attributes 　设置嵌入flash完成时object标签的id，name等属性

　　现在我们详细介绍一下swfobject.embedSWF方法，该方法共需要10个参数，下面是详细介绍：

　　swfUrl（String ，必须的）指定SWF的URL。  
　　id（String ，必须的）指定将会被Flash内容替换的HTML元素的id。  
　　width（String，必须的）指定SWF的宽。  
　　height（String，必须的）指定SWF的高。  
　　version（String，必须的）指定你发布的SWF对应的Flash Player版本（格式为：major.minor.release）。  
　　expressInstallSwfurl（String，可选）指定express install SWF的URL并激活Adobe express install。  
　　flashvars（ Object ,可选）用name:value对指定你的flashvars。  
　　params（Object ,可选）用name:value对指定你的嵌套object元素的params。  
　　Attributes（Object, 可选）用name:value对指定object的属性。  
　　callbackFn（JavaScript function, 可选）定义一个在执行embedSWF方法后，嵌入flash成功或失败后都可以回调的js 函数

　　对于不需要的参数可以省略，但是如果某个参数不需要，但是其后面需要设置某个具体的值的时候，请用 null 关键字占位，如：

　　swfobject.embedSWF("swf/test.swf", "altContent", "100%", "100%", "9.0.0",null, flashvars, params, attributes);

　　上面说了swfobject会自动判断客户端的FlashPlayer环境，在这里怎么没有体现呢？我们这就来介绍一下。swfobject检测客户端环境的方法体现在以下三个方面：

　　　　1）<div id="altContent">

　　　　上面的例子我们可以看到在这个div里面还有一些关于你要嵌入的Flash 的一些简单描述和一个现在FlashPlayer的链接地址。这个DIV很重要，它的作用就是在如果客户端没有安装FlashPlayer或者 FlashPlayer的版本不够高，这段标签将不被替换，以达到提示用户这部门内容要安装FlashPlayer才能正确显示。  
 

　　　　2）swfobject.embedSWF方法里面的version参数；

　　　　version参数告知了swfobject在满足那个Flahsplayer版本时才会替换上面的div标签，正确嵌入Flash文件，否则就不替换。

  
　　　　3）swfobject.embedSWF方法里面的 expressInstallSwfurl 参数

expressInstallSwfurl 参数是一个Adobe提供的用于自动升级FlashPlyaer的一个swf文件，当客户端安装了FlashPlayer 6+ 的版本，又达不到 verison参数描述的版本时，swfobject就会在页面里面显示这个expressinstall.swf文件，它会提醒你是否要自动升级 FlashPlayer。

　　另外，swfobject 还提供了两个检测FlashPlayer版本的方法：

　　　　swfobject.getFlashPlayerVersion()  
　　　　返回一个包含了已安装Flash Player主要版本（major:Number）、次要版本、minor:Number）、发行版本（release:Number）的JavaScript对象。  
  
　　　　  
　　　　swfobject.hasFlashPlayerVersion(versionStr)  
　　　　返回一个Boolean值，表明特定版本的Flash插件是否已被安装; 如：swfobject.hasFlashPlayerVersion(“9.0.124”)

　　　　到此，第一部分就介绍完了,关于swfobject的更多信息可以查阅这里：[http://code.google.com/p/swfobject/](http://code.google.com/p/swfobject/)

  
　　　　

**第二部分：JS与SWF的通讯方式 **

　　所谓通讯就是信息的交换，这里我们谈的是js与Flash之间的数据交换。

　　我们先来看一下**JS与Flash交换信息的三种方式。**

**　　　　1.通过Flashvars**

　　　　　　**Flashvars - Flash初始化信息的传递者，它由N组名值对组成**；当你的flash文件需要某些初始信息的时候就需要用到它。  
  
　　　　　　如果你使用的是前两种嵌入方法，应像这样：<param name="flashvars" value="text=这是flashvars传入的数据啊" /> 用 param 标签传递信息；  
  
　　　　　　如果你使用的是 swfobject 就是用上面提到的,定义一个flashvars的变量然后在调用swfobject.embedSWF方法时传入这个变量;  
  
　　　　　　或者上面的这两种方法都可以在你要嵌入的Flash文件地址后面加上问号传递 入："xxx.swf?name1=hello&name2=world ";

**　　　　2.JS调用Flash的方法，用参数传递信息给Flash或者得到Flash方法的返回信息**

　　　　　　Flash对象.flash方法名字(参数1，参数2, …);  
　　　　　　如：  
　　　　　　　　var flashObj = getFlashFun(“flashID”);  
　　　　　　　　var value = flashObj.flashFunName(args);

　　　　　　注意：  
　　　　　　1.参数个数必须跟Flash里面的参数个数一致  
　　　　　　2.非同域下需要得到Flash内部的允许 Security.allowDomain(… domains)

**　　　　3.Flash调用js方法得到js方法的返回信息 或 通过参数传递信息给JS  
  
　　　　　　**ExternalInterface.call(js的函数名称,  ”参数1”,  ”参数2”, …);

　　　　　　 如：

　　　　　　 var value:String = ExternalInterface.call(“jsFunName“,  ”argument1”,  ”argument2”);

  
　　　　　　注意非同域下设置 param 标签的allowScriptAccess属性的 value值为always，如：  
　　　　　　　　var params = {

　　　　　　　　　　allowScriptAccess: "always"  
　　　　　　　　}; 

　　　　　　　　或：

　　　　　　　　<param name="allowScriptAccess" value="always" />

  

**第三部分：常见问题分析与解决方法**

**　　1\. flash得不到正确的初始化参数**

　　　　–通过问号传递的参数被Flashvars方式传递的参数覆盖

**　　　  –**某些带"&"符号的flashvars值没有进行[encodeURIComponent](mk:@MSITStore:D:%5Cdhl%5Cstudy%5Cas%5Chandbook%5CAS3LCR_Flash_10.0_zh_CN.chm::/package.html)[](mk:@MSITStore:D:%5Cdhl%5Cstudy%5Cas%5Chandbook%5CAS3LCR_Flash_10.0_zh_CN.chm::/package.html) 编码  
  
  
　　**2. ****JS调用Flash方法失败**

　　　　–Flash没有初始化完成　　-延迟调用或者flash里面设置回调函数告知js flash文件初始化完成

　　　　–获取flash对象的方法浏览器兼容性问题　　\- 修改获取flash对象的js方法

　　　　–函数名不对应，或者Swf里面没有定义该方法

　　　　–Flash没有允许跨域调用其内部方法

　　　　–调用flash方法时参数数量不对应

　　　　–在遨游，TT等国内著名山寨版浏览器中刷新后无法调用Flash方法   
　　　　　　两个解决方案：

　　　　　　　　1. 在flash文件地址后面加随机值不让Flash文件缓存；  
　　　　　　　　2\. 在Flash里面延迟添加js的回调函数。

  
**　　3. Flash调用JS方法失败。**

　　　　–allowScriptAccess: “never“

　　　　–跨域时没有设置：allowScriptAccess: “always“

　　　　–FLASH调用JS的时候，JS对象还不存在  
  
  
  

　　**4. 弹层被Flash覆盖了!**

　　　　–设置 wmode的值为： opaque 或 transparent;

　　　　　　<param name=“wmode” value=“transparent” />

　　　　　　var params = { wmode: " transparent“   }; 

　　　　–或者把弹层也做成Flash的（这个可实施性不强。。。）

  

  

　　  
　　　**5\. Firefox下无法输入中文 **

　　　　–设置 wmode的值为： window;

  

　　　　在这里我们简单了解一下 wmode：　　　　

**　　　　window 模式**

　　　　默认情况下的显示模式，在这种模式下flash player有自己的窗口句柄，这就意味着flash影片是存在于Windows中的一个显示实例，并且是在浏览器核心显示窗口之上的，所以flash只 是貌似显示在浏览器中，但这也是flash最快最有效率的渲染模式。由于他是独立于浏览器的HTML渲染表面，这就导致默认显示方式下flash总是会遮 住位置与他重合的所有DHTML层。

但是大多数苹果电脑浏览器会允许DHTML层显示在flash之上，但当flash影片播放时会出现比较诡异的现象，比如DHTML层像被flash刮掉一块一样显示异常。  
 

**　　　　Opaque 模式**

　　　　这是一种无窗口模式，在这种情况下flash player没有自己的窗口句柄，这就需要浏览器需要告诉flash player在浏览器的渲染表面绘制的时间和位置。这时flash影片就不会在高于浏览器HTML渲染表面而是与其他元素一样在同一个页面上,因此你就可 以使用z-index值来控制DHTML元素是遮盖flash或者被遮盖。  
 

**　　　　Transparent 模式**

　　　　透明模式，在这种模式下flash player会将stage的背景色alpha值将为0并且只会绘制stage上真实可见的对象，同样你也可以使用z-index来控制flash影片的 深度值，但是与Opaque模式不同的是这样做会降低flash影片的回放效果，而且在9.0.115之前的flash player版本设置wmode=”opaque”或”transparent”会导致全屏模式失效。

了解了各种模式的实现方式和意义在以后的开发中就可以按照具体情况选择设置wmode属性的值了。

　　　　所以在没有弹层被遮盖的问题的时候，大家尽量设置wmode为window模式，已提高flash的渲染效率和一些诡异的bug