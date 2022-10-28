---
# type: posts 
title: "libGDX的日志"
date: 2018-08-05T11:55:14+0800
authors: ["Jianan"]
summary: "Application接口提供了简单但可以精确控制的日志打印功能。

日志消息可以输出为普通信息，或者是带可选异常的错误消息，也可以是调试信息："
series: ["libGDX", "libGDX手册"]
categories: ["翻译", "libGDX", "libGDX手册"]
tags: ["libGDX", "游戏引擎", "java", "日志"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 560
comment_num: 0
---

> 原文作者：libGDX  
原文地址：[https://github.com/libgdx/libgdx/wiki/Logging](https://github.com/libgdx/libgdx/wiki/Logging)   
译文作者：Jianan - qinxiandiqi@foxmail.com  
版本信息：本文基于2018-08-05版本翻译  
译文版权：[CC BY-NC-ND 4.0](http://creativecommons.org/licenses/by-nc-nd/4.0/)，允许复制转载，但必须保留译文作者署名及译文链接。

<br>
`Application`接口提供了简单但可以精确控制的日志打印功能。

日志消息可以输出为普通信息，或者是带可选异常的错误消息，也可以是调试信息：

```java
Gdx.app.log("MyTag", "my informative message");
Gdx.app.error("MyTag", "my error message", exception);
Gdx.app.debug("MyTag", "my debug message");
```

依赖于具体的底层平台，日志会被打印到控制台（桌面平台），或者是LogCat（Android平台）。在Html框架下，可以是`GwtApplicationConfiguration`提供的GWT`TextArea`，也可能是由html5自动控制。  

日志可以指定特定的输出级别：  

```java
Gdx.app.setLogLevel(logLevel);
```

这里的 `logLevel`可以是以下的值：  

  * Application.LOG_NONE: 屏蔽所有日志。
  * Application.LOG_DEBUG: 打印所有日志。
  * Application.LOG_ERROR: 只打印错误日志。
  * Application.LOG_INFO: 打印错误或普通日志。

<br>
[上一篇|查询libGDX运行时环境的相关信息](https://blog.csdn.net/qinxiandiqi/article/details/81427729)  
[下一篇|libGDX的线程](https://blog.csdn.net/qinxiandiqi/article/details/82080044)