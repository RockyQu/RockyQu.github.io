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

## 一、综述
> 在你完成项目打包发布之前，很有必要加上代码混淆来避免一些用户恶意对你的 APK 进行反编译，通过反编译非加密的 dex 文件就可以看到源码，甚至用 Android Studio Analyze APK 就可以分析源代码。如果没有特殊原因，所有 APP 都应该开启混淆。最近也是有项目需要加混淆，所以整理一个系列教程做为备份与日后学习。
  
增加混淆的必要性总结
* 加密代码、资源文件，增加逆向工程的难度  
* 可以自动移除未被使用的类、方法、属性，可以在一定程度上避免64K方法数  
* 减少 APK 体积，也是 APK 瘦身的一种方法

混淆的原理 [参考](https://mp.weixin.qq.com/s/WmJyiA3fDNriw5qXuoA9MA)  
Java 是一种跨平台、解释型语言，Java 源代码编译成的 class 文件中有大量包含语义的变量名、方法名的信息，很容易被反编译为 Java 源代码。为了防止这种现象，我们可以对 Java 字节码进行混淆。混淆不仅能将代码中的类名、字段、方法名变为无意义的名称，保护代码，也由于移除无用的类、方法，并使用简短名称对类、字段、方法进行重命名缩小了程序的size。
ProGuard由shrink、optimize、obfuscate 和 preverify 四个步骤组成，每个步骤都是可选的，需要哪些步骤都可以在脚本中配置。
* 压缩(Shrink): 侦测并移除代码中无用的类、字段、方法、和特性(Attribute)。  
* 优化(Optimize): 分析和优化字节码。  
* 混淆(Obfuscate): 使用a、b、c、d这样简短而无意义的名称，对类、字段和方法进行重命名。  
上面三个步骤使代码size更小，更高效，也更难被逆向工程。  
* 预检(Preveirfy):  在 Java 平台上对处理后的代码进行预检。

## 二、代码混淆基本配置
> 在主工程目录，找到 proguard-rules.pro 文件，它是你要编写混淆配置的文件，在这个文件中编写混淆规则。如下图，先添加这两个参数配置是否开启混淆 minifyEnabled 和混淆文件位置 proguardFiles 两行代码

![1](/assets/image/2018-03-15-Android proguard rules 1.png)  

* proguard-android.txt 这个文件是系统默认混淆文件一般不需要做修改
* 在 debug 版下也可以开启混淆做为测试
* Gradle 2.2 之后，defaultProguardFile 没有使用 SDK 目录下的 proguard-android.txt，而是使用了 gradle 自带的 proguard-android.txt，不同的 gradle 版本带有不同的默认混淆文件，比如在项目根目录的 build/intermediates/proguard-files/proguard-android.txt-2.3.3，即为 gradle 自带的混淆文件。在 proguard-android.txt-2.3.3 文件中也写有说明，Gradle 2.2 之后自带混淆文件  [参考](https://mp.weixin.qq.com/s/WmJyiA3fDNriw5qXuoA9MA)  

## 三、混淆规则
### 1、哪些是不需要混淆的？
* Android 四大组件
* native方法
* Java 反射用到的类
* 自定义控件
* 枚举类
* JavaBean
* Parcelable、Serializable 序列化类
* WebView 与 JS 交互所用到的类和方法
* 引入的第三方库

### 2、混淆共通部份

### 3、自定义混淆规则



-------------------

## 总结


-------------------