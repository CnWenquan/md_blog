---
title: hexo generate 处理异常问题
date: 2018-06-08 11:46:31
tags: hexo
categories:
    - hexo
---

### 文件编辑权限问题
使用hexo new ''创建文件后，编辑会提示权限不足：

```
INFO  Created: /document/github-blog/source/_posts/1.md
FATAL Something's wrong. Maybe you can find the solution here: http://hexo.io/docs/troubleshooting.html
Error: EACCES: permission denied, open '/document/github-blog/db.json'
    at Error (native)
FATAL EACCES: permission denied, open '/document/github-blog/db.json'
Error: EACCES: permission denied, open '/document/github-blog/db.json'
    at Error (native)
```
可以通过chown将db.json文件的权限更改为当前用户：
`sudo chown wenquan:staff /doc/blog/db.json`
### wordpress迁移hexo时hexo geberate发生错误，错误信息如下：

```
$ hexo generate
INFO  Start processing
FATAL Something's wrong. Maybe you can find the solution here: http://hexo.io/docs/troubleshooting.html
Template render error: (unknown path)
  Error: expected end of comment, got end of file

```
问题原因是文章中出现了：
{&#35; 或&#35;}字符
首先想到用 Markdown 转义字符 \ 进行转义，但问题依旧。

hexo generate 目标是将 .md 文件内容转为 html 文件，html 可以对字符编码。从网上查了下 &#35;的 html 编码为 `&#35;` 替换后问题解决。


