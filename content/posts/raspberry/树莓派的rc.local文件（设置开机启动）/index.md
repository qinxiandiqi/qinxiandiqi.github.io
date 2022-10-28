---
# type: posts 
title: "树莓派的rc.local文件（设置开机启动）"
date: 2014-09-30T16:58:53+0800
authors: ["Jianan"]
summary: "为了在树莓派启动的时候运行一个命令或程序，你需要将命令添加到rc.local文件中。这对于想要在树莓派接通电源后无需配置直接运行程序，或者不希望每次都手动启动程序的情况非常有用。

另一种替代定时任务的方法是使用cron和crontab。


EDITING RC.LOCAL（编辑rc.local文件）

在你的树莓派上，选择一个文本编辑器编辑/etc/rc.local文件。你必须使"
series: ["RaspBerry"]
categories: ["翻译", "RaspBerry"]
tags: ["RaspBerry", "树莓派", "rc.local", "开机启动"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 23043
comment_num: 3
---

  

为了在树莓派启动的时候运行一个命令或程序，你需要将命令添加到 **rc.local**
文件中。这对于想要在树莓派接通电源后无需配置直接运行程序，或者不希望每次都手动启动程序的情况非常有用。

  
另一种替代定时任务的方法是使用[cron和crontab](http://blog.csdn.net/qinxiandiqi/article/details/39475209)。

  

# EDITING RC.LOCAL（编辑rc.local文件）

  
在你的树莓派上，选择一个文本编辑器编辑 **/etc/rc.local** 文件。你必须使用root权限编辑，例如：

    
    
    sudo vim /etc/rc.local

  
在注释后面添加命令，但是要保证 **exit 0** 这行代码在最后，然后保存文件退出。

  

# WARNING（注意）

  
如果你的命令需要长时间运行（例如死循环）或者运行后不能退出，那么你必须确保在命令的最后添加 **“ &”**符号让命令运行在其它进程，例如：

    
    
    python /home/pi/myscript.py &

  
否则，这个脚本将无法结束，树莓派就无法启动。这个 **“ &”**符号允许命令运行在一个指定的进程中，然后继续运行启动进程。

  
另外，确保文件名使用绝对路径，而不是相对于你的home目录的相对路径。例如：使用 **/home/pi/myscript.py** 而不是用
**myscript.py** 。

  

  
原文地址：<http://www.raspberrypi.org/documentation/linux/usage/rc-local.md>

[  
](http://www.raspberrypi.org/documentation/linux/usage/rc-local.md)

