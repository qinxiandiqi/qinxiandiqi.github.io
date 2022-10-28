---
# type: posts 
title: "Java语言使用注解处理器生成代码——第三部分：生成源代码"
date: 2015-10-24T19:50:39+0800
authors: ["Jianan"]
summary: "本文是我的“使用Java语言注解处理器生成代码”系列第三部分，也是最后一部分。在第一部分中（请阅读这里），我们介绍了什么是Java语言的注解，以及一些常用的方法。在第二部分中（请阅读这里），我们介绍了注解处理器，以及注解处理器如何创建和运行。

现在，在第三部分中，我们要学习如何使用注解处理器来生成源代码。"
series: ["Java"]
categories: ["翻译", "Java"]
tags: ["java", "注解处理器", "源代码", "自动生成"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 3343
comment_num: 2
---

> 原文作者：deors  
原文地址：[https://deors.wordpress.com/2011/10/31/annotation-generators/](https://deors.wordpress.com/2011/10/31/annotation-generators/)  
译文作者：Jianan - qinxiandiqi@foxmail.com  
版本信息：本文基于2015-10-12版本进行翻译  
版权声明：本文经原文作者许可进行翻译，保留所有权利，未经允许不得复制和转载

<br>

本文是我的“使用Java语言注解处理器生成代码”系列第三部分，也是最后一部分。在第一部分中（请阅读[这里](http://blog.csdn.net/qinxiandiqi/article/details/48999291)），我们介绍了什么是Java语言的注解，以及一些常用的方法。在第二部分中（请阅读[这里](http://blog.csdn.net/qinxiandiqi/article/details/49182735)），我们介绍了注解处理器，以及注解处理器如何创建和运行。

现在，在第三部分中，我们要学习如何使用注解处理器来生成源代码。

<br>
# **序**
---
生成源代码很容易，但是生成正确的源代码就不容易了。要使用优雅高效的方法来生成源代码将是一个繁重的任务。

幸运的是从去年开始，MDE[1]（Model-Driven Engineering，也就是模型驱动工程设计，有时候也称为模型驱动开发或者模型驱动架构）已经有助于实践这个目标。这种设计更多的是倾向于艺术层面而不是科学——它针对的是经验丰富的程序员（译注：原文此句为task for ninja coders，国外有把经验丰富的程序员比作忍者的习惯）——是基于经过验证的流程和工具所提取出来的成熟方法论。

尽管我们可以认为生成源代码是MDE方法论的一个天然切入点，但是MDE涵盖的范围远远不止这些。

注解处理器只是众多我们用来生成源代码工具中的其中一种而已。

<br>
# **MDE中的Model和Meta-model**
---
在开始讲解如何使用注解处理器生成源代码相关细节之前，这里有几个我们需要先了解的概念，因为在接下来的章节中我们将会使用这些概念：model（模型）和meta-model（元模型）。

MDE的一个重要支柱就是它抽象的结构。我们将想要创建的软件系统在不同的细节层面使用不同的方法进行建模。当对一个抽象层建模之后，我们就可以对下一个和再下一个层面继续建模，直到一个可部署的产品被完整地建立起来。

从这个角度来看，无论我们使用的是哪一个细节层面，一个模型（覆盖的范围）都不会超过对应用来代表系统的抽象层。

元模型（meta-model），就是我们用来定义模型的规则。你可以认为它是模型的schema或者语法。

<br>
# **通过注解处理器生成源代码**
---
从一开始讨论到现在，注解处理器无疑是一种定义元模型和创建模型的优秀方法。注解类型扮演的是元模型角色，而一段代码块中所有注解的集合扮演的则是一个模型的角色。

我们可以利用模型来生成配置文件或者从一个已存在的源文件中派生出一个新的源文件。例如，创建一个远程代理，或者为被注解的bean创建一个可访问内部数据的入口对象。

这种方法的核心在于注解处理器。一个处理器能够读取源代码中的所有注解——也就是提取模型，并且通过它能够做任何我们想要做的事情——打开文件并添加内容等。Java编译器会处理好模型验证的问题（注解必须匹配在注解处理器中注册的类型）。

<br>
# **Filer**
---
在本系列的第二部分中曾经提到，每一个处理器都能够获取到一个processing environment对象，通过它能够获取到一些有趣的工具类对象。其中一个就是Filer。

在**javax.annotation.processing.Filer**[2]接口中定义了一些创建源文件、class文件或者生成资源的方法。通过使用Filer，我们可以确保使用了正确的文件目录，以避免丢失文件系统中生成的一些重要数据。

（另外，）我们需要关注的重点：一方面是考虑是否要写一个在javac上附加-d或者-s选项的生成器，另一方面就是Maven POM中定义文件夹。

下面是一个如何在注解处理器中创建Java源文件的例子。正如我们创建了一个Bean信息类，生成的类名与被注解的类名相同，只是在它后面添加上了“BeanInfo”后缀：

```java
if (e.getKind() == ElementKind.CLASS) {
    TypeElement classElement = (TypeElement) e;
    PackageElement packageElement =
        (PackageElement) classElement.getEnclosingElement();

    JavaFileObject jfo = processingEnv.getFiler().createSourceFile(
        classElement.getQualifiedName() + "BeanInfo");

    BufferedWriter bw = new BufferedWriter(jfo.openWriter());
    bw.append("package ");
    bw.append(packageElement.getQualifiedName());
    bw.append(";");
    bw.newLine();
    bw.newLine();
    // rest of generated class contents
  }
```

<br>
# **不要像我这样生成代码**
---
上面的例子非常简单有趣，但是很糟糕。

我们将从注解中获取所需信息（代表模型）的逻辑与生成文件（代表视图，译注：指的是MVC设计模式中的V层）的逻辑混合在一起。

使用这种方法很难写出一个像样的生成器。如果我们需要在这个过程加入更复杂的东西，那么这个过程就会变得非常繁杂，并且容易出错，也很难维护。

因此，我们需要一种更加优雅的方式：

* 将模型从视图中清晰的分离出来。
* 使用模板来减轻生成文件的任务压力。

让我们来看一个使用这种方式的例子：如何利用**Apache Velocity**来生成我们想要的生成器。

<br>
# **Velocity的历史简介**
---
Velocity，Apache软件基金会的一个项目，是一个用Java写的模板引擎，用于将模板和从Java对象中获取的数据进行混合生成各种文字类型的文件。

Velocity经常在当下流行的MVC模式中被用来渲染视图，或者在XML文件中作为XSLT的替代品进行数据转换。

Velocity拥有自己的语言，也就是Velocity Template Language（VLT），它是生成简单易读模板的关键。使用VLT，我们可以简单且直观地定义变量，控制流程和迭代，以及访问Java对象中包含的信息。

下面是一段Velocity模板片段：

```java
#foreach($field in $fields)
    /**
     * Returns the ${field.simpleName} property descriptor.
     *
     * @return the property descriptor
     */
    public PropertyDescriptor ${field.simpleName}PropertyDescriptor() {
        PropertyDescriptor theDescriptor = null;
        return theDescriptor;
    }
#end
#foreach($method in $methods)
    /**
     * Returns the ${method.simpleName}() method descriptor.
     *
     * @return the method descriptor
     */
    public MethodDescriptor ${method.simpleName}MethodDescriptor() {
        MethodDescriptor descriptor = null;
        return descriptor;
    }
#end
```

<br>
# **Velocity生成器使用方法**
---
现在我们决定使用Velocity来升级我们的生成器，我们需要按照下面的步骤进行重新设计：

* 写一个用来生成代码的模板。
* 注解处理器从每一轮的environment中读取被注解的元素并将它们保存到容易访问的Java对象中——包括一个保存field的map对象，一个保存method的map对象，类名和包名等等。
* 注解处理器实例化Velocity的context。
* 注解处理器加载Velocity的模板。
* 注解处理器创建源文件（通过使用Filer），并且连同Velocity Context将一个写入器（writer）传递给Velocity的模板。
* Velocity引擎生成源代码。

通过使用这种方法，你会发现处理器/生成器的代码非常清晰，结构良好，并且易于理解和维护。

下面让我们一步一步来实现：

## **步骤1：写模板**

为了简单起见，我们不会列出完整的BeanInfo生成器代码，只是列出部分与注解处理器一块编译时需要的field（成员变量）和method（方法）。

接下来让我们创建一个名为**beaninfo.vm**的（模板）文件，并把它放到包含注解处理器的Maven artifact项目的**src/main/resources**目录下。模板内容的示例如下：

```java
package ${packageName};

import java.beans.MethodDescriptor;
import java.beans.ParameterDescriptor;
import java.beans.PropertyDescriptor;
import java.lang.reflect.Method;

public class ${className}BeanInfo
    extends java.beans.SimpleBeanInfo {

    /**
     * Gets the bean class object.
     *
     * @return the bean class
     */
    public static Class getBeanClass() {

        return ${packageName}.${className}.class;
    }

    /**
     * Gets the bean class name.
     *
     * @return the bean class name
     */
    public static String getBeanClassName() {

        return "${packageName}.${className}";
    }

    /**
     * Finds the right method by comparing name & number of parameters in the class
     * method list.
     *
     * @param classObject the class object
     * @param methodName the method name
     * @param parameterCount the number of parameters
     *
     * @return the method if found, <code>null</code> otherwise
     */
    public static Method findMethod(Class classObject, String methodName, int parameterCount) {

        try {
            // since this method attempts to find a method by getting all
            // methods from the class, this method should only be called if
            // getMethod cannot find the method
            Method[] methods = classObject.getMethods();
            for (Method method : methods) {
                if (method.getParameterTypes().length == parameterCount
                    && method.getName().equals(methodName)) {
                    return method;
                }
            }
        } catch (Throwable t) {
            return null;
        }
        return null;
    }
#foreach($field in $fields)

    /**
     * Returns the ${field.simpleName} property descriptor.
     *
     * @return the property descriptor
     */
    public PropertyDescriptor ${field.simpleName}PropertyDescriptor() {

        PropertyDescriptor theDescriptor = null;
        return theDescriptor;
    }
#end
#foreach($method in $methods)

    /**
     * Returns the ${method.simpleName}() method descriptor.
     *
     * @return the method descriptor
     */
    public MethodDescriptor ${method.simpleName}MethodDescriptor() {

        MethodDescriptor descriptor = null;

        Method method = null;
        try {
            // finds the method using getMethod with parameter types
            // TODO parameterize parameter types
            Class[] parameterTypes = {java.beans.PropertyChangeListener.class};
            method = getBeanClass().getMethod("${method.simpleName}", parameterTypes);

        } catch (Throwable t) {
            // alternative: use findMethod
            // TODO parameterize number of parameters
            method = findMethod(getBeanClass(), "${method.simpleName}", 1);
        }

        try {
            // creates the method descriptor with parameter descriptors
            // TODO parameterize parameter descriptors
            ParameterDescriptor parameterDescriptor1 = new ParameterDescriptor();
            parameterDescriptor1.setName("listener");
            parameterDescriptor1.setDisplayName("listener");
            ParameterDescriptor[] parameterDescriptors = {parameterDescriptor1};
            descriptor = new MethodDescriptor(method, parameterDescriptors);

        } catch (Throwable t) {
            // alternative: create a plain method descriptor
            descriptor = new MethodDescriptor(method);
        }

        // TODO parameterize descriptor properties
        descriptor.setDisplayName("${method.simpleName}(java.beans.PropertyChangeListener)");
        descriptor.setShortDescription("Adds a property change listener.");
        descriptor.setExpert(false);
        descriptor.setHidden(false);
        descriptor.setValue("preferred", false);

        return descriptor;
    }
#end
}
```

注意在这个模板运作之前，我们需要将以下信息传递给Velocity：

* packageName：生成类的完整包名。
* className：生成类的类名。
* fields：源类中包含的filed的集合。我们需要从每个field中获取以下信息：
  * simpleName：filed的变量名。
  * type：filed的类型。
  * description：filed的自我描述（在本例中没有使用）
  * ……
* methods：源类中包含的method的集合。我们需要从每个method中获取以下信息：
  * simpleName：method的方法名。
  * arguments：method的参数（在本例中没有使用）
  * returnType：method的返回类型（在本例中没有使用）
  * description：method的自我描述（在本例中没有使用）
  * ……

所有的这些信息（也就是模型）都需要从源类里面匹配的注解中提取，并保存到JavaBean后传递给Velocity。

## **步骤2：注解处理器读取Model**

下面让我们创建一个注解处理器。正如本系列第二部分中所提到的，不要忘记给处理器添加注解，好让它能够处理BeanInfo注解类型：

```java
@SupportedAnnotationTypes("example.annotations.beaninfo.BeanInfo")
@SupportedSourceVersion(SourceVersion.RELEASE_6)
public class BeanInfoProcessor
    extends AbstractProcessor {
      ...
}
```

注解处理器的方法需要从注解和源类本身中提取构建模型所需要的信息。你可以将全部需要的信息都保存到JavaBean里面，不过在这个示例中我们使用的是**javax.lang.model.element**类型，因为我们不打算传递太多细节给Velocity（当然，需要的数据还是会传递过去的，在这个例子中我们要创建的是一个完整的BeanInfo生成器）：

```java
String fqClassName = null;
String className = null;
String packageName = null;
Map<String, VariableElement> fields = new HashMap<String, VariableElement>();
Map<String, ExecutableElement> methods = new HashMap<String, ExecutableElement>();

for (Element e : roundEnv.getElementsAnnotatedWith(BeanInfo.class)) {

    if (e.getKind() == ElementKind.CLASS) {

        TypeElement classElement = (TypeElement) e;
        PackageElement packageElement = (PackageElement) classElement.getEnclosingElement();

        processingEnv.getMessager().printMessage(
            Diagnostic.Kind.NOTE,
            "annotated class: " + classElement.getQualifiedName(), e);

        fqClassName = classElement.getQualifiedName().toString();
        className = classElement.getSimpleName().toString();
        packageName = packageElement.getQualifiedName().toString();

    } else if (e.getKind() == ElementKind.FIELD) {

        VariableElement varElement = (VariableElement) e;

        processingEnv.getMessager().printMessage(
            Diagnostic.Kind.NOTE,
            "annotated field: " + varElement.getSimpleName(), e);

        fields.put(varElement.getSimpleName().toString(), varElement);

    } else if (e.getKind() == ElementKind.METHOD) {

        ExecutableElement exeElement = (ExecutableElement) e;

        processingEnv.getMessager().printMessage(
            Diagnostic.Kind.NOTE,
            "annotated method: " + exeElement.getSimpleName(), e);

        methods.put(exeElement.getSimpleName().toString(), exeElement);
    }
}
```

## **步骤3：初始化Velocity Context并加载模板**

下面的代码片段展示了如何初始化Velocity Context并加载模板：

```java
if (fqClassName != null) {

    Properties props = new Properties();
    URL url = this.getClass().getClassLoader().getResource("velocity.properties");
    props.load(url.openStream());

    VelocityEngine ve = new VelocityEngine(props);
    ve.init();

    VelocityContext vc = new VelocityContext();

    vc.put("className", className);
    vc.put("packageName", packageName);
    vc.put("fields", fields);
    vc.put("methods", methods);

    Template vt = ve.getTemplate("beaninfo.vm");
    ...
}
```

Velocity配置文件，在本示例中名为**velocity.properties**，它应该被放置到**src/main/resources**目录下。下面是它的内容示例：

```java
runtime.log.logsystem.class = org.apache.velocity.runtime.log.SystemLogChute

resource.loader = classpath
classpath.resource.loader.class = org.apache.velocity.runtime.resource.loader.ClasspathResourceLoader
```

这组属性配置了Velocity的日志，以及一个用于查找模板的基本资源加载路径。

## **步骤4：创建新的源文件并生成源代码**

紧接着，让我们创建新的源文件，并将这个新文件作为模板的目标来运行模板。下面的代码片段展示了如何操作：

```java
JavaFileObject jfo = processingEnv.getFiler().createSourceFile(
    fqClassName + "BeanInfo");

processingEnv.getMessager().printMessage(
    Diagnostic.Kind.NOTE,
    "creating source file: " + jfo.toUri());

Writer writer = jfo.openWriter();

processingEnv.getMessager().printMessage(
    Diagnostic.Kind.NOTE,
    "applying velocity template: " + vt.getName());

vt.merge(vc, writer);

writer.close();
```

## **步骤5：打包并运行**

最后，注册注解处理器（记得添加本系列第二部分中提到过的service配置文件），然后打包。再通过终端命令行，Eclipse或者Maven工具在client（客户）项目中进行调用和编译。

假设client项目中的client类如下：

```java
package example.velocity.client;
import example.annotations.beaninfo.BeanInfo;
@BeanInfo public class Article {
    @BeanInfo private String id;
    @BeanInfo private int department;
    @BeanInfo private String status;
    public Article() {
        super();
    }
    public String getId() {
        return id;
    }
    public void setId(String id) {
        this.id = id;
    }
    public int getDepartment() {
        return department;
    }
    public void setDepartment(int department) {
        this.department = department;
    }
    public String getStatus() {
        return status;
    }
    public void setStatus(String status) {
        this.status = status;
    }
    @BeanInfo public void activate() {
        setStatus("active");
    }
    @BeanInfo public void deactivate() {
        setStatus("inactive");
    }
}
```

当我们在终端上执行**javac**命令后，我们可以在控制台上看到找到的被注解元素，以及生成的BeanInfo类：

```ruby
Article.java:6: Note: annotated class: example.annotations.velocity.client.Article
public class Article {
       ^
Article.java:9: Note: annotated field: id
    private String id;
                   ^
Article.java:12: Note: annotated field: department
    private int department;
                ^
Article.java:15: Note: annotated field: status
    private String status;
                   ^
Article.java:53: Note: annotated method: activate
    public void activate() {
                ^
Article.java:59: Note: annotated method: deactivate
    public void deactivate() {
                ^
Note: creating source file: file:/c:/projects/example.annotations.velocity.client/src/main/java/example/annotations/velocity/client/ArticleBeanInfo.java
Note: applying velocity template: beaninfo.vm
Note: example\annotations\velocity\client\ArticleBeanInfo.java uses unchecked or unsafe operations.
Note: Recompile with -Xlint:unchecked for details.
```

如果我们检查下源代码目录，我们将会找到我们生成的BeanInfo类。任务完成！

<br>
# **总结**
---
通过本系列文章，我们学习了如何利用Java 6注解处理器框架生成源代码的基础知识：

* 我们学习了什么是注解和注解类型，以及它们的常用方式。
* 我们学习了什么是注解处理器，如何写注解处理器，以及如何使用不同的工具运行注解处理器——Java编译器、Eclipse或者Maven。
* 我们讨论了一点关于模型驱动设计与代码生成的技术。
* 我们介绍了如何使用注解处理器创建能够与Java编译器完全交互的源代码生成器。
* 我们介绍了如何利用现有的生成框架（像Apache Velocity）基于注解处理器来创建优雅强大且易于维护的源代码生成器。

现在，是时候将这些内容应用到你的项目中了。思考一下生成技术！



---
[1]：如果你想要了解更多关于MDE的内容，请参考维基百科的这篇[文章](http://en.wikipedia.org/wiki/Model-driven_engineering)以及它的参考文献。  
[2]：Filer的API文档可以在[这里](http://download.oracle.com/javase/6/docs/api/javax/annotation/processing/Filer.html)在线查看。

<br>
> **译附：**   
译者根据本文整理并修改的maven demo项目：  
Github地址：[https://github.com/qinxiandiqi/AnnotationProcessorDemo](https://github.com/qinxiandiqi/AnnotationProcessorDemo)  
Tag标签：**part3**（执行*git checkout part3*命令检出）