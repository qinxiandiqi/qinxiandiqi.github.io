---
# type: posts 
title: "Android Gradle Plugin指南（四）——测试"
date: 2014-07-15T17:27:20+0800
authors: ["Jianan"]
summary: "原文地址：http://tools.android.com/tech-docs/new-build-system/user-guide#TOC-Testing


5、Testing（测试）


构建一个测试程序已经被集成到应用项目中，没有必要再专门建立一个测试项目。


5.1 Basics and Configuration（基本知识和配置）


正如前面所提到的，紧邻"
series: ["Android Gradle"]
categories: ["翻译", "Android Gradle"]
tags: ["Android Gradle", "gradle", "自动化测试", "单元测试", "合并"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 4729
comment_num: 3
---

原文地址：<http://tools.android.com/tech-docs/new-build-system/user-guide#TOC-
Testing>

#  

# 5、Testing（测试）

  

构建一个测试程序已经被集成到应用项目中，没有必要再专门建立一个测试项目。

  

## 5.1 Basics and Configuration（基本知识和配置）

  

正如前面所提到的，紧邻main sourceSet的就是androidTest sourceSet，默认路径在src/androidTest/下。

在这个测试sourceSet中会构建一个使用Android测试框架，并且可以部署到设备上的测试apk来测试应用程序。这里面包含单元测试，集成测试，和后续UI自动化测试。

这个测试sourceSet不应该包含AndroidManifest.xml文件，因为这个文件会自动生成。

  
下面这些值可能会在测试应用配置中使用到：

**     * testPackageName**

**     * testInstrumentationRunner**

**     * testHandleProfiling**

**     * testfunctionalTest**

  
正如前面所看到的，这些配置在defaultConfig对象中配置：

    
    
        android {
            defaultConfig {
                testPackageName "com.test.foo"
                testInstrumentationRunner "android.test.InstrumentationTestRunner"
                testHandleProfiling true
                testFunctionalTest true
            }
        }

在测试应用程序的manifest文件中，instrumentation节点的targetPackage属性值会自动使用测试应用的package名称设置，即使这个名称是通过defaultConfig或者Build
Type对象自定义的。这也是manifest文件需要自动生成的一个原因。

  
另外，这个测试sourceSet也可以拥有自己的依赖。

默认情况下，应用程序和他的依赖会自动添加的测试应用的classpath中，但是也可以通过以下来扩展：

    
    
        dependencies {
            androidTestCompile 'com.google.guava:guava:11.0.2'
        }

  

测试应用通过assembleTest task来构建。assembleTest不依赖于main中的assemble
task，需要手动设置运行，不能自动运行。

  
目前只有一个Build Type被测试。默认情况下是debug Build Type，但是这也可以通过以下自定义配置：

    
    
        android {
            ...
            testBuildType "staging"
        }

  

## 5.2 Running tests（运行测试）

  

正如前面提到的，标志性task connectedCheck要求一个连接的设备来启动。

这个过程依赖于androidTest task，因此将会运行androidTest。这个task将会执行下面内容：

**     * 确认应用和测试应用都被构建（依赖于assembleDebug和assembleTest）。**

**     * 安装这两个应用。**

**     * 运行这些测试。**

**     * 卸载这两个应用。**

  
如果有多于一个连接设备，那么所有测试都会同时运行在所有连接设备上。如果其中一个测试失败，不管是哪一个设备算失败。

  
所有测试结果都被保存为XML文档，路径为：

**     build/androidTest-results**

（这类似于JUnit的运行结果保存在build/test-results)

  

同样，这也可以自定义配置：

    
    
        android {
            ...
    
            testOptions {
                resultsDir = "$project.buildDir/foo/results"
            }
        }

这里的android.testOptions.resultsDir将由Project.file(String)获得。

  

## 5.3 Testing Android Libraries（测试Android库）

  

测试Android库项目的方法与应用项目的方法类似。

  
唯一的不同在于整个库（包括它的依赖）都是自动作为依赖库被添加到测试应用中。结果就是测试APK不单只包含它的代码，还包含了库项目自己和库的所有依赖。

库的manifest被组合到测试应用的manifest中（作为一些项目引用这个库的壳）。

  
androidTest task的变改只是安装（或者卸载）测试APK（因为没有其它APK被安装）。

  
其它的部分都是类似的。

  

## 5.4 Test reports（测试报告）

  

当运行单元测试的时候，Gradle会输出一份HTML格式的报告以方便查看结果。

Android plugin也是基于此，并且扩展了HTML报告文件，它将所有连接设备的报告都合并到一个文件里面。

  

### 5.4.1 Single projects（独立项目）

  

一个项目将会自动生成测试运行。默认位置为： **build/reports/androidTests**

  
这非常类似于JUnit的报告所在位置build/reports/tests，其它的报告通常位于build/reports/<plugin>/。

  
这个路径也可以通过以下方式自定义：

    
    
        android {
            ...
    
            testOptions {
                reportDir = "$project.buildDir/foo/report"
            }
        }

  

报告将会合并运行在不同设备上的测试结果。

  

### 5.4.2 Multi-projects reports（多项目报告）

  

在一个配置了多个应用或者多个库项目的多项目里，当同时运行所有测试的时候，生成一个报告文件记录所有的测试可能是非常有用的。

  
为了实现这个目的，需要使用同一个依赖文件（译注：指的是使用android gradle插件的依赖文件）中的另一个插件。可以通过以下方式添加：

    
    
        buildscript {
            repositories {
                mavenCentral()
            }
    
            dependencies {
                classpath 'com.android.tools.build:gradle:0.5.6'
            }
        }
    
        apply plugin: 'android-reporting'

这必须添加到项目的根目录下，例如与settings.gradle文件同个目录的build.gradle文件中。

  
之后，在命令行中导航到项目根目录下，输入以下命令就可以运行所有测试并合并所有报告：

    
    
    gradle deviceCheck mergeAndroidReports --continue

  

注意：这里的--
continue选项将允许所有测试，即使子项目中的任何一个运行失败都不会停止。如果没有这个选项，第一个失败测试将会终止全部测试的运行，这可能导致一些项目没有执行过它们的测试。

  

## 5.5 Lint support（Lint支持，译者注：Lint是一个可以检查Android项目中存在的问题的工具）

  

从0.7.0版本开始，你可以为项目中一个特定的Variant（变种）版本运行lint，也可以为所有[Variant](http://blog.csdn.net/qinxiandiqi/article/details/37906449)版本都运行lint。它将会生成一个报告描述哪一个Variant版本中存在着问题。

  
你可以通过以下lint选项配置lint。通常情况下你只需要配置其中一部分，以下列出了所有可使用的选项：

    
    
        android {
    
            lintOptions {
    
                // set to true to turn off analysis progress reporting by lint
                quiet true
                // if true, stop the gradle build if errors are found
                abortOnError false
                // if true, only report errors
                ignoreWarnings true
                // if true, emit full/absolute paths to files with errors (true by default)
                //absolutePaths true
                // if true, check all issues, including those that are off by default
                checkAllWarnings true
                // if true, treat all warnings as errors
                warningsAsErrors true
                // turn off checking the given issue id's
                disable 'TypographyFractions','TypographyQuotes'
                // turn on the given issue id's
                enable 'RtlHardcoded','RtlCompat', 'RtlEnabled'
                // check *only* the given issue id's
                check 'NewApi', 'InlinedApi'
                // if true, don't include source code lines in the error output
                noLines true
                // if true, show all locations for an error, do not truncate lists, etc.
                showAll true
                // Fallback lint configuration (default severities, etc.)
                lintConfig file("default-lint.xml")
                // if true, generate a text report of issues (false by default)
                textReport true
                // location to write the output; can be a file or 'stdout'
                textOutput 'stdout'
                // if true, generate an XML report for use by for example Jenkins
                xmlReport false
                // file to write report to (if not specified, defaults to lint-results.xml)
                xmlOutput file("lint-report.xml")
                // if true, generate an HTML report (with issue explanations, sourcecode, etc)
                htmlReport true
                // optional path to report (default will be lint-results.html in the builddir)
                htmlOutput file("lint-report.html")
    
               // set to true to have all release builds run lint on issues with severity=fatal
    
               // and abort the build (controlled by abortOnError above) if fatal issues are found
    
               checkReleaseBuilds true
    
                // Set the severity of the given issues to fatal (which means they will be
                // checked during release builds (even if the lint target is not included)
                fatal 'NewApi', 'InlineApi'
                // Set the severity of the given issues to error
                error 'Wakelock', 'TextViewEdits'
                // Set the severity of the given issues to warning
                warning 'ResourceAsColor'
                // Set the severity of the given issues to ignore (same as disabling the check)
                ignore 'TypographyQuotes'
            }
    
        }

  
  

  
[](http://tools.android.com/tech-docs/new-build-system/user-guide#TOC-Testing)

