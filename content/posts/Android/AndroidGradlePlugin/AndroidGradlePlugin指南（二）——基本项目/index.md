---
# type: posts 
title: "Android Gradle Plugin指南（二）——基本项目"
date: 2014-07-14T12:00:56+0800
authors: ["Jianan"]
summary: "原文地址：http://tools.android.com/tech-docs/new-build-system/user-guide#TOC-Basic-Project


3、Basic Project（基本项目）

一个Gradle项目的构建过程定义在build.gradle文件中，位于项目的根目录下。


3.1 Simple build files（简单的构建文件）
"
series: ["Android Gradle"]
categories: ["翻译", "Android Gradle"]
tags: ["Android Gradle", "项目结构", "Build Task", "Build Type", "签名配置"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 10847
comment_num: 7
---

原文地址：<http://tools.android.com/tech-docs/new-build-system/user-guide#TOC-
Basic-Project>

  

# 3、Basic Project（基本项目）

  
一个Gradle项目的构建过程定义在build.gradle文件中，位于项目的根目录下。  
  

## 3.1 Simple build files（简单的构建文件）

  
一个最简单的Gradle纯Java项目的build.gradle文件包含以下内容：  

    
    
        apply plugin: 'java'

这里引入了Gradle的Java插件。这个插件提供了所有构建和测试Java应用程序所需要的东西。  
  
最简单的Android项目的build.gradle文件包含以下内容：  

    
    
        buildscript {
            repositories {
                mavenCentral()
            }
    
            dependencies {
                classpath 'com.android.tools.build:gradle:0.11.1'
            }
        }
    
        apply plugin: 'android'
    
        android {
            compileSdkVersion 19
    
            buildToolsVersion "19.0.0"
    
        }

这里包括了Android build file的3个主要部分：  
  
buildscrip{...}这里配置了驱动构建过程的代码。  
在这个部分，它声明了使用Maven仓库，并且声明了一个maven文件的依赖路径。这个文件就是包含了0.11.1版本android gradle插件的库。  
注意：这里的配置只影响控制构建过程的代码，不影响项目源代码。项目本身需要声明自己的仓库和依赖关系，稍后将会提到这部分。  
  
接下来，跟前面提到的Java Plugin一样添加了android plugin。  
  
最后，andorid{...}配置了所有android构建过程需要的参数。这里也是Android DSL的入口点。  
默认情况下，只需要配置目标编译SDK版本和编译工具版本，即compileSdkVersion和buildToolsVersion属性。  
这个complieSdkVersion属性相当于旧构建系统中project.properites文件中的target属性。这个新的属性可以跟旧的target属性一样指定一个int或者String类型的值。  
  
重要：你只能添加android plugin。同时添加java plugin会导致构建错误。  
  
注意：你同样需要在相同路径下添加一个local.properties文件，并使用sdk.dir属性来设置SDK路径。  
另外，你也可以通过设置ANDROID_HOME环境变量，这两种方式没有什么不同，根据你自己的喜好选择其中一种设置。  
                  

## 3.2 Project Structure（项目结构）

  
上面提到的基本的构建文件需要一个默认的文件夹结构。Gradle遵循约定优先于配置的概念，在可能的情况尽可能提供合理的默认配置参数。  
  
基本的项目开始于两个名为“source sets”的组件，即main source code和test code。它们分别位于：  
   **  * src/main/  
    * src/androidTest/**  
里面每一个存在的文件夹对应相应的源组件。  
对于Java plugin和Android plugin来说，它们的Java源代码和资源文件路径如下：  
     *** java/  
    * resources/**  
但对于Android plugin来说，它还拥有以下特有的文件和文件夹结构：  
   **  * AndroidManifest.xml  
    * res/  
    * assets/  
    * aidl/  
    * rs/  
    * jni/**  
注意：src/androidTest/AndroidManifest.xml是不需要的，它会自动被创建。  
  
3.2.1 Configuring the Structure（配置项目结构）  
  
当默认的项目结构不适用的时候，你可能需要去配置它。根据Gradle文档，重新为Java项目配置sourceSets可以使用以下方法：  

    
    
        sourceSets {
            main {
                java {
                    srcDir 'src/java'
    
                }
    
                resources {
    
                    srcDir 'src/resources'
    
                }
    
            }
        }

注意：srcDir将会被添加到指定的已存在的源文件夹中（这在Gradle文档中没有提到，但是实际上确实会这样执行）。  
      
替换默认的源代码文件夹，你可能想要使用能够传入一个路径数组的srcDirs来替换单一的srcDir。以下是使用调用对象的另一种不同方法：  

    
    
        sourceSets {
            main.java.srcDirs = ['src/java']
            main.resources.srcDirs = ['src/resources']
        }

  
想要获取更多信息，可以参考Gradle文档中关于[Java
Pluign](http://gradle.org/docs/current/userguide/java_plugin.html)的部分。  
  
Android Plugin使用的是类似的语法。但是由于它使用的是自己的sourceSets，这些配置将会被添加在android对象中。  
以下是一个示例，它使用了旧项目结构中的main源码，并且将androidTest sourceSet组件重新映射到tests文件夹。  

    
    
        android {
            sourceSets {
                main {
                    manifest.srcFile 'AndroidManifest.xml'
                    java.srcDirs = ['src']
    
                    resources.srcDirs = ['src']
                    aidl.srcDirs = ['src']
    
                    renderscript.srcDirs = ['src']
    
                    res.srcDirs = ['res']
                    assets.srcDirs = ['assets']
    
                }
    
    
                androidTest.setRoot('tests')
            }
        }

  
注意：由于旧的项目结构将所有的源文件（java,aidl,renderscripthe和java资源文件）都放在同一个目录里面，所以我们需要将这些sourceSet组件重新映射到src目录下。  
注意：setRoot()方法将移动整个组件（包括它的子文件夹）到一个新的文件夹。示例中将会移动scr/androidTest/*到tests/*下。  
以上这些是Android特有的，如果配置在Java的sourceSets里面将不会有作用。  
  

以上也是将旧构建系统项目迁移到新构建系统需要做的迁移工作。

  

## 3.3 Build Tasks（构建任务）

  

### 3.3.1 General Tasks（通用任务）

  
添加一个插件到构建文件中将会自动创建一系列构建任务( **build tasks**
)去执行（注：gradle属于任务驱动型构建工具，它的构建过程是基于Task的）。Java plugin和Android plugin都会创建以下task：  
   **  * assemble：**这个task将会组合项目的所有输出。  
   **  * check：**这个task将会执行所有检查。  
     *** build：** 这个task将会执行assemble和check两个task的所有工作  
     *** clean：** 这个task将会清空项目的输出。  
实际上assemble，check，build这三个task不做任何事情。它们只是一个Task标志，用来告诉android
plugin添加实际需要执行的task去完成这些工作。  
  
这就允许你去调用相同的task，而不需要考虑当前是什么类型的项目，或者当前项目添加了什么plugin。  
例如，添加了findbugs plugin将会创建一个新的task并且让check task依赖于这个新的task。当check
task被调用的时候，这个新的task将会先被调用。  
  
在命令行环境中，你可以执行以下命令来获取更多高级别的task：  

    
    
        gradle tasks

  
查看所有task列表和它们之间的依赖关系可以执行以下命令：  

    
    
        gradle tasks --all

  
注意：Gradle会自动监视一个task声明的所有输入和输出。  
两次执行build task并且期间项目没有任何改动，gradle将会使用UP-TO-
DATE通知所有task。这意味着第二次build执行的时候不会请求任何task执行。这允许task之间互相依赖，而不会导致不需要的构建请求被执行。  
  

### 3.3.2 Java project tasks（Java项目的Task）

  
Java plugin主要创建了两个task，依赖于main task（一个标识性的task）：  
     *** assemble  
        * jar：**这个task创建所有输出  
**     * check  
        * test：**这个task执行所有的测试。  
jar task自身直接或者间接依赖于其他task：classes task将会被调用于编译java源码。  
testClasses task用于编译测试，但是它很少被调用，因为test task依赖于它（类似于classes task）。  
  
通常情况下，你只需要调用到assemble和check，不需要其他task。  
  
你可以在Gradle文档中查看[java
plugin](http://gradle.org/docs/current/userguide/java_plugin.html)的全部task。  
  

### 3.3.3 Android tasks

  
Android plugin使用相同的约定以兼容其他插件，并且附加了自己的标识性task，包括：  
     *** assemble：** 这个task用于组合项目中的所有输出。  
   **  * check：**这个task用于执行所有检查。  
**     * connectedCheck：**这个task将会在一个指定的设备或者模拟器上执行检查，它们可以同时在所有连接的设备上执行。  
   **  * deviceCheck：**通过APIs连接远程设备来执行检查，这是在CL服务器上使用的。  
**     * build：**这个task执行assemble和check的所有工作。  
**     * clean：**这个task清空项目的所有输出。  
这些新的标识性task是必须的，以保证能够在没有设备连接的情况下执行定期检查。  
注意build task不依赖于deviceCheck或者connectedCheck。  
  
一个Android项目至少拥有两个输出：debug APK（调试版APK)和release
APK（发布版APK）。每一个输出都拥有自己的标识性task以便能够单独构建它们。  
**     * assemble：  
        * assembleDebug  
        * assembleRelease**  
它们都依赖于其它一些tasks以完成构建一个APK需要多个步骤。其中assemble
task依赖于这两个task，所以执行assemble将会同时构建出两个APK。  
  
小提示：gradle在命令行终端上支持骆驼命名法的task简称，例如，执行gradle aR命令等同于执行gradle assembleRelease。  
  
check task也拥有自己的依赖：  
**     * check：  
        * lint  
    * connectedCheck：  
        * connectedAndroidTest  
        * connectedUiAutomatorTest(目前还没有应用到）  
    * deviceCheck: **这个test依赖于test创建时，其它实现测试扩展点的插件。  
  
最后，只要task能够被安装（那些要求签名的task），android plugin就会为所有构建类型（debug，release，test）安装或者卸载。  
  

## 3.4 Basic Build Customization（基本的构建定制）

  
Android plugin提供了大量DSL用于直接从构建系统定制大部分事情。  
  

### 3.4.1 Manifest entries （Manifest属性）

  
通过SDL可以配置一下manifest选项：  
**     * minSdkVersion  
    * targetSdkVersion  
    * versionName  
    * packageName  
    * package Name for the test application  
    * Instrumentation test runner**  
例如：  

    
    
        android {
            compileSdkVersion 19
    
            buildToolsVersion "19.0.0"
    
    
            defaultConfig {
                versionCode 12
    
                versionName "2.0"
    
                minSdkVersion 16
                targetSdkVersion 16
            }
        }

  
在android元素中的defaultConfig元素中定义所有配置。  
  
之前的Android
Plugin版本使用packageName来配置manifest文件中的packageName属性。从0.11.0版本开始，你需要在build.gradle文件中使用applicationId来配置manifest文件中的packageName属性。  
这是为了消除应用程序的packageName（也是程序的ID）和java包名所引起的混乱。  
  
在构建文件中定义的强大之处在于它是动态的。  
例如，可以从一个文件中或者其它自定义的逻辑代码中读取版本信息：  

    
    
        def computeVersionName() {
            ...
        }
    
        android {
            compileSdkVersion 19
            buildToolsVersion "19.0.0"
    
            defaultConfig {
                versionCode 12
                versionName computeVersionName()
                minSdkVersion 16
                targetSdkVersion 16
            }
        }

  
注意：不要使用与在给定范围内的getter方法可能引起冲突的方法名。例如，在defaultConfig{...}中调用getVersionName()将会自动调用defaultConfig.getVersionName()方法，你自定义的getVersionName()方法就被取代掉了。  
  

如果一个属性没有使用DSL进行设置，一些默认的属性值将会被使用。以下表格是可能使用到的值：

Property Name | Default value in DSL object | Default value  
---|---|---  
versionCode     | -1 | value from manifest if present  
versionName     | null     | value from manifest if present  
minSdkVersion   | -1 | value from manifest if present  
targetSdkVersion   | -1 | value from manifest if present  
packageName     | packageName     | value from manifest if present  
testPackageName | null     | app package name + “.test”  
testInstrumentationRunner | null     | android.test.InstrumentationTestRunner  
signingConfig     | null     | null  
proguardFile     | N/A (set only) | N/A (set only)  
proguardFiles     | N/A (set only) | N/A (set only)  
  

如果你在构建脚本中使用自定义代码逻辑请求这些属性，那么第二列的值将非常重要。例如，你可能会写：  

    
    
        if (android.defaultConfig.testInstrumentationRunner == null) {
            // assign a better default...
        }

如果这个值一直保持null，那么在构建执行期间将会实际替换成第三列的默认值。但是在DSL元素中并没有包含这个默认值，所以，你无法查询到这个值。  
除非是真的需要，这是为了预防解析应用的manifest文件。  
      

### 3.4.2 Build Types（构建类型）

  
默认情况下，Android Plugin会自动给项目设置同时构建应用程序的debug和release版本。  
两个版本之间的不同主要围绕着能否在一个安全设备上调试，以及APK如何签名。  
  
Debug版本采用使用通用的name/password键值对自动创建的数字证书进行签名，以防止构建过程中出现请求信息。Release版本在构建过程中没有签名，需要稍后再签名。  
  
这些配置通过一个BuildType对象来配置。默认情况下，这两个实例都会被创建，分别是一个debug版本和一个release版本。  
  
Android plugin允许像创建其他构建类型一样定制debug和release实例。这需要在buildTypes的DSL容器中配置：  

    
    
        android {
            buildTypes {
                debug {
                    applicationIdSuffix ".debug"
    
                }
    
                jnidebug.initWith(buildTypes.debug)
                jnidebug {
                    packageNameSuffix ".jnidebug"
                    jnidebugBuild true
                }
            }
        }

      
以上代码片段实现了以下功能：  
    * 配置默认的debug构建类型：将debug版本的包名设置为<app package>.debug以便能够同时在一台设备上安装debug和release版本的apk。  
    * 创建了一个名为“jnidebug”的新构建类型，并且这个构建类型是debug构建类型的一个副本。  
    * 继续配置jnidebug构建类型，允许使用JNI组件，并且也添加了不一样的包名后缀。  
      
创建一个新的构建类型就是简单的在buildType标签下添加一个新的元素，并且可以使用initWith()或者直接使用闭包来配置它。  
  

以下是一些可能使用到的属性和默认值：

Property name | Default values for debug | Default values for release / other  
---|---|---  
debuggable     | debuggable     | false  
jniDebugBuild     | false | false  
renderscriptDebugBuild   | false | false  
renderscriptOptimLevel   | 3 | 3  
packageNameSuffix     | null | null  
versionNameSuffix     | null | null  
signingConfig     | android.signingConfigs.debug | null  
zipAlign     | false     | true  
runProguard     | false | false  
proguardFile     | N/A (set only) | N/A (set only)  
proguardFiles     | N/A (set only) | N/A (set only)  
  

  
除了以上属性之外，Build Type还会受项目源码和资源影响：  
对于每一个Build Type都会自动创建一个匹配的sourceSet。默认的路径为：  

    
    
        src/<buildtypename>/

这意味着BuildType名称不能是main或者androidTest（因为这两个是由plugin强制实现的），并且他们互相之间都必须是唯一的。  
  
跟其他sourceSet设置一样，Build Type的source set路径可以重新被定向：  

    
    
        android {
            sourceSets.jnidebug.setRoot('foo/jnidebug')
        }

  
另外，每一个Build Type都会创建一个新的assemble<BuildTypeName>任务。  
  
assembleDebug和assembleRelease两个Task在上面已经提到过，这里要讲这两个Task从哪里被创建。当debug和release构建类型被预创建的时候，它们的tasks就会自动创建对应的这个两个Task。  
  
上面提到的build.gradle代码片段中也会实现assembleJnidebug
task，并且assemble会像依赖于assembleDebug和assembleRelease一样依赖于assembleJnidebug。  
  
提示：你可以在终端下输入gradle aJ去运行assembleJnidebug task。  
  
可能会使用到的情况：  
**     * release模式不需要，只有debug模式下才使用到的权限  
    * 自定义的debug实现  
    * 为debug模式使用不同的资源（例如当资源的值由绑定的证书决定）**  
BuildType的代码和资源通过以下方式被使用：  
**     * manifest将被混合进app的manifest  
    * 代码行为只是另一个资源文件夹  
    * 资源将叠加到main的资源中，并替换已存在的资源。**  
  

### 3.4.3 signing configurations（签名配置）

  
对一个应用程序签名需要以下：  
**     * 一个Keystory  
    * 一个keystory密码  
    * 一个key的别名  
    * 一个key的密码  
    * 存储类型**  
位置，键名，两个密码，还有存储类型一起形成了签名配置。  
      
默认情况下，debug被配置成使用一个debug keystory。debug keystory使用了默认的密码和默认key及默认的key密码。  
debug keystory的位置在$HOME/.android/debug.keystroe，如果对应位置不存在这个文件将会自动创建一个。  
  
debug构建类型会自动使用debug签名配置。  
  
可以创建其他配置或者自定义内建的默认配置。通过signingConfigs这个DSL容器来配置：  

    
    
        android {
            signingConfigs {
                debug {
                    storeFile file("debug.keystore")
                }
    
                myConfig {
                    storeFile file("other.keystore")
                    storePassword "android"
                    keyAlias "androiddebugkey"
                    keyPassword "android"
                }
            }
    
            buildTypes {
                foo {
                    debuggable true
                    jniDebugBuild true
                    signingConfig signingConfigs.myConfig
                }
            }
        }

以上代码片段修改debug keystory的路径到项目的根目录下。在这个例子中，这将自动影响其他使用到debug构建类型的构建类型。  
  
这里也创建了一个新的Single Config（签名配置）和一个使用这个新签名配置的新的Build Type（构建类型）。  
  
注意：只有默认路径下的debug keystory不存在时会被自动创建。更改debug keystory的路径并不会自动在新路径下创建debug
keystory。如果创建一个新的不同名字的SignConfig，但是使用默认的debug
keystore路径来创建一个非默认的名字的SigningConing，那么还是会在默认路径下创建debug
keystory。换句话说，会不会自动创建是根据keystory的路径来判断，而不是配置的名称。  
  
注意：虽然经常使用项目根目录的相对路径作为keystore的路径，但是也可以使用绝对路径，尽管这并不推荐（除了自动创建出来的debug keystore）。  
  
注意：如果你将这些文件添加到版本控制工具中，你可能不希望将密码直接写到这些文件中。下面Stack
Overflow链接提供从控制台或者环境变量中获取密码的方法：  
<http://stackoverflow.com/questions/18328730/how-to-create-a-release-signed-
apk-file-using-gradle>  
我们以后还会在这个指南中添加更多的详细信息。  
  

### 3.4.4 Running Proguard（运行 Proguard）

  
从Gradle Plugin for ProGuard version
4.10之后就开始支持ProGuard。ProGuard插件是自动添加进来的。如果Build
Type的runProguard属性被设置为true，对应的task将会自动创建。  

    
    
    android {
        buildTypes {
            release {
                runProguard true
                proguardFile getDefaultProguardFile('proguard-android.txt')
            }
        }
    
        productFlavors {
            flavor1 {
            }
            flavor2 {
                proguardFile 'some-other-rules.txt'
            }
    
        }
    }

  
发布版本将会使用它的Build Type中声明的规则文件，product flavor（定制的产品版本）将会使用对应flavor中声明的规则文件。  
  
这里有两个默认的规则文件：  
**     * proguard-android.txt  
    * proguard-android-optimize.txt**  
这两个文件都在SDK的路径下。使用getDefaultProguardFile()可以获取这些文件的完整路径。它们除了是否要进行优化之外，其它都是相同的。  
[](http://tools.android.com/tech-docs/new-build-system/user-guide#TOC-Basic-
Project)

