---
# type: posts 
title: "Android的进程优先级"
date: 2016-06-23T16:35:24+0800
authors: ["Jianan"]
summary: "android对于所有进程的处理态度都是尽可能不杀死。然而，资源总共就那么多，要是对所有进程都保持宽容的话，资源总会有消耗殆尽的时候。因此，在内存不足的情况，android系统需要根据一定的策略，选择性的杀死部分进程。这个策略就是对所有的进程标记优先级，优先级低的先杀死。  

android将进程的优先级分为5个层次。"
series: ["Android"]
categories: ["Android"]
tags: ["android", "进程", "优先级"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 6549
comment_num: 0
---

> 原文作者：Jianan - qinxiandiqi@foxmail.com  
原文地址：[http://blog.csdn.net/qinxiandiqi/article/details/51744782](http://blog.csdn.net/qinxiandiqi/article/details/51744782)  
版本信息：2016-06-23  
版权声明：本文采用[CC BY-NC-ND 4.0](http://creativecommons.org/licenses/by-nc-nd/4.0/)共享协议。允许复制和转载，但必须在文首显眼位置保留原文作者、原文链接、版本信息、版权声明等信息。不允许演绎和用于商业用途。

android对于所有进程的处理态度都是尽可能不杀死。然而，资源总共就那么多，要是对所有进程都保持宽容的话，资源总会有消耗殆尽的时候。因此，在内存不足的情况，android系统需要根据一定的策略，选择性的杀死部分进程。这个策略就是对所有的进程标记优先级，优先级低的先杀死。  

android将进程的优先级分为5个层次，按照优先级由高到低排列如下：

1. **前台进程（Foreground process）**。它表明用户正在与该进程进行交互操作，android系统依据下面的条件来将一个进程标记为前台进程：
    * 该进程持有一个用户正在与其交互的Activity（也就是这个activity的生命周期方法走到了onResume()方法）。
    * 该进程持有一个Service，并且这个Service与一个用户正在交互中的Activity进行绑定。
    * 该进程持有一个前台运行模式的Service（也就是这个Service调用了startForegroud()方法）。
    * 该进程持有一个正在执行生命周期方法（onCreate()、onStart()、onDestroy()等）的Service。
    * 该进程持有一个正在执行onReceive()方法的BroadcastReceiver。  

    一般情况下，不会有太多的前台进程。杀死前台进程是操作系统最后无可奈何的做法。当内存严重不足的时候，前台进程一样会被杀死。

2. **可见进程（Visible process）**。它表明虽然该进程没有持有任何前台组件，但是它还是能够影响到用户看得到的界面。android系统依据下面的条件将一个进程标记为可见进程：
    * 该进程持有一个非前台Activity，但这个Activity依然能被用户看到（也就是这个Activity调用了onPause()方法）。例如，当一个activity启动了一个对话框，这个activity就被对话框挡在后面。
    * 该进程持有一个与可见（或者前台）Activity绑定的Service。

3. **服务进程（Service process）**。除了符合前台进程和可见进程条件的Service，其它的Service都会被归类为服务进程。  

4. **后台进程（Background process）**。持有不可见Activity（调用了onStop()方法）的进程即为后台进程。通常情况下都会有很多后台进程，当内存不足的时候，在所有的后台进程里面，会按照LRU（最近使用）规则，优先回收最长时间没有使用过的进程。  

5. **空进程（Empty process）**。不持有任何活动组件的进程。保持这种进程只有一个目的，就是为了缓存，以便下一次启动该进程中的组件时能够更快响应。当资源紧张的时候，系统会平衡进程缓存和底层的内核缓存情况进行回收。  

如果一个进程同时满足上述5种优先级中的多个等级条件，android系统会优先选取其中最高的等级作为该进程的优先级。例如，一个进程持有一个Service（服务进程等级）和一个前台Activity（前台进程等级），那么操作系统会将这个进程标记为前台进程。

另外需要注意的是，如果一个进程为另外一个进程提供服务，那么这个进程的优先级不会低于享受服务的进程。例如，假设进程A中的content provider为进程B提供服务，或者进程A中有一个Service与进程B中的组件进程绑定，那么进程A的优先级至少要与进程B一致，或者高于进程B。

> 参考：[https://developer.android.com/guide/components/processes-and-threads.html](https://developer.android.com/guide/components/processes-and-threads.html)
