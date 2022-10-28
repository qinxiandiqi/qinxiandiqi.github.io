---
# type: posts 
title: "将IDEA maven项目中src源代码下的xml等资源文件编译进classes文件夹"
date: 2015-05-13T10:56:16+0800
authors: ["Jianan"]
summary: "如题，IDEA的maven项目中，默认源代码目录下的xml等资源文件并不会在编译的时候一块打包进classes文件夹，而是直接舍弃掉。
如果使用的是Eclipse，Eclipse的src目录下的xml等资源文件在编译的时候会自动打包进输出到classes文件夹。Hibernate和Spring有时会将配置文件放置在src目录下，编译后要一块打包进classes文件夹，所以存在着需要将xml等资源"
series: ["IDE"]
categories: ["IDE"]
tags: ["idea", "maven", "xml", "src", "classes"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 15048
comment_num: 5
---

  

如题，IDEA的maven项目中，默认源代码目录下的xml等资源文件并不会在编译的时候一块打包进classes文件夹，而是直接舍弃掉。

如果使用的是Eclipse，Eclipse的src目录下的xml等资源文件在编译的时候会自动打包进输出到classes文件夹。Hibernate和Spring有时会将配置文件放置在src目录下，编译后要一块打包进classes文件夹，所以存在着需要将xml等资源文件放置在源代码目录下的需求。

解决IDEA的这个问题有两种方式。

第一种是建立src/main/resources文件夹，将xml等资源文件放置到这个目录中。maven工具默认在编译的时候，会将resources文件夹中的资源文件一块打包进classes目录中。

第二种解决方式是配置maven的pom文件配置，在pom文件中找到<build>节点，添加下列代码：

    
    
        <build>
            <resources>
                <resource>
                    <directory>src/main/java</directory>
                    <includes>
                        <include>**/*.xml</include>
                    </includes>
                </resource>
            </resources>
        </build>

其中<directory>src/main/java</directory>表明资源文件的路径，<include>**/*.xml</include>表明需要编译打包的文件类型是xml文件，如果有其它资源文件也需要打包，可以修改或添加通配符。

  

