---
# type: posts 
title: "JavaWeb学习笔记 第三记"
date: 2014-07-04T09:23:08+0800
authors: ["Jianan"]
summary: "1、MyEclipse创建web project后，创建的WebRoot目录：META-INF、WEB-INF、index.jsp。其中，META-INF目录的文件不用直接与其打交道，最重要的是WEB-INF目录，里面存放了应用的文件，包括classes目录（存放应用编译好的class文件）、lib目录（应用需要的第三方类库必须放在这个目录下）、web.xml（整个web应用的核心，用于配置web"
series: ["JavaWeb学习笔记"]
categories: ["JavaWeb学习笔记"]
tags: ["javaweb", "tomcat", "web应用", "servlet", "jsp"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 795
comment_num: 0
---

1、MyEclipse创建web project后，创建的WebRoot目录：META-INF、WEB-INF、index.jsp。其中，META-
INF目录的文件不用直接与其打交道，最重要的是WEB-
INF目录，里面存放了应用的文件，包括classes目录（存放应用编译好的class文件）、lib目录（应用需要的第三方类库必须放在这个目录下）、web.xml（整个web应用的核心，用于配置web应用）。

  

2、Tomcat第二种部署web应用方法：将WEB-
INF文件夹拷贝到Tomcat的webapps目录下，此时web应用目录的名字就是浏览器请求格式的abs_path的根地址。相比第一种方法，比较麻烦，也不能映射地址。

  

3、第二种方法部署的web应用可以是一个文件夹，也可以使一个war的web应用的压缩文件，Tomcat启动时会将war解压成可用的web应用。

  

4、Servlet是Java的服务器编程，不同于SE的应用程序，Servlet程序是运行在服务器上。

  

5、Servlet的特点是没有main方法，web应用中的servlet类需要继承Servlet类或者它的子类，一般是继承HttpServlet。HttpServlet中定义了doGet和doPost等对应HTTP协议请求行方法符号的方法，当用户使用某种方法发出对这个Servlet请求时就会调用Servlet对应的方法执行。

  

6、用户能够访问Servlet需要对在web应用中WEB-INF目录下的web.xml配置文件中对Servlet进行配置：  
  
ServletName  
ServletClass  
  
  
ServletName  
ServletUrl  
  
其中ServletName是对Servlet类的一个别名，servlet和servlet-mapping两个标签中的servlet-
name必须一致。ServletClass为Servlet类的完整类名。ServletUrl为Servlet类的Url标识符，浏览器的请求格式中abs_path的根目录后就是ServletUrl，通过这个ServletUrl才能访问到Servlet类这个资源。

  

7、浏览器地址栏访问Tomcat服务器上Web引用Servlet详细过程：  
1）浏览器地址栏输入请求格式符：http://host:port/Webappname/resourcepath/Webappname/resourcepath即为abs_path）。（  
2）浏览器将请求格式符拆解为HTTP请求发送为host主机的port端口。  
3）Tomcat服务器与浏览器建立连接接收到浏览器请求，分析出请求目标资源的URL即abs_path。此时Tomcat服务器会查询它的server.xml配置文件是否存在Context标签，并且Context标签中的path属性值为/Webappname。如果存在且匹配，Tomcat则会定位到Context标签中docBase属性值指向的物理路径，也就是Web应用目录。要是没有找到匹配项，Tomcat会查找Tomcat下的Webapps目录中是否存在名为Webappname的Web应用，存在则定位到这个Web应用目录，不存在则向浏览器返回404错误。  
4）Tomcat能定位到Web应用目录后，对于Servlet，Tomcat会查询这个Web应用的web.xml配置文件中的所有servlet-
mapping标签，如果能匹配到servlet-mapping标签子标签url-
pattern值正好为resourcepath，则Tomcat会继续查找web.xml配置文件在的所有servlet标签，看是否存在servlet的servlet-
name值为servlet-mapping的servlet-name值相同，相同的话就能对应servlet标签中的子标签servlet-
class值，找到最终浏览器请求的Servlet类路径（Servlet的类全称）。如果这个过程失败，也就是找不到浏览器请求的Servlet资源，则返回404错误。  
5）Tomcat找到浏览器请求的Servlet后，会先查询这个Servlet类是否已经编译。如果已经编译，则直接去Web应用的classes目录中找到编译好的class文件直接执行；如果没有编译，则会进行编译后执行class文件。  
6）浏览器地址栏的请求默认使用GET方法请求，所以编译好的Servlet类执行时，Tomcat自动执行Servlet类的doGet方法，并将响应返回给浏览器，完成一次请求响应。

  

8、Servlet中会使用到的方法：  
1）HttpServletResponse.getWriter()：获得响应的打印输出流PrintWriter对象，通过向这个PrintWriter流中写入数据，由于带缓冲，最后使用PrintWriter.flush()方法强制输出缓冲中所有内容，就能够将PrintWriter流中的数据输出到浏览器。  
2）HttpServletRequest.getParameter(String
name)：获得请求中附加的数据中名字为name的字符串格式的值，通常用于提取表单提交的请求中的表单数据。

  

9、表单form的属性action的值为浏览器地址栏请求格式符中的abs_path，当表单中的submit按钮点击后，就会将表单的数据封装成HTTP请求发送给当前服务器的资源abs_path。

  

10、为了能够将表当提交后，接收表单请求处理数据的资源能够提取出请求中的表单数据，必须给表单的各个带值组件添加name属性，name的值将是HttpServletRequest.getParameter的参数值，比如文本框。

  

11、如果Tomcat的server.xml配置文件中为某个web应用配置了Context，并且属性reloadable的值为true，那么当该web应用的源代码文件发生修改后，一般Tomcat都能够自动重加载，但是对于web.xml等配置文件的修改，Tomcat很大可能不会自动重新加载，需要手动重启服务器。

  

12、JSP不需要配置web.xml文件，并且修改JSP文件后也不需要重启服务器，访问的时候会自动重新解析JSP文件。

  

13、Servlet可以看做是嵌套了HTML代码的Java源文件，JSP可以看做是嵌套了Java代码的HTML文件。

  

14、form表单的form标签的两个属性action和method。  
1）action的值为表单需要提交到的目标资源的url（如Servlet），如果url为abs_path则为绝对地址，而实际上action的值可以为相对地址，即直接写上Servlet的在web.xml中的servlet-
mapping的url-pattern的值。提交的时候，Tomcat会自动在该表单所在的web应用中的web.xml中
搜索，需要注意的是相对地址的前面不能出现“/”符号，以该符号开头为绝对地址。使用相对地址的好处是Tomcat的server.xml文件中多web应用的配置别名随时修改，一旦修改就需要对所有绝对路径进行修改，而使用相对路径就不用。  
2）method属性的值为get时，表单以get方式提交请求；值为post时，表单以post方式提交请求。浏览器地址栏的请求一定是get方式提交。

  

15、HTTP协议的get方法和post方法的区别：  
1）使用get方法提交表单，表单的数据会以?name1=value1&name2=value2...的形式附加在abs_path后面，而post则不会。因此，使用get方法提交的表单数据可以在浏览器的地址栏上看到提交的表单数据内容。  
2）产生区别的原因在于get方法将附加的数据附加在请求的请求行中(GET abs_path?name1=value1&name2=value2...
HTTP/1.1）；而post方法是将附加的数据附加在请求的请求体中，它将附加的数据附加在请求体的末尾，也就是其它请求命令之后用两个CRLF隔开再附加数据。  
3）由于请求的请求行字符有限制，所以当提交文件的时候必须使用post方法，也提倡提交表单数据使用post增加安全性。  
4）服务器端不管浏览器使用的是哪一种请求方式，在服务器端都使用同一的方式对请求的附加数据进行处理（多态）。

