---
title: css3 img图片裁剪
tags:
  - img
url: 715.html
id: 715
categories:
  - js/jquery/ajax
date: 2018-01-08 17:10:07
---

<!DOCTYPE html>

<html>

<head>

<style>

.fill {object-fit: fill;} //铺满

.contain {object-fit: contain;} //按一个边压缩,保持横纵比

.cover {object-fit: cover;} //居中裁剪，保持横纵比，大小完全填充

.scale-down {object-fit: scale-down;} 内容大小按默认的填充，但内容小不会撑满

.none {object-fit: none;}

</style>

</head>

<body>

<h1>The object-fit Property</h1>

<h2>No object-fit:</h2>

<img src="paris.jpg" alt="Paris" style="width:200px;height:400px">

<h2>object-fit: fill (this is default):</h2>

<img class="fill" src="paris.jpg" alt="Paris" style="width:200px;height:400px">

<h2>object-fit: contain:</h2>

<img class="contain" src="paris.jpg" alt="Paris" style="width:200px;height:400px">

<h2>object-fit: cover:</h2>

<img class="cover" src="paris.jpg" alt="Paris" style="width:200px;height:400px">

<h2>object-fit: scale-down:</h2>

<img class="scale-down" src="paris.jpg" alt="Paris" style="width:200px;height:400px">

<h2>object-fit: none:</h2>

<img class="none" src="paris.jpg" alt="Paris" style="width:200px;height:400px">

</body>

</html>