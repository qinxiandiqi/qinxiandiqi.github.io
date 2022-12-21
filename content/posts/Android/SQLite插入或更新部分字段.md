---
# type: posts 
title: SQLite插入或更新部分字段
date: 2022-12-18T22:28:11+08:00
authors: ["Jianan"]
summary: 
series: []
categories: []
tags: []
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: true
read_num: 0
comment_num: 0 
---

一个问题：SQLite如何插入或更新一条记录呢？  
这很简单嘛，插入一条记录，如果表中该记录已经存在（主键已经存在）就更新这条记录，大笔一挥，写下：
```sql
INSERT OR REPLACE table_name(column...) VALUES(...)
```


