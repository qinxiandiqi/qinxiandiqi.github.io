---
# type: posts 
title: "Java语言使用注解处理器生成代码 —— 第一部分：注解类型"
date: 2015-10-09T14:05:19+0800
authors: ["Jianan"]
summary: "从本文开始，我将开始写一系列关于Java语言使用注解处理器生成代码的文章，包括这种方法的强大之处。最后还会描述如何确保在编译的时候使用这种方法生成源代码。在这系列文章中，我们将会：
介绍Java语言的注解。
了解注解的常用方式以及使用范围。
了解注解处理器以及它们所代表的角色。
学习如何创建注解处理器。
学习如何在终端命令行、Eclipse和Maven中运行注解处理器。
学习如何使用注解处理器生成源"
series: ["Java"]
categories: ["翻译", "Java"]
tags: ["java", "注解", "Annotation"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 3465
comment_num: 0
---


> 原文作者：deors
原文地址：[https://deors.wordpress.com/2011/09/26/annotation-types/](https://deors.wordpress.com/2011/09/26/annotation-types/)
译文作者：Jianan - qinxiandiqi@foxmail.com
版本信息：本文基于2015-10-09版本进行翻译
版权声明：经原作者许可进行翻译，保留所有权利，未经允许不得复制和转载。

<br>
从本文开始，我将开始写一系列关于Java语言使用注解处理器生成代码的文章，包括这种方法的强大之处。最后还会描述如何确保在编译的时候使用这种方法生成源代码。

在这系列文章中，我们将会：

* 介绍Java语言的注解。
* 了解注解的常用方式以及使用范围。
* 了解注解处理器以及它们所代表的角色。
* 学习如何创建注解处理器。
* 学习如何在终端命令行、Eclipse和Maven中运行注解处理器。
* 学习如何使用注解处理器生成源代码。
* 学习如何使用像Apache Velocity这样的外部模板引擎来使用注解处理器生成源代码。  

<br>
# **序** 
---
Java的注解从第三版的Java语言规范[(1)][1]中开始引入，并在Java 5中开始支持。

通过注解，我们可以为源代码添加元数据信息，包括编译或部署信息、配置属性、编译行为或者代码质量检查。

与Javadoc不同，注解属于强类型，用到的注解在类路径下都能找到对应的注解类型定义。因此，定义的注解可以在运行时访问到，而Javadoc是不可能的。

<br>
# **注解语法**
---
注解通常出现在被注解的代码段前面，通常情况下都是独立成行并且与代码段同样缩进。

注解可以添加到package（包）、types（类型，包括类、接口、枚举和其它注解类型）、variable（变量、包括class、类实例、局部变量——包括定义在for或者while循环里面的局部变量）、constructions（构造方法）、methods（方法）和parameters（参数）。

最简单的注解结构就是内部什么元素都没有，例如：

```java
@Override()
public void theMethod() {…}
```

在这个例子中，括号可以省略（因为没有参数）：

```java
@Override
public void theMethod() {…}
```

注解也可以包含元素，这些元素是通过逗号分隔的键值对。允许使用的值类型包括java原生数据类型、String类型、枚举类型，以及这些类型的数组形式：

```java
@Author(name = "Albert",
        created = "17/09/2010",
        revision = 3,
        reviewers = {"George", "Fred"})
public class SimpleAnnotationsTest {…}
```

当注解内部只有一个元素，并且这个元素名为value的时候，元素名可以省略：

```java
@WorkProduct("WP00000182")
@Complexity(ComplexityLevel.VERY_SIMPLE)
public class SimpleAnnotationsTest {…}
```

注解可以为内部一些元素或者全部元素都设置默认值。在使用这类注解的时候，具备默认值的元素可以省略。

例如，假设有个注解为Author，它为内部元素revision（默认为1）以及reviewers（默认为空String数组）设置了默认值，下面使用这个注解的两种方式是等效的：

```java
@Author(name = "Albert",
        created = "17/09/2010",
        revision = 1,
        reviewers = {})
public class SimpleAnnotationsTest() {…}
@Author(name = "Albert",        // defaults are revision 1
        created = "17/09/2010") // and no reviewers
public class SimpleAnnotationsTest() {…}
```

<br>
# **注解的典型使用**
---
Java语言规范中定义了三种注解类型——它们都用于Java编译器：

* **@Deprecated：** 用来声明被注解的元素不应该再使用。每当这个注解被使用的时候，编译器就会生成一个警告。它应该与Javadoc的@deprecated一起使用，在Javadoc中解释舍弃被注解元素的原因。

* **@Override：** 用来声明被注解的元素重写了父类中声明的元素。如果被注解的元素没有在父类中找到对应的被重写的元素，编译器就会生成一个警告。尽管这个注解没有要求一定要使用，但是它对于错误判断非常有用——例如，如果在创建父类之后，有人又修改了父类方法的签名，那么我们在重新编译源代码的时候就能马上得到提醒。

* **@SuppressWarnings：** 让编译器抑制指定的警告类型，否则被注解的元素就会触发这类警告——例如，为了避免使用了被舍弃的API或者与旧代码（例如Java 5之前的版本）进行了未检查的交互而产生过多的编译器“干扰”。

自从引入这些注解后，很多库和框架都将注解集成到它们新的发行版本中。通过在源代码中使用注解，这些库和框架都简化甚至移除了所需要的配置文件。

下面是一些成功使用注解的例子：

* Java企业版和它的主要模块——企业级JavaBeans、Java持久化API或者Web Service API。
* Spring框架——用于核心模块中的配置、依赖注解和控制反转中，以及其它一些Spring项目。
* Sean、Weld、Guice.
* Apache Struts 2.  

<br>
# **注解类型**
---
在Java语言中注解类型是一种特殊的接口，用来自定义注解。

注解的定义要使用**@interface**关键字来代替**interface**：

```java
public @interface Author {
    String name();
    String created();
    int revision() default 1;
    String[] reviewers() default {};
}
public @interface Complexity {
    ComplexityLevel value() default ComplexityLevel.MEDIUM;
}
public enum ComplexityLevel {
    VERY_SIMPLE, SIMPLE, MEDIUM, COMPLEX, VERY_COMPLEX;
}
```

注解类型与普通的接口有一些区别：

* 只允许原生数据类型、String、枚举、class，以及这些类型的数组。注意注解类型中不允许常用的对象，也不允许数组的数组（因为每一个数组都是一个对象）。

* 注解元素的定义与定义方法的语法很类似，但是不允许有修饰符和参数。

* 注解元素的默认值定义需要使用**default**关键字，后面连接字面量、初始化数组或者枚举值。

正如其它的类或者接口，枚举类型也可以嵌套在注解类型的内部：

```java
public @interface Complexity {
    public enum Level {
        VERY_SIMPLE, SIMPLE, MEDIUM, COMPLEX, VERY_COMPLEX;
    }
    …
}
```

<br>
# **用于定义注解的注解**
---
JDK自带了一些注解，用于修改我们自定义的注解的默认行为。

* **@Documented：** 声明被@Documented注解后的注解类型，一旦使用，被注解的元素就应该连同注解都记录到Javadoc中。

* **@Inherited：** 声明被注解的注解类型能够继承到它的子类上。通过这种方法，如果当前子类没有受@Inherited注解的注解类型注解过，但是它的父类有，那么当前子类也能通过继承得到这个注解类型。不过它只适用于类继承，不适用于接口实现。

* **@Retention：** 声明被注解的注解类型生命周期。可选的值都包含在RetentionPolicy枚举中：**CLASS**（默认值——能被包含到class文件中，但是在运行时访问不到），**SOURCE**（创建class文件的时候会被编译器丢弃），**RUNTIME**（能够在运行过程中访问到）。

* **@Target：** 声明注解类型能够注解的元素类型。可选的值包含在ElementType枚举中：**ANNOTATION_TYPE**（用于注解类型），**CONSTRUCTOR**（用于构造方法），**FIELE**（用于成员变量），**LOCAL_VARIABLE**（用于局部变量），**METHOD**（用于方法），**PACKAGE**（用于包），**PARAMETER**（用于方法参数），**TYPE**（用于类、接口、枚举）。

***
本系列将会在第二部分中继续讲解：注解处理器。请阅读[这里](http://blog.csdn.net/qinxiandiqi/article/details/49182735).

[1]:http://java.sun.com/docs/books/jls/

  
