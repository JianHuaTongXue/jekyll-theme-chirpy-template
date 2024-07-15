---
layout: post
title: Win10中shift+鼠标右键调出cmd窗口
categories: [Window10]
tags: [Window10, CMD]
date: 2021-11-07 1:00:00.000000000 +08:00
---

## 痛点

使用Win10后，之前能按shift+鼠标右键时，菜单栏里面的cmd选项不见了，取而代之的是Powershell窗口，用习惯了cmd后，真的不习惯Powershell。


## 流程

1. Win + R或者鼠标右键Window打开运行窗口

	![](/assets/img/2021/1107/1107-cmd-1.png){: .shadow}
	_运行窗口_

2. 输入 `regedit` 确定后打开注册表编辑器

	![](/assets/img/2021/1107/1107-cmd-2.png){: .shadow}
	_运行窗口输入regedit_
	
3. 在注册表中定位到：\HKEY_CLASSES_ROOT\Directory\Background\shell\cmd，将右侧的`HideBasedOnVelocityId`重命名为`ShowBasedOnVelocityId`

>注意事项：修改的时候有可能会遇到权限不足的情况，无法重命名成功。

## 权限修改

1. 右键上面定位到的cmd文件，选择权限。

2. 权限界面选择用户名，然后勾选完全控制。

	![](/assets/img/2021/1107/1107-cmd-3.png){: .shadow}
	_权限窗口_
	
3. 在高级面板中：点击顶部的`更改所有者，输入要选择的对象名称处输入当前账号名称`，譬如输入xxx，再点`检查名称`并确定。回到上级菜单后继续确定即可，然后确定，再重名命就能成功。

	![](/assets/img/2021/1107/1107-cmd-4.png){: .shadow}
	_高级面板_

## 最终效果：

![](/assets/img/2021/1107/1107-cmd-5.png){: .shadow}
_最终效果_


## 参考资料:
[https://www.cxyzjd.com/article/qq_40512922/98763785](https://www.cxyzjd.com/article/qq_40512922/98763785){:target="_blank"}
