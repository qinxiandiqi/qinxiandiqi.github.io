---
# type: posts 
title: "Android Gradle Plugin指南（一）——简介"
date: 2014-07-14T11:00:19+0800
authors: ["Jianan"]
summary: "原文地址：http://tools.android.com/tech-docs/new-build-system/user-guide#TOC-Introduction


译者：google推出了全新的Android Studio集成开发环境，其中Android项目的结构与Eclipse的Android项目结构有很大的区别，原因就在于两开发环境使用的构建工具不同。Android Studi"
series: ["Android Gradle"]
categories: ["翻译", "Android Gradle"]
tags: ["android", "gradle", "groovy", "插件", "文档"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 8459
comment_num: 4
---

原文地址：<http://tools.android.com/tech-docs/new-build-system/user-guide#TOC-
Introduction>

  

译者：google推出了全新的Android
Studio集成开发环境，其中Android项目的结构与Eclipse的Android项目结构有很大的区别，原因就在于两开发环境使用的构建工具不同。Android
Studio使用Gradle构建工具，Eclipse的ADT插件使用的是Ant构建工具。因为两个构建工具的区别，导致习惯了Eclipse开发环境的开发者刚开始比较难适应Android
Studio。如果要迁移到Android
Studio，建议最好了解下Gradle构建工具。Gradle构建工具是任务驱动型的构建工具，并且可以通过各种Plugin插件扩展功能以适应各种构建任务。对应Android项目的Gradle插件就是Android
Gradle Plugin。本文是Google官方的Android Gradle
Plugin使用指南翻译，以方便我大天朝开发者学习。如英语水平还不错的同学，建议直接查看官方原文，本人的理解和翻译难免有所疏漏。

  

# 1、Introduction（简介）

  
本文档适用于0.9版本的Gradle plugin。由于我们在1.0版本之前介绍的不兼容，所以早期版本可能与本文档有所不同。  
  

## 1.1 Goals of the new Build System（gradle构建系统的目标）

  
采用Gradle作为新构建系统的目标：  
     ***** 让重用代码和资源变得更加容易。  
   **  * **让创建同一应用程序的不同版本变得更加容易，无论是多个apk发布版本还是同一个应用的不同定制版本。  
   **  *** 让构建过程变得更加容易配置，扩展和定制。  
     ***** 整合优秀的IDE  
  

## 1.2 Why Gradle？（为什么使用gradle）

  
Gradle是一个优秀的构建系统和构建工具，它允许通过插件创建自定义的构建逻辑。  
我们基于Gradle以下的一些特点而选择了它：  
   **  *** 采用了Domain Specific Language(DSL语言)来描述和控制构建逻辑。  
     ***** 构建文件基于Groovy，并且允许通过混合声明DSL元素和使用代码来控制DSL元素以控制自定义的构建逻辑。  
     ***** 支持Maven或者Ivy的依赖管理。  
   **  *** 非常灵活。允许使用最好的实现，但是不会强制实现的方式。  
   **  * **插件可以提供自己的DSL和API以供构建文件使用。  
   **  * **良好的API工具供IDE集成。  
  

# 2、Requirements（要求）

  
     ***** Gradle 1.10 或者 Gradle 1.11 或者 Gradle 1.12，并使用0.11.1插件版本。  
   **  * **SDK build tools 要求版本19.0.0。一些新的特征可能需要更高版本。  

  
[](http://tools.android.com/tech-docs/new-build-system/user-guide#TOC-
Introduction)

