---
# type: posts 
title: "JavaWeb学习笔记 第六记"
date: 2014-07-09T17:48:44+0800
authors: ["Jianan"]
summary: "1、重定向ServletResponse.setRedirect(String location)：将转移到location路径代表的页面上，如果location以“/”开头则说明它从Servlet的容器目录中寻找，也就是http://host:port/Servlet所在web应用的目录下寻找。的目录下查找转移目标文件；如果不是则从当前
2、请求转发和重定向的区别：
1）请求转发的整个过程中"
series: ["JavaWeb学习笔记"]
categories: ["JavaWeb学习笔记"]
tags: ["javaweb", "javascript", "javabean", "jsp", "html"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 763
comment_num: 0
---

1、重定向ServletResponse.setRedirect(String
location)：将转移到location路径代表的页面上，如果location以“/”开头则说明它从Servlet的容器目录中寻找，也就是http://host:port/的目录下查找转移目标文件；如果不是则从当前Servlet所在web应用的目录下寻找。

  

2、请求转发和重定向的区别：  
1）请求转发的整个过程中，源页面和目标页面都在同一个请求中，使用的都是同一个request对象。  
2）重定向的过程是：Servlet使用ServletResponse.setRedirect(String
location)转移到location页面，实际上是Servlet的这个方法已经向客户端返回响应Response，这个响应要求客户端访问location页面，客户端接收到这个响应后就会重新向服务器发送一个请求，访问location页面。也就是说，重定向的源页面和目标页面处于两个不同的请求中，所以重定向的目标页面中没有办法从request中取到源页面中的数据。

  

3、JSP或HTML页面使用JavaScript的声明一般写在head标签中，JavaScript的代码就写在script标签中。

  

4、JavaScript语言中方法function也是一个类，定义要写在script标签中，定义function的格式为function
functionName(Element){}。调用function的时候，如果function有返回值要用“return
functionName(Element)”调用；如果没有返回值则直接使用functionName(Element)调用。

  

5、JavaScript语言是基于事件机制的。对应于JS的事件机制，HTML中form标签拥有JS事件属性onsubmit、onclick等。比如onsubmit="retrun
funtionName()"调用方法funtionName方法并返回一个值（一般是boolean值），当返回为true时则提交表单数据，返回为false时不提交表单数据。

  

6、JavaScript内置对象document代表当前页面，可以帮助JS获取当前页面中的控件：  
1）getElementByID(String ID)：返回当前页面中ID代表的控件对象。  
2）getElementsByName(String
name)：返回当前页面中属性name的所有控件对象组成的数组，JavaScript中的数组类似于Java中集合，长度可变。  
3）getElementsByTarget(String target)：返回当前页面中标签为target的所有控件对象组成的数组。

  

7、JavaScript的alert()方法执行后将在浏览器弹出对话窗口。

  

8、全选页面中所有checkbox按钮可以通过JavaScript脚本来实现。利用document的getElementsByName或者getElementsByTarget等方法获得所有checkbox控件对象进行设置checkbox的checked属性（boolean值）。

  

9、JavaScript中所有判断是否相等的方法只有使用运算符“==”，包括比较字符串也是使用这个运算符。

  

10、客户端通过JavaScript验证的结果是不安全，因为用户可能通过浏览器知道页面的JS脚本代码知道验证的过程，并通过在请求中附加数据的方法直接访问服务器。为确保安全，除了在客户端验证之外，一般都要在服务器进行再次验证。

  

11、使用客户端验证的好处是可以降低服务器端的负担也可以提交响应速度。通过客户端的简单验证，当用户输入出现简单错误的时候就可以在客户端判断出来，不用将数据提交到服务器，服务器判断出错误后再返回响应提示错误，这样就提高了错误响应速度。

  

12、MVC设计模式：指的是将程序分为Model（模型，负责处理数据）、View（视图、负责显示和描述）、Controller（控制器，负责逻辑转向）三大模块。在Web应用设计中，View角色通常用JSP担任，Controller角色通常由Servlet担任，而Model是各种处理数据的类。在这种MVC设计模式中，Servlet用于接收用户请求并根据请求的性质调用相应的模型处理请求中的数据（通常一个MVC中包含多个Model用于处理不同的数据）。调用对应模型处理数据后，Servlet根据模型的处理结果决定将使用哪个View来显示并将结果转发给对应的View返回给客户端。

  

13、JavaBean是一种可重复使用并且跨平台的软件组件，分为具有用户界面和没有用户界面两种。没有用户界面的JavaBean主要用于负责处理事务，是JSP常访问的JavaBean。

  

14、JavaBean实质上就是一个Java类，只不过它需要符合一定的规范，标准的JavaBean需要符合以下特点：  
1）JavaBean必须以public修饰符修饰，也就是public类。  
2）JavaBean至少必须具有一个不带参数的构造方法。  
3）JavaBean要将自己的属性暴露在外面以JavaBean的身份被访问，必须提供这些属性的setXXX和getXXX方法。

  

15、JSP访问JavaBean的步骤：  
1）导入JavaBean类：通过<%@ page import="TypeClass（类全名）" %>导入。  
2）声明JavaBean变量：使用标签<jsp:useBean id="JavaBeanID" class="TypeClass" scope=""
/>来声明。在这个标签中必须显示定义属性id和class，id代表声明的JavaBean对象，class代表声明JavaBean类型。实际上，JSP将jsp页面解析为Servlet后，这个标签new构造了一个TypeClass类型的JavaBean对象，这个对象的变量名就是id的属性值。同时也通过_jspx_page_context.setAttribute("persion",persion,int
scope)方法设置该JavaBean对象的生命周期范围。因此，这个标签也相当于直接在JSP页面中使用<% TypeClass JavaBeanID =
new TypeClass(); %>。  
3）访问JavaBean变量之取值：使用标签<jsp:getProperty name="JavaBeanID" property="number"
/>来获取JavaBeanID对象的属性number的值，JSP解析为Servlet后，这个标签实际上解析为Tomcat利用反射机制提供的方法获得JavaBeanID对象，再使用这个对象的getXXX()方法获得number成员的值。因此，这个标签也相当于直接在JSP页面中使用<%=
JavaBeanID.getXXX(); %>。  
4）访问JavaBean变量之设值：使用标签<jsp:setProperty name="JavaBeanID" property="numbet"
value="value"
/>来设置JavaBeanID对象的属性number值为value。JSP解析为Servlet后，这个标签实际上解析为Tomcat利用反射机制提供的方法获得JavaBeanID对象，方法中也封装了使用getXXX方法。因此，这个标签也相当于直接在JSP页面中使用<%
JavaBeanID.setXXX(value); %>。

  

16、之所以可以在JSP页面中直接通过JSP脚本段使用Java语句访问JavaBean，但还是提倡使用JSP标签访问JavaBean，是因为这么做可以将Java代码与JSP的HTML代码分离。一方面使得JSP页面代码更加统一（全部是标签，与HTML很类似）；二是将Java代码分离后有利于维护JSP，利用JavaBean组件可重用性的特点提高网站的开发效率。

  

17、使用JSP标签访问JavaBean的本质是生成了JavaBean对象，所以在JavaBean声明标签之后，可以在JSP脚本段<%
%>中可以直接使用声明标签中JavaBean的id属性值来访问生成的JavaBean对象。

