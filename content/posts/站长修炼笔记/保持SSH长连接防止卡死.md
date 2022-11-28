---
# type: posts 
title: 保持SSH长连接防止终端卡死
date: 2022-11-27T23:00:24+08:00
authors: ["Jianan"]
summary: "SSH默认在长时间无交互情况下会自动断开连接，要想终端不卡死，只要保持SSH连接即可。"
series: ["站长修炼笔记"]
categories: []
tags: [SSH]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 0
comment_num: 0 
---

使用SSH进行远程连接的时候，假如终端长时间没有输入交互，终端就会卡死。这是因为SSH默认在长时间无交互情况下会自动断开连接，要想终端不卡死，只要保持SSH连接即可。 

SSH连接可以通过设置合适的SSH心跳来维持。
## 方式一：设置SSH服务端心跳配置

1、使用管理员权限修改服务端/etc/ssh/sshd_config配置文件：
```shell
sudo vim /etc/ssh/sshd_config
```

2、在配置文件中增加与客户端连接的心跳配置：
```text
# 心跳配置
ClientAliveInterval 30
ClientAliveCountMax 10
```
- ClientAliveInterval参数：设置间隔多长时间向客户端发送心跳
- ClientAliveCountMax参数：设置向客户端发送多少次心跳都失败后自动断开连接  

3、修改配置后重启ssh服务
```shell
sudo systemctl restart sshd
```

## 方式二：设置SSH客户端心跳配置
1、使用管理员权限修改本地/etc/ssh/ssh_config配置文件：
```shell
sudo vim /etc/ssh/ssh_config
```
2、在配置文件中增加与服务端连接的心跳配置：
```text
# 心跳配置
ServerAliveInterval 20
ServerAliveCountMax 999
```
- ServerAliveInterval参数：设置间隔多长时间向服务端发送心跳
- ServerAliveCountMax参数：设置向服务端发送多少次心跳都失败后自动断开连接

3、修改配置后重启ssh服务
```shell
sudo systemctl restart ssh
```
