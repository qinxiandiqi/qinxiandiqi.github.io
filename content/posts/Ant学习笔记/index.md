---
# type: posts 
title: "Ant学习笔记"
date: 2014-07-11T12:26:02+0800
authors: ["Jianan"]
summary: "1、Ant（another neat tool）是一个基于Java的生成工具，其作用类似于命令的批处理，通过设置一个xml文件后，Ant将会执行xml文件中指定的系列命令。这对于随着应用程序的生成过程变得更加复杂，又需要确保在每次生成期间都使用精确相同的生成步骤，同时实现尽可能多的自动化，以便于及时产生一致的生成版本非常重要。

2、Ant是基于命令行操作的。单纯使用ant命令时，Ant将会在"
series: ["Ant"]
categories: ["Ant"]
tags: ["ant", "java", "eclipse", "xml"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 1112
comment_num: 0
---

1、Ant（another neat
tool）是一个基于Java的生成工具，其作用类似于命令的批处理，通过设置一个xml文件后，Ant将会执行xml文件中指定的系列命令。这对于随着应用程序的生成过程变得更加复杂，又需要确保在每次生成期间都使用精确相同的生成步骤，同时实现尽可能多的自动化，以便于及时产生一致的生成版本非常重要。

  
2、Ant是基于命令行操作的。单纯使用ant命令时，Ant将会在使用命令的文件夹内寻找默认的build.xml配置文件执行预定义的系列命令操作，如果不存在build.xml文件则会提示Build
ailed生成失败。ant命令后也可直接显示指定xml文件，如ant -f hello.xml来显示使用hello.xml配置文件执行系列命令。

  
3、Ant最基本的xml文件格式：

    
    
    <?xml version="1.0" encoding="utf-8" ?> 
    <project default="defaName"> 
            <target name="defaName"> 
                    <mkdir dir="helloworld" /> 
                    <delete dir="helloworld" /> 
            </target> 
    </project>

    1）Ant的xml文件中根元素为project，一般都必须有属性default，其属性值为一个子元素target的属性name的属性值，表示该target为此project的默认执行目标。

    2）子元素target为一个Ant需要执行的命令目标，需要属性name来确定一个target，一个project中可以有多个target。每个target的子元素为需要执行的系列命令，如<mkdir dir=""/>创建一个文件夹，<delete dir=""/>删除一个文件夹。

  
4、使用Ant推荐将一次生成过程分割为多个target去完成以提高生成的灵活性。

  
5、Ant执行一个xml中的project只会执行project中属性名为project标签default值的target，比如project的default="first"，那么整个xml只会执行name值为“first”的target。如果要连续执行多个target的话，需要在target添加属性depends，其属性值为另一个target的name值，这么一来当Ant执行这个target时就会先执行其depends指定的另一个target。

  
6、使用ant命令也可以显式指定需要执行的target。在ant命令最后添加上需要执行的target的name值（多个target使用空格隔开），Ant就会依次执行指定的每一个target（此时不再自动执行默认的target）.

  
7、Ant的xml其它标签和属性：

    1）<description></description>其中包含的标签内容为注释内容。

    2）project可带属性name，表示该project的名称。

    3）Ant的xml标签可带属性description，属性值为该标签的说明。

  

8、Ant中的属性类似于编程语言中的属性，但是Ant中的属性赋值后就不能再改变，因为Ant只是一个生成工具。

  

9、Ant的属性一旦被设定，值就不能再改变。  
    1）定义Ant属性的方法：<property name="hello" value="world"/>  
    2）使用Ant属性的方法：${hell0}，中括号内为属性名，整个表达式为属性值。该表达式可以插入到任何使用值的地方。  
    3）location属性：其值用于表示一个路径字符串，特点是字符串中的分隔符"/"或"\"会自动根据不同的操作系统平台自动选用相应的分隔符。  
  
10、Ant的xml中target的属性depends值表示需要先执行的其它target的name，如果有多个target需要先执行，则depends值的多个target的name值需要使用","来分隔。  
  
11、Eclipse中集成了Ant工具，在项目中创建build.xml文档，Eclipse将会自动识别为Ant配置文档，可以使用ant执行并在控制台输出执行产生的信息。如果Eclipse没有识别出build.xml，则需要在window-
preferences-General-Editors-File Associations中创建build.xml文档类型，并为其选择默认编辑器为Ant
Editor。  
  
12、Ant执行后生成的文档一般在xml配置文档所在的目录中，也可以显示指定生成文档目录，在xml文档的project标签中添加属性basedir，其值为需要指定的文档目录路径（当值为"."时表示当前目录）。  
  
13、Eclipse提供了Ant视图。  
  
14、ant命令的另一个选项“-D”用于对Ant属性进行初始化赋值。如ant
-Dmetal=beryllium将会在执行build.xml中任务之间将build.xml中的属性metal赋值为beryllium。如果build.xml已经给metal赋值，那么新的值将覆盖旧的值。  
  
15、Ant可执行命令：  
    1）<javac srcdir="src" destdir="dest"></javac>：java源文件编译命令，将src目录下所有的源文件进行编译，编译后的class文件存放于dest目录下。如果没有指定属性destdir，则默认将编译好的class存放于源文件所在目录中。可带属性classpath（等价于javac的-classpath选项）和debug="true"（指示编译器应该带调试信息编译源文件）。另外，该命令只编译需要编译的源文件，如果源文件已经编译过并且没有发生改变，Ant不会再对该源文件进行编译，而是跳过。  
    2）
    
    
    <jar destfile="...dist.jar" basedir="classes">
            <manifest>
                <attribute name="Main-Class" value="mainclasspath"/>
            </manifest>
    </jar>

    将编译好的class类文件打包命令，classes文件夹中的所有class文件将被打包到dist.jar文件中，并且manifest向jar文件中的MANIFEST.MF文件添加属性Main-Class和属性值mainclasspath（完整的类名），指定了jar文件含main的类（可以执行的jar文件所需）。  
    3）<tstamp/>：初始化在生成环境中使用当前的时间和日期，之后可以使用Ant属性DSTAMP（设置当前日期，默认格式为YYYYMMDD）、TSTAMP（设置为当前时间，默认格式为HHMM）、TODAY（设置为当前日期，带完整年份，如2012年10月01日）。  
    4）<mkdir dir="newdir"/>：创建新目录newdir，如果父目录不存在则连同父目录一并创建，如果目录已经存在则忽略不执行。  
    5）<delete dir="olddir"/>：删除文件夹olddir。  
    6）<delete file="oldfile"/>：删除文件oldfile。  
    7）<copy file="srcfile" tofile="destfile"/>：复制文件srcfile到destfile，destfile为目标文件名（可以与原文件名不一致）。  
    8）<copu file="srcfile" todir="destdir"/>：复制文件srcfile到destdir目标文件夹，复制后的文件名与原文件名一致。  
    9）<move file="srcfile" tofile="destfile"/>：移动文件srcfile到destfile目标文件（名字可以不一致）。  
    10）<move file="srcfile" todir="destdir"/>：移动文件srcfile到destdir目标文件夹。  
    11）<zip destfile="output.zip" basedir="output" />：将文件夹output中的所有文件打包压缩到文件output.zip文件中。  
    12）<unzip dest="destdir" src="src.tar.gz"/>：将文件src.tar.gz解压缩到文件夹destdir中，默认覆盖已存在的重名的文件，可以使用属性overwrite控制是否覆盖。  
    13）<cvs cvsroot="crsRoot" package="chat" command="checkout" dest="destdir"></cvs>：关于CVS版本控制的操作，其中cvsroot属性为连接CVS服务器的参数（:pserver:qinxiandiqi:nan@localhost:C:\cvs；代表意义为:连接方式:用户名:密码@主机IP:主机CVS项目目录），package属性为操作的项目名称，command属性为执行的cvs操作命令，dest属性为操作的目标目录。  
    14）<replace file="input.txt" token="old" value="new"/>：文件内容替换操作，将input.txt文件中所有的old字符串替换为new。标签添加属性summary为true可以输出找到和替换的标记字符串实例的数目。  
    15）模式匹配：*/*.java匹配当前目录下所有目录里的所有java文件，**/*.java匹配当前目录下以及其所有后代子目录中的java文件。如：  

    
    
            <copy todir="destdir">
                <fileset dir="src">
                    <include name="*.java"/>
                </fileset>
            </copy>

    将src目录中所有java文件复制到destdir目录中。另外，fileset默认情况下包含指定src目录下的所有文件。另一方面，与include相对的元素exclude表示除去目录中符合exclude条件的文件。  
  
16、自定义Ant的xml文档命令标签（类似于JSP的自定义标签定义过程）：  
    1）定义标签的处理类，需要继承org.apache.tools.ant.Task类，并且重写类中方法execute()。  
    2）标签中的属性对应于标签处理类中的成员变量，只要提供成员变量的get和set方法，Ant能够自动将标签属性转换为处理类中的成员变量。  
    3）处理类中的方法execute()即为该标签需要执行的逻辑操作。  
    4）定义完标签的处理类后需要对处理类与标签进行关联，使用<taskdef name="tagName" classname="className" classpath="path" />。其中tagName即为自定义标签的名字，classname为标签处理类的类全名，classpath为处理类class文件所在路径。  
    5）与使用Ant提供的命令标签一样使用自定义标签。  
      
  

