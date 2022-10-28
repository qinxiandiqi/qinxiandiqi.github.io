---
# type: posts 
title: "XML学习笔记 第二记"
date: 2014-06-18T11:10:09+0800
authors: ["Jianan"]
summary: "2012-08-07

1、XML实体可分为普通内部外部实体和内部外部参数实体。普通内部外部实体用在XML文档中，而内部外部参数实体用在DTD声明中。

2、DTD声明普通内部实体语法：，调用时用格式“&实体名;”。

3、DTD声明普通外部实体语法：，表示使用URI或者URL指向文件的全部内容替换实体名，比较少用，调用时用格式“&实体名;”。

4、DTD声明内部参数实体语法：，"
series: ["XML学习笔记"]
categories: ["XML学习笔记"]
tags: ["xml"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 850
comment_num: 0
---

2012-08-07  
  
1、XML实体可分为普通内部外部实体和内部外部参数实体。普通内部外部实体用在XML文档中，而内部外部参数实体用在DTD声明中。  
  
2、DTD声明普通内部实体语法：<!ENTITY 实体名 实体值>，调用时用格式“&实体名;”。  
  
3、DTD声明普通外部实体语法：<!ENTITY 实体名 SYSTEM
"URI/URL">，表示使用URI或者URL指向文件的全部内容替换实体名，比较少用，调用时用格式“&实体名;”。  
  
4、DTD声明内部参数实体语法：<!ENTITY %实体名 实体值>，调用时用格式“%实体名;”。  
  
5、DTD声明外部参数实体语法：<!ENTITY %实体名 SYSTEM
"URI/URL">，表示使用URI或者URL指向文件的全部内容替换实体名，比较少用，调用时用格式“%实体名;”。  
  
6、XML的命名空间，当文档中相同元素名或属性名但定义内容部一致时，为了区分，需要使用命名空间。定义XML命名空间的语法为在首次使用该命名空间的地方使用xmlns：name="URL"，定义后在元素名或属性名前使用“name：”前缀就能标识该元素或属性是name命名空间的内容。命名空间使用全球唯一的URL作为命名空间的值，为的是保证命名空间的唯一性，但实际上URL只是一个标识字符串，URL是否有实际意义是没有关系的。XML的命名空间类似于Java中的package。  
  
——————————————————————————————————————————————  
  
2012-08-08  
  
1、Schema：与DTD一样，用于验证XML文档的有效性，并且比DTD提供更强大的功能和更细粒度的数据类型，还支持自定义数据类型，所以，Schema文档就是用来声明XML文档规则的。最重要的是，Schema本身也是一个XML文件，遵守XML规范，而DTD不是。  
  
2、Schema作为XML文档的一种，自然要遵循XML文档的基本语法，也需要验证它的有效性。Schema文档由DTD验证，所以Schema的本源也是DTD。  
  
3、Schema元素schema：Schema文档的根元素，属性有xmlns和targetNamespace。Schema文档的根元素规定必须为shemale，并且来自URL为http://www.w3.org/2001/XMLSchema的命名空间，Schema文档中使用的Schema元素和数据类型都来自于该命名空间，这个地址也标识了用于验证Schema的DTD文档。另外，这个命名空间的名字可以随便起，但是整个Schema文档中使用的Schema元素和数据类型都必须使用该命名空间名字标注。比如：<xs:schema
xmlns:xs="http://www.w3.org/2001/XMLSchema"
targetNamespace="http://mynamespace/myschema">，则文档中所有Schema元素和数据类型都必须用xs标注<xs:元素或类型名>。targetNamespace命名空间用于标识本Schema文档中自定义的元素和数据类型，其URL值可以自己定义。  
  
4、使用Schema文档校验XML文档方法：在被校验的XML文档根元素使用<根元素
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:noNamespaceSchemaLocation(Schema文档中没有定义targetNamespace的情况)/targetNamespace(有定义targetNamespace情况)="Schema文档地址">。  
  
5、Schema的数据类型分为简单类型和复杂类型（通过complexType定义）。简单类型又分为内置数据类型和用户自定义数据类型（通过simpleType定义）。内置数据类型又可再分为基本数据类型和扩展数据类型。  
  
6、Schema元素element：用于声明被校验XML文档的一个元素，主要属性有：  
    1）name：声明元素的名字；  
    2）type：声明元素的数据类型，可以使用Schema的数据类型，也可以使用自定义的数据类型。该类型要么是声明元素的属性，要么是声明元素标签之间的内容或者子元素。  
    3）ref：用于引用一个声明好的元素，如<xs:element ref="cat"/>，cat是本文档中已经声明的一个元素，该语句表示这里需要填充一个cat元素，常用于声明自定义数据类型中使用。  
    4）minOccurs：该属性值为一个整数，表示该声明元素最少要出现的次数。  
    5）maxOccurs：该属性值为一个整数，表示该声明元素最多能出现的次数，如果值为unbounded，则表示无限制。  
    6）substitutionGroup：属性值是一个已经定义的元素名字，表示可以使用属性值对应的元素替换属性归属的元素，比较少使用。  
  
7、Schema元素group：用于定义一个元素组，将过个元素进行捆绑，元素组中的元素要么全部使用，要么全部不使用，主要属性有：  
    1）name：元素组的名字；  
    2）ref：引用一个元素组，属性值为已经定义的元素组名。  
  
8、Schema元素attribute：用于声明一个属性，子元素只能是simpleType或者annotation，主要属性有：  
    1）name：声明属性的名字；  
    2）type：声明属性的数据类型，只能的简单类型，不能是复杂类型，因为属性不能再包含一个属性，没有属性的属性，只有属性的类型；  
    3）ref：引用一个声明过的属性，值为声明过的属性名，常用在complexType元素中；  
    4）use：申明属性的特点，属性值有optional（可以出现或者不出现）、prohibited（不准使用）、required（必须使用）。  
  
9、Schema元素attributeGroup：用于定义一个属性组，将多个属性捆绑在一起，调用一个属性组等于同时调用属性组里的所有属性。主要属性有：  
    1）name；属性组的名字；  
    2）ref：引用一个属性组，属性值为已经定义的属性组名。  
  
10、Schema元素restriction：用于将对已存在的简单类型取值限定在一个范围内，主要属性有base，其属性值为一个简单类型，常用于simpleType元素中做其子元素。限定范围的方法借用在restrict元素首尾标签之间使用Schema其它相关元素来限定：  
    1）<xs:minInclusive value="value"/>，限定值必须等于或大于value。  
    2）<xs:maxInclusive value="value"/>，限定值必须等于或小于value。  
    3）<xs:minExclusive value="value"/>，限定值必须大于value。  
    4）<xs:maxExclusive value="value"/>，限定值必须小于value。  
    5）<xs:enumeration value="value"/>，可使用多次来定义一个枚举类型，限定值  
必须为多个值中的一个。  
    ...  
  
11、在元素首尾标签之间定义的类型或者声明的元素和属性，类似于Java中的匿名内部类，所以这些类型或者元素和属性不需要name属性，它们只能被包含它们的元素使用。这种做法也相当于将元素首尾标签之间定义的内容用simpleType或complexType元素定义后，再以该元素的type属性指定。  
  
12、声明元素的type属性或者元素首尾标签之间定义的东西，将根据它们的特点作为声明元素的特定组成部分。如果是元素，则作为声明元素的子元素；如果是数据类型，则作为声明元素首尾标签之间的内容；如果是属性，则作为声明元素的属性。另外，如果type类型或标签之间的内容是complexType类型的话，则将complexType再拆分，然后根据以上规则将complexType内容作为声明元素特定的组成部分。

