---
# type: posts 
title: "JavaWeb学习笔记 第十二记"
date: 2014-07-10T11:18:37+0800
authors: ["Jianan"]
summary: "1、设置临时域名：Windows系统下C:\\WINDOWS\\system32\\drivers\\etc\\hosts文件，最后添加“IP地址 临时域名”，保存后即可在本机使用临时域名访问设置IP地址的主机。

2、Tomcat设置虚拟主机：Tomcat目录的conf目录中server.xml文件中

    子标签Host即为设置Tomcat虚拟主机，对应域名到对应的应用目录中。

3、E"
series: ["JavaWeb学习笔记"]
categories: ["JavaWeb学习笔记"]
tags: ["javaweb", "tomcat", "虚拟主机", "servlet"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 761
comment_num: 0
---

1、HttpServletResponse的对象resp的以下代码：

    
    
        resp.setContentType("text/xml;charset=utf-8");
        resp.setHeader("pargma","no-cache");
        resp.setHeader("cache-control","no-cache");

    分别用于通知客户浏览器服务器响应的数据媒体格式为text/xml，字符编码集为utf-8，浏览器不使用响应缓冲。对应使用Ajax更新数据的页面需要此设置，缓冲可能会影响页面数据的更新。

  
2、Ajax接收到服务器响应的xml数据时，接收到的是xml的DOM对象，将DOM对象转换为jquery对象就可以使用jquery中xml对象的find("element")方法获得xml文件中相应的元素节点对象，再使用.text()方法就能获得节点对象的内容。如：$.ajax()方法中的回调方法中$(returnedData).find("element").text()。

  
3、google提供使用ajax和json格式的image搜索api开发文档：https://developers.google.com/image-
search/v1/jsondevguide。实际上就是google提供了一组搜索图片的api网络服务，通过这些api访问google，google将搜索结果以json的形式返回。这个搜索图片的api为：https://ajax.googleapis.com/ajax/services/search/images?，只要在这个url后加上开发文档中指定的其它参数就能够获得google服务返回的结果。如必须的参数v=1.0（表示api的版本）、q=""（表示搜索的关键字）、start=""返回从搜索结果的第i幅图片开始的8幅图片信息）、rsz="1-8"（设置返回图片的大小级别）等等。google的图片搜索每次请求能够返回的图片json信息只有8组。

  
4、Java使用google api获取搜索图片服务结果过程（queryUrl为获取google服务的API搜索url）：

    
    
        URL url = new URL(queryUrl);
        URLConnection conn = url.openConnection();
        InputStream is = conn.getInputStream();
        InputStreamReader isr = new InputStreamReader(is);//返回结果是json字符串
        BufferedReader br = new BufferedReader(isr);
        StringBuffer buffer = new StringBuffer();
        String line = null;
        while(null != (line = br.readLine()){
            buffer.append(line);
        }
        br.close();
        isr.close();
        is.close();

    最终buffer.toString()获得字符串就是google搜索返回json结果字符串。

  
5、将使用指定url文件保存到本地的方法（fileUrl为指定文件的url）：

    
    
        URL url = new URL(fileUrl);
        InputStream is = url.openStream();
        OutputStream os = new FileOutputStream(new File("path"));
        int length = -1
        byte[] buffer = new byte[bufferSize];
        while(-1 != (length = is.read(buffer,0,bufferSize))){
            os.write(buffer,0,length);
        }

  

6、req.getSession().getServletContext().getRealPath("webFile")用于获得请求资源所在web应用的webFile资源在服务器上的真实文件系统路径。  
  

7、设置临时域名：Windows系统下C:\WINDOWS\system32\drivers\etc\hosts文件，最后添加“IP地址
临时域名”，保存后即可在本机使用临时域名访问设置IP地址的主机。

  
8、Tomcat设置虚拟主机：Tomcat目录的conf目录中server.xml文件中

    
    
        <Engine name="Catalina" defaultHost="localhost">
    
            <Host name="www.nan.com" appBase="././."/>
    
            <Host name="www.qinxiandiqi.com" appBase="././."/>
    
        </Engine>

    子标签Host即为设置Tomcat虚拟主机，对应域名到对应的应用目录中。

  
9、Eclipse中，Debug模式下，使用F6但不运行程序并在下一处断点位置暂停；F5单步运行程序。变量信息窗口中没有想要查看的变量，在代码窗口中选中某个变量后右击弹出快捷菜单，选择watch选项，右上方窗口中就会出现该变量。

  
10、Servlet除了doGet和doPost两个常用方法之外，还有getLastModified(HttpServletRequest
requset)常用方法，用于返回该文档的最后修改时间，默认为-1，表示该文档永远都是最新的。当Tomcat执行Servlet时，在调用doGet之前会先调用gatLastModified。此时，如果浏览器发现getLastModified返回值与上次访问时的返回值相同，则浏览器认为该文档没有更新，采用缓存而不要求执行doGet；如果getLastModified返回-1，则认为时刻都是最新的，总是要求执行doGet。

