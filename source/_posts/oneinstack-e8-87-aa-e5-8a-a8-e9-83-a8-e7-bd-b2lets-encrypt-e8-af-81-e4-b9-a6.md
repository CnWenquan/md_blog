---
title: OneinStack自动部署Let’s Encrypt证书
tags:
  - linux
url: 423.html
id: 423
categories:
  - 服务器
date: 2017-01-04 15:50:24
---

Let's Encrypt是一个由电子前哨基金会、Mozilla基金会、Akamai、密歇根大学、思科联合发起的一个项目。它旨在为站长提供一个免费的、完全自动化的证书申请过程，从而让整个互联网都能享受到HTTPS加密。Let’s Encrypt的证书申请过程非常简单、安全、快速、自动化并且免费。Let’s Encrypt是一个中间CA，它的CA证书由IdenTrust签发。IdenTrust是一个Root CA，受到所有主流浏览器的信任。从2015年10月后，Let’s Encrypt的中间CA证书被chrome、Firefox、Microsoft Edge、Safari和Opera所信任。

![](/ueditor/php/upload/image/20170104/1483516159607150.png)

最近官方做了调整，简化了获取证书的难度，并将项目名改为了certbot，以前叫letsencrypt。certbot可以自动化的申请，安装和更新证书。

[OneinStack](https://blog.linuxeye.com/wp-content/themes/begin/inc/go.php?url=https://oneinstack.com)已经内置Let's Encrypt，**vhost.sh绑定域名时自动申请、绑定、自动续期（默认90天）一步到位**，教程如下：

#### 安装

1.  wget http://mirrors.linuxeye.com/oneinstack.tar.gz
    
2.  tar xzf oneinstack.tar.gz
    
3.  cd oneinstack
    
4.  ./addons.sh
    

如下图：

![](https://static.oneinstack.com/images/addons.png)

出现绿色Let's Encrypt client install successfully!即表示certbot安装成功，如下图：

![](/ueditor/php/upload/image/20170104/1483516160641564.png)

### 使用证书

vhost.sh绑定域名ssl选择y，Let's Encrypt选择y

1.  ./vhost.sh
    

![](/ueditor/php/upload/image/20170104/1483516161680458.png)

#### 最后结果：

![blob.png](/ueditor/php/upload/image/20170104/1483516307259654.png "1483516307259654.png")

#### 注意事项

*   最新版oneinstack关闭Congratulations，默认挑过即成功
    
*   绑定定域名后会自动在crontab添加相应自动续期脚本
    
*   同一个ip在3小时内最多申请10个域名的证书
    
*   同一个根域名在七天内，只能注册5个证书。同一个子域名注册或更新都是要计算次数的