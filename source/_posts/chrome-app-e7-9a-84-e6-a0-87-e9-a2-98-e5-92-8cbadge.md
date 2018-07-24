---
title: Chrome App的标题和badge
tags:
  - chrome
url: 409.html
id: 409
categories:
  - js/jquery/ajax
date: 2016-12-26 16:02:02
---

将鼠标移至扩展图标上，片刻后所显示的文字就是扩展的标题。

在Manifest中，`browser_action`的`default_title`属性可以设置扩展的默认标题，比如如下的例子：

"browser_action": {
    "default_title": "Extension Title"}

在这个扩展中，默认标题就是“Extension Title”。还可以用JavaScript来动态更改扩展的标题，方法如下：

chrome.browserAction.setTitle({title: 'This is a new title'});

标题不仅仅只是给出扩展的名称，有时它能为用户提供更多的信息。比如一款聊天客户端的标题，可以动态地显示当前登录的帐户信息，如号码和登录状态等。所以如果能合理使用好扩展的标题，会给用户带来更好的体验。

标题我们已经清楚了，那么什么是badge呢？我们来看一幅图：

![enter image description here](http://www.ituring.com.cn/download/01YvzLOdJ5uQ)  
_扩展的标题和badge_

上图中，标有“Extension Title”的地方就是扩展标题，而标有“Badg”的地方就是badge。Badge是扩展为用户提供有限信息的另外一种方法，这种方法较标题优越的地方是它可以一直显示，其缺点是只能显示大约4字节长度的信息，这就是为什么上例中显示的是“Badg”而不是“Badge”。

目前来看使用badge比较典型的应用是音乐播放器，它们使用badge显示当前音乐播放的时间；另一些内容类的应用，如邮件、微博、RSS阅读器等，则显示未读条目。无论你打算用badge显示何种信息，请记住它只能显示4字节长度的内容。对于内容类的扩展，当用户未读条目足够多时，一般采用的解决方法是显示“999+”。

Badge目前只能够通过JavaScript设定显示的内容，同时Chrome还提供了更改badge背景的方法。如果不定义badge的背景颜色，默认将使用红色，就是上图显示的那样。

下面的代码显示了一个背景颜色为蓝色，内容为“Dog”的badge：

chrome.browserAction.setBadgeBackgroundColor({color: '#0000FF'});chrome.browserAction.setBadgeText({text: 'Dog'});

对于背景颜色的设定，设定值可以是十六进制的字符串颜色码，如`#FF0000`代表颜色；也可以是rgba格式的数组，但需要注意的是其中的alpha变量的取值范围同样为0-255，这与CSS有所区别。

下面的例子使用rgba的定义方式，将背景设置为50%透明度的绿色：

chrome.browserAction.setBadgeBackgroundColor({color: \[0, 255, 0, 128\]});

最后需要注意的一点，就是badge目前还不支持更改文字的颜色——始终是白色，所以应避免使用浅颜色作为背景。