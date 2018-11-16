---
layout: post
title: "Android 使用 so 文件存储私密数据，并增加签名防盗机制"
excerpt: "使用 .so 文件来存储 APP 私密数据，并且增加签名防盗机制的一种加密方案"
date: 2018-11-18
categories:
  - Android NDK
tags:
  - AndroidStudio NDK
  - NDK
  - JNI
---

## 0x0000 实际项目中引出的一些需求问题
有时你需要在客户端存放一些保密的数据，比如某些授权 Key ，如果直接写在 Java 中，会很容易被反编译看到，那么我们可以把这些数据存在 so 文件中，来增加反编译难度，顺便增加 APP 签名防盗机制来防止别人盗用 so 文件。

-------------------

## 0x0001 
NDK 配置看这里 → [Android Studio NDK 开发安装配置](https://rockycoder.cn/android%20ndk/2018/01/18/Android-Studio-JNI-Exercise.html)




-------------------