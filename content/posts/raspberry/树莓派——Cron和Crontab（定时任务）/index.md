---
# type: posts 
title: "树莓派 —— Cron 和 Crontab（定时任务）"
date: 2014-09-22T13:36:48+0800
authors: ["Jianan"]
summary: "Cron是Unix系统的一个配置定期任务的工具，用于定期或者以一定的时间间隔执行一些命令或者脚本；可执行的任务范围可以是每天夜里自动备份用户的home文件夹，也可以每个小时记录CPU的信息日志。

crontab（cron table）命令用于编辑执行中的定期任务列表，并且操作是基于每个用户的，每一个用户（包括root用户）都拥有自己的crontab。


EDITING CRONTAB"
series: ["RaspBerry"]
categories: ["翻译", "RaspBerry"]
tags: ["RaspBerry", "树莓派", "cron", "crontab", "定时任务"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 13334
comment_num: 0
---

  

Cron是Unix系统的一个配置定期任务的工具，用于定期或者以一定的时间间隔执行一些命令或者脚本；可执行的任务范围可以是每天夜里自动备份用户的home文件夹，也可以每个小时记录CPU的信息日志。

  
crontab（cron table）命令用于编辑执行中的定期任务列表，并且操作是基于每个用户的，每一个用户（包括root用户）都拥有自己的crontab。

  

# EDITING CRONTAB（编辑crontab）

  
运行crontab和-e选项来编辑cron table：

    
    
    crontab -e

  

# SELECT AN EDIROR（选择一个编辑器）

  
第一次运行crontab命令的时候会提示你选择一个编辑器。如果你不确定使用哪一个，你可以直接回车选择默认的nano编辑器。

  
每一项cron实体的内容都包含六个部分：分钟、小时、月份中的哪一天、年份中的哪一月、星期中的哪一天，还有定时执行的命令。

    
    
    # m h  dom mon dow   command
    
    # * * * * *  command to execute
    # ┬ ┬ ┬ ┬ ┬
    # │ │ │ │ │
    # │ │ │ │ │
    # │ │ │ │ └───── 星期中的哪一天(0-7)(从0到6代表星期日到星期六,也可以使用名字;7是星期天,等同于0)
    # │ │ │ └────────── 月份 (1 - 12)
    # │ │ └─────────────── 月份中的日 (1 - 31)
    # │ └──────────────────── 小时 (0 - 23)
    # └───────────────────────── 分钟 (0 - 59)

  

例如：

    
    
    0 0 * * *  /home/pi/backup.sh
    

这项cron实例将会在每一天的午夜执行backup.sh脚本。

  

  
原文地址：<http://www.raspberrypi.org/documentation/linux/usage/cron.md>

  

