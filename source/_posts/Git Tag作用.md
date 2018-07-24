---
title: Git Tag作用
tags:
  - 服务器
categories:
  - server
date: 2018-06-21 19:09:33
---

#### 个人理解
git tag和commit的commit-sha1有些类似，其实就是给当前的版本做个标记，以便回退到此版本。如果使用commit-sha1，大家都记不住sha1码，所以用tag标签作为标记。

发布一个版本的时候，我们通常先在版本库中打一个标签（tag）
#### 基本命令
1、git tag <name> 打一个新标签，标签都存储在本地，不会自动推送到远程
2、git tag：查看所有的tag标签
3、在历史提交上打tag：
查看commit id，使用commit id打tag




