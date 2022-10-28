---
# type: posts 
title: "树莓派Shell脚本文件"
date: 2014-09-29T18:17:06+0800
authors: ["Jianan"]
summary: "命令可以组合起来保存在文件中，并一块执行。一个简单的例子，复制下面的命令到你最喜欢的文本编辑器中：


现在，将其保存名为fun-script的文件。在你运行这个文件之前，你需要将它设置为可执行文件，可以使用修改模式的命令chmod来实现。每一个文件和文件夹都有自己的权限来限定哪些用户可以操作它或者不能操作它。在这个例子中，运行命令chmod +x fun-script，fun-script"
series: ["RaspBerry"]
categories: ["翻译", "RaspBerry"]
tags: ["RaspBerry", "树莓派", "shell", "脚本"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 7944
comment_num: 0
---

  

命令可以组合起来保存在文件中，并一块执行。一个比较愚蠢的例子，复制下面的命令到你最喜欢的文本编辑器中：

    
    
    while 1
    do
    echo Raspberry Pi!
    done

  

现在，将其保存为名为fun-script的文件。在你运行这个文件之前，你需要将它设置为可执行文件，可以使用修改模式的命令 **chmod**
来实现。每一个文件和文件夹都拥有自己的权限来限定哪些用户可以操作它或者不能操作它。在这个例子中，运行命令 **chmod +x fun-
script**，fun-script文件就变成可执行文件了。你可以输入 **./fun-script**
来运行这个文件（当然这个文件要在你当前的目录中）。这个脚本将会无限循环输出“Raspberry Pi！”，停止循环可以使用 **Ctrl +
C**键。这将会杀死当前终端中正在运行的命令（进程）。

  

原文地址：<http://www.raspberrypi.org/documentation/linux/usage/scripting.md>

[  
](http://www.raspberrypi.org/documentation/linux/usage/scripting.md)

