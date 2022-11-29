---
# type: posts 
title: "kotlin的属性初始器与属性Setter"
date: 2021-07-18T17:01:51+0800
authors: ["Jianan"]
summary: "一、完整的Kotlin属性声明
var|val &lt;propertyName&gt;[: &lt;PropertyType&gt;] [= &lt;property_initializer&gt;]
    [&lt;getter&gt;]
    [&lt;setter&gt;]

一个kotlin属性声明可分为必选部分：属性关键字、属性名、属性类型；以及可选部分：属性初始器（property_initializer）、Getter、Setter。
注意：val没有Setter。
二、问题：在属性初始"
series: ["Kotlin"]
categories: ["Kotlin"]
tags: ["kotlin", "Setter", "属性初始器"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 152
comment_num: 2
---

## 一、完整的Kotlin属性声明
```
var|val <propertyName>[: <PropertyType>] [= <property_initializer>]
    [<getter>]
    [<setter>]
```
一个kotlin属性声明可分为必选部分：属性关键字、属性名、属性类型；以及可选部分：属性初始器（property_initializer）、Getter、Setter。
**注意：val没有Setter。** 
## 二、问题：在属性初始器中为属性赋值会使用属性的Setter吗？

答案：不会。

例如以下代码：

```
package com.molidt.kotlin.demo

fun main() {
    val obj = Obj()
    println("obj.x = ${obj.x}")
    obj.x = 2F
    println("obj.x = ${obj.x}")
    obj.x = 3F
    println("obj.x = ${obj.x}")
}

class Obj {
    var x: Float = 1F
        set(value) {
            field = value
            println("set value:$value")
        }
}
```

结果输出：
```shell
obj.x = 1.0  
set value:2.0  
obj.x = 2.0  
set value:3.0  
obj.x = 3.0
```  

## 三、需要注意的使用场景

### 1）使用类构造器参数作为属性初始器不会使用属性Setter

例如：
```
package com.molidt.kotlin.demo

fun main() {
    val obj = Obj(1F)
    println("obj.x = ${obj.x}")
    obj.x = 2F
    println("obj.x = ${obj.x}")
    obj.x = 3F
    println("obj.x = ${obj.x}")
}

class Obj(x: Float) {
    var x: Float = x
        set(value) {
            field = value
            println("set value:$value")
        }
}
```
结果输出：

```shell
obj.x = 1.0  
set value:2.0  
obj.x = 2.0  
set value:3.0  
obj.x = 3.0
```

### 2）在类构造器中为属性赋值会使用属性的Setter

例如：
```
package com.molidt.kotlin.demo

fun main() {
    val obj = Obj(1F)
    println("obj.x = ${obj.x}")
    obj.x = 2F
    println("obj.x = ${obj.x}")
    obj.x = 3F
    println("obj.x = ${obj.x}")
}

class Obj {

    constructor(x: Float) {
        this.x = x
    }

    var x: Float
        set(value) {
            field = value
            println("set value:$value")
        }
}
```
结果输出：

```shell
set value:1.0  
obj.x = 1.0  
set value:2.0  
obj.x = 2.0  
set value:3.0  
obj.x = 3.0  
```
 
 __特别要注意：方式2的写法，IDE会提示将constructor简化成方式1的写法，实际上两者的执行过程并不完全相同。__ 

