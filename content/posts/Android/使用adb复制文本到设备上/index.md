---
# type: posts 
title: 使用adb复制文本到设备上
date: 2023-01-05T22:43:22+08:00
authors: ["Jianan"]
summary: "使用adb复制文本到设备上"
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

## 问题：想要把电脑上的文本复制到手机上？  

Ok！没问题。电脑上打开微信桌面版，登录微信，发送文本文件传输助手。手机上打开微信，点开文件传输助手，复制收到的文本。  
呼……好累。你是不是也经常干这样的事情？作为一个开发者怎么能容忍这么低效的事情，有没有更便捷的方式呢？别急，让我们的好朋友adb来帮个忙，借助adb的模拟输入指令把文本输入到设备上就行啦。具体步骤：  

1、手机连接电脑，在手机上随便打开并激活个输入框。既然是模拟文字输入，那么首先得要有个激活状态的输入框来接收模拟输入的文本。  
2、在电脑上打开终端，使用adb shell input text命令模拟文字输入：  
```shell
adb shell input text xxxxx(模拟输入的文本)
```
此时，应该能看到手机上激活的输入框已经追加上了adb命令模拟输入的文本。  

### 可能遇到的问题
Q: adb shell input命令报错了?
```java
java.lang.SecurityException: Injecting to another application requires INJECT_EVENTS permission
        at android.os.Parcel.createException(Parcel.java:2074)
        at android.os.Parcel.readException(Parcel.java:2042)
        at android.os.Parcel.readException(Parcel.java:1990)
        at android.hardware.input.IInputManager$Stub$Proxy.injectInputEvent(IInputManager.java:925)
        at android.hardware.input.InputManager.injectInputEvent(InputManager.java:886)
        at com.android.commands.input.Input.injectKeyEvent(Input.java:386)
        at com.android.commands.input.Input.access$100(Input.java:41)
        at com.android.commands.input.Input$InputText.sendText(Input.java:175)
        at com.android.commands.input.Input$InputText.run(Input.java:141)
        at com.android.commands.input.Input.onRun(Input.java:108)
        at com.android.internal.os.BaseCommand.run(BaseCommand.java:56)
        at com.android.commands.input.Input.main(Input.java:71)
        at com.android.internal.os.RuntimeInit.nativeFinishInit(Native Method)
        at com.android.internal.os.RuntimeInit.main(RuntimeInit.java:380)
Caused by: android.os.RemoteException: Remote stack trace:
        at com.android.server.input.InputManagerService.injectInputEventInternal(InputManagerService.java:662)
        at com.android.server.input.InputManagerService.injectInputEvent(InputManagerService.java:636)
        at android.hardware.input.IInputManager$Stub.onTransact(IInputManager.java:422)
        at android.os.Binder.execTransactInternal(Binder.java:1021)
        at android.os.Binder.execTransact(Binder.java:994)
```
出现这种问题是因为手机设备开发者选项中缺少模拟点击权限，在开发者选项中找到模拟点击权限，允许模拟点击就可以了：  
![](%E6%A8%A1%E6%8B%9F%E7%82%B9%E5%87%BB%E6%9D%83%E9%99%90.png)
