---
# type: posts 
title: "Android Wear Preview - Get Started With Developer Preview"
date: 2014-06-15T13:59:45+0800
authors: ["Jianan"]
summary: "Android Wear开发者预览版包含了tools和APIs，你可以通过它们在可穿戴Android设备上提供优秀的用户体验来增强你的应用程序的信息通知功能。

使用Android Wear开发者预览版，你可以：
在Android模拟器上运行Android Wear平台。
把你的Android设备和模拟器连接，并将Android设备上的信息作为Android Wear上的一张卡片来查看。
试用预览版支持库中的新API来增强你的应用程序通知功能，例如语音回复和信息通知页。

获取开发者预览版工具，你需要点击一"
series: ["Android Wear"]
categories: ["翻译", "Android Wear"]
tags: ["Android", "Android Wear", "可穿戴设备", "google"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 2274
comment_num: 2
---

\----------------------------------------------------------------------------------------------------------------------------------------------------------

原文作者：Google

原文地址：<http://developer.android.com/wear/preview/start.html>[](https://developers.google.com/maps/documentation/android-
api/intro)

原文版权：[Creative Commons 2.5 Attribution
License](http://creativecommons.org/licenses/by/2.5/)

译文作者：Jianan - qinxiandiqi@foxmail.com

版本信息：本文基于2014-06-15版本翻译

译文版权：[CC BY-NC-ND 4.0](http://creativecommons.org/licenses/by-nc-
nd/4.0/)，允许复制转载，但必须保留译文作者署名及译文链接，不得演绎和用于商业用途

\----------------------------------------------------------------------------------------------------------------------------------------------------------

  

# 前言

  

Android
Wear开发者预览版包含了tools和APIs，你可以通过它们在可穿戴Android设备上提供优秀的用户体验来增强你的应用程序的信息通知功能。

  
使用Android Wear开发者预览版，你可以：

  * 在Android模拟器上运行Android Wear平台。
  * 把你的Android设备和模拟器连接，并将Android设备上的信息作为Android Wear上的一张卡片来查看。
  * 试用预览版支持库中的新API来增强你的应用程序通知功能，例如语音回复和信息通知页。

  
获取开发者预览版工具，你需要点击一下注册按钮，然后按照注册流程提示操作。

  

[Sign Up for the Developer
Preview](http://developer.android.com/wear/preview/signup.htmlhttp://developer.android.com/wear/preview/signup.html)

  
注册之后，你将可以使用：

  * 预览版支持库中的新通知API
  * 使用新通知APIs的示例应用
  * 安装到你移动设备上的Android Wear Preview app（Android Wear预览版应用），这个应用用于连接你的设备和Android Wear模拟器。

  
注意：当前的Android
Wear开发者预览版只是用于开发和测试目的，不能用于发布产品。Google可能会大量修改该开发者预览版作为正式发行的Android Wear
SDK。你不用使用这个开发者预览版来公开发行你的应用。同样，当正式版SDK发行之后开发者预览版将不再被支持，这可能会导致使用开发者预览版的应用程序无法使用。

  

# Prerequisites（基本要求）

  

在你开始使用之前，你必须：

1、安装Android SDK：

Android SDK包含了所有构建一个Android项目需要的所有开发工具（同样提供了可使用的IDE工具）

2、注册Android Wear开发者预览版

你必须使用一个Gmail或者其它Google账号登记注册才能够下载预览版支持库和在Google Play Store上接收到Android Wear
Preview app。

  
注意：如果你使用ADT插件的Eclipse，你必须将ADT插件升级到22.6.1或者更高的版本。如果你使用的是Android
Studio，你必须更新到0.5.1或者更高版本。

  

# 1、Install the Android Wear System Image（安装Android Wear系统映像）

  

1、启动Android SDK Manager。

  * 如果是Eclipse，选择Window > Android SDK Manager
  * 如果是Android Studio，选择Tools > Android > SDK Manager

2、Tools下面，确认你使用ADT是22.6或者更高版本。

如果您的ADT版本低于22.6，你必须进行更新：

  1. 选择Android SDK Tools
  2. 点击Install package
  3. 同意license并点击Install
  4. 安装完成之后，重启Android SDK Manager

3、Android 4.4.2下面，选择Android Wear ARM EABI v7a System Image

注意：Android Wear的设计支持多核心系统架构。

4、Extra下面，确认你拥有最新版本的Android Support Library。如果有一个更新是用的，请选择Android Support
Library。如果你使用的的是Android Studio，请同时选择Android Support Repository。

5、点击Install package。

6、同意License并点击Install。

  

# 2、Set Up the Android Wear Emulator（配置Android Wear模拟器）

  

1、启动Android Virtual Device Manager（Android AVD Manager）

  * 如果是Eclipse，选择Window > Android Virtual Device Manager
  * 如果是Android Studio，选择Tools > Android > AVD Manager

2、点击New按钮

3、在AVD
Name一栏中，根据你要创建一个方形显示界面还是圆形显示界面，输入“AndroidWearSquare”或者“AndroidWearRound”。

4、Device一栏中，选择Android Wear Square或者Android Wear Round

5、Target一栏中，选择Android 4.4.2 - API Level 19（或者更高版本）

6、CPU/ABI一栏中，选择Android Wear ARM（armeabi-v7a）

注意：Android Wear的设计支持多核心处理器架构。

7、Skin一栏中，选择AndroidWearSquare或者AndroidWearRound。

8、其他的选项使用默认设置，然后点击OK按钮。

尽管真正的Android可穿戴设备没有提供键盘作为输入方法，你也应该保持勾选Hardware keyboard
present选项，以便你能够替代语音输入方法使用键盘输入文字到屏幕中。

9、在AVD列表中，选择你刚刚创建的条目，点击Start按钮。接下来弹出的窗口中，点击Launch按钮。

  
现在Android Wear模拟器已经启动，在开始测试你的应用通知之前，你必须在你的开发设备上安装Android Wear Preview
app（Android Wear预览版应用）并配对模拟器。

  
提示：为了提升模拟器的启动时间，你可以编辑你的AVD，在模拟器选项中打开Snapshot共鞥。当你启动模拟器的时候用，勾选Save to
snapshot再点击Launch按钮。一旦模拟器在运行的时候被关闭，这将会保存一个系统快照。再一次启动AVD，但是选择Launch from
snapshot并取消Save to snapshot。

  
注意：不要在Android Wear模拟器上安装应用。Android Wear系统不支持传统的Android应用，如果运行可能导致不可预知的错误。

  

# 3、Set Up the Android Wear Preview App（配置Android Wear预览版应用）

  

在Android Wear模拟器中查看你的应用通知信息，你必须在你的Android设备上（手机或者平板）安装Android Wear Preview应用。

  
获取Android Wear Preview app，你必须使用与Google Play
Store相同的Gmail或者其它google账号[注册开发者预览版](http://developer.android.com/wear/preview/signup.html)。  
注意：Android Wear Preview app使用Android4.3或者更高版本编译的，并且不能安装在Android模拟器上。

  
当你注册了开发者预览版之后，你将会收到一封确认邮件，里面包含了一个关于Android Wear Preview
app测试用程序的确认链接。一旦你确认，你有24小时的时间从Google Play Store上安装这个应用。

  
当你安装了Android Wear Preview app之后，你可以配置你的设备与Android Wear模拟器互相通信。

1、打开Android Wear Preview
app。你可以看到一个通知信息表明这个应用当前没有打开通知监听功能。点击这个信息打开系统设置，选择Android Wear
Preview，授予它访问通知的权限。

2、使用USB将你的设备连接上的你开发机器（计算机）。确认没有其它设备连接这台机器（计算机）。

3、确认Android Wear模拟器（前面章节创建的模拟器）已经运行。模拟器应该会显示时间和一个图标来显示当前没有设备连接。

4、打开一个命令行终端，将当前路径导航到Android SDK的platform-tools/目录下，然后运行

adb -d forward tcp:5601 tcp:5601

注意：每次使用USB连接你的设备都需要运行这行命令。

5、返回到Android Wear Preview app。它应该会显示已经连接到模拟器上。Android
Wear模拟器应该会显示一个‘g’图标，说明模拟器已经连接上你的设备。

  
现在，你设备上的通知也会显示在Android Wear模拟器上。

  

# 4、Add the Support Library to Your Project（添加支持库到你的项目中）

  

Android Wear perview support library（Android
Wear预览版支持库）包含了一些一些API可以帮助你优化你的应用的通知功能来获得更好的Android Wear用户体验。

  
获取预览版支持库，你必须[注册开发者预览版](http://developer.android.com/wear/preview/signup.html)。你注册后收到的确认邮件中包含了一个下载ZIP文件的链接，这个ZIP包含了预览版支持库和一些实例应用。

  
当你下载并解压了包之后，将preview support library（预览版支持库）添加到你的Android项目中。

  
如果你使用的是Eclipse：

  1. 在你的Android应用项目中，在项目根部目录下（即与AndroidManifest.xml文件相同的路径下）创建一个libs/目录。
  2. 将你的Android SDK目录中的v4支持库（例如<sdk>extras/android/support/v4/android-support-v4.jar）复制到你项目中的libs/目录中。
  3. 同样添加wearable-preview-support.jar文件到libs/目录中。
  4. 右键点击每一个JAR文件并且选择Build Path > Add to Build Path.

  
如果你使用的是Android Studio：

1、在你的Android应用项目中，在项目根部目录下（即与AndroidManifest.xml文件相同的路径下）创建一个libs/目录。

2、添加wearable-preview-support.jar文件到libs/目录下。

3、打开你的应用module下面的build.gradle文件。

4、添加包括v4支持库和Android Wear预览版支持库的依赖规则：

    
    
    dependencies {
    
        compile "com.android.support:support-v4:18.0.+"
    
        compile files('../libs/wearable-preview-support.jar')
    
    }

5、点击工具栏中的Sync Project with Gradle Files按钮。

  
为Android Wear优化你的应用通知功能，请参考[Creating Notifications for Android
Wear](http://developer.android.com/wear/notifications/creating.html)。

