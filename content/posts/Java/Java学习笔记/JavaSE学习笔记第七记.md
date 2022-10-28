---
# type: posts 
title: "JavaSE学习笔记 第七记"
date: 2014-06-01T11:19:26+08:00
authors: ["Jianan"]
summary: "代理模式和策略模式的区别，Java注解的使用，JUnit的使用，Java一场的使用。"
series: ["Java学习笔记"]
categories: []
tags: ["Java", "Java注解", "Java异常", "JUnit"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 949
---

## 2012-07-25

1. 每一个动态代理类都对应一个动态调用处理器InvocationHandler，因为动态代理类不具备方法的实现，动态代理类的方法依赖于动态调用处理器InvocationHandler的invoke方法来实现。

2. 静态代理的真实类和代理类关系是：真实类和代理类都是抽象角色的子类或者实现，并且代理类含有真实类的引用，通过代理类操作真实类。而策略模式对抽象角色子类的使用都是通过抽象角色来使用，策略模式的子类之间不能互相使用，因为一个子类没有包含另一个子类的引用。

3. 动态代理的真实类和代理类关系与静态代理的真实类和代理类关系类似，区别在于动态类的定义和实现逻辑与静态代理类不同。静态代理每一个真实类都必须手动定义一个代理类，并且每一个静态代理类中都必须重写真实类中的每一个方法（即抽象角色中的方法），这么一来，一旦真实类的数量多起来，需要手动定义的类数量将急剧上升，造成类数量的臃肿。而动态代理类的实现是通过InvocationHandler动态调用管理器和Proxy类在运行时动态定义和构造，定义一个包含Object引用变量的InvocationHandler实现类，就可以动态定义和构造任意真实类的代理，并且由于动态代理类的方法实现是交付给InvocationHandler的invoke方法实现，而InvocationHandler的invoke方法又是由这个方法中接收的参数method的invoke实现，所以动态代理类不需要对应真实类的每一个方法重写，一律使用InvocationHandler的invoke动态实现真实类方法的重写和调用。如此一来，动态代理的好处就是不用手动定义每一个代理类和代理类中的方法，被代理的独享可以在运行时动态改变，动态代理类实现的接口也可以在运行时改变，从而实现了灵活的动态代理关系，降低了定义类的数量。

4. Java Annotation：Java注解，JDK1.5新增的特性。

5. JDK1.5提供的三个常用注解：
    1. @Override（位于java.lang.Override），只能注解方法，在方法定义前使用该注解表示这个方法必须重写一个父类或接口的方法，如果没有则编译不通过，该注解可达到在编译时检查有无重写方法的作用。
    2. @Deprecated（位于java.lang.Deprecated），在方法定义前使用该注解表示该方法不建议被使用（一般是有更好的替代方法，或者该方法不够安全），注解后该方法名会被删除线划上，并且在调用该方法时会有不建议使用的警告。
    3. @SuppressWarnings（String[]）（位于java.lang.SuppressWarning），可注解除了注解类型之外的其它所有类型，接收一个字符串数组，可注解一个类或者方法。注解的功能由接收的字符串数组决定，常用的参数有“unchecked”表示压制检查警告，“Deprecated”表示压制使用不建议使用方法警告等等。当一个类被一个@SuppressWranings注解后，类中每一个方法都默认被这个注解注解，当类中方法还被自己的注解注解时，这个方法同时被两个注解注解。

6. 定义自己的注解类型：与定义接口类似，但是使用@Interface标志代替Interface，如public @Interface MyAnnotation{注解内容}。

7. 当自定义注解中包含定义属性时，要在属性名后加（），如“public String value（）；”，否则编译错误。如果要设定注解的默认值，要在属性名的()后使用“default + 属性值”的方式设置属性的默认值。

8. 使用带属性的自定义注解时，要在注解名后加（），并在括号内传递注解属性值，如@MyAnnotation（“myAnnotation”）。同时，若注解的属性名为value时，使用注解的括号内可直接写属性值，否则必须使用name=value的形式对应传递注解的属性参数，当属性有多个的时候，按照这个形式用逗号隔开就可以。

9. 自定义注解的方法只有使用@Interface一种，使用这个方法自定义的注解编译器默认会继承java.lang.annotation.Annotation接口。但是如果手动写一个接口继承这个java.lang.annotation.Annotation，这个接口也不是注解，就算是原来的这个java.lang.annotation.Annotation接口也不是一个注解。

10. 如果自定义的注解与使用注解的类不在同一个包中，那么同样需要把包含注解的包导入到使用注解的类中，导入方法与导入类包的方法相同。

11. 自定义的注解不能继承其它的Annotation类型（其它已定义的注解）或者接口，但是可以使用其它已定义的注解来注解自定义的注解。

12. 注解@Retention（位于java.lang.annotation.Retention):，只能用于注解注解类型，使用@Retention注解自定义注解类型可以告知编译器如何处理自定义的注解类型信息。

13. 枚举类型RetentionPolicy（位于java.lang.annotation.RetentionPolicy）：包含三个枚举常量SOURCE、CLASS、RUNTIME。SOURCE常量表示编译程序只在编译时使用注解信息，但不将注解信息保存到class文件中，所以不会在JVM中被读取；CLASS枚举常量表示编译器会在编译的时候使用注解信息，并且会将注解信息保存到class文件中，但是在VM加载class文件时不读取；RUNTIME枚举常量表示编译器在编译时会使用注解信息，并且会将注解信息保存到class文件中，在JVM运行时加载class文件会通过反射机制的API获取注解信息。

14. @Retention注解中包含一个RetentionPolicy枚举类型的属性（属性名为value，所以使用这个注解时可以直接传递参数），并且属性值默认是CLASS枚举常量。通过使用@Retention注解并制定其枚举常量来注解自定义注解类型，以此达到控制编译器处理自定义注解类型信息方法的目的。

15. 通过实现反射机制的相关类获取@Retention（RetentionPolicy.RUNTIME）注解的注解类型信息：实现反射机制的相关类Class、Method、Constructor、Field、Package等都直接或间接实现了AnnotatedElement接口，AnnotatedElement接口中提供了四个与Annotation相关的方法。因此，使用反射机制的相关类调用实现了的AnnotatedElement接口方法，可以获得该反射相关类代表的部分上是否存在@Retention（RetentionPolicy.RUNTIME）注解的注解类型以及其注解信息。

16. AnnotatedElement接口四个方法：
    1. <T extends Annotation\> getAnnotation(Class<T\> annotationClass>，如果存在annotationClass（该参数为注解的.class）注解类型的注解，则返回这个注解。
    2. Annotation[] getAnnotations（），如果存在注解则以注解数组形式全部返回。
    3. Annotation[] getDeclaredAnnotations()返回直接存在于此元素上的所有注释。
    4. boolean isAnnotationPresent<Class<? extends Annotation>>，接收一个Annotation类型，如果该元素上存在这个Annotation注解类型，则返回true，否则返回false。

17. 通过反射机制相关类获取注解引用变量后，可以利用这注解引用变量获取该注解中属性的值，获取方法为“注解引用变量.注解属性名()”，与对象获取属性值的方法后多加一个括号。

18. 注解@Target（ElementType[]），只能用于注解其它注解类型，接收一个ElementType枚举常量数组，表示被注解的注解类型能用于注解什么元素，由ElementType数组值决定。

19. ElementType枚举类型常量值：ANNOTATION_TYPE（只能注解注解类型）、CONSTRUCTOR（注解构造方法）、FIELD（注解属性）、LOCAL_VARIABLE（注解局部变量）、METHOD（注解方法）、PACKAGE（注解包）、PARAMETER（注解参数）、TYPE（注解类、接口、注解类型、枚举声明）。

20. @Documented只能用于注解注解类型，被它注解的注解类型所注解的元素在生成JavaDoc帮助文档的时候，会在相应元素上显示这个注解类型。如果没有使用@Documented注解的注解类型在生成JavaDoc帮助文档的时候不会保存到文档上。

21. Eclipse生成JavaDoc方法：Project-Generation Javadoc

22. @Inherited注解只能注解注解类型，当一个元素被它注解的注解类型注解后，继承该元素的元素能够被继承这个注解类型，反之则不会继承。

--- 

## 2012-07-26

1. JUnit：Java单元测试，经典版本有JUnit3.8（完全基于反射机制设计）和JUnit4.x（基于反射机制和注解设计）

2. 使用JUnit需要导入JUnit库（JUnit.jar）。

3. 使用JUnit3.8的类需要导入包import junit.framework.TestCase，并且使用的类需要继承TestCase类，同时需要进行单元测试的方法名必须以test开头，如果不以test开头则进行JUnit测试的时候不会测试这个方法。

4. 使用JUnit4.x的类需要导入包org.junit.Test，并且在需要测试的方法前添加注解@Test，那么使用JUnit测试的时候就会测试这个方法，否则不会测试。

5. JUnit原理（执行步骤）：
    1. 先获得需要测试类的Class对象。
    2. 通过Class对象获取测试类中所有public类型方法的Method数组。
    3. 遍历Method数组，取出每一个Method对象。
    4. 如果是JUnit3.8，则判断Method对象对应的方法名是否是test开头，是则执行这个方法，否则不执行；如果是JUnit4，则会调用每一个Method对象的isAnnotationPresent（Test.class），判断方法是否被@Test注解，是则调用method.invoke（）执行该方法，否则什么都不做。

6. 异常类：java.lang.Exception，java中所有的异常类都直接或间接的继承Exception。

7. 异常和错误：即Exception和java.lang.Error，它们都继承与java.lang.Throwable类，Exception异常是指可以处理的程序错误，而Error错误是不可处理的程序错误。

8. 运行时异常：也叫unchecked异常，java.lang.RuntimeException（直接继承Exception）或者直接及间接继承RuntimeException的异常，是运行期间抛出的异常，此类异常可以不必进行自行处理，JVM会自行处理，一般也不建议进行自行处理。

9. 非运行时异常：也叫checked异常，所有直接或间接继承Exception但非继承RuntimeException的异常都叫非运行时异常，此类异常必须自行进行异常处理，可以通过try-catch-finally处理，也可以使用throws处理。

10. 异常抛出的位置：
    1. 当程序运行的代码行出现异常时，会自动生成相应的异常类并抛出。
    2. new一个异常类，并使用throw关键字抛出。

11. 处理异常的方法：
    1. try{}catch（Exception e）{}finally{}：
    将可能出现异常的代码放置到try后的{}代码块中，如果其中代码出现异常，则会在出现异常的代码行生成一个对应的异常对象并抛出和不再执行try中出现异常之后的代码。此时会按照catch排列顺序遍历try之后的catch（try之后可以跟多个catch，也可以将catch省略，但省略后必须跟finally），当其中一个catch参数异常类类型与抛出异常类类型符合时，则执行这个catch代码块中的方法。不存在抛出类型与所有catch不匹配的情况，因为若存在可能抛出的异常与所有catch不匹配时，程序在编译的时候根本不能编译通过。另外，匹配catch参数的时候也只会有一个catch匹配，因为每次最多只有一个异常抛出。特别的，由于匹配catch是按照前后顺序匹配，如果多个catch的异常参数类型中存在继承关系，那么必须要将父类异常类型参数的catch排在子类catch之后，否则子类catch异常将永远没有机会调用，编译时不能通过并会提示子类catch无法到达，而没有继承关系的catch则先后顺序没有关系。try-catch之后，无论异常是否处理，都会执行finally中的代码，即使是try代码块中存在return，也会在return语句调用之前先执行finally代码块。但是如果try代码块中存在System.exit（0）语句，则不会执行finally代码块，因为exit（0）是结束JVM的语句。整个try-catch-finally执行完毕后，会继续执行这个处理结构之后的代码（如果处理结构中没有catch则不会执行结构之后的代码）。注：一般不将声明变量的代码放置到try代码块中，如果在try代码快中声明变量，那么在try-catch-finally结构之外使用声明的变量将会出现编译错误。
    2. 定义类时使用throws抛出相应异常：如果定义一个类的时候，类中代码可能抛出异常，可以在定义类的参数列表括号后使用“throws+对应异常类[,对应异常类]”来将异常抛出但是不处理这个异常。处理异常的方法在于调用该类方法的方法中，要用try-catch-finally结构处理，如果调用该类方法的方法没有处理，继续使用throws将异常抛出到上一级调用方法。如果都没有提供处理方法，继续用throws抛出知道main方法都继续throws，那么这个异常会JVM处理。
    3. try-catch-finally结构和throws组合：在catch代码块中再使用throw抛出一个异常，并用throws将异常抛到方法外。这种做法通常用于捕获代码自动生成的异常，并将这个异常重新包装成自定义的异常类，再以自定义的异常类抛出去处理，有利于形成具有特定处理信息的异常类。

12. 常见的运行时异常：NullPointerException，空指针异常，由于调用了某个对象的方法，但是该对象引用的值为null所导致。

13. 自定义异常类需要继承一个异常类，一般是Exception，也有继承RuntimeException，但比较少。Exception中含有一个带参数的构造方法Exception（String str），str为异常的描述信息，使用Exception继承的printStackTrace（）方法可以打印出异常描述信息和异常出现位置（JVM处理异常一般也是调用这个方法）。

14. 使用自定义异常类的方法通常是利用一些判断结构，如if结构，在判断出出现自定义异常的地方使用new，构造一个自定义异常类，并用throw将其抛出，同时在该方法声明后面使用throws抛出。之后处理自定义的方法与其他异常类的处理方法一致。
