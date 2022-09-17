---
# type: posts 
title: "JavaSE学习笔记 第九记"
date: 2014-06-02T10:41:04+08:00
authors: ["Jianan"]
summary: "Java文件系统和Java IO的使用。"
series: ["Java学习笔记"]
categories: []
tags: ["Java", "io", "文件系统"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read: 802
---

## 2012-07-29

1. Java的I/O系统主要由java.io包和java.nio包构成。

2. java.io.File类对象表示了磁盘上的一个文件或者目录，在java的io系统中，文件和目录都统一使用File类对象表示，其父类是Object。File类只是对磁盘上的文件或目录的抽象表示，提供了与平台无关的对文件或目录的操作方法，比如获取路径或者文件及目录的相关信息，并对他们进行创建. 删除、改名等管理工作。然而File只是抽象描述了文件或目录的属性和操作方法，但并没有提供怎样从文件读取和向文件存储等方法。

3. 路径分隔符：由于java中反斜杠\用作转义字符，所以路径中的反斜杠需要用双反斜杠\\转义表示。特别的，处理window系统中使用反斜杠做文件路径分隔符之外，其他系统都是使用正斜杠/做路径分隔符。因此，java中使用正斜杠/做抽象路径分隔符，无论什么系统使用正斜杠做路径分隔符，java都能自动转成系统适应的分隔符，所以推荐使用正斜杠/做java中的路径分隔符。

4. File的构造方法常用的有：File（String path）、File（File file，String str）、File(String str1,String str2)。值得注意的是，使用File构造方法构造一个File对象并不等于创建了这个File对象指向的文件或者目录，其中第二三个构造方法需要组合后能拼成一个正常的文件路径才不会影响到File对象对该文件或路径的操作。

5. File创建文件方法：createNewFile（）。只要File构造方法的参数路径存在，并且路径指向的文件不存在，就能够创建对应的文件。如果参数路径存在，且指向的文件也存在，则创建失败，返回false。如果参数路径不存在，则会抛出路径找不到的异常。

6. File创建目录的方法：mkdir（），该File指向的路径中，要创建的目录上一级路径不存在，则无法创建目录，返回false。mkdirs（），该File指向的路径中，要创建的目录上一级或上几级路径不存在，也能够正常创建目录，并把不存在的路径目录都创建出来。如果要创建的目录已经存在，则返回false。

7. File的isFile（）用于判断File是否指向一个文件；File的isDirectory（）用于判断File是否指向一个目录。

8. File的list（）用于将File指向目录内的所有内容的名字以字符串数组返回，包括文件和目录。File的listFiles（）方法用于将File对象指向的目录内所有File以File数组的形式返回。使用以上两个方法时，File对象必须时一个目录对象。

9. File的getName（）获取File对象指向文件的名字；File的getParent（）用于获取父目录名；File的getParnetFile（）用于获取父目录File对象；File的exists（）用于判断文件是否已经存在。File的delete（）用于删除File对象指向的文件（只有文件或者空目录才能删除，不为空的目录需要将目录内的所有文件都删除才能删除，一般是由递归算法）。

10. FileNameFilter接口，常用作list（FileNameFilter filter）或listFiles（FileNameFilter filter）方法的参数，以返回目录下符合一定条件的文件名或文件File对象。一般是在参数列表中使用匿名内部类实现这个接口，该接口只包含一个方法accept（File dir，String name），筛选目录下文件的筛选方法就写在这个accept方法体中。

11. File.separator静态常量，用于表示与系统有关的默认名称分隔符，在路径中可以始终File.separator代替路径的分隔符以组成完整的路径字符串。单独的File.separator表示根目录，即盘符。

12. 递归：方法内部再调用自己，或者一个方法调用另一个方法，而另一个方法又调回原来的方法。递归的特点就是循环嵌套调用，并且使用递归的方法中必须提供一个结束递归的出口（一般是一个if条件判断）。递归的过程分为递进和回归，先一层一层调用到出口层，返回确定结果后再一层一层将结果返回到最顶层。

13. Java的流（Stream）：Java的程序通过流来完成输入和输出，是生产或者消费信息的抽象，它通过java的输入和输出系统与物理设备连接，并且屏蔽掉设备之间的差异性，对于不同的物理设备，流都具有同样的行为方式。

14. 输入流和输出流：根据程序在使用数据时扮演的角色不同，流可以分为输入流和输出流。当程序从外部读取数据时，也就是程序是数据流的目的地，此时的数据流为输入流；当程序输入数据到外部时，也就是程序是数据流的源头，此时的数据流为输出流。总的来说，输出流和输入流是一种以程序为中心的相对概念。

15. 字节流和字符流：根据流结构上的不同，将流分为字节流和字符流。字节流以字节为处理单位，即八位二进制数据，一般用于读写图像或者声音等二进制数据。字符流以采用了统一编码标准的16位字符为单位，因此字符流可以国际化，在某些场合下，字符流具有比字节流更高的效率。

16. 综合输入输出流和字节字符流，可以将流再细分为：输入字节流（以InputStream抽象类为父类）、输出字节流（以OutputStream抽象类为父类）、输入字符流（以Reader抽象类为父类）、输出字符流（以Writer抽象类为父类）。

17. Java 1.0只存在字节流，Java 1.1开始出现字符流。字符流的出现只是为了处理字符提供方便有效的办法，但是字符流的底层本质上还是以字节的形式处理。

18. 流读取数据的逻辑：
    1. 打开一个输入流
    2. 循环判断是否还存在数据信息，如果还有数据信息，则读取一定单位的数据信息并继续循环，直到没有剩余数据信息为止。
    3. 关闭输入流

19. 流存储数据的逻辑：
    1. 打开一个输出流
    2. 循环判断是否还有需要存储的数据信息，如果还有数据信息，则存储一定单位的数据信息并继续循环，直到所有数据信息都存储完毕为止。
    3. 关闭输出流。

20. 根据流是否与目标直接打交道，可以将流分为节点流和过滤流。节点流直接与需要从中读取数据或者需要从中输出数据的目标打交道，比如从硬盘上某文件读取数据的流、程序输出数据到硬盘某个文件的输出流。过滤流是与节点流或者其他过滤流打交道的流，也就是它不直接与数据目标打交道，它的作用在于将节点流或者过滤流再封装以增加更多的数据信息和处理方法，并且它是同步的。

21. 所有流的关闭方法都是close（）。

22. InputStream的三个read方法：
    1. read()，三个read方法中唯一的一个抽象方法，规定从输入流中读取数据的下一个字节。因为不同子类read的具体需要不同，所以设计此read抽象方法以供子类实现时设计符合自己具体要求的read（）方法。
    2. read（byte[] b，int off，int len），本质上是方法体内通过对read（）方法的调用，来实现从数据流中读取不超过len个字节到以off索引为起始位置的b字节数组中去，返回值为实际读取的字节个数，如果没有字节可读取则返回-1。因为每个子类的read（）实现方法不同，所以子类继承这个方法就能根据子类read（）的读取规则完成这个方法的逻辑。
    3. read（byte[] b），本质上是调用read（b，0，b.length）。

23. String类具有一个将字节数组转换为字符串的构造方法：String（byte[] b，int off，int length），可以构造一个b数组中以off索引为其实位置，长度为length的字符串。String类也提供了getbyte（）方法将字符串转换为byte数组。

24. OutputStream的三个write方法（类似于InputStream三个read的原理）：
    1. write(int b)，三个write方法中唯一的一个抽象方法，规定将b个字节写入到输出流中。因为不同子类write的具体需要不同，所以设计此read抽象方法以供子类实现时设计符合自己具体要求的write（int b）方法。
    2. write（byte[] b，int off，int len），本质上是方法体内通过对write（int b）方法的调用，来实现将b字节数组中从off索引为起始位置的len个字节写入到输出流中，没有返回值。因为每个子类的write（int b）实现方法不同，所以子类继承这个方法就能根据子类write（int b）的写入规则完成这个方法的逻辑。
    3. write（byte[] b），本质上是调用read（b，0，b.length）。

25. FileInputStream继承了InputStream，是关于File的字节输入流。构造方法有FileInputStream（String url）、FileInputStream（File file）等，用于获取file指向的文件或者url路径指向的文件的字节输入流。

26. FileOutputSteam继承了OutputStream，是File的字节输出流。构造方法有FileOutputStream（File file，boolean append）和FileOutputStream（String url，boolean append），file或者url表示输出流要写入的file或者url指向的文件，如果文件不存在则会自动创建这个文件；append为true则当文件已经存在的情况下，使用write会将写入字节追加到文件尾，若为false则会删除文件的全部内容，然后写入。FileOutputStream（File file）和FileOutputStream（String url）的本质是对应前两个构造方法，append值为false的构造方法。

27. ByteArrayInputStream继承了InputStream，以byte数组为输入源的字节流，内部包含一个buf字节数组缓冲区（即要读取的数据）。构造方法有ByteArrayInputStream（byte[] buf，int off，int length），构造一个buf为缓冲区数组的字节数组输入流，并且将要读取的开始索引位置为off，读取的字节个数为length。另一个构造方法ByteArrayInputStream（byte[] buf）的本质为ByteArrayInputStream（buf，0，buf.length）。

28. ByteArrayOutputStream继承了OutputStream，以byte数组为输出对象的输出流，内部包含一个buf字节数组缓冲区（存放写出的数据）。构造方法有ByteArrayOutputStream（）和ByteArrayInputStream（int size）。使用它的write方法可以将一个byte数组写入到它的六种buf字节数组缓冲区，再使用它的toByArray（）方法创建一个新的byte数组存储流中的buf字节数组，也可以使用它的writeto（OutputStream out）方法将buf的全部内容写到输出流out中去。

29. FilterInputStream是继承InputStream的输入字节过滤流抽象类，输入字节过滤流都必须继承这个类，java提供的子类有DataInputStream（常用）、BufferedInputStream（常用）、LineNumberInputStream、PushbackInputStream。

30. FilterOutputStream是继承OutputStream的输出字节过滤流抽象类，所有输出字节过滤流都必须继承这个类，java提供的子类有DataOutputStream（常用）、BufferedOutputstream（常用）、PrintStream。

31. DataInputStream：数据字节输入流，构造方法为DataInputStream（InputStream in），相当于将in输入流再进行包装成DataInputStream。内部提供了大量读取基本数据类型的方法，这些方法将按照对应数据类型的字节长度依次读取in中的数据。例如：readBoolean（）、readByte（）、readChar（）、readDouble（）等等。

32. DataOutputStream：数据字节输出流，构造方法为DataOutputStream（OutputStream out），相当于将out输出流再进行包装称DataOutputStream。内部提供了大量写入基本数据类型的方法，这些方法会按照对应的基本类型占用的字节长度，以二进制的形式写入到out中。例如：writeBoolean、writeByte、writeBytes、writeChar、writeChars等。

33. BufferedInputStream：字节缓冲输入流，内部具有buf字节数组缓冲区，构造方法有BufferedInputStream（InputStream in）和BufferedInputStream（InputStream in，int size），用于将in输入流包装称BufferedInputStream，带有size时可指定内部buf缓冲区数组长度为size。使用它的read读取in数据，并不会直接将读取的数据存入到接收数据的数组b中，而是先存到内部buf数组中，等到buf数组满了后再一次把数据读回，使用close关闭流之前会强制将buf中数据返回。

34. BufferedOutputStream：字节缓冲输出流，内部具有buf字节数组缓冲区，构造方法有BufferedOutputStream（OutputStream out）和BufferedOutputStream（OutputStream out，int size），用于将out输出流包装称BufferedOutputStream，带有size时可指定内部buf缓冲区数组长度为size。使用它的write并不会直接将数据写出去，而是等到buf数组满了后再一次性写出去，或者调用flush（）时强制将buf中数据写出去，使用close关闭流时也会强制写出去。
