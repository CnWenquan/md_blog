---
title: Chrome App实现唯一ID
tags:
  - chrome
url: 415.html
id: 415
categories:
  - js/jquery/ajax
date: 2016-12-28 15:52:21
---

在manifest.json文件中的key这一项是打包文件使用的公钥，可以通过公钥来直接还原原始的扩展ID,具体原理可以参考下文。不过目前对于app,chrome完全禁止从官网之外的地方进行安装了。

  

转自：http://yunli.blog.51cto.com/831344/1211260

RSA[算法](http://lib.csdn.net/base/datastructure "算法与数据结构知识库")在Chrome的扩展程序中被用于对其.crx安装文件进行签名和生成ID。下面让我们逐一了解其技术细节。在继续之前，请注意一点，文中对于Web Store的技术细节是基于Chrome浏览器的角度通过一定的[测试](http://lib.csdn.net/base/softwaretest "软件测试知识库")而推测出来的，要彻底地了解Web Store的细节并不能仅通过本文。

  

扩展程序的安装文件有两种生成方式，一种方式是由扩展程序的开发者在开发的过程中生成，另一种是通过Google的Web Store（Google又称之为Gallery）提交由后台服务器生成。尽管两种方式有些差异，但我们可以从开发者生成这种方式中大概了解Web Store的行为。

  

扩展程序的开发者通过下图所列举的入口可以打开安装文件生成对话框。

[![150434670.png](/ueditor/php/upload/image/20161228/1482911434958841.png "1.png")](http://img1.51cto.com/attachment/201305/150434670.png)

  

在【打包扩展】对话框中（如下图），需要选择所需打包扩展程序的根目录，以及可选的私钥文件，这一文件与RSA算法有关，这是我们后面要讲的重点。

[![150529668.png](/ueditor/php/upload/image/20161228/1482911435496833.png "2.png")](http://img1.51cto.com/attachment/201305/150529668.png)

  

当我们不选择私钥文件进行打包时，Chrome会为我们生成一个后缀为.pem的私钥文件（文件的生成位置与所指定的扩展程序目录并列）。该文件中存放有RSA算法的私钥，且通过该私钥可以导出公钥。下图示例了如何通过openssl工具显示私钥文件和通过该文件导出公钥。

[![150630521.png](/ueditor/php/upload/image/20161228/1482911436443778.png "3.png")](http://img1.51cto.com/attachment/201305/150630521.png)

  

下面的活动图示例说明了Chrome在后台是如何打包一个扩展程序的。

[![150737142.png](/ueditor/php/upload/image/20161228/1482911437825204.png "4.png")](http://img1.51cto.com/attachment/201305/150737142.png)

  

所生成的.crx（二进制）文件格式如下图所示。需要特别说明的是，扩展程序的整个目录是以一个压缩包存放在.crx文件的最后面的，其中的Signature是通过Public Key所获得的签名，用于保证压缩文件内容的完整性。

[![154210722.png](/ueditor/php/upload/image/20161228/1482911438256553.png "5.png")](http://img1.51cto.com/attachment/201305/154210722.png)

  

至此相信读者已了解了RSA算法在.crx安装文件中的作用，接下来让我们了解扩展程序的ID（又被称为App ID）。Chrome中的每一个扩展程序都有一个ID值（下图示例了AdBlock扩展程序的ID值），该值是通过对Public Key进行Hash运算而获得的。

[![150917481.png](/ueditor/php/upload/image/20161228/1482911438213430.png "6.png")](http://img1.51cto.com/attachment/201305/150917481.png)  

  

下图示例说明了Chrome打包扩展程序时，如何在后台生成扩展程序的ID，其中也顺道说明了如何处理RSA密钥来源的。

[![151027393.png](/ueditor/php/upload/image/20161228/1482911439934355.png "7.png")](http://img1.51cto.com/attachment/201305/151027393.png)

  

请注意，由于ID是通过Public Key生成的，而Public Key又是由Private Key所导出的，如果想保证一个扩展程序的ID永远不变，则打包过程中必须使用同一个.pem文件。现实工作中，我们可以在第一次打包扩展程序时不指定.pem文件以便让Chrome为我们自动生成，然后在随后的所有打包动作中都指定该.pem文件。当然，如果不关心ID的不变性的话，可以让打包程序每次自动生成.pem文件，具体应如何使用读者可以根据自己的情景加以选择。

  

扩展程序一旦安装完成，需要通过Google Web Store的[Developer Dashboard](https://chrome.google.com/webstore/developer/dashboard)进行上传，以便用户能通过Web Store进行安装。与本地制作.crx文件不同的是，上传的文件必须是对扩展目录进行压缩而生成的.zip文件。需要注意的是，扩展程序的manifest.json中不能包含key和update_url两个域。通过研究Chrome安装好的扩展程序发现，其中的key域所存放的正是前面谈到的Public Key，且两个域都是服务器在后台所插入的。由于这种打包方式中不需指定.pem文件，因此我们可以推测打包服务器将为之自动生成Private Key。