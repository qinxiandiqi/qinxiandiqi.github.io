---
# type: posts 
title: "Windows平台下git报错Filename too long"
date: 2022-09-18T13:12:17+08:00
authors: ["Jianan"]
summary: "Windows平台下git报错Filename too long"
series: []
categories: ["Git"]
tags: ["git", "Windows"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read: 
---

在Windows平台下进行git操作可能会出现**Filename too long**错误，这是因为操作的文件路径或文件名超长，配置git支持长文件名可解决问题：
```shell
git config --global core.longpaths true
```
