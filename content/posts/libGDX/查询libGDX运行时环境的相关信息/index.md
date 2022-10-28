---
# type: posts 
title: "查询libGDX运行时环境的相关信息"
date: 2018-08-05T08:46:41+0800
authors: ["Jianan"]
summary: "Application接口提供了一些列查询libGDX运行时环境参数的方法。"
series: ["libGDX", "libGDX手册"]
categories: ["翻译", "libGDX", "libGDX手册"]
tags: ["libGDX", "查询", "运行时环境"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 278
comment_num: 0
---

> 原文作者：libGDX  
原文地址：[https://github.com/libgdx/libgdx/wiki/Querying](https://github.com/libgdx/libgdx/wiki/Querying)   
译文作者：Jianan - qinxiandiqi@foxmail.com  
版本信息：本文基于2018-08-05版本翻译  
译文版权：[CC BY-NC-ND 4.0](http://creativecommons.org/licenses/by-nc-nd/4.0/)，允许复制转载，但必须保留译文作者署名及译文链接。

<br>
`Application`接口提供了一些列查询libGDX运行时环境参数的方法。  
<br>
### 获取应用类型
---

有时候需要获取应用程序运行时所依赖的底层平台类型。`Application.getType()` 方法会返回应用程序实际运行时的底层平台类型：

```java
switch (Gdx.app.getType()) {
    case Android:
        // android specific code
        break;
    case Desktop:
        // desktop specific code
        break;
    case WebGl:
        // HTML5 specific code
        break;
    default:
        // Other platforms specific code
}
```

在Android平台上甚至可以查询应用程序运行时的Android系统版本号： 

```java
int androidVersion = Gdx.app.getVersion();
```

这个方法将会返回当前设备的Android SDK level，例如，Android 1.5会返回3。  

<br>
### 内存消耗情况
---

有时候为了调试或者分析应用程序，需要知道运行时的内存消耗情况。可以通过下面方法获取Java堆和native堆内存的消耗情况：  

```java
long javaHeap = Gdx.app.getJavaHeap();
long nativeHeap = Gdx.app.getNativeHeap();
```

这两个方法都会以bytes为单位返回当前对应堆内存的消耗情况。  

<br>
[上一章|libGDX的启动类和配置](https://blog.csdn.net/qinxiandiqi/article/details/81157151)
[下一章|libGDX的日志](https://blog.csdn.net/qinxiandiqi/article/details/81429696)