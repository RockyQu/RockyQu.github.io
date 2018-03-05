---
layout: post
title: "Android Studio NDK 开发简单实例教程"
excerpt: "简单实现了Java与C之间的互相调用实例"
date: 2018-1-18
categories:
  - Android NDK
tags:
  - AndroidStudio NDK
  - NDK
  - JNI
---

## 一、所需工具
> Android Studio 2.3.2、NDK、CMake、LLDB

## 二、概念说明
### 1、NDK（WNative Development Kit） 
> NDK 是一系列工具的集合，帮助开发者快速开发 C/C++ 的动态库。Android程序运行在Dalvik虚拟机中，NDK允许用户使用类似 C/C++ 之类的原生代码语言执行部分程序。NDK包括从 C/C++ 生成原生代码库所需要的工具和build files。
将一致的原生库嵌入可以在Android设备上部署的应用程序包文件（application packages files ，即.apk文件）中。支持所有未来Android平台的一系列原生系统头文件和库

**为何要用到NDK?**  
概括来说主要分为以下几种情况：
- 代码的保护。由于 apk 的 java 层代码很容易被反编译，而 C/C++ 库反汇难度较大。
- 提高程序的执行效率。将要求高性能的应用逻辑使用 C 开发，从而提高应用程序的执行效率。
- 便于移植。用 C/C++ 写得库可以方便在其他的嵌入式平台上再次使用。

在Android Studio 2.2 之后，工具中增加了 CMake 的支持，你可以这么认为，在 Android Studio 2.2 之后你有2种选择来编译你写的 c/c++ 代码。一个是 ndk-build + Android.mk + Application.mk 组合，另一个是 CMake + CMakeLists.txt 组合。这2个组合与Android代码和c/c++代码无关，只是不同的构建脚本和构建命令。本篇文章主要讲解后者的组合。（也是Android现在主推的）
  
参考[NDK](https://baike.baidu.com/item/NDK/5967464?fr=aladdin)

### 2、CMake
参考[CMake](https://baike.baidu.com/item/cmake)

### 3、LLDB
> 是一个高效的c/c++的调试器，是与LLVM编译器一起使用，提供了丰富的流程控制和数据检测,有效的帮忙我们调试程序。LLDB也已经取代GDB成为XCode的默认调试器，Android Studio中也可以使用LLDB调试NDK程序，在Android Studio也中可以LLDB,从SDK Tools中下载LLDB最新版本，配合Android Studio和gradle-experimental一起调试NDK项目，会更加的方便。

### 4、JNI
> 定义 Java Native Interface，即 Java 本地接口，使得 Java 与本地其他类型语言 C/C++ 交互

**特别注意**
- JNI是 Java 调用 Native 语言的一种特性
- JNI 是属于 Java 的，与 Android 无直接关系

### 5、ABI
> ABI（Application binary interface）应用程序二进制接口。不同的CPU 与指令集的每种组合都有定义的 ABI (应用程序二进制接口)，一段程序只有遵循这个接口规范才能在该 CPU 上运行，所以同样的程序代码为了兼容多个不同的CPU，需要为不同的 ABI 构建不同的库文件。当然对于CPU来说，不同的架构并不意味着一定互不兼容。

- armeabi设备只兼容armeabi；
- armeabi-v7a设备兼容armeabi-v7a、armeabi；
- arm64-v8a设备兼容arm64-v8a、armeabi-v7a、armeabi；
- X86设备兼容X86、armeabi；
- X86_64设备兼容X86_64、X86、armeabi；
- mips64设备兼容mips64、mips；
- mips只兼容mips；

### 6、NDK与JNI的关系
![1](/assets/image/2018-01-18-Android Studio JNI Exercise 1.png)  

参考[NDK与JNI的关系](http://blog.csdn.net/carson_ho/article/details/73250163)

-------------------

## 三、环境安装（Windows）
如下图
![2](/assets/image/2018-01-18-Android Studio JNI Exercise 2.png)  


## 四、项目结构
1、新建项目，勾选Include C++ support
![3](/assets/image/2018-01-18-Android Studio JNI Exercise 3.png)  

- C++ Standard是让我们选择C++标准
- Exceptions Support是添加C++中对于异常的处理，如选中，Android Studio会 
将 -fexceptions标志添加到模块级build.gradle文件的cppFlags中，Gradle会将其传递到CMake
- Runtime Type Information Support是启用支持RTTI，即运行时类型信息，是多态的主要组成部分，
通过运行时(runtime)确定使用的类型，执行不同的函数，复用(reuse)接口.
dynamic_cast<>可以使基类指针转换为派生类的指针，通过判断指针的类型，可以决定使用的函数。
typeid()，可以判断类型信息，判断指针指向位置，在多态中，可以判断基类还是派生类。这段意思还不是很理解，等后面学习C++时再继续。

-------------------

感谢你阅读这份教程

-------------------