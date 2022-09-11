---
# type: posts 
title: 生成新SSH密钥并添加到ssh-agent
date: 2022-09-11T13:05:10+08:00
featured: false
draft: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
series: [SSH]
categories: []
tags: [SSH]
images: []
summary: "新建SSH密钥，并添加到ssh-agent。"
---

## Window系统
### 新建ssh密钥
1. 打开Terminal（Power Shell终端），使用ssh-keygen生成新密钥：
    ```shell
    $ ssh-keygen -t ed25519 -C "your_email@example.com"
    ```
2. 提示新密钥保存位置，直接回车保存到默认位置：%USERPROFILE%/.ssh/id_ed25519
    ```shell
    > Enter a file in which to save the key (/c/Users/you/.ssh/id_algorithm):[Press enter]
    ```
3. 提示键入安全密码，直接回车不使用密码：
    ```shell
    > Enter passphrase (empty for no passphrase): [Type a passphrase]
    > Enter same passphrase again: [Type passphrase again]
    ```
4. 查看新密钥指纹：
    ```shell
    $ cat ~/.ssh/id_ed25519
    ```
### 添加新密钥到ssh-agent
1. Window系统ssh-agent服务默认禁用，先把ssh-agent服务启动方式改为自启动。以管理员身份打开Terminal（Power Shell终端），输入：
    ```shell
    Set-Service -Name ssh-agent -StartupType automatic
    ```
2. 添加新密钥到ssh-agent：
    ```shell
    $ ssh-add ~/.ssh/id_ed25519
    ```

## Linux或Mac系统
### 新建ssh密钥
1. 打开终端，使用ssh-keygen生成新密钥：
    ```shell
    $ ssh-keygen -t ed25519 -C "your_email@example.com"
    ```
2. 提示新密钥保存位置，直接回车保存到默认位置：%USERPROFILE%/.ssh/id_ed25519
    ```shell
    > > Enter a file in which to save the key (/home/you/.ssh/algorithm): [Press enter]
    ```
3. 提示键入安全密码，直接回车不使用密码：
    ```shell
    > Enter passphrase (empty for no passphrase): [Type a passphrase]
    > Enter same passphrase again: [Type passphrase again]
    ```
4. 查看新密钥指纹：
    ```shell
    $ cat ~/.ssh/id_ed25519
    ```
### 添加新秘钥到ssh-agent
1. 启动ssh-agent服务：
    ```shell
    $ eval "$(ssh-agent -s)"
    ```
2. 添加新密钥到ssh-agent：
    ```shell
    $ ssh-add ~/.ssh/id_ed25519
    ```