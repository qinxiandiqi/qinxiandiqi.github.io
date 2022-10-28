---
# type: posts 
title: "JavaWeb学习笔记 第四记"
date: 2014-07-08T10:07:11+0800
authors: ["Jianan"]
summary: "1、JSP的执行过程：
1）客户端发出JSP请求，Tomcat服务器查找对应的JSP文件，如果找不到则直接返回，发出404错误。
2）Tomcat服务器判断客户端请求JSP文件是否被修改过或者第一次调用，如果是则调用JSP引擎解析JSP文件，生成对应的Servlet源代码（存放于Tomcat目录下的work目录中），并对Servlet源代码进行编译生成class文件，再将class文件加载入内"
series: ["JavaWeb学习笔记"]
categories: ["JavaWeb学习笔记"]
tags: ["javaweb", "jsp"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 668
comment_num: 0
---

1、JSP的执行过程：  
1）客户端发出JSP请求，Tomcat服务器查找对应的JSP文件，如果找不到则直接返回，发出404错误。  
2）Tomcat服务器判断客户端请求JSP文件是否被修改过或者第一次调用，如果是则调用JSP引擎解析JSP文件，生成对应的Servlet源代码（存放于Tomcat目录下的work目录中），并对Servlet源代码进行编译生成class文件，再将class文件加载入内存执行；如果不是则直接执行已经生成好的class字节码或者从Tomcat的work目录下加载class文件再执行。

  

2、JSP的原始代码包含JSP元素和Template data两部分。  
1）JSP元素是JSP引擎直接处理的部分，这部分必须符合JAVA的语法，否则会编译错误，比如写在<% %>中的代码。  
2）Template data部分：JSP引擎不处理的部分，即标记在<% %>之外的部分，比如HTML标记等。

  

3、JSP中一个Java语句可以跨越在不同的<% %>块之间，比如：  

    
    
        <body>
            <% for(int i=0;i<2;i++){%>
            循环两次
            <%}%>
        </body>

   浏览器得到的响应代码为：  

    
    
        <body>
            循环两次
            循环两次
        </body>

产生此结果的原因在于JSP引擎对JSP文件解析成Servlet的规则，它对JSP中<%%>中的部分直接处理，也就是直接放置到Servlet代码中；对Template
data部分直接输出，也就是放置到out.writer(）中的字符串参数中，如以上代码解析的结果是：  

    
    
    	out.write("<body>\n");
    	for(int i=0;i<2;i++){
    	out.write("\t循环两次\n");
    	}
    	out.write("</body>\n");

因此，循环两次会被输出两次。

  

4、JSP的语法分为三大部分：脚本语法、编译器指令、动作语法。

  

5、脚本语法：  
1）HTML注释：这种注释会发送给客户端，注释的内容可添加JSP表达式，JSP会将表达式结果作为HTML注释返回给客户端。  
2）隐藏注释：<%-- comments --%>。这种注释不会发送给客户端，也叫做的JSP注释，同样可以嵌入JSP表达式。  
3）脚本段：<% ... %>。JSP的Java代码主要位置，实现JSP的Java逻辑。  
4）表达式：<%= ...
%>：JSP解析后会将表达式的结果返回给客户端。需要注意的是表达式的结尾不能添加“;”，因为JSP解析为Servlet之后表达式被解析为out.print(表达式内容);如果添加了分号就等于在out.print的参数中添加了分号，必将导致编译错误。相对的，脚本段中的Java语句之后就一定要添加分号，因为脚本段解析后直接将脚本段内容作为Servlet的代码。  
5）声明：<%! 声明语句
%>。将变量的声明放置于JSP的声明中，多个声明使用分号隔开，JSP解析后将把声明中的变量作为Servlet的成员变量声明，而将变量的声明放置于脚本段中，则JSP解析后将声明变量作为_jspService方法的局部变量（_jspService方法相当于自定义Servlet中的doGet和doPost方法）。由于Servlet使用单例模式设计，所以Servlet的成员变量将会被多个客户端请求共享，一个客户端对Servlet的成员变量进行修改会影响到其它客户端对该成员变量的取值，而局部变量不会，这是将变量声明写在JSP声明和脚本段中的区别。

  

6、编译器指令：  
1）页指令：<%@ page 属性1=属性值1 属性2=属性值2 ...
%>。页指令用于对当前JSP页面做一些设置，一个JSP页面可以有多个页指令，并且无论页指令写在JSP文档的哪个位置，作用域都是整个JSP文档。另外，除了import属性，其它属性最多只能在所有的页指令中出现一次。需要注意的是页指令不能作用于动态的包含文件，如[jsp:include]()。常用属性有：  
a）language="java"：声明脚本段语言的种类，目前只能使用java。  
b）import="package1，package2..."
：导入需要的Java包，与Java源文件中的import功能一样。JSP默认导入了java.lang.*、javax.servlet.*、javax.servlet.jsp.*、javax.servlet.http.*四个类包，无需再对这个四个类包进行导入。  
c）errorPage="relativeURL"：设置处理异常事件的JSP文件，属性值为JSP文件的URL。  
d）isErrorPage="true|false"：设置此页是否为出错页，如果是就可以使用exception对象。  
e）pageEncoding：设置编码格式，一般使用UTF-8。  
2）包含指令：<%@ include file="URL"
%>。包含指令用于向当前页面中包含指令的位置插入一个静态文件的内容，这个文件就是file属性值URL指向的文件（可以为绝对地址，也可以为相对地址）。使用包含指令的相当于使用了一个变量，变量的内容是其它静态文件的内容，这么做的好处是可以将多个JSP文件重复的大量代码抽取出来成独立的静态文件，在需要使用到的JSP位置使用包含指令包含进来，就可以降低代码的冗余和统一管理。常将JSP输入的HEML头和尾等固定不变的代码作为包含指令。  
3）taglib指令：<%@ taglib uri="URI" prefix="tagPrefix" %>。用于引入定制的标签库。

  

7、动作语法：  
1）<jsp:forward page="URL|<%= expression %>"
/>：请求转发动作，其中page属性的值可以是一个地址字符串或者表达式（表达式的结果也是一个地址字符串）表明请求转发到哪个文件，这个文件可以是JSP，也可以是程序段。当使用[jsp:forward]()请求转发从一个JSP文件向另一个文件转移时，传递了一个包含了用户请求的request对象，并且[jsp:forward]()标签之后的代码将不能执行，因为JSP解析为Servlet之后在该标签后添加了return。  
2）<jsp:include page="URL|<%= expression %>" flush="true|false"
/>：包含一个静态或者动态文件进来，与包含指令功能类似，区别在于[jsp:include]()可以包含动态文件，而包含指令不行。这个动态的特征在于[jsp:include]()标签可以传递带一个或多个参数的request对象给要包含进来的动态文件，动态文件可以从request对象中取出传递的参数赋值后形成最终的静态代码，再将这些静态代码包含进[jsp:include]()标签所在的位置。  
3）<jsp:param name="paramName" value="paramValew|<%= expression %>"
/>：用于传递参数，[jsp:forward]()和[jsp:include]()标签通过request对象传递的参数需要使用[jsp:param]()标签作为子标签。每个[jsp:param]()标签中的name属性值和value属性值就是一个键值对，代表一对参数（参数名和参数值），需要传递多个参数需要使用多个[jsp:param]()子标签。传递过去的文件通过传递过去的request对象方法getParameter(String
name)就能取出对应的参数值。

  

8、JSP内置九种对象（是服务器已经构造好的类对象，不是类本身）：  
1）请求对象request：javax.servlet.ServletRequest的子类（常用）  
2）响应对象response：javax.servlet.ServletResponse的子类  
3）页面上下文对象pageContext：javax.servlet.jsp.PageContext  
4）会话对象session：javax.servlet.http.HttpSession（常用）  
5）应用程序对象application：javax.servlet.ServletContext（常用）  
6）输出对象out：javax.servlet.jsp.JspWriter（常用）  
7）配置对象config：javax.servlet.ServletConfig  
8）页面对象page：java.lang.Objece  
9）异常对象exception：java.lang.Throwable

  

9、JSP内置对象request：代表来自客户端的请求，比如表单填写的信息等，是JSP解析成Servlet后的方法_jspService的请求参数。主要方法有：  
1）getParameter(String name)：返回参数name的第一个值的字符串表现形式。  
2）getParameterNames(）：返回一个Enumeration对象，其中包含了request对象中所有参数名，Enumeration包含方法hasMoreElements()用于判断是否还有下一个参数名，以及方法nextElement()返回一个参数名并将游标移动到下一个参数名。  
3）getParameterValues(String
name)：返回一个String数组，将name参数的所有值组合成一个String数组返回，比如多选按钮的返回值。

  

10、JSP的内置对象response并不常使用，向客户端输出字符（字符组成HTML文档等）常直接使用内置out对象向客户端输出，response的用途在于文件的下载（out的字符输出难以满足文件大量字节的传输）。

