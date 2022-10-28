---
# type: posts 
title: "JavaWeb学习笔记 第十一记"
date: 2014-07-10T11:13:20+0800
authors: ["Jianan"]
summary: "1、Servlet监听器HttpSessionListener：实现javax.servlet.http.HttpSessionListener接口的类，主要包含两个方法：
    1）sessionCreated(HttpSessionEvent se)：当创建一个Session时调用。
    2）sessionDestroyed(HttpSessionEvent se)：当销毁一个Ses"
series: ["JavaWeb学习笔记"]
categories: ["JavaWeb学习笔记"]
tags: ["javaweb", "servlet listenter", "EL表达式", "JSP标签", "Properties"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 835
comment_num: 0
---

1、Servlet监听器HttpSessionListener：实现javax.servlet.http.HttpSessionListener接口的类，主要包含两个方法：

    1）sessionCreated(HttpSessionEvent se)：当创建一个Session时调用。

    2）sessionDestroyed(HttpSessionEvent se)：当销毁一个Session时调用，需要注意不是浏览器一关闭就调用，Session的生命周期是由Session的不活动最长时间决定。

    3）部署HttpSessionListener监听器需要在web.xml文件中布置<listener>标签。

  
2、Servlet监听器HttpSessionAttributeListener：实现接口javax.servlet.http.HttpSessionAttributeListener的类，主要包括三个方法：

    1）attributeAdded(HttpSessionBindingEvent se)：当使用Session对象的setAttribute方法添加一个Attribute时调用的方法。

    2）attributeRemoved(HttpSessionBindingEvent se)：当使用Session对象的setAttribute方法更新一个Attribute值时调用的方法。

    3）attributeReplaced(HttpSessionBinding se)：当使用Session对象的removeAttribute方法时调用的方法。

    4）部署HttpSessionAttributeListener同样需要在web.xml文件中使用<listener>标签布置。

  
3、EL表达式：为了简化JSP中的Java代码的表达式，以减少JSP中对JSP脚本的使用。

  
4、EL表达式语法：${ expr }。

  
5、“\${ expr }”：在EL表达式前添加“\”将使EL表达式无效，与普通文本一样。

  
6、EL的内置对象：

    1）PageContext：类型为javax.servlet.jsp.pageContext，表示此JSP的PageContext。

    2）pageScope：类型为java.util.Map，是取得Page范围的属性名称对应的值。

    3）requestScope：类型为java.util.Map，是Request对象使用setAttribute方法存储的Attribute对应的值。

    4）sessionScope：类型为java.util.Map，是Session对象使用setAttribute方法存储的Attribute对应的值。

    5）applicationScope：类型为java.util.Map，是Application对应使用setAttribute方法存储的Attribute对应的值。

    6）param：类型为java.util.Map，是客户端发出请求附加的数据param，等同于Request的方法getParamter(String name)获得的值。

    7）paramValues：类型为java.util.Map，是客户端发出请求附加的数据param，等同于Request的方法getParamterValues(String name)获得的值的数组（附加数据一个name对应多个值时需要这种这个对象）。确定paramValues中的哪个值需要使用[]运算符来确定。

    8）header：类型为java.util.Map，等同ServletRequest.getHeader(String name)获得的值。

    9）headerValues：类型为java.util.Map，等同于ServletRequest.getHeaders(String name)获得的值的数据，需要使用[]运算符具体确定数组中哪个值。

    10）cookie：类型为java.util.Map，等同于HttpServletRequest.getCookies()获得的Cookie数组，需要使用[]运算符。

    11）initParam：类型为java.util.Map，等同于ServletContext.getInitParameter(String name)获得的String值。

  
7、EL提供.和[]两种运算符来存取数据，两者基本一样，区别在于[]运算符中的索引可以是确定的值也可以是变量（变量灵活性比较高），并且当存取的属性名称中包含一些特殊字符（.或者-
等非字母或数字的符号）时，必须使用[]运算符。

  
8、当EL表达式中的属性名没有指明具体范围时，EL表达式默认会先中Page范围查找这个属性。找不到的话则依次查找Request、Session、Application范围，如果找到则不继续让下查找并返回值，如果都找不到则返回null。

  
9、EL支持大多数数学和逻辑运算符，如+、-、*、/等。当运算符处理的数据类型不一致时能够根据类型转换规则自动转换数据类型。

  
10、EL表达式也可以处理JavaBean：${ JavaBeanName.Attribute }。

  
11、EL表达式的内置对象除了pageContext之外，其它的内置对象都只能够获取对应Attitude的值，因为它们的类型只是java.util.Map。但是pageContext的类型是PageContext，通过pageContext.request.*类型的样式可以获取PageContext的其它对象来处理更复杂的操作。

  
12、JSP标签技术从JSP
1.1版本开始支持，目的是使JSP代码更加简洁，运用这些可重用的标签能处理复杂的逻辑运算和事务，或者定义JSP网页的输出内容和格式。

  
13、创建自定义JSP标签的步骤：

    1）创建标签的处理类。

    2）创建标签库的描述文件。

    3）在JSP文件中引入标签库，然后便可以在JSP使用自定义的标签。

  
14、创建标签的处理类：创建标签的处理类需要继承javax.servlet.tagext.TagSupport或者javax.servlet.jsp.tagext.BodyTagSupport两个类其中一个，BodyTagSupport本身就是TagSupport的子类。一般继承后都需要重写doStartTag和doEndTag两个方法。

  
15、TagSupport类中的主要方法：

    1）int doStartTag()：当Servlet容器解析JSP时遇到自定义标签的起始标签时调用的方法，其返回值有Tag.SKIP_BODY或者Tag.EVAL_BODY_INCLUDE两种，分别代表忽略自定义标签之间的内容和正常执行自定义标签之间的内容，一旦忽略则自定义标签之间的内容等于不存在。

    2）int doEndTag()：当Servlet容器解析JSP时遇到自定义标签的结束标签时调用的方法，其返回值同样有Tag.SKIP_PAGE和Tag.EVAL_PAGE两种，分别代表忽略结束标签之后所有的JSP页面内容并将已有的输出内容立刻返回到客户的浏览器上，和代表按照正常的流程继续执行后续的JSP页面内容。

    3）setValue(String k,Object o)：在标签处理类中设置一对键值。

    4）getValue(String k)：获得在标签处理类中的键名为k的value。

    5）removeValue(String k)：移除标签处理类中键名为k的键值对。

    6）setPageContext(PageContext pc)：设置pageContext对象，该方法在Servlet容器调用doStartTag或doEndTag方法之前调用。

    7）setParent(Tag t)：设置嵌套了当前标签的上层标签的处理类，同样该方法再Servlet容器调用doStartTag或doEndTag方法之前调用。

    8）getParent()：获得嵌套当前标签的上层标签的处理类。

  
16、TagSupport类的两个重要属性：

    1）parent：代表当前标签的上层标签的处理类（类中定义为私有类型，所以J2EE帮助文档中没有显示这个属性）。

    2）pageContext：代表Web应用中的javax.servlet.jsp.PageContext对象。PageContext提供了public void setAttribute(String name,Object value,int scope)和public Object getAttribute(String name,int scope)两个方法用来获取scope范围内设置的Attribute属性。其中scope的值有PageContext.PAGE_SCOPE（page范围内设置的Attribute）、PageContext.REQUEST_SCOPE（request范围内设置的Attribute）、PageContext.SESSION_SCOPE（session范围内实质的Attribute）、PageContext.APPLICATION_SCOPE（application范围内实质的Attribute）。

  
17、JSP自定义标签处理业务的关键在于访问到JSP中的application、session、request和page范围内的共享数据。为了实现，所以TagSupport类中提供了方法setPageContext和setParent方法并且这两个方法在doStartTag和doEndTag方法调用之前调用，以此来保证自定义标签处理类的业务逻辑开始之前处理类获得PageContext对象和标签的上层标签处理类对象。之后，在doStartTag和doEndTag方法中就可以通过getParent方法获得上层标签的处理类（parent属性是private，所以需要方法获得），或者直接访问pageContext成员（pageContext成员是protected，所以在子类中可以直接访问）。通过这两个对象就能访问到JSP中的共享数据。

  
18、自定义标签也可具有自己的属性，并且自定义标签的属性需要在对应的处理类中定义属性名的成员变量和它的get\set方法。使用自定义标签，并且标签中有属性时，处理类的set方法会被调用。

  
19、创建标签库的描述文件，其后缀名为“.tld”，本质上描述文件是一个有效的xml文件，使用DTD进行验证。因此，描述文件需要添加以下声明：

    
    
        <?xml version="1.0" encoding="UTF-8"?>
    
        <!DOCTYPE taglib PUBLIC "-//Sun Microsystems, Inc.//DTD JSP Tag Library 1.2//EN" "http://java.sun.com/dtd/web-jsptaglibrary_1_2.dtd">

  

20、标签库描述文件的DTD验证中规定了tld文件根元素为<taglib>。其中必须由子元素：

    1）<tlib-version>（int）tldversion</tlib-version>：表示标签库描述文件的版本号。

    2）<jsp-version>1.1</jsp-version>：JSP的版本号，一般都是1.1。

    3）<short-name>tlibname</short-name>：标签库的简称。

    4）<uri>tliuri</uri>：最终要的子元素之一，代表引用该标签库的uri地址。

    5）<tag>tag</tag>：必须有一个或多个tag标签，一个tag标签代表一个自定义标签，必须含有子元素<name>（表示自定义标签的标签名）、<tag-class>（表示自定义标签的处理类，元素内容为处理类的全称）。可选元素<body-content>（表示自定义标签是否具有内容，值为empty表示该标签为空标签）、<attribute>（表示自定义标签的属性，带有子标签<name>表示属性名，<required>表示该元素是否必须具有）。

  
21、JSP中引入自定义标签库，使用JSP命令<%@taglib uri="" prefix=""
%>。其中属性uri就是标签库描述文件中的子元素<uri>的内容，prefix为JSP页面中引用该标签库标签的前缀。 如<prefic:tag>。

  
22、自定义标签库的描述文件必须放在/WEB-INF文件夹下，因为Web应用可能被部署到不同操作系统平台上，借由每个Web应用都具有WEB-
INF文件夹的特点，将描述文件放置到这个目录下可以提高Web应用的跨平台性。而且，Tomcat默认也会自动到这个目录查找自定义标签库的描述文件。

  
23、自定义标签的运转流程：

    1）Servlet容器解析JSP文件，使用taglib命令引入自定义标签库。

    2）解析JSP遇到自定义标签时，从标签库的描述文件中查找tag元素的class。先调用setPageContext和setParent方法，以及标签存在属性的话调用处理类中对应属性的set方法。

    3）解析到自定义标签开始标签，调用处理类的doStartTag方法。

    4）解析到自定义标签结束标签，调用处理类的doEndTag方法。

    需要注意的是！自定义标签处理类的调用是在得到请求执行JSP调用，但是在JSP解析阶段是将JSP中的自定义标签解析为Servlet中调用处理类方法的代码。

  
24、PageContext对象提供了方法getOut()可以获得一个JspWriter对象，这个对象又提供了多个重载print方法，通过这些方法可以将对应的内容写到JSP页面上，从而最终输出到客户的浏览器。

  
25、Java的标准配置文件，后缀名为“.properties”，文件的内容是一行一行的key=value，每一行都是一个键值对表达式。

  
26、对应Java的标准配置文件，Java的java.util.Properties类对处理properties文件特别合适。Properties类继承Hashtable类，并且它的Key和Value都是String，符合properties文件的键值对类型。Properties提供了三个重要方法：

    1）load(InputStream inStream)：从输入流中读取属性列表键值对，当使用FileInputStream将properties文件读取进来加载如Properties对象，便可以通过Properties对象方便获得properties文件中的键值。

    2）getProperties(String key)：返回指定key的Value，类型为String。

    3）getProperties(String key,String defaultValue）：返回指定key的Value，类型为String。如果不存在这个key，则返回defaultValue。

  
27、自定义标签与properties的结合使用：可以解决Web应用应对不用语言的情况，根据客户端的默认语言，Web应用对应使用不用语言的properties配置文件。

  
28、使用properties配置文件，通常需要一个初始化的Servlet，在这个初始化Servlet的init(ServletConfig
config）方法中将properties配置文件的键值加载进Properties对象中。因此，这个Servlet需要在web.xml文件中不提供<servlet-
mapping>并且在<servlet>标签中添加子标签<load-on-startup>设置启动加载的优先级。

  
29、Servlet的init方法中加载properties文件的方法(ps为Properties对象）：

    
    
        ServletContext context = config.getServletContext();
    
        InputStream in = context.getResourceAsStream("/WEB-INF/*.properties");
    
        ps.load(in);
    
        in.close();
    
        context.setAttribute("ps",ps);//将Properties对象放置到ServletContext全局对象中供其他组件使用。

  
30、标签处理类的TagSupport类中定义pageContext成员变量，通过它的getSession()方法同样可以获得当前HttpSession对象，从而获得session的共享数据。

  
31、如果有以下代码：  

    
    
    <%
    
    String path = request.getContextPath();
    
    String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
    
    %>
    
    <base href="http://www.baidu.com">

那你下面的href属性就会以你上面设的为基准,如:<a href="http://www.baidu.com/xxx.htm"></a>你现在就只需要写<a
href="xxx.htm"></a>

