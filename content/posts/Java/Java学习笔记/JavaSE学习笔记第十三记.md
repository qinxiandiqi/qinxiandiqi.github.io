---
# type: posts 
title: "JavaSE学习笔记 第十三记"
date: 2014-06-09T14:18:05+08:00
authors: ["Jianan"]
summary: "Java的网络编程。"
series: ["Java学习笔记"]
categories: []
tags: ["Java", "Socket", "Java网络编程"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 863
---

## 2012-08-02

1. 单例模式的两种实现方法中，如果将构造单例对象的方法放置到获取单例对象的方法中，在多线程的情况下有可能违反单例模式的要求产生不同的单例对象。而在定义的单例对象成员时就构造对象则不会出现这种情况。

2. URL包装的URL值必须包含协议名称，即使是HTTP也要包含，不同于浏览器会自动添加。

3. java.net.InetAddress类：用于封装IP地址和主机名的类。该类没有明显的构造函数，是采用工厂模式设计的类，构造一个InetAddress类需要通过类中的getLocalHost（）、getByName（）、getAllByName（）等static方法获得。getLocalHost（）用于获取封装本机IP的InetAddress对象，getByName(String name)用于获取主机名为name的InetAddress对象（一般name是用主机的域名），getAllByName（String name）用于获取主机名为name的一个InetAddress对象数组（有些主机有多个网卡可以拥有多个IP）。这些静态方法一旦获取失败会引发UnknowHostException异常。

4. Socket：是连接运行在网络上的两个程序间的双向通讯端点，分为服务器端Socket和客户端Socket。

5. java.net.ServerSocket类：实现服务器套接字的类，基于TCP协议！用于等待通过网络传入的请求，并可能想请求者返回结果。关闭ServerSocket需要使用close方法。
    1. 主要的构造方法有ServerSocket（int port），创建一个用于监听port端口的服务器套接字，如果port为0就会随机分配一个端口并创建监听这个端口的服务器套接字。
    2.  public Socket accept（）throws IOException：启动该ServerSocket对象监听port端口，在有连接请求传入之前一直处于阻塞状态，有连接请求传入时能够创建一个新端口号的Socket用于连接请求。

6. java.net.Socket类：两台机器之间通讯的端点，基于TCP协议！用于实现客户端套接字（实际上服务端套接字接收请求后也会在服务器端创建Socket）。Socket之间通过io流传送数据，当Socket要发送数据时要通过输出流，当Socket要读取数据时要通过输入流。关闭Socket需要使用它的close方法。
    1. 主要构造方法有Socket（String address，int port）：用于创建与IP为address，端口号为port相连的流套接字。一旦创建成功，就表示已经于该IP主机的port端口正常连接上。
    2. 通过getOutputStream（）获取套接字的输出流（对方Socket的输入流），通过getInputStream（）获取套接字的输入流（对方Socket的输出流）。

7. 使用Socket进行网络通讯的一般过程：
    1. 服务器程序创建一个ServerSocket对象绑定到一个特定的端口，并通过ServerSocket的accept方法启动等待监听客户的连接请求功能，一般将accept方法放置于死循环中，这样每当监听到客户连接请求时才能执行一次。
    2. 客户程序根据服务器程序所在主机名或IP以及端口号，创建Socket对象向服务器程序的ServerSocket监听的端口发出连接请求。
    3. 如果服务器接收到客户端程序的连接请求后，将会创建一个新的绑定到不同端口地址的Socket与客户端的Socket通过读写（getOutputStream和getInputStream）套接字进行通讯。同时服务器程序的ServerSocket仍然继续监听其端口的连接请求。

8. 127.0.0.1和localhost都代表本机。

9. 当服务器端和客户端已经各自建立起Socket连接后，如果通过Socket直接通讯的话，只能按照程序已经规定的格式依次使用读写流通讯（比如使用InputStream输入流读取数据多少次后再通过OutputStream输出多少次，顺序不能修改），而大多数情况下，什么时候读数据什么写数据是无法确定的。为了解决这个问题，需要为服务器端每一个Socket创建一个读取InputStream线程和输出OutputStream线程，也要为客户端每一个Socket创建一个读取InputStream线程和输出OutputStream线程，这么一来，读写操作分开又交由线程去处理，可以并发执行，无论服务端还是客户端在什么时候要进行读写都能正确接收。

10. Java提供了java.net.DatagramSocket类和java.net.DatagramPacket类来实现基于UDP协议的网络编程。在UDP编程中，没有严格区分服务端和客户端，其通讯方法类似于信件传送，UDP数据报中包含了发送方和接收方法的IP和端口与信息，发送方和接收方的角色是相对的。

11. java.net.DatagramSocket类：基于UDP协议的两台机器间的通讯端点。关闭DatagramSocket对象需要使用close方法。
    1. 主要构造方法有DatagramSocket（int port），用于创建监听prot端口的DatagramSocket对象，如果没有数据报接收就会进入阻塞状态，不往下执行。
    2. sent（DatagramPacket p），用于将数据报DatagramPacket对象发送出去。
    3. receive（DatagramPacket p），用于接收数据报DatagramPacket对象。

12. java.net.DatagramPacket类：封装UDP数据报的类。
    1. 主要构造方法有DatagramPacket（byte[] buf，int offset，int length，InetAddress address，int port）：创建一个DatagramPacket对象，其中buf为该对象封装的字节数组数据信息，offset表示封装buf数组数据的起始位置，length表示封装的字节数据长度，address表示该对象将要发送到的机器地址，port表示该对象将要发送往的机器上的端口位置。
    2. setData（byte[] buf，int offset，int length）：设置DatagramPacket对象封装的字节数组buf，offset为封装的起始位置，length表示封装的字节长度。
    3. getData（）：放回一个byte[]数组，获取DatagramPacket对象中封装的字节数组数据。
    4. getLength（）：获取DatagramPacket对象中封装的字节数组长度。
    5. getAddress（）：当DatagramSocket接收到一个DatagramPacket对象后，可使用此方法获取该对象是从哪个机器发过来的。
    6. getPort（）：当DatagramSocket接收到一个DatagramPacket对象后，可使用此方法获取该对象从发送机器的哪个端口发送出来的。

13. 使用DatagramSocket和DatagramPacket进行UDP通讯的一般过程：
    1. 创建DatagramSocket对象和DatagramPacket对象，用DatagramPacket封装需要发送的数据和接收方的InetAddress和端口。
    2. 使用DatagramSocket对象的sent方法将DatagramPacket方法发送出去。
    3. 接收方同样需要创建DatagramSocket对象和DatagramPacket对象，以及接收数据的byte[]空数组。DatagramSocket对象需要指定发送方DatagramPacket对象设置的端口。DatagramPacket对象使用空数组byte[]构造。
    4. 接收方使用DatagramSocket的receive方法将接收的DatagramPacket对象，便可以通过DatagramPacket对象的各种方法获取发送方发送的byte数组，以及发送方InetAddress和端口等信息。
    5. 反过来，接收方向发送方发送数据时，将发送方当做接收方，将接收方当做发送方按照以上步骤便可。

14. 同样类似于Socket，可以使用两个线程分管DatagramSocket的发送和接收。
