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
> 版本兼容性测试发现在 Android 5.0 以下出现 NoClassDefFoundError，这个错误一般是由于超出 64K 方法造成，此时需要做分包处理，常用方法是引入官方的 MultiDex 即可解决大部份，以下为基础步骤

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

以上方法基本能解决大部分问题，但是在 Android 5.0 以下系统版本中依然可能出现类似 NoClassDefFoundError 的错误，那么请往下看

-------------------

## 0x0001 64K 的由来
> Android 应用 (APK) 文件包含 Dalvik Executable (DEX) 文件形式的可执行字节码文件，其中包含用来运行您的应用的已编译代码。Android 系统安装一个应用的时候，有一步是对 DEX 进行优化，这个过程有一个专门的工具来处理，叫 DexOpt。DexOpt 的执行过程是在第一次加载 DEX 文件的时候执行的。这个过程会生成一个 OptimisedDex 文件。执行 OptimisedDex 的效率会比直接执行 DEX 文件的效率要高很多。但是在早期的 Android 系统中，即 Android 5.0 以下的版本，DexOpt 会把每一个类的方法 id 检索起来，存在一个链表结构里面。但是这个链表的长度是用一个 short 类型来保存的，导致了方法 id 的数目不能够超过 65536 个。当一个项目足够大的时候，显然这个方法数的上限是不够的。尽管在新版本的 Android 系统中，修复了这个问题，但是我们仍然需要对低版本的 Android 系统做兼容。
为了解决方法数超限的问题，需要将该 DEX 文件拆成两个或多个，为此谷歌官方推出了 MultiDex 兼容包，配合 AndroidStudio 实现了一个 APK 包含多个 DEX 的功能。

* Android 5.0 之前版本的 Dalvik 可执行文件分包支持
Android 5.0（API 级别 21）之前的平台版本使用 Dalvik 运行时来执行应用代码。默认情况下，Dalvik 限制应用的每个 APK 只能使用单个 classes.dex 字节码文件。要想绕过这一限制，您可以使用 Dalvik 可执行文件分包支持库，它会成为您的应用主要 DEX 文件的一部分，然后管理对其他 DEX 文件及其所包含代码的访问。
> 注：如果您的项目配置时所面向的 Dalvik 可执行文件分包使用的是 minSdkVersion 20 或更低版本，并且您将其部署到运行 Android 4.4（API 级别 20）或更低版本的目标设备上，则 Android Studio 会停用 Instant Run。

* Android 5.0 及更高版本的 Dalvik 可执行文件分包支持
Android 5.0（API 级别 21）及更高版本使用名为 ART 的运行时，后者原生支持从 APK 文件加载多个 DEX 文件。ART 在应用安装时执行预编译，扫描 classesN.dex 文件，并将它们编译成单个 .oat 文件，供 Android 设备执行。因此，如果您的 minSdkVersion 为 21 或更高值，则不需要 Dalvik 可执行文件分包支持库。
> 注：如果将应用的 minSdkVersion 设置为 21 或更高值，使用 Instant Run 时，Android Studio 会自动将应用配置为进行 Dalvik 可执行文件分包。由于 Instant Run 仅适用于调试版本的应用，您仍需配置发布构建进行 Dalvik 可执行文件分包，以规避 64K 限制。

## 0x0002 声明主 DEX 文件中需要的类
正常情况是不应出现此类情况的，因为构建工具能识别这些代码路径，但可能在代码路径可见性较低（如使用的库具有复杂的依赖项）时出现。因此，可以使用 multiDexKeepFile 或 multiDexKeepProguard 属性声明它们，以手动将这些其他类指定为主 DEX 文件中的必需项。如果类在 multiDexKeepFile 或 multiDexKeepProguard 文件中匹配，则该类会添加至主 DEX 文件。[参考](https://blog.csdn.net/changsimeng/article/details/70946156)

* multiDexKeepFile
请在主 APP 目录下创建一个名为 multidex-config.txt 的文件，在 multiDexKeepFile 中指定的这个件，该文件应该每行包含一个类，例:com/example/MyClass.class
```
android {
    buildTypes {
        release {
            multiDexKeepFile file 'multidex-config.txt'
            ...
        }
    }
}
```

* multiDexKeepProguard
请在主 APP 目录下创建一个名为 multidex-config.pro 的文件，在 multiDexKeepProguard 中指定的这个件，该文件的语法与 Proguard 相同，例:-keep com.example.MyClass.class
```
android {
    buildTypes {
        release {
            multiDexKeepProguard 'multidex-config.pro'
            ...
        }
    }
}
```

> 注：Gradle 读取相对于 build.gradle 文件的路径，因此 multidex-config.txt、multidex-config.pro 与 build.gradle 文件应在同一目录中

