---
# type: posts 
title: "JavaScript学习笔记 第五记"
date: 2014-07-11T10:41:17+0800
authors: ["Jianan"]
summary: "1、jquery对象的两个方法.html()返回的是标签对象包含的html内容（包括包含的其它标签元素一并输出），而.text()返回的是标签对象最终显示的文本格式（包含的其它格式标签作用在文本后输出最终的文本格式）。

2、jquery过滤选择器的表单对象属性过滤选择器：
    1）:enabled选择器：$(\"#form1 :enabled\")，选取id为form1的表单内所有可用的元"
series: ["JavaScript学习笔记"]
categories: ["JavaScript学习笔记"]
tags: ["javascript", "jquery", "选择器", "替代对象"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 908
comment_num: 0
---

1、jquery对象的两个方法.html()返回的是标签对象包含的html内容（包括包含的其它标签元素一并输出），而.text()返回的是标签对象最终显示的文本格式（包含的其它格式标签作用在文本后输出最终的文本格式）。

  
2、jquery过滤选择器的表单对象属性过滤选择器：

    1）:enabled选择器：$("#form1 :enabled")，选取id为form1的表单内所有可用的元素。注意form1之后有空格，加了空格即为层次选择器才能选择form表单内enabled的元素，没有空格则选择的是enabled的form1对象。

    2）:disabled选择器：$("#form1 :disabled")，选取id为form1的表单内所有不可用的元素。注意！form1之后有空格，加了空格即为层次选择器才能选择form表单内enabled的元素，没有空格则选择的是disabled的form1对象。

    3）:checked选择器：$("input:checked")，选取所有被选中的input元素，包括单选框和复选框。

    4）:selected选择器：$("select :selected")，选取所有选择列表select中被选中的选项元素。注意select之后有空格，加了空格表示层次选择器，匹配的所有select中的选项元素，没有加空格则匹配的是select元素。

  
3、jquery对象的方法.val()用于返回标签对象的value值，而.val(value)用于设置标签对象的value值。

  
4、HTML的select标签添加属性multiple="multiple"后，选择列表将变为多选选择列表。

  

5、jquery的select对象方法.change(function(){})，当select选择列表选择发生改变后就会调用这个function方法。

  
6、jquery对象的.each(function(){})方法用于向jquery对象中的每一个标签元素添加方法function。

  
7、jquery的表单选择器：

    1）:input选择器：$(":input")，选取所有<input>、<textarea>、<select>、<button>元素。

    2）:text选择器：选取所有单行文本框。

    3）:password选择器：选取所有密码框。    

    4）:radio选择器：选取所有单选框。

    5）:checkbox选择器：选取所有多选框。

    6）:submit选择器：选取所有提交按钮。

    7）:image选择器：选取所有的图像按钮。

    8）:reset选择器：选取所有的重置按钮。

    9）:button选择器：选取所有的按钮。

    10）:file选择器：选取所有的上传域。

    11）:hidden选择器：选取所有不可见元素。

  
8、$("#form1 :input")与$("#form1 input")的区别：$("#form1
:input")获取的是表单form1中素有的input、textarea、select和button元素，而$("#form1
input")是层次选择器选择的是form1中所有的后代input元素。

  
9、jquery的选择器$("element :selcetor")与$("element:selector")不同，$("element
:selector")选择的是element中符合selector条件的后代元素，$("element:selector")选择的是符合selector条件的element元素。

  
10、JavaScript推荐申明的jquery对象名称以$开头，如$element。

  
11、jquery对象的.attr(attribute)方法返回jquery对象的属性attribute的属性值，.attr(attribute,value)方法设置jquery对象的属性attribute值为value。

  
12、JavaScript创建新的DOM对象使用document.createElement("Tag")方法，创建好后可以使用新对象的setAttribute("attribute","value")方法设置新对象的属性，最后还要使用Element.appendChild("newElement")方法将新的DOM对象newElement插入到Element对象中，作为Element对象的子元素，所以使用这个appendChild方法的前提是Element对象是个可以带内容的对象。

  
13、jquery插入新节点的方法：

    1）使用append()方法：向每个匹配的元素内部追加内容。如原有<p>我想说：</p>，使用$("p").append("<b>你好</b>")后，结果为<p>我想说：<b>你好</b></p>。

    2）使用appendTo()方法：将所有匹配的元素追加到指定的元素中，实际上是append()方法的倒置。如<p>我想说：</p>，使用$("<b>你好</b>").appendTo("p")之后，结果为<p>我想说：<b>你好</b></p>。

    3）perpend()方法：向每个匹配元素的内部前置内容。如<p>我想说：</p>，使用$("p").prepend("<b>你好</b>")之后，结果为<p><b>你好</b>我想说：</p>.

    4）perpendTo()方法：将所有匹配的元素前置到指定的元素中，实际上是perpend()方法的倒置。如<p>我想说：</p>，使用$("<b>你好</b>").appendTo("p")之后，结果为<p><b>你好</b>我想说：</p>。

    5）after()方法：在每个匹配的元素之后插入内容。如<p>我想说：</p>，使用$("p").after("<b>你好</b>")方法后，结果为<p>我想说：</p><b>你好</b>。

    6） insertAfter()方法：将所有匹配元素插入到指定元素的后面，实际上是after()方法的倒置。如<p>我想说：</p>，使用$("<b>你好</b>").insertAfter("p")之后，结果为<p>我想说：</p><b>你好</b>。

    7）before()方法：在每个匹配元素之前插入内容。如<p>我想说：</p>，使用$("p").before("<b>你好</b>")之后，结果为<b>你好</b><p>我想说：</p>。

    8）insertBefore()方法：将所有匹配元素插入到指定的元素前面，实际上是堆before()方法的倒置。如<p>我想说：</p>，使用$("<b>你好</b>").insertBefore("p")之后，结果为<b>你好</b><p>我想说：</p>。

    

14、jquery插入新节点的方法都采用方法链的设计模式设计。

  
15、通过$("<p>你好</p>")的方式就可以直接创建一个p标签的jquery节点对象。

  
16、jquery插入新节点的系列方法的参数也可以直接是一个jquery对象，如$("p").perpend($("#insert"))。特别的，当参数是已存在的标签对象是，使用这些列方法就可以达到间接移动页面标签位置的作用，如$("ul
li:eq(2)").insertAfter("ul li:eq(4)")就可以将ul列表中第三个li移动到第五个li后面。

  
17、每个jquery对象都提供方法.remove()，调用这个方法的jquery对象将会从页面中被移除，方法的返回值为被删除的对象，也就是说被删除的对象实际上还存在内存里。remove方法也可带参数，参数为一个过滤条件（jquery的过滤选择器）。如$("ul
li").remove("li[title!=2]")将会移除ul列表中所有title不等于2的li。

  
18、jquery对象的.empty()方法用于将jquery对象的内容清除。

  
19、jquery对象的.clone(boolean)方法用于复制一个jquery对象，当参数为true时则连同原对象的事件监听器一起复制，当参数为false是则源对象的事件监听器不复制。不带参数则默认参数为false。

  
20、$("ul
li").click(function(){$(this).clone().appendTo("ul");})，其中$(this)表示当前的li对象，因为jquery在底层上是遍历整个li组成的数组的。

  
21、jquery代替对象：

    1）$("p").replaceWith("<a>repalce</a>")，使用a标签的代替p标签，replaceWith的参数也可以是jquery对象变量。

    2）$("<a>replace</a>").replaceAll("p")，使用a标签代替页面中所有的p标签。

  
22、jquery包围对象：

    1）$("p").wrap("<a><b></b></a>")，将整个p标签包围到warp参数标签的最内层，本例结果为<a><b><p></p></b></a>。

    2）$("p").wrapInner("<a><b></b></a>")，将p标签的内容包含到warpInner参数的最内层，本例结果为<p><a><b>p标签内容</b></a></p>。

  
23、jquery对象获取或添加属性方法.attr()，移除属性方法.removeAttr()。当要使用attr方法设置多个属性和属性值时，需要使用格式attr({attribute1:value1,attribute2:value2...})。

  
24、jquery对象设置css类样式方法：

    1）Element.attr("class","className")：设置Element对象的class属性值为className样式。

    2）Element.addClass("className")：添加Element对象的一个class属性值className。由于同一个标签的class引用css类选择器可以多个（多个class值之间用空格分隔），因此，使用addClass方法会检查Element是否已经具有className样式，有则不添加，没有则添加。而attr方法则直接覆盖掉原有class值。

    3）Element.removeClass()：带参数时移除Element指定参数的class值，不带参数则移除Element全部class值。

    4）Element.toggleClass("className")：如果Element带有className的class值则移除这个className值，如果不带有className的class值则添加这个className值。

    5）Element.hasClass("className")：检查Element是否具有className的class值，有则返回true，没有返回false。

    6）Element.is(".className")：功能与hasClass方法一样。实际上is方法还有其它功能。

  
25、jquery对象的html()和text()方法都可带字符串参数，当字符串参数中含有标签符号时，html方法会将标签符号当做标签来处理，而text方法会将标签符号转义成字符以字符来处理。

  
26、jquery对象的focus(function(){})方法用于对象获得焦点时触发，blur(function(){})方法用于对象失去焦点时触发。

  
27、DOM对象具有属性defaultValue，表示对象最初的value值。

  
28、jquery对象的.children()方法返回对象的子元素数组。

  

