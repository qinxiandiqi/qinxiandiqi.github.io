---
# type: posts 
title: "二进制包安装golang"
date: 2015-02-12T23:08:43+0800
authors: ["Jianan"]
summary: "之前讲过arm平台上的golang的源代码编译安装，这次补充下golang官方提供的其它平台上二进制包安装方法。"
series: ["go"]
categories: ["go"]
tags: ["go", "二进制包", "安装", "golang"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 3022
comment_num: 0
---

  

之前讲过[arm平台上的golang的源代码编译安装](http://blog.csdn.net/qinxiandiqi/article/details/42918067)，这次补充下golang官方提供的其它平台上二进制包安装方法。

  

# 1、下载golang二进制包

  

首先是要下载golang的二进制包，官方下载地址：<https://golang.org/dl/>

选择对应平台的二进制包，目前golang官方只提供了以下平台的二进制包：

1.1 基于386或amd64处理器的Mac OS X 10.6+平台二进制包

1.2 基于386或amd64处理器的FreeBSD 8+平台的二进制包

1.3 基于386或amd64处理器的Linux 2.6.23+平台的二进制包，需要注意的是不支持CentOS/RedHat 5平台

1.4 基于386或amd64处理器的Window XP+平台的二进制包

如果你的平台不在上列，则无法使用官方提供的二进制包安装，需要直接编译源代码安装。

  

# 2、清理旧版本golang

  

如果先前已经安装了旧版本的golang，在安装新版本之前需要先清理旧版本的golang，分两个步骤：

  

## 2.1 删除旧版本golang目录

  

通常情况下，Linux、Mac OS
X或者FreeBSD平台的go目录在/usr/local/go，Window平台的go目录可能在C:\go。也有可能在你自定义的其它路径，请直接删除即可。

  

## 2.2 删除版本golang环境变量

  

只需要从PATH环境变量删除旧版本go目录的bin路径即可。  

FreeBSD或者Linux通常修改/etc/profile或者$HOME/.profile，根据你显现配置PATH环境变量的位置决定。

Mac OS X平台上，如果旧版本使用package安装包方式安装，需要删除`/etc/paths.d/go`文件。

Window平台上，到系统属性的高级属性配置PATH变量。

  

# 3、Linux、Mac OS X或FreeBSD平台上的tar压缩包安装

  

## 3.1 解压tar压缩包

  

下载对应的tar压缩包之后，执行以下命令将压缩包解压到/usr/local目录下：

    
    
    sudo tar -C /usr/local -xzf goxxx.tar.gz

其中goxxx.tar.gz为你所下载go压缩包，解压后go的目录为/usr/local/go。  

  

## 3.2 配置环境变量

  

将/usr/local/go/bin路径配置到PATH环境变量中，可以添加在/etc/profile或者$HOME/.profile文件中：

    
    
    export PATH=$PATH:/usr/local/go/bin

配置完后，根据你配置的文件执行source /etc/profile或者source $HOME/.profile让环境变量生效。

  

## 3.3 自定义安装路径

  

不选择/usr/local目录，选择其它路径也是可以的，只要将压缩包解压到你想要的目录下就可以，只不过需要多添加一个 **GOROOT**
环境变量指明你自定义的路径。因此，配置环境变量的内容为：

    
    
    export GOROOT=自定义go路径
    export PATH=$PATH:$GOROOT/bin

同样执行source命令让配置的环境变量生效。

  

# 4、Mac OS X平台的package包安装

  

Mac OS
X平台下使用package包来安装，按照操作提示进行安装。它会将go安装到/usr/local/go，并自动将/usr/local/go/bin配置到PATH环境变量中。打开新的终端才能检查到go的环境变量，已打开的终端检测不到。所以要测试go，需要打开新的终端。

  

# 5、Window平台的安装

  

## 5.1 window平台的msi安装包安装

  

打开msi安装包，根据提示安装go。默认会将go安装到C:\go目录，并将C:\go\bin添加到PATH环境变量中。同样，你需要打开新的终端才能看到配置的环境变量生效。

  

## 5.2 window平台的zip压缩包安装

  

与Linux等平台的tar压缩包安装类似，可以将zip压缩包解压到任何路径，官方建议解压到C:\go目录。同样需要自己手动添加GOROOT环境变量指明自己的go目录路径，并将go目录下的bin路径添加到PATH环境变量中。

  

# 6、测试安装

  

建立hello.go文件，输入以下代码：

    
    
    package main
    
    import "fmt"
    
    func main() {
        fmt.Printf("hello, world\n")
    }

保存后，在终端上执行hello.go文件：

    
    
    $ go run hello.go

如果你看到hello world说明安装成功。  

  

