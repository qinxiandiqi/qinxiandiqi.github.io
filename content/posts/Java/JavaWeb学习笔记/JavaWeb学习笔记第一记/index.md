---
# type: posts 
title: "JavaWeb学习笔记 第一记"
date: 2014-07-02T12:34:49+0800
authors: ["Jianan"]
summary: "1、HTML超链接标签中设置属性target=\"_blank\"可以令超链接在新标签页中打开。


2、HTML标签table子标签：
1）tr：表格行标签，table row
2）td：表格列标签
3）th：表格列标题标签，table head
4）属性align，控制文本的对齐方式，center为居中对齐。
5）属性width，可接收百分数，表示占用比例。


3、表单fo"
series: ["JavaWeb学习笔记"]
categories: ["JavaWeb学习笔记"]
tags: ["html", "css"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 765
comment_num: 0
---

1、HTML超链接标签中设置属性target="_blank"可以令超链接在新标签页中打开。

  

2、HTML标签table子标签：  
1）tr：表格行标签，table row  
2）td：表格列标签  
3）th：表格列标题标签，table head  
4）属性align，控制文本的对齐方式，center为居中对齐。  
5）属性width，可接收百分数，表示占用比例。

  

3、表单form标签的子标签：  
1）子标签input加属性type和属性值text，input标签的type默认属性值，表示一个文本框。  
2）子标签input加属性type和属性值password：密码框。  
3）子标签input加属性type和属性值checkbox：多选按钮。  
4）子标签input加属性type和属性值radio：单选按钮。将多个radio类型单选按钮组合到同一个组中，需要在input标签中添加属性name，只要name的值相同，radio单选按钮就在同一个组中。  
5）子标签select：下拉列表。  
6）option：select的子标签，每个option为select下拉列表的下拉选项。  
7）子标签testarea：多行文本框。  
8）子标签input加属性file：将显示一个按钮，点击可弹出文件选择框选择上传文件等操作。  
9）子标签input加属性submit：表单提交按钮，如果没有指定提交目标，默认将把表单数据提交给当前页面。  
10）子标签input加属性reset：将表单恢复到最初的状态。  
11）子标签input加属性button：一个普通的按钮，可再加onClick属性增加按钮相应操作，通常操作为一种JS动作。

  

4、浏览器内核：  
1、靠近标准内核：webkit，速度比较快。  
2、非标准内核：trident，也叫IE内核。

  

5、HTML空格： &nbsp;

  

6、HTML标签img，其属性src可以接收一个本地图片路径或者网络图片路径。

  

7、最快的浏览器Chrome，可以利用CPU硬件加速。

  

8、CSS基本语法：选择器{属性:属性值;
...}，选择器为需要定义样式的标签名；如果属性值由多个单词构成需要对属性值加双引号；多个属性属性值对之间需要用时“;”隔开。

  

9、链接外部css文件方法：在HTML文件的head标签中添加便可链接到指定css文件。

  

10、CSS支持同时多个选择器进行样式设置：选择器,选择器...{属性:属性值;...}。

  

11、CSS支持选择器类：选择器.classname{属性:属性值;...}；HTML文档中使用该选择器标签需要添加标签属性class，属性值为CSS中定义的选择器类classname。这样才能这种这个选择器类设置的显示样式。同一个标签中不能设置多个class。

  

12、定义所有标签的选择器类：.classname{属性:属性值;...}。这样定义的选择器类在所有的标签中都可以使用标签的class属性获得样式。

  

13、CSS的id选择器，定义方法与class类选择器定义方法类似，不同点在于将类选择器中的.改为#。使用方法也与类选择器类似，但是不提倡同一个HTML文档多次使用一个id选择器，但是多次使用也不会造成什么问题，因为浏览器对HTML标签是宽容处理。

  

14、class选择器和id选择器名字不要以数字开头，否则firefox浏览器无法识别。

  

15、css中的属性值和属性值单位之间不要留空格，有些浏览器可能无法识别。

  

16、html的标签hr，没有结束标签，用于画一条分割线。

  

17、在css中，如果一个选择器的样式被定义了多次，那么最终应用的在html的样式是根据定义的选择器优先级，先选择优先级最高的选择器样式，再组合优先级低的选择器中高优先级选择器没有的样式进行组合，并将组合的样子作为最终使用的样式。简单说，就是css将样式向最丰富化使用。

  

18、CSS设置超链接a标签的伪选择器：  
1）a:link：超链接没有被选择和没有鼠标放置在超链接上面状态时的样式。  
2）a:visited：访问过的超链接样式。  
3）a:hover：鼠标放置在超链接上面时的样式。  
4）a:active：鼠标点击超链接时的样式。

