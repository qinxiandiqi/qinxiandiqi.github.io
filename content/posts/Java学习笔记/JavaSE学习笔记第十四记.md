---
# type: posts 
title: "JavaSE学习笔记 第十四记"
date: 2014-06-11T14:42:51+08:00
authors: ["Jianan"]
summary: "Java的xml解析"
series: ["Java学习笔记"]
categories: []
tags: ["Java", "工厂模式", "XML", "DOM", "SAX"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read: 803
---

1. 简单工厂模式：是一种创建类对象的模式，由一个工厂类根据传入的参数动态决定创建哪一个产品类的实例。

2. 简单工厂模式的角色：
    1. 工厂类角色：与用户直接打交道的角色，通常是一个具体类。该角色中包含一个静态工厂方法，用户通过该静态工厂方法传入一定的参数，静态工厂方法根据传入的参数创建对应的具体产品对象返回。
    2. 抽象产品角色：简单工厂模式要创建所有对象的父类，通常是一个接口或者抽象类。在工厂类的静态工厂方法中的返回类型就是抽象产品类。
    3. 具体产品角色：实际要创建的类对象，都继承或者实现抽象产品类，是一个具体类，一个简单工厂模式中一般具有多个具体产品类。

3. XML文档解析的两种接口规范：DOM和SAX。接口需要实现，不同编程语言对这两种接口规范的实现代码不同，但是实现的结果是一样的，实现完成的功能必须符合接口的规范。因此，可以说这两种接口规范是与语言无关，也与平台无关的。这也导致XML可作为不同语言、不同应用程序、不同平台之间交换数据的载体。

4. DOM是W3C指定的一套XML分析器的标准接口规范，全称为Document Object Model，即文档对象模型。基于DOM的XML分析器将一个XML文档转换为一个树形对象模型的集合，模型中的每一个节点都是一个对象。在应用程序中，就是通过DOM的XML分析器将XML文档进行树形结构的对象化，再通过DOM提供的接口来操作XML文档对象。可以说，XML文档代表的是数据，而DOM代表的是如何去处理这些数据。

5. 通过DOM接口可以在任何时候访问XML文档中的任何一部分数据（因为它们都读取到内存中），所以DOM接口的机制也称为随机访问机制。但另一方面，由于DOM是根据树形结构将XML的数据对象化，如果XML数据很多的时候，将会特别占用内存，遍历树的耗时也会比较高，这也是DOM的缺点。

6. DOM树模型结构：
    1. 文档，即根节点：文档节点是整个文档中所有其它节点的父节点，文档节点是整个文档的入口。备注：根节点不等于根元素节点，根节点之后才是根元素节点。
    2. 元素节点：跟据XML文档中对应的元素创建，元素节点可以拥有其它子元素节点，以及属性节点和文本节点。同时，元素节点也是唯一拥有属性节点的节点类型。
    3. 属性节点：根据XML文档中的属性创建，实际上也不认为它是元素节点的子节点。
    4. 文本节点：包含数据的节点，一般是对应XML文档中的元素内容。
    5. 其它不常见节点：CDATA、注释、处理指令。

7. DOM的四个基本接口：Node、Document、NodeList、NameNodeMap。其中Document接口是堆XML文档操作的入口，从Node接口中继承过来。Node是其它大多数接口的父接口，像Document、Element、Attribute、Text、Comment等代表一个XML元素或属性和内容的接口都是从Node中继承过来，因为DOM中把XML中的每个标签和属性都当做一个Node处理。NodeList接口是一些节点的集合，包含了某个节点的所有子节点。NameNodeMap接口也是一些节点的集合，可以建立节点名和节点之间的一一对应关系，一般用于元素和属性之间的对应关系。

8. Node接口：org.w3c.dom.Node，是其它XML树结构对应节点的父接口，在DOM树结构中具有重要地位，它定义一整套处理DOM节点的基本方法。Node借口中的主要方法：
    1. getElementsByTagName(String name)：该方法将返回一个NodeLise对象，对象中是该Node下所有名字为name的标签Node集合。
    2. getFirstChild（）：返回该Node下的第一个子节点的Node，如果不存在则返回null。
    3. getNodeValue（）：返回该Node的值，取决于其类型（见JDK帮助文档Node固定值表）。
    4. getNodeType（）：返回该Node类型代表的short值，Node类型与short对应表详见JDK帮助文档。
    5. getTextContent（）：返回该Node节点及其子节点的文本内容，根据Node具体类型的不同将采用不同的内容拼接方法（见JDK帮助文档），特别的，子节点之间的空格当做一个空白的Text节点。

9. Document接口：org.w3c.dom.Document，继承Node接口，代表了整个XML文档，XML其它Node都按照一定的顺序包含在Document节点对象内，并排雷成一个树形结构。因此，要对XML文档进行操作总是要先通过解析XML文档获得该文档的而一个Document对象才能继续后续操作。由于Document接口中定义了大量创建其它DOM节点对象的工厂方法，也提供了大量操作其它DOM节点对象的方法，从而实现成为访问XML文档的入口。Document接口中的主要方法：
    1. getDocumentElement（）：获得该XML文档的根元素Element对象返回。
    2. 

10. NodeList接口：org.w3c.dom.NodeList（没有继承Node接口）。NodeList相当于一个Node的集合，该接口中只提供了两个方法：
    1. getLength（）：获取NodeList中Node的个数。
    2. item（int index）：获取NodeList中索引号为index的Node对象，索引从0开始计算起。

11. Element接口：org.w3c.dom.Element，继承Node，代表XML文档中的元素，是DOM中唯一可以包含属性的节点。主要方法有：
    1. getTagName（）：获得该Element元素的标签名返回。
    2. getChildNodes()：获得该Element的子元素Node的集合NodeList返回，需要注意的是该方法将Element下子元素标签之间的空格也当做一个空Text的Node，所以此时使用NodeList的getLength方法返回值包括空格数在内。
    3. getAttributes（）：返回一个NameNodeMap对象，其中包含该Element的Attr节点对象。

12. Attr接口：org.w3c.dom.Attr，继承Node，代表XML文档中元素的属性。虽然它是Node，严格来说，它不能作为一个DOM树中的节点，因为Attr实际上是包含在Element中，所以并不能看做是Element的子对象。也正是因为这原因，它从Node继承来的getparentNode（）、getpreviousSibling（）、getnextSibling（）方法的返回值都是null。

13. Text接口：org.w3c.dom.Text，继承Node接口，代表XML文档中元素的文本内容，在DOM树结构中，元素的文本内容也使用Node，并且它不能有子节点，继承Node的相关操作不能使用。Text节点的NodeName为“#text”。

14. 接口都需要实际类的实现，所以以上Node等接口在实际上是需要通过java其它包下的一些实际类来实现，而用于一般不需要直接与这些实际实现类打交道，通过接口及工厂模式的方法来实现DOM接口。

15. XML解析器实质上就是一段代码，它能够读入一个XML文档并分析文档的结构。DOM的解析器通过解析XML文档就能为XML文档在逻辑上建立一个树模型，其树的节点是一个个对象，通过DOM接口存取这些对象就能存取XML文档的数据内容。

16. JAXP（Java API for Parsing）：用于XML解析的Java API，由SUN公司提供。

17. Java应用程序使用XML的一般步骤：
    1. javax.xml.parsers.DocumentBuilderFactory抽象类：通过DocumentBuilderFactory解析器工厂类的静态newInstance()方法创建一个DocumentBuilderFactory解析器工厂类对象dbf（DocumentBuilderFactory既是工厂类角色又是抽象产品角色）。newInstance()方法能根据一个系统变量（决定构建哪一种具体产品类的参数）来决定创建哪一种解析器工厂对象，只要该系统变量改变就能创建出不同的解析器。这种做法的好处是：无论最终生成的是哪一种解析器，它都符合JAXP的规范，所以只要改变系统变量就能创建出不同的解析器工厂对象，最终创建出另一种解析器，而程序照常可以使用新解析器（JAXP接口不变），不需要改变任何代码。
    2. 通过DocumentBuilderFactory类对象dbf，通过它的newDocumentBuilder()方法创建一个它对应的具体DocumentBuilder解析器对象db，也就是这个db就代表了一个具体的DOM解析器。
    3. 通过DocumentBuilder对象db的parse（type）解析一个XML文档，该方法将返回一个Document对象，也就是该XML文档的根节点。parse是重载方法，有多种类型参数可选，一般使用parse（String url）。
    4. 通过Document对象以及它提供的各种Node方法实现对XML的操作。
