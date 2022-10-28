---
# type: posts 
title: "JavaWeb学习笔记 第十记"
date: 2014-07-10T10:56:20+0800
authors: ["Jianan"]
summary: "1、FilterChain链的形成：定义好各个链中的Filter过滤器后，决定FilterChain链中各个过滤器顺序的关键在于web.xml文档中Filter配置标签的前后顺序。当web.xml中配置的多个的子标签内容是同一个或包含有同一个url时，这个几个标签代表的Filter过滤器就存在于同一个FilterChain链中，并且它们在链中的顺序按Filter的标签在web.xml中的先后顺序排"
series: ["JavaWeb学习笔记"]
categories: ["JavaWeb学习笔记"]
tags: ["javaweb", "filter", "servlet", "listener"]
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

1、FilterChain链的形成：定义好各个链中的Filter过滤器后，决定FilterChain链中各个过滤器顺序的关键在于web.xml文档中Filter配置标签的前后顺序。当web.xml中配置的多个<filter-
mapping>的子标签<url-
pattern>内容是同一个或包含有同一个url时，这个几个标签代表的Filter过滤器就存在于同一个FilterChain链中，并且它们在链中的顺序按Filter的<filter>标签在web.xml中的先后顺序排列。

  
2、Filter中doFilter方法里面对FilterChain的doFilter方法的调用语句是个分界线：在调用FilterChain的doFilter方法之前的语句是在请求传入FilterChain的时候执行，分界线之后的代码是在请求传达到最终请求资源并返回响应反向通过FilterChain的过程中执行。简单的说，FilterChain的doFilter方法之前的代码针对Request对象进行处理，方法之后的代码针对Response对象进行处理。

  
3、如果配置了Filter的url-
pattern标签为/*，则对所有Web资源进行过滤。如果要排除其中几个资源不过滤，只能在doFilter方法中使用代码进行排除，如调用HttpServletRequest的getRequestURI等方法获取请求的资源的URI，再根据URI决定是否不进行过滤。

  
4、Filter可以实现对敏感字符的过滤，在doFilter方法中使用String的replace方法可以对字符串进行过滤。

  
5、Filter在web.xml中的<filter>标签可以添加多个子标签<inti-param><param-name></param-
name><param-value></param-value><init-
param>，这个子标签用于配置Filter的一些初始化参数。在Filter的init方法中可以通过inti的参数FilterConfig的getInitParameter(String
name)方法获得name对应的value，或者通过FilterConfig的getInitParameterNames()方法获得所有参数的name组成的字符串数组。

  
6、Filter中同样可以访问到ServletContext中的数据，可以通过init方法中的FilterConfig的方法getServletContext()获得当前web应用的ServletContext对象，或者在doFilter方法中通过HttpServletRequest的getSession().getServletContext()来获得。

  
7、对于判断用户是否登录的代码通常放置在Filter中，因为这部分验证是通用的；对于判断用户是否有权限访问当前资源的代码通常直接放置在该资源中，因为这部分代码是针对特定资源的。

  

8、Servlet监听器ServletContextListener：实现接口javax.servlet.ServletContextListener，接口中有两个方法：  
    1）contextDestroyed(ServletContextEvent sce)：当ServletContext对象（JSP中的application）被销毁时自动调用的方法，该方法在所有的Servlet和Filter的Destroy方法调用之后再调用，也就是最后一个被调用。  
    2）contextInitialized(ServletContextEvent sce)：当ServletContext对象被创建的时候自动调用的方法，由于每个Web应用只有唯一一个ServletContext对象，并且在Web中其它组件之前创建出来，所以启动Web应用时这个方法最先被调用。  
    3）要让实现ServletContextListener的类监听一个Web应用，必须在该Web应用的web.xml配置文件中添加<listener><listener-class>监听类的全名</listener-class></listener>。  
  
9、Servlet监听器ServletContextAttributeListener：实现接口javax.servlet.ServletContextAttributeListener，接口中有三个方法：  
    1）attributeAdded(ServletContextAttributeEvent scab)：当使用ServletContext对象的setAttribute方法添加Attribute时自动调用的方法。  
    2）attributeRemoved(ServletContextAttributeEvent scab)：当使用ServletContext对象的removeAttribute(String name)方法删除一个Attribute时自动调用的方法。  
    3）attributeReplaced(ServletContextAttributeEvent scab)：当使用ServletContext对象的setAttribute方法更改一个已有Attribute的值时自动调用的方法。  
    4）要让实现ServletContextAttributeListener的类监听一个Web应用，需要在web.xml配置文件中添加<listener><listener-class>监听类的全名</listener-class></listener>。  
  
10、事件类javax.servlet.ServletContextAttributeEvent中对应ServletContext的Attribute的两个方法：  
    1）String getName()：获得更改Attribute的名字。  
    2）Object getValue()：获得更改Attribute的值。  
  
  

