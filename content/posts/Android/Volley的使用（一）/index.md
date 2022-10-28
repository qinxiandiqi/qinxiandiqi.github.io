---
# type: posts 
title: "Volley的使用（一）"
date: 2015-05-26T16:33:13+0800
authors: ["Jianan"]
summary: "Volley是google推荐的Android网络数据访问处理的库，具有简化网络数据访问、多并发、支持缓存、允许取消网络请求、支持自定义复杂网络数据请求等优点。另外，Volley也提供了处理大量网络图片、处理Json数据的工具。不过，Volley也有它的局限性。它不适用于大文件数据的下载，因为Volley在解析网络数据的过程中会将这些数据都放在内存中，处下载大型文件可能会导致内存OOM问题。在开发"
series: ["Android"]
categories: ["Android"]
tags: ["android", "Volley", "网络"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 1974
comment_num: 0
---

  

Volley是google推荐的Android网络数据访问处理的库，具有简化网络数据访问、多并发、支持缓存、允许取消网络请求、支持自定义复杂网络数据请求等优点。另外，Volley也提供了处理大量网络图片、处理Json数据的工具。不过，Volley也有它的局限性。它不适用于大文件数据的下载，因为Volley在解析网络数据的过程中会将这些数据都放在内存中，处下载大型文件可能会导致内存OOM问题。在开发中，大部分情况都不需要下载大型数据文件，所以Volley还是能够满足大部分开发需求的。

  

# 1、Volley源码的下载

  

Volley源码使用git工具管理，可以使用git工具下载：

    
    
    git clone https://android.googlesource.com/platform/frameworks/volley

  
由于众所周知的原因，不会翻墙的童鞋也可以到github上面下载：

    
    
    git clone https://github.com/mcxiaoke/android-volley.git

  

# 2、Volley的主要类

  

2.1
Request<T>：Request类是Volley的核心类之一，是个抽象类，代表网络请求的操作。Volley的com.android.volley.toolbox包中提供了Request类的几个具体子类，包括StringRequest（网络请求返回的数据类型是字符串）、ImageRequest（网络请求返回的数据类型是Bitmap）、JsonObjectRequest（网络请求返回的数据类型是JsonObject），以及JsonArrayRequest（网络请求返回的数据类型是JsonArray）。想要发起一次网络请求，必须先创建一个具体的Request对象。一般直接使用toolbox中提供的Request具体子类，当然可以直接继承Request来实现特殊的网络请求。

  

2.2
RequestQueue：RequestQueue也是Volley的核心类之一，该类中包含了缓存管理和网络请求管理。想要发起网络请求，必须先获取一个RequestQueue对象，然后将创建的具体Request对象通过RequestQueue.add()方法添加进来，Request的网络请求才会真正执行。

  

2.3
DiskBasedCache：管理硬盘缓存的类，RequestQueue类中的缓存管理默认由DiskBasedCache类实现，该类中默认的缓存大小为5M。

  

2.4
BasicNetwork：管理网络请求的类，RequestQueue类通过BasicNetwork类实现网络请求的管理。这个类需要提供一个HttpStack对象才能真正实现网络请求，Volley提供了两个HttpStack子类：HurlStack（内部通过HttpURLConnection实现）和HttpClientStack(HttpClient
client)。

  

2.5
Volley：Volley的工具类，通过Volley.newRequestQueue()方法可以返回一个RequestQueue对象。在newRequestQueue方法中会将DiskBasedCache和BasicNetwork组装到最终返回的RequestQueue对象中。Volley类中有多个newRequestQueue方法的重载，所有的这些方法最终都是通过Volley.newRequestQueue(Context,
HttpStack, int)方法实现，这个方法第二个参数HttpStack为BasicNetwork提供Http
client实现，如果为null，newRequestQueue会自动提供最适合当前Android版本的HttpStack对象；第三个参数为DiskBasedCache提供缓存的大小，默认为5*1024*1024，也就是5M。如果没有特殊的需求，建议直接使用Volley.newRequestQueue创建RequestQueue对象。

  

# 3、Volley的线程结构设计

  

Volley的架构从功能上来区分，主要跨越了主线程（也就是UI线程）、缓存线程和网络线程。以下是Volley的线程结构设计图：

![](d33767e571b00f3c4e03fefe01feb260.png)  

图中蓝色部分为主线程、绿色部分为缓存线程（只有一个）、橙色部分为网络线程池。整个发送网络请求的过程大致如下：

3.1
主线程中通过Volley.newRequestQueue()等方法获取RequestQueue对象，创建具体的Request对象并通过RequestQueue.add(Request)方法添加该对象。

3.2
添加Request对象到RequestQueue后会进入缓存线程，在缓存线程查询是否有该Request对象的缓存。如果有，则直接将缓存的响应数据返回到主线程，本次访问结束。如果没有，则进入下一步。

3.3
从网络线程池中分配一个空闲的线程给该Request对象（使用Volley.newRequestQueue()创建的RequestQueue默认线程池最多4个线程），并执行Request对象定义的相关网络访问操作，以及解析数据，保存数据等等，网络访问完毕后返回主线程。

  

# 4、Volley项目导入使用

  

使用Volley有三种方式：

4.1 第一种方式直接将Volley的源码打包出jar文件放到自己Android项目的libs目录下

4.2 使用maven管理工具的项目可以在pom.xml文件中添加下面依赖：

    
    
    <dependency>
        <groupId>com.mcxiaoke.volley</groupId>
        <artifactId>library</artifactId>
        <version>{latest-version}</version>
    </dependency>

4.3 使用Android Studio的可以只项目module的gradle文件中添加下面依赖：

    
    
    compile 'com.mcxiaoke.volley:library:1.0.16'

  

# 5、使用StringRequest

  

StringRequest的使用比较简单，直接上代码：

    
    
        
        String url = "http://www.baidu.com";
        StringRequest mRequest;
        TextView tvStringRequest;
        @Override
        public void onResume() {
            super.onResume();
            mRequest = new StringRequest(Request.Method.GET, url, new Response.Listener<String>() {
                @Override
                public void onResponse(String response) {
                    tvStringRequest.setText(response);
                }
            }, new Response.ErrorListener() {
                @Override
                public void onErrorResponse(VolleyError error) {
                    tvStringRequest.setText(error.getMessage());
                }
            });
            
            tvStringRequest.setText("start call...");
            Volley.newRequestQueue(this).add(mRequest);
        }
    
        @Override
        public void onStop(){
            if(mRequest != null){
               mRequest.cancel();
            }
            super.onStop();
        }
    

其中，在onStop()中使用Request.cancel()方法取消网络请求访问。

另外，由于要访问网络，所以需要在manifest中添加访问网络的权限。

  

  

