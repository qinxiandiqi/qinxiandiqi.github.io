---
# type: posts 
title: "解决Spring配置文件无法读取properties属性问题"
date: 2015-09-26T20:30:27+0800
authors: ["Jianan"]
summary: "在Spring项目的配置文件中引用properties属性文件中的属性，运行时无法识别properties属性文件中的属性引用，但properties属性文件和属性明明已经存在，例如：    要在Spring中使用外部properties属性文件，需要在Spring配置文件中添加bean后处理器PropertyPlaceholderConfigurer，并指明外部properties属性文件的路径："
series: ["spring"]
categories: ["spring"]
tags: ["spring", "properties", "配置文件"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 17294
comment_num: 0
---

在Spring项目的配置文件中引用properties属性文件中的属性，运行时无法识别properties属性文件中的属性引用，但properties属性文件和属性明明已经存在，例如：  

![这里写图片描述](363ef68e363163affe187f0ed6683e88.png)  

![这里写图片描述](50f139a161b141e180eede6ce9df7261.png)

要在Spring中使用外部properties属性文件，需要在Spring配置文件中添加bean后处理器**PropertyPlaceholderConfigurer**，并指明外部properties属性文件的路径：  

```xml
<bean class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
	<property name="locations" value="properties路径"/>
</bean>
```

如果有多个properties属性文件，可以使用下面方式：

```xml
<bean class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
    <property name="locations">
        <list>
            <value>properties路径</value>
            <value>properties路径</value>
            ...
        </list>
    </property>
</bean>
```


