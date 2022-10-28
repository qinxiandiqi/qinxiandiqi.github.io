---
# type: posts 
title: "JavaWeb学习笔记 第七记"
date: 2014-07-09T18:04:32+0800
authors: ["Jianan"]
summary: "1、JSP标签用于为JavaBeanID对象的number成员赋值elementname的值。param的值通常是request中的附加的数据的数据名，在JSP解析为Servlet后这部分实际上就是使用request.getParameter(elementname)。需要注意的是param属性和value属性不能同时使用，一个jsp:setProperty标签中只能使用两者其一。
2、jsp:"
series: ["JavaWeb学习笔记"]
categories: ["JavaWeb学习笔记"]
tags: ["javaweb", "jsp", "servlet", "tomcat"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 941
comment_num: 0
---

1、JSP标签<jsp:setProperty property="number" name="JavaBeanID"
param="elementname"
/>用于为JavaBeanID对象的number成员赋值elementname的值。param的值通常是request中的附加的数据的数据名，在JSP解析为Servlet后这部分实际上就是使用request.getParameter(elementname)。需要注意的是param属性和value属性不能同时使用，一个<jsp:setProperty>标签中只能使用两者其一。

  
2、<jsp:useBean>属性scope可选值：page、request、session、application。<jsp:useBean>标签在JSP解析为Servlet后会做两件是：先new一个JavaBean对象，再调用_jspx_page_context.setAttribute("JavaBeanID",JavaBeanID,PageContext.XXX_SCOPE)方法。其中_jspx_page_context是一个PageContext对象，PageContext一般由Tomcat服务器使用，它的setAttribute方法将JavaBeanID以PageContext.XXX_SCOPE方法存到对应的内置对象中，它的getAttribute("JavaBeanID",PageContext.XXX_SCOPE)方从对应的内置对象中取出JavaBean对象。

    1）page是默认值，表示声明的JavaBean对象只在当前页面内存活。当客户请求访问的当前JSP页面通过<jsp:forward>标签转移到另一个文件或者客户请求访问的当前JSP页面顺利执行完毕并向客户端发回响应后，JavaBean对象的生命周期就停止。

    2）request表示只能存活于一个请求的范围内。客户每次发出请求访问JSP页面的时候都会创建一个新的JavaBean对象，这个JavaBean对象可以在请求访问的当前JSP页面，或者当前JSP页面使用<%@ include>和<jsp:forward>包含的其它JSP文件中使用。一旦客户端发送的这个请求得到响应，JavaBean对象的生命周期就结束。也就是说，request为属性值的JavaBean对象可以存活在这个request对象访问的所有页面中，通过这个request的getAttribute(JavaBeanID)就能够从request中获得这个JavaBean对象。

    3）session表示存活于一个会话范围内。只要是同一个session对象，就能够拥有同一个JavaBean对象，同样通过session.getAttribute(JavaBeanID)可以获得session对象中的JavaBean对象。

    4）application表示存活于当前web应用范围内。同样通过application.getAttribute(JavaBeanID)可以访问到这个JavaBean对象。

  
3、服务器返回给客户端的响应显示出现乱码的情况，通常是客户端提交的数据编码方式与服务器端编码方式不同导致。客户端通常使用ISO-8859-1编码方式提交数据，但为正常显示中文提倡使用utf-8编码方式。为此，需要在服务器端对客户端提交的数据进行转码，利用String的构造方法String(byte[]
,String type)进行转码。使用框架设计的web应用在框架本身基本能够自己完成编码的统一。

  
4、Servlet和Servlet容器：

    1）Servlet是和平台无关的服务器端组件，运行在Servlet容器中。

    2）Servlet容器负责Servlet和客户的通信以及调用Servlet的方法，比如Tomcat。

    3）Servlet和客户的通信采用“请求/响应”的模式。

  
5、Servlet的作用：

    1）创建并返回基于客户请求的动态HTML页面。

    2）创建可嵌入到现有HTML页面中的部分HTML页面。

    3）与其它服务器资源（如数据库或者基于Java的应用程序）进行通信。**

  
6、Servlet框架的两个构成Java包：

    1）javax.servlet：定义了所有Servlet类都必须实现或者扩展的通用接口和类，可能包含多种协议的Servlet。

    2）javax.servlet.http：定义了采用HTTP协议通信的HttpServlet类，编写Web应用直接与这个类包打交道比较多。

  
7、所有的Servlet都必须实现javax.servlet.Servlet接口，HttpServlet类实现了这个接口，所以可以直接继承HttpServlet类。

  
8、Servlet接口中提供了三个代表Servlet生命周期的方法：

    1）init(ServletConfig config)：当Servlet被初始化的时候由Servlet自动回调的方法。

    2）service(ServletRequest req,ServletResponse res)：当Servlet接收到客户端请求后自动回调的处理请求的方法。

    3）destroy()：当Servlet生命周期结束时，由Servlet容器自动回调的销毁方法。

  
9、javax.servlet.GenericServlet类：实现了Servlet接口，提供了不基于任何通信协议的通用Servlet，对Servlet接口的init和destroy方法进行了简单实现，而对service方法定义为abstract抽象方法，需要它的子类去具体实现。虽然GenericServlet提供了对Servlet接口的简单实现，但是不建议直接继承该类来实现自己的Servlet，而是建议继承它的子类，如HttpServlet。

  
10、javax.servlet.http.HttpServlet类：继承了GenericServlet类和实现了Serializable序列化接口，是基于HTTP协议的Servlet。由于继承了GenericServlet，所以对service方法进行了简单实现，而且还提供了重载方法service(HttpServletRequest
req,HttpServletResponse resp)。Tomcat响应请求的过程：

    1）客户端向Tomcat服务器发送HTTP请求，Tomcat接收到HTTP请求后为该请求创建HttpServletRequest对象和HttpServletResponse对象。

    2）Tomcat将生成的HttpServletRequest和HttpServletResponse对象发送给Servlet接口的service(ServletRequest res,ServletResponse resp)方法。HttpServlet对Servlet接口的这个service方法的实现就是将接收的参数res和resp对象强制还原回HttpServletRequest和HttpServletResponse对象，并将还原的对象发送了HttpServlet重载的service(HttpServletRequset req,HttpSerlvetRespone resp)方法。

    3）HttpServlet重载的service方法接收到req和resp对象后，会从req对象中使用getMethod方法获得请求的请求方法，并对应请求方法调用HttpServlet用户重写的doGet、doPost等方法处理请求并返回响应（没有重写doGet或者doPost方法都是根据客户端请求的HTTP协议版本号返回不同的错误页面，所以必须重写这些doXXX方法）。

    注：关联Tomcat源代码，查看Tomcat实现HttpServlet的源代码。

  
11、request、session、application等含有setAttribute方法的对象，这些方法的底层是通过HashMap来实现的。

  

