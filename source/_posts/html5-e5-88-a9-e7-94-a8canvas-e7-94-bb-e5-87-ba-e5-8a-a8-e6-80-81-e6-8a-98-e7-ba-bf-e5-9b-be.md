---
title: HTML5 利用Canvas画出动态折线图
url: 500.html
id: 500
categories:
  - js/jquery/ajax
date: 2017-05-18 15:57:30
tags:
---

HTML5 Canvas标签用于绘制图像，不过Canvas元素本身并没有绘制能力（它仅仅是图形的容器）您必须使用叫本来完成实际的绘图任务。  

* * *

直接上代码：  

<canvas id="myCanvas" width="300" height="50" style="border:1px solid #d3d3d3;">
您的浏览器不支持该插件！！
</canvas>

<script>

var res = \[\];



function setData(item){
	if(res.length>10){
		res.shift();
	}
	res.push(item);
	return res;
}
function start_draw(){
	
	var c=document.getElementById("myCanvas");
	//getContext()方法可返回一个对象，该对象提供了用于在画布上绘图的方法和属性。
	var ctx=c.getContext("2d");
	ctx.clearRect(0,0,c.width,c.height);//清除画布
	ctx.beginPath();//起始一条路径，或重置当前路径              
	ctx.lineWidth="1";//设置线条宽度
	ctx.strokeStyle="yellow";  // 绿色路径
	var item = Math.random() * 50;
	res = setData(item);
	var len = res.length;
	$.each(res, function(i,val) {
		if(i==0){
			ctx.moveTo(i*c.width/10,res\[i\]);
		}else{
			ctx.lineTo(i*c.width/10,res\[i\]);
		}
	});
	
	ctx.stroke();
	
	
}

setInterval("start_draw()",3000);

</script>

* * *

效果图（动态变动的哦！）：  

![Canvas.png](/ueditor/php/upload/image/20170518/1495094185517799.png "1495094185517799.png")