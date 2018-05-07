---
layout: post
title: "使用 Ubuntu 编译 ijkplayer 源码"
excerpt: "使用 Ubuntu 编译 ijkplayer 源码，并可以在 Android Studio 进行二次开发"
date: 2018-4-18
categories:
  - ijkplayer
tags:
  - ijkplayer
---

## 0x0000 安装 Ubuntu
我用的是 Oracle VM VirtualBox [下载地址](https://www.virtualbox.org/) 虚拟机来安装 Ubuntu [下载地址](https://www.ubuntu.com/download)，不会对已安装的系统造成什么影响。

- 坑，安装完成 VM 之后新建虚拟机，报错：不能为虚拟电脑 XXX 打开一个新任务，此时需要通过安装 “Oracle VM VirtualBox Extension Pack” 扩展包解决此问题


### 1、作用与意义 
首先必须要了解一下 Java 线程机制的概念，一个任务在线程的 run 方法中执行，当 run 方法结束，线程也就终止了。这种机制的问题在于，线程对系统的开销较大，由于任务的数量多、密度大、要求快速响应 UI 的更新任务，那么这种方式的效率就很低下，所以 Android 引出 Handler 机制来解决多线程通信的问题。Handler 线程永不退出，在 run 方法中轮询任务，有任务就处理，没任务就等待，在 Android 中多数使用在非主线程（非UI线程）作耗时操作，在主线程中做更新 UI 操作，这就是 Handler 的作用与意义。 
最核心的思想一句话就是多线程通信，解决多线程并发问题

### 2、流程图
Handler 消息处理机制的流程图，请结合下面的《二》源码分析一起看这张图，不断的反复理解

![1](/assets/image/2018-03-15-Android proguard rules 1.png)  

-------------------

## 二、源码分析
> 下面是 Android Open Source 中的 Handler 源码，去掉了一些注释和非核心代码

### 1、Message
Message 定义消息相关的描述和属性信息

https://github.com/Bilibili/ijkplayer

-------------------




## 总结


-------------------

## 相关链接


-------------------