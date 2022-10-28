---
# type: posts 
title: "树莓派 —— rsync（同步）"
date: 2014-09-22T13:41:59+0800
authors: ["Jianan"]
summary: "你可以使用rsync工具在不同计算机之间同步文件夹。你可能想要从你的桌面计算机或者笔记本上传送文件到你的树莓派上，并保持这个文件的更新。或者，你想要把你的树莓派拍摄的照片自动传送到你的计算机上。

使用基于SSH的rsync可以让你自动将文件传送到你的计算机上。

以下的例子是将你的树莓派上的照片文件夹设置为自动同步到你的计算机上：

在你的计算机上创建一个名为camera的目录："
series: ["RaspBerry"]
categories: ["翻译", "RaspBerry"]
tags: ["RaspBerry", "树莓派", "rsync", "同步"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 4191
comment_num: 0
---

  
你可以使用rsync工具在不同计算机之间同步文件夹。你可能想要从你的桌面计算机或者笔记本上传送文件到你的树莓派上，并保持这个文件的更新。或者，你想要把你的树莓派拍摄的照片自动传送到你的计算机上。  
  
使用基于SSH的rsync可以让你自动将文件传送到你的计算机上。  
  
以下的例子是将你的树莓派上的照片文件夹设置为自动同步到你的计算机上：  
  
在你的计算机上创建一个名为camera的目录：  

    
    
    mkdir camera

  
登录树莓派运行 **hostname -I**
命令查看树莓派的IP地址。在这例子中，树莓派已经创建了定时任务，它每隔一分钟就会拍摄一张照片，并使用时间戳将照片保存在SD卡的本地camera目录下。  
  
现在在你的计算机上运行以下命令（替换成你自己的树莓派IP地址）：  

    
    
    rsync -avz -e ssh pi@192.168.1.10:camera/ camera/

  
这个命令将会从树莓派的camera文件夹下所有的文件复制到你的计算机camera目录下。  
  
为了能够保持文件夹的同步，需要在[cron](http://blog.csdn.net/qinxiandiqi/article/details/39475209)中运行这个这个命令。  

  

  

原文地址：<http://www.raspberrypi.org/documentation/remote-access/ssh/rsync.md>

[  
](http://www.raspberrypi.org/documentation/remote-access/ssh/rsync.md)

