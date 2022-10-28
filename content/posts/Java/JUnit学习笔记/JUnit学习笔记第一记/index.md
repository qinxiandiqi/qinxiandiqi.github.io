---
# type: posts 
title: "JUnit 学习笔记 第一记"
date: 2014-06-29T12:11:31+0800
authors: ["Jianan"]
summary: "1、在开发一个大型项目的时候，一般都会把项目实现逻辑分层由不同的开发人员各自实现，不同层次之间会发生互相的调用等等。这么一来，一旦一个部分出现错误，就可能导致其它部分的错误，为了查明错误的来源往往需要逐层排查，耗费很多时间和成本。因此，有必须保证不同开发人员提交的程序代码的正确性，至少是符合开发人员自己的要求。这个保证就需要开发人员对自己开发的代码进行测试，JUnit框架就是为了解决这个问题而出现"
series: ["JUnit学习笔记"]
categories: ["JUnit学习笔记"]
tags: ["JUnit", "测试", "单元测试", "Java"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 1111
comment_num: 0
---

1、在开发一个大型项目的时候，一般都会把项目实现逻辑分层由不同的开发人员各自实现，不同层次之间会发生互相的调用等等。这么一来，一旦一个部分出现错误，就可能导致其它部分的错误，为了查明错误的来源往往需要逐层排查，耗费很多时间和成本。因此，有必须保证不同开发人员提交的程序代码的正确性，至少是符合开发人员自己的要求。这个保证就需要开发人员对自己开发的代码进行测试，JUnit框架就是为了解决这个问题而出现。

  

2、JUnit版本有3.8和4.x系列，两个系统之间差别比较大，4.x版本对JUnit进行很大的修改，包括很多类包的名字，而且增加注解，所以使用JUnit测试框架的时候的要明确使用的JUnit版本。

  

3、JUnit的实现就是将目标代码与测试代码分开，由测试代码来测试目标代码执行过程是否符合开发人员的要求。分开的好处是测试通过后发布代码，只发布目标代码隐去测试代码，方便简洁。

  

4、Eclipse内置了JUnit单元测试框架，包括3和4两个版本系列，对项目使用JUnit单元测试时，要先通过add
Library选择JUnit版本，Eclipse会自动添加JUnit库到工程。

  

5、JUnit的最佳实践方法：  
1）项目中对应默认源代码src文件夹，新建名为test的source folder源代码文件夹，该文件夹用于存放测试源代码。  
2）目标类与测试类应该放在同一个包下面，这样可以在测试类中不必导入源代码所在包。方法是在test源代码文件夹中建立与src目标源代码文件夹包名字相同的包，同个项目中不同源代码文件夹中名字相同的类包是等同于在同一个类包中的，虽然在物理上目标源代码文件和测试源代码文件夹不是同一个文件夹，但是他们生成的class文件都是存放在bin文件夹中。  
3）测试类的命名规则：在目标类的类名前或者类名后添加Test作为对应测试类的类名。

  

6、JUnit单元测试的目的不是为了证明开发人员的代码是正确的，而是证明开发人员的代码没有错误。JUnit单元测试能通过只能说明目标代码通过了开发人员为其目的设计的测试代码，经得起测试代码的测试，但是不能保证经过测试的目标代码就一定正确，可能还存在开发人员测试代码没有涉及到的问题。

  

7、JUnit3.8的测试类都必须继承junit.framework.TestCase类，该类是一个抽象类，继承了junit.framework.Assert和实现了接口junit.framework.Test。TestCase类中有两个重要方法：  
1）setUp()：测试类中每一个测试方法调用之前调用，测试类可以通过重写该方法来实现对每个测试方法都需要的一些初始化工作，比如：每一个测试方法都需要构造目标类对象，可以在setUp方法中构造目标类对象，这么一来，每个测试方法调用之前都能获得一个目标类对象，并且每个测试方法或的目标类对象都不相同（每个测试方法前都调用一次setUp，初始化一次目标类对象），以符合测试类每个测试方法都互相独立的要求）。  
2）tearDown()：与setUp()方法对应，每一个测试类方法结束后都会调用一次tearDown方法，可以通过重写tearDown方法来完成一些每个测试方法都需要的收尾工作。

  

8、JUnit3.8中，所有的测试方法都必须满足以下原则：  
1）测试方法的属性必须都是public的。  
2）测试方法的返回类型必须都是void。  
3）测试方法必须没有方法参数。  
4）测试方法名必须以test开头。

  

9、在测试方法的测试代码中，常用到的类是junit.framework.Assert，该类也是TestCase类的父类。Assert类中提供了大量重载的静态方法assertEquals，该方法用于比较各种类型的值，JUnit的判断就是根据这个方法来判断是否符合开发人员预想的要求。在测试代码中，通过对目标类对象的操作，获得一个结果，再调用assertEquals相应的重载方法比较结果与自己的预期值。这样，在使用JUnit运行的时候，JUnit会根据每个测试方法中的assertEquals的结果输出通过（green
bar）还是错误（red bar）。

  

10、JUnit3.8单元测试要求每个测试类都必须互相独立，每个测试方法都必须互相独立，各自之间不存在任何依赖关系，包括测试方法的执行顺序等等，因为每个测试方法都针对于一个问题来设计，如果存在依赖关系，出现错误的话，将很难排查。

  

11、JUnit3.8单元测试的类Assert类中的静态方法fail（），一旦测试代码中调用此方法就
一定会在JUnit结果中输出测试失败，常将此方法用于测试抛异常。方法是在目标代码中可能会抛出异常的地方将异常throws，在测试代码中使用try-
catch结构捕捉异常。当希望测试不抛出异常的时候，将fail方法放置到catch中，如果没有抛出异常则正常测试；当希望测试抛出异常的时候，将fail放置到try的测试代码后，并在try-
catch结构外使用Assert.assertEquals比较抛出的异常（assertEquals方法可以比较Object），如果没有抛出异常就会在测试结果中显示测试失败。

  

12、JUnit3.8单元测试的junit.textui.TestRunner.run(Class
classtest)静态方法用于以命令行的方式测试类测试类classtest，传入参数是类对应的Class。该方法将以命令行的方式输出测试结果到控制台上，由于是命令行方式，所以测试效率比较高，一般都采用这种方法。

  

13、JUnit3.8单元测试的junit.awtui.TestRunner.run(Class
classtest)静态用于以可视化界面的方式测试测试类classtest，同样传入的参数是测试类的Class。该方法将以可视化图形界面的方法将测试结果输出。

  

14、JUnit3.8单元测试的一个原则是测试类测试后对程序所有数据应该没有任何影响，也就是说测试之前是什么状态，测试之后应该就还是什么状态。比如，测试类需要用到数据库一些数据，在测试后之后这些数据库的数据应该没有任何印象，原来是什么样子，测试之后应该就还是什么样子。

  

15、在JUnit3.8单元测试的测试代码中，由于测试代码测试目标代码的要通过创建目标类对象，再通过目标类对象调用目标代码来进行测试，所以在测试代码中无法直接测试目标类中的private方法。为了能够测试目标类中的private方法，可以由两种方法：  
1）将目标类中的private方法改为public方法，测试完后再改回来，不建议使用这种方法，不符合JUnit测试不影响目标代码的原则。  
2）通过反射机制调用目标类中的private方法，强烈建议使用的方法。

  

16、JUnit3.8的测试套件测试自动化：当要执行多个测试类中的代码时，一个一个测试类进行JUnit运行测试的效率很低，所以JUnit3.8提供了测试套件可以一次执行多个测试类的测试。测试套件的使用方法需要构造junit.framework.TestSuite类对象，在这之前需要先设计一个与普通测试类一样继承TestCase的测试类，并在测试类中重写public
static Test
suite(){}方法，在该方法的方法块中构造TestSuite对象，并使用TestSuite对象的addTestSuite（Class
TestClass)方法添加需要执行的测试类的Class对象，最后return构造的的TestSuite对象。这么一来，只要JUnit3.8执行这个测试类，就会自动调用suite这个方法，相当与一次将这个测试类中TestSuite包含的其它测试类都执行一遍，实现测试自动化。

  

17、JUnit3.8单元测试的junit.extensions.RepeatedTest继承了TestDecorator类，使用了装饰设计模式。RepeatedTest的构造方法RepeatedTest(Test
test,int
repeat)可以构造一个连续将test测试类执行repeat次的RepeatedTest对象。需要注意的是，RepeatedTest重复执行的是测试类test中的某个测试方法，因此test测试类对象必须是使用Test(String
str)方法构造的一个Test对象，这个对象代表了要执行的测试方法是test测试类中方法名为str的测试方法，这样，RepeatedTest对象才能知道要执行test对象中哪个方法repeat次。另外，TestSuite类中除了addTestSuite(Class
class)方法之外，还有addTest(Test test)方法可以添加一个实现Test接口的测试对象，通过addTest(Test
test)方法可以向TestSuite添加一个RepeatedTest对象。

  

18、JUnit4全面引入注解Annotation来执行编写的测试，这是JUnit3.8与JUnit4最大的区别。

  

19、JUnit4并不像JUnit3.8要求每个测试了都必须继承TestCase类，而且在测试类中的测试方法也不要求以test开头。JUnit4中确定一个方法为测试方法的标识是一个被public修饰且返回值为void类型的方法前是否有注解@Test。如果有，那么这个方法就是测试方法，可以被JUnit运行。（虽然JUnit4并没有要求测试方法名前添加test，但是为了是方法名具有表征性，推荐还是要在测试方法名前添加test）。

  

20、JUnit4使用注解@Before代替JUnit3.8中的setUp方法，只要是被@Before注解的方法在每个测试方法运行之前都会先被调用；类似的，JUnit4使用注解@After代替JUnit3.8的tearDown方法。同样，虽然JUnit4没有限制方法名，但还是推荐使用setUp和tearDown方法名。

