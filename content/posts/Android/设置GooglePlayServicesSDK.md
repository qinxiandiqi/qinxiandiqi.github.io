---
# type: posts 
title: "设置Google Play Services SDK(Set Up Google Play Services SDK)"
date: 2014-06-09T18:35:28+08:00
authors: ["Jianan"]
summary: "Google Play Services SDK的初始化。"
series: ["Android"]
categories: ["翻译", "Android"]
tags: ["Android", "翻译", "Google Play Service"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 18457
comment_num: 1
---


> 原文作者：Google  
原文地址：http://developer.android.com/google/play-services/setup.html  
原文版权：Creative Commons 3.0 Attribution License  
译文作者：Jianan - qinxiandiqi@foxmail.com  
版本信息：本文基于2014-06-09版本开始翻译，基于2015-10-17版本重新翻译  
​译文版权：CC BY-NC-ND 4.0，允许复制转载，但必须保留译文作者署名及译文链接，不得演绎和用于商业用途

​
## 一、前言
开发一个使用[Google Play services API](https://developers.google.com/android/reference/packages)的应用，你需要为你的项目配置Google Play services SDK。如果还没有你还没有安装Google Play services SDK，现在你可以按照[Android SDK Packages指南](https://developer.android.com/sdk/installing/adding-packages.html)来获取。

为了能够测试使用Google Play Services的应用，你需要具备：
- 一个运行Android2.3或者更高系统版本，并且包含google Play stroe的Android设备
- 一个运行基于Android4.2.2或者更高版本google APIs平台系统的Android虚拟机
## 二、添加Google Play Services到你的项目中（Add Google Play Services to Your Project）
根据你的开发环境选择下个面的步骤添加Google Play Services到你的项目中：

### 2.1 Android Studio
#### 2.1.1 配置你的app可使用Google Play Services API：
1. 打开你的应用module中的build.grade文件。
注意：Android Studio项目包含一个顶级build.gradle文件以及每个module中各包含一个build.grade文件。确认编辑的是你的应用module中的文件。参考Building Your Project with Gradle获取更多耿玉Gradle的内容。
2. 在dependencies下面添加一个play-services最新版本的构建规则。例如
    ```gradle
        apply plugin:'android'
        ...
        dependencies{
            compile 'com.google.android.gms:play-services:8.1.0'
        }
    ```

    当你每次更新Google Play services版本之后，你需要取保更新这里的版本号。

    注意：如果你的app中引用的的方法数量超过了65K限制，你的app可能会编译失败。为了解决这个问题，你可以通过只选择你的app需要的Google Play services API去参与编译，而不是全部api都包含进去。更多关于此做法的信息请参考下面选择性编译API到你的可执行程序中章节。
3. 保存修改，并且点击工具栏中的Sync Project with Gradle Files按钮。

现在你开始使用Google Play services API的功能进行开发了。

#### 2.1.2 选择性编译API到你的可执行程序中（Selectively compiling APIs into your executable）
在Google Play Services 6.5之前的版本，你必须将整个api的包全部编译到你的app里面去。在一些情况下，这么做很难保持app中全部方法的数量在65536这个限制之下（包括framework的api，依赖库的api，以及你自己项目的代码）。

从6.5版本开始，你可以有选择的编译部分Google Play service API到你的app中。例如，如果只是需要Google Fit和Android Wear API，那么你可以在build.grade文件中做以下替换：

原来是：
```gradle
compile 'com.google.android.gms:play-services:8.1.0'
```
替换成：
```gradle
compile 'com.google.android.gms:play-services-fitness:8.1.0'
compile 'com.google.android.gms:play-services-wearable:8.1.0'
```
表1展示了一个分离的API列表，表中描述了你可以独立选择编译到你的app中的模块，以及如何在build.gradle文件中声明。有部分API没有一个单独的library，它们被包含在基础library里面（当你需要用到一个没有分离出来的API时，这个基础library会自动包含进来。）

表1：独立的API模块以及相应的build.grade描述

| Google Play services API | 在build.grade中的描述 |
|--|------------|
| Google+ | com.google.android.gms.play-services-plus:8.1.0 |
|Google Account Login|com.google.android.gms.play-services-identity:8.1.0|
|Google Actions,Base Client Library|com.google.android.gms.play-services-base:8.1.0|
|Google App Indexing|com.google.android.gms.play-services-appindexing:8.1.0|
|Google App Invites|com.google.android.gms.play-services-appinvite:8.1.0|
|Google Analytics|com.google.android.gms.play-services-analytics:8.1.0|
|Google Cast|com.google.android.gms.play-services-cast:8.1.0|
|Google Cloud Messaging|com.google.android.gms.play-services-gcm:8.1.0|
|Google Drive|com.google.android.gms.play-services-drive:8.1.0|
|Google Fit|com.google.android.gms.play-services-fitness:8.1.0|
|Google Location,Activity Recognition,and Places|com.google.android.gms.play-services-location:8.1.0|
|Google Map|com.google.android.gms.play-services-maps:8.1.0|
|Google Mobile Ads|com.google.android.gms.play-services-ads:8.1.0|
|Mobile Vision|com.google.android.gms.play-services-vision:8.1.0|
|Google Nearby|com.google.android.gms.play-services-nearby:8.1.0|
|Google Panorama Viewer|com.google.android.gms.play-services-panorama:8.1.0|
|Google Play Game services|com.google.android.gms.play-services-games:8.1.0|
|SafetyNet|com.google.android.gms.play-services-safetynet:8.1.0|
|Google Wallet|com.google.android.gms.play-services-wallet:8.1.0|
|Android Wear|com.google.android.gms.play-services-wearable:8.1.0|

注意：在Google Play services的library中已经包含了ProGuard指令用于保留需要的类。Android Gradle Plugin会自动添加一个AAR（Android压缩包）包中的ProGuard配置文件，并将这个包添加到你的ProGuard配置里面。在项目的创建过程中，Android Studio会自动创建这个ProGuard配置文件以及build.grade属性供ProGuard（程序）使用。在Android Studio中使用ProGuard，你必须在build.grade文件中的buildTypes里面启用[ProGuard](https://developer.android.com/tools/help/proguard.html)设置。更多信息，请参考ProGuard指南。

### 2.2 Eclipse(ADT插件)或其它方式
#### 2.2.1 配置你的app可使用Google Play services API：
1. 将<android-sdk\>/extras/google/google_play_services/libproject/google-play-services_lib路径下的项目复制到你维护你的app项目的目录下。
2. 将这个library项目导入到你的Eclipse工作空间。点击File>Import，选择Android>Existing Android Code into Workspace，然后浏览选择复制过来的library项目并导入。
3. 在你的app项目里面，链接引入Google Play services项目。参考[使用Eclipse和ADT插件管理Android项目](http://blog.csdn.net/qinxiandiqi/article/details/30033357)或者[使用命令行链接library项目](https://developer.android.com/tools/projects/projects-cmdline.html#ReferencingLibraryProject)获取更多相关信息。  
注意：你必须链接复制到你开发工作空间的库项目，不能直接链接到Android SDK目录下的库项目（译者注：SDK Manager更新的Google Play Services版本的时候可能会有变化，所以不建议）。
4. 当你将Google Play services库项目作为依赖库链接到你的应用项目之后，你需要打开你项目中的manifest文件，并且添加以下子标签到<application>标签下：
    ```xml
    <meta-data android:name="com.google.android.gms.version"
	    androdi:value="@integer/google_play_services_version"/>
    ```
    一旦你将库项目链接到你的项目，你就可以开始使用Google Play service APIs提供的功能进行开发。

### 2.2.2 添加Proguard排除
为了防止ProGuard剥离了一些需要的类，需要在/proguard-project.txt文件中添加以下内容：
```proguard
-keep class * extends java.util.ListResourceBundle {
    protected Object[][] getContents();
}

-keep public class com.google.android.gms.common.internal.safeparcel.SafeParcelable {
    public static final *** NULL;
}

-keepnames @com.google.android.gms.common.annotation.KeepName class *
-keepclassmembernames class * {
    @com.google.android.gms.common.annotation.KeepName *;
}

-keepnames class * implements android.os.Parcelable {
    public static final ** CREATOR;
}
```
## 三、确保设备安装了Google Play services APK（Ensure Devices Have the Google Play services APK）
正如[Google Play services概述](https://developers.google.com/android/guides/overview.html)中所说的，在Android 2.3或者更高版本中，Google Play通过Google Play Store应用为用户提供升级服务。但是，可能不是所有用户都能立刻收到更新。因此，在你尝试使用API之前，你应该确认当前版本是可用的。  
    重要：由于很难确定每一台设备的情况，你应该经常在使用Google Play services功能之前检查Google Play services APK的版本是否兼容。

由于每个应用对Google Play services的使用不同，你可以根据需要决定检查Google Play services版本的时机。例如，如果你的应用在各种情况下都需要使用到Google Play services，你可能就需要在应用一启动的时候就做检查。如果Google Play services只是你应用的一个可选部分，那么可以只在用户进入到需要使用Google Play services部分的时候再做检查。

强烈建议你使用GoogleApiClient类来连接Google Play services的功能。这种方法允许你添加一个OnConnectionFailedListenter到你的GoogleApiClient，实现其中的onConnectionFailed()回调方法就能判断当前设备是否有安装合适版本的Google Play services APK。如果由于Google Play APK没有安装或者版本过时就会导致连接失败，回调可能会收到类似SERVICE_MISSING、SERVICE_VERSION_UPDATE_REQUIRED，或者SERVICE_DISABLED这样的错误码。学习更多关于如何创建自己的GoogleApiClient并处理这样的连接错误，请参考连接[Google APIs](https://developers.google.com/android/guides/api-client.html).

还有其它方法就是通过使用isGooglePlayServicesAvailable()方法。你可以在主Activity的onResume()方法中调用这个方法。如果返回的结果是SUCCESS，说明Google Play services APK的版本是最新的，你可以继续创建一个连接。如果返回的结果是SERVICE_MISSING,SERVICE_VERSION_UPDATE_REQUIRED或者SERVICE_DISABLED，那么用户将需要去安装更新。在这种情况下，可以调用GooglePlayServicesUtil.getErrorDialog()方法并且将错误码传递进去。这个方法会返回一个对话框，它会显示对应的错误信息，并且提供一个操作将用户引导去Google Play Store安装更新。


接下来开始一个Google Play service的连接（大部分Google API，如Google Drive、Google+、Games等，都需要这个连接）。请参考连接[Google APIs](https://developers.google.com/android/guides/api-client.html)。

​

​
