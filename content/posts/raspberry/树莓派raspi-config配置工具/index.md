---
# type: posts 
title: "树莓派raspi-config配置工具"
date: 2014-09-08T14:17:01+0800
authors: ["Jianan"]
summary: "raspi-config是由Alex Bradbury设计并维护的树莓派配置工具，适用于Raspbian系统。


1、USAGE（使用）

当你第一次启动Raspbian的时候会有rasp-config的提示。打开这个配置工具，只需要在终端上简单输入以下命令：

sudo raspi-config
要求sudo管理员权限是因为你要修改的文件不属于pi用户所有。


你将会看"
series: ["RaspBerry"]
categories: ["翻译", "RaspBerry"]
tags: ["RaspBerry", "树莓派", "raspi-config工具"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 8292
comment_num: 0
---

  

raspi-config是由Alex Bradbury设计并维护的树莓派配置工具，适用于Raspbian系统。

  

# 1、USAGE（使用）

  
当你第一次启动Raspbian的时候会有rasp-config的提示。打开这个配置工具，只需要在终端上简单输入以下命令：

    
    
    sudo raspi-config

  
要求sudo管理员权限是因为你要修改的文件不属于pi用户所有。

  
你将会看到一个蓝色的屏幕，中间有带选项的灰色框，类似下面：

![](dae7db3b72d6571a6b203b94ed3f9ed2.png)  

  

# 2、MOVING AROUND THE MENU（在菜单中移动）

  
使用上下光标控制键来移动可选选项中的高光显示。按右光标控制键跳出选项菜单转移到底部的<Select>和<Finish>按钮。按左光标控制键返回选项菜单。另外，使用tab键可以在这里面互相切换。

  
注意在一些有很多选项的菜单中（例如时区城市的选择菜单），你可以直接输入对应选项的相关内容来跳转到菜单中对应的选项。例如，输入“L”将会跳转到Lisbon，发现距离London只有2个选项，这样就避免了滚动所有的选项来查找的麻烦。

  

# 3、WHAT PASPI-CONFIG DOES（raspi-config可以做什么）

  
总的来说，raspi-
config的目标是提供便捷的方法来修改大多数常用的配置。它会自动去修改对应的/boot/config.txt和其它标准的Linux配置文件。有一些选项会要求重启系统来使设置生效。如果你修改了这些选项，raspi-
config会询问你是否在按下<Finish>按钮之后重启系统。

  

# 4、MENU OPTION（菜单选项）

  

## 4.1 EXPAND FILESYSTEM（扩展文件系统）

  
如果你使用NOOBS的方式安装Raspbian系统，那么你可以忽略这部分，因为NOOBS在安装系统的时候已经自动扩展了文件系统。然而，如果你是自己将系统映像文件写入到SD上，那么SD卡上的一部分空间将没有被使用。这个选项可以另外挂载3GB以上的容量。选择这个选项可以将SD卡剩余的空间全部挂载上来，为你的文件提供更大的空间。你需要重启树莓派来让这个设置生效。注意这个选项没有确认过程，选择之后会直接开始扩展空间。

  

## 4.2 CHANGE USER PASSWORD（修改用户密码）

  
树莓派默认的用户是pi，密码是raspberry。你可以在这里修改pi的密码。其它用户管理可以参考[这里](http://blog.csdn.net/qinxiandiqi/article/details/39136023)。

  

## 4.3 ENABLE BOOT TO DESKTOP OR SCRATCH（设置启动进入桌面或者Scratch）

  
你可以修改你的树莓派启动时的操作。使用这个选项可以修改你的启动首选项是进入命令行终端，还是桌面环境，或者直接进入Scratch编程环境。

  

## 4.4 INTERNATIONALISATION OPTION（国际化选项）

  
选择Internationalisation Options，然后点击回车将会进入以下子菜单：

  

### 4.4.1 CHANGE LOCALE（修改本地属性）

  
选择一个本地属性，例如en_GB.UTF-8 UTF-8

  

### 4.4.2 CHANGE TIMEZONE（修改时区）

  
选择你自己的时区，先选择地域例如Europe（欧洲）；然后选择城市例如London。可以直接输入目标字符跳转到对应的选项上。

  

### 4.4.3 CHANGE KEYBOARD LAYOUT（修改键盘类型）

  
这个选项将会打开另一个允许你选择的键盘类型的菜单。它将会花费比较长的时间读取所有的键盘类型。修改通常会立即生效，但也可能会要求重启。

  

## 4.5 ENABLE CAMERA（启用摄像头）

  
为了使用树莓派的摄像头模块，你必须在这里启用它。选择这个选项，然后选择Enable。这将会确保至少有128M内存分配给GPU。

  

## 4.6 ADD TO RASTRACK（添加Rastrack）

  
Rastrack是由社区中的树莓派用户提供自己的地理位置形成的Google
Map，它展示了一张全世界树莓派爱好者的分布图。这是在2012年由年轻的树莓派爱好者Ryan
Walmsley创建的。可以在[rastrack.co.uk](http://rastrack.co.uk/)访问Rastrack。

  
你可以使用这个选项将你的地理位置添加到这张地图中。

  

## 4.7 OVERCLOCK（超频）

  
将你的树莓派CPU超频是可以的。默认的主频是700MHz，但是它可以超频到1000MHz。你能够达到的主频是不确定的，主频太高可能会导致不稳定问题。选择这个选项将会提示以下警告：

  
**你应该明白超频可能会导致你的树莓派寿命缩短。如果你超频到一定频率导致系统不稳定，应该尝试降低主频。在系统启动的时候按住“shift”可以暂时禁用超频。**

  

## 4.8 ADVANCED OPTIONS（高级选项）

  

### 4.8.1 OVERSACN

  
旧电视机的画幕有很很多种不同的尺寸，有一些的机柜还覆盖到屏幕上。电视机的画幕因此使用了黑色边框来保证画面没有丢失，这就是overscan。现代的电视机和显示器不需要这个边框，而且信号也没有支持。如果初始的文字显示超出了屏幕的显示范围，你就需要启用overscan将黑色边框带回来。

  
所有的设置将会在重启系统之后开始作用。你也可以直接编辑[config.txt](http://www.raspberrypi.org/documentation/configuration/config-
txt.md)来进行更详细的设置。

  
在一些显示情况下，特别是显示器，禁用overscan会让画幕填充整个屏幕并校正。还有其它的情况可能需要启用overscan并判断它的值。

  

### 4.8.2 HOSTNAME（主机名）

  
设置网络中树莓派的主机名。

  

### 4.8.3 MEMORY SPLIT（分割内存）

  
修改分配给GPU的内存。

  

### 4.8.4 SSH

  
启用或者禁用使用SSH远程终端登录你的树莓派。

  
SSH允许你在另外一台计算机上使用命令行终端远程登录你的树莓派。禁用将会导致SSH服务不会在开机的时候启动以释放程序的资源。更多关于SSH的内容参考[这里](http://www.raspberrypi.org/documentation/remote-
access/ssh/README.md)。注意SSH默认是启用的。如果你的树莓派直接连接到公共网络，你应该禁用SSH，除非你确认已经为树莓派上每一个用户设置了密码。

  

### 4.8.5 SPI（串行外设接口）

  
启用或者禁用自动加载SPI内核模块，需要其它的外设产品例如PiFace。

  

### 4.8.6 AUDIO（音频）

  
强制音频通过HDMI或者3.5毫米接口输出。更多信息请参考[audio
configuration](http://blog.csdn.net/qinxiandiqi/article/details/39136195)。

  

### 4.8.7 UPDATE（更新）

  
将这个工具升级到最新版本。

  

## 4.9 ABOUT RASPI-CONFIG（关于raspi-config）

  
选择这个选项将会显示以下信息：

  
**这个工具提供更加直接的途径来设置树莓派的初始配置。尽管这个工具可以在任何时候运行，但是如果你已经深度定制了你的设备，其中一些选项可能没有作用。**

  

## 4.10、 FINISH（完成）

  
当你完成了你的修改后点击这个按钮，你将会被询问是否要重启设备。如果是第一次使用最好是重启设备。如果你选择了扩展你的SD卡，在启动过程中可能会有点延迟。

  

# 5、 DEVELOPMENT OF THIS TOOL（本工具的开发）

  
本工具的源代码托管在<https://github.com/asb/raspi-config>，你可以在这里查看问题和推送请求。

  

  

原文地址：<http://www.raspberrypi.org/documentation/configuration/raspi-config.md>  

  

