---
layout: post
title: "Android Studio 代码基本混淆"
excerpt: "Android 基础代码混淆教程整理"
date: 2018-3-15
categories:
  - 混淆
tags:
  - 混淆
  - 代码混淆
---

## 一、为什么需要混淆
> 在你完成项目打包发布之前，很有必要加上代码混淆来避免一些用户恶意对你的 APK 进行反编译，通过反编译非加密的 dex 文件就可以得到和源码几乎一模一样的工程。
dex基本概念：Java 源文件通过 Java 编译器生成 CLASS 文件，再通过 dx 工具转换为 classes.dex 文件。  
DEX文件从整体上来看是一个索引的结构，类名、方法名、字段名等信息都存储在常量池中，这样能够充分减少存储空间，相较于 Java 字节码文件更适合手机设备。




## 三、使用模板
模板制作完成后，将整个文件夹放到 activities 文件夹下
![2](/assets/image/2018-03-10-Android Studio Template 2.png)  

重启 Android Studio，即可看到模板了
![6](/assets/image/2018-03-10-Android Studio Template 6.png)  

-------------------

## 总结
已上模板代码已全部开源到 [GitHub](https://github.com/RockyQu/FramesTemplate) 欢迎共同学习交流(゜▽゜)つロ

-------------------