---
# type: posts 
title: "逃离CSDN，一键打包CSDN博客"
date: 2022-10-31T16:32:23+08:00
authors: ["Jianan"]
summary: "逃离CSDN，一键打包CSDN博客"
series: []
categories: ["python"]
tags: ["python", "博客搬家"]
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

## 一、逃离CSDN？

在CSDN上面断断续续写了十年的文章，回想CSDN的名声这十几年从金字招牌演变到令人唾弃，如今我也决定逃离，不免令人遗憾。你要说CSDN做错了什么？可能也没什么大错，只不过是没跟上时代而已。在自媒体浪潮中，抓不住自己的定位，服务没跟上又想割韭菜，结果作者和读者双头得罪。随着越来越多的人出走CSDN，劣币又驱逐良币，再加上CSDN自己不断作死的一些骚操作，整体社区氛围越来越恶化并且恶性循环。一个品牌一旦臭了，无论做什么事情都事倍功半，要想再挽回可是非常艰难的事情了。  

实际上，我对CSDN还是很感激的，十年前的CSDN可是个大佬云集的地方。我曾经为了追逐大佬，也为了博客排名而奋笔疾书。也正因为在CSDN上坚持写了很多文章，不断逼迫自己研究了很多东西，从建立了自己在IT行业立足的自信。技术人的自信是很重要的，很多时候是自信决定了你在技术这条道上能走多远。从这点出发，我还是很感激CSDN的。

## 二、怎么逃离，CSDN搬家是个问题

既然决定逃离了，那么CSDN博客搬家是个问题。CSDN并没有提供文章批量导出之类的工具，放弃这么多年的文章很可惜，得想个办法把文章打包迁移出来。

结合在网上找到的一些文章，还有分析CSDN的网络请求，于是写了个python工具blog_packaging_tools。因为我是用hugo来部署自己的网站，所以这个工具的主要目的就是把CSDN上自己的文章下载下来，转换为markdown文件格式（包括下载文章中的图片），并按hugo文件组织的结构保存。  

目前项目已经开源，有需要的可以自取。项目中抽象了博客的类型和打包流程，不满足自己要求的话，也可以按自己的要求实现下Blog、Post、Packer等几个类，适当改造一下来满足自己的要求。 

项目地址：[https://github.com/qinxiandiqi/blog_packaging_tools](https://github.com/qinxiandiqi/blog_packaging_tools)  

## 三、blog_packaging_tools使用方式

1. clone项目到本地：  
    ```
    git clone https://github.com/qinxiandiqi/blog_packaging_tools.git
    ```
2. 确认本地已经安装python3环境，进入clone下来的blog_packaging_tools项目目录，使用pip/pip3安装项目以来python模块：
    ```
    pip install -r requirements.txt 
    ```
3. 复制项目根目录下`config.ini.sample`文件到根目录下，重命名为`config.ini`文件。
4. 填写`config.ini`配置文件下csdn相关参数。
    - blog_id: csdn博客的id
    - author：csdn博客作者名
    - cookie：csdn博客的cookie。cookie的获取方式：  
        使用chrome相关浏览器登录csdn后，按F12打开开发者工具。刷新自己的csdn博客主页，在开发者工具中找到自己博客主页的网络请求，在header头参数中找到cookie字段，右键菜单选择复制值，复制到`config.ini`配置文件的cookie字段（注意保留配置文件中的单引号）
        ![cookie](./cookie.png)
    - start_page: 扫描csdn文章开始分页页码，默认1，从第1页开始扫描
    - end_page: 扫描csdn文章结束分页页码，默认100。当没有下一页或到达指定结束分页时停止扫描。
5. `blogs/packer/hugo/template.md`文件为导出hugo markdown文章模板，需要修改导出文章模板的话可以修改这个文件，具体支持的参数可查看默认模板文件中的参数。
6. 执行打包：
    ```
    python3 main.py
    ```
7. 结果输出到根目录下的output目录

