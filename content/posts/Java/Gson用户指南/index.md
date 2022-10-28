---
# type: posts 
title: "Gson用户指南"
date: 2014-09-07T12:02:18+0800
authors: ["Jianan"]
summary: "作者：Inderjeet Singh, Joel Leitch, Jesse Wilson

1、Overview（概览）

Gson是一个Java类库，用于将Java对象转换为它们所代表的JSON数据，也可以用于将一个JSON字符串转换为对应的Java对象。Gson是一个开源项目，托管于http://code.google.com/p/google-gson。

Gson可以用于任意"
series: ["Gson"]
categories: ["翻译", "Gson"]
tags: ["gson", "json", "java"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 3154
comment_num: 0
---

  

作者：Inderjeet Singh, Joel Leitch, Jesse Wilson

  

# 1、Overview（概览）

  

Gson是一个Java类库，用于将Java对象转换为它们所代表的JSON数据，也可以用于将一个JSON字符串转换为对应的Java对象。Gson是一个开源项目，托管于<http://code.google.com/p/google-
gson>。

  
Gson可以用于任意的Java对象，包括已经存在但你没有对应源代码的对象。

  

# 2、Goals for Gson（Gson的目标）

  
    * 提供像toString()和构造方法（工厂方法）一样简单使用的机制来将Java对象转换为JSON或者反过来将JSON转换为Java对象。

    * 允许将已经存在并且不可修改的对象转换JSON，或者反过来。

    * 允许为对象自定义映射关系。

    * 支持任意复杂的对象。

    * 生成紧凑又易读的JSON输出。

  

# 3、Gson Performance and Scalability（Gson的性能和可扩展性）

  
以下是我们在一个台式机（皓龙双核，8GB内存，64位Ubuntu系统）下进行大量测试得出的指标。你可以使用[PerformanceTest](http://code.google.com/p/google-
gson/source/browse/trunk/gson/src/test/java/com/google/gson/metrics/PerformanceTest.java)
类进行重新测试。

    * String字符串：反序列化一个超过25MB的字符串没有任何问题（参考PerformanceTest类中的disabled_testStringDeserializationPerformance方法）。

    * 大型集合对象：

        ** 序列化一个包含一百四十万个对象的集合（参考PerformanceTest中的disabled_testLargeCollectionSerialization方法）

        ** 反序列化一个包含八万七千个对象的集合（参考PerformanceTest中的disabled_testLargeCollectionDeserialization方法）

    * Gson 1.4将字节数组和集合的限制从80KB提升到11MB。

  
注意：删除disabled_前缀后再运行测试。我们添加这个前缀是为了防止每一次运行JUnit测试都要将这些测试重新运行一遍。

  

# 4、Gson Users（Gson用户）

  
Gson原本是为Google内部大量项目创建使用的，但是现在已经有大量公司和公共项目都在实现，详细可查看[这里](https://sites.google.com/site/gson/gson-
users)。

  

# 5、Using Gson（Gson的使用）

  
Gson主要的类即为Gson类，你可以简单的使用new
Gson()创建一个Gson对象。另外也可以使用GsonBuilder这个类，它允许使用参数（例如版本控制等等）来才创建一个Gson实例。

  
Gson实例并不会保存Json操作的状态。因此，你可以重用同一个Gson对象进行多次Json序列化和反序列化操作。

  

## 5.1 Primitives Examples（基本示例）

  
（序列化）

    
    
    Gson gson = new Gson();
    gson.toJson(1);            ==> 结果为 1
    gson.toJson("abcd");       ==> 结果为 "abcd"
    gson.toJson(new Long(10)); ==> 结果为 10
    int[] values = { 1 };
    gson.toJson(values);       ==> 结果为 [1]

  

（反序列化）

    
    
    int one = gson.fromJson("1", int.class);
    Integer one = gson.fromJson("1", Integer.class);
    Long one = gson.fromJson("1", Long.class);
    Boolean false = gson.fromJson("false", Boolean.class);
    String str = gson.fromJson("\"abc\"", String.class);
    String anotherStr = gson.fromJson("[\"abc\"]", String.class);

  

## 5.2 Object Examples（对象示例）

  

    
    
    class BagOfPrimitives {
      private int value1 = 1;
      private String value2 = "abc";
      private transient int value3 = 3;
      BagOfPrimitives() {
        // no-args constructor
      }
    }

  

（序列化）

    
    
    BagOfPrimitives obj = new BagOfPrimitives();
    Gson gson = new Gson();
    String json = gson.toJson(obj);
    ==>结果为{"value1":1,"value2":"abc"}

  

注意，你不能序列化一个内部包含循环引用（比如包含自身引用）的对象，那会导致无限递归。

  

（反序列化）

    
    
    BagOfPrimitives obj2 = gson.fromJson(json, BagOfPrimitives.class);  
    ==> obj2对象与obj对象一样

  

### 5.2.1 Finer Points with Objects（关于对象的一些细节）

  

    * 完美支持对象的私有成员变量。

    * 不需要任何注解来声明一个成员变量是否需要进行序列化和反序列化。类中所有的成员变量（包括父类的成员）默认都要进行序列化和反序列化。

    * 如果一个成员变量使用了transient关键字标识，默认情况下它将被忽略，将不会进行JSON的序列化和反序列化。

    * 对于null值的正确处理：

        ** 进行序列化的时候，一个值为null的成员在输出中将被忽略。

        ** 进行反序列化的时候，对应JSON数据中丢失的成员变量将会使用null。

    * 使用synthetic关键字标识的成员将被忽略，不进行JSON的序列化和反序列化。

    * 对应外部类，内部类、匿名类和局部类中的成员将被忽略，不进行序列化和反序列化。

  

## 5.3 Nested Classes（including Inner Classes）—— 嵌套类（包括内部类）

  

Gson可以很容易的序列化静态嵌套类。

  
Gson也可以反序列化静态嵌套类。但是，Gson没办法自动序列化一个纯内部类。因为即使内部类的构造方法不需要参数，但实际上也需要一个外部类对象的引用。这个外部类对象在反序列化的时候已经是不可访问的了。你可以通过将内部类标识为静态类或者提供自定义的InstanceCreator来解决这个问题。以下是例子：

    
    
    public class A {
      public String a;
    
      class B {
    
        public String b;
    
        public B() {
          // No args constructor for B
        }
      }
    }

  

注意：上面的类B默认情况下不能使用Gson序列化。

  

Gson没办法将{"b":"abc"}反序列化为B类的实例，因为B是一个内部类。如果使用static class
B将B类标识为静态内部类，那么Gson就能够将这个字符串反序列化为B类的实例。另一种解决方法是为B类写一个自定义的InstanceCreator：

    
    
    public class InstanceCreatorForB implements InstanceCreator<A.B> {
      private final A a;
      public InstanceCreatorForB(A a)  {
        this.a = a;
      }
      public A.B createInstance(Type type) {
        return a.new B();
      }
    }

  

上面的方法可以解决这个问题，但是不推荐。

  

## 5.4 Array Examples（数组示例）

  

    
    
    Gson gson = new Gson();
    int[] ints = {1, 2, 3, 4, 5};
    String[] strings = {"abc", "def", "ghi"};

  

(序列化)

    
    
    gson.toJson(ints);     ==> 结果为 [1,2,3,4,5]
    gson.toJson(strings);  ==> 结果为 ["abc", "def", "ghi"]

  

(反序列化)

    
    
    int[] ints2 = gson.fromJson("[1,2,3,4,5]", int[].class);
    ==> ints2数组与ints数组一样

  

我们同样支持任意复杂类型的多维数组。

  

## 5.5 Collections Examples（集合示例）

  

    
    
    Gson gson = new Gson();
    Collection<Integer> ints = Lists.immutableList(1,2,3,4,5);

  

(序列化)

    
    
    String json = gson.toJson(ints); ==> json is [1,2,3,4,5]

  

(反序列化)

    
    
    Type collectionType = new TypeToken<Collection<Integer>>(){}.getType();
    Collection<Integer> ints2 = gson.fromJson(json, collectionType);
    ==> ints2集合与ints集合一样

  

相当可怕的：注意我们是如何定义集合的类型的，很不幸单纯在Java中没有办法解决这个问题。

  

### 5.5.1 Collections Limitations（集合的局限性）

  

    * 能够序列化任意对象类型的集合，但是没办法反序列化，因为没有办法让我们指定结果对象的类型。

    * 反序列化的时候，集合必须是一个具体的泛型。

  
然而只要遵循良好的Java编程习惯，所有的这些都是意义的，很少会成为一个问题。

  

## 5.6 Serializing and Deserializing Generic Types（序列化和反序列化泛型）

  

当你调用toJson(obj)方法的时候，Gson会调用obj.getClass()方法来获取类中成员变量的信息来进行序列化。同样的，你可以直接传递MyClass.class对象到formJson(json,
MyClass.class)方法，这种做法非常适合对象不是泛型的情况。然而，如果对象是一个泛型，由于Java类型的擦除关系，泛型的信息将会丢失。下面的例子可以很好的说明这个问题：

    
    
    class Foo<T> {
      T value;
    }
    Gson gson = new Gson();
    Foo<Bar> foo = new Foo<Bar>();
    gson.toJson(foo); // 没办法正确序列化foo.value
    
    gson.fromJson(json, foo.getClass()); // 将foo.value反序列化为Bar对象失败

  

上面的代码将结果解析为Bar对象失败，是因为Gson调用foo.getClass()方法来获取它的类信息，但是这个方法返回的是一个原始类，也就是Foo.class。这意味着Gson没有办法知道那是一个Foo<Bar>类型的对象，因此失败。

  
为了解决这个问题，你可以为你的泛型指定一个当前的参数类型。你可以使用TypeToken类来实现：

    
    
    Type fooType = new TypeToken<Foo<Bar>>() {}.getType();
    gson.toJson(foo, fooType);
    
    gson.fromJson(json, fooType);

  
上面的fooType实际是定义了一个局部匿名内部类，这个类里面包含了一个getType()方法可以返回完整的参数类型。  
  

## 5.7 Serializing and Deserializing Collection with Objects of Arbitrary
Types（序列化和反序列化任意对象类型的集合）

  

有时候你需要处理一些包含混合数据类的JSON数组，例如：

    
    
    ['hello',5,{name:'GREETINGS',source:'guest'}]

  

其中相对的集合如下：

    
    
    Collection collection = new ArrayList();
    collection.add("hello");
    collection.add(5);
    collection.add(new Event("GREETINGS", "guest"));

  

里面对应的Event类如下：

    
    
    class Event {
      private String name;
      private String source;
      private Event(String name, String source) {
        this.name = name;
        this.source = source;
      }
    }

  

你可以使用Gson序列化这个集合而不需要指定任何东西：toJson(collection)将会得到以上输出。

然而，使用fromJson(json,
Collection.class)进行反序列化将会失败，因为Gson没办法知道如何映射其中的类型。Gson要求你在fromJson中提供一个集合的泛型版本。因此，你有三种选择：

第一种选择：使用Gson的解析API（底层的数据流解析器或者DOM解析器JsonParser）来解析出数组元素，然后为每一个数组元素调用Gson.fromJson()方法。这是一种比较好的实现方法，具体做法可以参考以下例子：

    
    
    /*
     * Copyright (C) 2011 Google Inc.
     *
     * Licensed under the Apache License, Version 2.0 (the "License");
     * you may not use this file except in compliance with the License.
     * You may obtain a copy of the License at
     *
     * http://www.apache.org/licenses/LICENSE-2.0
     *
     * Unless required by applicable law or agreed to in writing, software
     * distributed under the License is distributed on an "AS IS" BASIS,
     * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
     * See the License for the specific language governing permissions and
     * limitations under the License.
     */
    package com.google.gson.extras.examples.rawcollections;
    
    import java.util.ArrayList;
    import java.util.Collection;
    
    import com.google.gson.Gson;
    import com.google.gson.JsonArray;
    import com.google.gson.JsonParser;
    
    public class RawCollectionsExample {
      static class Event {
        private String name;
        private String source;
        private Event(String name, String source) {
          this.name = name;
          this.source = source;
        }
        @Override
        public String toString() {
          return String.format("(name=%s, source=%s)", name, source);
        }
      }
    
      @SuppressWarnings({ "unchecked", "rawtypes" })
      public static void main(String[] args) {
        Gson gson = new Gson();
        Collection collection = new ArrayList();
        collection.add("hello");
        collection.add(5);
        collection.add(new Event("GREETINGS", "guest"));
        String json = gson.toJson(collection);
        System.out.println("Using Gson.toJson() on a raw collection: " + json);
        JsonParser parser = new JsonParser();
        JsonArray array = parser.parse(json).getAsJsonArray();
        String message = gson.fromJson(array.get(0), String.class);
        int number = gson.fromJson(array.get(1), int.class);
        Event event = gson.fromJson(array.get(2), Event.class);
        System.out.printf("Using Gson.fromJson() to get: %s, %d, %s", message, number, event);
      }
    }

  

第二种选择：注册一个Collection.class的类型适配器来查找数组中的每一个元素，并映射为合适的对象。这种做法的缺点是它可能会破坏Gson反序列化中的其它集合类型。

  
第三种选择：注册一个MyCollectionMemberType的类型适配器，并使用Collection<MyCollectionMemberType>的fromJson。这种做法只适合数组是顶层元素的情况，或者你可以改变字段的类型以匹配Collection<MyCollectionMemberType>。

  

## 5.8 Built-in Serializers and Deserializers（内置的序列化构造器和反序列化解析器）

  

Gson为常用的类提供了内置的序列化构造器和反序列化解析器（但是默认的设置可能不太适合你具体的要求）。

下面是这些类的列表：

  1、java.net.URL匹配类似“http://code.google.com/p/google-gson/”的字符串。

  2、java.net.URI匹配类似“/p/google-gson/”的字符串。

  （译注：这段话什么意思我一直理解不了= =）

你同样也能在[这里](https://sites.google.com/site/gson/gson-type-adapters-for-common-
classes-1)找到类似JodaTime这样常用类的源代码。

  

## 5.9 Custom Serialization and Deserialization（自定义序列化和反序列化）

  

有时候默认的配置可能不符合你的要求。这种情况在处理类库中的类（DateTime等等）时经常出现。

Gson允许你注册自己自定义的序列化构造器和反序列化解析器。这需要通过定义两个部分来完成：

  * Json序列化构造器：需要为一个对象定义自定义的序列化过程。

  * Json反序列化解析器：需要为一个类型定义自定义的反序列化过程。

  * 实例构造器：如果有不带参数的构造器可以访问或者已经注册了反序列化解析器，那么可以不需要提供。

  

    
    
    GsonBuilder gson = new GsonBuilder();
    gson.registerTypeAdapter(MyType2.class, new MyTypeAdapter());
    gson.registerTypeAdapter(MyType.class, new MySerializer());
    gson.registerTypeAdapter(MyType.class, new MyDeserializer());
    gson.registerTypeAdapter(MyType.class, new MyInstanceCreator());

  

registerTypeAdapter将会检查类型适配器是否实现了多个接口，如果有会全部注册上去。

  

### 5.9.1 Writing a Serializer（设计一个序列化构造器）

  

下面是自定义JodaTime DateTime类序列化构造器的例子：

    
    
    private class DateTimeSerializer implements JsonSerializer<DateTime> {
      public JsonElement serialize(DateTime src, Type typeOfSrc, JsonSerializationContext context) {
        return new JsonPrimitive(src.toString());
      }
    }

  

Gson在序列化时进入DateTime对象将会调用toJson()方法。

  

### 5.9.2 Writing a Deserializer（设计一个反序列化解析器）

  

下面是自定义JodaTime DateTime类反序列化解析器的例子：

    
    
    private class DateTimeDeserializer implements JsonDeserializer<DateTime> {
      public DateTime deserialize(JsonElement json, Type typeOfT, JsonDeserializationContext context)
          throws JsonParseException {
        return new DateTime(json.getAsJsonPrimitive().getAsString());
      }
    }

  

当Gson需要将一段JSON字符串片段解析为DataTime对象的时候将会调用fromJson()方法。

  

### 5.9.3 Finer points withs Serializers and Deserializers（序列化构造器和反序列化解析器的细节）

  

你经常需要为一个原始类型的泛型注册一个单一的处理器。

  * 例如，假设你有一个“Id”类来代表或者转换Id（例如内部代表和外部代表）

  * Id<T>类型对于所有的泛型都具有相同的序列化过程：输出本质代表的Id值。

  * 反序列过程很类似但是不完全一样：需要调用“new Id(Class<T>, String)”来返回一个Id<T>实例。

  
Gson支持注册一个单一的处理器来处理这些事情。你也可以为一个指定的类型（例如Id<RequiersSpecialHandling>需要指定的处理过程）注册特定的处理器。

toJson()和fromJson()包含的泛型类型参数可以帮你为所有对应相同原始类型的泛型写一个单一的处理器。

  

## 5.10 Writing an Instance Creator（设计一个实例构造器）

  

反序列化一个对象的时候，Gson需要为对应的类创建一个默认的实例。

规范的类会为序列化和反序列化提供一个不带参数的构造方法（无论是public或者private的构造方法）。

典型的情况是你需要处理类库中没有定义不带参数构造方法的类，你就需要一个实例构造器。

  

### 5.10.1 Instance Creator Example（实例构造器示例）

    
    
    private class MoneyInstanceCreator implements InstanceCreator<Money> {
      public Money createInstance(Type type) {
        return new Money("1000000", CurrencyCode.USD);
      }
    }

  

Type可以是一个相关的泛型类型：

  * 当需要特定泛型类型信息的时候，调用构造器是非常有用的做法。

  * 例如，Id类保存了将要被创建Id的类信息。

  

### 5.10.2 InstanceCreator for a Parameterized Type（带参数类型的实例构造器）

  

有时候你要实例化的类型是一个带参数的类型。通常这不是一个问题，因为实际实例是原始类型中的一种。

以下是一个例子：

    
    
    class MyList<T> extends ArrayList<T> {
    }
    
    class MyListInstanceCreator implements InstanceCreator<MyList<?>> {
        @SuppressWarnings("unchecked")
      public MyList<?> createInstance(Type type) {
        // No need to use a parameterized list since the actual instance will have the raw type anyway.
        return new MyList();
      }
    }

  

然而，有时候你需要创建对应实际带参数类型的实例。这种情况下，你可以使用传递进createInstance方法的类型参数。以下是例子：

    
    
    public class Id<T> {
      private final Class<T> classOfId;
      private final long value;
      public Id(Class<T> classOfId, long value) {
        this.classOfId = classOfId;
        this.value = value;
      }
    }
    
    class IdInstanceCreator implements InstanceCreator<Id<?>> {
      public Id<?> createInstance(Type type) {
        Type[] typeParameters = ((ParameterizedType)type).getActualTypeArguments();
        Type idType = typeParameters[0]; // Id has only one parameterized type T
        return Id.get((Class)idType, 0L);
      }
    }

  

上面的例子中，由于没有实际传递进来的带参数类型，Id类的实例没办法创建。我们通过传递type参数到方法里面来解决这个问题。上面例子中的type对象是一个java带参数类型，假如是Id<Foo>，那么创建的实例应该是Id<Foo>实例。因为Id类只包含一个带参数类型的参数T，我们直接使用getActualTypeArgument()方法返回的数组中的第一个元素，在这个例子中也就是Foo.class。

  

## 5.11 Compact Vs. Pretty Printing for JSON Output Fromat（对比Gson紧凑型和优雅型的输出格式）

  

Gson提供的默认输出格式是紧凑型的JSON格式。这意味着在输出的JSON结构中没有任何空白，也就是在输出的JSON中字段名和字段值、对象成员、数组中的对象之间没有任何留白。另外，“null”字段将会在输出中被忽略（注意null值仍然包含在对象的集合或者数组中）。参考下面Null
Object Support章节来配置Gson输出所有的null值。

  
如果你想要使用优雅的格式输出，你可以使用GsonBuilder配置你的Gson实例。由于我们的public
API中没有开放JsonFormatter，因此没有办法配置默认JSON输出的设置和边距。目前，我们只提供了一个默认的JsonPrintFormatter，它规定输出的格式每一行的长度为80个字符，2个字符缩进，4个字符的右边距。

  
下面的例子展示了如何使用默认的JsonPrintFormatter取代JsonCompactFormatter配置一个Gson实例。

    
    
    Gson gson = new GsonBuilder().setPrettyPrinting().create();
    String jsonOutput = gson.toJson(someObject);Gson gson = new GsonBuilder().setPrettyPrinting().create();
    String jsonOutput = gson.toJson(someObject);

  
  

## 5.12 Null Object Support（空对象支持）

  

Gson对于null字段的默认处理是忽略掉，因为这样才能生成更加紧凑的输出格式。然而，客户端必须为这些字段定义默认值，这样JSON格式才能转换回对应的Java对象。

  
以下是配置一个Gson实例来输出null的方法：

    
    
    Gson gson = new GsonBuilder().serializeNulls().create();

  

注意：当使用Gson序列化null值的时候，它将添加一个JsonNull元素到JsonElement结构中。因此，这个对象能够被用于自定义的序列化和反序列化。

  
下面是示例：

    
    
    public class Foo {
      private final String s;
      private final int i;
    
      public Foo() {
        this(null, 5);
      }
    
      public Foo(String s, int i) {
        this.s = s;
        this.i = i;
      }
    }
    
    Gson gson = new GsonBuilder().serializeNulls().create();
    Foo foo = new Foo();
    String json = gson.toJson(foo);
    System.out.println(json);
    
    json = gson.toJson(null);
    System.out.println(json);
    

  

======== 输出结果 ========  
{"s":null,"i":5}  
null  

## 5.13 Versioning Support（版本支持）

  

可以使用@Since注解来维护同一个对象的多个版本。这个注解可以用于类、字段、未来发布、方法。为了充分利用这个特性，你必须配置你的Gson实例忽略掉版本比一些版本号更大的字段或者对象。如果Gson实例没有设置版本，那么它将序列化和反序列所有的字段和类而不考虑版本的影响。

  

    
    
    public class VersionedClass {
      @Since(1.1) private final String newerField;
      @Since(1.0) private final String newField;
      private final String field;
    
      public VersionedClass() {
        this.newerField = "newer";
        this.newField = "new";
        this.field = "old";
      }
    }
    
    VersionedClass versionedObject = new VersionedClass();
    Gson gson = new GsonBuilder().setVersion(1.0).create();
    String jsonOutput = gson.toJson(someObject);
    System.out.println(jsonOutput);
    System.out.println();
    
    gson = new Gson();
    jsonOutput = gson.toJson(someObject);
    System.out.println(jsonOutput);

  

======== 输出结果 ========

{"newField":"new","field":"old"}

{"newerField":"newer","newField":"new","field":"old"}

  

## 5.14 Excluding Fields From Serialization and Deserialization（在序列化和反序列中排除字段）

  

Gson提供了多种途径来排除顶级类、字段和字段类型。以下是排除字段和类的一些方法。如果以下方法对于你需求来说不安全，那么你也可以直接自定义序列化构造器和反序列化解析器来实现。

  

### 5.14.1 Java Modifier Exclusion（Java修正器排除）

  

默认情况下，如果你将一个字段声明为transient，这个字段就会被排除。同样，如果一个字段被标识为“static”，那么默认也会被排除。如果你想把一些transient字段也包含进来，那么你可以尝试以下做法：

    
    
    import java.lang.reflect.Modifier;
    
    Gson gson = new GsonBuilder()
        .excludeFieldsWithModifiers(Modifier.STATIC)
        .create();

  

注意：你可以同时在excludeFieldsWithModifiers方法中包含多个Modifier数值，例如：

    
    
    Gson gson = new GsonBuilder()
        .excludeFieldsWithModifiers(Modifier.STATIC, Modifier.TRANSIENT, Modifier.VOLATILE)
        .create();

  

### 5.14.2 Gson's @Expose（Gson的@Expose注解）

  

这个功能提供一种途径来表示你的对象中哪些字段是JSON的序列化和反序列化时候要排除的。要使用这个注解，你需要使用以下方式创建Gson实例：

    
    
    new GsonBuilder().excludeFieldsWithoutExposeAnnotation().create();

这个Gson实例将会排除类中没有使用@Expose注解的字段。

  

### 5.14.3 User Defined Exclusion Strategies（用户自定义排除策略）

  

如果上述排除字段和类型的方法不符合你的要求，你可以定义自己的排除策略然后添加到Gson中。参考[ExclusionStrategy](http://google-
gson.googlecode.com/svn/trunk/gson/docs/javadocs/com/google/gson/ExclusionStrategy.html)的Java文档获取更多信息。

  
下面的例子展示了如何排除由指定“@Foo”注解标识和顶层类型（或者声明字段类型）String的字段。

  

    
    
      @Retention(RetentionPolicy.RUNTIME)
      @Target({ElementType.FIELD})
      public @interface Foo {
        // Field tag only annotation
      }
    
      public class SampleObjectForTest {
        @Foo private final int annotatedField;
        private final String stringField;
        private final long longField;
        private final Class<?> clazzField;
    
        public SampleObjectForTest() {
          annotatedField = 5;
          stringField = "someDefaultValue";
          longField = 1234;
        }
      }
    
      public class MyExclusionStrategy implements ExclusionStrategy {
        private final Class<?> typeToSkip;
    
        private MyExclusionStrategy(Class<?> typeToSkip) {
          this.typeToSkip = typeToSkip;
        }
    
        public boolean shouldSkipClass(Class<?> clazz) {
          return (clazz == typeToSkip);
        }
    
        public boolean shouldSkipField(FieldAttributes f) {
          return f.getAnnotation(Foo.class) != null;
        }
      }
    
      public static void main(String[] args) {
        Gson gson = new GsonBuilder()
            .setExclusionStrategies(new MyExclusionStrategy(String.class))
            .serializeNulls()
            .create();
        SampleObjectForTest src = new SampleObjectForTest();
        String json = gson.toJson(src);
        System.out.println(json);
      }

  

======== 输出结果 ========

{"longField":1234}

  

## 5.15 JSON Field Naming Support（JSON字段命名支持）

  

Gson支持一些预定义的字段命名策略来将标准的Java字段名（例如小写字母开头的骆驼命名法——“sampleFieldNameInJava”）覆盖为Json的字段名（例如sample_field_name_in_java或者SampleFieldNameInJava）。参考[FieldNamingPolicy](http://google-
gson.googlecode.com/svn/trunk/gson/docs/javadocs/com/google/gson/FieldNamingPolicy.html)类查看预定义的命名策略。

  
Gson同样也提供了一个基于策略的注解允许客户端为每一个字段自定义名字。注意基于策略的注解会检查字段的名字，如果注解值提供了一个无效的名字将会抛出“Runtime”异常。

  
下面例子展示了如何同时使用Gson的预定义命名策略和注解命名策略特性：

    
    
    private class SomeObject {
      @SerializedName("custom_naming") private final String someField;
      private final String someOtherField;
    
      public SomeObject(String a, String b) {
        this.someField = a;
        this.someOtherField = b;
      }
    }
    
    SomeObject someObject = new SomeObject("first", "second");
    Gson gson = new GsonBuilder().setFieldNamingPolicy(FieldNamingPolicy.UPPER_CAMEL_CASE).create();
    String jsonRepresentation = gson.toJson(someObject);
    System.out.println(jsonRepresentation);

  

======== 输出结果 ========

{"custom_naming":"first","SomeOtherField":"second"}

  

如果你需要自定义命名策略（参考这个[讨论](http://groups.google.com/group/google-
gson/browse_thread/thread/cb441a2d717f6892)），你可以使用@SerializedName注解。

  

## 5.16 Sharing State Across Custom Serializers and
Deserializers（通过自定义序列化构造器和反序列化解析器共享状态）

  

有时候你需要通过自定义序列化构造器和反序列化解析器来共享状态（参考这里的[讨论](http://groups.google.com/group/google-
gson/browse_thread/thread/2850010691ea09fb)）。你可以通过以下三种方法来达到目的：

  1、使用静态变量保存共享的状态。

  2、声明序列化构造器或反序列化解析器作为父类的内部类，然后使用父类的实例变量保存共享的状态。

  3、使用Java ThreadLocal。

1和2是非线程安全的做法，3是线程安全的。

  

## 5.17 Streaming （流操作）

  

由于Gson的对象模型和数据绑定，你可以使用Gson读写一个数据流。你可以组合数据流和对象模型入口来获取最佳的实现。

  

# 6、Issues in Designing Gson（设计中的问题）

  

参考[Gson design document](https://sites.google.com/site/gson/gson-design-
document)中关于我们在设计Gson过程中遇到的问题。里面也包含Gson和其它用于Json转换的Java类库的比较。

  

# 7、Future Enhancements to Gson（Gson未来的强化）

  

对于最新的功能增强建议列表或者你有什么新的建议，可以参考[Issue Session](http://code.google.com/p/google-
gson/issues/list)。

  

  

原文地址：<https://sites.google.com/site/gson/gson-user-guide>

  

  

