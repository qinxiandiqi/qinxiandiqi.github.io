---
# type: posts 
title: "JavaWeb学习笔记 第九记"
date: 2014-07-10T10:46:29+0800
authors: ["Jianan"]
summary: "1、HttpServletRequest和HttpServletResponse对象由Servlet容器负责创建，对于每一个HTTP请求，Servlet容器都会创建一个HttpServletRequest对象和HttpServletResponse对象。

2、Web服务器跟踪客户状态的四种常见方法：
    1）建立含有跟踪数据的隐藏字段（如input标签type属性值为hidden的控件"
series: ["JavaWeb学习笔记"]
categories: ["JavaWeb学习笔记"]
tags: ["javaweb", "session", "filter", "cookie"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 858
comment_num: 0
---

1、HttpServletRequest和HttpServletResponse对象由Servlet容器负责创建，对于每一个HTTP请求，Servlet容器都会创建一个HttpServletRequest对象和HttpServletResponse对象。

  
2、Web服务器跟踪客户状态的四种常见方法：

    1）建立含有跟踪数据的隐藏字段（如input标签type属性值为hidden的控件）。

    2）重写包含额外参数的URL。

    3）使用持续的Cookie。

    4）使用Servlet API中的Session机制（依赖于Cookie）。

  
3、Session指的是在一段时间内，单个客户与Web服务器的一连串相关的交互过程，在一个Session中，客户可能会多次请求同一个网页，也有可能请求不同的服务器资源。

  
4、Session的运行机制：

    1）当客户首次访问服务器支持Session的资源时，Servlet容器将创建一个HttpSession对象，在HttpSession对象中存放客户的状态信息。

    2）Servlet容器创建HttpSession对象的时候会为HttpSession对象分配一个唯一的标识符（很长以防止重复），称为Session ID。当客户首次访问Web服务器可支持Session的资源并返回响应时，HttpServletResponse对象中会自动添加一个Cookie对象，这个Cookie的name为SESSIONID，value为HttpSession的Session ID。客户得到服务器的响应后就得到了Session的Cookie。

    3）当这个客户向该Web服务器再次发出请求时，Servlet容器就能够从HttpServletRequest对象中获取客户端保存的Session ID，然后根据Session ID找到相应的HttpSession对象，从而获得该客户的状态信息。如果是一个新的客户发出请求，Servlet容器必然无法从客户端获取Session ID，也就找不到匹配的HttpSession对象，Servlet容器就会重复步骤1，为新的客户创建一个新的HttpSession对象以及后续系列操作。

    4）当客户端（也就是浏览器）关闭的时候，并没有向Web服务器发出请求，自然Servlet容器无法获知客户端已经关闭，Servlet容器也就没有将该客户的HttpSession对象销毁。HttpSession的销毁一般是根据HttpSession设定的无活动最长时间由服务器自动销毁。

  
5、HttpSession的主要方法：

    1）getID()：返回Session的ID。同一个浏览器只要未关闭，再开启这个浏览器的另一个标签页或者窗口，它们使用的都是同一个Session。而当这个浏览器关闭，就算再开启这个浏览器的另一个窗口所使用的Session也不是同一个Session。

    2）invalidate()：使当前的Session失效，Servlet容器会释放HttpSession对象占有的资源。一般不会使用这个方法，该方法主要提供给服务器使用。

    3）isNew()：判断当前Session是否是新创建的Session。

    4）setMaxInactiveInterval()：设定当前Session可以处于不活动状态的最大时间间隔，一旦超过这个时间间隔，Session就会自动失效。当它接收一个整数参数时表示不活动状态的最大时间间隔（单位为秒）；当它为负数时，表示不限制Session处于不活动状态的时间。

  
6、设置HttpSession最大不活动状态时间的方法：

    1）使用setMaxInactiveInterval()方法，设置的只是调用该方法的Session对象。

    2）进入Tomcat服务器管理后台，在控制台上设置具体Web应用的所有Session。

    3）在Web应用的web.xml文档中添加元素：<session-config><session-timeout>min</session-timeout></session-config>。其中min为最长处于不活动状态的时间（单位为分钟）。

    4）默认的Session最长不活动状态时间为30分钟，30分钟是个合理的时间，没有特殊需要，一般不改变。

  
7、不支持Session的情况：

    1）客户端禁止Cookie。

    2）JSP显示使用<%@ page session="false">

  
8、Session结束生命周期的情况：

    1）客户端浏览器关闭，Session结束现象的本质：Session Cookie存在于浏览器线程中，一旦浏览器线程结束，存在浏览器中的Session Cookie也随着一起销毁。一旦Session Cookie被销毁，服务器将没有办法再接收到SessionID为被结束SessionCookie的ID值。等到Session有效期到期时，服务器自动销毁Session。如果Session被设置为永远不销毁，则服务器永远存在这个Session，但无法再访问到，长期下来服务器上将会堆积很多无法访问的Session，造成服务器内存溢出崩溃。为使浏览器关闭时能够向服务器发送销毁Session的请求，可以使用JS脚本在Web应用的每个页面中调用内置对象window.onclose方法，使得在关闭浏览器之前向服务器发送一个销毁Session的请求。然而，如果浏览器崩溃或者浏览器进程被强制杀死，这种做法也无能为力。

    2）Session最长无活动时间到期，Session自动被服务器销毁。最为推荐的结束Session方法。

    3）调用Session的invalidate方法，一般也很少使用。

  
9、对于用户登陆的Web应用，一般都需要使用Session来记录跟踪用户的活动，并在每个页面执行之前都需要通过Session获取用户登陆信息来判断该请求是否拥有相关权限。

  
10、Filter过滤器：Java
Servlet规范2.3中定义的对Servlet容器的请求和响应进行检查和修改的组件，在使用上与Servlet非常类似，可以将Filter当做特殊的Servlet。

  
11、Filter的特点：

    1）Filter本身不生成请求和响应对象，只提供过滤作用。

    2）Filter能够在它负责过滤的组件调用之前检查Request对象，修改Request Header和Request内容。

    3）Filter能够在它负责过的的组件调用之后检查Response对象，修改Response Header和Response内容。

    4）Filter负责过滤的组件可以是任何一种Web组件，包括Servlet、JSP、HTML等。

  
12、定义自己的Filter，需要直接实现javax.servlet.Filter接口，这个接口中含有三个方法：

    1）init(FilterConfig)：Filter过滤器初始化方法，当Servlet容器创建Filter过滤器实例后就会调用这个方法。方法中可以通过FilterConfig对象读取web.xml文件中Filter过滤器的初始化参数。

    2）destroy()：Servlet容器在销毁Filter对象之前调用这个方法，在这个方法中可释放Filter的占有资源。

    3）doFilter(ServletRequest,ServletRespone,FilterChain)：Filter中最核心的方法，用于完成实际的过滤操作。当客户请求访问与该Filter过滤器关联的URL资源时，Servlet容器将先调用过滤器的doFilter方法。需要注意的是，doFilter方法接收的参数是ServletRequest和ServletResponse对象，通常需要将这两个参数强制转换为实际的HttpServletRequest和HttpServletResponse对象。而且！由于Filter和Filter或者Web组件实际上是串成一条链，当请求进入Filter的doFilter方法时，执行完过滤操作后需要使用它的参数FilterChain的方法doFilter(ServletRequst,ServletRespone)执行链中的下一个资源，否则请求将终止于当前Filter对象。

  
13、定义自己的Filter步骤：

    1）实现Filter接口，并实现接口的中三个方法。

    2）配置web.xml文件，与Servlet非常类似，需要配置两个标签：
    
    
            <filter>
    
                <filter-name>filtername</filter-name>
    
                <filter-calss>filtercalss</filter-class>
    
            </filter>
    
            <filter-mapping>
    
                <filter-name>filtername</filter-name>
    
                <url-pattern>url</url-pattern>
    
            </filter-mapping>

    与Servlet的配置标签不同的是所有的Filter配置标签都必须写在所有的Servlet配置标签之上，否则无效。另外，filter-mapping标签中的url-pattern子标签内容就是Filter过滤器需要过滤的资源的url，可以使用“/*”表示过滤Web应用的所有资源。

  
14、javax.servlet.FilterChain接口：其中只定义了一个方法doFilter(ServletRequest,ServletRespone)。Filter与它所过滤的Filter或者其它web组件构成一条FilterChain链，当一个Filter过滤后，需要调用Filter的doFilter方法中的参数FilterChain的doFilter(ServletRequest,ServletRespone)才能够将请求继续往下一个过滤器或者最终的Web资源发送。

  
15、Filter的运行流程：

    1）Servlet容器启动的时候，会自动加载所有的Filter对象（Filter是单例模式，所以所有的请求都共享同一个Filter对象，也不建议将需要更改的变量定义为Filter的成员）。一旦Filter加载失败（如Filter的init代码抛异常等），整个Web应用就无法启动，因此必须保证每个Filter代码的正确。

    2）Servlet容器接收到客户端请求后，生成对应的HttpServletRequest和HttpServletResponse对象，并将这两个对象发送给请求资源的FilterChain的第一个Filter。

    3）Filter接收到请求后，进行过滤操作，再调用FilterChain的doFilter方法将请求发送给FilterChain的下一个资源，直到请求到达最终的请求资源或者Filter过滤处理不符合要求直接返回响应为止。

    4）若请求能到达最终求的资源，资源处理请求后返回响应，这个响应同样需要经过之间经过的FilterChain链，只不过顺序相反。

    5）响应通过FilterChain链，向客户端返回最终的响应。

