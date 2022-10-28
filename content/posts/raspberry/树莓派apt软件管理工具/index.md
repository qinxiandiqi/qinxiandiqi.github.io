---
# type: posts 
title: "树莓派apt软件管理工具"
date: 2014-09-29T12:44:28+0800
authors: ["Jianan"]
summary: "管理安装、升级和卸载软件最简单的方法就是使用Debian上的APT（高级包管理工具）。如果一个软件被打包成Debian上的包并且适用于树莓派的ARM架构，那么这个软件包同样兼容于Raspbian。

安装或者卸载软件包的时候你需要root用户权限，因此你的用户必须是sudoer用户，或者你必须使用root用户登录。更多信息参考用户管理和root用户。

安装新的包，或者更新已有的包，你需要"
series: ["RaspBerry"]
categories: ["翻译", "RaspBerry"]
tags: ["RaspBerry", "树莓派", "apt", "软件", "管理"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 3295
comment_num: 0
---

  

管理安装、升级和卸载软件最简单的方法就是使用Debian上的APT（高级包管理工具）。如果一个软件被打包成Debian上的包并且适用于树莓派的ARM架构，那么这个软件包同样兼容于Raspbian。

  
安装或者卸载软件包的时候你需要root用户权限，因此你的用户必须是sudoer用户，或者你必须使用root用户登录。更多信息参考[用户管理](http://blog.csdn.net/qinxiandiqi/article/details/39136023)和[root用户](http://blog.csdn.net/qinxiandiqi/article/details/39638975)。

  
安装新的包，或者更新已有的包，你需要连接互联网。

  
注意安装软件会消耗你的SD卡存储空间，因此你需要关注磁盘空间并使用合适大小的SD卡。

  
同样要注意安装软件的时候会进行加锁操作，因此你不能同时安装多个软件。

  

# SOFTWARE SOURCES（软件源）

  
APT在你的树莓派上的 **/etc/apt/sources.list** 文件中保存了一个软件源列表。在安装软件之前，你应该使用 **apt-get
update** 更新你的包列表：

    
    
    sudo apt-get update

  

# INSTALLING A PACKAGE WITH APT（使用apt安装一个软件包）

  

    
    
    sudo apt-get install tree
    

输入以上命令之后将会提示用户安装这个包需要多少存储空间，以及确认安装这个软件包。输入Y（或者直接回车，因为yes是默认操作）将允许安装。可以通过添加
**-y** 选项跳过这一步：

    
    
    sudo apt-get install tree -y

  
安装这个软件包，使用户可使用tree这个软件。

  

# USING AN INSTALLED PACKAGE（使用已经安装的软件包）

  
tree是一个命令工具，可以提供当前目录的可视化结构，以及所有内容。

  
输入tree运行tree命令，例如：

    
    
    tree
    ..
    
    ├── hello.py
    ├── games
    │   ├── asteroids.py
    │   ├── pacman.py
    │   ├── README.txt
    │   └── tetris.py

  
输入 **man tree** 获取tree的用户手册。

输入 **whereis tree** 显示tree安装位置：

    
    
    tree: /usr/bin/tree

  

# UNINSTALLING A PACKAGE WITH APT（使用APT卸载包）

  

## REMOVE（卸载）

  
你可以使用 **apt-get remove** 卸载一个包：

    
    
    sudo apt-get remove tree

  
用户会被提示是否要卸载。同样，添加 **-y** 选项可以跳过确认步骤。

  

## PURGE（清除）

  
你可以使用 **apt-get purge** 命令完整的移除包以及它所相关的配置文件。

  

    
    
    sudo apt-get purge tree

  

# UPGRADING EXISTING SOFTWARE（更新已安装软件）

  
如果有软件可以更新，你可以使用 **sudo apt-get update** 获取所有更新，并使用 **sudo apt-get upgrade**
安装所有可以更新的包。如果只更新特定的软件包而不更新其它过期的软件包，你可以使用 **sudo apt-get install somepackage**
来更新（这对于存储空间不足或者下载带宽比较小的情况比较有用）。

  

# SEARCHING FOR SOFTWARE（查询软件）

  
你可以使用关键字查询一个包的档案信息：

    
    
    apt-cache search:
    
    apt-cache search locomotive
    sl - Correct you if you type `sl' by mistake

  
你也可以使用以下命令在安装软件之前查询更多关于该包的信息：

    
    
    apt-cache show:
    
    apt-cache show sl
    Package: sl
    Version: 3.03-17
    Architecture: armhf
    Maintainer: Hiroyuki Yamamoto <yama1066@gmail.com>
    Installed-Size: 114
    Depends: libc6 (>= 2.4), libncurses5 (>= 5.5-5~), libtinfo5
    Homepage: http://www.tkl.iis.u-tokyo.ac.jp/~toyoda/index_e.html
    Priority: optional
    Section: games
    Filename: pool/main/s/sl/sl_3.03-17_armhf.deb
    Size: 26246
    SHA256: 42dea9d7c618af8fe9f3c810b3d551102832bf217a5bcdba310f119f62117dfb
    SHA1: b08039acccecd721fc3e6faf264fe59e56118e74
    MD5sum: 450b21cc998dc9026313f72b4bd9807b
    Description: Correct you if you type `sl' by mistake
     Sl is a program that can display animations aimed to correct you
     if you type 'sl' by mistake.
     SL stands for Steam Locomotive.

  

  
原文地址：<http://www.raspberrypi.org/documentation/linux/software/apt.md>

  

