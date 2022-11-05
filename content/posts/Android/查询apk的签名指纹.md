---
# type: posts 
title: 查询apk的签名指纹
date: 2022-11-04T23:14:16+08:00
authors: ["Jianan"]
summary: "查看apk签名指纹的两种方式"
series: ["Android"]
categories: ["Android"]
tags: ["android", "apk", "签名指纹"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 0
comment_num: 0 
---

> 查询apk的签名指纹本质上都是使用jdk的keytool工具来查看，只不过是原材料不同。

## 一、从apk查询签名指纹
这种方式适用于只有已签名apk文件的情况，比如从应用市场下载的apk。
1. 解压apk，目的是获取apk内的META-INF/CERT.RSA文件，apk的签名信息就存放于CERT.RSA文件中。  
    PS：如果是linux系统可使用unzip解压命令解压
    ```shell
    unzip -o -d $target_path xxx.apk
    ```
    其中参数说明：
    - -o：不提示的情况下直接覆盖目标目录/文件
    - -d：指定解压到目标目录/文件
    - $target_path：指定解压路径
    - xxx.apk：待解压apk
2. 使用keytool工具查看apk签名指纹
    ```shell
    keytool -printcert -file $target_path/META-INF/CERT.RSA
    ```
## 二、直接从签名证书查询
如果手头已经有apk的签名证书，那么可以直接从签名证书中查询：
```shell
keytool -list -v -keystore $keystore_file [-storepass password]
```



