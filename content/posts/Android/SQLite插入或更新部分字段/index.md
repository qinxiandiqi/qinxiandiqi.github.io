---
# type: posts 
title: SQLite插入或更新部分字段
date: 2022-12-18T22:28:11+08:00
authors: ["Jianan"]
summary: "如何在SQLite中插入或更新表中的部分字段呢？"
series: ["SQLite"]
categories: ["SQLite", "Sql"]
tags: ["SQLite", "Sql"]
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

## 业务场景
在业务开发中可能会遇到这种场景：在SQLite数据库表中有多个字段，现在想要插入一条新记录，这条新记录没有包含表中全部字段的数据。要求如果表中没有这条记录就直接插入，如果表中已经有相同主键的记录就更新这条记录，新记录没有提供数据的字段要保持不变。  

## 怎么处理？
看着很简单是吧？但要怎么做呢？  
为了让业务场景更具象一点，我们举个例子：假设我们现在有个IM聊天会话列表的数据表tb_conversations，包含会话id（主键）、会话名name、未读消息数unread（默认值为0）、会话是否置顶is_sticky（默认值为0，0代表否，1代表是），共4个字段。  
先来创建这张表：
```sql
CREATE TABLE tb_conversations (
    id        INTEGER PRIMARY KEY,
    name      TEXT,
    unread    INTEGER DEFAULT 0,
    is_sticky INTEGER DEFAULT 0
);
```
现在我们跟老王有个会话，会话id是1，name是老王。我们想把老王这个会话置顶，SQL语句怎么写？用**UPDATE**吗？不，因为不知道表中有没有老王这条记录，如果当前表中老王这条记录不在的话，UPDATE不会起作用。聪明的你应该想到了，用INSERT OR REPLACE INTO：
```sql
INSERT OR REPLACE INTO tb_conversations(id, name, is_sticky) VALUES(1, "老王", 1);
```
因为当前表中没有老王这条记录，所以执行的结果是直接插入记录。因为sql语句中没有提供unread字段，所以插入的时候自动赋值为默认值0：  
![首次插入记录](%E9%A6%96%E6%AC%A1%E6%8F%92%E5%85%A5%E8%AE%B0%E5%BD%95.png)  

这个时候，老王发了一条消息过来，消息未读数变成1，表要怎么更新？同样的，因为不知道表中是否已经有老王这条记录（在实际业务中，你在不先查表得情况下，其实很难肯定在每次操作数据库的时候，表中是否已经有对应记录），所以还是INSERT OR REPLACE INTO：
```sql
INSERT OR REPLACE INTO tb_conversations(id, name, unread) VALUES(1, "老王", 1);
```
看看结果：  
![覆盖插入记录](%E8%A6%86%E7%9B%96%E6%8F%92%E5%85%A5%E8%AE%B0%E5%BD%95.png)  

可以看到unread字段已经替换成1。诶，等等，is_sticky字段怎么变成0了？明明第一次插入记录的时候把会话置顶了，is_sitkcy是1呀？这是因为当前表中已经有老王这条记录了，所以这次INSERT OR REPLACE INTO的执行效果是REPLACE。**REPLACE在替换的时候，对于没有提供数据的字段会直接使用这个字段的默认值。** is_sticky字段的默认值是0，所以REPLACE之后就变成0了。  
这可不是我们想要的效果，总不能我们把老王的会话置顶了，老王一消息过来，置顶就被取消了。


