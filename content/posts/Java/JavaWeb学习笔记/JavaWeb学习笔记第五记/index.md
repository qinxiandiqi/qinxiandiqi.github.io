---
# type: posts 
title: "JavaWeb学习笔记 第五记"
date: 2014-07-08T10:18:17+0800
authors: ["Jianan"]
summary: "1、JSP内置页面上下文对象pagContext：代表当前页面运行的一些属性，常用方法有findAttribute、getAttribute、getAttributesScope、getAttributeNamesInScope等，一般很少使用这个对象，Servlet容器才会使用这个对象。


2、JSP内置会话对象session：代表服务器和客户端所建立的会话，当需要在不同的JSP页面中保"
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
read_num: 781
comment_num: 0
---

1、JSP内置页面上下文对象pagContext：代表当前页面运行的一些属性，常用方法有findAttribute、getAttribute、getAttributesScope、getAttributeNamesInScope等，一般很少使用这个对象，Servlet容器才会使用这个对象。

  

2、JSP内置会话对象session：代表服务器和客户端所建立的会话，当需要在不同的JSP页面中保留客户信息的情况下使用，比如在线购物，客户轨迹跟踪等，只要规定的时间内浏览器没有关闭，它的生命周期就没有结束。主要方法有setAttribute(String
name ,Object value)和getAttribute(String name).

  

3、JSP内置应用程序对象application：负责提供应用程序在服务器运行时的一些全局信息，常用方法有getMimeType和getRealPath等。

  

4、JSP内置输出对象out：与response对象不同，通过out对象发送的内容将是浏览器需要显示的内容，属于文本级别，可以通过out对象直接向客户端写一个有程序动态生成HTML文件。常用方法除了print和println之外，还有clear、clearBuffer、flush、getBufferSize和getRemaining等一些控制out数据缓冲区的方法。

  

5、JSP内置配置对象config：提供一些配置信息，常用方法有getInitParameter和getInitParameterNames以获得Servlet的初始化参数。

  

6、JSP内置页面对象page：代表正在运行的由JSP文件产生的类对象，不建议使用。

  

7、JSP内置异常对象exception：代表了JSP文件运行时所产生的异常对象，只有当JSP页面使用了<%@ page
isErrorPage="true" %>命令后才能使用这个exception对象。

  

8、request对象的getParameter方法与getAttribute方法的区别：getParameter方法获取的是客户端随着请求传递过来的数据，getAttribute方法获取的数据是服务器端通过setAttribute(String
name,Object
value)方法设置的数据（通常使用该方法都需要将结果进行向下具体类型的强制转换）。由于以上区别，所以getParameter方法没有对应的setParameter，客户端的数据通常都附加在请求后面或者由表单提交，服务器没办法决定客户端提交的数据是什么。

  

9、request、session、application：它们都拥有getAttribute和setAttribute方法，区别在于：  
1）request通过这两个方法设置的数据只能存活于request生命周期的范围内。request的生命周期就是客户端发送一个请求的服务器，服务器接收到请求后就会创建一个request对象封装客户端的请求信息，服务器对请求处理完毕向客户端的返回响应对象response后，request的生命周期就结束，也就是request存活于一个请求和响应之间。  
2）session通过这两个方法设置的数据只能存活于session生命周期的范围内。一个现象是当客户端向服务器发出请求后服务器就会创建一个会话session，只要在一定时间内浏览器没有关闭，浏览器和服务器之间就只存在一个session，一旦超时或者浏览器关闭session生命周期结束。  
3）application（ServletContext对象）通过这两个方法设置的数据只能存活于application生命周期内。application的生命周期就是只要服务器没有关闭，application的生命周期就没有结束，无论是哪个客户端访问服务器，它所获得的application都是同一个对象。而一旦服务器重启，application将被销毁并重新构造一个新的application对象。  
4）request对象使用setAttribute方法存数据，并且当前JSP页面使用[jsp:forward]()转移到另一个页面，在另一个页面中通过request对象的getAttribute可以取出原跳转页面设置的数据。原因在于使用[jsp:forward]()标签请求转发到新的页面，客户端是不知道服务器进行页面的跳转的。由始至终，对于客户端来说只是发送了一个请求，服务器请求转发到新的页面后，新的页面也处于同一个请求中，使用的是同一个request对象。相对的，如果不是使用[jsp:forward]()标签请求转发，而是使用超链接链接到另一个页面，对于客户端来说实际上是重新发送了一个请求，两个页面处于不同的请求使用不同的request对象，理所当然取不到另一个request对象中的数据。  
5）一个JSP页面向session对象使用setAttribute方法存入数据后，只要客户端浏览器在一定时间内一直没有关闭，那么这段时间内服务器都可以使用getAttribute方法取得数据，也就是session中的数据一直存在。

  

10、Servlet中的请求转发：通过doGet或doPost等方法的HttpServletRequest参数的getRequestDispatcher(String
url)方法构造一个RequestDispatcher对象，其中url是需要定向到的页面地址。获得RequestDispatcher对象后，使用它的forward(HttpServletRequst
req,HttpServletResponse
resp)方法就能将请求req和响应resp转发到url页面上。这种做法在Web应用中很常用，Web应用里面通常使用Servlet处理业务逻辑，而JSP用于显示页面（JSP编写HTML代码方便）。将Servlet业务逻辑处理的结果传递给JSP通常都是使用这种请求转发，将Servlet中的结果使用setAttribute方法存储到request对象中转发到显示页面JSP上，由JSP取出结果显示。

  

11、application.getRealPath(String
url)：用于返回url指向文件在服务器系统上的物理路径，这个方法对于上传下载文件需要使用的io流非常有用。

  

12、表单类型hidden：。这种表单类型不会显示在浏览器页面上，但是在提交按钮提交的时候会将该表单类型的名字和值也一并提交，通常用于在提交数据中附加用户不需要知道但是服务器需要知道的数据。向导式的注册通常也使用这种hidden而不用session，在Web开发中处理登陆使用session保存登陆信息之外，能不用session就不用session，因为session占用服务器资源。

