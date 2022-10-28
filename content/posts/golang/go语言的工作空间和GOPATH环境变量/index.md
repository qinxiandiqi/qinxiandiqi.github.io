---
# type: posts 
title: "go语言的工作空间和GOPATH环境变量"
date: 2015-02-19T15:10:19+0800
authors: ["Jianan"]
summary: "go语言并没有强制一定要使用一定的工作空间和项目结构，对于小型的go程序依靠go run等命令就可以直接编译运行。然而，保持良好的工作空间和文件结构，对于管理源代码和发布程序都是非常有帮助的。对于大型的go语言项目，工作空间则是一定要的。


1、go语言的工作空间结构


go语言的工作空间其实就是一个文件目录，目录中必须包含src、pkg、bin三个目录。
其中src目录用于存放"
series: ["go"]
categories: ["go"]
tags: ["go语言", "工作空间", "GOPATH", "golang", "go"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 8019
comment_num: 0
---

  

go语言并没有强制一定要使用一定的工作空间和项目结构，对于小型的go程序依靠go
run等命令就可以直接编译运行。然而，保持良好的工作空间和文件结构，对于管理源代码和发布程序都是非常有帮助的。对于大型的go语言项目，工作空间则是一定要的。

  

# 1、go语言的工作空间结构

  

go语言的工作空间其实就是一个文件目录，目录中必须包含src、pkg、bin三个目录。

其中src目录用于存放go源代码，pkg目录用于package对象，bin目录用于存放可执行对象。使用go的编译命令工具可以将源代码或package编译后的二进制输出对应存储到bin和pkg目录中。src目录中的源代码根据package名分类到对应的子目录中，并且可以使用各种版本控制工具。举个例子，go的工作空间目录结构大致如下：  

    
    
    bin/
        hello                          # 可执行命令
        outyet                         # 可执行命令
    pkg/
        linux_amd64/
            github.com/golang/example/
                stringutil.a           # package对象
    src/
        github.com/golang/example/
            .git/                      # Git仓库数据
            hello/
                hello.go               # 源代码
            outyet/
                main.go                # 源代码
                main_test.go           # 测试源代码
            stringutil/
                reverse.go             # package源代码
                reverse_test.go        # 测试源代码

上面的工作空间中包含了一个名为example的仓库，其中包含了hello和outyet两个命令，还有一个stringutil库。另外，一个工作空间中通常都会包含多个仓库。

  

# 2、GOPATH环境变量

  

GOPATH是go语言中跟工作空间相关的环境变量，这个变量指定go语言的工作空间位置。

当你建立工作空间目录后，你需要把工作空间目录的路径添加的GOPATH环境变量中。GOPATH环境变量支持多个值，如果你有多个工作空间，可以把多个工作空间值都添加到这个环境变量中，window系统使用分号";"分隔不同值，Linux或Unix系统使用冒号”:“分隔不同值。另外，还要将所有工作空间的bin路径添加到PATH环境变量中。在Linux系统下可以在~/.profile文件末尾添加如下内容：

    
    
    $ export GOPATH=你的工作空间路径
    $ export PATH=$PATH:$GOPATH/bin

当然，如果你的工作空间不止一个，PATH变量中不能直接使用$GOPATH/bin，要分别将各个工作空间中的bin路径添加进去。

需要注意的是，GOPATH环境变量的值不能与安装的go目录相同。go目录中同样有src、pkg、bin等类似工作空间的目录结构，不过其中包含的是go的标准模块，最好不要将自己的工作空间和go目录混合，对于以后升级go版本也比较容易。

  

  

  

