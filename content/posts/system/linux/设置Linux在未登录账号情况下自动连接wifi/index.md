---
# type: posts 
title: "设置Linux在未登录账号情况下自动连接wifi"
date: 2020-12-29T15:09:51+0800
authors: ["Jianan"]
summary: "最近将闲置的PC安装了Deepin，作为内网的一台服务器。一般使用场景都是通过ssh远程登录，但是发现设备通电开机后，如果没有登录账号，wifi是不会自动连接的。这就有点尴尬了，每次通电开机后都要手动去机器上登录下账号，进入了桌面环境后再连接个wifi，然后才能在内网用ssh远程登录操作，非常不方便。
期望机器能在通电进入系统后，即使没有登录账号也能自动连接wifi。可以使用Linux的网络管理工具的命令：
nmctl device wifi connect [ssid wifi名字] password ["
series: ["Linux"]
categories: ["Linux"]
tags: ["linux", "wifi", "ssh"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 719
comment_num: 1
---

最近将闲置的PC安装了Deepin，作为内网的一台服务器。一般使用场景都是通过ssh远程登录，但是发现设备通电开机后，如果没有登录账号，wifi是不会自动连接的。这就有点尴尬了，每次通电开机后都要手动去机器上登录下账号，进入了桌面环境后再连接个wifi，然后才能在内网用ssh远程登录操作，非常不方便。

期望机器能在通电进入系统后，即使没有登录账号也能自动连接wifi。可以使用Linux的网络管理工具的命令：

```shell
nmctl device wifi connect [ssid wifi名字] password [wifi密码]
```

只要上面的命令成功连接wifi后，以后每次通电启动系统，不必登录账号进入桌面环境也能自动重连wifi，这样就再也不用去手动登录下桌面环境啦。
