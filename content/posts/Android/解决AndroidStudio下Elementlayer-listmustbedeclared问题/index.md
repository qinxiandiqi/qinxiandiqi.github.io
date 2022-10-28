---
# type: posts 
title: "解决Android Studio下Element layer-list must be declared问题"
date: 2015-07-08T22:44:29+0800
authors: ["Jianan"]
summary: "最近将一个项目从Eclipse转到Android Studio。项目中使用了环信demo中的一些xml资源，转换后发现color资源文件夹下诸如layer-list或者shape等标签报Element xxx must be declared错误，大意就是layer-list或者shape这些标签没有定义。
layer-list或者shape等这些标签是常用的标签，Android Studio居"
series: ["Android"]
categories: ["Android"]
tags: ["android studio", "layer-list", "shape", "Element", "Declared"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 12070
comment_num: 5
---

  

最近将一个项目从Eclipse转到Android Studio。项目中使用了环信demo中的一些xml资源，转换后发现color资源文件夹下诸如layer-
list或者shape等标签报Element xxx must be declared错误，大意就是layer-list或者shape这些标签没有定义。

  

layer-list或者shape等这些标签是常用的标签，Android
Studio居然报没有定义错误，在Eclipse中却没有这个问题。网上不少人说这是Android Studio的一个bug，事实正相反，这是Android
Studio的优点。

  

对于这个问题，首先要了解layer-list、shape等这些标签是什么东西。每一种标签都有对应的资源类，layer-
list、shape等等标签代表的其实是个drawable资源。layer-
list最终会解析为LayerDrawable，shape会解析为ShapeDrawable，其它的标签类似。由此可以看出layer-
list或者shape等资源是drawable资源，应该放到drawable资源文件夹下。color资源不包括drawable资源，当然没有定义drawable类型的标签。

  

Eclipse不像Android Studio，对资源类型的检查没有那么严格，所以没有报错误。我觉得这倒是Android
Studio的优点，是什么资源就应该放到什么位置，不容易让人产生疑惑。所以在Android
Studio下的解决方法就是把这些资源文件移动到drawable资源文件夹下，这个问题解决。

