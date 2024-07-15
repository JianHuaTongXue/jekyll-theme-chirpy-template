---
layout: post
title: Git和TortoiseGit（小乌龟）的安装及使用
categories: [Git]
tags: [Git, TortoiseGit, Github]
date: 2021-11-06 23:40:24.000000000 +08:00
---

## 安装流程

1. 先下载git，按照相应的系统，[下载Git](https://git-scm.com/downloads "下载Git"){:target="_blank"}，然后，一直next即可完成安装

2. 安装TortoiseGit小乌龟，[下载TortoiseGit小乌龟](https://tortoisegit.org/download/ "下载TortoiseGit小乌龟"){:target="_blank"}，同样的操作，只需要一直next即可完成安装，但是，需要注意的是必须先安装git，再安装git小乌龟。

3. 安装语言包，同样是在下载小乌龟的地方[下载小乌龟语言包](https://tortoisegit.org/download/ "下载小乌龟语言包"){:target="_blank"}，然后一直next即可，要先装完小乌龟再安装语言包。

	![](/assets/img/20211106/tortoisegit_language_packs.png){: .shadow}
	_TortoiseGit语言包_

4. 安装完语言包，文件夹空白处右键选择 TortoiseGit -> Setting，如下图：

	![](/assets/img/2021/1107/1107-git-1.png){: .shadow}
	_TortoiseGit设置语言_

5. 选择 General -> Language -> 中文(简体)(中国)，再点下面的确定，如下图：

	![](/assets/img/2021/1107/1107-git-2.png){: .shadow}
	_TortoiseGit选择简体中文_
	
## 使用教程

1. 空白文件夹右键，选择Git克隆，粘贴URL，目录会对应生成，点击确定，。

	![](/assets/img/2021/1107/1107-git-3.png){: .shadow}
	_TortoiseGit克隆项目_
	
2. 然后就是等待拉取，拉取成功，如下所示：

	![](/assets/img/2021/1107/1107-git-4.png){: .shadow}
	_TortoiseGit克隆成功_
	
3. 修改项目中的文件，然后右键 Git提交，输入日志信息后，点击提交，这次提交是提交本地仓库，最后确定完，先同步下，再推送到远程仓库。

	![](/assets/img/2021/1107/1107-git-5.png){: .shadow}
	_TortoiseGit提交修改到本地仓库_


## 参考资料:
[https://www.jianshu.com/p/33108325fc87](https://www.jianshu.com/p/33108325fc87){:target="_blank"}

[https://www.cnblogs.com/gopark/p/8145559.html](https://www.cnblogs.com/gopark/p/8145559.html){:target="_blank"}