---
# type: posts 
title: "Android Studio Electric Eel无法启动无法编译？"
date: 2023-01-16T13:43:32+08:00
authors: ["Jianan"]
summary: "升级Android Studio Electric Eel| 2022.1.1版本后，无法启动无法编译了？"
series: ["Android"]
categories: ["Android"]
tags: ["Android", "Android Studio"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: true
read_num: 0
comment_num: 0 
---

Android Studio又发布正式版本了，这次是Android Studio Electric Eel | 2022.1.1，代言动物是电鳗……  

每次Google发布Android Studio，我都是又恨又爱，很是忐忑。虽然我每次都对新版本寄予厚望，期望Google老爹能优化下性能，解决下旧版难以忍受的bug。然而……但是……不出意外总会出意外，Google每次都要整出一些糟心事。这次是更新后点点菜单什么的直接卡死，不然就是项目无法编译，Gradle project sync永远都是failed，而且Build没有任何错误输出：   
![编译失败](%E7%BC%96%E8%AF%91%E5%A4%B1%E8%B4%A5.png)  
怎么办呢？查查Android Studio的日志文件，发现了这个错误日志：  
```java
org.gradle.tooling.GradleConnectionException: Could not run phased build action using connection to Gradle distribution 'https://services.gradle.org/distributions/gradle-7.2-bin.zip'.
	at org.gradle.tooling.internal.consumer.ExceptionTransformer.transform(ExceptionTransformer.java:55)
	at org.gradle.tooling.internal.consumer.ExceptionTransformer.transform(ExceptionTransformer.java:29)
	at org.gradle.tooling.internal.consumer.ResultHandlerAdapter.onFailure(ResultHandlerAdapter.java:43)
	at org.gradle.tooling.internal.consumer.async.DefaultAsyncConsumerActionExecutor$1$1.run(DefaultAsyncConsumerActionExecutor.java:69)
	at org.gradle.internal.concurrent.ExecutorPolicy$CatchAndRecordFailures.onExecute(ExecutorPolicy.java:64)
	at org.gradle.internal.concurrent.ManagedExecutorImpl$1.run(ManagedExecutorImpl.java:48)
	at java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1128)
	at java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:628)
	at java.base/java.lang.Thread.run(Thread.java:829)
Caused by: org.gradle.internal.jvm.JavaHomeException: The supplied javaHome seems to be invalid. I cannot find the java executable. Tried location: C:\Program Files\Android\Android Studio\jre\bin\java.exe
	at org.gradle.internal.jvm.Jvm.findExecutable(Jvm.java:183)
	at org.gradle.internal.jvm.Jvm.getJavaExecutable(Jvm.java:208)
	at org.gradle.internal.jvm.Jvm.forHome(Jvm.java:119)
	at org.gradle.launcher.daemon.context.DaemonCompatibilitySpec.javaHomeMatches(DaemonCompatibilitySpec.java:64)
	at org.gradle.launcher.daemon.context.DaemonCompatibilitySpec.whyUnsatisfied(DaemonCompatibilitySpec.java:40)
	at org.gradle.launcher.daemon.context.DaemonCompatibilitySpec.isSatisfiedBy(DaemonCompatibilitySpec.java:35)
	at org.gradle.launcher.daemon.context.DaemonCompatibilitySpec.isSatisfiedBy(DaemonCompatibilitySpec.java:25)
	at org.gradle.launcher.daemon.client.DefaultDaemonConnector.getCompatibleDaemons(DefaultDaemonConnector.java:192)
	at org.gradle.launcher.daemon.client.DefaultDaemonConnector.connectToIdleDaemon(DefaultDaemonConnector.java:157)
	at org.gradle.launcher.daemon.client.DefaultDaemonConnector.connect(DefaultDaemonConnector.java:125)
	at org.gradle.launcher.daemon.client.DaemonClient.execute(DaemonClient.java:144)
	at org.gradle.launcher.daemon.client.DaemonClient.execute(DaemonClient.java:98)
```
没有找到JavaHome，所以尝试找Android Studio安装目录下的jre：C:\Program Files\Android\Android Studio\jre\bin\java.exe。原来是运行时问题，我的机器上确实没有配置JAVA_HOME等环境变量，但Android Studio自带java运行时，也不至于找不到运行时呀！去Android Studio安装目录下看看：    
![空jre](%E7%A9%BAjre.png)  

Android Studio自带的jre居然是空的？去Android Studio的Gradle设置里面看看Gradle的运行时环境配置的是什么：  
![gradle运行时](gradle%E8%BF%90%E8%A1%8C%E6%97%B6.png)  

jbr？jbr是什么鬼？原来JBR是JetBrains基于OpenJDK开发的一个java运行时，它主要针对字体渲染、连字、HiDPI支持、窗口/焦点子系统、性能等地方做了改进，以及修复了一些OpenJDK的bug。现在JetBrains全家桶的默认Java运行时都已经迁移到JBR，这次的Android Studio Electric Eel版本估计是同步了上游的IDEA版本，所以也迁移到JBR运行时。看了下Android Studio的安装目录，确实也有jbr文件夹，里面是jbr运行时。然而，但是，为啥gradle编译的时候，默认还是去找jre目录呢？把Android Studio安装目录下的jre空文件夹删掉试试：  
![编译成功](%E7%BC%96%E8%AF%91%E6%88%90%E5%8A%9F.png)  

居然就可以了……  

猜测是Android Studio把内置运行时默认也改到JBR运行时了，但是某些功能，比如菜单呀、gradle编译呀，对Java运行时的查找优先级可能是：JavaHome环境变量 -> Android Studio内置jre路径 -> Android Studio内置jbr路径。Android Studio升级的时候把旧版的jre运行时删了，但是没删干净，留下了jre空目录。这就导致了部分功能还是优先找jre目录做java运行时，但jre已经不完整。所以，这到底是JetBrains的锅呢，还是Google的锅呢？？

## 总结 

Android Studio升级到Electric Eel版本后，如果出现Studio无法启动，点击菜单崩溃或者gradle无法编译的情况，可以看看Android Studio的日志文件（可以通过Studio的Help菜单的 Show Log in Explorer选项找到），看看里面是不是有找不到jre运行时的异常。如果是的话，就去Android Studio的安装目录，找找里面有没有jre文件夹，把整个文件夹删掉就可以了。