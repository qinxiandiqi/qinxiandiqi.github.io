---
# type: posts 
title: "transient修饰对象成员变量反序列化为null"
date: 2020-12-21T16:54:31+0800
authors: ["Jianan"]
summary: "1. transient关键字的用途
用于在实现Serializable接口的类中标记成员变量，使该类对象在序列化和反序列化过程中忽略该成员变量的处理。
2. transient序列化和反序列化过程中的处理方式

在序列化过程中，transient关键字修饰的成员变量默认处理方式使直接忽略
在反序列化过程中，transient关键字修饰的成员变量默认赋值该成员变量类型的默认值，例如int型为0，boolean为false，对象类型为null。

3. transient默认处理方式引发的问题
反序列化过程中"
series: ["Java"]
categories: ["Java"]
tags: ["Java", "transient", "serializable"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 671
comment_num: 2
---


## 1. transient关键字的用途
用于在实现Serializable接口的类中标记成员变量，使该类对象在序列化和反序列化过程中忽略该成员变量的处理。
## 2. transient序列化和反序列化过程中的处理方式
1. 在序列化过程中，transient关键字修饰的成员变量默认处理方式使直接忽略
2. 在反序列化过程中，transient关键字修饰的成员变量默认赋值该成员变量类型的默认值，例如int型为0，boolean为false，对象类型为null。
## 3. transient默认处理方式引发的问题
反序列化过程中，transient修饰的成员变量赋值可能不是期望的默认值，例如：
```java
import java.io.*;
public class JavaSerializableObj {
    
    public static void main(String[] args) {
        Obj obj = new Obj();
        System.out.println("Obj.member = " + obj.getMember() + "; Obj.ignore = " + obj.getIgnore());
        try {
            FileOutputStream fos = new FileOutputStream("obj");
            ObjectOutputStream oos = new ObjectOutputStream(fos);
            oos.writeObject(obj);
            oos.close();
            System.out.println("write obj to file success");
        } catch (Exception ex) {
            ex.printStackTrace();
        }
        try {
            FileInputStream fis = new FileInputStream("obj");
            ObjectInputStream ois = new ObjectInputStream(fis);
            obj = (Obj) ois.readObject();
            ois.close();
            System.out.println("read obj from file success");
        } catch (Exception ex) {
            ex.printStackTrace();
        }
        System.out.println("Obj.member = " + obj.getMember() + "; Obj.ignore = " + obj.getIgnore());
    }
    public static class Obj implements Serializable {
        private final String member = "Member";
        private final transient String ignore = new String("Ignore");
        public String getMember() {
            return member;
        }
        public String getIgnore() {
            return ignore;
        }
    }
}
```
以上输出的结果为：
```text
Obj.member = Member; Obj.ignore = Ignore
write obj to file success
read obj from file success
Obj.member = Member; Obj.ignore = null
```
Obj类中transient修饰的成员变量ignore为final类型，并且默认值为new String("Ignore")，但反序列化后缺为null，因为在反序列化过程中对象类型的值默认处理为null，这可能就不是业务逻辑所期望的。
**注意：如果这里ignore的初始值改为ignore = "ignore"（非new出来的对象），反序列化却可以正常恢复ignore默认值，原因是final String字符串对象以字符串字面量赋值，java编译器编译的时候其实把该对象编译优化为常量，反序列化自然就没有问题了。**
## 4. 自定义序列化和反序列化过程
为了解决transient关键字在序列化和反序列化过程中带来的问题，对有特殊业务要求的场景可以通过自定义序列化和反序列化过程来解决这些问题。
Serializable接口提供了三个自定义序列化过程的方法：
```java
/**
* 处理对象序列化过程，默认应该调用out.defaultWriteObject()方法序列化
* 非static和transient修饰的成员变量
*/
private void writeObject(java.io.ObjectOutputStream out)
       throws IOException
/**
* 处理对象反序列化过程，默认应该调用in.defaultReadObject()方法反序列化
* 非station和transient修饰的成员变量
*/
private void readObject(java.io.ObjectInputStream in)
       throws IOException, ClassNotFoundException;
/**
* 用于反序列化过程中识别到未定义成员变量时的回调处理，常见于同个类在反序列化时修改了继承关系，
* 或远程传输过来的序列化数据版本与当前类的序列化版本不一致。
*/
private void readObjectNoData()
       throws ObjectStreamException;
 ```
针对上面的问题可以将Obj类处理为：
```java
    public static class Obj implements Serializable {
        private final String member = "Member";
        private final transient String ignore = new String("Ignore");
        public String getMember() {
            return member;
        }
        public String getIgnore() {
            return ignore;
        }
        
        private void readObject(java.io.ObjectInputStream in)
                throws IOException, ClassNotFoundException{
            in.defaultReadObject();
            //final变量需要通过反射来赋值
            try {
                Field ignoreField = getClass().getDeclaredField("ignore");
                ignoreField.setAccessible(true);
                ignoreField.set(this, new String("Read Ignore"));
                ignoreField.setAccessible(false);
            } catch (NoSuchFieldException | IllegalAccessException e) {
                e.printStackTrace();
            }
        }
    }
 ```
输出结果为：
```text
Obj.member = Member; Obj.ignore = Ignore
write obj to file success
read obj from file success
Obj.member = Member; Obj.ignore = Read Ignore
```
