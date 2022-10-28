---
# type: posts 
title: "JavaScript学习笔记 第二记"
date: 2014-07-10T12:10:05+0800
authors: ["Jianan"]
summary: "1、JavaScript的Cookie：添加cookie格式为document.cookie=\"key=value;expries=有效日期\"。其中有效日期为GMT事件格式，可以使用new Date()的toGMTString()方法获得；删除cookie格式为document.cookie=\"key=;expries=当前日期\"。

2、两种类型的cookie：
    1）持久性cook"
series: ["JavaScript学习笔记"]
categories: ["JavaScript学习笔记"]
tags: ["javascript", "function", "对象", "继承", "object"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 837
comment_num: 0
---

1、JavaScript的Cookie：添加cookie格式为document.cookie="key=value;expries=有效日期"。其中有效日期为GMT事件格式，可以使用new
Date()的toGMTString()方法获得；删除cookie格式为document.cookie="key=;expries=当前日期"。

  
2、两种类型的cookie：

    1）持久性cookie，存储在客户端的硬盘上。

    2）session的cookie，不会保存在客户端的硬盘上，放在浏览器进程所处的内存中，当浏览器关闭的时候session的cookie就被销毁。

  
3、cookie保存在客户端硬盘上的文件格式：

    1）NS内核浏览器：Cookie.txt

    2）IE内核浏览器：用户名@域名.txt

  
4、JavaScript的parseInt(param)方法会将参数中的数字提取出来，不是数字的字符丢弃。

  
5、JavaScript获取得控件对象后，对象具有属性style，通过属性style可以控制该控件的一些风格。比如table为Table控件，通过table.style.display="none"可以将table控件隐藏，通过table.style.display="block"可以将table控件以block块的样式显示。

  
6、JavaScript语句在客户端执行，JSP语句在服务器执行。

  
7、JavaScript中的function：

    1）在JavaScript中，函数（function）就是一个Function对象。

    2）function methodName(paramelist){}等同于var methodName = function(paremlist){}。可以看出methodName其实就是一个Function对象，所以只要methodName同名，写在后面的就会覆盖前面的function，本质上就是对methodName变量重新赋值。因此，在JavaScript中没有方法重载的说法。

    3）function定义的变量是Function对象，可以通过var methodName = new Function("parame1","parame2"...,"methodbody");来定义一个Function对象，其中parame1为方法的参数名，最后一个参数methodbody为方法体的内容，参数和方法体内容全部以字符串的形式做为Function构造方法的参数。

    4）调用Function对象传递的参数其实是一个arguments对象，这个对象是一个Array对象，长度为传递参数的个数，数组中的第i个元素可以通过arguments[i]获得。

    5）调用function的参数可以与定义function的参数数量不同，少于定义的参数数量时，后面没有提供的参数默认值为Undefined类型的值undefined；多于定义的参数数量时，多出来的参数也会传递到function中。在传递参数的本质是通过arguments来传递，所以，即使定义function没有定义参数但在调用时传递了参数，通过arguments同样可以获得传递过来的参数。

  

8、每个function对象的arguments.length表示实际接收到的参数个数，而methodName.length表示function期望得到的参数个数（也就是定义的参数个数）。  
  
9、JavaScript的5中原始数据类型，包括Undefined、Null、Boolean、Number、String：  
    1)Undefined类型：只有一个值undefined。当使用var声明一个变量时，如果没有对该变量进行赋值，默认值就是Undefined类型的undefined值。Undefined本身是Null类型的派生类型。  
    2）Null类型：只有一个值null。在if的判断语句中，除了null和undefined两个值之外的其它值都等同于true。  
    3）Boolean：拥有过两个值true和false。  
    4）Number：数值类型。  
    5）String：字符串类型，在JavaScript中没有char类型，因此在JavaScript中使用""和‘’都表示字符串。  
  
10、typeof在JavaScript中是一个单目运算符，用于运算变量的类型，运算结果只有5个值：undefined、boolean、number、string、object。前四个返回值对应JavaScript的原始数据类型（Undefined和Null返回值都是undefined），最后一个返回值object对应对象变量，只要是对象的返回值都是object。  
  
11、JavaScript中3中类型强制转换：Boolean(value)、Number(value)、String(value)。如果在强制类型转换前加上new标识符则不是强制类型转换，而是构造对象，如new
String(value)是构造一个String对象，使用typeof运算符的结果是object而不是string。  
  
12、JavaScript中的所有对象都是从Object对象继承过来，Object中属性是不可枚举的，使用Object.propertyEnumerable("element")返回的结果是false，所以不能使用for..in语句遍历Object中的属性。  
  
13、JavaScript是一种动态编程语言，可以动态添加已有对象的属性和方法，也可以动态删除对象的属性和方法。比如object为已有的对象：  
    1）动态添加对象的属性和方法：object.newElement = value 或者 object["newElement"] = value。其中newElement为新的属性或者方法的名称，value新添加属性或者方法的类型，可以是JavaScript的原始数据类型，也可以是对象。当newElement为方法时，value为Function对象。  
    2）动态删除对象的属性和方法：使用单目运算符delete object.newElement。  
  
14、JavaScript的Array数组具有方法sort()用于对Array数组进行排序，它的排序方法是将数组中的元素先转换成字符串，再根据字符串从小到大的排序规则排序。因此，这个方法对于数字类型的排序不准确，sort方法也支持传递一个function对象作为参数来修正sort的排序方法。可以定义一个Function对象比较两个参数，返回值为正数、负数和0三种，sort根据返回结果排序，原理类似于Java的compare类。  
  
15、JavaScript中定义对象的方法（JavaScript中没有类的感念，只有对象）：  
    1）基于已有对象进行扩充属性和方法，原理是根据JavaScript语言的动态性，向已有对象动态添加新的属性和方法来形成自定义的对象。但这种做法的缺点是不能重复构造这种对象，也就是一次性和临时性的。另外，定义一次性对象有种便捷方法就是使用{}括号，如：var object = { name1:value1;name2:value2...}，其中name为对象属性名称，value为属性内容，可以使原始数据类型或者对象（name为方法时，value为function对象）。  
    2）工厂方式，利用function来构造对象。既然没有类，那么就使用方法将扩充已有对象的代码放到function中，并将扩充的对象以返回值的形式返回，那么只要调用function就能够重复构造这种对象。 如：  

    
    
    function createObject(){
            var object = new Object();
            object.name = "name" ;
            object.get =function(){};
            return object;
    }

    调用的时候直接使用var object = createObject();  
    以上做法有一个缺点就是每构造一个对象出来都会创建一份对象的属性和方法，但是方法不需要每个对象都创建一份，同类对象共享一份就够了。解决的方法就是把对象内的方法定义移动到function之外，再在对象内调用外部的方法。  
    3）构造函数方式：与工厂方式很类似，不同之处在于function中不需要定义个对象变量，也不需要使用return语句，并且在构造对象使不是调用构造方法，而是使用new关键字。 如：  

    
    
    function createObject(){
            this.name = "name";
            this.get = function(){};
    }

    调用的时候使用var object = new createObject();  
    这么做的原因在于使用function定义的本来就是一个Function对象，使用new构造的时候就会默认构造一个Function对象出来，function中的this就是它本身的这个Function对象，最后也会默认返回它自身这个对象。  
    4）原型方式（prototype）：这种方式与扩充已有对象的方式很类似，不同的是扩充已有对象的方式扩充的只是当前的这一个对象，而原型方式扩充的是当前类的原型。如：
    
    
    function Person(){}
    Person.prototype.name = "name";
    Person.prototype.get = function(){};

    这么一来使用new Person()构造的Person对象就拥有过name和get两个属性。需要注意的是使用原型添加的属性在构造出来的所有对象中共享。如果是个对象，那么构造出来的所有对象的这个属性指向的都是同一个对象，一处改变就会影响其它对象；但对于常量属性来说就不会，修改的对象属性指向新的常量不会影响其它对象。使用原型方式还有一个缺点在于无法在构造函数中为属性赋初值，只能在对象生成之后再去改变。  
    5）原型方式+构造函数方式：为解决原型方式共享属性的缺点，将属性使用构造函数方式实现，将方式使用原型方式实现，这样就更加贴近Java的规则。  
    6）动态原型方式：为了将方法放进构造函数内部，又不会每构造一个对象都会重新生成一个方法，因此，引入一个标识量来判定是否第一次构造，第一次构造就使用原型方法添加方法，不是第一次就跳过添加方法。如：
    
    
        function Person(){
            this.name = "name";
            if(typeof Person.flag == "undefined"){
                Person.prototype.get = function(){};
                Persion.flag = ture;
            }
        }

  
16、JavaScript的继承：  
    1）对象的冒充，关键在于JavaScript的this标示符表示的是当前对象或者调用当前Function对象的对象。如：  

    
    
            function Parent(name){
                this.name = name;
            }
            function Child(name){
                this.method = Parent;
                this.method(name);
                delete this.method;
            }

    这个例子中Child先定义了属性method类型为Parent，然后通过method属性调用Parent方法，关键就是这句this.method(name)，因为function中的this在被其它对象调用的情况下代表的是这个对象，所以this.method(name)就是Child对象调用Parent，Parent中的this也就变成了Child，相当于Child拥有了name属性。通过这个原理，只要Parent中使用this定义的属性和方法统统都能够被Child继承过来。  
    对象冒充的缺点在于多级继承的时候容易出现混乱，当子对象中存在与父对象相同名称的属性时就会覆盖父对象的属性。  
    2）call方式继承：每一个Function对象都具有call方法，call方法的第一个参数是传递给Function对象中的this，剩下的参数逐一赋给Function中的其它参数。如：  

    
    
            function Parent(name){
                this.name = name;
            }
            function Child(name){
                Parent.call(this,name);
            }

    这个例子中Child的对象就具有了Parent的属性name。  
    3）apply方式继承：每一个Function对象也都具有apply方法，它的使用与call方法基本一样，第一个参数与call方法第一个参数功能一样。两个方法的不同之处在于后续的参数，apply只有两个参数，第二个参数是一个Array数组，数组中的元素就是传递参数，相当于对call方法的后续参数组合成一个数组而已。两个方法之间本质上基本没有区别。  
    4）原型链方式继承：这种方法的关键在于每个对象具有的属性prototype，对象.prototype可以看做是定义对象内部的this。如：  

    
    
            function Parent(){};
            Parent.prototype.name = "hello";
            function Child(){};
            Child.prototype = new Parent();

    以上例子，Child通过prototype赋予Parent对象等于Child与Parent对象一样，只要再使用Child.prototype.Element添加Child自己的属性就等同于继承了Parent又添加了自己的属性。但是这种方式的缺点在于无法给构造函数传递参数。  
    5）混合方式（最推荐的方式）：混合call\apply和原型继承方式。如：  

    
    
            function Parent(hello){
                this.hello = hello;
            }
            Parent.prototype.sayHello = function(){};
            
            function Child(hello,world){
                Parent.call(this,hello);
                this.world = world;
            }
            Child.prototype = new Parent();     
    

    以上例子中，最后还需要使用Child.protytype = new Parent()是因为call方法没有继承到sayHello，sayHello是通过prototype在function外部定义的，call只能继承到function中this定义的属性。  
  
17、JavaScript的console对象只能用于FireFox浏览器，这个对象能够像FireFox的FireBug插件的控制台输出相关信息：  
    1）console.log(message)：输出日志信息。  
    2）console.info(message)：输出信息。  
    3）console.warn(message)：输出警告信息。  
    4）console.error(message)：输出错误信息。  
    5）console.debug(message)：输出调试信息。

  

