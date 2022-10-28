---
# type: posts 
title: "Linux或者Mac系统使用SSH连接树莓派"
date: 2014-09-10T19:13:36+0800
authors: ["Jianan"]
summary: "你可以在一台Linux或者Mac计算机（或者另一个树莓派）的终端上使用SSH连接你的树莓派，并且不需要其它软件。

你需要知道你的树莓派IP地址以便连接上它。查询IP，可以在树莓派的终端上输入命令“hostname -I”。另外，如果你运行的树莓派没有显示器，你可以查看你的路由器上的设备列表或者使用像nmap这样的工具。

在计算机的终端上复制黏贴以下命令，但是要把其中的替换你的树莓派IP"
series: ["RaspBerry"]
categories: ["翻译", "RaspBerry"]
tags: ["RaspBerry", "树莓派", "ssh", "linux", "mac"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 11708
comment_num: 2
---

  

你可以在一台Linux或者Mac计算机（或者另一个树莓派）的终端上使用SSH连接你的树莓派，并且不需要其它软件。

  
你需要知道你的树莓派IP地址以便连接上它。查询IP，可以在树莓派的终端上输入命令“hostname
-I”。另外，如果你运行的树莓派没有显示器，你可以查看你的路由器上的设备列表或者使用像nmap这样的工具。

  
在计算机的终端上复制黏贴以下命令，但是要把其中的<IP>替换你的树莓派IP。终端上要使用Ctrl + Shift + V进行黏贴。

    
    
    ssh pi@<IP>

  
如果你接收到一个connection timed out（连接超时）的错误，很有可能是你输错了你的树莓派IP。

  
如果连接成功，你将会看到一个安全验证的警告。输入yes继续，你只会在第一次连接的时候看到这个警告。

  
有时候你的树莓派可能会占用了你的计算机以前曾经连接过（也有可能是在其它网络中连接过）的IP地址，你将会得到一个警告并要求清空你的已知设备列表。根据指示操作，然后再次使用ssh命令连接应该就能成功连接上。

  
下一步，你将会被提示输入pi用户的密码进行登录，Raspbian默认的密码时raspberry。现在，你应该能够看到树莓派的提示符，它代表了在树莓派上找到的一个用户。

如果你在树莓派上添加了其他用户，你也可以使用同样的方式连接，只需要将用户名替换，例如eben@192.168.1.5.

    
    
    pi@raspberrypi ~ $

  
现在你已经远程登录了树莓派，可以直接在上面执行命令。

  
获取更多ssh命令的文档只需要在终端上输入man ssh。

  
配置你的树莓派允许通过公共/私有键值对进行无密码SSH访问，可以参考[passwordless
SSH](http://blog.csdn.net/qinxiandiqi/article/details/39206323)指南。

  
原文地址：<http://www.raspberrypi.org/documentation/remote-access/ssh/unix.md>

[  
](http://www.raspberrypi.org/documentation/remote-access/ssh/unix.md)

