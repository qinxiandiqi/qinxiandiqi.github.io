---
# type: posts 
title: "JavaScript学习笔记 第一记"
date: 2014-07-10T11:46:44+0800
authors: ["Jianan"]
summary: "1、JavaScript是一种可以嵌入在Web页面中的面向对象和事件驱动的解释性语言，使用动态绑定（在运行时才对对象进行检查）。

2、JavaScript区分大小写，语句以行为单位，可以加“;”或者不加。

3、Web页面中嵌入JavaScript需要使用标签，可以嵌入到页面中的任何位置。

4、使用JavaScript的基本格式：
    1） document.write(\"H"
series: ["JavaScript学习笔记"]
categories: ["JavaScript学习笔记"]
tags: ["javascript", "function", "类"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 852
comment_num: 0
---

1、JavaScript是一种可以嵌入在Web页面中的面向对象和事件驱动的解释性语言，使用动态绑定（在运行时才对对象进行检查）。

  
2、JavaScript区分大小写，语句以行为单位，可以加“;”或者不加。

  
3、Web页面中嵌入JavaScript需要使用<script>标签，可以嵌入到页面中的任何位置。

  
4、使用JavaScript的基本格式：

    1）<script> document.write("Hello World");</script>

    2）<script language="JavaScript">document.write("Hello World");</script>

    3）<script language="JavaScript" type="text/JavaScript"> document.write("Hello Wrold");</script>

    4）<script language="JavaScript1.2">document.write("Hello Wrold");</script>

    5）引用外部js文件：<script src="js-url"></script>。在外部js文件中不需要使用<script>标签，直接写JavaScript语句，引用后将自动把js文件中的JavaScript语句包含到<script>中进来。

  
5、JavaScript可以嵌入到Web页面的任何位置，执行顺序是<head>标签中的JavaScript代码在加载Web页面的时候被装载进来，但是其中的function不执行，必须调用后才会执行。<body>中的JavaScript一旦加载便立刻执行。通常将定义function和全局变量的语句放到<head>标签中。

  
6、HTML中的某些标签使用JavaScript协议调用JavaScript语句，如href="JavaScript:alert('Hello
World')" 等同于 href="#" onClick="alert('Hello World')"。

  
7、对于JavaScript变量，没有定义在function中的变量为全局变量，使用var声明定义在function中的变量为局部变量，但是在function中没有使用var声明直接使用的变量也是全局变量。

  
8、JavaScript的with语句：with(对象){...}。with语句是对象操作的简化语句，with后的括号内为一个对象，中括号中可以直接使用对象的内容，相当于默认在中括号中使用的方法或变量前添加了“对象.”。

  
9、for(var i in object)：遍历对象object中所有的属性，其中i为每个属性的属性名，可以使用object[i]获得i属性的值。

  
10、JavaScript不像Java语言有很规范的定义类语法，JavaScript中定义对象使用function，function
name(parmes){}类似于Java中的类的构造方法。使用function就等于声明了一个类，通过new
name就可以构造一个该类型的对象。由于JavaScript弱类型的特性，在function中使用this.AtributeName就等于为该类声明一个属性名为AttributeName的属性，并且类型不限（JavaScript中没有强的类型规范）。

  
11、JavaScript内置对象Date，使用new Date()可以构造一个Date对象。Date中主要方法：

    1）getYear()：返回距离1900年的年数，获得当前年份需要在返回值的基础上加1900.

    2）getMonth()：返回当前月份数，范围为0-11，因此需要在返回值的基础上加1.

    3）getDate()：返回当前的日期数，范围为1-31.

    4）getDay()：返回星期数，范围为0-6.

    4）getHours()：返回当前的时数，范围为0-23.

    5）getMinutes()：返回当前的分钟数，范围为0-59.

    6）getSeconds()：返回当前的秒数，范围为0-59.

  
12、JavaScript内置对象Array，使用new Array(数组长度)或new
Array(元素1，元素2...)或者“数组名称=[元素1，元素2...]”构造Array数组对象。JavaScript中的Array类似于Java中的集合，因为它的长度不限制（当长度不够时，Array会自动扩容），存储的元素类型也不限制（JavaScript只用一种类型标识符var）.

  
13、JavaScript内置对象Array的主要属性和方法：

    1）length：Array表示数组长度的属性。

    2）join(分隔符)：方法，将数组元素组合成字符串，并使用参数分隔符分隔每个元素，不提供参数时默认使用“,”分隔符。

    3）toString()：方法，返回数组的字符串，以“,”为元素分隔符，相当于使用join()方法。

    4）reverse()：方法，将数组的元素倒序，这种修改直接在原数组上修改，也就是改变将影响原数组。

    5）valueOf()：方法，返回数组值，相当于使用join()方法。

    6）push()：方法，向数组中添加一个元素。

    7）pop()：方法，移除数组中一个元素。

  
14、当Array数组的元素又是一个Array数组时，这个Array数组便成为多维数组。

  
15、JavaScript的document.write()方法中参数组合多个数据，可以使用“+”运算符，也可以使用“,”运算符，两者没有区别。

  
16、JavaScript的内置对象String，使用new
String(字符串常量)或者“字符串变量="字符串常量"”构造String对象，主要的属性和方法有：

    1）length：属性，String字符串的长度。

    2）charAt(索引)：方法，返回索引位置的字符。

    3）indexOf("字符串")：返回字符串在对象中的索引位置。

    4）lastIndexOf("字符串")：返回字符串在对象中的索引位置（反向搜索）。

    5）replace("字符串1","字符串2")：使用字符串2替换字符串1。

    6）search("字符串")：返回字符串在对象中的索引位置。

    7）subString(索引1,索引2)：返回索引1到索引2的子字符串（不包括索引2位置上的字符）。

  
17、定义自己的JavaScript类需要使用function，在JavaScript中的this不一定就是指当前对象，也可以是使用当前对象的对象。在function中，使用this.AttributeName定义类的属性，同样也是使用this.MethodName定义类的方法，所不同的是这个MethodName是另外一个function定义的类，这个类将当做一个方法使用。使用类方法的时候，根据类名.MethodName()就可以使用类中的方法。

  
18、JavaScript是基于事件驱动的，在很多HTML标签中添加属性onClick、onMouseOver、onMouseOut等，属性值为一个JavaScript表达式，当接收到相应的事件后边会触发对应的JavaScript处理方法。

  
19、JavaScript的定时器：

    1）内置window对象的setTimeout()方法返回一个定时器对象。使用格式为：定时器对象名称=setTimeout("<表达式>",time毫秒数)。这个定时器对象将在time毫秒后执行<表达式>的内容。

    2）内置window对象的setInterval()方法返回一个循环定时对象。使用格式为：定时器对象名称=setInterval("<表达式>",time)。这个定时器对象将每隔time毫秒执行一次<表达式>的内容。

    3）window对象的clearInterval(定时器对象名称)方法用于终止定时器对象。

  
20、document.getElementById(tagID).innerHTML用于获取标签ID为tagID的标签的HTML内容。

  
21、JavaScript中对于HTML的每个标签都可以处理为一个对象，在一个标签或者控件中使用JavaScript的方法时，将this作为参数即表示将这个控件对象作为参数。如<input
type="passwrod" onblur="checkPassword(this)">。

  
22、控件的属性onblur表示监听焦点离开控件的监听器。

  
23、JavaScript的confirm(tipcontext)方法用于弹出选择对话框，当选择确定时返回true，选择取消时返回false。

  
24、JavaScript的window.location.href=url，用于跳转到url地址。

  
25、JavaScript内置对象screen的属性：

    1）availHeight：浏览器内容显示区的实际高度。

    2）availWidth：浏览器内容县市区的实际宽度。

    3）height：显示器屏幕的高度。

    4）width：显示器屏幕的宽度。

  
26、JavaScript事件绑定的两种方法：

    1）直接在控件标签中使用相应事件监听器属性，如<input type="button" onclick="test();">。

    2）在<script>标签中使用JavaScript脚本绑定，先使用document.getElementById()等方法获取控件对象，在调用控件对象相应的事件监听属性赋予function处理方法（注意值只是function的方法名，后面不带括号）。这种绑定方法需要将<script>脚本位置放置到控件标签之后，因为HTML解析是按照先后顺序解析，放在控件标签前面将绑定到一个空对象上。另外这种绑定也可以使用匿名function，即直接在给控件对象事件监听属性赋值时定义function(){}。

  
27、JavaScript的事件绑定处理方法function获取事件对象时，根据不同浏览器可能需要在function的参数使用参数event。FireFox的处理方法是事件触发后生成是个事件对象作为function的参数（如果function带有event参数的话）。IE的处理方法是使用内置事件对象event，function有没有带参数都可以在function方法中通过event获得事件对象。

  
28、JavaScript内置对象history方法back()，用于返回历史页面，不带参数默认返回上一个历史页面，等同于history.back(-1)，返回到历史第i个页面使用history.bake(-i)。对于<a
href="#" onclick="history.back();return false;">，如果onclick后面不添加return
false语句，那么执行back方法返回历史页面后又由于返回true继续实行href="#"，从而又返回到当前页面，导致back方法失效。

  
29、<body>标签的属性onload：该事件监听属性的特点是当整个网页内容全部加载完毕后触发这个方法。

  
30、JavaScript的document.links用于获得页面中所有的超链接地址组成的数组，通过document.links[i]可以获得页面中的第i个超链接地址。

  
31、JavaScript的document.from[i]用于获得页面中的第i个form表单对象，通过document.forms[i].elements[j]可以获得第i个表单中的第j个控件对象。

  
32、JavaScript中的控件对象调用focus()方法可以让该控件获得焦点。

  

