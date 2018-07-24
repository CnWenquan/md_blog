
---
title: sublime python环境(代码自动补全，调试python脚本等插件)
tags:
  - mac
categories:
  - mac
date: 2018-07-12 21:09:33
---

安装完python和sublime后，为了方便脚本编写，需要安装几款常用的python插件，安装插件前，首先需要安装package install，安装方法如下：

### 简单的安装方法：

从菜单 View - Show Console 或者 ctrl + ~ 快捷键，调出 console。将以下 Python 代码粘贴进去并 enter 执行，不出意外即完成安装。以下提供 ST3 和 ST2 的安装代码：

**Sublime Text 3：**

```
import urllib.request,os; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); open(os.path.join(ipp, pf), 'wb').write(urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ','%20')).read())
```

**Sublime Text 2：**

```
import urllib2,os; pf='Package Control.sublime-package'; ipp = sublime.installed_packages_path(); os.makedirs( ipp ) if not os.path.exists(ipp) else None; urllib2.install_opener( urllib2.build_opener( urllib2.ProxyHandler( ))); open( os.path.join( ipp, pf), 'wb' ).write( urllib2.urlopen( 'http://sublime.wbond.net/' +pf.replace( ' ','%20' )).read()); print( 'Please restart Sublime Text to finish installation')
```

安装完成后，快捷键 Ctrl+Shift+P，输入 install 选中Install Package并回车，输入或选择你需要的插件回车就安装了（注意左下角的小文字变化，会提示安装成功）。 
![](/../images/WX20180712-104739.png)

**常用的插件有：**

**SublimeREPL** 可以用于运行和调试一些需要交互的程序（可以直接运行当前程序，或者使用ipython、pdb调试代码——-Tools—SublimeREPL—Python）。 
**SublimeCodeIntel** 可以支持代码的自动补全以及成员/方法提示等功能。 
**AutoPep8** 调整Python代码，使其符合PEP8的要求(ctrl+shift+8)。

编写简单脚本的话，这3个插件足矣！


