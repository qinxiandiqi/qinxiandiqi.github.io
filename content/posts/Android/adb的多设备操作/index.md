---
# type: posts 
title: Adb的多设备操作
date: 2023-01-08T21:04:38+08:00
authors: ["Jianan"]
summary: "连接多个设备，adb命令无法执行"
series: ["Android"]
categories: ["Android"]
tags: ["Android", "adb"]
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

我相信你在使用adb的时候肯定有遇到这个错误： 
```shell
adb.exe: more than one device/emulator
```
什么情况呢？当你的电脑连接了多台真机或模拟器的时候，如果你要去执行一些操作设备的adb命令（比如常用的adb install），就可能会遇到这个问题。这是因为，当前连接的设备多于一个，adb不知道你要操作哪一个，它糊涂了。  
那要怎么办呢？简单，把其它真机拔掉，模拟器关掉，只留下要操作的真机或模拟器。  
**？？？**  
别别别！别这么粗暴，你是不是经常这么干？这样太low了，其实adb早就考虑过这些情况了，不然Android Stuido的Run怎么能够应对这些情况。  
既然是真机/模拟器太多了，那么只要告诉adb你要操作的是哪台真机/模拟器不就行了。adb其实提供了一些指定操作的设备的指令和参数： 

1、**-d**：如果你当前连接了多台模拟器和一台真机，你想要操作真机，那么你在adb命令后加上-d选项就代表你要操作的是这台真机。比如此时要安装apk到这台唯一真机上：
```shell
adb -d install xxx.apk
```

2、**-e**：如果你当前连接了多台真机和一台模拟器，你想要操作模拟器，那么你可以在adb命令后加上-e选项，这就代表你指定要操作这台模拟器。比如此时要安装apk到这台唯一模拟器上：
```shell
adb -e install xxx.apk
```

3、**-s [SERIAL]**：如果你当前连接了多台真机和多台模拟器，前面的-d和-e就没办法确定操作设备了，这个时候就需要-s选项了。-s选项后需要指定目标设备的序列号作为参数，这个设备序列号怎么来的呢？用adb devices指令查询，每一行前面的字符串就是这台设备的序列号。下面是我当前连接的设备情况：
```shell
PS C:\Users\nan> adb devices
List of devices attached
4745ca3b        device
emulator-5554   device
```
现在我要指定安装apk到4745ca3b这台真机上，我就可以这么操作：
```shell
adb -s 4745ca3b install xxx.apk
```