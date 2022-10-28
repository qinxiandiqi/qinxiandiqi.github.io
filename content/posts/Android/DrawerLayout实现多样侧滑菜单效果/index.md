---
# type: posts 
title: "DrawerLayout实现多样侧滑菜单效果"
date: 2017-06-22T12:35:49+0800
authors: ["Jianan"]
summary: "改变DrawerLayout的默认侧滑效果，比如实现常见的主内容区域跟随侧滑菜单滑动而滑动，甚至如QQ侧滑菜单等复杂效果。实现的关键在于利用**DrawerLayout.addDrawerListener(DrawerLayout.DrawerListener)**方法，给DrawerLayout添加DrawerListener监听。"
series: ["Android Tips"]
categories: ["Android Tips"]
tags: ["android", "抽屉式菜单", "滑动效果", "侧滑菜单"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 727
comment_num: 0
---

> 原文作者：Jianan - qinxiandiqi@foxmail.com  
原文地址：[http://blog.csdn.net/qinxiandiqi/article/details/73556668](http://blog.csdn.net/qinxiandiqi/article/details/73556668)  
版本信息：2017-06-21  
版权声明：本文采用[CC BY-NC-ND 4.0](http://creativecommons.org/licenses/by-nc-nd/4.0/)共享协议。允许复制和转载，但必须在文首显眼位置保留原文作者、原文链接、版本信息、版权声明等信息。不允许演绎和用于商业用途。

DrawerLayout抽屉菜单常用来做简单的左右侧滑菜单，默认效果是侧滑菜单覆盖在主内容区域上：

![默认效果](3c5e82b0d8801bede937b4a2d20be11c.gif)

可以改变DrawerLayout的默认侧滑效果，比如实现常见的主内容区域跟随侧滑菜单滑动而滑动，甚至如QQ侧滑菜单等复杂效果。实现的关键在于利用**DrawerLayout.addDrawerListener(DrawerLayout.DrawerListener)**方法，给DrawerLayout添加DrawerListener监听。

DrawerLayout.DrawerListener是监听DrawerLayout状态变化的一个内部类。实现DrawerListener.onDrawerSlide(View drawerView, float slideOffset)方法，第一个drawerView参数为作为侧滑菜单的View，第二个参数slideOffset为侧滑菜单滑动过程中移动的距离比例，范围从0到1。当侧滑菜单在滑动的过程中会回调这个方法，借助这个方法可以知道侧滑菜单当前的滑动距离从而调整其它View的属性，这样就可以实现多种多样的侧滑效果。

例如简单实现主内容区跟随侧滑菜单滑动而滑动的效果：

```java
drawerLayout.addDrawerListener(new DrawerLayout.DrawerListener() {
            @Override
            public void onDrawerSlide(View drawerView, float slideOffset) {
                vMain.setX(drawerView.getWidth() * slideOffset);
            }

            ...
        });
```

其中vMain为主内容区的View，看下效果：

![这里写图片描述](9b5de4f86a6ee9afa22404af56cf90a8.gif)