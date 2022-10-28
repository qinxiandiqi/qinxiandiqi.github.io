---
# type: posts 
title: "树莓派home目录"
date: 2014-09-26T14:22:22+0800
authors: ["Jianan"]
summary: "当你登入树莓派并打开一个终端窗口，或者你替换了图形用户界面直接开机启动到命令行模式，你将会开始于home目录下。假如你的用户名是pi，那么这个路径就是/home/pi。

这个地方是用户保存用户自己的文件，其中包括了用户保存桌面上的文件的文件夹Desktop，以及其它一些文件和文件夹。

在命令中导航到你的home目录，只需要简单输入cd命令然后回车。这相当于你输入了cd /home/pi"
series: ["RaspBerry"]
categories: ["翻译", "RaspBerry"]
tags: ["RaspBerry", "树莓派", "home"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 3935
comment_num: 0
---

  

当你登入树莓派并打开一个终端窗口，或者你替换了图形用户界面直接开机启动到命令行模式，你将会开始于home目录下。假如你的用户名是pi，那么这个路径就是
**/home/pi** 。

  
这个地方是用户保存用户自己的文件，其中包括了用户保存桌面上的文件的文件夹Desktop，以及其它一些文件和文件夹。

  
在命令中导航到你的home目录，只需要简单输入 **cd** 命令然后回车。这相当于你输入了 **cd /home/pi**
（这里的pi是你的用户名）。你也可以使用波浪符号（ **~** ），例如 **cd ~** ，这个符号可以用于表示相对于你的home目录路径。例如，
**cd ~/Desktop/** 等同于 **cd /home/pi/Desktop** 。

  
导航到 **/home/** 目录下并运行 **ls** ，你将会看到系统中每一个用户的home目录。

  
注意如果是以root用户登录，输入 **cd** 或者 **cd ~** 将会导航到root用户的home目录；不同于普通用户，这个路径将导航到
**/root/** 而不是 **/home/root/** 。更多信息请参考[root
user](http://www.raspberrypi.org/documentation/linux/usage/root.md)。

  
如果你有一些文件不希望丢失，你可以备份你的home目录。详细请参考[backing
up](http://blog.csdn.net/qinxiandiqi/article/details/39555399)。

  

原文地址：<http://www.raspberrypi.org/documentation/linux/filesystem/home.md>

