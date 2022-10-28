---
# type: posts 
title: "XML学习笔记 第三记"
date: 2014-06-18T11:14:10+0800
authors: ["Jianan"]
summary: "2012-08-09

1、 Schema元素list：用于从一个特定数据类型集合中选择一个或多个数据，常用于simpleType中，作为其子元素。主要属性有itemType，其属性值是一个简单类型，表示指定的数据类型集合。例如：
表示取值只能取一个或多个date类型的数据，多个数据之间用空格隔开。

2、Schema元素union：用于组合多种简单数据类型，常用于simpleType元"
series: ["XML学习笔记"]
categories: ["XML学习笔记"]
tags: ["schema", "xml"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 1045
comment_num: 0
---

2012-08-09  
  
1、
Schema元素list：用于从一个特定数据类型集合中选择一个或多个数据，常用于simpleType中，作为其子元素。主要属性有itemType，其属性值是一个简单类型，表示指定的数据类型集合。例如：  
<xs:list itemType="xs:date"/>表示取值只能取一个或多个date类型的数据，多个数据之间用空格隔开。  
  
2、Schema元素union：用于组合多种简单数据类型，常用于simpleType元素中做其子元素。主要属性有memberType，其值为一个或多个简单数据类型，多个简单数据类型之间用空格隔开。例如<xs:union
memberType="type1
type2"/>表示取值只能取type1类型或者type2类型。与元素enumeration的区别在于，union组合的是数据类型，而enumeration组合的是具体数据类型的一些具体值。  
  
3、Schema元素simpleType：用于定义一个简单类型，它决定了元素和属性值的约束以及相关信息，主要属性有name，表示该用户自定义数据类型的名字。其主要类内容通过Schema三种元素来使用：  
    1）restrict元素：用于限定一个范围。如：  
        <xs:simpleType name="MyType">  
            <xs:restriction base="xs:integer">  
                <xs:minInclusive value="0"/>  
            </xs:restriction>  
        </xs:simpleType>  
    2）list元素：用于从指定列表中选择一个值。如：  
        <xs:simpleType name="MyType">  
            <xs:list itemType="xs:date"/>  
        </xs:simpleType>  
    3）union元素：用于组合一定的数据类型。如：  
        <xs:simpleType name="MyType">  
            <xs:union>  
                <xs:simpleType>  
                    <xs:list itemType="xs:string"/>  
                </xs:simpleType>  
                <xs:simpleType>  
                    <xs:list itemType="xs:date"/>  
                </xs:simpleType>  
            </xs:union>  
        </xs:simpleType>  
  
4、Schema元素complexType：用于定义一个复合类型，也是能决定一组元素和属性值的约束和相关信息，主要属性有name，标识该符合类型的名字。  
  
5、simpleType和complexType的区别：  
    1）simpleType的定义中不能包含element和attribute，它相当于是对Schema的内置数据类型进行重新的包装，本质上它还是一个Schema内置数据类型，只是对这些内置数据类型做了约束或者合并。在element中使用type引用定义的simpleType，只能作为element的内容，并且element不能再包含子元素或者属性。也就是说，声明的element如果引用或者包含了一个simpleType，那么这个element只能是一个无属性无子元素只有简单类型数据内容的元素；在attribute中使用type引用定义的simpleType，只能作为attribute的属性值，可以看出它的使用方法和作用与使用内置数据类型没什么区别。  
    2）complexType的定义中可以包含element和attribute，当然也可以包括内置数据类型以及simpleType，它相当于是能将element、attribute、数据类型进行封装的容器。引用complexType的element将会根据complexType中包含的内容将其过度给引用的element。  
  
6、Schema元素simpleContent：只能作用于complexType中，用于对它的内容进行约束和扩展。一个complexType元素包含了一个simpleContent，就不能再包含其它元素。同时，simpleContent的内容也只能包含一个extension元素或者一个restriction元素或者一个annotation元素，extension元素可再包含一个或多个attribute属性元素。若声明的元素使用包含simpleContent的complexType，则该声明元素不可以再包含除了unique、key、keyref之外的其它元素，simpleContent下的extension会过渡为声明元素的内容，extension的子元素attribute会过渡委声明元素的属性，也就是说该声明元素最多只能拥有多个属性或多个内容（complexType外部的unique可组合简单类型）。如果simpleContent的extension中不定义attribute，则包含simpleContent的complexType与simpleType没有区别。由此可以看看出，simpleContent实际上就是为了弥补simpleType只能拥有内容不能拥有属性而产生的，它可以比simpleType多包含属性。  
  
7、Schema元素extension：可以作为simpleContent的子元素，主要属性有base，其值为一个数据类型，用于表示引用包含simpleContent的complexType的声明元素的内容的数据类型。如：<xs:extension
base="xs:string"/>。extension可以包含多个子元素attribute，它们将过渡为element的属性。  
  
8、java.lang.Desktop类的静态getDesktop（）方法可以获取一个Desktop对象。该对象的open（File
file）可以调用本地系统关联的程序打开file指向的文件，就跟在系统上直接打开该文件的效果一样，如果无法找到关联的程序打开文件就会抛出异常。  
  
9、Schema元素choice：用于包含一个或多个定义或引用的element，并且只能定义在complexType里面。它有两个属性，一个是minOccurs，一个是maxOccurs，属性值都是整数类型。整个choice用于表示choice包含的所有element最少要出现minOccurs次，最多不能超过maxOccurs次，至于出现的是哪个element则只要是choice里面的element就行，次数符合要求就可以。  
  
10.Schema元素sequence：同choice类似，也是用于包含一个或多个定义或引用的element，通常也是定义在complexType里面，也有两个属性minOccurs和maxOccurs，属性值为整数。与choice的区别在于，sequence中的所有element必需按照定义在sequence中排列的先后顺序成组出现，并且最少出现minOccurs次，最多出现maxOccurs次。也就是说，sequence中的次数单位是sequence中所有element按顺序排列的组，choice的次数单位是choice中单个element。  
  
11、Schema文档的后缀名为“.xsd”。  
  
12、Schema文档不像DTD可以使用DOCTYPE指定哪一个是根元素，它并没有指定文档中定义的哪一个元素是根元素。凡是使用Schema校验的XML文档，其根元素可以使Schema文档中定义的任何一个根元素。选定一个根元素后，Schema文档中没有包含在该元素下的元素在XML文档中都不能出现。因此，设计Schema文档的时候，通常都会设计一个根元素包含文档中其它所有元素，并在校验的XML文档中使用该元素作为根元素已使用文档中定义的其它元素。

