---
# type: posts 
title: "树莓派和VNC"
date: 2014-09-18T18:12:25+0800
authors: ["Jianan"]
summary: "有时候直接操作树莓派不是很方便。你可能想要使用其它计算机通过远程来控制树莓派。

VNC是系统分享的一个图形界面，能够让你从一台计算机远程控制另一台计算机的桌面接口。它从操作计算机端发送键盘和鼠标事件，并通过网络接收到远程计算机的屏幕更新。

你将在你的计算机上的一个窗口中看到树莓派的桌面。你可以在这个桌面上跟直接控制树莓派一样操作。

-  在你的树莓派上（使用一个显示器或者通过SS"
series: ["RaspBerry"]
categories: ["翻译", "RaspBerry"]
tags: ["RaspBerry", "树莓派", "vnc", "远程控制", "图形"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 1647
comment_num: 0
---

  

有时候直接操作树莓派不是很方便。你可能想要使用其它计算机通过远程来控制树莓派。

  
VNC是系统分享的一个图形界面，能够让你从一台计算机远程控制另一台计算机的桌面接口。它从操作计算机端发送键盘和鼠标事件，并通过网络接收到远程计算机的屏幕更新。

  
你将在你的计算机上的一个窗口中看到树莓派的桌面。你可以在这个桌面上跟直接控制树莓派一样操作。

  

-  在你的树莓派上（使用一个显示器或者通过SSH），安装TightVNC包：
    
    
    sudo apt-get install tightvncserver

  

-  下一步，运行TightVNC服务，它将会提示你输入密码和一个可选的只读密码：
    
    
    tightvncserver

  

-  在终端上启动一个VNC服务。下面的例子在VNC上启动了一个端口为(:0)的全屏HD分辨率会话：
    
    
    vncserver :0 -geometry 1920x1080 -depth 24

  

-  现在，在你的计算机上，安装并运行VNC客户端：

    -  在Linux计算机上可以使用下面命令安装xtightvncviewer包：
    
    
    sudo apt-get install xtightvncviewer

    -  其他平台，可以从[tightvnc.com](http://www.tightvnc.com/download.php)上下载TightVNC。

  

# AUTOMATION AND RUN AT BOOT（开机自动运行）

  

你可以在树莓派上创建一个简单的文件保存一些命令来运行VNC服务，保存后以便不用再记住这些命令：

  
-  创建一个包含下面shell脚本的文件：
    
    
    #!/bin/sh
    vncserver :0 -geometry 1920x1080 -depth 24 -dpi 96

  

-  保存文件，例如保存为vnc.sh。

-  设置文件为可执行文件：
    
    
    chmod +x vnc.sh

  

-  之后你就可以在任何时候使用下面命令来执行这个文件：
    
    
    ./vnc.sh

  

**设置机器启动的时候自动运行：**

-  在树莓派的终端上以root用户登录：
    
    
    sudo su

  

-  导航到/etc/init.d/目录下：
    
    
    cd /etc/init.d/

  

-  创建一个包含以下脚本的文件：
    
    
    ### BEGIN INIT INFO
    # Provides: vncboot
    # Required-Start: $remote_fs $syslog
    # Required-Stop: $remote_fs $syslog
    # Default-Start: 2 3 4 5
    # Default-Stop: 0 1 6
    # Short-Description: Start VNC Server at boot time
    # Description: Start VNC Server at boot time.
    ### END INIT INFO
    
    #! /bin/sh
    # /etc/init.d/vncboot
    
    USER=root
    HOME=/root
    
    export USER HOME
    
    case "$1" in
     start)
      echo "Starting VNC Server"
      #Insert your favoured settings for a VNC session
      /usr/bin/vncserver :0 -geometry 1280x800 -depth 16 -pixelformat rgb565
      ;;
    
     stop)
      echo "Stopping VNC Server"
      /usr/bin/vncserver -kill :0
      ;;
    
     *)
      echo "Usage: /etc/init.d/vncboot {start|stop}"
      exit 1
      ;;
    esac
    
    exit 0

  

-  保存这个文件，例如保存为vncboot。

-  设置这个文件为可执行文件：
    
    
    chmod 755 vncboot

  

-  启用基于开机顺序依赖：
    
    
    update-rc.d /etc/init.d/vncboot defaults

  

-  如果启用基于开机顺序依赖成功，你将会看到：
    
    
    update-rc.d: using dependency based boot sequencing

  

-  如果失败，你将会看到：
    
    
    update-rc.d: error: unable to read /etc/init.d//etc/init.d/vncboot

  

-  这个时候你可以使用以下命令：
    
    
    update-rc.d vncboot defaults

  

-  重启你的树莓派，你将会发现VNC服务已经运行了。

  

现在你可以在你的PC机或者笔记本电脑上使用VNC客户端连接VNC服务并控制树莓派。可以根据你的计算机操作系统查看以下相应指南：

[Linux](http://blog.csdn.net/qinxiandiqi/article/details/39401445)

[Mac OS](http://blog.csdn.net/qinxiandiqi/article/details/39401663)

[Windows](http://blog.csdn.net/qinxiandiqi/article/details/39436105)

  
  

原文地址：<http://www.raspberrypi.org/documentation/remote-access/vnc/README.md>

[  
](http://www.raspberrypi.org/documentation/remote-access/vnc/README.md)

