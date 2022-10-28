---
# type: posts 
title: "JavaSE学习笔记 第六记 —— 代理模式"
date: 2012-10-14T08:24:27+08:00
authors: ["Jianan"]
summary: "Java的枚举类型，反射的的使用，代理模式（静态代理和动态代理）。"
series: ["Java学习笔记"]
categories: []
tags: ["Java", "枚举", "反射", "代理模式"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 1061
---

## 2012-07-23

1. 枚举类型(JDK1.5增加的新特性）：严格来说并不是类，但是具有跟类相同的级别。类似于类定义，使用与类class相同级别的关键字enum来定义枚举类型，例如：public enum Color{RED, BLUE}。可以单独用一个java源文件定义一个枚举类型。定义声明之后，使用枚举的方法都与类的使用方法一致。

2. 枚举类型提供了两个静态方法：values()和valueOf()。values()返回枚举类型的数据数组。valueOf()方法将一个与成员变量名称相同的字符串自动转换成对应的枚举成员。

3. 枚举类型的本质是一个继承java.lang.Enum的类，枚举类型的每一个枚举成员实质上就是一个枚举类，并且这个枚举成员是final和static以及public修饰的，所以枚举成员可以直接用“枚举类型.成员”使用。从本质上，在枚举类中照样可以定义普通类成员变量和方法，包括构造方法，一旦定义了构造方法，那么定义枚举成员的时候也要使用构造方法的形式（本质是构造枚举对象）。而且，枚举类型中的成员（类）不同于普通类，它们是在编译的时候直接生成，编译后在运行时不能再改变，也就是说在编译后枚举类型就已经完全确定下来了。

4. JDK1.5增加了类似C语言的格式化输出，System.out.printf("%d,%s",a,b)。

5. 泛型EnumSet集合，只能接收Enum类或者其子类的类型参数，如：EnumSet<Color\>。向EnumSet枚举集合添加元素可使用of(element)，of的参数列表element只能接收对应枚举类的枚举成员。EnumSet的complementOf(EnumSet enumset)方法返回接收的枚举类型中除了enumset包含的枚举成员之外的枚举成员集合。EnumSet的noneOf(Enum)方法创建一个空的Enum枚举类型的集合，使用add方法添加的时候只能添加Enum枚举类型的成员。

6. 普通的集合同样可以将枚举类型当成类型参数接收。

7. 静态导入（JDK1.5增加新特性）：使用import static导入其他包内的静态成员变量或者静态方法，路径要一直指定到具体的静态成员和静态方法上，那么在使用的时候可以直接使用不需要具体类名.成员。与普通import的区别在于普通导入的路径指定到具体类就可以了，静态导入要一直指定到成员和方法上，并且只能导入静态的成员变量和方法，另外静态导入使用时无需写具体类名。

8. java的反射机制：使用new构造类实例和通过类调用类的方法等过程，在编译器编译的时候就已经知道会构造类或使用类的方法，但是反射机制不一样，反射机制强调是在运行时动态创建类对象和动态调用类方法，也就是说，编译时并不知道要创建什么类，调用什么类方法，直到运行时才能知道代码要创建类或调用类。通过反射机制，能够调用类的私有成员和方法。

9. 反射机制主要通过5个类来实现：
    1. Class<T\>类，位于java.lang包中，代表了一个类，每一个类中都已有一个成员变量class保存该类所属的Class类，一个程序中的多个同类型对象都共享同一个Class。
    2. Field类，位于java.lang.reflect包中，代表该类的成员变量，在反射机制中使用类的成员变量需要通过Field类使用。
    3. Method类，位于java.lang.reflect包中，代表类的方法，在反射机制中使用类的方法需要通过Method类使用。
    4. Constructor类，位于java.lang.reflect包中，代表类的构造方法，在反射机制中通过该类使用类的构造方法。
    5. Array类，位于java.lang.reflect包中，提供了动态创建数组，以及访问数组元素的静态方法。
    动态机制之所以能够在运行时实现，在于这五个类提供动态实现的方法，在运用动态机制的时候，必须通过以上五个类来间接操作。

10. 反射机制的相关使用过程：
    1. 首先要获取一个类的Class，Class类中提供了一个static方法forName（String）方法，参数为要获取具体的类完整类名，能够返回要获取具体类的Class，如Class<?> classType = Class.forName("java.lang.Object")；另一种获取Class的方法是通过每一个类的都具有的成员变量class，直接从“类名.class”中获取；还有一种方法是利用从Object类继承下来的final方法getClass()方法返回调用该方法对象的Class，此方法需要用对象调用。
    2. 反射机制创建类实例，Class类中提供了方法newInstance()借用该Class对应类中不带参数的构造方法创建类实例，并返回对象引用。如果要使用带参数列表的构造方法，则需要借助Constructor的newInstance（Object<T>...）方法——首先利用Class的getConstructor(Class<?>...)方法获取带相应参数构造方法的Constructor（通常用Class[]数组做参数，如果是使用不带参数构造方法，要传递Class[]{}空数组），再利用该Constructor的newInstance（Object<T\>...）构造新实例（通常newInstance参数为Object数组，即使是不带参数也要使用空Object[]{}数组表示空），需要注意的是可变参数数组的元素要前后对应。
    3. 反射机制使用类成员变量，通过Field获取。Class提供了getDeclaredFields()返回代表所有成员变量的Field[]数组，或者通过getDeclaredField(String）返回指定变量名的Field对象。Field的方法getName()可以返回该Field代表成员变量的名称。
    4. 反射机制使用类方法，通过获取Method操作，Class类中提供了getDeclaredMethod()方法获取该类中所有方法的数组，即返回值为Method[]；Class提供的getMethod(String，Class<T>...)接收一个方法名字符串和可变参数Class<T>（可变参数接收多个Class，通常用Class[]数组传递），可以返回一个方法名为String，参数为Class...的Method。获取Method对象后，Method类中提供了方法invoke(String，Object...)，String表示调用的是哪个对象，Object...可变参数接收一个或多个Object类型参数（通常以Object数组传递），返回值是一个Object类型（具体使用返回值时可再强制转换）。
    5. 反射机制构造数组，通过Array的newInstance()方法。Array重载了两个newInstance静态方法，其中newInstance(Class<T\> componentType,int length)构造一个一维数组，长度为length，元素类型为Class<T\> componentType关联的类。如果创建的是多维数组，要使用newInstance(Class<?>... componeneType,int... dimensions)，componentType表示数组的比较类型对应的Class（已知数组可通过Class的getComponentType()返回该数组的比较类型，实际也就是数组的元素类型，三维数组的比较类型是二维数组，二维数组的比较类型是一维数组，不是数组的比较类型为null），dimensions使用散列int或者int数组表示多维数组各维度的长度（从左到右为高维度到低维度）。Array的get(array，int...)方法可获取数组array对应维度的值。

11. Class对象在构造该类的实例之前就已经存在，一个Class对象是JVM在加载一个类的时候自动创建的，里面包含了该类的所有信息，包括成员变量和方法，是java语言至关重要的类。

12. 原生数据类型的包装类.TYPE返回的是包装类对应的原生数据类型，.class返回的是class+包装类的完整类名。

---

## 2012-07-24

1. Class中getDeclaredXxx与getXxx的区别：getXxx只能返回public修饰的成员变量和方法，getDeclaredXxx可以返回任意修饰符修饰的变量和方法。反射机制可以破坏类的封装性，使用类的私有成员和私有方法，此时要使用getDeclaredXxx获取相应的Method、Field、Constructor才有可能。

2. Method、Field、Constructor都继承于AccessibleObject，该类中提供了一个方法setAccessible（boolean），当boolean为true时，表示强制取消检查访问限制，当boolean为false时，则正常检查访问限制。只有通过setAccessible()设置为true取消访问检查，才有可能破坏类的封装性，使用类的私有成员和私有方法。

3. 类中定义的set和get方法，本质上也是使用反射机制才有可能实现。

4. Field中的set（Object obj，Object value）可以设置对象obj中该Field关联的成员变量值为value。如果是私有成员，也需要先用setAccessible（true）强制取消访问权限检查才可实现。

5. native修饰的方法表示本地方法，即不是使用java来实现，而是由C或C++来实现。

6. Class的构造方法为private修饰，所以Class不能手动创建。

7. 类调用类对象的getSuperclass可以获取父类的Class。

8. 代理模式：为其它对象提供一个代理以控制这个对象的访问，代理相当于客户端与目标对象之间的中介，并带有自己附加的一些功能。

9. 代理模式设计的角色：
    1. 抽象角色：声明真实对象和代理对象的共同接口或抽象类。
    2. 真实角色：即真实对象，是代理角色所代表的对象，是客户端最终要 引用的对象。
    3. 代理角色：代理角色内部包含对真实对象的引用，从而能够操作真实对象。同时，代理对象提供与真实对象相同的接口以便能够在任何时刻代替真实对象和使用真实对象。并且，代理对象可以在执行真实对象操作的时候附加自己的操作，相当于代理对象是对真实对象的封装。

10. 静态代理设计过程：
    1. 定义抽象角色，一个真实对象和代理对象共同要实现的抽象类或者接口。
    2. 定义真实角色，也就是要被代理的类，此类要继承或实现抽象角色的抽象类或者接口，实现里面的抽象方法。
    3. 定义代理角色，也就代理类。此类也要继承或实现抽象角色的抽象类或接口，并且实现里面的抽象方法。同时，代理类中还要声明一个真实类的引用变量，并通过相关方法从外部传递一个真实类对象引用进来，或者代理类内部new一个真实类对象。另外，代理类中实现抽象类或者接口中的方法体中，利用代理类中的这个真实类对象引用调用真实类中相应的方法，也就是代理类的方法最终实现是由它代理的真实类的方法类实现，但是此时，在调用真实类的方法前后可以插入代理类自己的一些方法，完成捆绑代理类的一些操作。
    4. 客户端使用代理类操作真实类，可以使用抽象角色引用变量接收代理类对象引用，使用抽象角色中方法时，根据多态会调用代理类中的方法，而代理类中的方法由是对真实类中相应方法的封装，由此间接操纵了真实类。

11. 动态代理需要java.lang.reflect包下的接口InvocationHandler和类Proxy来实现。

12. 动态代理实际过程：
    1. 定义抽象角色，动态代理的抽象角色只能是一个接口，不能是抽象类。
    2. 定义真实角色，也就是被代理的类，要实现抽象角色接口中的方法。
    3. 定义实现InvocationHandler接口的类。动态代理中不再需要手动定义代理角色（代理类由运行过程中生成），但是将代理类需要包含的一个真实类对象引用变量转移到实现InvocationHandler的类中，所以这个实现InvocationHandler类要定义一个可以接收真实类对象引用的引用变量（一般是Object引用变量，这样就可以代理任意类型的真实类，动态代理的特点也就在于此，可以动态生成代理类，不需要每使用一个代理类就要定义一个代理类），并在构造方法或者定义其他方法给这个变量赋真实类对象引用值。其次，这个类中最重要的是实现InvocationHandler接口中的方法public Object invoke（Object obj，Method method，Object[] args），其中第一个参数一般是指调用该方法的代理类对象（一般情况下用不到），method是被代理的方法，args是被代理的方法的参数列表。动态代理的客户端运行代理类方法的时候，实际上使用的是这个类的invoke方法，invoke方法中接收的method参数是动态代理底层利用反射机制自动生成的被调用方法对应的Method对象，args参数是动态代理将被调用方法传递的参数组合起来的数组。因此，实现InvocationHandler接口的类中invoke方法要根据传回来的Method对象使用Method对象的invoke方法，所以Method的invoke方法要接收被调用对象（一般是本类InvocationHandler中接收真实类引用的成员变量，它通过它才能调用真实类的对应方法达到通过代理控制真实类的目的）和被调用方法的参数（本类invoke方法接收到的args参数），并且代理类方法中要附加自己的代码要插入到调用Method的invoke方法前后。
    4. 客户端动态生成代理类和构造代理类对象。动态代理的客户端要构造真实类对象和实现InvocationHandler接口的类对象，然后使用Proxy.newProxyInstance（ClassLoader loader，Class[] interfaces，InvocationHandler h）动态生成代理类（类名为“$Proxy+阿拉伯数字”）和代理类对象，并将生成的代理类对象返回（Object类型，使用时可以强制转换）。该方法中loader参数接收实现InvocationHandler类的类加载器ClassLoader，可以通过Class类的getClassLoader方法获得，可以使用真实类的ClassLoader或者实现InvocationHandler类的ClassLoader，一个类的ClassLoader可以装载很多类；第二个参数interfaces接收一个接口数组，自动生成的代理类会实现interfaces数组中所有的接口，一般要接收真实类实现的接口，因为代理类要实现与真实类的共同接口；第三个参数要接收InvocationHandler，也就是实现InvocationHandler接口的类，使用生成的代理类方法时，动态代理底层会传递参数h中的invoke方法所需要的参数，将代理类的方法实现转移给h中的invoke方法。
    5. Proxy.newProxyInstance返回的代理类对象Object可以强制转换成接收的参数interfaces数组中的任意一种接口类型（多态特性，代理类是interfaces数组中所有接口的实现类），便可以调用强制转换后的接口中拥有的每一个方法，代理都会自动包装调用过程中的方法参数传递给InvocationHandler中的invoke，让invoke实现代理方法。