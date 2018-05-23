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
> ANR 全称 “Application Not Responding”，即“应用程序无响应“。系统会向用户显示一个对话框，用户可以选择“等待”而让程序继续运行，或“强制关闭”。App 的响应能力是由 Activity Manager 和 Window Manager 系统服务来监控的

* 输入事件5s内没被处理（键盘输入, 触摸屏幕）
* BroadcastReceiver 在规定时间内没处理完(前台广播为10s，后台广播为60s)
* Service (前台服务为10s，后台服务为60s)

-------------------

## 0x0001 导出 ANR 文件
> 发生 ANR 后，系统会收集 ANR trace 日志信息，日志文件会被保存到 /data/anr/traces.txt 中，你可以使用 adb 提取此文件

* 检查是否已经配置 adb，在 Android Studio Terminal 窗口输入 adb 回车，未配置显示如下信息
![1](/assets/image/2018-05-22-Android Application Not Responding 1.png)

*  配置 adb，新建一个名称为 Android 的系统变量，变量值写入你自己 SDK
![2](/assets/image/2018-05-22-Android Application Not Responding 2.png)

* 在系统环境变量 Path 下添加 %Android%
![3](/assets/image/2018-05-22-Android Application Not Responding 3.png)

* 重启 AS 再次在 Terminal 窗口输入 adb 查看，成功后如下图
![4](/assets/image/2018-05-22-Android Application Not Responding 4.png)

### 导出 traces.txt 文件，如果设备没有 root 需使用如下命令，先复制到 SDCard，再导出就可以了
![5](/assets/image/2018-05-22-Android Application Not Responding 5.png)

-------------------

## 0x0001 分析 ANR 问题
具体问题需要具体分析，这个例子是在主线程中强行Sleep的ANR日志
![6](/assets/image/2018-05-22-Android Application Not Responding 6.png)