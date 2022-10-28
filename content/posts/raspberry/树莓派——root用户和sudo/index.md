---
# type: posts 
title: "树莓派——root用户和sudo"
date: 2014-09-28T11:28:34+0800
authors: ["Jianan"]
summary: "Linux操作系统是一个多用户操作系统，它允许多个用户登录和使用一台计算机。为了保护计算机（和其他用户的隐私），用户都被限制了能做的事情。

大多数用户都允许运行计算机上大部分程序，并且编辑和保存存放在他们自己home目录中的文件。一般用户都不允许编辑其他用户的文件和一些系统文件。然而，在Linux系统上有一个特殊用户叫做超级用户，通常用户名为root。这个超级用户访问计算机没有限制，几乎可以"
series: ["RaspBerry"]
categories: ["翻译", "RaspBerry"]
tags: ["RaspBerry", "树莓派", "sudo", "超级用户"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 5614
comment_num: 0
---

  

Linux操作系统是一个多用户操作系统，它允许多个用户登录和使用一台计算机。为了保护计算机（和其他用户的隐私），用户都被限制了能做的事情。

  
大多数用户都允许运行计算机上大部分程序，并且编辑和保存存放在他们自己home目录中的文件。一般用户都不允许编辑其他用户的文件和一些系统文件。然而，在Linux系统上有一个特殊用户叫做超级用户，通常用户名为root。这个超级用户访问计算机没有限制，几乎可以做所有事情。

  

# SUDO

  
你通常不以root用户登录计算机，但是可以使用 **sudo**
命令来获得超级用户权限。如果你登录树莓派使用的是pi用户，那么你就是以普通用户身份登录。你可以在你想要运行的程序之前添加sudo命令来以root用户身份运行程序。

  
例如，如果你想要在树莓派上安装额外的软件，你通常需要使用 **apt-get** 工具。为了能够更新可使用的软件列表，你需要在 **agt-get**
命令之前添加 **sudo** 命令前缀： **sudo apt-get update** 。

  
查看更多[apt命令](http://blog.csdn.net/qinxiandiqi/article/details/39668119)信息。

  
你同样也可以使用 **sudo su**
命令来运行一个超级用户shell终端。一旦以超级用户的身份运行命令，那么就没有什么能够防止造成系统伤害的错误。相当于关闭了机器上的安全防护。虽然这样能够更容易访问系统内部的东西，但是造成损害的风险更大。建议你只在需要超级用户权限的时候以超级用户身份运行命令，在不需要超级用户权限的时候及时退出超级用户shell终端。

  

# WHO CAN USE SUDO？（谁可以使用sudo）

  
如果任何用户都能够在命令之前添加sudo，安全性就会遭到破坏，因此只有指定的用户才能使用sudo获取计算机管理员的权限。pi用户已经包含在
**sudoer** 文件中。允许其他用户使用超级用户权限，你可以将这些用户添加到sudo分组，或者使用 **visudo** 添加他们。

  
更多详细信息请参考[用户管理](http://blog.csdn.net/qinxiandiqi/article/details/39136023)。

  

  

原文地址：<http://www.raspberrypi.org/documentation/linux/usage/root.md>

  

