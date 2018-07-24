---
title: 创建Chrome桌面提醒
tags:
  - chrome
url: 411.html
id: 411
categories:
  - js/jquery/ajax
date: 2016-12-26 17:10:57
---

![blob.png](/ueditor/php/upload/image/20161226/1482742785258993.png "1482742785258993.png")

Chrome提供了桌面提醒功能，这个功能可以为用户提供更加丰富的信息，相对于badge

  

要使用桌面提醒功能，需要在Manifest中声明notifications权限：

"permissions": \["notifications"\]

创建桌面提醒非常容易，只需指定标题、内容和图片即可。

$(document).ready(function(){
	//getCurIp();
	chrome.extension.sendMessage({cmd: "getNewsArr"},function(response) {
		if(response.arr){
			var len=response.arr.length;
			for(var i=0;i<len;i++){
				$('body').append("<br>"+response.arr\[i\]+'<br>');
			}
		}
	});//
	$('input\[type=button\]').on('click',function(){
		var ntype=$(this).attr('id');
		chrome.extension.sendMessage({cmd: "notify",type:ntype},function(response) {			
		});
	})
	
});

chrome.windows.onRemoved.addListener(function (windowId){
		console.log('ddd');
});

function notify(ntype){
	var opt=null;
	switch(ntype){
		case 'basic':
			opt= {
			type: ntype,
			title: "桌面提醒",
			message: "中大奖了！",
			iconUrl: "icon128.png",
			
		  }
			break;
		case 'image':
			opt= {
			type: ntype,
			title: "桌面提醒",
			message: "中大奖了！",
			iconUrl: "icon128.png",
			imageUrl:"image.jpg",
		  }
		break;
		case 'list':
			opt= {
			type: ntype,
			title: "桌面提醒",
			message: "中大奖了！",
			iconUrl: "icon128.png",
			items: \[{ title: "1.", message: "下班了"},
					{ title: "2.", message: "吃饭了."},
					{ title: "3.", message: "中奖了."}\]
		  }
		break;
		case 'progress':
			opt= {
			type: ntype,
			title: "桌面提醒",
			message: "当前进度...",
			iconUrl: "icon128.png",			
			progress:80
			}
		break;
		}
		
	   chrome.notifications.create('',opt,function(id){
		
	   })
}

桌面提醒窗口提供四种事件：ondisplay、onerror、onclose和onclick.

除了用户主动关闭桌面提醒窗口外，还可以通过cancle方法自动关闭。下面的代码可以实现5秒后自动关闭窗口的效果。

setTimeout(function(){
    notification.cancel();
    },5000);

源码地址：  

https://pan.baidu.com/s/1hsJUFf2