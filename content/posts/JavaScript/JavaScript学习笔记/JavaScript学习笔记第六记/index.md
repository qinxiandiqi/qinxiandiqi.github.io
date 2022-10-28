---
# type: posts 
title: "JavaScript学习笔记 第六记"
date: 2014-07-11T12:09:56+0800
authors: ["Jianan"]
summary: "1、浏览器的事件机制：冒泡事件机制，IE浏览器的冒泡机制是内容元素的事件处理后逐层抛出给外层元素进行事件处理，其它浏览器基本沿用这个方向，但也有相反的。

2、浏览器自动生成的事件对象even方法.stopPropagation()可以阻断事件继续冒泡到下一层，而.perventDeafult()可以阻止这个对象的默认行为，如表单对象的action事件默认行为是提交表单。而在事件处理方法中使用"
series: ["JavaScript学习笔记"]
categories: ["JavaScript学习笔记"]
tags: ["javascript", "jquery", "json", "gson", "动画"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 701
comment_num: 0
---

1、浏览器的事件机制：冒泡事件机制，IE浏览器的冒泡机制是内容元素的事件处理后逐层抛出给外层元素进行事件处理，其它浏览器基本沿用这个方向，但也有相反的。

  
2、浏览器自动生成的事件对象even方法.stopPropagation()可以阻断事件继续冒泡到下一层，而.perventDeafult()可以阻止这个对象的默认行为，如表单对象的action事件默认行为是提交表单。而在事件处理方法中使用return
false可以阻断事件继续冒泡也可阻止默认动作。

  
3、jquery绑定事件的方法.bind("eventName","para",function(){})，第一个参数eventName表示监听的事件类型，如click表示单击事件、mouseover表示鼠标进入元素事件，mouseout表示鼠标离开元素事件；第二个参数为可选参数，表示可以向事件对象传递的额外数据；第三个参数为事件的处理方法。另外bind中第一个参数"evnetName"也可以通知指定多个eventName事件类型，如"click
mouseover"。

  
4、jquery的$(document).ready(function(){})方法中function的执行在整个页面的DOM架构加载完毕后就开始执行，而纯JavaScript的window.onload(function(){})方法中的function在整个页面完全加载完毕，包括页面中的其它链接文件全部下载完毕后才开始执行。

  
5、jquery关于动画的方法，这些方法一般都可接收四种参数：slow表示0.6秒，normal表示0.4秒，fast表示0.2秒，以及具体的毫秒数值。参数表示执行完该方法需要的时间，没有参数则表示为0秒即马上。这些方法全部使用方法链设计：

    1）show()方法：使用这个方法的标签在show方法被执行后才会显示。

    2）hide()方法：使用该方法的标签在hide方法执行后将会被隐藏。

    3）fadeOut()方法：使用该方法的标签将在指定的时间内淡出。

    4）fadeIn()方法：使用该方法的标签将在指定的时间内淡入。

    5）slideUp()方法：使用该方法的标签将在指定的时间内由下往上被收起。

    6）slideDown()方法：使用该方法的标签将在指定的时间内由上往下被拉出。

    7）animate({attribute1:value1,attribute2:value2...},time,function(){})方法：其中第一个参数由花括号括起来，其中每对值为标签需要改变的属性对；第二个参数time为完成这些改变需要的时间；第三个参数为完成改变后自动回调的方法。如$("div").animate({left:"+=500px"},3000,function(){alert(3);})，div标签将会向右移动到距离由编剧500px像素的地方，特别的，500前的“+=”会每执行一次后自动以500的距离增加，即第二次执行将变成left:1000px，否则第二次执行将原地不动。多个animate以方法链形式连接时将会逐个执行，而链中的其它属性方法（如css）则会直接先执行。

  
6、jquery对象.is(":visible")用于判断该jquery对象是否可见，返回值为boolean。

  
7、jquery中与bind方法相对应的是unbind()方法。当unbind不带参数时则取消绑定标签上所有的事件监听；当unbind接收一个event事件类型做参数时则取消绑定标签上的这个event的事件监听；当unbind接收一个event事件类型和一个具体event事件对象做参数时，则取消标签上指定的这个event事件对象。

  
8、jquery类似于bind的另一个绑定方法.one("eventName",function(){})，区别在于one绑定的事件使用一次后就失效。

  
9、jquery对象提供方法.trigger("eventName")，用于模拟触发一个eventName事件，相当于直接调用这个事件的处理方法.eventName()。

  
10、JSON（JavaScript Object
Notation）：可代替XML进行数据传输（相比较XML，传输相同的数据需要的流量要小的多得多），但本身不是一门新的语言，是利用JavaScript的子集创建出来的一种数据格式。多种语言都支持这种JSON数据格式，也就是可以利用JSON数据格式在不同语言之间传递。

  
11、JSON基于两种结构：

    1）基于name/value的字符串，可以看做是JSON的对象，如{name1:value1,name2:value2,...}。

    2）基于有序value的列表，可以看做是JSON的数组，如[value1,value2,...]。

    3）JSON的对象或者数组严格按照规定的格式书写。

    4）JSON的对象中不包括方法。

  
12、JSON的类型书写格式：

    1）object：{name1:value1,name2:value2,...}。以花括号包围，每对name和value之间用“:”分隔，对与对之间使用“,”分隔。这种格式是JavaScript内置支持的格式，也就是JavaScript本身就能解析这种格式。

    2）array：[value1,value2,value3...]。以中括号包围，value之间使用“,”分隔。

    3）value：可以使string（字符串类型）、number（数值类型）、object（对象类型）、array（数组类型）、boolean（true或false）、null。

    4）string：字符串类型，由一个或多个Unicode字符构成，使用""包围就表示string类型，其中也可使用转义字符将字符转义。特别的，JSON中没有字符的概念，字符就是长度为1的字符串。

    5）number：数值类型，包括整数和浮点数。JSON不支持八进制和十六进制数。

  
13、XML转JSON例子：

    XML格式：
    
    
        <users>
            <user gender='male'>
                <username type='string'>aaa</username>
            </user>
            <user gender="male'>
                <username type='string'>bbb</username>
            </user>
        </users>

    转为JSON格式：
    
    
        {users:[
            {user:{gender:'male',username:['aaa',type:'string']}},
            {user:{gender:'male',username:['bbb',type:'string']}}
            ]
        }

  
14、JavaScript内置支持JSON，要获取13例中aaa，比如该JSON字符赋给var
json，则通过json.user[0].user.username[0]便可获得aaa。

  
15、在页面中引入json的媒体格式为application/json，而html媒体格式为text/html，css文件媒体格式为text/css，xml文件媒体格式为text/xml。

  
16、JSON没有命名空间，没有验证器，不可以扩展，不是XML。对于JSON来说，每一个JSON对象就已经相当于一个命名空间。

  
17、Douglas，JSON创始人为Java语言设计的douglascrockford-JSON-
java类包，需要将Douglas设计的JSON类放置到org.json包下，并在Java项目中import导入。

  
18、douglascrockford-JSON-java中主要类：

    1）JSONObject，其构造方法JSONObject(String json)能够通过JSON格式字符串构造出JSONObject对象，如果json字符串不符合JSON的object格式将构造失败并抛出异常。对象中主要提供了大量获取json中各种数据类型的name/value的value值方法。如getString(String name)、getJSONObject(String name)、getInt(String name)等等。

    2）JSONArray，其构造方法JSONArray(String json)能够通过JSON格式字符串构造出JSONArray对象，如果json字符串不符合JSON的array格式将会构造失败并抛出异常。对象中主要提供了获取json中各种数据类型的value值。如getString(int index)、getJSONObject(int index)、getInt(int index)等。

  
19、使用广泛的另一个为Java设计的JSON：Gson，google设计的，能够支持泛型的JSON。使用Gson需要将Gson的jar包导入到Java项目中。

  
20、Gson中主要的类com.google.gson.Gson，通过这个类基本能实现Gson的操作。Gson类中主要两个方法：

    1）String toJson(Object object)：接收一个对象，返回值为将对象的属性转换为JSON格式的对象的字符串。

    2）<T> T formJson(String json,Class<T> classofT)：将json格式字符串转换为Java类对象。

  
21、jquery中提供的支持Ajax的方法：

    1）
    
    
    $.ajax({
            url:url,
            type:type,
            dataType:"html",
            data:params,
            success:function(returnedData){}
     });

    其中url参数为请求的服务器资源，type为发送请求的方式（POST、GET等），dataType为得到服务器响应的数据类型（默认为"html"，即字符串格式；也可以为json或者xml），data为请求附加的数据（无论是GET还是POST方式都以同样的方式附加data数据，数据json对象的方式附加，如{username:'abc',age:20}），success:function为请求成功得到服务器响应后回调的方法。

    2）$.post("url",data,function(returnedData,status){})：jquery对ajax的post提交方式的简化。第一个参数url为请求的服务器资源；data为请求附加的数据（同样适用json的数据格式）；function的returnedData参数表示服务器响应的数据，status为服务器响应的状态（如200,403等）。方法中并没有指定服务器响应数据的媒体格式，因为这个媒体格式在服务器发送响应之间就使用resp.setContectType("application/json;charset=utf-8")的方式确定。

    3）$.get("url",data,function(returnedData,status){})：使用方式和参数意义与post方法一致，不同的只是发出请求的方法是GET而已。

