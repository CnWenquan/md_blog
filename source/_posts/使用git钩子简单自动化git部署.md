---
title: 使用git钩子简单自动化git部署
date: 2018-06-09 14:34:31
tags: server
categories:
    - server
---
## 运行机制
假如把您正在开发的本地工作副本比喻为主分支，大多数情况下，人们会将代码推送到远程服务器，比如github或gitlab，然后将其拖到生产服务器上。

但是，在这里，我们添加一个“裸”的git仓库，我们在生产服务器上创建并直接向我们的分支直接发送该服务器。此git存储库使用‘git-hook’将推送时间作用于您的服务器上的部署目录。这样就节省了中间操作者。

这样的好处是节省了中间人，具有加密通信的高安全性（使用ssh密钥，只有经过授权的人员才能访问服务器）以及在部署时使用.sh脚本的高灵活性。

## 流程介绍
1. 创建一个文件夹以部署到生产服务器
2. 在制作服务器上添加一个裸存储库
3. 将post-recrive钩子脚本添加到裸存储库（给权限可执行）
4. 将刚创建在生产服务器上的远程存储库添加到本地存储库
5. push到生产服务器，测试一下

### 1、创建要部署到的文件夹
`$mkdir ~/deploy-folder`

### 2、在制作服务器上添加一个裸存储库
`$ git init --bare ~project.git`

### 3、添加接收钩子
当从本地机器推送完成并移动文件到位时，执行该script。它在project.git/hooks/中进行记录，并被命名为“post-receive”.打开hooks文件夹你可以看到有post-receive.sample文件，你可以`copy post-receive.sample post-receive`，然后你可以使用vim编辑它。编辑内容如下：

```
#!/bin/bash
TRAGET="/home/deploy-folder"
GIT_DIR="/home/www.git"
BRANCH="master"

while read oldrev newrev ref
do
	# only checking out the master (or whatever branch you would like to deploy)
	if [[ $ref = refs/heads/$BRANCH ]];
	then
		echo "Ref $ref received. Deploying ${BRANCH} branch to production..."
		git --work-tree=$TRAGET --git-dir=$GIT_DIR checkout -f
	else
		echo "Ref $ref received. Doing nothing: only the ${BRANCH} branch may be deployed on this server."
	fi
done
```

### 4、添加到本地仓库
`$ cd ~/path/to/working-copy/ $ git remote add production demo@yourserver.com:project.git `

### 5、推送到生产服务器
`git push production master`
正常推送说明搭建成功


