---
# type: posts 
title: "C语言的fopen文件操作相对路径到底相对哪？"
date: 2023-02-02T00:13:45+08:00
authors: ["Jianan"]
summary: 
series: ["征服C/C++"]
categories: ["C/C++"]
tags: ["C", "C++"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 0
comment_num: 0 
---

## C语言怎么打开文件？

C语言要打开一个文件可以使用标准库的fopen函数，来看下fopen函数的声明：
```c
FILE *fopen(const char *filename, const char *mode)
``` 
其中，filename参数的类型是字符串，用于指定要打开文件的路径，可以是绝对路径，也可以是相对路径。mode参数的类型也是字符串，用于指定打开文件的操作模式，比如"r"(只读模式)等等。  

那么，这里要讨论的是filename参数使用相对路径的时候，相对位置到底是哪里呢？  
1、源代码文件所在路径？  
2、源代码编译后的可执行文件所在路径？  
3、我们自己当前所在的路径？  

**你觉得是哪里？**  

答案是3，我们自己当前所在的路径。Show me the code，写个代码证明一下。 
 
## 证明fopen函数相对路径的位置

创建个新目录CFileTest，在目录下面创建main.c源文件，hello.txt文本文件，以及build子目录（存放编译后的可执行文件）。目录结构如下：  
```shell
CFileTest
  - build
  - hello.txt
  - main.c
```
hello.txt只是一个普通的文本文件，里面只有Hello World字符串。  
main.c源代码的功能也很简单，就是使用fopen函数打开相对路径下的hello.txt文本文件，并打印出文件内的字符串：  
```c
#include <errno.h>
#include <stdio.h>
#include <string.h>

int main() {
  FILE* pFile;
  //以只读模式打开相对路径下的hello.txt文件
  pFile = fopen("hello.txt", "r");

  //如果文件打开失败，打印错误码和错误描述
  if (pFile == NULL) {
    int errorCode = errno;
    char* errorMsg = strerror(errorCode);
    printf("Open file fail, errorCode:%d, errorMsg:%s\n", errorCode, errorMsg);
    return -1;
  }

  //如果文件打开成功，打印出文件内容
  int c;
  while (1) {
    c = fgetc(pFile);
    if (feof(pFile)) {
      printf("\n");
      break;
    }
    printf("%c", c);
  }

  fclose(pFile);
  return 0;
}
```
1、证明相对路径不是相对于源文件

现在进入build子目录，在build子目录下编译main.c，并把可执行文件指定到build目录：  
```shell
nan@HWin-Jianan:~/CFileTest$ cd build
nan@HWin-Jianan:~/CFileTest/build$ gcc ../main.c -o main
nan@HWin-Jianan:~/CFileTest/build$ ls -l
total 20
-rwxr-xr-x 1 nan nan 17016 Feb  4 00:14 main
```  
可以看到在build子目录下已经生成main可执行文件，当前的目录结构如下：  
```shell
CFileTest
  - build
    - main
  - hello.txt
  - main.c
```
在build目录执行下main可执行文件： 
```shell
nan@HWin-Jianan:~/CFileTest/build$ ./main
Open file fail, errorCode:2, errorMsg:No such file or directory
```
可以看到hello.txt文件和main.c源文件同在一个目录，但结果却报错了，找不到hello.txt这个文件。说明"hello.txt"这个相对路径不是相对于源文件的路径。 

2、证明相对路径不是相对于可执行文件所在路径

把hello.txt文件移动到build目录下：  
```shell
nan@HWin-Jianan:~/CFileTest/build$ mv ../hello.txt ./
```
当前目录结构如下：
```shell
CFileTest
  - build
    - hello.txt
    - main
  - main.c
```
在build目录下继续执行main可执行文件： 
```shell
nan@HWin-Jianan:~/CFileTest/build$ ./main
Hello World!
``` 
可以看到成功打印出了hello.txt文件内容"Hello World"，这能确定相对路径是相对于可执行文件所在路径了吗？别急，别忘了我们自己当前所在路径也在build目录下，也有可能是因为相对于我们自己当前所在路径才成功找到文件的。那么，切换下我们自己当前所在的路径看看，切换到上一级CFileTest目录再执行看看：
```shell
nan@HWin-Jianan:~/CFileTest/build$ cd ..
nan@HWin-Jianan:~/CFileTest$ ./build/main
Open file fail, errorCode:2, errorMsg:No such file or directory
```
又找不到文件了，可以确定相对路径也不是相对于可执行文件所在的路径。  
> 说明：可执行文件所在路径不一定就是我们自己当前所在的路径。比如上面这次实验，可执行文件在build目录下，我们自己当前所在的路径在CFileTest目录下。

3、证明相对路径相对于我们自己当前所在路径  

再次把hello.txt文件移动到CFileTest目录下：
```shell
nan@HWin-Jianan:~/CFileTest$ mv ./build/hello.txt ./
``` 
移动后目录结果恢复为： 
```shell
CFileTest
  - build
    - main
  - hello.txt
  - main.c
``` 
在CFileTest目录下执行可执行文件： 
```shell
nan@HWin-Jianan:~/CFileTest$ ./build/main
Hello World!
``` 
成功找到hello.txt文件，说明相对路径是相对于我们自己当前所在路径的。  
**就这？** 有更直接的证据吗？那就gdb debug模式打个断点，打印下执行文件时当前的路径。  
首先重新gcc编译下源文件，增加-g参数保留下调试信息：  
```shell
nan@HWin-Jianan:~/CFileTest$ gcc -g main.c -o ./build/main
```
使用gdb调试main可执行文件，在源代码第8行"pFile = fopen("hello.txt", "r");"处打个断点,然后运行程序到这个断点：  
```shell
nan@HWin-Jianan:~/CFileTest$ gdb ./build/main
GNU gdb (Ubuntu 9.2-0ubuntu1~20.04.1) 9.2
...
Reading symbols from ./build/main...
(gdb) b 8
Breakpoint 1 at 0x1235: file main.c, line 8.
(gdb) r
Starting program: /home/nan/CFileTest/build/main

Breakpoint 1, main () at main.c:8
8         pFile = fopen("hello.txt", "r");
```
输入pwd命令，打印可执行文件时当前路径： 
```shell
(gdb) pwd
Working directory /home/nan/CFileTest.
``` 
可以看到，文件执行时的工作目录在CFileTest，与我们自己当前所在的目录一致。

## 总结
C语言的fopen函数，如果指定相对路径的文件需要注意一下，相对路径是相对于我们自己当前所在的路径，而不是源文件或者可执行文件的路径。

