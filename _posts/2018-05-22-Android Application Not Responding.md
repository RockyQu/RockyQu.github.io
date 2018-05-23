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

## 0x0000 触发 ANR 条件
> ANR 全称 “Application Not Responding”，即“应用程序无响应“。系统会向用户显示一个对话框，用户可以选择“等待”而让程序继续运行，或“强制关闭”

* 输入事件(按键和触摸事件)5s内没被处理
* BroadcastReceiver 的事件(onRecieve方法)在规定时间内没处理完(前台广播为10s，后台广播为60s)
* Service 前台20s后台 200s未完成启动
* ContentProvider 的 publish 在10s内没进行完

> 注：后三种情况，以 BroadcastReviever 为例，在 onRecieve() 方法执行10秒内没发生第一种ANR(也就是在这个过程中没有输入事件或输入事件还没到5s)才会发生Receiver timeout，否则将先发生事件无相应ANR，所以onRecieve()是有可能执行不到10s就发生ANR的，所以不要在onRecieve()方法里面干活，service的onCreate()和ContentProvider的onCreate()也一样，他们都是主线程的，不要在这些方法里干活，这个会在本文最后再细说。


-------------------





-------------------
