---
# type: posts 
title: "树莓派的.bashrc和.bash_aliases文件"
date: 2014-09-30T17:43:58+0800
authors: ["Jianan"]
summary: "在你的home目录中，你可以找到一个包含用户配置的隐藏文件.bashrc。你可以根据自己的需要修改这个文件。

文件中为你提供了一些有用的调整设置，默认情况下其中一些设置是被注释掉的。

例如，一些ls命令的别名：
alias ls='ls --color=auto'
#alias dir='dir --color=auto'
#alias vdir='vdir --color=auto"
series: ["RaspBerry"]
categories: ["翻译", "RaspBerry"]
tags: ["RaspBerry", "树莓派", ".bashrc", ".bash_aliases"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 3034
comment_num: 3
---

  

在你的home目录中，你可以找到一个包含用户配置的隐藏文件 **.bashrc** 。你可以根据自己的需要修改这个文件。

  
文件中为你提供了一些有用的调整设置，默认情况下其中一些设置是被注释掉的。

  
例如，一些 **ls** 命令的别名：

    
    
    alias ls='ls --color=auto'
    #alias dir='dir --color=auto'
    #alias vdir='vdir --color=auto'
    
    alias grep='grep --color=auto'
    alias fgrep='fgrep --color=auto'
    alias egrep='egrep --color=auto'

  
类似上面提供的别名是用来帮助其它系统的用户熟悉使用（例如 **dir** 相当于DOS/Windows系统中的 **ls** ）。另外一些是为了给像
**ls** 和 **grep** 这些命令的输出结果添加颜色。

  
其中也提供了更多 **ls** 命令的变体别名：

    
    
    # some more ls aliases
    #alias ll='ls -l'
    #alias la='ls -A'
    #alias l='ls -CF'

  
Ubuntu用户应该对这些设置更加熟悉，因为这是Ubuntu发行版本的默认设置。取消这些命令的注释就可以让这些别名在稍后生效。

  
以 **“#”** 开头的命令行表示被注释。为了启用它们，只需要将 **“#”** 移除，等下次启动你的树莓派就会激活这些设置。

  
这里同样有关于 **.bash_aliases** 文件的引用，默认情况下这个文件并不存在：

    
    
    if [ -f ~/.bash_aliases ]; then
        . ~/.bash_aliases
    fi

  
这里的if条件用于在导入这个文件之间检查文件是否存在。

  
你可以创建这个 **.bash_aliases** 文件，并在里面添加更多的别名，例如：

    
    
    alias gs='git status'

  
你也可以直接添加其它东西到这个文件中或者是其它文件，只要将文件像 **.bash_aliases** 文件一样包含进来就可以。

  

  

原文地址：<http://www.raspberrypi.org/documentation/linux/usage/bashrc.md>

  

  

