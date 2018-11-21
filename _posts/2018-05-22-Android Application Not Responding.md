---
layout: post
title: "Android 开发中导致 ANR 的一些注意事项小结"
excerpt: "总结 ANR 类型、如何导致的 ANR、如何避免、解决办法"
date: 2018-5-22
categories:
  - Android
tags:
  - ANR
---

## 0x00 触发 ANR 条件
> ANR 全称 “Application Not Responding”，即“应用程序无响应“。系统会向用户显示一个对话框，用户可以选择“等待”而让程序继续运行，或“强制关闭”。App 的响应能力是由 Activity Manager 和 Window Manager 系统服务来监控的

* 输入事件5s内没被处理（键盘输入, 触摸屏幕）
* BroadcastReceiver 在规定时间内没处理完(前台广播为10s，后台广播为60s)
* Service (前台服务为10s，后台服务为60s)

-------------------

## 0x01 导出 ANR 文件
> 发生 ANR 后，系统会收集 ANR trace 日志信息，日志文件会被保存到 /data/anr/traces.txt 中，你可以使用 adb 提取此文件

* 检查是否已经配置 adb，在 Android Studio Terminal 窗口输入 adb 回车，未配置会显示如下信息
![1](/assets/image/2018-05-22/2018-05-22-Android Application Not Responding 1.png)

*  配置 adb，新建一个名称为 Android 的系统变量，变量值写入你自己 SDK
![2](/assets/image/2018-05-22/2018-05-22-Android Application Not Responding 2.png)

* 在系统环境变量 Path 下添加 %Android%
![3](/assets/image/2018-05-22/2018-05-22-Android Application Not Responding 3.png)

* 重启 AS 再次在 Terminal 窗口输入 adb 查看，成功后如下图
![4](/assets/image/2018-05-22/2018-05-22-Android Application Not Responding 4.png)

* 查看是否存在 traces.txt 文件
![7](/assets/image/2018-05-22/2018-05-22-Android Application Not Responding 7.png)

* 导出 traces.txt 文件，如果设备没有 root 需使用如下命令，先复制到 SDCard，再导出就可以了
![5](/assets/image/2018-05-22/2018-05-22-Android Application Not Responding 5.png)

-------------------

## 0x02 分析 ANR 问题
具体问题需要具体分析，这个例子是在主线程中强行 Sleep 的 ANR 日志，如下图，会找到类似 java.lang.Thread.sleep 的信息
![6](/assets/image/2018-05-22/2018-05-22-Android Application Not Responding 6.png)

-------------------

## 0x03 降低 ANR 概率 [参考](https://www.jianshu.com/p/fa962a5fd939)
* 主线程读取数据：母庸质疑在主线程中不能读取网络数据，但系统是允许主线程从数据库或者其他地方获取数据，但这种操作 ANR 风险很大，也会造成掉帧等，影响用户体验  
  - 避免在主线程进行数据库操作，由其是大量比较耗时的查询等操作，这个我有亲身经历，比如在进入 Activity 的 onCreate 里面读取较大的文件会明显感觉到卡顿，所以请放到异步里执行
  - 慎用 SharedPreference ，避免存储超大的 Value [详细讲解](http://weishu.me/2016/10/13/sharedpreference-advices/)
* 不要在 BroadcastReciever 的 onRecieve() 方法中干过多的活，尤其应用在后台的时候。一种解决方案是直接开的异步线程执行，但此时应用可能在后台，系统优先级较低，进程很容易被系统杀死，所以可以选择开个 IntentService 去执行相应操作，即使是后台 Service 也会提高进程优先级，降低被杀可能性
* 各个组件的生命周期函数都不应该有太耗时的操作，即使对于后台 Service 或者 ContentProvider 来讲，应用在后台运行时候其 onCreate()时候不会有用户输入引起事件无响应 ANR，但其执行时间过长也会引起 Service 的 ANR 和 ContentProvider 的 ANR
* 尽量避免主线程的被锁的情况，在一些同步的操作主线程有可能被锁，需要等待其他线程释放相应锁才能继续执行，这样会有一定的 ANR 风险，对于这种情况有时也可以用异步线程来执行相应的逻辑。如果主线程被死锁了基本就等于要发生 ANR 了