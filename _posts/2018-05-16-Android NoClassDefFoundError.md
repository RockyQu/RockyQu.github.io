---
layout: post
title: "Android 5.0 以下版本出现 java.lang.NoClassDefFoundError"
excerpt: "记一次由 NoClassDefFoundError 引出的 MultiDex 分包问题解析"
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

以上方法基本能解决大部分问题，但是在 Android 5.0 以下依然可能出现类似 NoClassDefFoundError 的错误，那么请往下看

-------------------

## 0x0001 65K 的由来
> 当 Android 系统安装一个应用的时候，有一步是对 Dex 进行优化，这个过程有一个专门的工具来处理，叫 DexOpt。DexOpt 的执行过程是在第一次加载 Dex 文件的时候执行的。这个过程会生成一个 OptimisedDex 文件。执行 OptimisedDex 的效率会比直接执行 Dex 文件的效率要高很多。但是在早期的 Android 系统中，即 Android 5.0 以下的版本，DexOpt 会把每一个类的方法 id 检索起来，存在一个链表结构里面。但是这个链表的长度是用一个 short 类型来保存的，导致了方法 id 的数目不能够超过 65536 个。当一个项目足够大的时候，显然这个方法数的上限是不够的。尽管在新版本的 Android 系统中，DexOpt 修复了这个问题，但是我们仍然需要对低版本的 Android 系统做兼容。
为了解决方法数超限的问题，需要将该 Dex 文件拆成两个或多个，为此谷歌官方推出了 MultiDex 兼容包，配合 AndroidStudio 实现了一个 APK 包含多个 Dex 的功能。

## 0x0002 手动分包主 DEX 文件中需要的类
> 正常这种在已经引入 MultiDex 的情况下依然出来该问题是不应该的，因为构建工具能识别这些代码路径，


https://blog.csdn.net/changsimeng/article/details/70946156
https://www.jianshu.com/p/48523f05c569
，此时就要手动分包把某些报 NoClassDefFoundError 的类加入到主 Dex 包下

-------------------