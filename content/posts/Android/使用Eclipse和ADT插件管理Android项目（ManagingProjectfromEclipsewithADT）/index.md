---
# type: posts 
title: "使用Eclipse和ADT插件管理Android项目（Managing Project from Eclipse with ADT）"
date: 2014-06-11T10:31:36+0800
authors: ["Jianan"]
summary: "简介

使用Eclipse和ADT插件可以提供可视化界面和向导来创建三种类型的项目（Android项目，库项目，以及测试项目）：
*一个Android项目包含了将项目打包成.apk安装文件所需要的所有文件和资源。你需要为你最终需要安装到设备上的应用创建一个Android项目。
*你也可以设计一个Android项目作为库项目，它可以共享给其它需要依赖于它的项目。一旦一个Android项目被设"
series: ["Android"]
categories: ["翻译", "Android"]
tags: ["Android", "ADT", "eclipse", "library"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 2119
comment_num: 0
---

\------------------------------------------------------------------------------------------------------------------------------------------------------------

原文作者：Google

原文地址：<http://developer.android.com/tools/projects/projects-
eclipse.html#ReferencingLibraryProject>

原文版权：[Creative Commons 3.0 Attribution
License](http://creativecommons.org/licenses/by/3.0/)

译文作者：Jianan - qinxiandiqi@foxmail.com

版本信息：本文基于2014-06-11版本开始翻译，基于2015-10-18版本重新翻译

译文版权：[CC BY-NC-ND 4.0](http://creativecommons.org/licenses/by-nc-
nd/4.0/)，允许复制转载，但必须保留译文作者署名及译文链接，不得演绎和用于商业用途

\------------------------------------------------------------------------------------------------------------------------------------------------------------

  

# 简介

  

使用Eclipse和ADT插件可以提供可视化界面和向导来创建三种类型的项目（Android项目，库项目，以及测试项目）：

  * 一个Android项目包含了将项目打包成.apk安装文件所需要的全部文件和资源。你需要为你最终要安装到设备上的应用创建一个Android项目。  

  * 你也可以设计一个Android项目作为库项目，它给其它需要依赖于它的项目共享。一旦一个Android项目被设计成库项目，它将不能被直接安装到设备上。  

  * 测试项目继承了JUnit的测试功能并提供了Android测试项目特有的功能。更多创建一个测试项目信息，可以参考 [Testing from Eclipse with ADT](http://developer.android.com/tools/testing/testing_eclipse.html)。 

  

# 1\. Creating an Android Project（创建一个Android项目）

  

ADT插件提供了一个创建新项目的向导，以便你能够快速创建一个新的Android项目（或者从已有代码上创建一个项目）。创建一个新项目的步骤：

    1.1 选择 File > New > Project.

    1.2 选择 Android > Android Application Project，然后点击Next。

    1.3 为新项目填写基本设置：

        1.3.1 输入Application Name（应用名）。这个名称将会在应用安装到设备上之后作为应用图标的标题。

        1.3.2 输入Project Name（项目名）。当你的项目创建的时候，这个名字将作为你项目的文件夹名。

        1.3.3 输入Package Name（包名）。这个类包命名空间将为你的应用源代码文件创建初始化包结构，并且也会添加你应用的Android manifest文件中座位package的属性值。当你发布你的应用程序时，这个manifest值将会提供作为一个唯一识别码以标识你的应用。这个包名必须遵循Java程序设计语言的包名命名规范。

        1.3.4 选择一个Minimum Request SDK（最低要求的SDK版本）。这个设置说明了你的应用程序支持的最低Android系统版本。你的manifest文件中，它将座位<uses-sdk>标签的minSdkVersion属性值。

        1.3.5 选择一个Target SDK（目标SDK）。这个设置说明了你的应用需要测试的最高Android系统版本。同时，manifest文件中的targetSdkVersion属性值将会被设置为这个值。

        注意：你可以随时更改你项目的target SDK：在Package Explorer视图中右键点击你的项目打开菜单，选择Properties，选择Android，然后修改期望的Project Build Target.

        1.3.6 选择一个Compile With API version（编译版本）。这个设置指定了用于编译你的项目的目标SDK版本。我们强烈建议使用最新的API版本。

        1.3.7 选择Theme（主题）。这个设置指定了应用到你项目上的视觉样式。

        1.3.8 点击Next

    1.4 在Configure Project（配置项目）页面， 选择期望的设置并且点击Next下一步。不要修改Create activity选项，以便你能够从一些主要部件启动你的应用。

    1.5 在Configure Launcher Icon（配置启动图标）页面，创建一个图标并点击Next下一步。

    1.6 在Create Activity（创建Activity）页面，选择activity模板并选择下一步。更多关于Android模板的信息，可以参考Using code Templates.

    1.7 点击Finish，向导将创建一个根据你刚才配置的新项目。

    提示：你同样可以从工具栏的New![eclipse-new.png](125ebb08dd19a8c28ec01cc00fdc3815.png)按钮启动创建新项目向导。

  

# 2.Setting up a Library Project（配置一个库项目）

  

库项目也是一个标准的android项目，所以你可以像创建一个新android项目一样创建一个库项目。

创建库项目的步骤：

    2.1 选择File > New > Project。

    2.2 选择Android > Android Application Project，然后点击Next下一步。

    2.3 为项目设置基础配置，包括Application Name，Project Name，Package Name和SDK设置。

    2.4 在Configure Project页面，选择Mark this project as a library选项以标记项目为库项目。

    2.5 设置其他你期望的选项并点击Next下一步。

    2.6 根据提示完成创建想到，并创建新的库项目。

  
你同样也可以配置一个已经存在的应用项目作为库项目。你只需要打开项目的Properties配置菜单，并且勾选is Library选项。如下图1：

![adt-props-isLib.png](https://img-
blog.csdn.net/20140612085650484?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcWlueGlhbmRpcWk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)  

  
具体步骤如下：

    1\. 在Package Explorer视图中，打开项目右键菜单，选择Properties。

    2\. 在Properties窗口中，左边侧边栏中选择Android属性组进入右边栏中Library属性。

    3\. 勾选is Library，并点击Apply。

    4\. 点击OK关闭Properties窗口。

一旦你创建了一个库项目或者将一个已存在的项目配置为库项目，那么你就可以在其他android项目中引用它。更新信息可以参考下面Referencing a
library project章节。

  

## Creating the manifest file

  

跟标准的android项目一样，库项目同样需要在项目的manifest文件中声明所有使用到的组件。更多信息可以参考[AndroidManifest.xml](http://developer.android.com/guide/topics/manifest/manifest-
intro.html)文档。例如，TicTacToeLib示例库项目中声明GameActivity：  

    
    
    <manifest>
      ...
      <application>
        ...
        <activity android:name="GameActivity" />
        ...
      </application>
    </manifest>

  

# 3.Referencing a Library Project（链接一个库项目）

  

如果你开发的项目需要包含外部库项目共享的代码和资源，你可以很方便的在项目的Properties中添加一个依赖库。

添加一个依赖库需要按照以下步骤：

    3.1 首先确定你的应用程序项目和库项目都在你的工作空间中。如果其中一个不在你的工作空间中，先将其导入。

    3.2 在Package Exploer视图中，右键打开需要依赖的项目菜单，选择Properties。

    3.3 在Properties窗口中，左边栏中选择Android属性组，右边栏将像是Library属性。

    3.4 点击Add按钮打开Project Selection对话框。

    3.5 在可选择的库项目列表中，选择需要的库项目后点击OK。

    3.6 对话框关闭后，点击Properties窗口中的Apply按钮。

    3.7 点击OK按钮关闭Properties窗口。

  
一旦Properties窗口关闭之后，Eclipse将会重新构建项目以包含依赖的库项目进来。

  
图2显示了Properties窗口，你可以添加库项目依赖并且上下调整它们的优先级。

![adt-props-libRef.png](https://img-
blog.csdn.net/20140612085713468?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcWlueGlhbmRpcWk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)  
  

如果你添加了多个依赖库，你可以通过选择依赖库并且通过Up和Down按钮来调整它们的优先级。这个工具将从低优先级（列表底部的库项目）到高优先级（列表顶部的库项目）顺序混合这些依赖库到你的项目中。如果多个库项目定义了同一个资源ID，那么工具将会选择优先级最高的项目中的资源。另外，项目自己本身拥有最高优先级，它的资源永远都会优先于其它依赖库被使用。

  

## Declaring library components in the manifest file（manifest文件中配置依赖库的的组件）

  

如果你使用了依赖库中的组件，你必须在你应用程序项目的manifest文件中声明这些使用到的组件（没有使用的可以不声明）。例如，你必须声明<activity>,<service>,<receiver>,<provider>等等，也包括<permission>,<uses-
library>等等类似的标签。

声明依赖库中的组件，必须包括它们完整的包名。

例如，TicTacToeMain示例项目组中声明依赖库中的GameActivity：

    
    
    <manifest>
      ...
      <application>
        ...
        <activity android:name="com.example.android.tictactoe.library.GameActivity" />
        ...
      </application>
    </manifest>

  

更多关于manifest文件的信息，可以参考[AndroidManifest.xml](http://developer.android.com/guide/topics/manifest/manifest-
intro.html)文档。

  

  

