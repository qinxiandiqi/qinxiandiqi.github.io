---
# type: posts 
title: "使用SSH无密码验证访问树莓派"
date: 2014-09-11T15:00:45+0800
authors: ["Jianan"]
summary: "让你的计算机每次连接树莓派都不需要输入密码是可以实现的，你只需要生成一个SSH key。


1、CHECK FOR EXISTING SSH KEYS（检查已存在的SSK key）

首先，检查你的计算机（你用来连接树莓派的设备）是否已经有SSH key：
ls ~/.ssh
如果你看到像id_rsa.pub或者id_dsa.pub这样的文件，那么你就已经配置好key了。你可以直接"
series: ["RaspBerry"]
categories: ["翻译", "RaspBerry"]
tags: ["RaspBerry", "树莓派", "SSH远程登录", "无密码", "rsa"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 7097
comment_num: 0
---

  

让你的计算机每次连接树莓派都不需要输入密码是可以实现的，你只需要生成一个SSH key。

  

# 1、CHECK FOR EXISTING SSH KEYS（检查已存在的SSK key）

  
首先，检查你的计算机（你用来连接树莓派的设备）是否已经有SSH key：

    
    
    ls ~/.ssh

  
如果你看到像 **id_rsa.pub** 或者 **id_dsa.pub**
这样的文件，那么你就已经配置好key了。你可以直接跳过生成key这个步骤（或者使用 **rm id*** 命令删除这些文件，然后重新生成key）。

  

# 2、GENEERATE NEW SSH KEYS（生成新的SSH key）

  
生成一个新的SSH Key可以输入以下命令（选用一个类似“ **< 你的名字>@<你的设备>**”这样可辨识的主机名，这里我们使用eben@pi）：

    
    
    ssh-keygen -t rsa -C eben@pi

  
你也可以使用引号来添加更加详细的带有空格描述，例如 **ssh-keygen -t rsa -C "Raspberry Pi #123"**。

  
输入上面的命令之后，你要被要求输入保存key的位置。我们建议你直接回车使用默认的路径（/home/XXX/.ssh/id_rsa）。

  
你也会被要求输入一个密码。这是一种附加的安全保障，确保在没有你的密码情况下无法使用你的key。这样，即使有人复制了你的key，他们也没有办法冒充你进行访问。如果你要使用一个密码，则输入这个密码然后回车，之后根据提示再输入密码确认一次。如果直接回车什么都不输入则表示不使用密码。

  
现在你应该就能在你的home目录下的.ssh文件夹中看到id_ras和id_ras.pub这样的文件：

    
    
    ls ~/.ssh
    authorized_keys  id_rsa  id_rsa.pub  known_hosts

  
这里的 **id_rsa** 文件就是你的私有key，将这个key继续保存在你的计算机上。 **id_rsa.pub**
文件是你的公共key，你需要把这个可以放到你想要连接的设备上。当你想要连接的设备匹配你的公共key和私有key成功，它就会允许你访问。

  
输入以下命令查看你的公共key内容：

    
    
    cat ~/.ssh/id_rsa.pub

  
公共key的格式应该如下：

    
    
    ssh-rsa <REALLY LONG STRING OF RANDOM CHARACTERS> eben@pi

  

# 3、COPY YOUR PUBLIC KEY TO YOUR RASPBERRY PI（将你的公共Key复制到你的树莓派上）

  
将你的公共key复制到你的树莓派上，在你的计算机终端上使用下面命令将公共key附加到你的树莓派上的 **authorized_keys**
文件，发送是通过SSH实现（将其中USERNAME和IP-ADDRESS替换为树莓派上的用户名和树莓派的IP）：

    
    
    cat ~/.ssh/id_rsa.pub | ssh <USERNAME>@<IP-ADDRESS> 'cat >> .ssh/authorized_keys'

  
注意这次你仍然需要验证你的密码。

  
现在尝试使用 **ssh <USER>@<IP-ADDRESS>**命令，你应该可以不需要密码直接就能连接上去。

  
如果你看见“Agent admitted failure to sign using the
key”信息，你需要将你的RSA或者DSA标识添加到认证代理上，执行以下命令使用ssh-agent：

    
    
    ssh-add

  
如果这仍然不起作用，使用 **rm ~/.ssh/id*** 命令删除你的key，然后根据本文档重新配置一遍。

  
通过SSH发送文件，你同样可以使用scp命令（安全复制）。参考[SCP
指南](http://www.raspberrypi.org/documentation/remote-access/ssh/scp.md)获取更多信息。

**  
付：SCP 指南 ——**  

scp是一个通过SSH发送文件的命令。这意味着你可以在两台计算机之间发送文件，也就是说可以在你的树莓派和桌面计算机或者笔记本之间互相发送文件。

  
原文地址：<http://www.raspberrypi.org/documentation/remote-
access/ssh/passwordless.md>

[  
](http://www.raspberrypi.org/documentation/remote-access/ssh/passwordless.md)

