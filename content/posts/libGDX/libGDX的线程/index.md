---
# type: posts 
title: "libGDX的线程"
date: 2018-08-26T13:37:49+0800
authors: ["Jianan"]
summary: "所有ApplicationListener接口中的方法都会在同个OpenGL的渲染线程中被调用。对于大多数游戏来说，一般在ApplicationListener.render()方法中实现游戏的更新逻辑，这个方法会在渲染线程中执行。

任何涉及OpenGL的图形操作都需要在渲染线程上执行。如果在其它线程上执行会导致一些无法预测的结果，这是因为OpenGL Context只有在渲染线程中才处于激活状态，对于大多数Android设备而言，在其它线程中创建OpenGL Context会导致一些问题，因此不支持这"
series: ["libGDX", "libGDX手册"]
categories: ["翻译", "libGDX", "libGDX手册"]
tags: ["libGDX", "java", "游戏引擎", "线程", "线程安全"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 671
comment_num: 0
---

> 原文作者：libGDX  
原文地址：[https://github.com/libgdx/libgdx/wiki/Threading](https://github.com/libgdx/libgdx/wiki/Threading)   
译文作者：Jianan - qinxiandiqi@foxmail.com  
版本信息：本文基于2018-08-26版本翻译  
译文版权：[CC BY-NC-ND 4.0](http://creativecommons.org/licenses/by-nc-nd/4.0/)，允许复制转载，但必须保留译文作者署名及译文链接。

<br>

所有`ApplicationListener`接口中的方法都会在同个OpenGL的渲染线程中被调用。对于大多数游戏来说，一般在`ApplicationListener.render()`方法中实现游戏的更新逻辑，这个方法会在渲染线程中执行。

任何涉及OpenGL的图形操作都需要在渲染线程上执行。如果在其它线程上执行会导致一些无法预测的结果，这是因为OpenGL Context只有在渲染线程中才处于激活状态，对于大多数Android设备而言，在其它线程中创建OpenGL Context会导致一些问题，因此不支持这种操作。 

要从其它线程传递数据到渲染线程，我们建议使用[Application.postRunnable()](https://github.com/libgdx/libgdx/tree/master/gdx/src/com/badlogic/gdx/Application.java#173)。渲染线程在渲染下一帧，并且在`ApplicationListener.render()`方法回调之前会执行Runnable中的代码。  

```java
new Thread(new Runnable() {
   @Override
   public void run() {
      // do something important here, asynchronously to the rendering thread
      final Result result = createResult();
      // post a Runnable to the rendering thread that processes the result
      Gdx.app.postRunnable(new Runnable() {
         @Override
         public void run() {
            // process the result, e.g. add it to an Array<Result> field of the ApplicationListener.
            results.add(result);
         }
      });
   }
}).start();
```

<br>

## libGDX有哪些类是线程安全的？
---

除非在类文档中明确标记为线程安全，否则libgdx中的类都不是线程安全的。

假如要操作图形或音频相关的内容，你永远不应该对这些操作进行多线程处理，例如，在多个线程中处理scene2D组件。

<br>
## HTML5
---
JavaScript本质上是单线程的，所以不可能进行多线程操作。或许[Web Workers](http://www.whatwg.org/specs/web-apps/current-work/multipage/workers.html)会是将来的实现方式，不过，这种方式的数据是通过线程之间的消息传递来传递的。而Java的线程使用的是不同的原理和机制，并不能直接将Java的线程代码移植到Web Workers。

<br>
[上一篇|libGDX的日志](https://blog.csdn.net/qinxiandiqi/article/details/81429696)  
[下一篇|与特定平台相关的代码](https://github.com/libgdx/libgdx/wiki/Interfacing-with-platform-specific-code)