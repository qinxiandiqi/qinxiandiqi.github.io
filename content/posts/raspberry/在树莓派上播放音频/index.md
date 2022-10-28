---
# type: posts 
title: "在树莓派上播放音频"
date: 2014-09-09T14:17:26+0800
authors: ["Jianan"]
summary: "播放一个MP3文件，在命令行终端上用“cd”命令导航到.mp3文件所在的路径，然后输入以下命令：
omxplayer example.mp3这将会通过你的显示器内置音箱或者你的耳机接口连接设备播放example.mp3音频文件。

如果你需要一个示例音频文件，你可以输入以下命令来获取：
wget http://goo.gl/MOXGX3 -O example.mp3 --no-check-c"
series: ["RaspBerry"]
categories: ["翻译", "RaspBerry"]
tags: ["RaspBerry", "树莓派", "音频", "mp3"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 11612
comment_num: 1
---

  

播放一个MP3文件，在命令行终端上用“cd”命令导航到.mp3文件所在的路径，然后输入以下命令：  

    
    
    omxplayer example.mp3

这将会通过你的显示器内置音箱或者你的耳机接口连接设备播放example.mp3音频文件。  
  
如果你需要一个示例音频文件，你可以输入以下命令来获取：  

    
    
    wget http://goo.gl/MOXGX3 -O example.mp3 --no-check-certificate

  
如果你没有听到任何声音，请确认你的耳机或者音箱已经正确连接上。注意omxplayer不使用ALSA（高级Linux声音体系），因此可以忽略[audio
Configuration](http://blog.csdn.net/qinxiandiqi/article/details/39136195)中通过raspi-
config或者amixer的设置。  
  
如果omxplayer自动识别当前音频输出设备错误，你可以使用以下命令强制使用HDMI输出：  

    
    
    omxplayer -o hdmi example.mp3

或者你也可以强制使用耳机接口输出：  

    
    
    omxplayer -o local example.mp3

  
  
原文地址：<http://www.raspberrypi.org/documentation/usage/audio/README.md>

  

