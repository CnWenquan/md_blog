---
title: webuploader遇到的坑
url: 392.html
id: 392
categories:
  - js/jquery/ajax
date: 2016-12-07 17:49:14
tags:
---

1、描述：一个界面中需要上传文件，但设置了auto: 'true',选择后直接上传，遇到的问题是，上传后要是错误文件，无法重新上传，对比上传前后div样式，无任何变化，分析应该不是用的bootstrap摸态框问题，最后通过各方面调试与查资料，应该是webuploader上传成功后保存了一个文件列表，清除的办法：

在上传成功后调用：

        uploader.reset();