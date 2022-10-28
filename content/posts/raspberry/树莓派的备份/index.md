---
# type: posts 
title: "树莓派的备份"
date: 2014-09-25T18:36:18+0800
authors: ["Jianan"]
summary: "强烈建议定期备份一些重要文件。备份通常不单限于用户文件，还可以是配置文件、数据库、已安装的软件、设置，甚至是整个系统快照。

这里我们会指导你通过一些备份技术为你的树莓派系统备份。


HOME FOLDER（Home目录）

一种比较好的备份Home目录方法是使用tar命令生成一个目录快照压缩文件，然后复制到你的PC电脑或者云端存储上。输入下面命令来备份home目录：
cd /h"
series: ["RaspBerry"]
categories: ["翻译", "RaspBerry"]
tags: ["RaspBerry", "树莓派", "备份"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 2670
comment_num: 0
---

  

强烈建议定期备份一些重要文件。备份通常不单限于用户文件，还可以是配置文件、数据库、已安装的软件、设置，甚至是整个系统快照。

  
这里我们会指导你通过一些备份技术为你的树莓派系统备份。

  

# HOME FOLDER（Home目录）

  
一种比较好的备份Home目录方法是使用 **tar** 命令生成一个目录快照压缩文件，然后复制到你的PC电脑或者云端存储上。输入下面命令来备份home目录：

    
    
    cd /home/
    tar czf pi_home.tar.gz pi

  

这里在/home/目录下创建了一个名为pi_home.tar.gz的tar压缩文件。你可以通过USB或者网络将这个文件复制到你其他的机器设备上。

  

# MYSQL

  

如果你在树莓派上使用了MySQL数据库，备份数据库同样也是比较重要的。备份一个单一的数据库，可以使用 **mysqldump** 命令：

    
    
    mysqldump recipes > recipes.sql

  

从备份文件中恢复数据库，需要在mysql导入这个备份文件，提供证书（如果需要的话）和数据库名字。

注意数据库必须是已经存在的，因此要先创建它：

    
    
    mysql -Bse "create database recipes"
    cat recipes.sql | mysql recipes

  

另外，你还可以使用pv命令（默认没有安装，需要使用 **apt-get install pv**
命令安装）查看MySQL处理备份文件的进度。这对大文件非常有用：

    
    
    pv recipes | mysql recipes

  

# SD CARD IMAGE（SD卡映像）

  

可能你需要备份整张SD卡映像，这样你才能在原SD卡丢失或者损坏的情况下恢复到新的SD卡。你可以使用将映像写入到SD卡的方法来备份SD卡，只是顺序要反过来。

  
在Linux或者Mac系统上，例如：

    
    
    sudo dd bs=4M if=/dev/sdb of=raspbian.img

  

这里将会在你的PC上创建一个映像文件，你可以将这个映像文件写入到其它的SD卡来复制相同的内容和设置。恢复或者复制到其它的SD卡，也是使用 **dd**
命令，只不过要反过来：

    
    
    sudo dd bs=4M if=raspbian.img of=/dev/sdb

  
更多信息请参考：[installing SD card
imagers](http://www.raspberrypi.org/documentation/installation/installing-
images/README.md)  
  

# AUTOMATION（自动化）

  
你可以写一个Bash脚本来让这些备份操作自动化执行，甚至可以使用[cron](http://blog.csdn.net/qinxiandiqi/article/details/39475209)让这些操作定期执行。  

  

  

原文地址：<http://www.raspberrypi.org/documentation/linux/filesystem/backup.md>

[  
](http://www.raspberrypi.org/documentation/linux/filesystem/backup.md)

