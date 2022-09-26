---
# type: posts 
title: GLSL的uniform变量自动优化
date: 2022-09-26T23:17:53+08:00
authors: ["Jianan"]
summary: "glsl的uniform变量在未使用的情况下会被自动删除！"
series: ["opengl"]
categories: ["opengl"]
tags: ["glsl", "opengl"]
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

在opengl的着色器中，如果定义的uniform全局变量只是定义但并没有使用，或者使用了该uniform变量的代码不可触达或对着色器结果不产生影响，那么opengl在编译链接着色器程序之后，会自动将着色器中的这些uniform变量删除。  
这个优化可能会导致在业务代码中查询使用该uniform变量时，发生变量不能存在的错误，不了解这个自动优化机制容易让人费解。
例如：
```glsl
uniform vec4 u_color;
```
或者
```glsl
uniform vec4 u_color;

if (u_color.a >= 0.0) {}
```
以上的u_color变量将会被自动删除。
