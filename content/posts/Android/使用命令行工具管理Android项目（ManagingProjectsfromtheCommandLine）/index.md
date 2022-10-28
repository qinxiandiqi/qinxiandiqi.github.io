---
# type: posts 
title: "使用命令行工具管理Android项目（Managing Projects from the Command Line）"
date: 2014-06-11T18:34:30+0800
authors: ["Jianan"]
summary: "前言

Android Tool支持通过命令行创建三种类型的项目。一个Android项目包括了打包成.apk安装文件锁需要的一切文件和资源。
一个Android项目包括了打包成.apk安装文件所需要的一切文件和资源。你需要为你最终安装到设备上的应用创建一个Android项目"
series: ["Android"]
categories: ["翻译", "Android"]
tags: ["Android", "command", "library"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 3942
comment_num: 0
---

\----------------------------------------------------------------------------------------------------------------------------------------------------------

原文作者：Google

原文地址：<http://developer.android.com/tools/projects/projects-
cmdline.html#ReferencingLibraryProject>

原文版权：[Creative Commons 2.5 Attribution
License](http://creativecommons.org/licenses/by/2.5/)

译文作者：Jianan - qinxiandiqi@foxmail.com

版本信息：本文基于2014-06-11版本开始翻译，基于2015-10-23版本重新翻译

译文版权：[CC BY-NC-ND 4.0](http://creativecommons.org/licenses/by-nc-
nd/4.0/)，允许复制转载，但必须保留译文作者署名及译文链接，不得演绎和用于商业用途

\----------------------------------------------------------------------------------------------------------------------------------------------------------

  

  

# 前言

  

Android Tool支持通过命令行创建三种类型的项目。一个Android项目包括了打包成.apk安装文件所需要的一切文件和资源。

  * 一个Android项目包括了打包成.apk安装文件所需要的一切文件和资源。你需要为你最终安装到设备上的应用创建一个Android项目。  

  * 你可以将Android项目项目设计成一个库项目，以便能够给其它依赖于它的项目共享。一旦一个Android项目被设计成库项目，它将不能直接安装到设备上。  

  * 测试项目扩展了JUnit测试特性，并且提供Android项目测试功能。更多关于创建一个测试项目内容，可以参考[Testing from other IDEs](http://developer.android.com/tools/testing/testing_otheride.html)。

  

# 1\. Creating an Android Project（创建一个Android项目）

  

你需要使用Android
tool来创建Android项目。当你创建了一个Android项目，它将生成一个项目文件夹，里面包含了默认的程序文件，子文件，配置文件和构建文件等。

  
创建一个新的Android项目，你需要打开一个命令行终端，将路径导航到你的SDK目录中的tools/路径下，然后运行：

    
    
    android create project \
    --target <target_ID> \
    --name <your_project_name> \
    --path path/to/your/project \
    --activity <your_activity_name> \
    --package <your_package_namespace>

  * target指的是你项目的“build target”。它指明你想要重新构建项目所要使用的Android平台（包括一些扩展的平台，比如Google APIs）。获取可使用的target和它们对应的ID，可以运行android list targets命令查看。  

  * name指的是你的项目名称。这是个可选的选项，如果提供，当你编译你的项目时候，这个名字将作为打包出来的.apk文件名。  

  * path指定你的项目文件夹路径。如果指定路径上的文件夹不存在，它将会自动创建。  

  * activity指明你项目中默认的Activity类名。这个类将会被创建在<path_to_your_project>/src/<your_package_namespace_path>/下。如果你没有设置name属性，这个属性也会作为你的.apk文件名。  

  * package是你项目的包命名空间，遵循Java程序设计语言的包命名规范。

以下是示例：

    
    
    android create project \
    --target 1 \
    --name MyAndroidApp \
    --path ./MyAndroidAppProject \
    --activity MyAndroidAppActivity \
    --package com.example.myandroid

一旦你创建好了项目，你就可以开始进行开发。你可以将项目移动到任何你想要进行开发的方法，但是你还是需要使用Android SDK的platform-
tools/目录下的adb工具来将你的应用发送到模拟器中（稍后会讨论）。因此，你可能需要在你的项目和platform-tools两个文件夹之间互相切换。

  
 **小提示：** 你可以将platform-tools/和tools/路径添加到环境变量PATH中。

 **注意：**
你最好不要移动SDK的路径，因为这将会导致项目local.properties文件中SDK路径属性出错。如果你需要更新SDK路径，可以使用android
update project命令，详细参考下章节Updating a Project。

  

# 2\. Updating a Project（更新项目）

  

如果你想要为你的项目升级Android SDK版本，或者是想要从以后的代码中构建新项目，你可以使用android update
project命令来更新项目到新的开发环境中。你同样可以使用这个命令为已存在的项目更新build
targe（对应创建时的\--target选项）和项目名（对应创建时的\--name选项）。android
tool将会自动创建一些丢失的，或者需要更新，或者Android项目必须的文件（前面章节提到的文件）。

  
更新一个已存在的android项目，需要打开一个命令行终端，导航到SDK的tools/目录下，然后运行以下命令：

    
    
    android update project --name <project_name> --target <target_ID>
    --path <path_to_your_project>

  * target指的是你项目的“build target”。它指明你想要重新构建项目所要使用的Android平台（包括一些扩展的平台，比如Google APIs）。获取可使用的target和它们对应的ID，可以运行android list targets命令查看。  

  * path指的是你的项目路径。  

  * name指的是项目名称。这个选项是可选的，如果你不需要更新项目名称，则不需要提供这个参数。

下面是示例：  

    
    
    android update project --name MyApp --target 2 --path ./MyAppProject

  

# 3\. Setting up a Library Project（配置一个库项目）

  

一个库项目也是一个标准的Android项目，所以你可以使用创建一个新Android项目的方法来创建一个库项目。特别的，你可以使用android
tool来生成一个新的库项目以及所需要的文件和文件夹。

  
创建一个库项目，你需要导航到<sdk>/tools/路径下，然后使用以下命令：

    
    
    android create lib-project --name <your_project_name> \
    --target <target_ID> \
    --path path/to/your/project \
    --package <your_library_package_namespace>

create lib-
project命令将会创建一个标准的项目结构，其中包含了预设的属性值来向编译系统说明这个项目是个库项目。它通过在project.properties文件中添加以下属性行：

android.library = true

一旦命令执行完，库项目将会被创建。之后，你可以根据以下章节所说的添加源代码和资源进去。

如果你想要将一个已经存在的Android应用项目转换成库项目，好让其它应用能够使用。那么你同样可以在Android应用项目的project.properties文件中设置android.library=true属性来完成转换。

  

## 3.1 Creating the manifest file（创建manifest文件）

  

跟标准的Android项目一样，库项目的manifest文件必须声明所有它包含的共享出去的组件。更多信息可以参考[AndroidManifest.xml](http://developer.android.com/guide/topics/manifest/manifest-
intro.html)说明文档。

  
例如，TicTacToeLib示例库项目声明了GameActivity：

    
    
    <manifest>
      ...
      <application>
        ...
        <activity android:name="GameActivity" />
        ...
      </application>
    </manifest>

  

## 3.2 Updating a library project（更新库项目）

  

如果你想要更新库项目的构建参数（build target，location），可以使用以下命令：

    
    
    android update lib-project \
    --target <target_ID> \
    --path path/to/your/project

  

# 4\. Referencing a Library Project（引用一个库项目）

  

如果你开发的项目需要从一个库项目中引入共享的代码和资源，你可以很简单的在项目的构建参数中添加一个库项目的引用。

  
添加一个库项目的引用，你需要在命令行中导航到<sdk>/tools/路径下，然后使用下面命令：

    
    
    android update project \
    --target <target_ID> \
    --path path/to/your/project
    --library path/to/library_projectA

这个命令更新了应用项目的构建参数，添加了一个库项目的引用。特别的，它会添加一个android.library.reference.n属性到项目的project.properties文件中。例如：

    
    
    android.library.reference.1=path/to/library_projectA

如果你添加了多个库项目引用，你可以通过编辑project.properties文件，为每个引用的.n设置合适的值来设置它们的相对优先级（组合顺序），假设当前有一下引用：

    
    
    android.library.reference.1=path/to/library_projectA
    android.library.reference.2=path/to/library_projectB
    android.library.reference.3=path/to/library_projectC

你可以重新排序这些引用的优先级，给library_projectC最高优先级：

    
    
    android.library.reference.2=path/to/library_projectA
    android.library.reference.3=path/to/library_projectB
    android.library.reference.1=path/to/library_projectC

注意应用的.n序号必须从“1”开始，并且排序中间不能有“漏号”（即不能有类似1,2,4这样的排序情况）。排在“漏号”后面的引用会被忽略掉。

在编译期间，所有依赖库将会一次性按照从最低优先级到最高优先级的顺序组合进应用项目中。注意一个库项目本身不能依赖于其他库项目，因为，在编译期间库项目库项目组合进应用项目之前，它们之间不会不互相组合。

  

## 4.1 Declaring library components in the manifest file（在manifest文件中声明库项目的组件）

  

在应用想用的manifest文件中，你必须将所有从依赖库从导入进来使用到的组件进行声明。例如，你必须声明使用到的

<activity>,<service>,<receiver>,<provider>等等组件，也包括<permission>,<uses-
library>已经类似的标签。

声明使用到的依赖库组件必须使用它们的完整包名声明。

例如，TicTacToeMain示例应用项目声明库项目中的GameActivity：

    
    
    <manifest>
      ...
      <application>
        ...
        <activity android:name="com.example.android.tictactoe.library.GameActivity" />
        ...
      </application>
    </manifest>

更多关于manifest文件的信息，可以参考[AndroidManifest.xml](http://developer.android.com/guide/topics/manifest/manifest-
intro.html)说明文档。

  

## 4.2 Building a dependent application（编译一个依赖的应用）

编译一个依赖一个或多个库项目的应用项目，你可以使用[Building and
Running](http://developer.android.com/tools/building/index.html)文档中描述的标准Ant构建命令和编译模式来构建。工具将会编译并且组合所有依赖库作为编译项目的一部分。不需要额外的命令或者步骤。

  

