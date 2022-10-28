---
# type: posts 
title: "树莓派用户管理"
date: 2014-09-08T13:20:36+0800
authors: ["Jianan"]
summary: "树莓派的用户管理需要在命令行终端上处理。默认的用户名是pi，密码为raspberry。你可以添加用户并修改每一个用户的密码。


1、CHANGE YOUR PASSWORD（修改你的密码）

当你使用pi用户登录之后，你可以使用passwd命令修改你的密码。
在命令行中输入passwd，然后按回车。命令行终端会提示你输入当前的密码进行验证，验证通过后会要求你输入新的密码。输完按回车完"
series: ["RaspBerry"]
categories: ["翻译", "RaspBerry"]
tags: ["RaspBerry", "树莓派", "用户管理", "user"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 15649
comment_num: 0
---

\------------------------------------------------------------------------------------------------------------------------------------------

原文作者：Raspberry Pi Foundation

原文地址：<http://www.raspberrypi.org/documentation/linux/usage/users.md>  

原文版权：[CC BY-SA](http://creativecommons.org/licenses/by-sa/4.0/)

译文作者：Jianan - qinxiandiqi@foxmail.com

版本信息：基于原文2014-09-08版本进行翻译

译文版权：[CC BY-SA](http://creativecommons.org/licenses/by-
sa/4.0/)，允许复制转载和演绎，但必须保留译者署名和译文链接，并遵守相同共享协议

\------------------------------------------------------------------------------------------------------------------------------------------

  

树莓派的用户管理需要在命令行终端上处理。默认的用户名是pi，密码为raspberry。你可以添加用户并修改每一个用户的密码。

  

# 1、CHANGE YOUR PASSWORD（修改你的密码）

  

当你使用pi用户登录之后，你可以使用passwd命令修改你的密码。  

如果你使用的用户拥有sudo权限，你可以通过其他用户的用户名和passwd命令修改他们的密码。例如，sudo passwd
bob将允许你设置bob用户的密码，以及用户的其他可选值（例如姓名）。可以简单按回车跳过这些选项。

在命令行中输入passwd，然后按回车。命令行终端会提示你输入当前的密码进行验证，验证通过后会要求你输入新的密码。输完按回车完成，你会被要求重新输入一遍新密码进行确认。注意当你输入密码的时候，命令行上不会显示任何字符。一旦你完成确认并且没有错误，命令行会打印成功信息（passwd:
password updated successfully），新的密码也会立即生效。

  

# 2、REMOVE A USE'S PASSWORD（删除用户密码）

  
你可以通过sudo passwd bob -d命令删除用户bob的密码。

  

# 3、CREATE A NEW USER（创建一个新用户）

  
你可以使用adduser命令在你的树莓派设备上创建其他用户。

  
输入sudo adduser bob，将会提示你输入新用户bob的密码，也可以直接回车留空白表示不设置密码。

  

## 3.1 HOME FOLDER（Home文件夹）

  
当你创建一个新的用户，新的用户将会在/home/目录下创建自己的home文件夹。pi用户的home文件夹位于/home/pi/。

  

## 3.2 SKEL

  
上面创建了一个新的用户，/etc/skel/目录下的文件也会复制一份到用户的home文件夹下。你可以根据你自己的喜好添加或者修改类似/etc/skel/中.bashrc这样以“.”开头的文件，这些修改后的版本将会被应用到新创建的用户上。

  

# 4、SUDOERS（sudo用户）

  
树莓派上默认的pi用户是一个sudoer（sudo用户）。这种类型的用户在命令行上输入命令之前先输入sudo就能拥有root权限执行命令，也能够通过sudo
su完全切换到root用户。

  
添加一个用户到sudoer用户组，需要使用一个sudo用户输入sudo visudo命令打开配置文件，找到文件中“# User privilege
specification”下面的“root ALL=(ALL:ALL)
ALL”。复制这一行并将其中的root替换为对应的用户名。为了免验证密码使用root权限，修改为NOPASSWD:ALL。下面的例子给予了bob用户免密码访问sudo权限：

    
    
    # User privilege specification
    root  ALL=(ALL:ALL) ALL
    bob   ALL = NOPASSWD: ALL

  
保存并退出以完成修改。千万小心，因为这些操作可能会意外移除你自己的sudo权限。

  
注意你可以通过以下命令修改visudo命令使用的编辑器（默认的编辑器是Nano）：

    
    
    update-alternatives --set editor /usr/bin/vim.tiny

  
上面的命令将编辑器修改为Vim。

  

# 5、DELETE A USER（删除用户）

  
你可以使用userdel命令将你系统上的一个用户删除掉。附加-r标签可以同时删除它们的home文件夹：

    
    
    sudo userdel -r bob

  

  

