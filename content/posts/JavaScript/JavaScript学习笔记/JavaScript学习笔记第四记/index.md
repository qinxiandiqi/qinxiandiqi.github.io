---
# type: posts 
title: "JavaScript学习笔记 第四记"
date: 2014-07-11T10:36:26+0800
authors: ["Jianan"]
summary: "1、选择器的目的是为了获取页面中的标签元素，jquery提供了四类选择器：基本选择器、层次选择器、过滤选择器、表单选择器。

2、jquery的基本选择器：
    1）ID选择器：$(\"#idvalue\")，选取id为idvalue的元素。返回的数组中只有一个元素，即使页面中存在多个相同id的元素也只获得最前面的元素，因为ID本来就不提倡同个页面中出现多个。
    2）类选择器：$(\""
series: ["JavaScript学习笔记"]
categories: ["JavaScript学习笔记"]
tags: ["javascript", "jquery", "选择器"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 705
comment_num: 0
---

1、选择器的目的是为了获取页面中的标签元素，jquery提供了四类选择器：基本选择器、层次选择器、过滤选择器、表单选择器。

  
2、jquery的基本选择器：

    1）ID选择器：$("#idvalue")，选取id为idvalue的元素。返回的数组中只有一个元素，即使页面中存在多个相同id的元素也只获得最前面的元素，因为ID本来就不提倡同个页面中出现多个。

    2）类选择器：$(".classvalue")，选取页面中所有class为classvalue的元素，返回的是一个数组对象。

    3）元素选择器：$("element")，选取页面中所有元素名称为element的元素，返回的是一个数组对象。

    4）通配选择器：$("*")，选择页面中所有的元素，返回的是一个数组对象。

    5）组合选择器：$("seletor1,seletor2...")，使用“,”分隔各个选择条件，返回的数组对象中元素将包含所有符合其中任意一个条件的元素。

  
3、jquery的层次选择器：

    1）后代选择器：$("ancestor descendant")，选择符合ancestor条件的元素下的所有后代元素descendant，也就是只要是符合ancestor条件的元素包含的descendant元素全部都会被选择，返回的是一个数组对象。

    2）子选择器：$("parent > child")，选择符合parent条件的元素下所有的child子元素，返回的是一个数组对象。与后代选择器的区别是选择的是符合parent条件元素包含的所有child子元素，而不是parent下所有的child元素。

    3）next选择器：$("prev + next")，选择符合prev条件的元素的下一个与prev同等级的next元素，也就是选择与prev为兄弟元素的下一个next元素。返回的是一个数组对象，因为一个页面中可能出现多个符合prev条件的元素，这些元素的next元素全部都会被选择。

    4）~选择器：$("prev ~ siblings")，选择符合prev条件元素后面的所有兄弟siblings元素，返回的是一个数组对象。

  
4、$("prev + next")等价于方法$("prev").next("next")。

  
5、$("prev ~ siblings")等价于方法$("prev").nextAll("siblings")。

  
6、方法$("prev").siblings("siblings")用于获得所有与prev同等级的siblings元素，无论前后位置。

  
7、jquery的过滤选择器又分为基本过滤、内容过滤、可见性过滤、属性过滤、子元素过滤、表单对象属性过滤几种。

  
8、jquery过滤选择器的基本过滤选择器：

    1）:first选择器：$("element:first")，用于获得页面中所有element元素中的第一个element元素对象，返回的是单个对象。

    2）:last选择器：$("element:last")，用于获得页面中所有element元素中的最后一个element元素对象，返回的是单个对象。

    3）:not(selector)选择器：$("element:not(selector)")，用于获得页面中所用element元素符合selector条件的元素，返回的是一个element元素数组对象。如$("div:not(.notclass)")将获得页面中所有div中没有使用notclass的div元素。

    4）:even选择器：$("element:even")，获取页面所有element中排序索引为偶数的element元素组成的数组对象，索引从0开始，返回的是element数组对象

    5）:odd选择器：$("element:odd")，获取页面中所有element中排序索引为奇数的element元素组成的数组对象，索引从0开始，返回的是element数组对象。

    6）:eq(index)选择器：$("element:eq(index)")，获取页面所有element元素中排序索引为index的element元素独享，索引从0开始，返回的是单个element对象。

    7）:gt(index)选择器：$("element:gt(index)")，获取页面所有element元素中排序索引index之后（不包括本身）的所有element元素数组对象，索引从0开始，返回的是element数组对象。

    8）:lt(index)选择器：$("element:lt(index)")，获取页面中所有element元素中排序索引index之前（不包括本身）的所有element元素数组对象，索引从0开始，返回的是element数组对象。

    9）:header选择器：$(":header")，获取页面中所有标题元素，如<h1>、<h2>等。返回的是数组对象。

    10）:animated选择器：$("element:animated")，选取页面中正在执行动画的element对象，返回的是element数组对象。

  
9、jquery过滤选择器的内容过滤选择器：

    1）:contains('text')选择器：$("element:contains('text')")，选取页面中含有文本内容为text的element元素，返回的是element元素数组集合。

    2）:empty选择器：$("element:empty")，选取页面中不包含子元素（包括文本）的所有element元素，返回的是element元素数组集合。

    3）:has(selector)选择器：$("element:has(selector)")，选取页面中所有包含selector元素的element元素，返回element元素数组集合。

    4）:parent选择器：$("element:parent")，选取所有拥有子元素（包括文本元素）的element元素，返回element元素数组集合。

  
10、jquery过滤选择器的可见性过滤器：

    1）:hidden选择器：$("element:hidden")，选取页面中所有隐藏的element元素，返回element元素数组。

    2）:visible选择器：$("element:visible")，选取页面中所有可见的element元素，返回element元素数组。

  
11、jquery为$(document).ready(function(){})提供了两种简化方式：

    1）$().ready(function(){})

    2）$(function(){})

  
12、jquery过滤选择器的属性过滤器：

    1）[attribute]过滤器：$("element[attribute]")，选取所有拥有属性attribute的element元素，返回数组。

    2）[attribute=value]选择器：$("element[attribute=value]")，选取拥有属性attribute且属性值为value的所有element元素。

    3）[attribute!=value]选择器：$("element[attribute!=value]")，选取attribute属性值不为value的所有element元素，包括不拥有attribute属性的element元素。

    4）[attribute^=vlaue]选择器：$("element[attribute^=value]")，选取拥有attribute属性且属性值以value开头的所有element元素。

    5）[attribute$=value]选择器：$("element[attribute$=value]")，选取拥有attribute属性且属性值以value结束的的所有element元素。

    6）[attribute*=value]选择器：$("element[attribute*=value]")，选取拥有attribute属性且属性值中含有value的所有element元素。

    7）[selector1][selector2]...选择器：$("element[selector1][selector2]...")，选取符合所有selector条件的element元素。

  
13、jquery过滤选择器的子元素过滤器：

    1）:nth-child(index/even/odd/equation)选择器：$("element:nth=child(index/even/odd/equation)，选取每个父元素下第index个子元素的或者奇偶元素，返回的是一个元素集合，索引从1开始。注意匹配的是所有是父元素的element，即使这个element又是另一个element的子元素，这是与:eq(index)等选择器最大的区别。

    2）:first-child选择器：$("element:first-child")，将每个是父元素的element的第一个子元素返回，返回的是一个数组。而:first只返回单个元素。

    3）:last-child选择器：$("element:last-child")，选取每个父元素element的最后一个子元素，与:first-child类似。

    4）:only-child选择器：$("element:only-child")，选取所有只拥有一个子元素的element元素。

  

