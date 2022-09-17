---
# type: posts 
title: "JavaSE学习笔记 第十一记 —— 线程"
date: 2014-06-05T10:00:14+08:00
authors: ["Jianan"]
summary: "Java线程的使用"
series: ["Java学习笔记"]
categories: []
tags: ["Java", "线程"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read: 793
---

1. 线程的实现方法：
    1. 继承Thread类，并重写run方法。
        - Thread类是专门用来创建线程和对线程进行操作的类，其中定义了许多方法对线程进行操作。继承Thread的子类线程类要把线程需要实现的代码写到run（）方法中，线程对象实现线程的时候执行的线程内容就是run方法中的内容。由于Thread类的run方法中只是个判断是否存在实现Runnable接口对象的语句，继承Tread类的子类没有传入Runnable对象，所以什么都不做，因此要求每个继承Thread的线程类都必须重写run方法，否则继承的线程类对象什么都不做。
        - 另外，启动线程的方法只有使用线程对象的start（）方法一种，调用start方法会先为线程准备好线程需要的系统资源，然后再启动线程并运行线程的run方法。如果直接调用线程对象的run方法则跟调用一个普通类的普通方法一样，不会形成新的线程执行。另外，一个线程对象只能使用start方法启动一次，多次使用start方法不允许，将抛出异常，即使是该线程对象的run方法已经执行完毕，线程也已经结束，也不能使用start方法重新启动。
    2. 将实现Runnable接口的类对象以参数的形式传递给Thread的构造方法Thread（Runnable target），构造一个Thread对象，并使用Thread的start方法启动线程。Runnable接口只定义了一个run方法，所以实现Runable接口的类必须实现这个run方法，线程启动后也是运行这个run方法。常见的形式是使用匿名内部类的形式实现Runnable接口：new Thread（new Runnable（）{public void run（）{}}；。

2. Thread类和Runnable接口的联系：Thread类本身就是一个实现Runnable接口的类，所以Thread类中实现了run方法。Thread类中也包含一个Runnable接口对象target，当使用Thread（Runnable target）构造方法接收一个Runnable接口对象构造一个Thread对象时，底层代码实际是调用init方法，将接收的Runnable接口对象赋给Thread内部的target成员变量。Thread类的run方法为if（null ！= target）{target.run()}，所以，使用Thread（Runnable target）方法构造的Threa对象实际运行的是Runnable接口对象中的run方法。而继承Thread类的子类重写run方法，所以Thread子类对象实际运行的run方法是子类重写的run方法。

3. 构造Thread对象或者其子类对象的时候，如果没有指定线程对象的名字，默认会使用“Thread-threadInitNumber”作为线程对象的线程名。其中threadInitNumber是Thread的static整型成员变量，nextThreadNum（）方法会对其自动增加。

4. 线程的实现方法中，如果线程类已经继承了其他类，那么只能使用Runnable接口方法。

5. 线程的结束方法不建议使用Thread的stop（）方法，因为该方法非常不安全，安全的结束就是让run方法运行完毕自动结束。推荐的方法是设置一个boolean变量，并在在run方法中设置一个以boolean变量为判断依据的循环，再使用另一个方法设置boolean变量的值，通过这个方法来决定线程的运行。例如采用以下结构：
    ```java
        public class MyThread extends Thread{
            private boolean flag=true;
            @Override
            public void run(){
                while(flag){
                    ...
                }
            }
            public void stopRunning(){
                flag=false;
            }
        }
    ```
6. 线程的生命周期（状态）：
    1. 创建状态：使用new构造线程对象时的状态，此时系统不为它分配资源，只是一个空的线程对象而已。
    2. 可运行状态：使用Thread的start方法后，系统为线程分配需要的系统资源（除了CPU），并调用run方法，但是此时的线程还是没有进入运行状态，只是可以随时被运行而已。
    3. 运行状态：可运行状态的线程获得CPU资源后就可以立即执行，即进入运行状态。
    4. 不可运行状态：运行状态中的线程调用了sleep方法，或者调用了wait方法等待特定条件满足，或者是线程输入或输出阻塞时，线程会放弃占有CPU进入不可运行状态，直到条件满足后再进入可运行状态。
    5. 消亡状态：当线程的run方法执行结束后，线程自然消亡。

7. 线程的优先级：线程创建时，子线程继承父线程的优先级；线程创建后，可通过调用setPriority（int）方法改变优先级，接收的优先级参数是1-10的整数，Thread类中定义了三个优先级常量MIN_PRIORITY(1)/MAX_PRIORITY(10)/NORM_PRIORITY(5)，没有指定优先级的情况下，使用默认优先级NORM_PRIORITY。

8. 一般情况下，线程调度器会调用优先级最高的线程运行，但是也要根据具体操作系统的线程调度策略决定，不同操作系统的策略可能不同，线程的调度顺序可能不同。因此，最保险的方法是通过向线程代码中添加条件控制来确保线程的执行。

9. 当线程中调用了yield（）方法时，线程会让出CPU让其他线程先执行；当线程中调用了sleep方法时，线程会进入睡眠状态；线程也会由于I/O操作而受到阻塞，停止执行；当另一个更高优先级的线程出现是，线程也有可能阻塞；在支持时间片的系统中，线程的时间片用完后也会停止运行。

10. 当多个线程访问同一个对象的成员变量或者类的静态成员变量时，多个线程对该对象的成员变量或类的静态成员变量的影响是相互的，因为它们共用同一份对象的成员变量或类的静态成员变量。如果多个线程访问的是同一个对象的局部变量，则每个线程都拥有一份局部变量的拷贝，多个线程之间的局部变量互不影响。如果多个线程访问的是不同对象的成员变量，理所当然多个线程之间互不影响，它们不共同操作一份数据。

11. 多个线程共享同一个数据可能会由于并发性导致数据的错误，解决的方法是每个线程访问该数据之前对该数据进行加锁，加锁后的数据不能再被其他线程访问，直到加锁的线程执行完毕或者抛出异常后解锁，此方法又叫多线程的同步。

12. Java中为实现多线程的同步，让每个对象都带有一个锁（Lock，也叫监视器monitor），并提供了多线程同步关键字synchronized，可用于修饰方法或者代码块：
    1. synchronized修饰方法：当线程调用被synchronized修饰的方法时，会将该方法归属的对象进行上锁。对象一旦上锁，对象中所有被synchronized修饰的方法都不能被其它线程调用（没有被synchronized修饰的方法仍然可以被其它线程调用），直到上锁线程将该方法执行完毕或抛出异常后解锁。另外，对于被synchronized修饰的static方法，由于static方法归属于类，线程调用被synchronized修饰的static方法时，加锁的对象是该类的Class对象，此时由于类的所有对象都共享同一个Class对象，所以其他线程调用该类其它对象的static方法也无法进行（一般也没有利用类的对象调用类的static方法）。
    2. synchronized修饰代码块：此方法必须在synchronized（Object obj）{}中显示指定要上锁的对象obj（只要是对象都可以）。当一个线程运行到synchronized修饰的代码块时就会将obj上锁，此时其它线程如果运行到需要将obj上锁的synchronized代码块或者方法时都无法运行，直到synchronized代码块执行完毕或者抛出异常才将对象obj解锁。代码块中必须使用obj对象来调用notify或者wait方法，否则会抛出没有获得相应对象锁但却要操作锁相关方法的异常。当obj参数为this，并且代码块中为整个方法代码时，功能与使用synchronized修饰方法一样。

13. synchronized修饰方法的同步是粗粒度的，synchronized修饰代码块的同步是细粒度的，适合精细控制同步。当一个方法中代码比较多，而且可能会引起并发错误的代码只有少量时，适合使用同步代码块以提高并发度提升效率。同步率越高，并发性越差，效率越低，多线程的优势就体现不出来。

14. 受synchronized保护的数据应该是private的，否则如果是public，外部同样可以通过对象来直接访问，那么同步保护就没有意义。

15. 由于同步机制，线程声明周期的状态多了上锁阻塞状态。当运行中的线程由于需要的数据被其它线程上锁时，线程就会转入上锁阻塞状态，进入锁池（Lock pool），直到该线程需要的数据解锁时，才会重新进入可运行状态。
