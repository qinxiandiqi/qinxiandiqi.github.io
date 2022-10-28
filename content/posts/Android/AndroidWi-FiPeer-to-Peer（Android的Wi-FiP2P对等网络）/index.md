---
# type: posts 
title: "Android Wi-Fi Peer-to-Peer（Android的Wi-Fi P2P对等网络）"
date: 2014-11-17T18:51:14+0800
authors: ["Jianan"]
summary: "Wi-Fi peer-to-peer（P2P，对等网络），它允许具备相应硬件的Android 4.0（API level 14）或者更高版本的设备可以直接通过wifi而不需要其它中间中转节点就能直接通信（Android的Wi-Fi P2P框架符合Wi-Fi联盟的Wi-Fi Direct™直连认证标志）。使用这些API，你可以搜索并连接其它同样支持Wi-Fi P2P的设备，然后再通过一个高速的连接进"
series: ["Android"]
categories: ["翻译", "Android"]
tags: ["android", "wi-fi", "p2p", "对等网络"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 4813
comment_num: 0
---

  

Wi-Fi peer-to-peer（P2P，对等网络），它允许具备相应硬件的Android 4.0（API level
14）或者更高版本的设备可以直接通过wifi而不需要其它中间中转节点就能直接通信（Android的Wi-Fi P2P框架符合Wi-Fi联盟的Wi-Fi
Direct™直连认证标志）。使用这些API，你可以搜索并连接其它同样支持Wi-Fi
P2P的设备，然后再通过一个高速的连接进行互相通信，并且这个连接的有效距离要比蓝牙连接的有效距离要长的多。这对于需要在用户之间共享数据的应用程序非常有用，例如多玩家游戏或者照片分享之类应用。

  
Android的Wi-Fi P2P API主要包含以下几个部分：

*****
使用[WifiP2pManager](http://developer.android.com/reference/android/net/wifi/p2p/WifiP2pManager.html)类定义的方法搜索、请求并连接到其它对等设备。

*****
监听WiFiP2pManager的方法调用是否成功或者失败的Listener（监听器）。当调用WiFiP2pManager的方法时，每一个方法都可以接收一个指定的监听器作为参数。

***** 由Wi-Fi P2P框架识别并发送的指定事件类型的Intent，例如撤销一个连接或者新发现了一个对等设备。

  
通常你需要一起使用这三个主要部分的API。例如，你可以在调用discoverPeers()方法的时候提供[WifiP2pManager.ActionListener](http://developer.android.com/reference/android/net/wifi/p2p/WifiP2pManager.ActionListener.html)监听器对象，这样你就能得到ActionListener.onSuccess()和ActionListener.onFailure()方法的通知。如果discoverPeers()方法发现对等设备列表有更新，同样也会发送
**WIFI_P2P_PEERS_CHANGE_ACTION** 类型的Intent广播。

  

# 1、API概览

  
WifiP2pManager类提供了与设备上的Wi-Fi硬件进行交互的方法，例如搜索和连接对等设备。以下是可用的操作：

  
**表一：Wi-Fi P2P方法**

方法 | 描述  
---|---  
initialize() | 在Wi-Fi框架上注册应用程序。这个方法必须在调用其它Wi-Fi P2P方法之前调用。  
connect() | 使用指定配置与一台设备开始一个P2P对等连接。  
cancelConnect() | 取消正在进行中的P2P对等网络组的交互。  
requestConnectInfo() | 请求一台设备连接信息。  
createGroup() | 创建一个对等网络组，并将当前设备作为群组的拥有者。  
removeGroup() | 移除当前的P2P对等网络组。  
requestGroupInof() | 请求P2P对等网络组信息。  
discoverPeers()  | 开始搜索对等网络（设备）。  
requestPeers() | 请求当前已发现的对等网络（设备）。  
  
  
WifiP2pManager的方法允许你传递一个listener监听器，这样Wi-Fi
P2P框架才能通知你的activity所调用方法的状态。可使用的listener监听器接口和相应调用这些监听器的WifiP2pManager方法如下表：

  
**表二：Wi-Fi P2P监听器**

监听器接口 | 可被使用的操作  
---|---  
WifiP2pManager.ActionListener |
connect(),cancelConnect(),createGroup(),removeGroup(),discoverPeers()  
WifiP2pManager.ChannelListener |  initialize()  
WifiP2pManager.ConnectionInfoListener | requestConnectInfo()  
WifiP2pManager.GroupInfoListener | requestGroupInfo()  
WifiP2pManager.PeerListListener | requestPeers()  
  
  
Wi-Fi P2P的API定义某些Wi-Fi P2P事件发生时要广播的Intent，例如发现了一个新的对等网络（设备）或者一台设备的Wi-
Fi状态改变的事件。你可以在你的应用程序中创建并注册一个广播接受者来处理下面这些Intent：

**  
表三：Wi-Fi P2P Intents**

Intent | 描述  
---|---  
WIFI_P2P_CONNECTION_CHANGED_ACTION | 当设备的Wi-Fi连接状态改变时会发送这个广播。  
WIFI_P2P_PEERS_CHANGED_ACTION |
当你调用discoverPeers()方法就会收到这个广播。如果你在你的应用程序中处理这个intent，你同样需要调用requestPeers()来获取一个最新的对等网络（设备）列表。  
WIFI_P2P_STATE_CHANGED_ACTION | 当设备的Wi-Fi P2P启用或者禁用时会发送这个广播。当设备的Wi-Fi
P2P启用或者禁用时会发送这个广播。  
WIFI_P2P_THIS_DEVICE_CHANGED_ACTION | 当设备的细节被修改时会发送这个广播，例如设备的名字。  
  

# 2、为Wi-Fi P2P的Intent创建一个广播接收器

  
广播接收器（broadcase
receiver）允许你接收Android系统广播出来的Intent，这样你的应用程序才能响应你所感兴趣的时间。创建一个广播接收器来处理Wi-Fi P2P
intent的基本步骤如下：

  
2.1
创建一个继承BroadcaseReceiver的类。在这个类的构造方法中，你最好将WifiP2pManager、WifiP2pManager.Channel，还有注册了这个广播接收器的activity作为参数传递进来。这样才能让广播接收器向activity发送更新的同时，还能在需要的时候访问Wi-
Fi硬件和通信的信道。

2.2
在广播接收器中，检查onReceive()方法中你所感兴趣的intent。基于接收到的intent完成一些必要的操作。例如，如果广播接收器接收到一个WIFI_P2P_PEERS_CHANGE_ACTION类型的Intent，你可以调用requestPeers()方法来获取当前已发现的对等网络（设备）。

  
下面的代码展示如何创建一个典型的广播接收器。这个广播接收器携带一个WifiP2pManager对象和一个activity最为参数，并在广播接收器接到一个intent的时候，使用这两个对象来适当完成所需要的操作：

    
    
    /**
     * 一个接收Wi-Fi P2P重要事件的广播接收器
     */
    public class WiFiDirectBroadcastReceiver extends BroadcastReceiver {
    
        private WifiP2pManager mManager;
        private Channel mChannel;
        private MyWiFiActivity mActivity;
    
        public WiFiDirectBroadcastReceiver(WifiP2pManager manager, Channel channel,
                MyWifiActivity activity) {
            super();
            this.mManager = manager;
            this.mChannel = channel;
            this.mActivity = activity;
        }
    
        @Override
        public void onReceive(Context context, Intent intent) {
            String action = intent.getAction();
    
            if (WifiP2pManager.WIFI_P2P_STATE_CHANGED_ACTION.equals(action)) {
                // 检查Wi-fi是否已经启用，并通知适当的activity
            } else if (WifiP2pManager.WIFI_P2P_PEERS_CHANGED_ACTION.equals(action)) {
                // 调用WifiP2pManager.requestPeers()来获取当前的对等网络（设备）列表
            } else if (WifiP2pManager.WIFI_P2P_CONNECTION_CHANGED_ACTION.equals(action)) {
                // 响应新的连接或者断开连接
            } else if (WifiP2pManager.WIFI_P2P_THIS_DEVICE_CHANGED_ACTION.equals(action)) {
                // 响应本台设备wifi状态的改变
            }
        }
    }

  

# 3、创建一个Wi-Fi P2P应用程序

  
创建一个Wi-Fi
P2P应用程序涉及到为应用程序创建并注册一个广播接收器，搜索对等网络（设备），连接一台对等设备，还有向一台对等设备传输数据。下面的章节描述如何完成这些操作。

  

## 3.1 初始设置

  
在使用Wi-Fi P2P的API之前，你必须确认你的应用程序能够访问对应的硬件并且设备支持Wi-Fi P2P协议。如果支持Wi-Fi
P2P，你就可以获取一个WifiP2pManager实例，创建并注册你的广播接收器，然后开始使用Wi-Fi P2P的API。

  
3.1.1 在Android的manifest文件中请求使用设备上Wi-Fi硬件的权限，并为你的应用程序声明正确的最小SDK版本号：

    
    
    <uses-sdk android:minSdkVersion="14" />
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
    <uses-permission android:name="android.permission.CHANGE_WIFI_STATE" />
    <uses-permission android:name="android.permission.CHANGE_NETWORK_STATE" />
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />

  
3.1.2 检查Wi-Fi P2P能够支持并且已经启用。进行这项检查的一个较好的地方是在你的广播接收器中，当广播接收器接收到
**WIFI_P2P_STATE_CHANGED_ACTION** 的时候。从而通知你的activity关于Wi-Fi P2P的状态并做出相应的应对。

    
    
    @Override
    public void onReceive(Context context, Intent intent) {
        ...
        String action = intent.getAction();
        if (WifiP2pManager.WIFI_P2P_STATE_CHANGED_ACTION.equals(action)) {
            int state = intent.getIntExtra(WifiP2pManager.EXTRA_WIFI_STATE, -1);
            if (state == WifiP2pManager.WIFI_P2P_STATE_ENABLED) {
                // Wifi P2P已经启用
            } else {
                // Wi-Fi P2P没有启用
            }
        }
        ...
    }

  
3.1.3
在你的activity的onCreate()方法中获取一个WifiP2pManager实例，并通过调用initialize()方法将你的应用程序注册到Wi-
Fi P2P框架中。这个方法会返回一个WifiP2pManager.Channel对象，这个对象被用于连接你的应用程序和Wi-Fi
P2P框架。你同样需要使用这个WifiP2pManager和WifiP2pManager.Channel对象，还有你的activity引用创建一个你的广播接收器实例。这样才能允许你的广播接收器通知你的activity所感兴趣的事件并进行更新。它同样能让你在需要的时候操作设备的Wi-
Fi状态。

    
    
    WifiP2pManager mManager;
    Channel mChannel;
    BroadcastReceiver mReceiver;
    ...
    @Override
    protected void onCreate(Bundle savedInstanceState){
        ...
        mManager = (WifiP2pManager) getSystemService(Context.WIFI_P2P_SERVICE);
        mChannel = mManager.initialize(this, getMainLooper(), null);
        mReceiver = new WiFiDirectBroadcastReceiver(mManager, mChannel, this);
        ...
    }

  
3.1.4 创建一个intent过滤器，并在你的广播接收器中添加相同的Intent以进行检查：

    
    
    IntentFilter mIntentFilter;
    ...
    @Override
    protected void onCreate(Bundle savedInstanceState){
        ...
        mIntentFilter = new IntentFilter();
        mIntentFilter.addAction(WifiP2pManager.WIFI_P2P_STATE_CHANGED_ACTION);
        mIntentFilter.addAction(WifiP2pManager.WIFI_P2P_PEERS_CHANGED_ACTION);
        mIntentFilter.addAction(WifiP2pManager.WIFI_P2P_CONNECTION_CHANGED_ACTION);
        mIntentFilter.addAction(WifiP2pManager.WIFI_P2P_THIS_DEVICE_CHANGED_ACTION);
        ...
    }

  
3.1.5 在activity的onResume()方法中注册你的广播接收器，并在onPause()方法中注销：

    
    
    /* 使用需要匹配的intent值注册广播接收器 */
    @Override
    protected void onResume() {
        super.onResume();
        registerReceiver(mReceiver, mIntentFilter);
    }
    /* 注销广播接收器 */
    @Override
    protected void onPause() {
        super.onPause();
        unregisterReceiver(mReceiver);
    }

  
当你已经获取到WifiP2pManager.Channel对象并且设置好广播接收器，你的应用程序就可以调用Wi-Fi P2P的方法和接收Wi-Fi
P2P的intent。

你现在可以实现你的应用程序并且通过调用WifiP2pManager中的方法来使用Wi-Fi
P2P的功能。下面的章节将阐述如何执行常见的操作，比如搜索和连接对等设备。

  

## 3.2 搜索对等网络（设备）

  
搜索允许被连接的对等设备，可以调用discoverPeers()方法来检测附近范围中允许被发现的对等网络（设备）。这个方法的执行过程是异步的，如果你有创建并传递一个WifiP2pManager.ActionListener对象作为参数，你可以在这个对象中的onSuccess()和onFailure()方法中接收到操作执行的结果。其中onSuccess()方法只是通知你搜索程序已经成功执行，但是不会提供任何实际搜索到的对等设备信息；

    
    
    mManager.discoverPeers(channel, new WifiP2pManager.ActionListener() {
        @Override
        public void onSuccess() {
            ...
        }
    
        @Override
        public void onFailure(int reasonCode) {
            ...
        }
    });

  
如果搜索程序成功执行并且检测到对等网络和设备，系统将会发送 **WIFI_P2P_PEERS_CHANGED_ACTION**
类型的intent广播，你可以在广播接收器中监听这个广播以获取对等网络设备列表。当你的应用程序接收到
**WIFI_P2P_PEERS_CHANGED_ACTION**
类型的intent，你就可以使用requestPeers()方法来请求已被发现的对等网络设备列表。下面的代码展示了如何设置这些步骤：

    
    
    PeerListListener myPeerListListener;
    ...
    if (WifiP2pManager.WIFI_P2P_PEERS_CHANGED_ACTION.equals(action)) {
    
        // 从wifi p2p manager请求可使用的对等设备。这是一个异步调用的方法，
        // 调用的activity将会在PeerListListener.onPeersAvailable()的回调方法中得到列表结果。
        if (mManager != null) {
            mManager.requestPeers(mChannel, myPeerListListener);
        }
    }

  
这个requestPeers()方法同样是异步执行的方法，当对等设备列表可用时，它会在WifiP2pManager.PeerListListener接口定义的onPeersAvailable()方法中将结果返回给你的activity。onPeersAvailable()方法会提供一个WifiP2pDeviceList对象，你可以通过这个对象找到你想要连接的对等设备。

  

## 3.3 连接对等设备

  
当你从获取到的可连接对等设备列表中找出你想要连接的对等设备之后，你就可以调用connect()方法链接这个设备。这个方法的调用需要一个WifiP2pConfig对象，这个对象包含了想要连接的设备信息。你可以通过WifiP2pManager.ActionListener得到连接成功或者失败的结果。下面的代码展示了如何连接目标设备：

    
    
    //从WifiP2pDeviceList中获取一个对等设备
    WifiP2pDevice device;
    WifiP2pConfig config = new WifiP2pConfig();
    config.deviceAddress = device.deviceAddress;
    mManager.connect(mChannel, config, new ActionListener() {
    
        @Override
        public void onSuccess() {
            //连接成功的逻辑处理操作
        }
    
        @Override
        public void onFailure(int reason) {
            //连接失败的逻辑处理操作
        }
    });

##  

## 3.4 传输数据

  
一旦连接建立成功，你就可以在设备之间通过socket传输数据。传入数据的基本步骤如下：

  
3.4.1 创建一个SeverSocket。这个socket会在一个端口上等待客户端的连接，并且会一直阻塞直到连接请求出现，因此这个方法需要在后台线程执行。

3.4.2 创建一个客户端的Socket。这个客户端使用服务端ServerSocket的IP地址和端口来连接服务端设备。

3.4.3 从客户端发送数据到服务端。当客户端的socket成功连接上服务端的socket之后，你就可以通过字节流将数据从客户端发送到服务端。

3.4.4
服务端的ServerSocket等待着客户端的连接（通过accept()方法）。这个方法会一直阻塞直到客户端连接上，因此要在其它线程中调用这个方法。当连接上之后，服务端设备就能够接收到客户端发送过来的数据。完成对这些数据的一些操作，例如保存为一个文件或者显示给用户。

  
下面的例子修改于Wi-Fi P2P Demo（可以在Android
SDK中找到），它展示了如何创建客户端和服务端之间的socket通信，并从通过一个service从客户端向服务端发送了一个JPEG图片。完整的代码，可以直接查看Wi-
Fi P2P Demo。

    
    
    public static class FileServerAsyncTask extends AsyncTask {
    
        private Context context;
        private TextView statusText;
    
        public FileServerAsyncTask(Context context, View statusText) {
            this.context = context;
            this.statusText = (TextView) statusText;
        }
    
        @Override
        protected String doInBackground(Void... params) {
            try {
    
                /**
                 * 创建一个ServerSocket并等待客户端的连接.这个方法会一直阻塞直到接受了一个客户端连接
                 */
                ServerSocket serverSocket = new ServerSocket(8888);
                Socket client = serverSocket.accept();
    
                /**
                 * 如果代码能够执行到这里，说明已经连接上一个客户端并且开始传输数据
                 * 将来自客户端的数据流保存为一个JPEG文件
                 */
                final File f = new File(Environment.getExternalStorageDirectory() + "/"
                        + context.getPackageName() + "/wifip2pshared-" + System.currentTimeMillis()
                        + ".jpg");
    
                File dirs = new File(f.getParent());
                if (!dirs.exists())
                    dirs.mkdirs();
                f.createNewFile();
                InputStream inputstream = client.getInputStream();
                copyFile(inputstream, new FileOutputStream(f));
                serverSocket.close();
                return f.getAbsolutePath();
            } catch (IOException e) {
                Log.e(WiFiDirectActivity.TAG, e.getMessage());
                return null;
            }
        }
    
        /**
         * 启动能够处理JPEG图片的activity
         */
        @Override
        protected void onPostExecute(String result) {
            if (result != null) {
                statusText.setText("File copied - " + result);
                Intent intent = new Intent();
                intent.setAction(android.content.Intent.ACTION_VIEW);
                intent.setDataAndType(Uri.parse("file://" + result), "image/*");
                context.startActivity(intent);
            }
        }
    }

  
在客户端上，通过一个客户端的socket连接服务端的socket并传输数据。这个例子传输了一个客户端设备文件系统的上的JPEG文件。

    
    
    Context context = this.getApplicationContext();
    String host;
    int port;
    int len;
    Socket socket = new Socket();
    byte buf[]  = new byte[1024];
    ...
    try {
        /**
         * 使用服务端IP和端口还有连接超时时间创建一个客户端socket
         */
        socket.bind(null);
        socket.connect((new InetSocketAddress(host, port)), 500);
    
        /**
         * 从一个JPEG文件创建一个字节流，并且传输到socket的输出流上。这些数据将会在服务端设备上接收到。
         */
        OutputStream outputStream = socket.getOutputStream();
        ContentResolver cr = context.getContentResolver();
        InputStream inputStream = null;
        inputStream = cr.openInputStream(Uri.parse("path/to/picture.jpg"));
        while ((len = inputStream.read(buf)) != -1) {
            outputStream.write(buf, 0, len);
        }
        outputStream.close();
        inputStream.close();
    } catch (FileNotFoundException e) {
        //catch logic
    } catch (IOException e) {
        //catch logic
    }
    
    /**
     * 当数据传输完成或者发生异常时关闭已经打开的socket。
     */
    finally {
        if (socket != null) {
            if (socket.isConnected()) {
                try {
                    socket.close();
                } catch (IOException e) {
                    //catch logic
                }
            }
        }
    }

  
  

原文地址：<http://developer.android.com/guide/topics/connectivity/wifip2p.html>

  

  

