---
layout: post
title: "Android 5.0 以下版本出现 java.lang.NoClassDefFoundError"
excerpt: "记一次版本适配遇到的问题"
date: 2018-5-11
categories:
  - MultiDex
tags:
  - NoClassDefFoundError
  - MultiDex
---

## 0x0000 前言
> 版本兼容性测试发现在 Android 5.0 以下出现 NoClassDefFoundError，这个错误一般是由于超出 64K 方法造成，此时需要做分包处理，常用方法是引入官方的 MultiDex 解决，但是当你引入此工具后，问题未解决依然报类似的错误，此时就要用到分包的高级操作，把某些报 NoClassDefFoundError 的类手动加入到主包下，请看如下教程



-------------------





-------------------
