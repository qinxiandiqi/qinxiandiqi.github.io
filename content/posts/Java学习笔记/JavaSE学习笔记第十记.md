---
# type: posts 
title: "JavaSE学习笔记 第十记"
date: 2014-06-04T14:31:47+08:00
authors: ["Jianan"]
summary: "Java的io操作，装饰模式的一般构成，常见字符集的区别，Java对象的序列化和反序列化。"
series: ["Java学习笔记"]
categories: []
tags: ["Java", "装饰模式", "io", "字符集", "序列化和反序列化"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read: 824
---

## 2012-07-30

1. 装饰模式：又叫包装模式，能够在不创造更多子类的情况下动态地将对象的功能加以扩展，是继承的一种替代方案。一个类对象装饰另一个类对象，就可以以装饰对象的方法处理被装饰对象，而整个处理过程对客户端是透明的，实际的过程是装饰对象的处理方法调用被装饰对象的处理方法，并在被装饰对象的处理方法上添加新的功能，也就是说最终是把客户端的调用委托到被装饰类。这种模式的好处就是在不造成类数量增加的情况下，构造更多功能的对象。

2. 装饰模式与继承的区别：装饰模式用来扩展特定对象的功能，并且是动态的，在运行时分配添加的职责，因此灵活性比较好；而继承是扩展类的功能，一旦扩展就不能修改，是静态的，在编译时分配添加的职责，因此灵活性比较差。另外，一个规定的对象同时能被对个装饰对象装饰，客户端可以通过选择合适的装饰对象操作被装饰对象。

3. 装饰模式的角色：
    1. 抽象构建角色（Component）：一般是一个抽象类或者接口，用来规范被装饰对象的结构。如：InputStream和OutputStream。
    2. 具体构建角色（Concrete Component）：也就是真实对象，继承或实现了抽象构建角色。如：节点流FileInputStream和FileOutputStream。
    3. 装饰角色（Decorator）：继承或实现抽象构建角色的抽象类或者接口或者类，以实现客户端对象可以使用和具体构建角色相同的方式与装饰对象进行交互。装饰角色接收客户端对象的所有请求，装饰角色内部持有一个抽象构建角色的引用变量，通过这个引用才能最终将客户端的请求转发给构建角色，由构建角色去完成。如：过滤流FilterInputStream和FileOutputStream。
    4. 具体装饰角色（Concrete Decorator）：继承或实现抽象装饰角色的类，具体实现对被装饰对象的附加功能添加在这个类中，而不是装饰角色中。如：过滤流DataInputStream和DataOutputStream。

4. new DataInputStream（new BufferedInputStream（new FileInputStream（“C：\test.text”）））；

5. Java中采用Unicode字符，一个字符16位，字符流Reader和Wrider处理的都是Unicode字符，一次至少两个字节16位。

6. InputStreamReader直接继承Reader，是字节流通向字符流的桥梁。构造方法有InputStreamReader（InputStream in）、InputStreamReader（InputStream in，String cs）等，用于接收一个InputStream对象，并使用cs编码格式编码，如果没有cs参数则使用系统默认编码格式。类中提供了getEncoding（）方法可以返回该流使用的字符编码格式名称。可以将InputStreamReader看做是把InputStream装饰成处理字符流的过滤流。

7. OutputStreamWriter直接继承Writer，是字节通向字符流的桥梁。构造方法有OutputStreamWriter（OutputStream out）. OutputStreamWriter（OutputStream out，String cs）等，用于接收一个OutputStream对象，并使用cs格式编码，如果没有cs参数则使用系统默认的编码格式编码。类中提供了getEncoding（）方法可以返回该流使用的字符编码格式名称。flush（）方法可以强制刷新该流的缓冲。可以将OutputStreamReader看做是把OutputStream装饰成处理字符流的过滤流。由于继承了Writer，所以继承了其中的writer（String str）方法，可以一次写入一个字符串。

8. BufferedReader直接继承Reader，类比于BufferedInputStream，只是类中的字节缓冲数组变更为字符缓冲数组，其类中其他方法使用方式与BufferedInputStream中方法使用几乎相同，是一个接收Reader的过滤流。类中提供了一个readLine（）方法读取Reader流中的一个文本行，并将游标转移到该行的行尾，如果游标已经到行尾，则会返回null。

9. BufferedWriter直接继承Writer，类比于BufferedOutputStream，只是类中的字节缓冲数组变更为字符缓冲数组，其类中的方法使用方式与BufferedOutputStream中的方法几乎相同，是一个接受Writer的过滤流。由于继承了Writer，所以继承了其中的writer（String str）方法，可以一次写入一个字符串。

10. FileReader继承InputStreamReader，类似于FileInputStream，只是处理的对象是字符。

11. FileWriter继承OutputStreamWriter，类似于FileOutputStream，只是处理对象是字符。如果FileWriter依赖的文件不存在则会自动创建这个文件，如果FileWriter试图打开一个只读文件就会抛出一个IOException异常。

12. CharArrayReader继承Reader，类似于ByteArrayInputStream，只是字节数组更改为字符数组。

13.CharArrayWriter继承Writer，类似于ByteArrayOutputStream，只是字节数组更爱为字符数组。

14. System.in其实是一个InputStream标准输入流，通常是从键盘输入；System.out其实是一个PrintStream标准输出流，一般是输出到控制台。

15. String的getChars（int srcBegin，int srcEnd，char[] dst，int dstBegin）方法用于将原字符串中的srcBegin到srcEnd的字符以字符数组的方式复制到字符数组dst的dstBegin位置开始。

16. ASCII（American Standard Code for Information Interchange）：美国信息互换标准代码，采用8位二进制位编码，将英文中常用的字符、数字符号与最高位为0，相应十进制数为0-127的数值对应进行编码。另外还有128个扩展ASCII编码，其最高位为1，用于编制一些指标附后和其它符号。

17. GB2312：信息交换用汉字编码字符集-基本集，使用两个字节表示一个中文字符，并且每个字节的最高位都是1.

18. GBK：GB2312的扩展，完全兼容GB2312，并且多容纳了繁体中文和一些不常用的汉字。

19.ISO-8859-1：西方国家使用的字符编码集，属于单字节的字符集，其中英文只用了其中数值小于128的部分。不兼容汉字。

20. Unicode：通用字符集，可以对所有语言文字进行编码，每个字符都使用两个字节，Java中采用这个编码方式以实现字符的跨平台。这种编码的缺点在于在internet上传输的效率比较低，因为一些使用一个字节就能表示的字符也使用两个字节表示，高字节填0。

21. UTF-8：不定长字符编码方式，根据字符需要的字节长度分配字节，有些字符用一个字节，有些两个，有些三个。互联网上常用这种编码方式，传输效率高，字符容量大。

22. java.io.RandomAccessFile：随即访问文件类，直接继承Object，并实现了DataInput和DataOutput接口。构造方法有RandomAccessFile（File file，String access）或者RandomAccessFile（String name，String access），其中file或者name表示依赖的File对象或者文件名，access表明使用文件的方式是“r”（只读）还是“rw”（读写）。另外，由于类中包含一个long变量pos作为流的游标，使用该类的getFilePointer（）可以获得当前游标的位置，使用该类的seek（long pos）可以将游标定位到pos位置，使用skipBytes（int n）可以将游标向后移动n个字节。由于该类实现了DataInput和DataOutput接口，所以类中提供了大量读写各种数据类型的方法，特别的有readUTF（）和writeUFT（String str）方法以UFT的格式读写数据。该类的特点在于类中包含了输入流方法和输出流方法，相当于输入流和输出流的合体。

23. java.nio.Charset类中的availableCharsets（）方法可以返回一个SorteMap<String，Charset\>排序映射。其中的Key为当前系统所支持的字符集名。

24. 序列化/反序列化：将对象转换为字节流（因为对象不是字符，转换成字符流没有意义）保存起来，并在以后从字节流中还原这个对象的机制。若把一个对象序列化后保存到永久存储设备上，这个过程也叫做持久化。

25. 声明对象可序列化：一个对象能够序列化的前提是对象必须实现Serializable或者Externalizable接口。其中Serializable接口只是一个标识性接口，接口中没有定义任何方法，实现该接口仅是表示这个类可以被序列化而已。继承一个可序列化的类，其子类也可序列化，即序列化特性可以被继承。

26. 被序列化的对象中如果存在其它对象的引用，则其它对象的引用也会被序列化，并且引用的对象又包含其它对象的引用也会被序列化，也就是序列化会根据对象中的引用一层一层连接下去序列化。

27. 对象序列化的时候，如果对象中的某成员无法序列化，则会抛出NotSerializableException异常。此时，若将该无法被序列化的对象使用关键字transient（瞬间）修饰，则序列化的时候不会序列化该成员，对象的序列化能够正常进行。

28. 对象序列化的时候，不会序列化对象中的static变量和方法（因为它们属于类，不属于对象），也不会序列化被transient修饰的成员。只会将对象中的成员变量序列化，写入到字节流中（通过字节流可写入到存储设备上永久保存）。如果成员变量是一个对象的引用，则会按照同样序列化的规则序列化引用的对象，以此类推各个对象引用。

29. ObjectOutputStream类：继承了OutputStream类，并且实现了ObjectOutput接口，ObjectOutput接口又实现了DataOutput接口。构造方法有ObjectOutputStream（OutputStream out），可接收一个输出字节流，是一个过滤流，主要用于实现对象的序列化。另外，由于实现了DataOutput接口，所以类中包含大量写入各种类型数据的方法。

30. 将对象序列化的方法：使用ObjectOutputStream类的writeObject（Object obj）方法。默认会将obj的类、类的签名、以及类和其所有超类的非transient非static成员变量写入到输出流中，即实现了序列化。

31. ObjectInputStream类，继承了InputStream类，并且实现了ObjectInput接口，而ObjectInput接口又实现了DataInput接口。构造方法有ObjectInputStream（InputStream in），可接收一个输入字节流，也是一个过滤流，主要用于实现对象的反序列化。另外，由于实现了DataInput接口，所以类中包含了大量读取各种数据类型的方法。

32. 对象反序列化的方法：使用ObjectInputStream类的readObject（）方法，返回一个Object对象。由于对象在序列化的时候保存了对象的类信息，所以readObject方法无需参数，就能将当前游标所在位置后边的对象按该对象序列化时的类信息恢复出来。恢复的时候，并不会调用该对象类的任何构造方法，仅是根据保存的状态信息在内存中重新构建对象而已。又由于对象序列化的时候不会序列化transient修饰的成员变量，但保存的类信息中存在该成员的信息，所以反序列化后，被transient由于序列化时没有将值保存下来，其值只能是其数据类型的默认值。

33. 序列化和反序列化过程中，如果需要对对象进行特殊处理，不按照java提供的默认序列化和反序列化方法进行，可自定义序列化和反序列化的处理方法。方法为在需要被序列化和反序列化的类中实现两个方法：
    1）序列化方法：private void writeObject（java.io.ObjectOutputStream stream）throws IOException{}；在方法体中写入自己的处理对象成员变量与写入ObjectOutputStream流的代码，当对该类对象进行序列化的时候便会调用这个方法进行序列化，不再使用默认序列化方法。
    2）反序列化方法：private void readObject（java.io.ObjectInputStream stream）throws IOException，ClassNotFoundException{}；在方法体重写入自己处理对象成员变量与ObjectInputStream流的代码，当对该类对象进行反序列化的时候就会自动调用这个方法进行反序列化，不再使用默认的反序列化方法处理。

34. 进程：进程是一个运行中的程序，每一个程序运行的时候都需要在进程中执行代码，操作系统会为一个程序分配该程序进程所需要的资源，包括内存空间等。进程与进程之间的内部数据和状态都是完全独立的，所以进程与进程之间的切换代价比较高。

35. 线程：每个进程至少包含一个线程，一个进程可以包含多个线程，每个线程可独立完成进程中的一个工作，是程序内的一个顺序控制流，只能使用分配给程序的资源和环境。并且线程运行时只需要很少的资源，通常只有寄存器数据以及程序执行时的堆栈，所以线程与线程之间的切换代价比较小。同一个进程之间的线程共享一块内存空间和系统资源，它们有可能会互相影响。

36. 多任务处理包括基于进程和基于线程两种对任务处理，它们都是为最大限度使用CPU资源，以提高CPU效率。

37. Java语言内置支持多线程变成，其他大多数编程语言都需要通过外部库链接来实现多线程编程。

38. Java程序中，每当程序启动的时候都会自动启动一个线程，main方法就运行在这个自动启动的主线程上，因此这个线程也叫做main thread。每个Java程序至少都有一个线程，这个线程就main线程。
