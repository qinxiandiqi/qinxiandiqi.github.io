---
# type: posts 
title: "libGDX的应用框架"
date: 2018-07-01T14:34:08+0800
authors: ["Jianan"]
summary: "作为libGDX的核心（译注：也是libGDX实现跨平台的基础），libGDX提供了6个通用接口来与具体的操作系统进行交互，不同的操作系统对这6个接口有不同的具体实现。"
series: ["libGDX", "libGDX手册"]
categories: ["翻译", "libGDX", "libGDX手册"]
tags: ["libGDX", "open GL", "游戏引擎", "java", "跨平台"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 299
comment_num: 0
---

> 原文作者：libGDX  
原文地址：[https://github.com/libgdx/libgdx/wiki/The-application-framework](https://github.com/libgdx/libgdx/wiki/The-application-framework)   
译文作者：Jianan - qinxiandiqi@foxmail.com  
版本信息：本文基于2018-06-30版本翻译  
译文版权：[CC BY-NC-ND 4.0](http://creativecommons.org/licenses/by-nc-nd/4.0/)，允许复制转载，但必须保留译文作者署名及译文链接，不得演绎和用于商业用途。

<br>
# 模块
---

作为libGDX的核心（译注：也是libGDX实现跨平台的基础），libGDX提供了6个通用接口来与具体的操作系统进行交互，不同的操作系统对这6个接口有不同的具体实现。 

  * *[Application](https://github.com/libgdx/libgdx/tree/master/gdx/src/com/badlogic/gdx/Application.java)接口*：运行的应用程序并负责通知API终端（译注：实现相关API，接收libGDX提供服务的处理终端程序，例如你的游戏实现程序）与应用程序级别相关的事件，例如窗口尺寸变化事件。它也提供了一些日志工具和查询方法，例如内存使用情况等。  
  * *[Files](https://github.com/libgdx/libgdx/tree/master/gdx/src/com/badlogic/gdx/Files.java)接口*：暴露具体平台底层文件系统的接口。它提供了一些抽象方法，用于获取在libGDX自定义的文件系统中不同路径来源的文件，需要注意的是这些文件不是直接使用Java的File类。（译注：简单来说，libGDX自己定义了一套处理文件的文件系统和接口，这个文件系统在不同的平台上进行不同的实现，以屏蔽不同平台的文件系统细节，从而实现跨平台）。  
  * *[Input](https://github.com/libgdx/libgdx/tree/master/gdx/src/com/badlogic/gdx/Input.java)接口*：负责将用户的输入通知API终端的接口，例如鼠标、键盘、触屏或者加速度计事件。它支持轮询和事件驱动处理模式。 
  * *[Net](https://github.com/libgdx/libgdx/tree/master/gdx/src/com/badlogic/gdx/Net.java)接口*：提供通过HTTP/HTTPS跨平台访问资源方式的接口，它也包括创建TCP服务和socket客户端方式。 
  * *[Audio](https://github.com/libgdx/libgdx/tree/master/gdx/src/com/badlogic/gdx/Audio.java)接口*：提供播放音效和音乐流的接口，通过这些接口就好像你可以直接访问PCM音频输入/输出设备一样。
  * *[Graphics](https://github.com/libgdx/libgdx/tree/master/gdx/src/com/badlogic/gdx/Graphics.java)接口*：暴露OpenGL ES 2.0(如果可用的话)的接口，它允许查询或者设置视频的模式或者其它类似的操作。  

<br>
# 启动类
---

针对特定平台需要编写特定的代码，这段代码也叫做程序的启动类。对于每一个特定的平台都需要一段特定的代码来创建一个实现Application接口的类实例（这个Application接口实现类具体如何实现是由运行时依赖的平台决定的）。例如桌面平台的这段启动类代码可能如下，它的底层使用Lwjgl作为依赖平台： 

```java
public class DesktopStarter {
   public static void main(String[] argv) {
      LwjglApplicationConfiguration config = new LwjglApplicationConfiguration();
      new LwjglApplication(new MyGame(), config);
   }
}
```

对于Android平台，相应的启动类代码可能如下：  

```java
public class AndroidStarter extends AndroidApplication {
   public void onCreate(Bundle bundle) {
      super.onCreate(bundle);
      AndroidApplicationConfiguration config = new AndroidApplicationConfiguration();
      initialize(new MyGame(), config);
   }
}
```

这两个类的代码通常放在不同的project里面，例如一个桌面project和一个Android project。在[Project的设置，运行和调试](https://github.com/libgdx/libgdx/wiki/Project-setup,-running-&-debugging)章节会介绍在Eclipse工程项目里面这些project文件的布局。 

应用程序的实际逻辑代码是放在[ApplicationListener](https://github.com/libgdx/libgdx/tree/master/gdx/src/com/badlogic/gdx/ApplicationListener.java)接口的实现类里面（在上面的例子里，这个实现类是MyGame）。这个类的实例会被传递到底层平台的Application接口实现类的初始化方法。Application会在适当的时机回调ApplicationListener的相关方法（具体参考[libGDX的生命周期](https://blog.csdn.net/qinxiandiqi/article/details/80874868)）。 

参考[libGDX的启动类和配置](https://github.com/libgdx/libgdx/wiki/Starter-classes-and-configuration)章节查看具体的入口类细节。  

<br>
# 访问模块
---

上面提到的libGDX模块可以通过[Gdx](https://github.com/libgdx/libgdx/tree/master/gdx/src/com/badlogic/gdx/Gdx.java)类的静态变量访问。这实际上是一些全局变量，以便能够简单访问到libGDX的各个模块。虽然这一般被认为是一种很糟糕的代码实现，但我们仍然决定使用这种机制来缓解在各个代码库中传递和管理各种环境引用而引发的痛苦。  

要访问模块，例如访问audio音频模块的代码简单如下：  

```java
// 使用audio音频模块创建一个16位PCM采样率的AudioDevice
AudioDevice audioDevice = Gdx.audio.newAudioDevice(44100, false);
```

`Gdx.audio`是一个底层（平台）实例的引用，它是在Application实例初始化应用程序的时候创建的。其它的模块也是通过类似的方法来访问，例如：`Gdx.app`可以获取Application实例，`Gdx.files`可以访问Files模块的实例等等。

[下一章|libGDX的生命周期](https://blog.csdn.net/qinxiandiqi/article/details/80874868)