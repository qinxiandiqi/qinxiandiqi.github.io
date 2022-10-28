---
# type: posts 
title: "Bash脚本的空格和“期待一元表达式”错误"
date: 2014-11-30T20:37:27+0800
authors: ["Jianan"]
summary: "很少自己写Bash脚本，一写就出现了一些奇怪的问题，主要还是对Bash脚本的语法不够熟悉，毕竟很少使用。
当做记录一下吧，今天因为空格导致的一些脚本问题：


1、Bash脚本中的赋值符号“=”前后不能有空格。例如给变量number赋值要写成“number=1”，不能写成“number = 1”。大多数编程语言都会忽略掉一些没有意义的空格，例如对于Java语言上面两种写法在语法上都是正确，"
series: ["Linux"]
categories: ["Linux"]
tags: ["bash", "脚本", "空格", "期待一元表达式"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 31293
comment_num: 5
---

  

很少自己写Bash脚本，一写就出现了一些奇怪的问题，主要还是对Bash脚本的语法不够熟悉，毕竟很少使用。

当做记录一下吧，今天因为空格导致的一些脚本问题：

  

1、Bash脚本中的赋值符号“=”前后不能有空格。例如给变量number赋值要写成“number=1”，不能写成“number =
1”。大多数编程语言都会忽略掉一些没有意义的空格，例如对于Java语言上面两种写法在语法上都是正确，但是Bash脚本不会。

  

2、Bash脚本中的“["和"];"中括号是个语法标识符，前后一定要留空格。例如：if [ "$number" -el 1 ]" then...
如果前后没有空格就会导致语法错误，提示”期待一元表达式“或者缺少一部分中括号之类的一些语法错误。

  

另外还有很多Bash常见的语法陷阱，以下两篇博文总结的挺好，别人辛辛苦苦写的文章我就不抄过来了，感兴趣的童鞋请移步：

1、Bash的陷阱：<http://blog.charlee.li/bash-pitfalls/>

2、Bash空格的那些事：<http://www.igigo.net/post/archives/152>  

