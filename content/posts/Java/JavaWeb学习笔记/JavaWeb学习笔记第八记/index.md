---
# type: posts 
title: "JavaWeb学习笔记 第八记"
date: 2014-07-09T18:13:11+0800
authors: ["Jianan"]
summary: "1、ServletRequest接口中主要方法：
    1）getAttribute：根据参数给定的属性名返回属性值。
    2）getContentType：返回客户请求数据MIME类型（MIME类型是定义在Tomcat的web.xml文档中的一组媒体类型）。
    3）getInputStream：返回以二进制方式直接读取客户请求数据的输入流。
    4）getParamete"
series: ["JavaWeb学习笔记"]
categories: ["JavaWeb学习笔记"]
tags: ["javaweb", "tomcat", "cookie", "web应用", "servlet"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 691
comment_num: 0
---

1、ServletRequest接口中主要方法：

    1）getAttribute：根据参数给定的属性名返回属性值。

    2）getContentType：返回客户请求数据MIME类型（MIME类型是定义在Tomcat的web.xml文档中的一组媒体类型）。

    3）getInputStream：返回以二进制方式直接读取客户请求数据的输入流。

    4）getParameter：根据给定的参数名返回参数值。

    5）getRemoteAddr：返回远程客户主机的IP地址。

    6）getRemoteHost：返回远程客户主机名。

    7）getRemotePort：返回远程客户主机的端口。

  
2、ServletResponse接口中的主要方法：

    1）getOutputStream：返回可以向客户端发送二进制数据的输出流对象ServletOutputStream。

    2）getWriter：返回可以向客户端发送字符数据的PrintWriter对象。

    3）getCharacterEncoding：返回Servlet发送的响应数据的字符编码集。

    4）getContentType：返回Servlet发送的响应数据的MIME类型。

    5）setContentType：设置Servlet发送的响应数据的MIME类型。

  
3、Servlet容器装载Servlet的时刻：

    1）Servlet容器启动时自动装载某些Servlet，这种Servlet需要在Web应用中的web.xml文档中对应的servlet标签中添加子标签<load-on-startup></load-on-startup>，标签内容接收一个整数，表示Servlet容器启动时加载的优先级。

    2）Servlet容器启动后，客户首次向Servlet发出请求。

    3）Servlet的类文件被更新后，重新装载Servlet。

  
4、Servlet被装载后，Servlet容器将创建一个Servlet实例，并且调用Servlet的init方法进行初始化。整个Servlet生命周期中，init方法只会被调用一次。

  
5、Servlet终止的时刻（Servlet终止的时候会先调用Servlet的destroy方法，在destroy方法中可以释放Servlet占有的资源）：

    1）Servlet所在的Web应用被终止。

    2）Servlet容器终止运行。

    3）Servlet容器重新装载Servlet的新实例。

  
6、有些Servlet在web.xml文件中只有<servlet>标签没有<servlet-
mapping>标签，这就说明客户端无法访问这个Servlet。这样的Servlet在<servlet>标签下通常都有子标签<load-on-
startup>，目的是在web应用启动的使用自动加载这个servlet，并且通过这个servlet的init方法来完成一些全局性的初始化工作。这种Servlet被加载后无法再访问到。

  
7、Tomcat容器中，将客户端随着请求发送过来的参数放在一个HashTable对象中个，这个HashTable是一个String-
String[]的键值映射，也就是说getParameter和setParameter方法的底层是通过操作HashTable对象来实现。

  
8、Web应用的启动时刻：

    1）Servlet容器启动的时候会自动启动容器中所有的web应用。

    2）通过Servlet控制台启动web应用。

  
9、Tomcat控制台管理web应用：Tomcat的conf目录下的tomcat-
users.xml文件用于配置Tomcat服务器的管理账户。浏览器直接登录Tomcat首页从Status入口可以用Tomcat服务器的管理账户登录，在从applications入口进入可以管理Tomcat的Web应用，包括提交web应用并部署web应用，以及卸载web应用。

  
10、ServletContext和Web应用的关系：当Servlet容器启动Web应用的时候会自动为每个Web应用创建一个唯一的ServletContext对象（JSP中将这个对象内置为application对象），ServletContext与Web应用的服务器端组件共享内存。ServletContext是单例模式，因此一个Web应用也只能有一个ServletContext对象，又由于它的特性称为一个Web应用的全局对象。

  
11、获得ServletContext对象的方法：

    1）JSP中已经将其所在Web应用的ServletContext对象内置为JSP对象application。

    2）Servlet中通过ServletRequest.getSession().getServletContext()获得。

  
12、Servlet/JSP技术基于多线程运行，具有很高的执行效率，但也再带一些多线程问题，其根源在于：

    1）Web应用的Servlet都是单例模式，同一个Servlet无论创建多少个对象都是同一个Servlet对象。

    2）Tomcat每接收到一个客户端请求后生成了对应的Request和Response对象，并为这个请求创建了一个线程。线程中构造请求中的Servlet对象，由于Servlet是单例模式，所以多个客户端发送对同一个Servlet的请求的线程中使用Servlet对象实际上都是同一个Servlet对象，也就是多个客户端共享同一个Servlet对象和对象中的成员变量。

    3）由于每个客户端对同一个Servlet请求共享同一个Servlet对象中的成员变量，而多个客户端在各自处理线程中并发运行。当它们访问Servlet对象中的成员变量就可能导致多线程的同步问题。

  
13、解决Servlet/JSP同步问题方案：

    1）不使用Servlet成员变量，使用局部变量。最提倡最推荐使用的做法。

    2）使用同步代码块：synchronized{}。

    3）Servlet实现javax.servlet.SingleThreadModel接口，该接口是个标志接口，在Servlet2.4中已经废弃。实现这个接口的Servlet将使Servlet以单线程的方式运行，也就是同一个时刻只有一个线程执行Servlet的service方法，这么一来就失去了Servlet多线程的优势，因此强烈不推荐使用！

  
14、Thread.currentThread().getName()静态方法可以获得当前线程的名字（这个名字是唯一的）。

  
15、Cookie是用于访问Web服务器时，服务器在客户端硬盘上存放的信息，服务器可以根据Cookie来跟踪用于，对于需要区分用户的场合（如电子商务）特别有用。

  
16、Cookie对象中包含了一对Key/Value，与Cookie相关的方法：

    1）构造方法：Cookie(String cookiename,String cookievalue)，构造一个名字为cookiename，值为cookievalue的Cookie对象。

    2）setMaxAge(int expiry)：设置该Cookie在客户端的存活时间，当参数expiry为正数时表示Cookie对象在客户端硬盘上能够存活的秒数（单位为秒）；当expiry为负数时，表示当客户端（也就是浏览器）关闭的时候销毁这个Cookie；当expiry为0时，客户端销毁这个Cookie。

    3）HttpServletResponse.addCookie(Cookie cookie)：Cookie是由服务器创建但是存放在客户端，所以需要将服务器创建的Cookie对象发送为客户端，也就是通过HttpServletResponse响应对象。addCookie方法可以多次使用，也就是一个响应可以向客户端发送多个Cookie，保存多对Cookie键值对。

    4）HttpServletRequest.getCookies()：服务器可以从客户端取回Cookie，所以需要通过客户端的请求HttpServletRequest对象来获取。由于客户端可以保存多个Cookie，所欲getCookies()方法返回的是一个Cookie[]数组。

  

