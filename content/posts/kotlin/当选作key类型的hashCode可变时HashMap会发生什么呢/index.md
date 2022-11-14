---
# type: posts 
title: "当选作key类型的hashCode可变时，HashMap会发生什么呢?"
date: 2022-11-07T23:21:18+08:00
authors: ["Jianan"]
summary: "当选作key类型的hashCode可变时，HashMap会发生什么呢?"
series: ["Kotlin"]
categories: ["Kotlin", "Java"]
tags: ["Kotlin", "Java", "HashMap", "key"]
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

了解Java/Kotlin的都知道，HashMap的数据存取是由key的hashCode决定的。一般选作key的类型，它的hashCode都是不可变的，比如常用的基本数据类型或者字符串等。但，当key类型的hashCode是可变时，又会发生什么有趣的事情呢？  
来看下面这个有趣的例子：    

先定义一个hashCode可随类成员动态变化的类：
```kotlin
class Key(var name: String) {
    override fun equals(other: Any?): Boolean {
        if (this === other) return true
        if (javaClass != other?.javaClass) return false

        other as Key

        if (name != other.name) return false

        return true
    }

    override fun hashCode(): Int {
        return name.hashCode()
    }
}
```
这个Key类的hashCode是由其成员name的值决定。当name成员发生变化时，Key类型的hashCode也会变化。现在将这个类型作为HashMap的key类型，然后进行以下单元测试的数据存取测试：

```kotlin
@Test
fun testHashMapKey() {
    //定义HashMap容器
    val map = HashMap<Key, Any>()
    //创建初始键值对并存入HashMap
    val key = Key("before")
    val value = Any()
    map[key] = value
    
    //在键值对存入HashMap后，修改key对象的name值，key的hashCode发生变化
    key.name = "after"

    //变化后，虽然还是同个key对象，但已不能从HashMap中获取该key的value值了
    assertNull(map[key]) //True
    //然而，此时HashMap中还存储第一次存储的“key”和value，HashMap的size还是1
    assertEquals(map.size, 1) //True
    //取出当前HashMap中存储的唯一一对键值对，依然是先前存入的键值对，是同一对对象
    assertEquals(map.keys.first(), key) //True
    assertEquals(map.values.first(), value) //True

    //再次存储这对键值对，此时HashMap中有两个键值对，但实际上他们是同一组key-value对象
    map[key] = value
    assertEquals(map.size, 2) //True
}
```
该单元测试结果通过：  
![测试结果](HashMapTestResultPass.png)

可以看到，当HashMap的key类型hashCode可变时，会有以下后果：
1. 当key对象的hashCode改变后，无法以这个对象作为key获取先前已存入的value值，有极大的可能导致内存泄漏（该value对象在业务层面无感知无法获取，但被hashMap持有不释放）
2. HashMap的size长度非业务预期，在业务层面将不可信。例如上面例子，同个key对象修改前后多次存储导致size增长。

## 结论
在使用HashMap等Hash算法容器时，需要特别注意作为key类型的hashCode是否会变化。  
当key类型的hashCode可变时，要注意当key的hashCode发现变化后，对已存储的数值和后续存储规则是否产生影响，这个影响是否符合业务预期。
