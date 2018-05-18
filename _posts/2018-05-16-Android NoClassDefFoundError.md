---
layout: post
title: "Android 5.0 以下版本出现 java.lang.NoClassDefFoundError"
excerpt: "记一次由 NoClassDefFoundError 引出的 MultiDex 分包问题"
date: 2018-5-16
categories:
  - MultiDex
tags:
  - NoClassDefFoundError
  - MultiDex
---

## 0x0000 前言
> 版本兼容性测试发现在 Android 5.0 以下出现 NoClassDefFoundError，这个错误一般是由于超出 65K 方法造成，此时需要做分包处理，常用方法是引入官方的 MultiDex 即可解决大部份，以下为基础步骤

### 引入 MultiDex 步骤
* 引入依赖库
```
compile 'com.android.support:multidex:1.0.1'
```

* App build.gradle 开启 Multidex 开关
```
multiDexEnabled true
```

* 重写 Application.attachBaseContext() 方法加入
```
MultiDex.install(this);
```

以上方法基本能解决大部分问题，但是在 Android 5.0 以下依然可能出现类似 NoClassDefFoundError 的错误

-------------------



，此时就要手动分包把某些报 NoClassDefFoundError 的类加入到主 Dex 包下

-------------------