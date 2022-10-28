---
# type: posts 
title: "Android蓝牙"
date: 2014-10-27T15:22:10+0800
authors: ["Jianan"]
summary: "Android平台提供了对蓝牙网络协议栈的支持，它允许设备通过无线的方式与其它设备进行数据交换。应用程序框架通过Android Bluetooth APIs提供了对蓝牙功能的访问。这些API允许应用以无线的方式连接其它蓝牙设备，启用点对点或者多点对多点的无线功能。使用蓝牙的API，Android应用程序可以执行以下工作：

* 扫描其它蓝牙设备
* 查询本地蓝牙适配器以配对蓝牙设备
* 建"
series: ["Android"]
categories: ["翻译", "Android"]
tags: ["android", "bluetooth", "蓝牙", "socket", "通讯"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 4438
comment_num: 0
---

  

Android平台提供了对蓝牙网络协议栈的支持，它允许设备通过无线的方式与其它设备进行数据交换。应用程序框架通过Android Bluetooth
APIs提供了对蓝牙功能的访问。这些API允许应用以无线的方式连接其它蓝牙设备，启用点对点或者多点对多点的无线功能。使用蓝牙的API，Android应用程序可以执行以下工作：

  
***** 扫描其它蓝牙设备  
***** 查询本地蓝牙适配器以配对蓝牙设备  
***** 建立RFCOMM通道  
***** 通过服务搜索连接其它设备  
***** 传输或者接收其它设备的数据  
***** 管理多个连接  
  
本文档描述了如何使用Classic
Bluetooth（传统蓝牙）。传统蓝牙是耗电操作的理想选择，例如Android设备之间的数据流传输和通讯。为了适应低能耗蓝牙设备，Android
4.3（API Level 18）的API开始支持Bluetooth Low Energy（BLE）。更多信息，请参考Bluetooth Low
Energy。  
  

# 1、The Basics（基础部分）

  
本文档描述了如何使用Android Bluetooth
APIs来完成四种需要使用蓝牙通信的任务：设置蓝牙，搜索已经配对或者附近区域允许访问的设备，连接设备，还有在设备之间传输数据。  
  
所有的Bluetooth API都可以在android.bluetooth包中访问到。下面是你创建蓝牙连接需要使用到的类和接口的摘要：  
  

## 1.1 BluetoothAdapter

  
代表Local Bluetooth
Adapter（本地蓝牙适配器、蓝牙发送接收器）。BluetoothAdapter是所有蓝牙交互的入口点。使用这个对象，你可以搜索发现其他蓝牙设备，查询已配对配备列表，使用已知MAC地址实例化一个BluetoothDevice对象，创建
一个BluetoothServiceSocket来监听来自其它设备的通信。

  

## 1.2 BluetoothDevice

  
代表Remote Bluetooth
Device（远程的蓝牙设备）。使用这个对象可以通过一个BluetoothSocket来请求一个远程设备的连接，或者查询这个远程设备的信息，例如名字、地址、类别、以及配对状态。  
  

## 1.3 BluetoothSocket

  
代表Bluetooth socket（蓝牙套接字）的接口（类似于一个TCP
socket）。这是一个连接点，它允许一个应用程序通过InputStream和OutputStream与另一个蓝牙设备交换数据。  
  

## 1.4 BluetoothServerSocket

  
代表一个open server
socket（开放的服务套接字），用于监听即将传入的请求（类似于一个TCP的ServerSocket）。为了能够连接两台Android设备，一台设备必须使用这个类开放一个server
socket。当一个远程蓝牙设备发送一个连接请求给这台设备的时候，BluetoothServerSocket接受了这个请求就会返回一个已连接的BluetoothSocket。  
  

## 1.5 BluetoothClass

  
这个类描述了一台蓝牙设备的一般特征（characteristic）和功能（capabilities）。这是一串只读的属性，定义了这台设备的主要和次要设备类型，以及它的服务。然而，它对这台设备所支持的全部Bluetooth
Profile和蓝牙功能的描述并不可靠，但作为对设备类型的一种提示还是非常有用的。  
  

## 1.6 BluetoothProfile

  
代表一个Bluetooth Profile的接口。Bluetooth Profile是一个针对设备之间基础蓝牙通讯的无线接口规范。例如Hands-Free
profile（免提规范）。更多关于profile的讨论，请参考后面的Working with Profiles章节。  
  

## 1.7 BluetoothHeadset

  
提供了对手机蓝牙耳机的支持。它包括蓝牙耳机和Hands-Free(v1.5)profiles。  
  

## 1.8 BluetoothA2dp

  
定义了高质量音频流怎样通过蓝牙连接以数据流的方式从一台设备传输到另一个设备。“A2DP”代表Advanced Audio Distribution
Profile（高级音频分发）。  
  

## 1.9 BluetoothHealth

  
代表一个Health Device Profile（健康设备）的代理，用来控制蓝牙服务。  
  

## 1.10 BluetoothHealthCallback

  
用于实现BluetoothHealth回调函数的虚拟类。你必须继承这个类并实现其中的回调函数，才能够接收到关于应用程序的注册状态和蓝牙通道状态的更新。  
  

## 1.11 BluetoothHealthAppConfiguration

  
代表一个第三方蓝牙健康应用程序在注册后并与一个远程蓝牙健康设备通讯的应用配置。  
  

## 1.12 BluetoothProfile.ServiceListener

  
一个用于通知BluetoothProfile IPC客户端连接或者断开服务的接口（这意味着有一个内部服务运行着一个指定的Profile）。  
  

# 2、Bluetooth Permission（蓝牙权限）

  
为了在你的应用程序上可以使用蓝牙功能，你必须声明蓝牙权限 **android.permission.BLUETOOTH**
。你需要这个权限才能执行一些蓝牙通讯，例如请求一个连接，接受一个连接，传输数据。  
如果你的应用还要启动设备搜索或者管理蓝牙设置，那还需要声明 **android.permission.BLUETOOTH_ADMIN**
权限。大多数应用需要这个权限只是为了搜索附近的蓝牙设备。这个权限提供的其它功能不应该去使用，除非这个应用程序是一个类似“电池管理”的应用，用户期望通过这个应用去管理蓝牙设置。注意：如果你使用BLUETOOTH_ADMIN权限，那么你必须同样声明BLUETOOTH权限。  
  
在你的应用manifest文件中声明蓝牙权限，例如：  

    
    
    <manifest ... >
      <uses-permission android:name="android.permission.BLUETOOTH" />
      ...
    </manifest>

  
参考[<uses-permission>](http://developer.android.com/guide/topics/manifest/uses-
permission-element.html)获取更多关于声明应用权限的信息。  
  

# 3、Setting up Bluetooth（设置蓝牙）

  
在你的应用程序通过蓝牙通讯之前，你需要确认设备是否支持蓝牙。如果支持，还要再确认是否有启用蓝牙。  
  
如果不支持蓝牙，那么你应该友好的禁用一些蓝牙功能。如果支持蓝牙，但是没有启用，你应该在不离开你的应用程序情况下请求用户启用蓝牙。这种设置需要使用BluetoothAdapter，通过两个步骤完成：  
  

## 3.1 获取BluetoothAdapter

  
几乎所有使用蓝牙的Activity都要求有BluetoothAdapter。可以通过调用静态的getDefaultAdapter()方法获取BluetoothAdapter。这个方法会返回一个代表设备自身蓝牙适配器的BluetoothAdapter。这个BluetoothAdapter适用于整个系统，你的应用程序可以通过这个对象与系统交互。如果getDefaultAdapter()返回null，可能是设备不支持蓝牙，你的操作到此为止。例如：  

    
    
    BluetoothAdapter mBluetoothAdapter = BluetoothAdapter.getDefaultAdapter();
    if (mBluetoothAdapter == null) {
        // 设备不支持蓝牙
    }

  

## 3.2 启用蓝牙

  
下一步，你需要确认蓝牙已经启用。调用isEnable()方法检查当前蓝牙是否已经启用。如果这个方法返回false，表示蓝牙已经禁用。请求启用蓝牙，要调用startActivityForResult()和使用action为
**ACTION_REQUEST_ENABLE** 的Intent。这里将会通过系统设置弹出一个启用蓝牙的请求（不会停止你的应用程序）。例如：  

    
    
    if (!mBluetoothAdapter.isEnabled()) {
        Intent enableBtIntent = new Intent(BluetoothAdapter.ACTION_REQUEST_ENABLE);
        startActivityForResult(enableBtIntent, REQUEST_ENABLE_BT);
    }

  

将会出现一个对话框请求用户授权启用蓝牙，如图。如果用户点击“Yes”，系统将会启用蓝牙，焦点也会在程序完成（或者失败）之后返回到你的应用程序。

![](https://img-
blog.csdn.net/20141027144036437?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcWlueGlhbmRpcWk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)  

传递到startActivityForRequest()的REQUEST_ENABLE_BT常量是自己定义的一个整数（必须大于0），系统会将其作为requestCode参数传递到你的onActivityResult()方法中。  
如果启用蓝牙成功，你的activity就会在onActivityResult()中接收到一个 **RESULT_OK** 的result
code。如果蓝牙由于一个错误（或者是用户点击了“No”）而启用失败，那么result code将会是RESULT_CANCELED。  
  
另外，你的应用程序同样可以使用广播监听 **ACTION_STATE_CHANGED**
这种类型的Intent，因为系统在蓝牙状态发生改变的时候会发送广播。这种广播包含extra数据 **EXTRA_STATE** 和
**EXTRA_PREVIOUS_STATE** ，分别表示新的和旧的的蓝牙状态。这些extra数据可能值包括 **STATE_TURNING_ON** 、
**STATE_ON、STATE_TURNING_OFF** 和 **STATE_OFF**
。在你的应用程序运行的时候监听这个广播对于检测蓝牙状态引起的变化非常有用。  
  
提示：启用设备可被发现模式同样会自动启用蓝牙。如果你计划在进入蓝牙功能的Activity之前始终启用设备可被发现，你可以跳过上面步骤2。阅读关于下面启用可被发现模式部分。  
  

# 4、Finding Devices（搜索设备）

  
你可以使用BluetoothAdapter通过设备搜索或者查询已配对设备列表来发现远程设备。  
  
设备搜索是一个扫描程序，它会搜索本地附近启用了蓝牙的设备，并从这些设备上获取一些信息（这个过程有时简称为“发现中”、“查询中”或者“扫描中”）。然而，在本地区域范围内的蓝牙设备只有在它是允许被发现的模式下才会响应其它设备扫描发现的请求。如果一个设备是可被发现的状态模式，它会通过共享一些信息来响应搜索发现的请求，例如设备名字，设备类型和它唯一的MAC地址。使用这些信息，执行扫描的设备才能创建一个连接连接上被发现的设备。  
  
如果是第一次与远程设备建立连接，（本地设备）将会自动提示用户一个配对的请求。当设备配对成功之后，远程设备的基本信息（例如设备名字，类型和MAC地址）将会被保存，并且可以使用Bluetooth
APIs读取。使用已知的远程设备的MAC地址，可以在任何时候建立与远程设备的连接，而不需要预先执行扫描（即使远程设备现在不在附近范围内）。  
  
注意设备配对和设备连接两者之间有点不同。设备配对意味着两个设备彼此之间都知道对方的存在，并且共享一个连接key可以进行互相验证和创建一个加密的连接。设备连接意味着设备之间当前共享一个RFCOMM通道，能够互相传输数据。当前的Android
Bluetooth API要求在创建一个RFCOMM连接之前要先配对。（当你使用Bluetooth APIs创建一个加密连接的时候，配对是自动执行的）  
  
下面的章节阐述如何发现一个已经配对的设备，或者使用设备扫描发现一个新设备。  
  
注意：Android设备默认是不可被发现的。用户可以通过系统设置让设备在短时间内可被发现，或者一个应用程序可以要求用户在不离开应用程序的情况启用设备的可被发现模式。如何启用设备可被发现模式请参考下面讨论。  
  

## 4.1 Querying paired devices（查询配对设备）

  
在执行设备扫描之前，查询已配对设备列表看看目标设备是否已经被发现过是非常值得的。可以通过调用getBondedDevice()方法来实现，它会返回一系列代表已配对设备的BluetoothDevice。例如，你可以查询所有已配对设备，然后使用ArrayAdapter显示所有设备的名字给用户：  

    
    
    Set<BluetoothDevice> pairedDevices = mBluetoothAdapter.getBondedDevices();
    // 如果有已配对设备
    if (pairedDevices.size() > 0) {
        // 循环查看已配对设备
        for (BluetoothDevice device : pairedDevices) {
            // 添加名字和地址到一个ArrayAdapter中，以便在ListView中显示
            mArrayAdapter.add(device.getName() + "\n" + device.getAddress());
        }
    }

  
为了能够建立一个连接，只需要从BluetoothDevice中获取MAC地址。在这个例子中，它被保存为ArrayAdapter的一部分，然后展示给用户。MAC地址可以在稍后再提取出来以进行连接。你可以从下面Connecting
Device章节了解更多关于创建连接的内容。  
  

## 4.2 Discovering devices（搜索设备）

  
开始搜索设备，只需要简单调用startDiscovery()方法。这个方法是异步的，调用这个方法后会马上返回一个Boolean值表示搜素过程有没有成功开始执行。搜索过程通常会扫描12秒，之后在查询页面中扫描每一个发现的设备获取它们的蓝牙名称。  
  
你的应用程序必须注册一个BroadcastReceiver监听 **ACTION_FOUND**
类型的Intent，以接收每一个被发现设备的信息。每发现一个设备，系统都会广播一个 **ACTION_FOUND**
类型的Intent。这个Intent携带了extra数据 **EXTRA_DEVICE** 和 **EXTRA_CLASS**
，分别包含了一个BluetoothDevice和一个BluetoothClass。例如，下面注册了一个广播来处理被发现的设备：  

    
    
    // 创建一个监听ACTION_FOUND的BroadcastReceiver
    private final BroadcastReceiver mReceiver = new BroadcastReceiver() {
        public void onReceive(Context context, Intent intent) {
            String action = intent.getAction();
            // 当发现了一个设备
            if (BluetoothDevice.ACTION_FOUND.equals(action)) {
                // 从Intent中获取BluetoothDevice对象
                BluetoothDevice device = intent.getParcelableExtra(BluetoothDevice.EXTRA_DEVICE);
                // 将名字和地址添加到ListView的ArrayAdapter中
                mArrayAdapter.add(device.getName() + "\n" + device.getAddress());
            }
        }
    };
    // 注册这个BroadcastReceiver
    IntentFilter filter = new IntentFilter(BluetoothDevice.ACTION_FOUND);
    registerReceiver(mReceiver, filter); // 不要忘记在onDestroy方法中注销这个广播

  
为了能够建立一个连接，只需要从BluetoothDevice中获取MAC地址。在这个例子中，它被保存为ArrayAdapter的一部分，然后展示给用户。MAC地址可以在稍后再提取出来进行连接。你可以从下面Connecting
Device章节了解更多关于创建连接的内容。  
  
注意：执行设备搜索对于BluetoothAdapter是一个重量级的程序，它会消耗大量的资源。一旦你发现了一个设备想要建立连接，在尝试连接之前应该确保已经调用cancelDiscovery()方法停止扫描。同样，如果你已经与一个设备建立了连接再执行搜索程序，这个连接的可用带宽会显著减少，因此你不应该在建立连接之后还进行设备搜索。  
  

## 4.3 Enabling discoverability（启用可被发现模式）

  
如果你想让本机设备可被其它设备发现，要使用Action为 **ACTION_REQUEST_DISCOVERABLE**
的Intent调用startActivityForResult(Intent,
int)方法。这将会通过系统设置发出启用可被发现模式的请求（并且不会停止你的应用程序）。默认情况下，设备会进入可被发现模式120秒。你可以为Intent添加
**EXTRA_DISCOVERABLE_DURATION**
附加数据自定义时长。在一个应用程序中最多可以设置3600秒，如果将值设置为0就表示设备永远进入可被发现模式。任何小于0或者大于3600的值都会自动设置为120秒。例如下面的代码片段将时长设置为300：  

    
    
    Intent discoverableIntent = new
    Intent(BluetoothAdapter.ACTION_REQUEST_DISCOVERABLE);
    discoverableIntent.putExtra(BluetoothAdapter.EXTRA_DISCOVERABLE_DURATION, 300);
    startActivityForResult(discoverableIntent, REQUEST_DISCOVERABLE);//REQUEST_DISCOVERABLE为自定义的requestcode值

  
如图，会显示一个对话框请求用户授权让设备进入可被发现模式。如果用户点击“Yes”，设备将会在执行的时间内进入可被发现模式。你的Activity将会在onActivityResult()方法中得到回调，并且resultCode等于设备可被发现的时长。如果用户点击“NO”或者遇到了错误，resultCode将会是
**RESULT_CANCELED** 。  
![](https://img-
blog.csdn.net/20141027145249375?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcWlueGlhbmRpcWk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)  

  

注意：如果设备没有启用蓝牙，那么启用设备的蓝牙可被发现模式会自动启用设备蓝牙。

  
设备将会在配置的时间内一直进入可被发现模式。如果你想要在可被发现模式发生改变的时候得到通知，你可以注册一个BroadcastReceiver监听
**ACTION_SCAN_MODE_CHANGE** 类型的Intent。这种Intent包含了 **EXTRA_SCAN_MODE** 和
**EXTRA_PREVIOUS_SCAN_MODE** 附加数据，分别代表新的和旧的扫描模式。它们的值可能为
**SCAN_MODE_CONNECTABLE_DISCOVERABLE** 、 **SCAN_MODE_CONNECTABLE** 或者
**SCAN_MODE_NONE** ，本别代表设备进入可被发现可连接模式、不可被发现但是可连接模式、不可被发现也不可连接模式。  
  
如果你只是要与远程设备建立连接，那么你不需要启用设备的可被发现模式。只有当你希望你的应用程序托管一个Server
socket来接收传入的连接才需要启用设备可被发现模式，因为远程设备在建立连接之前必须要能够发现这个设备。  
  

# 5、Connecting Devices（连接设备）

  
为了能够让你的应用程序在两台设备之间建立连接，你必须实现服务端和客户端的机制，因为一台设备必须启动一个server
socket，另一台设备必须创建连接（使用服务端设备的MAC地址来创建一个连接）。只有服务端设备和客户端各自都有一个连接在同一个RFCOMM通道上的BluetoothSocket才算完成连接。在这个时候，每一台设备才能获得一个输入和输出流，数据传输才可以开始进行，这部分会在下面Managing
a Connection章节讨论。这个章节讨论如何在两台设备间建立连接。  
  
服务端设备和客户端设备获取BluetoothSocket的方法不同。服务端设备将会在接受一个传入的连接请求时获得。客户端设备会在它打开与服务端设备的RFCOMM通道时获得。  
  
一种解决方案是自动将每一台设备作为服务端，这样每一台设备都拥有一个server
socket可以监听连接。之后，每一台设备都可以启动一个与其它设备的连接并变成客户端设备。另外，也可以明确一台设备作为服务端根据需要启动一个server
socket，其它设备只要简单的建立连接。  
  

注意：如果两台设备之前没有配对，那么Android
Framework将会在连接过程中自动提示配对请求信息或者对话框，如图。因此尝试连接其它设备的时候，你的应用程序不需要考虑设备是否已经配对。你的RFCOMM连接将会被阻塞，直到用户配对成功或者失败（用户拒绝配对，或者配对失败，或者超时）。

![](https://img-
blog.csdn.net/20141027145701922?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcWlueGlhbmRpcWk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)  

  

## 5.1 Connecting as server（作为服务端连接）

  
当你尝试去连接两台设备，其中一台设备必须作为服务端持有一个BluetoothServerSocket对象。server
socket的目的是为了监听传入的连接请求，并在接收请求之后提供一个已连接的BluetoothSocket。当从BluetoothServerSocket获取到BluetoothSocket之后，BluetoothServerSocket应该回收掉，除非你还要接收其它连接。  
  
下面是设置一个server socket并接收一个连接的一般过程：  
  
**5.1.1 调用listenUsingRfcommWithServiceRecord(String,
UUID)方法获取一个BluetoothServerSocket。**
方法中的字符串参数是一个表示你的服务的名字，系统将会自动把这个字符串写入到设备上一个新的Service Discovery
Protocol（SDP）数据库实体中（名字可以是任意的，可以简单使用你的应用程序名字）。另一个参数UUID也会被包含到SDP实体中，并且会作为与客户端设备连接协议的基础。也就是，当客户端设备尝试连接服务端设备的时候，它会携带一个UUID来标识服务端设备上的哪一个服务才是客户端设备要连接的。这些UUID必须匹配后才能接受连接请求（在下一步中讨论）。  
  
**About UUID：** 一个Universally Unique
Identifier（UUID，通用唯一的ID）是一个标准128格式的字符串ID，用于唯一标识的信息。UUID的关键是它是一个足够大的值，你可以选择任意随机值也不会引起冲突。在这个例子中，它被用来唯一标识你的应用程序的蓝牙服务。为你的应用程序获取一个UUID，你可以使用网上随机生成的众多UUID中的一个，然后使用UUID类的fromString(String)方法生成一个UUID。  
  
**5.1.2 调用accept()方法开始监听连接请求。**
这个方法会阻塞，直到接受了一个连接请求或者出现异常才会返回。只有当一个远程设备发送一个携带UUID的连接请求，并且请求的UUID能够匹配注册server
socket时的UUID，这个连接请求才会被接受。连接成功后，accept()方法就会返回一个已经连接上的BluetoothSocket。  
  
**5.1.3  除非你想要接受其它连接，否则你应该马上调用close()方法。**这样才能够释放server
socket和它所占用的资源，但是不会关闭accept()方法返回已连接的BluetoothSocket。不像TCP/IP协议，RFCOMM在同一时间同一条通道上只允许连接一个客户端，因此在BluetoothServerSocket接受一个连接socket之后立即调用close()方法是有意义的。  
  
accept()方法不应该在UI线程中执行，因为这个方法会被阻塞，妨碍其它应用程序的交互。通常都是由你的应用程序新开一个线程执行所有BluetoothServerSocket或者BluetoothSocket的操作。终止一个会阻塞的方法，例如accept()，可以在其他线程调用BluetoothServerSocket或者BluetoothSocket的close()方法，阻塞的方法会立即返回。注意所有BluetoothServerSocket或者BluetoothSocket中的方法都是线程安全的。  
  
例子：  
以下是关于服务端机制接收传入连接的简单线程：  

    
    
    private class AcceptThread extends Thread {
        private final BluetoothServerSocket mmServerSocket;
     
        public AcceptThread() {
            // 使用一个临时对象，稍后赋值给mmServerSocket,
            // 因为mmServerSocket被声明为final
            BluetoothServerSocket tmp = null;
            try {
                // MY_UUID是应用的UUID字符串,同样也会在客户端的代码中使用到
                tmp = mBluetoothAdapter.listenUsingRfcommWithServiceRecord(NAME, MY_UUID);
            } catch (IOException e) { }
            mmServerSocket = tmp;
        }
     
        public void run() {
            BluetoothSocket socket = null;
            // 保持监听知道出现异常或者返回一个socket
            while (true) {
                try {
                    socket = mmServerSocket.accept();
                } catch (IOException e) {
                    break;
                }
                // 如果接受了一个连接
                if (socket != null) {
                    // 执行连接的管理操作(在一个独立的线程中)
                    manageConnectedSocket(socket);
                    mmServerSocket.close();
                    break;
                }
            }
        }
     
        /** 将会终止socket的监听，并让线程结束 */
        public void cancel() {
            try {
                mmServerSocket.close();
            } catch (IOException e) { }
        }
    }

  
在这个例子中只会接受一个传入的连接，因此只要接受了一个连接就能获取一个BluetoothSocket，应用程序会将获取到的BluetoothSocket发送到一个独立的线程中，然后关闭BluetoothServerSocket并终止循环。  
  
注意accept()方法返回的BluetoothSocket已经连接成功，你不能再调用它的connect()方法（这是你在客户端中需要调用的方法）。  
  
manageConnectedSocket()方法是应用程序假定的方法，它应该要为传输数据创建一个线程，这会在后面Managing a
Connection章节讨论。  
  
你应该尽可能在你监听完传入的连接之后关闭你的BluetoothServerSocket。在这个例子中，获取到BluetoothSocket之后就马上调用了close()方法。你同样可能需要为你的线程提供一个公共的方法来关闭私有的BluetoothServerSocket，以便在你需要停止server
socket监听的时候停止。  
  

## 5.2 Connecting as a client（作为客户端连接）

  
为了能够与一个远程设备（一个持有server
socket的设备）建立连接，你首先需要获取一个代表远程设备的BluetoothDevice对象。（获取一个BluetoothDevice对象的方法已经在上面Finding
Device章节阐述过）。之后你需要使用BluetoothDevice来请求一个BluetoothSocket并建立连接。  
  
以下是基本步骤：  
  
**5.2.1
使用BluetoothDevice对象调用createRfcommSocketToServiceRecord(UUID)方法获取一个BluetoothSocket对象。**
这里获取的BluetoothSocket对象将会连接到对应的BluetoothDevice。这里的UUID必须匹配服务端设备启用BluetoothServerSocket（调用listenUsingRfcommWithServiceRecord(String,
UUID)）时所用的UUID。使用相同的UUID可以在你的应用程序中写死一个UUID字符串，然后在服务端和客户端中使用。  
  
**5.2.2 调用connect()方法创建连接。**
在这里调用这个方法后，系统将会在远程设备上执行SDP查询匹配UUID。如果查询成功，远程设备就会接受这个连接，它会在连接期间共享RFCOMM通道，然后connect()返回。这个方法也是个阻塞的方法。如果因为某种原因连接失败或者connect()方法超时（大概12秒后），这个方法就会抛出一个异常。因为connect()方法也是个阻塞的方法，所以执行这个方法也要在一个在主线程之外独立一个线程执行。  
  
注意：你需要经常确保你在调用connect()方法的时候设备没有执行设备搜索。如果正在搜索设备，那么尝试连接的过程将会显著变慢，连接失败的几率也会更大。  
  
例子  
以下是创建一个蓝牙连接的简单线程例子：  

    
    
    private class ConnectThread extends Thread {
        private final BluetoothSocket mmSocket;
        private final BluetoothDevice mmDevice;
     
        public ConnectThread(BluetoothDevice device) {
            // 使用临时对象稍候赋值给mmSocket,
            // 因为mmSocket的类型是final
            BluetoothSocket tmp = null;
            mmDevice = device;
     
            // 获取一个BluetoothSocket来连接给定的BluetoothDevice
            try {
                // MY_UUID是应用的UUID字符串, 也是服务端使用的UUID字符串
                tmp = device.createRfcommSocketToServiceRecord(MY_UUID);
            } catch (IOException e) { }
            mmSocket = tmp;
        }
     
        public void run() {
            // 取消扫描设备，因为扫描会让连接速度变慢
            mBluetoothAdapter.cancelDiscovery();
     
            try {
                // 通过socket连接设备,这将会阻塞
                // 直到连接成功或者抛出异常
                mmSocket.connect();
            } catch (IOException connectException) {
                // 连接失败;关闭socket并退出
                try {
                    mmSocket.close();
                } catch (IOException closeException) { }
                return;
            }
     
            // 进行连接管理的操作 (在一个指定的线程中)
            manageConnectedSocket(mmSocket);
        }
     
        /** 将会取消正在进行中的连接,并关闭socket */
        public void cancel() {
            try {
                mmSocket.close();
            } catch (IOException e) { }
        }
    }

  
注意cancelDiscovery()方法需要在连接之前调用。你应该经常在连接之前调用这个方法，调用这个方法的时候不需要确定当前是否正在扫描其它设备（但是如果你真的要检查当前是否正在扫描设备，你也可以调用isDiscovery()方法）。  
  
manageConnectedSocket()方法是应用程序假定的方法，它应该要为传输数据创建一个线程，这会在后面Managing a
Connection章节讨论。  
  
当你对BluetoothSocket的操作完成之后，你应该调用close()方法来释放资源。当你调用这个方法后，会立刻关闭连接中的socket并回收所有网络资源。  
  

# 6、Managing a Connection（管理连接）

  
当你成功连接两台或者更多设备，每一台设备都会有一个已连接的BluetoothSocket。这就是所有有趣事情的开端，因为你可以开始在设备之间共享数据。使用BluetoothSocket，一般的传出数据过程是很简单的：  
  
**6.1
通过socket用getInputStream()和getOutputStream()方法分别获取InputStream和OutputStream对象来处理数据传输。  
6.2 使用read(byte[])和write(byte[])方法从数据流中读写数据。**  
  
以上就是整个过程。  
  
当然，也有一些细节的东西需要考虑。首先，你需要为所有数据流的读写操作分派一个特定的线程。这是非常重要的，因为read(byte[])和write(byte[])方法都是会阻塞的方法。read(byte[])方法在从数据流中读取到数据之前会一直阻塞。write(byte[])方法不会经常阻塞，但是在远程设备没有及时调用read(byte[])方法并且中间缓冲区满的时候也会进行阻塞。因此，你的线程中的主循环应该专门用来从InputStream中读取数据。线程中再指定一个公共方法将数据写入到OutputStream中。  
  
例子  
以下是整个流程大概怎么进行的例子：  

    
    
    private class ConnectedThread extends Thread {
        private final BluetoothSocket mmSocket;
        private final InputStream mmInStream;
        private final OutputStream mmOutStream;
     
        public ConnectedThread(BluetoothSocket socket) {
            mmSocket = socket;
            InputStream tmpIn = null;
            OutputStream tmpOut = null;
     
            // 使用临时对象获取输入输出流，
            // 因为成员输入输出流的类型是fianl
            try {
                tmpIn = socket.getInputStream();
                tmpOut = socket.getOutputStream();
            } catch (IOException e) { }
     
            mmInStream = tmpIn;
            mmOutStream = tmpOut;
        }
     
        public void run() {
            byte[] buffer = new byte[1024];  // 数据流的数据缓冲区
            int bytes; // 从read()方法返回的字节数
     
            // 保持对InputStream的监听，直到有异常抛出
            while (true) {
                try {
                    // 从InputStream中读取数据
                    bytes = mmInStream.read(buffer);
                    // 将获取的字节发送到UI activity
                    mHandler.obtainMessage(MESSAGE_READ, bytes, -1, buffer)
                            .sendToTarget();
                } catch (IOException e) {
                    break;
                }
            }
        }
     
        /* 在main activity中调用这个方法将数据发送到远程设备 */
        public void write(byte[] bytes) {
            try {
                mmOutStream.write(bytes);
            } catch (IOException e) { }
        }
     
        /* 在main activity中调用这个方法来停止连接 */
        public void cancel() {
            try {
                mmSocket.close();
            } catch (IOException e) { }
        }
    }

  
构造方法中获取必要的数据流，并且一旦执行，线程就会开始等待从InputStream中过来的数据。当read(byte[])方法从数据流中获取到字节数据后，这些数据就会被从main
activity得到的Handle对象发送到main activity中。然后线程循环阻塞，继续等待从数据流中传输过来的更多字节数据。  
  
发送输出数据只需要在main
activity中简单调用线程的write()方法，并将字节数据传递进去。这个方法会简单的调用write(byte[])方法将数据发送到远程设备。  
  
线程中的cancel()方法是非常重要的，有这个方法才能在任何时候通过关闭BluetoothSocket来中止连接。当你用完蓝牙连接之后，你应该要调用这个方法。  
  
使用Bluetooth APIs的实例，请参考[Bluetooth Chat sample
app](http://developer.android.com/resources/samples/BluetoothChat/index.html)。  
  

# 7、Working with Profile（使用Profile）

  
从Android 3.0开始，Bluetooth API已经支持Bluetooth profiles。一个Bluetooth
profile就是一种设备之间基于蓝牙通信的无线接口规范。比如Hands-Free
profile（免提）。当一部手机想要连接无线耳机的时候，两台设备都必须支持Hands-Free profile。  
  
你可以实现BluetoothProfile接口定义自己的类来指定一种特定的Bluetooth profile。Android Bluetooth
API已经提供了以下Bluetooth profile的实现：  
  
*** Headset。** Headset
profile对用于手机的蓝牙耳机提供了支持。Android提供了BluetoothHeadset类，这个类是一个代理，通过进程间通信（IPC）来控制蓝牙耳机服务。这里面包括了蓝牙耳机和Hands-
Free(v1.5) profile。BluetoothHeadset类同样提供了对AT命令的支持。更多关于这方面主题的讨论，请参考后面Vendor-
specific AT commands章节。  
  
*** A2DP。** Advanced Audio Distribution
Profile（A2DP，高级音频分发），它定义了高质量音频如何通过蓝牙连接以数据流的方式从一台设备到另一台设备进行传输。Androd提供了BluetoothA2dp类，同样是个代理，通过进程通信控制Bluetooth
A2DP。  
  
*** Health Device。** Android 4.0开始支持Bluetooth Health Device
Profile（HDP，蓝牙健康设备）。这允许你使用蓝牙创建与支持蓝牙的健康设备进行通信的应用程序，例如心律监控器、血压计、温度计、体重计等等。查看所支持的设备，以及它们相应的专业测量数据编码，请参考[www.bluetooth.org](http://www.bluetooth.org/)中的Bluetooth
Assigned Numbers。注意这些数值同样在ISO/IEEE
11073-20601[7]规范中作为MDC_DEV_SPEC_PROFILE_*命名编码附件引用。更多关于HDP的讨论，请参考后面Health
Device Profile章节。  
  
以下是使用profile的基本步骤：  
（1）获取一个默认的BluetoothAdapter，在上面Setting Up Bluetooth（设置蓝牙）部分已经提到。  
（2）使用getProfileProxy()方法建立与profile相关联代理对象的连接。在下面的例子中，这个profile代理对象是一个BluetoothHeadset对象的实例。  
（3）配置一个BluetoothProfile.ServiceListener对象。这个监听器对象会通知BluetoothProfile的IPC客户端是否已经连接或者断开服务。  
（4）在onServiceConnected()方法中获取一个profile代理对象。  
（5）一旦你获得profile代理对象，你就可以使用它来监视连接的状态和执行其它一些与profile相关的方法。  
  
例如，以下的代码片段展示了如何连接一个BluetoothHeadset代理对象然后控制Headset profile。  

    
    
    BluetoothHeadset mBluetoothHeadset;
     
    // 获取默认的adapter
    BluetoothAdapter mBluetoothAdapter = BluetoothAdapter.getDefaultAdapter();
     
    // 建立与代理的连接
    mBluetoothAdapter.getProfileProxy(context, mProfileListener, BluetoothProfile.HEADSET);
     
    private BluetoothProfile.ServiceListener mProfileListener = new BluetoothProfile.ServiceListener() {
        public void onServiceConnected(int profile, BluetoothProfile proxy) {
            if (profile == BluetoothProfile.HEADSET) {
                mBluetoothHeadset = (BluetoothHeadset) proxy;
            }
        }
        public void onServiceDisconnected(int profile) {
            if (profile == BluetoothProfile.HEADSET) {
                mBluetoothHeadset = null;
            }
        }
    };
     
    // ... 调用mBluetoothHeadset的方法
     
    // 使用完之后关闭代理连接
    mBluetoothAdapter.closeProfileProxy(mBluetoothHeadset);

  

## 7.1 Vendor-specific AT commands（特定于供应商的AT命令）

  
从android 3.0开始，应用程序可以注册系统广播接收耳机预定义的特定于供应商的AT命令发出的广播（例如Plantronics
+XEVENT命令）。例如，一个应用程序可以接收关于连接设备电池情况的广播，然后通知用户或者执行一些需要的操作。创建接收
**ACTION_VENDOR_SPECIFIC_HEADSET_EVENT** 类型Intent的广播来处理耳机的vendor-specific
AT命令。  
  

## 7.2 Health Device Profile（健康设备的Profile）

  
Android 4.0开始支持Bluetooth Health Device
Profile（HDP，健康设备的profile）。这允许你创建使用蓝牙与支持蓝牙的健康设备进行通信的应用程序，例如心律监控器、血压计、温度计、体重计等等。Bluetooth
Health
API包括了BluetoothHealth类、BluetoothHealthCallback类和BluetoothHealthAppConfiguration类，这些都在上面The
Basic（基础部分）章节提到过。  
  
使用Bluetooth Health API，了解以下HDP关键概念是非常有用的：

概念 | 描述  
---|---  
Source |
HDP定义的一个角色。一个source就是一个健康设备，它能够发送医学数据（体重、血糖指标、体温等等）到一个智能设备，例如android手机或者平板。  
Sink | HDP定义的一个角色。一个sink就是一个智能设备，能够接收医学数据。在一个Android
HDP应用程序中，sink由一个BluetoothHealthAppConfiguration对象担任。  
Registration | 指的是为一个特定的健康设备注册一个sink。  
Connection | 指的是打开一条健康设备和智能设备（例如Android手机或平板）的通信通道。  
  
  

## 7.3 Creating an HDP Application（创建一个HDP应用程序）

  
以下是创建一个Android HDP应用程序需要调用的基本步骤：  
（1）获取一个BluetoothHealth代理对象的引用。类似于耳机和A2DP
profile设备，你必须使用一个BluetoothProfile.ServiceListener和 **BluetoothProfile.HEALTH**
的profile类型调用getProfileProxy()方法，创建一个于profile代理对象的连接。  
（2）创建一个BluetoothHealthCallback，并注册一个应用程序配置（BluetoothHealthAppConfiguration）作为health
sink。  
（3）创建一个与健康设备的连接。一些设备会启动这个连接，对于这种设备不需要进行此步骤。  
（4）当成功连接上一个健康设备之后，使用文件描述读写这个健康设备。接收到的数据需要使用实现IEEE 11073-xxxxx规范的健康管理器解析。  

（5）完成之后，关闭健康通道并注销应用程序。当有扩展活动的时候，这个通道同样会被关闭。

  

本步骤中完整的代码示例请参考[Bluetooth HDP（Health Device
Profile）](http://developer.android.com/resources/samples/BluetoothHDP/index.html)。  
  
  

原文地址：<http://developer.android.com/guide/topics/connectivity/bluetooth.html>

  

  

