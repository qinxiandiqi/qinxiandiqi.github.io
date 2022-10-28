---
# type: posts 
title: "ubuntu下buffalo wzr-hp-300nh路由器刷DD-WRT系统"
date: 2015-06-14T21:50:39+0800
authors: ["Jianan"]
summary: "朋友送了个WZR-HP-300NH的buffalo路由器，只是系统被刷成了openwrt，而且还不知道管理员账号密码= =。。。刷了openwrt系统后，buffalo路由器的恢复出厂设置按钮也失效了，估计是openwrt系统的兼容问题，只能是想办法重新刷下系统了。
google上找了下tftp刷机的教程，本来以为是很简单的事情，结果还是折腾了好几天。下面是我测试成功的方法：
需要准备的工具："
series: ["路由器"]
categories: ["路由器"]
tags: ["tftp", "buffalo", "dd-wrt", "刷系统"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 2665
comment_num: 0
---

  

朋友送了个WZR-HP-300NH的buffalo路由器，只是系统被刷成了openwrt，而且还不知道管理员账号密码=
=。。。刷了openwrt系统后，buffalo路由器的恢复出厂设置按钮也失效了，估计是openwrt系统的兼容问题，只能是想办法重新刷下系统了。

google上找了下tftp刷机的教程，本来以为是很简单的事情，结果还是折腾了好几天。下面是我测试成功的方法：

需要准备的工具：一台PC机（我用的Ubuntu系统）、一条网线、buffalo路由器、另一台交换机或者路由器（我用的是腾达路由器）。  

1、进入Ubuntu，下载好要刷机的系统包了，想来想去还是去官网下载了DD-
WRT系统包来刷，链接地址：<http://www.buffalotech.com/support-and-
downloads/downloads>，不过下载的速度很慢。

2、将buffalo路由器用网线连接交换机或者其它路由器，我用的是之前一直在用的腾达路由器（用的久了不是很稳定，电子产品都这样，就不吐槽了）。为方便说明，下面直接用腾达路由器以示跟buffalo路由器做区别。

3、PC机连接腾达路由器（wifi或者网线都可以），登录腾达路由器，将腾达路由器局域网网段调整到192.168.11.xxx网段上，分配固定ip
192.168.11.1给buffalo路由器，分配固定ip 192.168.11.2给PC机。

4、进入Ubuntu，使用下面命令在Ubuntu的arp缓存列表上添加buffalo路由器的信息：

    
    
    sudo arp -s 192.168.11.1 xx:xx:xx:xx:xx:xx

其中的xx:xx:xx:xx:xx:xx为buffalo路由器的mac地址，在buffalo路由器背后的标签上可以找到。arp是地址解析协议，可以在发送报文的时候将ip地址解析为mac物理地址。

5、安装tftp软件包，在Ubuntu终端下，使用下面命令安装：

    
    
    sudo apt-get update
    sudo apt-get install tftp-hpa

6、使用tftp命令传送系统包：

    
    
    tftp 192.168.11.1
    
    tftp> verbose
    tftp> binary
    tftp> trace
    tftp> rexmt 1
    tftp> timeout 60
    tftp> put wzrhpg300nh-pro-v24sp2-14998b.enc

最后put指令的参数为系统包的文件名。执行完这个命令后，tftp就会开始寻找192.168.11.1的目标机器传送系统包，如果目标机器无响应，60秒之后就会显示超时。所以在执行这个命令之后，必须马上把buffalo路由去断电重启。buffalo只有在通电后的几秒钟内才能接收tftp传送过来的系统包并重装系统，错过这个时间就会自动进入原来的系统。在传输系统包的过程中，终端上会显示tftp传输数据包的数据，传输完成后会显示传输成功。这个时候，buffalo路由器开始更新系统，buffalo路由器上的diag红灯会开始闪烁，这个过程要花费好几分钟甚至十几分钟。耐心等待红灯停止闪烁，系统就重装好了，buffalo会自动重启。

重启之后，用网线连接buffalo路由去和电脑，在浏览器通过192.168.11.1地址就能访问buffalo路由器。首次访问会进入设置账号和密码界面，设置自己的账号名和密码，之后就能进入buffalo路由器的设置界面了，刷buffalo路由器系统到此结束。

  
整个刷系统的过程最重要的就是交换机或者另一台路由器（上面的腾达路由器）分配固定的ip地址。之前在网上找到大部分的教程的步骤都是先安装tftp工具，用网线直接连接电脑和路由器，设置电脑的网络和IP，再使用tftp工具传输buffalo系统包到buffalo路由器上。tftp传输系统包的过程中要掐准时间给buffalo断电重启，有的说10秒，有的说10几秒。。。感觉太不靠谱，而且这些方法我也没成功过。基本上都是在tftp的put指令执行完毕之后超时，连接不上buffalo路由器。我的推测是buffalo路由器在断电重启后可以接收tftp数据的模式下，pc机没有正确分配192.168.11.1
ip给buffalo路由器。等到pc机网络ip分配成功后，buffalo已经切换了接收tftp数据模式，正常进入了buffalo路由器的当前系统。所以使用了另一个交换机或者路由器强制分配了固定的ip来确保buffalo路由器在断电重启的过程中分配到的ip没有变化。

  

  

