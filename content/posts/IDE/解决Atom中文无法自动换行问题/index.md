---
# type: posts 
title: "解决Atom中文无法自动换行问题"
date: 2015-09-09T15:47:35+0800
authors: ["Jianan"]
summary: "Atom是Github开发的一个开源编辑器，类似于SublimeText，功能很强大，界面很漂亮，详情可查看官网atom.io。 
  如题，Atom默认会根据窗口宽度对文本进行自动软换行处理（如果没有的话，可以在File->Settings下将Soft Wrap选项的勾打上），然而在处理中文文本的时候自动换行会失效。这是Atom的一个bug，github的issues上已经有人解决了这个问题，只是"
series: ["IDE"]
categories: ["IDE"]
tags: ["atom", "中文自动换行", "编辑器"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 24568
comment_num: 3
---

&emsp;&emsp;Atom是Github开发的一个开源编辑器，类似于SublimeText，功能很强大，界面很漂亮，详情可查看官网[atom.io](https://atom.io/)。
&emsp;&emsp;如题，Atom默认会根据窗口宽度对文本进行自动软换行处理（如果没有的话，可以在File->Settings下将Soft Wrap选项的勾打上），然而在处理中文文本的时候自动换行会失效。这是Atom的一个bug，github的issues上已经有人解决了这个问题，只是还没有添加到项目中去。目前可以通过添加AtomicChar这个插件来解决这个问题，方法如下：

1. 打开File->Settings->Install
2. 搜索框中输入atomicchar，点击Install安装，如图
![安装AtomicChar](a7244ea4e8759f79d9f1fac862cb6b54.png)
3. 重启Atom，中文自动换行解决。