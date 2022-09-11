+++
title = "Java SE 学习笔记 第四记"
date = 2012-10-09T11:15:11+08:00
draft = false
summary = "Java的集合和字典，list、set、map、tree；设计模式之策略模式。"
description = "Java的集合和字典，list、set、map、tree；设计模式之策略模式。"
tags = [
    "Java",
    "Set",
    "List",
    "Map",
    "Tree",
    "策略模式"
]
series = ["Java学习笔记"]
read = 1213
+++

## 2012-07-19

1. java中的链表节点使用封装的类，节点类包括节点数据和前驱后继节点的引用，java中没有指针的概念，所以使用链表只能使用引用，也就是引用类型变量做“指针”。

2. LinkedList链表的底层实现实质上是由数据类型为Object和前驱后继Entry引用变量组成的Entry节点类的双向链表，所以LinkedList链表可以添加任意类元素（Object的子类）。添加新元素的时候，LinkedList会将对象封装成Entry类实例后插入到LinkedList链表中。基于链表的特性，LinkedList无长度限制，添加删除链表节点会关系到节点上下引用值的改变。
    ```java
    class Entry{
        Entry previous; 
        Object element;
        Entry next;
    }
    ```

3. JDK提供了Stack类和Queue接口，可以通过LinkedList类的方法构造Queue类。

4. Java中的Set集合与数学意义上的集合是一致的，集合中不能有相同的元素。

5. HaseSet中的元素没有顺序性，符合数学集合的无序性。使用add向HaseSet添加已存在元素会返回false，表明添加不成功。

6. Object类的equals()方法的特点：
    - 自反性，x.equals(x)的值为true，x不为null；
    - 对称性，x.equals(y)与y.equals(x)的值一致；
    - 传递性，x.equals(y)的值为true，且y.equals(z)的值为true，则有x.equals(z)的值也为true；
    - 一致性，只要x和y没有改变，无论调用多少次x.equals(y)，结果都不会改变；
    - 对于非空引用x.equals(null)的结果必定为false。

7. Object类的haseCode()方法特点：
    - 在java引用的一次执行过程中，同一个对象只要没有修改，无论调用多少次haseCode()，返回值都相同；
    - 如果两个对象用equals比较的结果为true，那么这两个对象的haseCode()返回值一致；
    - 如果两个对象用equals比较的结果是false，那么这两个对象的haseCode()返回值可以相同，也可以不同，Java推荐使用不同值可以提高程序的性能；
    - Object默认的haseCode()返回值是对象的地址，所以Object的haseCode()对于不同对象的返回值是不同的。

8. HaseSet集合不允许存在相同的元素是通过以下机制实现：当向HaseSet添加新元素的时候，HaseSet会首先调用要添加对象的hashCode()方法，并与HaseSet已存在元素的HaseCode进行比较，如果都不相同，则直接添加新元素进集合；如果HaseCode的比较结果存在相同的元素，则进一步调用equals比较要添加的元素和HaseCode相同的元素，equals的结果为true的话，则拒绝添加新的元素，若为false，则将新元素添加进HaseSet集合。因此，如果使用Object的hashCode()和equals()，添加内容相同的对象时，由于HaseCode值相同，所以即使对象内容相同也能一起添加进去。而对于重写了hashCode()和equals()方法的子类要根据具体重写的方法决定是否能添加进内容相同的对象，比如String类的hashCode()返回采用字符串的内容进行计算获得，所以内容相同的String对象的HaseCode值也相同，自然不能添加进HaseSet。

9. 一般重写equals()方法的时候，最好也重写hashCode()方法。在Eclipse里可以通过Source-Generate hashCode()和 equals()命令选择一定类属性自动重写hashCode()和equals()方法。

10. HaseSet没有get方法，要想从HaseSet中取出元素，需要使用迭代器来使用。利用HaseSet的iterator()可以返回一个该HaseSet的迭代器，再使用循环结构配合iterator的hasNext()方法判断是否存在下一个元素和next()返回下一个元素，返回后hasNext位置会自动指向下一个元素。当然，由于HaseSet元素是无序的，所以返回的结果顺序不一定就是元素添加进HaseSet的顺序。

11. SortedSet接口继承了Set接口，同样不能存在相同的元素，但是增加了排序功能，主要的实现类由TreeSet。

12. 向TreeSet添加元素的时候，TreeSet会根据元素自动升序排序添加，如果添加的元素无法与已存在元素进行比较，则会抛出类型转换异常。此时的解决方法可以是在构造TreeSet的时候使用带Comparator参数的构造方法，指定一个Comparator实现类，这个类里提供了元素之间比较的方法compara()。

13. 指定自定义Comparator的TreeSet构造方法为TreeSet（Comparator comparator），所以实现自定义排序方法，需要自定义实现Comparator接口的类，类中必须实现方法int comparator（Object arg0，Object arg1）（默认是arg0>arg1时返回整数，小于时返回负数，相等时返回0）。利用自定义的比较类，在TreeSet构造方法参数中new一个自定义的Comparator实现类实例就能创建按照自定义比较规则排序的TreeSet。默认的TreeSet使用升序排序，要修改为降序也要通过以上方法重新定义降序的Comparator实现类来完成。

14. 类似于Arrays类为数组提供了大量static操作方法，Collections类为集合提供了大量static操作方法，例如：reverseOrder()为目标集合返回一个与目标集合排序相反的Comparator；sort（Collection，Comparator）为集合Collection进行Comparator规则的排序；shuffle（List list）为列表List打算元素顺序；min()和max()获取集合中的最小和最大值。。。。方法详见JDK开发文档Collection类。

---

## 2012-07-20

1. 映射Map是一个接口，元素是一对Key和Value对象，且不能存在相同的Key，一个Key最多映射到一个值上。

2. HaseMap是Map接口常用的实现类，同样HaseMap中元素是无序的，用put向HaseMap添加元素，如果先后添加的元素Key值相同，则会修改原Key的Value，不会再添加一个元素。使用get(Key)获取HaseMap中Key的值。使用KeySet()方法可以获得HaseMap中Key的集合，并且返回的Set集合是由HaseMap维护的，即当HaseMap中Key发生变化会关联到Set中元素的变化，反过来也一样；使用value()可以返回HaseMap的Value集合Collection，同样返回的Collection也是由HaseMap维护的，值的改变会互相影响。之所以返回的Key集合用Set和Value用Collection，是因为Key在HaseMap中是唯一的，使用Set则不存在相同的元素，但是value却不一定是唯一，所以使用Collection。

3. HaseMap每一对映射的实质在底层上就是一个HaseMap内部类Entry实例，Entry类封装了一对Key和Value，并提供了get和set方法。使用HaseMap的entry()方法可以返回HaseMap的Entry的Set集合，利用Set和Set的迭代器iterator可以遍历整个HaseMap。

4. TreeMap类似于TreeSet，可以实现元素自动排序，默认的排序方法是根据Key进行升序排序。添加自定义排序方法的TreeMap与TreeSet类似。

5. 策略模式（Strategy Pattern）:策略模式的实现根据是多态。

6. 策略模式的组成有三大部分抽象策略角色（通常是接口或者抽象类）、具体策略角色（具体的实现接口或抽象类的实现类）、环境角色（即环境类，具有接口或者抽象类的引用作为环境类的成员变量，并且提供set和get设置接口或抽象类的方法，以及其他封装接口或者抽象类的方法，以供客户类使用）。

7. 策略模式的实现步骤分3步：1）编写抽象策略角色，一般是公共接口，设定接口的方法；2）编写具体策略角色，即策略类，封装相关算法和行为的接口实现方法；3）在环境角色即环境类中，保存一个接口的引用，并完成环境类的set和get或者构造方法，以对接口引用的赋值。

8. 策略模式的使用过程：定义了公共接口和相关接口的实现类，整个策略模式的关键在于环境类中保存的成员变量是接口的引用，而不是接口实现类的引用，并且环境类中所有需要接口做参数的方法，参数类型和方法中调用接口的方法都是依据接口的引用，而不是接口具体实现类的引用。由此，只要在客户类使用环境类的时候，为环境类的接口成员赋予具体的实现类引用，在环境类中，依据多态的特征，环境类就能知道将调用哪个具体实现类对接口的实现方法。也就是说，在客户类中，传入环境类中的具体实现类不同，环境类使用的方法过程不同，虽然方法名相同（实现接口的类中必须实现接口的方法）。

9. 策略模式的优点：策略模式中各个组成部分是弱连接的，只要接口类型不改变，各个组成部分中实现代码可以改变而不会影响其他部分。各个策略类实现接口的代码不同，环境类中使用的是接口的方法，而非具体实现类的方法，只要客户端传入具体实现类的引用，环境类就能根据多态知道使用的是哪个实现类的方法。好处是：环境类只需要根据接口引用和接口的方法名就能编写相应的业务逻辑方法，而只要传入的实现类引用不同，环境类的方法自然就变成了另一种业务逻辑，从而提升了软件的可重用性。

10. HaseSet底层实质上是一个HaseMap，HaseSet中的元素实质上是HaseMap中的Key，而每一个Key的Value都是同一个Object，所以使用add向HaseSet中添加元素本质上是向HaseMap中put一个Key为HaseSet元素，值为一个final的Object元素。

11. Hase负载因子：表明达到负载因子比例的时候就认为哈希数组将近满，另外开辟一个更大的数组以满足要求。

12. HaseMap的在底层上的实质是一个HaseMap内部类Entry的数组，HaseMap类内部有一个成员变量table就是一个HaseMap的Entry数组，使用默认HaseMap构造方法构造的HaseMapEntry数组长度为16，hase负载因子为0.75。并且，由于Entry类内部有一个成员变量next，可以指向一个Entry对象，所以table数组每个Entry元素实际上也是一个Entry链表。根据这个本质和Hase的特点，HaseMap就是一个封装了Entry数组的类，并且具有Hase数组元素添加方式的特征。

13. HaseMap添加put新元素（一对键值，实质是一个Entry类对象）的过程本质：HaseMap根据新键值的Key的HaseCode和table数组的长度，通过Hase算法计算出新键值要添加进table数组的索引号。此时，HaseMap会判断该索引号位置上是否已经存在HaseMap元素，如果不存在，则直接将新键值（Entry）添加进table数组。如果该位置已经存在一个Entry，则HaseMap会遍历这个Entry链表，将添加的新Entry与链表上的每一个Entry进行equals比较。当链表上存在一个Entry比较后的返回值是true时，说明这个Entry的Key与将要添加的Entry的Key相同，根据HaseMap规则，HaseMap会取出原有Entry的Value返回，并将新添加的Entry的Value替换这个旧的Value。当遍历整个Entry链表后无true返回值，HaseMap则会为要添加的Entry调用addEntry方法，该方法会new一个新的Entry对象存放要添加的键值，并将这个新Entry的next指向原本该table数组位置上的Entry对象，再将这个新建的Entry对象插入到table数组这位置上，形成该位置上新的Entry链表，新的Entry对象为该链表的表头（之所以要将新Entry对象作为链表的表头，是因为操作系统认为刚使用的数据在不久的将来有很大的概率会再次使用，所以将新Entry对象作为表头可以提高效率）。
    **注**：由于put新键值的数组位置是由Key根据Hase算法计算获得，所以Key值相同的键值获得的插入位置一定在同一个位置，因此只要遍历该位置上的Entry对象就能知道HaseMap中是否已经存在相同Key的键值。另外，同一个位置上的Entry链中可能存在不同的Key值，因为不同的Key值也可能得出相同的位置。