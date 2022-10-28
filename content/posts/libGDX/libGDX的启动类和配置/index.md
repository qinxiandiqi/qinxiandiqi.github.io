---
# type: posts 
title: "libGDX的启动类和配置"
date: 2018-07-22T18:26:50+0800
authors: ["Jianan"]
summary: "对于每个目标平台，我们都必须编写对应的启动类。这个类根据特定的底层平台实现`Application`接口，同时也实现提供应用逻辑代码的`ApplicationListener`接口。这个启动类依赖于具体的底层平台，下面让我们来看看启动类在每个底层平台上的实现和配置。"
series: ["libGDX", "libGDX手册"]
categories: ["翻译", "libGDX", "libGDX手册"]
tags: ["libGDX", "游戏引擎", "跨平台", "启动类", "初始化"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 1055
comment_num: 0
---

> 原文作者：libGDX  
原文地址：[https://github.com/libgdx/libgdx/wiki/Starter-classes-and-configuration](https://github.com/libgdx/libgdx/wiki/Starter-classes-and-configuration)   
译文作者：Jianan - qinxiandiqi@foxmail.com  
版本信息：本文基于2018-07-22版本翻译  
译文版权：[CC BY-NC-ND 4.0](http://creativecommons.org/licenses/by-nc-nd/4.0/)，允许复制转载，但必须保留译文作者署名及译文链接。

<br>
对于每个目标平台，我们都必须编写对应的启动类。这个类根据特定的底层平台实现`Application`接口，同时也实现提供应用逻辑代码的`ApplicationListener`接口。这个启动类依赖于具体的底层平台，下面让我们来看看启动类在每个底层平台上的实现和配置。  

本文假设你已按照[libGDX的项目设置、运行和调试](https://github.com/libgdx/libgdx/wiki/Project-setup,-running-&-debugging)一文中的说明进行操作，并已将生成的Core、桌面、Android和HTML5项目代码导入到Eclipse。（译注：该创建libGDX项目文章已经过时，新项目创建方法请参考[使用gradle创建libGDX项目](https://github.com/libgdx/libgdx/wiki/Project-Setup-Gradle)）

<br>
# 桌面平台 (基于LWJGL) 
---

打开`my-gdx-game`项目中的`Main.java`类，你会看到以下代码：  

```java
package com.me.mygdxgame;

import com.badlogic.gdx.backends.lwjgl.LwjglApplication;
import com.badlogic.gdx.backends.lwjgl.LwjglApplicationConfiguration;

public class Main {
   public static void main(String[] args) {
      LwjglApplicationConfiguration cfg = new LwjglApplicationConfiguration();
      cfg.title = "my-gdx-game";
      cfg.useGL30 = false;
      cfg.width = 480;
      cfg.height = 320;
		
      new LwjglApplication(new MyGdxGame(), cfg);
   }
}
```
上面的代码首先实例化了[LwjglApplicationConfiguration](https://github.com/libgdx/libgdx/tree/master/backends/gdx-backend-lwjgl/src/com/badlogic/gdx/backends/lwjgl/LwjglApplicationConfiguration.java)。此类允许设置各种配置，例如初始屏幕分辨率，是否使用OpenGL ES 2.0或3.0（当前还处于实验阶段）等。有关更多信息，请参阅此类的[Javadocs](https://libgdx.badlogicgames.com/nightlies/docs/api/com/badlogic/gdx/backends/lwjgl/LwjglApplicationConfiguration.html)。

设置好这个配置对象之后，下一步开始实例化`LwjglApplication`。它的构造方法包含一个`MyGdxGame()`类对象，这个MyGdxGame类实际上是实现了游戏逻辑的ApplicationListener接口实例。  

在这之后就会有一个窗口被创建出来，并开始执行ApplicationListener接口中的相关生命周期方法，详情请参考[libGDX的生命周期](https://blog.csdn.net/qinxiandiqi/article/details/80874868)。  

<br>
# 桌面平台 (基于LWJGL3) 
---

等待补充……

<br>
# Android平台
---

Android应用程序不使用`main()`方法作为启动类入口点，而是需要一个Activity。打开`my-gdx-game-android`项目中的`MainActivity.java`类：  

```java
package com.me.mygdxgame;

import android.os.Bundle;

import com.badlogic.gdx.backends.android.AndroidApplication;
import com.badlogic.gdx.backends.android.AndroidApplicationConfiguration;

public class MainActivity extends AndroidApplication {
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        
        AndroidApplicationConfiguration cfg = new AndroidApplicationConfiguration();
        
        initialize(new MyGdxGame(), cfg);
    }
}
```

Android的主要入口是Activity的`onCreate()`方法。请注意，上面代码中的MainActivity继承了`AndroidApplication`类，而`AndroidApplication`又继承类Activity。类似桌面平台的启动类，首先要创建配置实例[AndroidApplicationConfiguration](https://github.com/libgdx/libgdx/tree/master/backends/gdx-backend-android/src/com/badlogic/gdx/backends/android/AndroidApplicationConfiguration.java)。配置完成后，调用`AndroidApplication.initialize()`方法，与`ApplicationListener`实例（本例中是MyGdxGame）一起传入。有关配置类可用的设置参数，请参阅[AndroidApplicationConfiguration Javadocs](http://libgdx.badlogicgames.com/nightlies/docs/api/com/badlogic/gdx/backends/android/AndroidApplicationConfiguration.html)。  

Android应用程序可以有多个Activity，但Libgdx游戏通常只应包含一个Activity。如果游戏需要显示多个分屏，应通过libgdx来实现，而不是使用多个Activity。这样做的原因是创建一个新的Activity意味着创建一个新的OpenGL Context，这是非常耗时的，而且也意味着必须重新加载所有图形资源。

<br>
## 基于Fragment的libgdx
---

Android SDK引入了一个API，可以为屏幕的特定区域创建控制器，甚至可以在多个屏幕上轻松重复使用。此API称为[Fragments API](http://developer.android.com/guide/components/fragments.html)。Libgdx现在也可以以Fragment的形式引入，做为更大屏幕的一部分。要创建包含Libgdx的Fragment，fragment必须继承`AndroidFragmentApplication`，并使用下面方式重写`onCreateView()`方法：  
``` java
    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
        return initializeForView(new MyGdxGame());
    }
```

使用fragment引入libGDX的代码与上面使用Activity引入libGDX的步骤和代码有些许区别：  
1. 添加[Android V4](https://developer.android.com/tools/support-library/setup.html#libs-without-res)支持库。如果你的Android项目还没有添加这个支持库，那你必须添加这个依赖库以便后面使用FragmentActivity。
2. 修改AndroidLauncher继承FragmentActivity，而不是继承AndroidApplication。
3. AndroidLauncher这个activity需要实现AndroidFragmentApplication.Callbacks接口。
4. 创建继承AndroidFragmentApplication类的Fragment，这个fragment是libGDX的实现。
5. 添加上面提到的`initializeForView()`代码到Fragment的onCreateView方法中。
6. 最后，将AndroidLauncher Activity的content替换为这个libGDX fragment。  

例如:
```java
// 2. 修改AndroidLauncher继承FragmentActivity，而不是继承AndroidApplication。
// 3. AndroidLauncher activity实现AndroidFragmentApplication.Callbacks接口
public class AndroidLauncher extends FragmentActivity implements AndroidFragmentApplication.Callbacks
{
   @Override
   protected void onCreate (Bundle savedInstanceState)
   {
      super.onCreate(savedInstanceState);

      // 6. 最后，将AndroidLauncher Activity的content替换为这个libGDX fragment。
      GameFragment fragment = new GameFragment();
      FragmentTransaction trans = getSupportFragmentManager().beginTransaction();
      trans.replace(android.R.id.content, fragment);
      trans.commit();
   }

   // 4. 创建继承AndroidFragmentApplication类的Fragment，这个fragment是libGDX的实现。
   public static class GameFragment extends AndroidFragmentApplication
   {
      // 5. 添加initializeForView()到Fragment的onCreateView方法中。
      @Override
      public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
      {  return initializeForView(new MyGdxGame());   }
   }


   @Override
   public void exit() {}
}
```

### AndroidManifest.xml文件

除了`AndroidApplicationConfiguration`，Android应用程序还要对AndroidManifest.xml文件进行配置，该文件位于Android项目的根目录中。你大概需要进行以下的配置：

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.me.mygdxgame"
    android:versionCode="1"
    android:versionName="1.0" >

    <uses-sdk android:minSdkVersion="8" android:targetSdkVersion="15" />

    <application
        android:icon="@drawable/ic_launcher"
        android:label="@string/app_name" >
        <activity
            android:name=".AndroidLauncher"
            android:label="@string/app_name"
            android:screenOrientation="landscape"
            android:configChanges="keyboard|keyboardHidden|orientation">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```

#### 目标Sdk版本 ####

将targetSDKVersion设置为你要的目标Android版本。  

#### 屏幕方向和设备配置更改 ####

除了targetSdkVersion，activity的`screenOrientation`和`configChanges`属性通常也要设置。  

`screenOrientation`属性指定应用程序屏幕的固定方向。如果应用程序可以同时使用横向和纵向模式，则可以省略此项。  

`configChanges`属性*至关重要*，它应始终设置为上面显示的值。省略此属性意味着每次物理键盘滑出/插入或设备方向发生变化时，应用程序都将重新启动（译注：忽略这个属性，默认设备这些属性发生修改时会重新启动该Activity，由于libGDX基于Activity注入，Activity重启也意味着libGDX实例重启）。如果是`screenOrientation`属性被省略，libgdx应用程序将回调`ApplicationListener.resize()`方法以指示方向更改。然后，API客户端可以相应地重新调整应用程序的布局。  

#### 权限 ####

如果应用程序需要能够写入设备的外部存储器（例如SD卡），或者需要访问互联网，又或者需要使用振动器或想要录制音频，则需要将以下权限添加到`AndroidManifest.xml`文件中：  

```xml
	<uses-permission android:name="android.permission.RECORD_AUDIO"/>
	<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
	<uses-permission android:name="android.permission.VIBRATE"/>
```

用户通常对申请许多权限的应用程序感到疑惑，因此请明智地选择申请这些权限。 

为使唤醒锁定起作用，`AndroidApplicationConfiguration.useWakeLock`需要将其设置为true。  

如果游戏不需要加速计或指南针访问，建议通过将`AndroidApplicationConfiguration.useAccelerometer`和`AndroidApplicationConfiguration.useCompass`字段设置false来禁用它们 。  
 
如果你的游戏需要陀螺仪传感器，则必须设置`AndroidApplicationConfiguration.useGyroscope`为true（默认情况下它是被禁用，这样可以节省电量）。  

有关如何为应用程序设置图标等其他属性的详细信息，请参阅[Android开发人员指南](http://developer.android.com/guide/index.html)。  

<br>
## 动态壁纸   
---

Libgdx提供了一种简单易用的方法来为Android创建[动态壁纸](http://android-developers.blogspot.co.at/2010/02/live-wallpapers.html)。动态壁纸的启动类是`AndroidLiveWallpaperService`，下面是一个例子：  

```java
package com.mypackage;

// imports snipped for brevity 

public class LiveWallpaper extends AndroidLiveWallpaperService {
	@Override
	public ApplicationListener createListener () {
		return new MyApplicationListener();
	}

	@Override
	public AndroidApplicationConfiguration createConfig () {
		return new AndroidApplicationConfiguration();
	}

	@Override
	public void offsetChange (ApplicationListener listener, float xOffset, float yOffset, float xOffsetStep, float yOffsetStep,
		int xPixelOffset, int yPixelOffset) {
		Gdx.app.log("LiveWallpaper", "offset changed: " + xOffset + ", " + yOffset);
	}
}
```

当你的动态壁纸在选择器中显示或创建后在主屏幕上显示时，`createListener()`和`createConfig()`方法会被回调。

当用户在主屏幕上滑动屏幕时，`offsetChange()` 方法会被调用，并且告诉你当前屏幕偏离中心屏幕多少。这个方法会在渲染线程上执行，因此你不必同步任何内容。

除了启动类之外，你还必须创建一个描述壁纸的XML文件，我们暂且称之为livewallpaper.xml。在Android项目的`res/`资源文件夹下创建一个`xml/`文件夹，并将文件放在这个文件夹里面（`res/xml/livewallpaper.xml`）。下面是xml文件的内容：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<wallpaper
       xmlns:android="http://schemas.android.com/apk/res/android"  
       android:thumbnail="@drawable/ic_launcher"
       android:description="@string/description"
       android:settingsActivity="com.mypackage.LivewallpaperSettings"/>
```

文件中的`thumbnail`定义了你的动态壁纸在选择器中P显示的缩略图，`description`和`settingsActivity`定义了当用户点击动态壁纸选择器中的“设置”时将显示的描述和Activity。这Activity应该只是一个标准的Activity，它有一些小组件可以更改动态壁纸的一些设置，例如背景颜色和类似的东西。你可以将这些设置存储在SharedPreferences中，稍后就可以在动态壁纸的ApplicationListener中通过`Gdx.app.getPreferences()`加载它们。

最后，你需要在AndroidManifest.xml文件中添加内容。以下是包含简单设置Activity的动态壁纸的示例：

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
      package="com.mypackage"
      android:versionCode="1"
      android:versionName="1.0"
      android:installLocation="preferExternal">
	<uses-sdk android:minSdkVersion="7" android:targetSdkVersion="14"/>	
	<uses-feature android:name="android.software.live_wallpaper" />
		
	<application android:icon="@drawable/icon" android:label="@string/app_name">
		<activity android:name=".LivewallpaperSettings" 
				  android:label="Livewallpaper Settings"/>
		
		<service android:name=".LiveWallpaper"
            android:label="@string/app_name"
            android:icon="@drawable/icon"
            android:permission="android.permission.BIND_WALLPAPER">
            <intent-filter>
                <action android:name="android.service.wallpaper.WallpaperService" />
            </intent-filter>
            <meta-data android:name="android.service.wallpaper"
                android:resource="@xml/livewallpaper" />
        </service>				  	
	</application>
</manifest> 
```

这个manifest定义了：

  * 它要使用动态壁纸功能，请参阅`<uses-feature>`标签。
  * 允许绑定壁纸的权限，请参阅`android:permission`属性。
  * 指定壁纸设置的Activity
  * 指定livewallpaper的service，它指向livewallpaper.xml文件，请参阅`meta-data`。

注意动态壁纸从Android 2.1 (SDK level 7)之后才开始支持。

动态壁纸在触摸输入方面有一些限制。通常只会报告点击/拖放事件。如果你想要接收全部触摸事件，你可以将`AndroidApplicationConfiguration#getTouchEventsForLiveWallpaper`标志设为true以接收完整的多点触控事件。

<br>
## Daydreams（VR）
---

从Android 4.2开始，当设备处于空闲或停靠状态时，用户可以设置[Daydreams](http://developer.android.com/about/versions/android-4.2.html#Daydream)（译注：Google在Android上引入的VR平台）显示出来。这些Daydreams类似于屏保，可以用来显示相册等内容。Libgdx可以让你轻松写出这样的Daydreams应用。  

Daydream的启动类是AndroidDaydream。下面是一个例子：

```java
package com.badlogic.gdx.tests.android;

import android.annotation.TargetApi;
import android.util.Log;

import com.badlogic.gdx.ApplicationListener;
import com.badlogic.gdx.backends.android.AndroidApplicationConfiguration;
import com.badlogic.gdx.backends.android.AndroidDaydream;
import com.badlogic.gdx.tests.MeshShaderTest;

@TargetApi(17)
public class Daydream extends AndroidDaydream {
   @Override
   public void onAttachedToWindow() {
      super.onAttachedToWindow();      
	  setInteractive(false);

      AndroidApplicationConfiguration cfg = new AndroidApplicationConfiguration();
      ApplicationListener app = new MeshShaderTest();
      initialize(app, cfg);
   }
}
```

只需要继承AndroidDaydream，重写onAttachedToWindow方法，添加配置和ApplicationListener就可以初始化你的Daydream。

除了Daydream本身，你还可以提供配置用的Activity，让用户配置你的Daydream。这可以是普通的Activity，也可以是libgdx的`AndroidApplication`。以空Activity为例：  

```java
package com.badlogic.gdx.tests.android;

import android.app.Activity;

public class DaydreamSettings extends Activity {

}
```

必须将此配置Activity指定为Daydream服务的metadata。在Android项目的res/xml文件夹中创建一个xml文件，并指定如下activity：  

```xml
<dream xmlns:android="http://schemas.android.com/apk/res/android"
 android:settingsActivity="com.badlogic.gdx.tests.android/.DaydreamSettings" />
```

最后，像往常一样在AndroidManifest.xml中添加配置activity的部分，以及daydream的service描述，如下所示：  

```xml
<service android:name=".Daydream"
   android:label="@string/app_name"
   android:icon="@drawable/icon"
   android:exported="true">
   <intent-filter>
	   <action android:name="android.service.dreams.DreamService" />
	   <category android:name="android.intent.category.DEFAULT" />
   </intent-filter>
   <meta-data android:name="android.service.dream"
	   android:resource="@xml/daydream" />
</service>
```

<br>
# iOS/Robovm平台
---

等待补充……  

<br>
# HTML5/GWT平台
---

HTML5/GWT应用程序的主要启动类是`GwtApplication`。打开my-gdx-game-html5项目中的`GwtLauncher.java`类：  

```java
package com.me.mygdxgame.client;

import com.me.mygdxgame.MyGdxGame;
import com.badlogic.gdx.ApplicationListener;
import com.badlogic.gdx.backends.gwt.GwtApplication;
import com.badlogic.gdx.backends.gwt.GwtApplicationConfiguration;

public class GwtLauncher extends GwtApplication {
   @Override
   public GwtApplicationConfiguration getConfig () {
      GwtApplicationConfiguration cfg = new GwtApplicationConfiguration(480, 320);
      return cfg;
   }

   @Override
   public ApplicationListener createApplicationListener () {
      return new MyGdxGame();
   }
}
```

启动类主要由两个方法组成，`GwtApplication.getConfig()`和`GwtApplication.createApplicationListener()`。 前者必须返回一个[GwtApplicationConfiguration](https://github.com/libgdx/libgdx/tree/master/backends/gdx-backends-gwt/src/com/badlogic/gdx/backends/gwt/GwtApplicationConfiguration.java)实例， 这个实例指定HTML5应用程序的各种配置。`GwtApplication.createApplicatonListener()`方法为程序运行提供 `ApplicationListener`实例.

### 模块文件 ###

GWT需要引用每个jars/projects目录下的Java代码。此外，每个jars/projects都需要有一个模块定义文件，后缀为gwt.xml。

在示例项目的设置中，html5项目的模块文件如下所示：  

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE module PUBLIC "-//Google Inc.//DTD Google Web Toolkit trunk//EN" "http://google-web-toolkit.googlecode.com/svn/trunk/distro-source/core/src/gwt-module.dtd">
<module>
   <inherits name='com.badlogic.gdx.backends.gdx_backends_gwt' />
   <inherits name='MyGdxGame' />
   <entry-point class='com.me.mygdxgame.client.GwtLauncher' />
   <set-configuration-property name="gdx.assetpath" value="../my-gdx-game-android/assets" />
</module>
```

上面指定了另外两个继承的模块（gdx-backends-gwt模块和Core项目模块），指明启动类（GwtLauncher），以及相对于HTML5项目根目录的assets资源目录。

注意，gdx-backend-gwt模块和Core项目都有一个类似的模块定义文件，里面可能指定了其他依赖项。*你不能使用不包含模块文件和源文件的jars/projects模块！* 

有关模块和依赖关系的更多信息，请参阅[GWT开发人员指南](https://developers.google.com/web-toolkit/doc/1.6/DevGuide)。

### 反射支持 ###

由于各种原因，GWT不支持Java反射。Libgdx有一个内部仿真层，它将为少数几个内部类生成反射信息。这意味着如果你使用libgdx的[Json序列化](https://github.com/libgdx/libgdx/wiki/Reading-%26-writing-JSON)功能，你将遇到问题。你可以通过指定要为哪些包和类生成反射信息来解决此问题。为此，你可以将配置属性放在GWT项目的gwt.xml文件中，如下所示：

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<module>
    ... other elements ...
    <extend-configuration-property name="gdx.reflect.include" value="org.softmotion.explorers.model" />
    <extend-configuration-property name="gdx.reflect.exclude" value="org.softmotion.explorers.model.HexMap" />
</module>
```
你可以通过添加更多extend-configuration-property元素来添加多个包和类。  

此功能目前属于实验性使用，使用风险自负。

### 载入画面 ###

libgdx HTML5应用程序会预加载所有能在`gdx.assetpath`中找到的资源。在此加载过程中，将显示一个加载画面，该加载画面通过GWT小部件实现。如果要自定义此加载画面，只需覆盖该`GwtApplication.getPreloaderCallback()`方法（在上例中，该方法在`GwtLauncher`类中）。以下示例使用Canvas绘制一个非常简单，丑陋的加载画面：  

```java
long loadStart = TimeUtils.nanoTime();
public PreloaderCallback getPreloaderCallback () {
  final Canvas canvas = Canvas.createIfSupported();
  canvas.setWidth("" + (int)(config.width * 0.7f) + "px");
  canvas.setHeight("70px");
  getRootPanel().add(canvas);
  final Context2d context = canvas.getContext2d();
  context.setTextAlign(TextAlign.CENTER);
  context.setTextBaseline(TextBaseline.MIDDLE);
  context.setFont("18pt Calibri");

  return new PreloaderCallback() {
	 @Override
	 public void done () {
		context.fillRect(0, 0, 300, 40);
	 }

	 @Override
	 public void loaded (String file, int loaded, int total) {
		System.out.println("loaded " + file + "," + loaded + "/" + total);
		String color = Pixmap.make(30, 30, 30, 1);
		context.setFillStyle(color);
		context.setStrokeStyle(color);
		context.fillRect(0, 0, 300, 70);
		color = Pixmap.make(200, 200, 200, (((TimeUtils.nanoTime() - loadStart) % 1000000000) / 1000000000f));
		context.setFillStyle(color);
		context.setStrokeStyle(color);
		context.fillRect(0, 0, 300 * (loaded / (float)total) * 0.97f, 70);
		
		context.setFillStyle(Pixmap.make(50, 50, 50, 1));
		context.fillText("loading", 300 / 2, 70 / 2);
	 }

	 @Override
	 public void error (String file) {
		System.out.println("error: " + file);
	 }
  };
}
```

请注意，你只能使用纯GWT工具来显示加载画面，libgdx相关的API要在预加载完成后才可用。

[上一章|libGDX的模块](https://blog.csdn.net/qinxiandiqi/article/details/81054305)  
[下一章|查询libGDX运行环境相关信息](https://blog.csdn.net/qinxiandiqi/article/details/81427729)  