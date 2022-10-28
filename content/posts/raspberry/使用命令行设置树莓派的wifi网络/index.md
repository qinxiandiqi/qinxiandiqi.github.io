---
# type: posts 
title: "使用命令行设置树莓派的wifi网络"
date: 2014-10-02T18:42:26+0800
authors: ["Jianan"]
summary: "如果你没有登录到常用的图形用户界面，这种方法就适合用来设置树莓派的wifi。尤其是在你没有屏幕或者有线网络，仅使用串口控制线的时候。另外，这种方法也不需要额外的软件，所有需要的东西都已经包含进了树莓派。


GETTING WIFI NETWORK DETAILS（获取wifi网络详情）

为了扫描wifi网络，可以使用sudo iwlist wlan0 scan命令。这个命令会列出所有"
series: ["RaspBerry"]
categories: ["翻译", "RaspBerry"]
tags: ["RaspBerry", "树莓派", "wifi", "命令行"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 5692
comment_num: 0
---

  

如果你没有登录到常用的图形用户界面，这种方法就适合用来设置树莓派的wifi。尤其是在你没有屏幕或者有线网络，仅使用串口控制线的时候。另外，这种方法也不需要额外的软件，所有需要的东西都已经包含进了树莓派。

  

# GETTING WIFI NETWORK DETAILS（获取wifi网络详情）

  
为了扫描wifi网络，可以使用 **sudo iwlist wlan0 scan**
命令。这个命令会列出所有可使用的wifi网络，以及网络的一些有用信息。例如：

  
1、 **ESSID: "testing" ** ：这是wifi网络的名字。

2、 **IE:IEEE 802.11i/WPA2 Version1**
：这部分表示网络的验证方式，在这里是WPA2，这是一种用于替代WPA1的更新更加安全的无线网络标准。本指南应该适用于WEP、WPA或者WPA2，但是可能不适用企业版WPA2。

  
你同样需要wifi网络的密码。大多数家庭路由器（默认密码）都有标注在路由器背面的标签上。在这个例子中，搜索到的wifi网络的ESSID（ssid）是testing，并且密码（psk）是testingPassword。

  

# ADDING THE NETWORK DETAILS TO THE RASSBERRY PI（添加网络到树莓派上）

  
使用nano编辑器打开 **wpa-supplican** t配置文件：

    
    
    sudo nano /etc/wpa_supplicant/wpa_supplicant.conf

  
在文件的底部添加下面内容：

    
    
    network={
        ssid="The_ESSID_from_earlier"
        psk="Your_wifi_password"
    }

  
在本示例网络中，我们应该添加为：

    
    
    network={
        ssid="testing"
        psk="testingPassword"
    }

  
现在按 **ctrl +x**键然后按 **y** 键，最后再按 **回车键** 。

  
这个时候， **wpa-supplicant** 在几秒钟内应该就会注意到设置已经改变了，并且会尝试去连接这个网络。如果没有，那么就需要使用 **sudo
ifdown wlan0**

和 **sudo ifup wlan0** 命令手动重启接口，或者直接使用 **sudo reboot** 命令重启树莓派。

  
你可以使用 **ifconfig wlan0** 命令确认是否已经成功连接上网络。如果 **inet addr**
中已经有地址了，说明树莓派成功连接上了网络。如果没有，请检查你的密码和ESSID是否正确。

  

  
原文地址：<http://www.raspberrypi.org/documentation/configuration/wireless/wireless-
cli.md>

[  
](http://www.raspberrypi.org/documentation/configuration/wireless/wireless-
cli.md)

