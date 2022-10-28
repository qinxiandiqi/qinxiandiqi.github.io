---
# type: posts 
title: "安装Python的easy_install工具和BeautifulSoup模块"
date: 2015-01-22T17:31:36+0800
authors: ["Jianan"]
summary: "1、esay_install


easy_install是Python的发行包管理工具，类似于linux的apt-get或者yum包管理工具，使用easy_install可以很方便的获取第三方的Python发行模块。
安装方法：
1.1 Mac OS X 系统可以在终端执行以下命令：

curl https://bootstrap.pypa.io/ez_setup.py -o -"
series: ["Python"]
categories: ["Python"]
tags: ["python", "easy_install", "BeautifulSoup4", "爬虫"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 3603
comment_num: 1
---

  

# 1、esay_install

  

easy_install是Python的发行包管理工具，类似于linux的apt-
get或者yum包管理工具，使用easy_install可以很方便的获取第三方的Python发行模块。

安装方法：

1.1 Mac OS X 系统可以在终端执行以下命令：  

    
    
    curl https://bootstrap.pypa.io/ez_setup.py -o - | sudo python 

1.2 Linux系统可以执行以下命令：  

    
    
    wget https://bootstrap.pypa.io/ez_setup.py -O - | sudo python 

1.3 Window系统:  

Window系统可以直接下载[ez_setup.py](https://bootstrap.pypa.io/ez_setup.py)文件并运行

  

# 2、BeautifulSoup4

  

BeautifulSoup4是一个Python解析html或者xml的工具模块，使用这个模块做Python爬虫也是很不错的。

安装方法：

2.1 Debain或Ubuntu可以通过系统软件包管理安装  

    
    
    $sudo apt-get install Python-bs4 

2.2 使用easy_install或者pip安装：  

    
    
    $ sudo easy_install beautifulsoup4 
    或
    $ sudo pip install beautifulsoup4

另外附上BeautifulSoup4的中文文档<http://www.crummy.com/software/BeautifulSoup/bs4/doc/index.zh.html>  

还有自己写的Python图片爬虫：<https://github.com/qinxiandiqi/Sexy> 或
<http://git.oschina.net/qinxiandiqi/Sexy>[  
](http://git.oschina.net/qinxiandiqi/Sexy)

  

  

