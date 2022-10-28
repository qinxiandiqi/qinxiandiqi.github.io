---
# type: posts 
title: "树莓派安装node.js"
date: 2015-06-28T09:52:25+0800
authors: ["Jianan"]
summary: "由于树莓派是arm架构，node.js上并没有提供arm架构的二进制包下载。试过使用apt包管理安装和直接node.js源码编译安装（编译了四五个小时。。。），都没有成功，报非法指令错误，估计还是编译的处理器架构错误导致的。不过，google上有些小伙伴成功了，我也不清楚是为什么，可能是编译的版本问题。
这里提供一个比较简单的方法，亲测有效。其实就是在官网的历史列表里面找到了个旧版本v0.10."
series: ["RaspBerry"]
categories: ["RaspBerry"]
tags: ["node.js", "arm架构", "raspberry", "树莓派"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 2153
comment_num: 0
---

  

由于树莓派是arm架构，node.js上并没有提供arm架构的二进制包下载。试过使用apt包管理安装和直接node.js源码编译安装（编译了四五个小时。。。），都没有成功，报非法指令错误，估计还是编译的处理器架构错误导致的。不过，google上有些小伙伴成功了，我也不清楚是为什么，可能是编译的版本问题。

这里提供一个比较简单的方法，亲测有效。其实就是在官网的历史列表里面找到了个旧版本v0.10.28的arm架构二进制包，官网的历史列表
http://nodejs.org/dist。

部署方法如下：

    
    
    wget http://nodejs.org/dist/v0.10.28/node-v0.10.28-linux-arm-pi.tar.gz
    tar -xzf node-v0.10.28-linux-arm-pi.tar.gz

  
解压后就能看到node-v0.10.28-linux-arm-
pi目录，里面的bin目录包含了node和npm。为了使用方便，将bin目录路径配置到PATH环境变量里面。

编辑~/.bashrc文件，在文件中添加NODE环境变量，并添加到PATH中：

    
    
    export NODE=你的node-v0.10.28-linux-arm-pi的路径
    export PATH=$PATH:$NODE/bin

  

保存后，执行 source ~/.bashrc命令。这样就可以直接使用node和npm命令，可以试试node -v或者npm
-v命令打印下当前的版本号，正确打印出来则成功。

