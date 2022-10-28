---
# type: posts 
title: "Android Gradle Plugin指南（五）——Build Variants（构建变种版本）"
date: 2014-07-17T14:55:48+0800
authors: ["Jianan"]
summary: "原文地址：http://tools.android.com/tech-docs/new-build-system/user-guide#TOC-Build-Variants


6、 Build Variants（构建变种版本）


新构建系统的一个目标就是允许为同一个应用创建不同的版本。

这里有两个主要的使用情景：
    1、同一个应用的不同版本。例如一个免费的版本和一个收"
series: ["Android Gradle"]
categories: ["翻译", "Android Gradle"]
tags: ["Android Gradle", "flavor", "variant", "多版本输出", "gradle"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 29295
comment_num: 4
---

原文地址：<http://tools.android.com/tech-docs/new-build-system/user-guide#TOC-
Build-Variants>

  

# 6、 Build Variants（构建变种版本）

  

新构建系统的一个目标就是允许为同一个应用创建不同的版本。

  
这里有两个主要的使用情景：

    1、同一个应用的不同版本。例如一个免费的版本和一个收费的专业版本。

    2、同一个应用需要打包成不同的apk以发布Google Play Store。查看<http://developer.android.com/google/play/publishing/multiple-apks.html>获取更多详细信息。

    3、综合1和2两种情景。

这个目标就是要让在同一个项目里生成不同的APK成为可能，以取代以前需要使用一个库项目和两个及两个以上的应用项目分别生成不同APK的做法。

  

## 6.1 Product flavors（不同定制的产品）

  

一个product flavor定义了从项目中构建了一个应用的自定义版本。一个单一的项目可以同时定义多个不同的flavor来改变应用的输出。

  
这个新的设计概念是为了解决不同的版本之间的差异非常小的情况。虽然最项目终生成了多个定制的版本，但是它们本质上都是同一个应用，那么这种做法可能是比使用库项目更好的实现方式。

  

Product flavor需要在productFlavors这个DSL容器中声明：

    
    
        android {
            ....
    
            productFlavors {
                flavor1 {
                    ...
                }
    
                flavor2 {
                    ...
                }
            }
        }

这里创建了两个flavor，名为flavor1和flavor2。

注意：flavor的命名不能与已存在的Build Type或者androidTest这个sourceSet有冲突。

  

## 6.2 Build Type  + Product Flavor = Build Variant（构建类型+定制产品=构建变种版本）

  

正如前面章节所提到的，每一个Build Type都会生成一个新的APK。

  
Product Flavor同样也会做这些事情：项目的输出将会拼接所有可能的Build Type和Product
Flavor（如果有Flavor定义存在的话）的组合。

  
每一种组合（包含Build Type和Product Flavor）就是一个Build Variant（构建变种版本）。

  
例如，在上面的Flavor声明例子中与默认的debug和release两个Build Type将会生成4个Build Variant：

    ***** Flavor1 - debug

    ***** Flavor1 - release

    ***** Flavor2 - debug

    ***** Flavor2 - release

  

项目中如果没有定义flavor同样也会有Build
Variant，只是使用的是默认的flavor和配置。默认的flavor是没有名字的，所以生成的Build Variant列表看起来就跟Build
Type列表一样。

  

## 6.3 Product Flavor Configuration（Product Flavor的配置）

  

每一个flavor都是通过闭包来配置的：

    
    
        android {
            ...
    
            defaultConfig {
                minSdkVersion 8
                versionCode 10
            }
    
            productFlavors {
                flavor1 {
                    packageName "com.example.flavor1"
                    versionCode 20
                }
    
                flavor2 {
                    packageName "com.example.flavor2"
                    minSdkVersion 14
                }
            }
        }

注意ProductFlavor类型的 **android.productFlavors.*** 对象与 **android.defaultConfig**
对象的类型是相同的。这意味着它们共享相同的属性。

  
defaultConfig为所有的flavor提供基本的配置，每一个flavor都可以重设这些配置的值。在上面的例子中，最终的配置结果将会是：

    *** flavor1**  
        * packageName: com.example.flavor1  
        * minSdkVersion: 8  
        * versionCode: 20  
    *** flavor2**  
        * packageName: com.example.flavor2  
        * minSdkVersion: 14  
        * versionCode: 10  
  

通常情况下，Build Type的配置会覆盖其它的配置。例如，Build Type的 **packageNameSuffix** 会被追加到Product
Flavor的packageName上面。

  
也有一些情况是一些设置可以同时在Build Type和Product Flavor中设置。在这种情况下，按照个别为主的原则决定。

例如，signingConfig就这种属性的一个例子。

signingConfig允许通过设置 **android.buildTypes.release.signingConfig**
来为所有的release包共享相同的SigningConfig。也可以通过设置
**android.productFlavors.*.signingConfig** 来为每一个release包指定它们自己的SigningConfig。

  

## 6.4 Sourcesets and Dependencies（源组件和依赖关系）

  

与Build Type类似，Product Flavor也会通过它们自己的sourceSet提供代码和资源。

  

上面的例子将会创建4个sourceSet：  

    * android.sourceSets.flavor1：位于src/flavor1/

    * android.sourceSets.flavor2：位于src/flavor2/

    * android.sourceSets.androidTestFlavor1：位于src/androidTestFlavor1/

    * android.sourceSets.androidTestFlavor2：位于src/androidTestFlavor2/

这些sourceSet用于与 **android.sourceSets.main** 和Build Type的sourceSet来构建APK。

  
**下面的规则用于处理所有使用的sourceSet来构建一个APK：**

**     * 多个文件夹中的所有的源代码（src/*/java）都会合并起来生成一个输出。**

**     * 所有的Manifest文件都会合并成一个Manifest文件。类似于Build Type，允许Product
Flavor可以拥有不同的的组件和权限声明。**

**     * 所有使用的资源（Android res和assets）遵循的优先级为Build Type会覆盖Product
Flavor，最终覆盖main sourceSet的资源。**

**     * 每一个Build Variant都会根据资源生成自己的R类（或者其它一些源代码）。Variant互相之间没有什么是共享的。**

  
最终，类似Build Type，Product
Flavor也可以有它们自己的依赖关系。例如，如果使用flavor来生成一个基于广告的应用版本和一个付费的应用版本，其中广告版本可能需要依赖于一个广告SDK，但是另一个不需要。

    
    
        dependencies {
            flavor1Compile "..."
        }

在这个例子中，src/flavor1/AndroidManifest.xml文件中可能需要声明访问网络的权限。

  

每一个Variant也会创建额外的sourceSet：

    * android.sourceSets.flavor1Debug：位于src/flavor1Debug/

    * android.sourceSets.flavor1Release：位于src/flavor1Release/

    * android.sourceSets.flavor2Debug：位于src/flavor2Debug/

    * android.sourceSets.flavor2Release：位于src/flavor2Release/

这些sourceSet拥有比Build Type的sourceSet更高的优先级，并允许在Variant的层次上做一些定制。

  

## 6.5 Building and Tasks（构建和任务）

  

我们前面提到每一个Build Type会创建自己的 **assemble <name>task**，但是Build Variant是Build
Type和Product Flavor的组合。

  
当使用Product Flavor的时候，将会创建更多的assemble-type task。分别是：

    **1、assemble <Variant Name>：**允许直接构建一个Variant版本，例如assembleFlavor1Debug。

    **2、assemble <Build Type Name>：**允许构建指定Build Type的所有APK，例如assembleDebug将会构建Flavor1Debug和Flavor2Debug两个Variant版本。

    **3、assemble <Product Flavor Name>：**允许构建指定flavor的所有APK，例如assembleFlavor1将会构建Flavor1Debug和Flavor1Release两个Variant版本。

另外 **assemble** task会构建所有可能组合的Variant版本。

  

## 6.6 Testing（测试）

  

测试multi-flavors项目非常类似于测试简单的项目。

  
**androidTest** sourceSet用于定义所有flavor共用的测试，但是每一个flavor也可以有它自己特有的测试。

  
正如前面提到的，每一个flavor都会创建自己的测试sourceSet：

    * android.sourceSets.androidTestFlavor1：位于src/androidTestFlavor1/

    * android.sourceSets.androidTestFlavor2：位于src/androidTestFlavor2/

  
同样的，它们也可以拥有自己的依赖关系：

    
    
        dependencies {
            androidTestFlavor1Compile "..."
        }

  

这些测试可以通过main的标志性 **deviceCheck task** 或者main的 **androidTest task**
（当flavor被使用的时候这个task相当于一个标志性task）来执行。

  
每一个flavor也拥有它们自己的task来这行这些测试： **androidTest <VariantName>**。例如：

    * androidTestFlavor1Debug

    * androidTestFlavor2Debug

  
同样的，每一个Variant版本也会创建对应的测试APK构建task和安装或卸载task：

    * assembleFlavor1Test

    * installFlavor1Debug

    * installFlavor1Test

    * uninstallFlavor1Debug

    * ...

  
最终的HTML报告支持根据flavor合并生成。

下面是测试结果和报告文件的路径，第一个是每一个flavor版本的结果，后面的是合并起来的结果：

    * build/androidTest-results/flavors/<FlavorName>

    * build/androidTest-results/all/

    * build/reports/androidTests/flavors<FlavorName>

    * build/reports/androidTests/all/

  

## 6.7 Multi-flavor variants

  

在一些情况下，一个应用可能需要基于多个标准来创建多个版本。例如，Google Play中的multi-
apk支持4个不同的过滤器。区分创建的不同APK的每一个过滤器要求能够使用多维的Product Flavor。

  
假如有个游戏需要一个免费版本和一个付费的版本，并且需要在multi-
apk支持中使用ABI过滤器（译注：ABI，应用二进制接口，优点是不需要改动应用的任何代码就能够将应用迁移到任何支持相同ABI的平台上）。这个游戏应用需要3个ABI和两个特定应用版本，因此就需要生成6个APK（没有因计算不同Build
Types生成的Variant版本）。

然而，注意到在这个例子中，为三个ABI构建的付费版本源代码都是相同，因此创建6个flavor来实现不是一个好办法。

相反的，使用两个flavor维度，并且自动构建所有可能的Variant组合。

  
这个功能的实现就是使用Flavor Groups。每一个Group代表一个维度，并且flavor都被分配到一个指定的Group中。

    
    
        android {
            ...
    
            flavorGroups "abi", "version"
    
            productFlavors {
                freeapp {
                    flavorGroup "version"
                    ...
                }
    
                x86 {
                    flavorGroup "abi"
                    ...
                }
    
                ...
            }
        }

andorid.flavorGroups数组按照先后排序定义了可能使用的group。每一个Product Flavor都被分配到一个group中。

  

上面的例子中将Product
Flavor分为两组（即两个维度），为别为abi维度[x86,arm,mips]和version维度[freeapp,paidapp]，再加上默认的Build
Type有[debug,release]，这将会组合生成以下的Build Variant：

    * x86-freeapp-debug

    * x86-freeapp-release

    * arm-freeapp-debug

    * arm-freeapp-release

    * mips-freeapp-debug

    * mips-freeapp-release

    * x86-paidapp-debug

    * x86-paidapp-release

    * arm-paidapp-debug

    * arm-paidapp-release

    * mips-paidapp-debug

    * mips-paidapp-release

android.flavorGroups中定义的group排序非常重要（Variant命名和优先级等）。

  
每一个Variant版本的配置由几个Product Flavor对象决定：

    * android.defaultConfig

    * 一个来自abi组中的对象

    * 一个来自version组中的对象

  
flavorGroups中的排序决定了哪一个flavor覆盖哪一个，这对于资源来说非常重要，因为一个flavor中的值会替换定义在低优先级的flavor中的值。

  
flavor groups使用最高的优先级定义，因此在上面例子中的优先级为：

    abi > version > defaultConfig

  
Multi-flavors项目同样拥有额外的sourceSet，类似于Variant的sourceSet，只是少了Build Type：

    * android.sourceSets.x86Freeapp：位于src/x86Freeapp/

    * android.sourceSets.armPaidapp：位于src/armPaidapp/

    * etc...

这允许在flavor-combination的层次上进行定制。它们拥有过比基础的flavor sourceSet更高的优先级，但是优先级低于Build
Type的sourceSet。

