---
# type: posts 
title: "libGDX截取超出屏幕大小的图片"
date: 2017-11-18T23:03:37+0800
authors: ["Jianan"]
summary: "ibGDX截屏的过程，实际上就是读取帧缓冲区的一帧像素数据后封装成图片数据，再输出到图片文件，截屏出来的图片能有大小受限于帧缓冲区的大小。"
series: ["libGDX"]
categories: ["libGDX"]
tags: ["libgdx", "截屏", "尺寸", "帧缓冲区"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 916
comment_num: 0
---

> 原文作者：Jianan - qinxiandiqi@foxmail.com  
原文地址：[http://blog.csdn.net/qinxiandiqi](http://blog.csdn.net/qinxiandiqi)  
版本信息：2017-11-18  
版权声明：本文采用[CC BY-NC-ND 4.0](http://creativecommons.org/licenses/by-nc-nd/4.0/)共享协议。允许复制和转载，但必须在文首显眼位置保留原文作者、原文链接、版本信息、版权声明等信息。不允许演绎和用于商业用途。

libGDX本身已经提供了截屏功能，先看下libGDX的[wiki](https://github.com/libgdx/libgdx/wiki/Taking-a-Screenshot)提供的方法：
```java
byte[] pixels = ScreenUtils.getFrameBufferPixels(0, 0, Gdx.graphics.getBackBufferWidth(), Gdx.graphics.getBackBufferHeight(), true);

Pixmap pixmap = new Pixmap(Gdx.graphics.getBackBufferWidth(), Gdx.graphics.getBackBufferHeight(), Pixmap.Format.RGBA8888);
BufferUtils.copy(pixels, 0, pixmap.getPixels(), pixels.length);
PixmapIO.writePNG(Gdx.files.external("mypixmap.png"), pixmap);
pixmap.dispose();
```
简单解释下上面这段代码，整个截屏的过程分两步走：  

1.  获取将要截屏出来的图片像素数据。libGDX提供了ScreenUtils这个工具类，使用这个工具类的getFrameBufferPixels()方法，再设定相关参数获取当前帧缓存中指定位置的像素数据（byte字节数组）。
2.  将获取的图片像素数据输出到Pixmap对象（Pixmap是libGDX提供的用于处理图片的类），再由pixmap将图片像素处理为最终的图片数据并输出到文件，完成截屏图片输出。

总结一下，**libGDX截屏的过程，实际上就是读取帧缓冲区的一帧像素数据后封装成图片数据，再输出到图片文件。**

那么问题来了，截屏出来的图片最大能有多大，绘制在屏幕外边的内容是否也能截图出来？
可以换一种方式来思考一下。既然截屏的过程实际上就是保存帧缓冲区中的一帧像素数据，截屏出来的图片能有多大自然受限于帧缓冲区的大小。那么libGDX的帧缓冲区到底有多大呢？

libGDX游戏引擎的优点就是开源，所有源码都可以查看。libGDX在android平台上底层使用的是OpenGL，初始化OpenGL的代码被放在AndroidGraphics类中。这个类实现了android.opengl.GLSurfaceView.Renderer接口，在onSurfaceCreated()方法中有这么一段设置OpenGL viewport的代码：
```java
	@Override
	public void onSurfaceCreated (javax.microedition.khronos.opengles.GL10 gl, EGLConfig config) {
		...
		Display display = app.getWindowManager().getDefaultDisplay();
		this.width = display.getWidth();
		this.height = display.getHeight();
		...
		gl.glViewport(0, 0, this.width, this.height);
	}
```
可以看到OpenGL viewport的宽高使用了当前设备屏幕的宽高，也就是说，libGDX默认的帧缓冲区大小就是设备屏幕的尺寸，截屏最大也就是截取屏幕上显示的内容，超出帧缓冲区的内容由于没有记录像素数据，保存下来就变成黑色区域。

在某些需求下，期望可以将逻辑上绘制在屏幕范围之外的内容也能够截屏保存下来，怎么办呢？改变帧缓冲区的大小，调整帧缓冲区的大小超出屏幕，渲染绘制之后，屏幕范围之外的帧缓冲区也就有数据了。

libGDX提供了FrameBuffer这个工具可以创建一个帧缓冲区，在FrameBuffer.begin()和FrameBuffer.end()方法之间绘制的内容都会输入到FrameBuffer缓冲区中。截屏的流程调整如下：
```java
//创建一个帧缓冲区，指定帧缓冲区的像素格式类型、帧宽高等属性
FrameBuffer frameBuffer = new FrameBuffer(Pixmap.Format.RGBA8888, bufferWidth, bufferHeight, false);

//开始绘制，从这里开始绘制的数据都会输出到frameBuffer缓冲区，输入的才可以截屏出来
frameBuffer.begin();
...

//开始截屏，还是上文提到的截屏方法，不过这里帧缓冲区已经变成我们创建的FrameBuffer缓冲区
//captureWidth和captureHeight为最终截图的尺寸，不得大于设定的缓冲区尺寸大小
byte[] pixels = ScreenUtils.getFrameBufferPixels(0, 0, captureWidth, captureHeight, true);
Pixmap pixmap = new Pixmap(captureWidth, captureHeight, Pixmap.Format.RGBA8888);
BufferUtils.copy(pixels, 0, pixmap.getPixels(), pixels.length);
PixmapIO.writePNG(Gdx.files.external("mypixmap.png"), pixmap);
pixmap.dispose();

//截屏结束，关闭缓冲区
frameBuffer.end();	
//不再使用的frameBuffer需要销毁
frameBuffer.dispose();	
```
需要注意，不再使用FrameBuffer需要dispose销毁。

