---
# type: posts 
title: "JavaWeb学习笔记 第二记"
date: 2014-07-03T09:24:47+0800
authors: ["Jianan"]
summary: "1、HTTP协议是基于请求/响应，无状态的协议。


2、HTTP1.0协议的浏览器与服务区通讯过程：
1）客户发起连接
2）客户发起请求
3）服务器响应请求
4）服务器关闭连接
3、HTTP1.1在HTTP1.0的基础上增加了持续连接功能改善HTTP1.0的性能。在HTTP1.0协议下，客户每发一个请求都要先建立一个连接，如果一个HTML页面上包含多个资源（图片，css文件等），"
series: ["JavaWeb学习笔记"]
categories: ["JavaWeb学习笔记"]
tags: ["tomcat", "http协议", "web", "javaweb"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 876
comment_num: 0
---

1、HTTP协议是基于请求/响应，无状态的协议。

  

2、HTTP1.0协议的浏览器与服务区通讯过程：  
1）客户发起连接  
2）客户发起请求  
3）服务器响应请求  
4）服务器关闭连接

  

3、HTTP1.1在HTTP1.0的基础上增加了持续连接功能改善HTTP1.0的性能。在HTTP1.0协议下，客户每发一个请求都要先建立一个连接，如果一个HTML页面上包含多个资源（图片，css文件等），那么有多少个资源就得建立多少次连接还有本身自己的一个连接。这么一来容易造成网络上的拥堵，HTTP1.1的持续连接为解决这个问题，可以将用户的连接保持一段时间（由服务器设置保留多长时间），在保持的这段时间内，用户发送的请求不需要重新建立连接，用户可以一次性将请求全部发出而不需要等到一个请求得到相应关闭连接后在发送一个请求。

  

4、浏览器HTTP请求格式：http://host[: port][abs_path]  
1）http表示请求的协议名，不同的协议要用不同的协议名，如ftp。  
2）host表示请求的服务器IP地址，也可以使域名，域名将会通过DNS服务器解析为IP地址。  
3）:port：服务器监听的端口，默认为80端口。  
4）abs_ptah：请求服务器上的资源的URL统一资源定位符地址，如果没有指定资源，需要使用"/"代替（浏览器会自动添加）。

  

5、HTTP请求三个组成部分：请求行、消息报头、请求正文。在浏览器中输入网址后，浏览器会自动将地址栏的内容进行分析，封装和组合成HTTP请求三部分向服务器发出请求。

  

6、HTTP请求行格式：Method Request-URL HTTP-Version
CRLF。其中，Method为请求方法符号，必须为大写；Request-URL，请求的资源统一定位符，也就是浏览器请求格式的中abs_path；HTTP-
Version，HTTP协议的版本，一般为HTTP/1.1；CRLF，回车换行，在请求行中，各部分之间以空格分隔，并且除了结尾的CRLF之外，不允许出现单独的CR或者LF字符。请求行举例：GET
/test.html HTTP/1.1 (CRLF).

  

7、请求行中的主要方法符号：  
1）GET，请求获取有Request-URL所标识的资源，在浏览器的地之栏中直接输入网址的方式去访问网页的时候，浏览器使用GET方式向服务器获取资源。  
2）POST，在Request-URL所标识的资源后附加新的数据，也是用于向服务器发送请求，并要求服务器接受附加在请求后面的数据，常使用在表单的提交上。  
3）HEAD，请求获取Request-
URL所标识的资源的相应消息报头，与GET方法几乎一样，不同在于HEAD方法只是请求消息的报头，通常用户测试超链接的有效性，是否可访问，已经最近是否更新等。

  

8、HTTP的响应也由三部分组成：状态行、消息报头、响应正文。

  

9、HTTP响应状态行格式：HTTP-Version Status-Code Reason-Phrase CRLF。其中HTTP-
Version为服务器HTTP协议的版本，Status-Code表示服务器发回的响应代码，Reason-
Phrase表示状态代码的文本描述，CRLF表示回车换行。

  

10、HTTP状态行响应代码类别：  
1）1xx：指示信息，表示请求已经接收，继续处理。  
2）2xx：成功，表示请求已经被成功接收，理解，接收。  
3）3xx：重定向，要完成请求必须进行更进一步的操作。  
4）4xx：客户端错误，请求有语法错误或者请求无法实现。  
5）5xx：服务器端错误，服务器未能实现合法的请求。

  

11、常见HTTP状态行响应代码和对应描述：  
1）200，OK：客户端请求成功。  
2）400，Bad Request：由于客户端请求有语法错误，不能被服务器所理解。  
3）401，Unauthorized：请求未经授权，这个状态代码必须和WWW-Auhtenticate报头域一起使用。  
4）403，Forbidden：服务器接收到请求，但是拒绝提供服务，服务器通常会在响应正文中给出不提供服务的原因。  
5）404，NotFound：请求的资源不存在，比如输入错误的URL。  
6）500，Internal Server Error：服务器发生不可预期错误，导致无法完成客户的请求。  
7）503，Service Unavailable：服务器当前不能够处理客户的请求，在一段时间之后服务器可能会恢复正常。

  

12、telnet命令：telnet IP/主机域名 端口号，用于宣称登陆某主机端口。比如登陆百度主页：telnet [
www.baidu.com](http://www.baidu.com) 80，登陆后出现黑窗口，此时输入HEAD /index.html HTTP/1.1
(LRCF） Host：www.baidu.com后，两次回车（第一次表示输入完毕，第二次表示发出请求），将得到百度服务器的响应。

  

13、Tomcat服务器下主要目录说明：  
1）bin：存放Tomact的一些可执行程序，其中bat文件是在window系统下使用，sh文件是在Linux系统下使用。  
2）conf：存放一些XML等类型的配置文件。  
3）lib：Tomcat使用到的一些jar库文件，Tomcat本身也是由Java编写的。  
4）logs：Tomcat的日志文件存放目录。  
5）temp：Tomcat的临时文件目录。  
6）webapps：用于部署web应用程序，比如网站等。  
7）work：Tomcat编译用户的应用程序后产生的临时文件存放目录。  
8）其它文件：发布声明等。

  

14、Tomcat原来的名字叫Catalina，这也是为什么Tomcat需要配置环境变量CATALINA_HOME（变量值为Tomcat的目录）的原因。另外Tomcat需要配置JAVA_HOME环境变量，因为Tomcat本身也是Java程序。

  

15、telnet命令下连接使用HTTP的服务器，发送请求的时候，在请求行下可加Connection:Keep-
Alive命令持续连接，HTTP1.1协议默认使用这个命令。相对的，Connection:close则发送请求响应后立刻中断连接。

  

16、MyEclipse中创建Web
Project后，生成两个目录src和WebRoot，src存放java源代码，WebRoot存放Web应用文件。访问Web应用，需要使用浏览器通过服务器来访问到服务器上的Web应用，所以需要将Web应用部署到服务器上才能查看Web应用的情况。

  

17、部署Web应用到Tomcat服务器上最方便的方法：在Tomcat目录的conf目录下，打开server.xml服务器配置文件，找到文件倒数第四行，在上面添加XML元素：  
，其中path为浏览器请求格式中代表该web引用URL统一资源定位符abs_path的根目录，truepath为本地系统上web应用的WebRoot目录地址，这样在浏览器地址栏中通过端口号后添加“path/WebRoot下资源名字”就能访问到该资源，也就是说，用户使用path访问服务器就等于访问服务器的truepath路径，path和truepath之间是映射好的逻辑路径与物理路径的关系，这是出于安全考虑，用户不应该知道访问的资源在服务器上的物理路径。而reloadable表示当web应用的文件发生更改后是否自动刷新服务器，大部分时候会刷新，但有时需要手动刷新。

