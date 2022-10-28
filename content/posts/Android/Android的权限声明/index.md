---
# type: posts 
title: "Android的权限声明"
date: 2016-04-28T16:23:56+0800
authors: ["Jianan"]
summary: "每一个android app都运行在一个限制访问的沙盒中。如果一个app需要访问它所在沙盒之外的资源和信息，那么这个app就需要声明适当的权限。这个权限声明要求将你的app需要的权限全部列举在App的manifest文件中。
根据权限不同的隐私敏感程度级别，系统可能会自动授予该权限，也有可能需要请求设备用户授权才能获取该权限。例如，如果你的app请求授予打开设备闪光灯的权限，系统将会自动授予这个权限。但是，如果你的a"
series: ["Android"]
categories: ["翻译", "Android"]
tags: ["android", "permission", "权限"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 3679
comment_num: 0
---

> 原文作者：Google
原文地址：[http://developer.android.com/intl/zh-cn/training/permissions/declaring.html](http://developer.android.com/intl/zh-cn/training/permissions/declaring.html)  
原文版权：[Creative Commons 2.5 Attribution License](http://creativecommons.org/licenses/by/2.5/)  
译文作者：Jianan - qinxiandiqi@foxmail.com  
版本信息：本文基于2016-04-27版本翻译  
译文版权：[CC BY-NC-ND 4.0](http://creativecommons.org/licenses/by-nc-nd/4.0/)，允许复制转载，但必须保留译文作者署名及译文链接，不得演绎和用于商业用途

<br>
# **前言**
---

每一个android app都运行在一个限制访问的沙盒中。如果一个app需要访问它所在沙盒之外的资源和信息，那么这个app就需要声明适当的权限。这个权限声明要求将你的app需要的权限全部列举在App的manifest文件中。
根据权限不同的隐私敏感程度级别，系统可能会自动授予该权限，也有可能需要请求设备用户授权才能获取该权限。例如，如果你的app请求授予打开设备闪光灯的权限，系统将会自动授予这个权限。但是，如果你的app需要读取用户的通讯录联系人，系统就会请求用户是否授予读取联系人的权限。根据android不同的系统版本，请求用户授予app权限的时机可能是在安装app的时候（在android 5.1或者更早的系统版本上），也有可能是在app运行的过程中（在android 6.0或者更高的系统版本上）。

<br>
# **判断你的App究竟需要哪些权限**
---

当你在开发app的时候，你应该注意使用那些需要申请权限的功能。通常情况下，一个app需要使用非它自己本身创建的信息或者资源，以及执行会影响设备或者其它app的操作时，它就需要申请权限。例如，如果一个app需要访问网络，使用设备摄像头，或者打开关闭wifi，那么这个app就需要对应的权限。系统的权限列表，请查看[普通等级和危险等级的权限](http://developer.android.com/guide/topics/security/permissions.html#normal-dangerous)。  
你的app只需要申请app直接执行的操作所需要的权限。如果你的app只是请求其它app来执行任务或者提供信息，那么你的app不需要申请这些任务或者信息所需要的权限。例如，如果你的app需要读取用户的地址簿，那么你需要申请**READ_CONTACT**权限。但是，如果你的app使用一个*Intent*来请求用户的通讯录app获取信息，你的app就不需要任何相关的权限，不过，通讯录的app就需要申请相关的权限。更多的详情，请参考[考虑使用Intent](http://developer.android.com/intl/zh-cn/training/permissions/best-practices.html#perms-vs-intents)。  

<br>
# **添加权限到Manifest**
---

声明app需要的权限，必须使用**&lt;uses-permission&gt;**标签在app的manifest文件中作为**&lt;manifest>**元素的子标签进行声明。例如，一个需要发送短信的app在manifest文件中的声明大致如下：  
``` xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
        package="com.example.snazzyapp">

    <uses-permission android:name="android.permission.SEND_SMS"/>


    <application ...>
        ...
    </application>

</manifest>
```

当你声明一个权限之后，系统的行为根据权限的隐私程度来决定。如果声明的权限不涉及用户的隐私，那么系统会自动授予这个权限。如果这个权限可能会涉及到用户的隐私信息，系统就会咨询用户是否要授予该权限的申请。更多关于不同权限类型的信息，请参考[普通等级和危险等级的权限](http://developer.android.com/guide/topics/security/permissions.html#normal-dangerous)。
