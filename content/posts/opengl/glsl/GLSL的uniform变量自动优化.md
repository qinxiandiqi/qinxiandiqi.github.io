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

在opengl的着色器中，如果定义的uniform全局变量只是定义但并没有使用，或者使用了该uniform变量的代码不可触达，又或者对着色器结果不产生影响，那么opengl在编译链接着色器程序之后，会自动将着色器中的这些uniform变量删除。  
这个优化会导致在业务代码中查询使用该uniform变量时，发生变量不能存在的错误，不了解这个自动优化机制容易让人费解。  
## 举例说明  
1、以下顶点着色器的uniform变量u_color只是定义但从未使用过，编译后会被删除。
```glsl
attribute vec4 a_position;
attribute vec4 a_color;
attribute vec2 a_texCoord0;
uniform mat4 u_projTrans;
uniform vec4 u_color;  //只定义未使用
varying vec4 v_color;
varying vec2 v_texCoords;

void main() {
    v_color = a_color;
    v_color.a = v_color.a * (255.0/254.0);
    v_texCoords = a_texCoord0;
    gl_Position = u_projTrans * a_position;
}
```
2、以下顶点着色器的uniform变量u_color虽然有被使用，但它所处的位置永远不可被执行，编译后会被删除。
```glsl
attribute vec4 a_position;
attribute vec4 a_color;
attribute vec2 a_texCoord0;
uniform mat4 u_projTrans;
uniform vec4 u_color;
varying vec4 v_color;
varying vec2 v_texCoords;

void main() {
    //定义并使用，但所处的位置永远不可被执行到
    if (1.0 >= 2.0) {
        u_color.a = 1.0
    } 
    v_color = a_color;
    v_color.a = v_color.a * (255.0/254.0);
    v_texCoords = a_texCoord0;
    gl_Position = u_projTrans * a_position;
}

```
3、以下顶点着色器的uniform变量u_color虽然有被使用，所处的位置也会被执行到，但它的执行对计算结果不产生任何影响，编译后也会被删除。
```glsl
attribute vec4 a_position;
attribute vec4 a_color;
attribute vec2 a_texCoord0;
uniform mat4 u_projTrans;
uniform vec4 u_color;
varying vec4 v_color;
varying vec2 v_texCoords;

void main() {
    if (u_color.a >= 0.0) {} //定义并使用，但对计算结果不产生影响
    v_color = a_color;
    v_color.a = v_color.a * (255.0/254.0);
    v_texCoords = a_texCoord0;
    gl_Position = u_projTrans * a_position;
}

```

