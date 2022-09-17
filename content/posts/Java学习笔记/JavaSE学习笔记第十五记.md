---
# type: posts 
title: JavaSE学习笔记第十五记
date: 2014-06-11T14:50:37+08:00
authors: ["Jianan"]
summary: "Java的XML解析"
series: ["Java学习笔记"]
categories: []
tags: ["Java", "XML"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read: 760
---

1. NameNodeMap接口：org.w3c.dom.NameNodeMap，其中保存了一个字符串名字与一个Node对象的映射，一般是Attr属性Node。虽然可以通过正常的索引访问NameNodeMap中的Node，但NameNodeMap中的Node实际是没有顺序的。NameNodeMap中主要方法：
    1. item（int index）：返回索引值为index的Node。
    2. getNamedItem（String name）：返回对应字符串为name的Node对象。

2. Comment接口：org.w3c.dom.Comment，继承Node，代表一个注释。主要方法有：getData（）：返回该Comment注释内容的字符串。

3. SAX（Simple APIs for XML）：即XML简单应用程序接口，另一种解析XML文档的一种接口规范。

4. SAX的工作模式：SAX提供的访问模式是一种顺序访问模式，并且是基于观察者模式的事件驱动机制来解析访问XML文档。SAX在解析一个XML文档的时候，它并不把整个XML文档都读进内存，而是按照XML文档的“行”为单位来解析。一行XML语句解析完后就继续解析下一行，被解析后的XML语句并不将其数据保存在内存中，所以SAX的解析无法再返回读取解析过的内容，除非重新解析整个XML文档，因此SAX的解析不是随机而是顺序性的。SAX在逐行解析的时候，会分析XML语句的语法，判定当前字节是XML与语法中的哪一部分，是否符合XML的语法要求，然后就会根据解析到的信息触发相应的事件给监听 器或适配器捕捉，XML数据的处理交给监听 器或适配器处理。类似于GUI的事件机制，SAX解析时触发的事件给监听 器或适配器捕捉后，处理方法的具体代码是由应用程序提供的，监听 器或适配器只是提供了相应事件处理方法的接口。之所以说SAX是简单应用程序接口也正是因为它只做了分析XML的一些简单工作，具体怎么处理交由应用程序自己控制。

5. DOM与SAX的区别：DOM对XML文档的解析是可以实现数据的随即存取的，因为DOM把整个XML文档一次性读取到内容中构建了一个DOM对象树，基于DOM这个实现模式，DOM比较损耗内存，当XML过于庞大的时候可能会导致内存溢出。SAX对XML的解析是顺序访问的，基于事件处理机制，并不把整个XML文档都读取进内存，基于SAX的实现模式，SAX分析器实现比较简单，对内存的要求比较低，是实现的效率比较高，对于只读取XML文档中的数据而不对其进行更改的应用程序来说，SAX显然更加适合。

6. Java对SAX的实现过程：
    1. 同样需要先构造SAX解析器工厂对象，与DOM相似，也是基于简单工厂模式来构造SAX解析器工厂对象。Java提供了抽象类javax.xml.parsers.SAXParserFactory的静态方法newInstance()来返回一个根据当前系统环境变量参数构造的SAXParserFactory解析器工厂对象，也就是说SAXParserFactory类同样承担了既是工厂类又是产品类的角色。
    2. 通过SAXParserFactory的静态方法newSAXParser()返回一个具体的SAX解析器对象javax.xml.parsers.SAXParser。
    3. 通过SAXParser对象的parse()方法解析一个XML文档，SAXParser类提供了多个重载parse方法（详见JDK帮助文档），常用parse(File file ,DefaultHandler dh)方法解析XML文档file。
    4. parse方法中的org.xml.sax.helpers.DefaultHandler类实现了ContentHandler、DTDHandler、EntityResolver、ErrorHandler等监听 器接口，是捕捉SAX解析器触发事件的适配器。类似于GUI的各种Adapter适配器的使用方法，其中提供了多个相应事件的处理方法供用户重写，如：遇到文档开始时促发startDocument()方法，遇到元素开始标签的时候触发startElement()方法，遇到元素之间字符内容时候触发characters()方法，遇到元素结束标签时候触发endElement()方法，遇到文档结束的时候触发endDocument()方法，详细见JDK帮助文档。

7. Stack栈类的peek方法是取出栈顶的值，但是不从栈中弹出该值。

8. 见张龙老师毕生心血之Schema学习总结pdf文档！！

9. JAXP是用于对XML语法分析的Java APIs，包含了三个包：
    1. org.w3c.dom，W3C推荐的用于XML标准规范的文档对象模型的Java工具。
    2. org.xml.sax，用于对XML进行语法分析的事件驱动的简单API。
    3. javax.xml.parsers，工厂化的工具，允许应用程序开发人员获得并配置特殊的语法分析器工具。

10. JAXP对Java的缺点：
    1. 可以说JAXP是完全按照DOM和SAX的规范在Java语言上的实现，DOM和SAX规定什么接口以及接口有什么功能，JAXP就完全按照这些固定在Java语言下进行实现。然而，DOM和SAX规范根据接口定义语言来写的，它们的任务是在不同语言实现中定义一个最低的通用标准，也就是不是专为Java设计的。这种设计方法虽然能够保留了在不同语言中非常相似的API，但是在Java中会让程序员感觉到麻烦，因为DOM和SAX的规则并不完全对应Java语言的规则，比如Java对于字符串有String类，但是DOM规范定义自己的Text类。
    2. 在实际应用中，SAX没有文档修改、随机访问以及输出的功能；DOM的树形层次结构非常严格，对应XML在DOM中每个元素都是一个节点，几乎都扩展与Node接口和返回Node的一系列方法，但是在Java语言中应用部方便，比如Node向叶类型做向下类型转换需要的代码冗长并难以理解。
    3. W3C DOM是接口驱动的，公共的DOM API仅由接口组成，使用时w3c标准大量使用工厂化的类和类似的灵活性但不直接的模式，例如解析器需要工厂模式创建。

11. JDOM：专为Java语言开发的一个使用XML的工具，是一个开源项目，为解决Java直接使用DOM和SAX接口的JAXP带来的不方便而研发。在具体使用上是一个第三方开发的类包。

12. JDOM基于树形结构，用纯Java的技术对XML文档实现解析、生成、序列化以及其它多种操作，是直接为Java编程服务的。相比较JAXP，它利用Java语言强有力的诸多特性（方法重载、集合概念等），把SAX和DON的功能有效的结合起来，提供了用Java语言读写和操作XML的新API。这些API函数在直接、简单和高效的前提下，尽可能的隐藏了原来DOM和SAX使用XML过程中的复杂性，进行了最大限度的优化。

13. JDOM的优点：
    1. JDOM是Java平台专用的，给Java程序员提供丰富并且和Java语言类似的环境。比如，JDOM基本都使用String类处理XML中的文本值，使用JDOM还可以利用Java的一些累计，如List等。
    2. JDOM没有层次性，对应XML中的各个部分，元素就是Element类对象，属性就是Attribute类对象，XML文档本身就是Document类对象。也就是说，在JDOM中，每一种XML部分都有具体类对应，而不是作为一个含糊的Node。
    3. JDOM是类驱动的，JDOM的对象就是像Document、Element和Attribute这些类的对象，所以创建一个新的JDOM对象就跟Java语言中用new构造一个对象那么容易，使用起来比使用工厂化接口配置更来得直接。

14. JDOM类包的组成：
    1. org.jdom，包含了所有xml文档要素的java类，如Document等。
    2. org.jdom.adapters，包含了与dom适配的java类。
    3. org.jdom.filter，包含了xml文档的过滤器类。
    4. org.jdom.input，包含了读取xml文档的类。
    5. org.jdom.output，包含了写入xml文档的类。
    6. org.jdom.transfrom，包含将jdomxml文档接口转换为其它xml文档接口。
    7. org.jdom.xpath，包含了对xml文档xpath操作的类。

15. 方法链编程风格：基于调用一个对象的方法后，方法的返回值是调用的对象。这么做的好处就是可以在方法后面直接继续调用该对象的其它方法，JDOM中类的方法大都支持这种风格。如：element.addContent(elementother).setAttribute("name","value")。

16. JDOM常用org.jdom.output.XMLOutputter类的output方法来将程序新建或更改的内容写入到指定的XML文档中。

17. JDOM的org.jdom.output.Format类用于设置输出XML文档的格式，XMLOutputter类的构造方法可以接收一个Format对象。

18. JDOM的org.jdom.input包下的SAXBuilder类、DOMBuilder类、ResultSetBuilder类常用与解析XML文档。

19. JDOM解析一个XML文档后，并在程序中进行了修改，这些修改只是保存在内存中相应的对象里，并没有直接修改到解析的XML文档中。如果要将修改保存到XML文档中，需要使用XMLOutputter对象将Document对象写会才会保存到文档中。

20. JDOM的Format类的getRowFormat方法可以去除XML文档中不必要的空白，这也是XMLOutputter类默认的格式，这种格式可以减少XML在网络上传输的数据量，从而提高传输效率。

21. dom4j：由第三方开发的另一种专为Java语言处理XML文档的类包，同样提供了大量符合Java语法习惯的API供开发者使用，而且大部分API的使用方式与JDOM类似（开发dom4j的成员很多是开发JDOM的成员）。与JDOM不同的是，Document、Element、Attribute等不是类，而是接口。创建Document、Element、Attribute等接口对象是由org.dom4j.DocumentHelper类对象的静态成员方法闯将，DocumentHelper类提供了大量创建这些接口对象的static方法，可以说，DocumentHelper类就是一个辅助类。

22. dom4j将Document输出到XML文档是通过org.dom4j.io.XMLWriter类来实现，其中有多种构造方法，可以接收XML文档的输出流，如果输出流是一个Writer流的话，在使用write(Document document)之后，需要调用XMLWriter的flush方法或者close方法将缓冲输出才会将document的XML数据输出到文档上。

23. dom4j解析一个XML文档通过org.dom4j.io.SAXReader类的read方法解析，read方法返回一个Document接口对象，通过Document可以访问XML文档中的各种部件。

24. org.dom4j.io.DOMReader类同样用于解析XML文档，该类的read方法可以接收一个org.w3c.dom.Document对象，并返回一个org.dom4j.Document对象，也就是将一个JAXP的Document对象转换成dom4j的Document对象，由于在同个源文件中使用两个同名类，所以类名需要使用全名。

25. 开发dom4j应用程序详见dom4j帮助文档。
