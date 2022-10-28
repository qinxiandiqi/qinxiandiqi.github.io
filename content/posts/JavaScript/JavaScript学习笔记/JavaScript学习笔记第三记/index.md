---
# type: posts 
title: "JavaScript学习笔记 第三记"
date: 2014-07-11T10:26:02+0800
authors: ["Jianan"]
summary: "1、路径中..表示上一层路径。

2. JavaScript编码规范：通常在JavaScript不希望外界访问的成员和方法名以下划线开始。

3. jsUnit测试函数的要遵循的规则与JUnit 3.8类似（比如说测试函数名以test开头等）。

4. 对于JsUnit来说，其setUp和tearDown方法与JUnit的运行原理是不同的，JUnit中的setUp和tearDown之间"
series: ["JavaScript学习笔记"]
categories: ["JavaScript学习笔记"]
tags: ["javascript", "ajax", "xmlhttprequest", "jquery", "JsUnit"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 658
comment_num: 0
---

1、路径中的..表示上一层路径。

  
2\. JavaScript编码规范：通常在JavaScript中不希望外界访问的成员和方法名以下划线开始。

  
3\. jsUnit测试函数的要遵循的规则与JUnit 3.8类似（比如说测试函数名以test开头等）。

  
4\.
对于JsUnit来说，其setUp和tearDown方法与JUnit的运行原理是不同的，JUnit中的setUp和tearDown之间是没有关系的，也就是说不同的测试方法运行在不同的测试对象之中，而JsUnit的各个测试函数是运行在同一个测试页面中。因此setUp和tearDown会针对同一个变量进行操作。

  

5、JsUnit中对应于JUnit4.x中的@BeforeClass注解（整个测试之前执行一次）的方法为setUpPage，要求方法块中最后一行代码必须为setUpPageStatus
= "complete";，这行代码告诉JsUnit方法已经执行结束，否则测试肯定超时不通过。然而，JsUnit中没有对应@AfterClass的方法。

  

6、Ajax（Asynchronous JavaScript and
XML）：异步的JavaScript和XML，通过Ajax可以实现保持当前页面发出HTTP请求与服务器交互后，依据返回的响应只更改当前页面的一部分内容。  
  
7、Ajax实现的关键在于浏览器提供了XMLHttpRequest对象，但是不同的浏览器对XMLHttpRequest的支持不同，主要是IE和其它浏览器的区别。为了让代码能兼容不同的浏览器，获得XMLHttpRequest对象一般使用以下方法：  

    
    
        var xmlHttpRequest = null；
        //检测是否是IE浏览器，IE提供了ActiveXObject对象支持XMLHttpRequest。
        if(window.ActiveXObject){
            xmlHttpRequest = new ActiveXObject("Microsoft.XMLHTTP");
        }
        //除了IE之外的其它浏览器直接提供XMLHttpRequest实现XMLHttpReques对象。
        else if(window.XMLHttpRequest){
            xmlHttpRequest = new XMLHttpRequest();
        }

  
8、使用Ajax向服务器发送请求，需要先使用XMLHttpRequest对象的open方法设置HTTP请求方式和请求资源，以及XMLHttpRequest对象onreadystatechange属性关联响应的数据处理方法，最后再使用XMLHttpRequest对象的send方法向服务器发送数据。另外，由于有些小众化的浏览器不支持Ajax，没有提供XMLHttpRequest对象，所以这部分代码最后放在if(null
!= xmlHttpRequest){}判断语句中。  
    1）open(method,url,asyn,username,password)：method参数表示发送HTTP请求的方法（GET或者POST）；url为请求的服务器组件，如果是使用GET方法，附加的数据要写在url后面（与浏览器使用GET提交附加数据的格式一样）；asyn为boolean值，为true表示使用异步操作，为false表示使用同步操作，IE支持同步，FireFox不支持同步，因为使用同步就失去了Ajax的意义。  
    2）onreadystatechange：为该属性关联一个Function方法对象，也就是Function对象名后不能加括号，加了括号表示执行这个function方法。这个方法会在请求状态改变的时候被调用，XMLHttpRequest.readyState属性用于表示当前的状态。发送一个请求并得到响应的过程中一共有4中状态，各用1-4表示：1表示XMLHttpRequest对象刚刚构造完毕，但是还没有向服务器发送请求；2表示开始向服务器发送请求，但还没有收到响应；3表示开始接收到服务器的响应，但响应还没有完全发送过来；4表示已经完全接收到服务器的响应。  
    3）send（String elements）：向服务器发送请求，elements参数表示请求的附加数据。当使用GET方法时，不需要参数，因为GET方法的参数附加在请求资源后面，此时可以不写参数或者使用null做参数。当使用POST方法时，有附加数据的话需要将附加数据的字符串形式作为参数，附加数据的字符串格式与GET方法附加在请求资源后面的参数格式一样。  
  
9、服务器返回XMLHttpRequest请求的响应后，获得服务器响应的数据需要通过XMLHttpRequest的两个属性：  
    1）XMLHttpRequest.responseText：服务器直接通过字符串返回响应数据时使用这个属性可以获得响应的字符串数据。  
    2）XMLHttpRequest.responseXML：服务器以XML文档的格式返回响应数据的时候，通过这个属性获得响应的XML文档。  
  
10、Servlet设置Response对象不使用缓存的方法，添加以下代码：  
    resp.setHeader("pragma","no-cache");  
    resp.setHeader("cache-control","no-cache");  
  
11、XMlHttpRequest使用POST方式提交附加数据，需要在send方法之前设置请求头信息，将请求数据编码方式设置为表单编码方式：XMLHttpRequest.setRequestHeader("Content-
Type","application/x-www-form-urlencoded")。  
  
12、jquery是一个js库，目的是为了简化JavaScript的使用，通过jquery只需要使用简单的代码就可以完成很复杂的工作，原因在于jquery库的JavaScript已经完成了很复杂的工作。  
  
13、使用jquery需要在页面中导入jquery库，<script type="text/javascript" src="jqueryurl"
/>，jqueryurl为jquery的js文件路径。  
  
14、jquery库中提供了与DOM相对应的内置对象，jquery大部分将DOM对象中的属性包装为方法，并采用方法链的设计模式设计方法。使用jquery就是通过使用jquery对应DOM对象的对象方法，因此，DOM对象可以转换为jquery对象。另外jquery对象也可以转换为DOM对象。转换的方法有：  
    1）直接从页面获取DOM对象的方法：通过document对象的getElementByID或者getElementsByTagName等方法获得。  
    2）直接从页面获取jquery对象的方法：$("TagName")获取标签名为TagName的jquery对象数组；$("#ID")获取此ID的jquery数组，但实际上只获取第一个此ID的标签元素；$(".Element")获取CSS对象名为Element的jquery对象。特别需要注意的是获取的jquery对象是个数组，即使是页面中只有一个对象也是以数组的形式返回。操作获取的jquery对象实际上操作的整个数组的中的每个元素，jquery底层的代码就是遍历数组中每个元素来操作的。  
    3）DOM对象转换为jquery对象：$(DOM对象)。$()类似于强制转换，将DOM对象强制转换为jquery对象，转换后的对象就可以使用jquery对象的提供的非常便利的方法。  
    4）jquery对象转换为DOM对象：直接从页面获取的jquery对象都是以数组对象，使用“对象[i]”或者“对象.get(i)”获得的元素就是DOM对象。  
  
15、jquery对象的html()方法对应DOM对象的innerHTML属性，都是获得该对象的内容。  
  
16、jquery对象的click(function(){})方法对应DOM对象的onclick属性，在点击事件触发的时候得到调用。  
  
17、jquery的$(document).ready(function(){})对应DOM的window.onload属性，区别在于jquery的多个ready方法都会在页面加载完毕后全部按照顺序执行，而DOM的onload属性关联的function方法再页面加载完毕后只执行最后一个onload关联的function。原因在DOM的onload属性是对象，后一个关联Function对象会覆盖前面Function关联对象，所以得不到调用。  
  
18、jquery中获取的jquery对象是个数组，即使页面中不存在符合条件的标签，也会获取到一个jquery数组，只是这个数组是个空数组。对这个jquery对象进行操作，如果是个空数组则jquery会自动过滤掉不执行操作；相比较之下，如果页面标签不存在，对获取的DOM对象进行操作将抛出错误。每个jquery对象都有属性length代表该对象中的标签元素个数。  
  
19、jquery对象提供关于操作标签属性的方法一般都具有一个参数和两个参数的版本，带两个参数的方法一般都用于设置属性的值（第一个参数为属性名，第二个参数为新的属性值），带一个参数的方法一般用于返回属性的值。如jquery对象的css()方法，它具有css(type,value)用于设置css样式属性type的值为value，以及css(type)用于获取css样式的属性type的值。  
  
20、对于css中很多使用“-”连接符的属性（如background-
color），在JavaScript中必须取消“-”连接符并将连接符的下一个字母改为大写（如backgroundColor）。  

  

