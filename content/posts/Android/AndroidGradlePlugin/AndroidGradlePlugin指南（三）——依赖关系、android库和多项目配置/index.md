---
# type: posts 
title: "Android Gradle Plugin指南（三）——依赖关系、android库和多项目配置"
date: 2014-07-14T12:17:27+0800
authors: ["Jianan"]
summary: "原文地址：http://tools.android.com/tech-docs/new-build-system/user-guide#TOC-Dependencies-Android-Libraries-and-Multi-project-setup


4、Dependencies，Android Libraries and Multi-project setup（依赖关系，Androi"
series: ["Android Gradle"]
categories: ["翻译", "Android Gradle"]
tags: ["Android Gradle", "依赖关系", "库项目", "多项目配置"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 11277
comment_num: 3
---

原文地址：<http://tools.android.com/tech-docs/new-build-system/user-guide#TOC-
Dependencies-Android-Libraries-and-Multi-project-setup>

  

# 4、Dependencies，Android Libraries and Multi-project
setup（依赖关系，Android库和多项目设置）

  
Gradle项目可以依赖于其它组件。这些组件可以是外部二进制包，或者是其它的Gradle项目。

  

## 4.1 Dependencies on binary packages（依赖二进制包）

  

### 4.1.1 Local packages（本地包）

  
配置一个外部库的jar包依赖，你需要在compile配置中添加一个依赖。

    
    
        dependencies {
            compile files('libs/foo.jar')
        }
    
        android {
            ...
        }

  

注意：这个dependencies DSL标签是标准Gradle API中的一部分，所以它不属于android标签。

  
这个compile配置将被用于编译main application。它里面的所有东西都被会被添加到编译的classpath中，同时也会被打包进最终的APK。

以下是添加依赖时可能用到的其它一些配置选项：

**     * compile：main application（主module）。**

**     * androidTestCompile：test application（测试module）。**

**     * debugCompile：debug Build Type（debug类型的编译）。**

**     * releaseCompile：release Build Type（发布类型的编译）。**

因为没有可能去构建一个没有关联任何Build
Type（构建类型）的APK，APK默认配置了两个或两个以上的编译配置：compile和<buildtype>Compile.

创建一个新的Build Type将会自动创建一个基于它名字的新配置。

  
这对于debug版本需要使用一个自定义库（为了反馈实例化的崩溃信息等）但发布版本不需要，或者它们依赖于同一个库的不同版本时会非常有用。

  

### 4.2.2 Remote artifacts（远程文件）

  
Gradle支持从Maven或者Ivy仓库中拉取文件。

  
首先必须将仓库添加到列表中，然后必须在依赖中声明Maven或者Ivy声明的文件。

    
    
        repositories {
            mavenCentral()
        }
    
    
        dependencies {
            compile 'com.google.guava:guava:11.0.2'
        }
    
        android {
            ...
        }

注意：mavenCentral()是指定仓库URL的简单方法。Gradle支持远程和本地仓库。

注意：Gradle会遵循依赖关系的传递性。这意味着如果一个依赖本身依赖于其它东西，这些东西也会一并被拉取回来。

  
更多关于设置依赖关系的信息，请参考[Gradle用户指南](http://gradle.org/docs/current/userguide/artifact_dependencies_tutorial.html)和[DSL文档](http://gradle.org/docs/current/dsl/org.gradle.api.artifacts.dsl.DependencyHandler.html)。

  

## 4.2 Multi project setup（多项目设置）

  
Gradle项目也可以通过使用多项目配置依赖于其它Gradle项目。

  
多项目配置的实现通常是在一个根项目路径下将所有项目作为子文件夹包含进去。

  
  
例如，给定以下项目结构：

    
    
        MyProject/
         + app/
         + libraries/
            + lib1/
            + lib2/

  
我们可以定义3个项目。Grand将会按照以下名字映射它们：

**     :app**

**     :libraries:lib1**

**     :libraries:lib2**

  
每一个项目都拥有自己的build.gradle文件来声明自己如何构建。

另外，在根目录下还有一个setting.gradle文件用于声明所有项目。

这些文件的结构如下：

    
    
        MyProject/
         | settings.gradle
         + app/
            | build.gradle
         + libraries/
            + lib1/
               | build.gradle
            + lib2/
               | build.gradle

其中setting.gradle的内容非常简单：

    
    
        include ':app', ':libraries:lib1', ':libraries:lib2'

这里定义了哪一个文件夹才是真正的Gradle项目。

  
其中:app项目可能依赖于这些库，这是通过以下依赖配置声明的：

    
    
        dependencies {
            compile project(':libraries:lib1')
        }

  
更多关于多项目配置的信息请参考[这里](http://gradle.org/docs/current/userguide/multi_project_builds.html)。

  

## 4.3 Library projects（库项目）

  
在上面的多项目配置中，:libraries:lib1和:libraries:lib2可能是一个Java项目，并且:app这个Android项目将会使用它们的jar包输出。

  
但是，如果你想要共享代码来访问Android API或者使用Android样式的资源，那么这些库就不能是通常的Java项目，而应该是Android库项目。

  

### 4.3.1 Creating a Library Project（创建一个库项目）

  
一个库项目与通常的Android项目非常类似，只是有一点小区别。

  
尽管构建库项目不同于构建应用程序，它们使用了不同的plugin。但是在内部这些plugin共享了大部分相同的代码，并且它们都由相同的com.android.tools.build.gradle.jar提供。

    
    
        buildscript {
    
            repositories {
    
                mavenCentral()
    
            }
    
    
            dependencies {
    
                classpath 'com.android.tools.build:gradle:0.5.6'
    
            }
    
        }
    
    
        apply plugin: 'android-library'
    
    
        android {
    
            compileSdkVersion 15
    
        }

这里创建了一个使用API 15编译SourceSet的库项目，并且依赖关系的配置方法与应用程序项目的配置方法一样，同样也支持自定义配置。

  

### 4.3.2 Differences between a Project and a Library Project（普通项目和库项目之间的区别）

  
一个库项目的main输出是一个.aar包（它代表Android的归档文件）。它组合了编译代码（例如jar包或者是本地的.so文件）和资源（manifest，res，assets）。

一个库项目同样也可以独立于应用程序生成一个测试用的apk来测试。

  
标识Task同样适用于库项目（assembleDebug，assembleRelease），因此在命令行上与构建一个项目没有什么不同。

  
其余的部分，库项目与应用程序项目一样。它们都拥有build type和product flavor，也可以生成多个aar版本。

记住大部分Build Type的配置不适用于库项目。但是你可以根据库项目是否被其它项目使用或者是否用来测试来使用自定义的sourceSet改变库项目的内容。

  

### 4.3.3 Referencing a Library（引用一个库项目）

  
引用一个库项目的方法与引用其它项目的方法一样：

    
    
        dependencies {
    
            compile project(':libraries:lib1')
    
            compile project(':libraries:lib2')
    
        }

  

注意：如果你要引用多个库，那么排序将非常重要。这类似于旧构建系统里面的project.properties文件中的依赖排序。

  

### 4.3.4 Library Publication（库项目发布）

  
一般情况下一个库只会发布它的release
[Variant](http://blog.csdn.net/qinxiandiqi/article/details/37906449)（变种）版本。这个版本将会被所有引用它的项目使用，而不管它们本身自己构建了什么版本。这是由于Gradle的限制，我们正在努力消除这个问题，所以这只是临时的限制。

  
你可以控制哪一个Variant版本作为发行版：

    
    
        android {
    
            defaultPublishConfig "debug"
    
        }

注意这里的发布配置名称引用的是完整的Variant版本名称.Relesae，debug只适用于项目中没有其它特性版本的时候使用。如果你想要使用其它Variant版本取代默认的发布版本，你可以：

    
    
        android {
    
            defaultPublishConfig "flavor1Debug"
    
        }

  
将库项目的所有Variant版本都发布也是可能的。我们计划在一般的项目依赖项目（类似于上述所说的）情况下允许这种做法，但是由于Gradle的限制（我们也在努力修复这个问题）现在还不太可能。

默认情况下没有启用发布所有Variant版本。可以通过以下启用：

    
    
        android {
    
            publishNonDefault true
    
        }

  
理解发布多个Variant版本意味着发布多个arr文件而不是一个arr文件包含所有Variant版本是非常重要的。每一个arr包都包含一个单一的Variant版本。

发布一个变种版本意味着构建一个可用的arr文件作为Gradle项目的输出文件。无论是发布到一个maven仓库，还是其它项目需要创建一个这个库项目的依赖都可以使用到这个文件。

  
Gradle有一个默认文件的概念。当添加以下配置后就会被使用到：

    
    
        compile project(':libraries:lib2')

  
创建一个其它发布文件的依赖，你需要指定具体使用哪一个：

    
    
        dependencies {
    
            flavor1Compile project(path: ':lib1', configuration: 'flavor1Release')
    
            flavor2Compile project(path: ':lib1', configuration: 'flavor2Release')
    
        }

  
重要：注意已发布的配置是一个完整的Variant版本，其中包括了build type，并且需要像以上一样被引用。

重要：当启用非默认发布，maven发布插件将会发布其它Variant版本作为扩展包（按分类器分类）。这意味着不能真正的兼容发布到maven仓库。你应该另外发布一个单一的Variant版本到仓库中，或者允许发布所有配置以支持跨项目依赖。

