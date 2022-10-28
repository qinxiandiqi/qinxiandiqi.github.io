---
# type: posts 
title: "app在android 6.0或以上平台版本运行过程中请求权限"
date: 2016-05-04T14:03:56+0800
authors: ["Jianan"]
summary: "从android 6.0（API 23）开始，安装app时不需要对app的权限申请进行授权，而是在app运行的时候，用户才需要对app进行授权。这种流程精简了app的安装过程，用户不需要在安装或者升级app的时候进行授权操作。这同样也给了用户更多对app功能的控制能力；例如，用户可以选择给一个照相app访问摄像头的权限，但不给它访问设备地理位置的权限。用户也可以通过app的设置界面，随时撤销对app授予的权限。"
series: ["Android"]
categories: ["翻译", "Android"]
tags: ["android", "权限", "动态申请"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 4588
comment_num: 0
---

> 原文作者：Google
原文地址：[http://developer.android.com/intl/zh-cn/training/permissions/requesting.html](http://developer.android.com/intl/zh-cn/training/permissions/requesting.html)  
原文版权：[Creative Commons 2.5 Attribution License](http://creativecommons.org/licenses/by/2.5/)  
译文作者：Jianan - qinxiandiqi@foxmail.com  
版本信息：本文基于2016-04-27版本翻译  
译文版权：[CC BY-NC-ND 4.0](http://creativecommons.org/licenses/by-nc-nd/4.0/)，允许复制转载，但必须保留译文作者署名及译文链接，不得演绎和用于商业用途

<br>
# 前言
---

从android 6.0（API 23）开始，安装app时不需要对app的权限申请进行授权，而是在app运行的时候，用户才需要对app进行授权。这种流程精简了app的安装过程，用户不需要在安装或者升级app的时候进行授权操作。这同样也给了用户更多对app功能的控制能力；例如，用户可以选择给一个照相app访问摄像头的权限，但不给它访问设备地理位置的权限。用户也可以通过app的设置界面，随时撤销对app授予的权限。

系统权限可以被分为两大类，普通权限和危险权限：

* 普通权限不会引起用户隐私泄露的风险。如果你的app在manifest文件中列举了一个普通权限，系统会自动对申请的普通权限直接授权。  
* 危险权限允许app访问用户的机密信息。如果的你app在它的manifest中列举了一个普通权限，系统会自动授权。但如果你列举的是一个危险权限，用户就必须明确处理是否授权给你的app。

更多相关信息，请参考[普通权限和危险权限](http://developer.android.com/guide/topics/security/permissions.html#normal-dangerous)。  

无论是哪个android版本，你的app都必须在它的manifest文件上声明它需要的所有普通权限和危险权限，详情请参考[声明权限](http://blog.csdn.net/qinxiandiqi/article/details/51275412)。只不过，声明权限的结果因系统版本和app的target SDK不同而不同：

* 如果设备运行在android 5.1或者更低版本的系统上，或者app的target SDK是22或者更低：如果你的app在它的manifest文件中声明了一个危险权限，那么用户在安装app的时候就必须授权这个危险权限给你的app；如果用户不同意授予这个权限，系统就拒绝安装这个app。  
* 如果设备运行在android 6.0或者更高的系统版本上，并且app的target SDK是23或者更高：app必须在manifest文件中声明所有需要的权限，并且在app运行的过程中必须请求用户对每个使用到的危险权限进行授权。用户可以选择授权或者拒绝每一个权限，如果用户拒绝了授权，app同样可以继续运行，只是对应的功能无法使用。  

> **注意：** 从android 6.0（API 23)开始，不管app的target SDK是不是低版本的SDK，用户都可以随时取消任意app已经授权过的权限。不管你的app是什么target SDK版本，当你的app缺少一个需要的权限时，你都应该测试的app功能是否正常。  

本章节内容将探讨如何使用android support library来检查和请求权限。android的framework在android 6.0（API 23）版本上提供了类似的方法。不过，相对来说使用support library的方法会比较简单，因为这样你的app在调用这些方法之前就不需要检查当前运行的android系统版本是多少。  

<br>
# 检查权限
---

如果你的app需要一个危险权限，那么你每次执行需要这个危险权限的操作时，你都必须检查是否已经申请到这个权限。用户随时都可以撤销授予过的权限，因此，尽管一个app昨天可以正常使用摄像头，但不代表今天仍然拥有使用摄像头的权限。  

检查你的app是否拥有过一个权限，需要调用ContextCompat.checkSelfPermission()方法。例如，下面的代码片段检查了activity是否拥有写日历的权限：  
``` java
// Assume thisActivity is the current activity
int permissionCheck = ContextCompat.checkSelfPermission(thisActivity,
        Manifest.permission.WRITE_CALENDAR);
```
如果app拥有这个权限，方法会返回PackageManager.PERMISSION_GRANTED，app可以继续执行相关操作。如果app不具备该权限，方法会返回PERMISSION_DENIED，app需要明确请求用户授予该权限。

<br>
# 申请权限
---

如果你的app需要使用一个在它的manifest文件中声明的危险权限，那么在使用之前它必须请求用户授予该权限。android提供了一系列方法供你申请权限。请求这些方法会唤起一个标准的android对话框，这个对话框你不能自定义。  

## 解释app为什么需要权限

在一些情况下，你可能想要帮助用户理解为什么你的app需要一个权限。例如，如果用户启动一个摄影app，用户应该对app请求使用摄像头的权限不会感到惊讶，但是用户可能无法理解为什么app还要求访问设备的地理位置和通讯录。在你申请一个权限之前，你应该考虑给用户提供一个解释。但是要注意不要用解释过多骚扰用户；如果你提供了太多的解释，用户可能会厌烦你的app并且卸载掉。  

只有在用户已经拒绝了权限请求的情况下，你才有可能需要向用户提供一个解释。如果用户继续尝试使用需要申请权限的功能，但是这个权限之前已经被用户拒绝，这就说明用户可能不清楚为什么app的这个功能需要申请权限。类似这样的情况，那么显示一个权限申请解释就是一种很好的解决方法。  

为了帮助判断用户在什么情况下需要一个权限解释，android提供了一个工具方法，ActivityCompat.shouldShowRequestPermissionRationale()。如果app之前申请过一个权限，但这个权限被用户拒绝了，这个方法就会返回true。  
> **注意：** 如果用户在过去拒绝权限申请的同时还勾选了系统权限申请对话框中的**不再询问**选项，这个方法也会返回false。如果是在设备权限设置中禁止了app使用的权限，用这个方法判断这些权限同样也会返回false。  

## 申请你需要的权限

如果你的app目前不具备它所需要的权限，app必须调用requestPermissions()多个重载方法中的一个来申请适当的权限。这些方法中，你的app除了将所需要的权限作为参数传递进去之外，还需要传递一个整型值作为本次权限请求的标识码。这些方法都是异步的：方法会直接返回，等到用户响应对话框之后，系统才会回调app的回调方法把结果传递回来，结果中包括app调用requestPermissions()方法时传递进来的请求码。  

下面的代码检查app是否拥有读取用户联系人的权限，如果没有，在需要的时候会申请权限：  
``` java
// Here, thisActivity is the current activity
if (ContextCompat.checkSelfPermission(thisActivity,
                Manifest.permission.READ_CONTACTS)
        != PackageManager.PERMISSION_GRANTED) {

    // Should we show an explanation?
    if (ActivityCompat.shouldShowRequestPermissionRationale(thisActivity,
            Manifest.permission.READ_CONTACTS)) {

        // Show an expanation to the user *asynchronously* -- don't block
        // this thread waiting for the user's response! After the user
        // sees the explanation, try again to request the permission.

    } else {

        // No explanation needed, we can request the permission.

        ActivityCompat.requestPermissions(thisActivity,
                new String[]{Manifest.permission.READ_CONTACTS},
                MY_PERMISSIONS_REQUEST_READ_CONTACTS);

        // MY_PERMISSIONS_REQUEST_READ_CONTACTS is an
        // app-defined int constant. The callback method gets the
        // result of the request.
    }
}
```  
> **注意：** 当你的app调用requestPermissions()方法时，系统会弹出一个标准的对话框给用户。你的app无法配置或者修改这个对话框。如果你需要向用户提供一些的信息或者解释，你应该在调用requestPermissions()方法之前进行这些操作，请参考上面小节**解释app为什么需要权限**。  

## 处理权限请求的响应

当你的app申请权限之后，系统会显示一个对话框给用户。当用户响应之后，系统会回调你的app的**onRequestPermissionsResulet()**方法，将响应结果传递过来。你的app必须重写这个方法来判断申请的权限是否被授予了。回调方法会接收到与你传递到**requestPermissions()**方法中相同的请求码。例如，如果一个app请求READ_CONTACTS访问权限，它可能需要以下的响应代码：  

``` java
@Override
public void onRequestPermissionsResult(int requestCode,
        String permissions[], int[] grantResults) {
    switch (requestCode) {
        case MY_PERMISSIONS_REQUEST_READ_CONTACTS: {
            // If request is cancelled, the result arrays are empty.
            if (grantResults.length > 0
                && grantResults[0] == PackageManager.PERMISSION_GRANTED) {

                // permission was granted, yay! Do the
                // contacts-related task you need to do.

            } else {

                // permission denied, boo! Disable the
                // functionality that depends on this permission.
            }
            return;
        }

        // other 'case' lines to check for other
        // permissions this app might request
    }
}
```  
这个系统显示的对话框会描述你的app需要访问的权限组，它不会列举具体使用到的哪一个权限。例如，如果你请求READ_CONTACTS权限，系统对话框只会描述你的app需要访问设备的通讯录。用户只需要对每一个权限组授权一次。一旦对一个权限组授权之后，如果你的app请求同一个权限组中的其它权限（同样也要在app的manifest文件声明），系统会自动授予这些权限。当你请求这些同个权限组中的其它权限时，系统也会回调你的onRequestPermissionsResult()方法并传递PERMISSION_GRANTED参数，回调的结果就跟用户已经通过系统对话框明确同意你的权限申请一样。  

> **注意：** 你的app仍然需要明确请求每一个它需要的权限，不过用户是否已经同意授权同个权限组中的其它权限。此外，每个权限组包含的权限在未来android的发布版中也有可能会变化。你的代码逻辑不应该建立在使用到的几个权限是否被包含在同一个权限组中的假设之上。  

例如，假设你在app的manifest中声明了READ_CONTACTS和WRITE_CONTACTS权限。如果你已经请求过READ_CONTACTS权限并且用户通过授权申请，当你再请求WRITE_CONTACTS权限时，系统会立即把这个权限也授权给app，不需要再与用户进行授权交互。  

如果用户拒绝了你的权限申请，你的app应该采取适当的响应操作。例如，你的app可能需要显示一个对话框来解释为什么你的app无法继续执行下去的原因是需要被用户拒绝的权限。  

当系统请求用户授权时，用户可以选择告诉系统需要再次请求这个权限。在这种情况下，无论app调用多少次requestPermissions()方法来申请权限，系统都会直接决绝申请。系统会回调你的onRequestPermissionsResult()回调方法并且传递PERMISSION_DENIED参数值，整个回调过程就跟用户已经直接明确拒绝你的权限申请一样。这意味着当你调用requestPermissions()方法时，你不能假设与用户直接交互的操作一定会发生。
