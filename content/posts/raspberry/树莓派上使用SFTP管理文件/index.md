---
# type: posts 
title: "树莓派上使用SFTP管理文件"
date: 2014-09-21T10:57:16+0800
authors: ["Jianan"]
summary: "SFTP（SSH文件传输协议）是一种网络协议，提供了基于SSH协议的文件访问、文件传输和文件管理方法。

通过使用SFTP，你可以很容易修改、浏览以及编辑树莓派上的文件。SFTP比起FTP更容易配置，因为Raspbian默认已经启用SSH。


Filezilla

在filezilla-project.org上对应你的操作系统下载最新版本的FileZilla客户端。

启动Fi"
series: ["RaspBerry"]
categories: ["翻译", "RaspBerry"]
tags: ["RaspBerry", "树莓派", "filezilla", "sftp", "文件传输"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 4851
comment_num: 0
---

  

SFTP（SSH文件传输协议）是一种网络协议，提供了基于SSH协议的文件访问、文件传输和文件管理方法。

  
通过使用SFTP，你可以很容易修改、浏览以及编辑树莓派上的文件。SFTP比起FTP更容易配置，因为Raspbian默认已经启用SSH。

  

# Filezilla

  
在[filezilla-project.org](https://filezilla-
project.org/)上对应你的操作系统下载最新版本的FileZilla客户端。

  
启动FileZilla并进入File（文件） > Site manager（站点管理）。

  
在对话框中填写你的树莓派的IP地址，用户名和密码（默认用户名是pi，密码是raspberry），并选择SFTP协议。

  
点击Connect（连接）按钮，你将会看到用户的home目录。

  

# UBUNTU USING NAUTILUS（Ubuntu使用Nautilus）

  
在客户机上打开Nautilus。

  
选择File > Conncect to Server

  

    
    
    Type: SSH
    Server: <The Pi's IP address>
    Port: 22 (default)
    User name: pi (default)
    Password: raspberry (default)

  
查看[IP地址](http://www.raspberrypi.org/documentation/troubleshooting/hardware/networking/ip-
address.md)。

  

  

原文地址：<http://www.raspberrypi.org/documentation/remote-access/ssh/sftp.md>

[  
](http://www.raspberrypi.org/documentation/remote-access/ssh/sftp.md)

