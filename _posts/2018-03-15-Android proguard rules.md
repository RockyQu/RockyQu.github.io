---
layout: post
title: "Android Studio 代码基本混淆"
excerpt: "Android 基础代码混淆教程整理"
date: 2018-3-15
categories:
  - 混淆
tags:
  - 混淆
  - 代码混淆
---

## 一、综述
> 在你完成项目打包发布之前，很有必要加上代码混淆来避免一些用户恶意对你的 APK 进行反编译，通过反编译非加密的 dex 文件就可以看到源码，甚至用 Android Studio Analyze APK 就可以分析源代码。如果没有特殊原因，所有 APP 都应该开启混淆。最近也是有项目需要加混淆，所以整理一个系列教程做为备份与日后学习。
  
增加混淆的必要性总结
* 加密代码、资源文件，增加逆向工程的难度  
* 可以自动移除未被使用的类、方法、属性，可以在一定程度上避免64K方法数  
* 减少 APK 体积，也是 APK 瘦身的一种方法

混淆的原理 [参考](https://mp.weixin.qq.com/s/WmJyiA3fDNriw5qXuoA9MA)  
Java 是一种跨平台、解释型语言，Java 源代码编译成的 class 文件中有大量包含语义的变量名、方法名的信息，很容易被反编译为 Java 源代码。为了防止这种现象，我们可以对 Java 字节码进行混淆。混淆不仅能将代码中的类名、字段、方法名变为无意义的名称，保护代码，也由于移除无用的类、方法，并使用简短名称对类、字段、方法进行重命名缩小了程序的size。
ProGuard由shrink、optimize、obfuscate 和 preverify 四个步骤组成，每个步骤都是可选的，需要哪些步骤都可以在脚本中配置。
* 压缩(Shrink): 侦测并移除代码中无用的类、字段、方法、和特性(Attribute)。  
* 优化(Optimize): 分析和优化字节码。  
* 混淆(Obfuscate): 使用a、b、c、d这样简短而无意义的名称，对类、字段和方法进行重命名。  
上面三个步骤使代码size更小，更高效，也更难被逆向工程。  
* 预检(Preveirfy):  在 Java 平台上对处理后的代码进行预检。

## 二、代码混淆基本配置
> 在主工程目录，找到 proguard-rules.pro 文件，它是你要编写混淆配置的文件，在这个文件中编写混淆规则。如下图，先添加这两个参数配置是否开启混淆 minifyEnabled 和混淆文件位置 proguardFiles 两行代码

![1](/assets/image/2018-03-15-Android proguard rules 1.png)  

* proguard-android.txt 这个文件是系统默认混淆文件一般不需要做修改
* 在 debug 版下也可以开启混淆做为测试
* Gradle 2.2 之后，defaultProguardFile 没有使用 SDK 目录下的 proguard-android.txt，而是使用了 gradle 自带的 proguard-android.txt，不同的 gradle 版本带有不同的默认混淆文件，比如在项目根目录的 build/intermediates/proguard-files/proguard-android.txt-2.3.3，即为 gradle 自带的混淆文件。在 proguard-android.txt-2.3.3 文件中也写有说明，Gradle 2.2 之后自带混淆文件  [参考](https://mp.weixin.qq.com/s/WmJyiA3fDNriw5qXuoA9MA)  

## 三、混淆规则
### 1、哪些是不需要混淆的？
* Android 四大组件
* native方法
* Java 反射用到的类
* 自定义控件
* 枚举类
* JavaBean
* Parcelable、Serializable 序列化类
* WebView 与 JS 交互所用到的类和方法

### 2、基本混淆指令

```
# 代码混淆压缩比，在0~7之间
-optimizationpasses 5

# 混合时不使用大小写混合，混合后的类名为小写
-dontusemixedcaseclassnames

# 指定不忽略非公共库的类和类成员
-dontskipnonpubliclibraryclasses
-dontskipnonpubliclibraryclassmembers

# 这句话能够使我们的项目混淆后产生映射文件
# 包含有类名->混淆后类名的映射关系
-verbose

# 不做预校验，preverify是proguard的四个步骤之一，Android不需要preverify，去掉这一步能够加快混淆速度
-dontpreverify

# 保留Annotation不混淆
-keepattributes *Annotation*,InnerClasses

# 避免混淆泛型
-keepattributes Signature

# 抛出异常时保留代码行号
-keepattributes SourceFile,LineNumberTable

# 指定混淆是采用的算法，后面的参数是一个过滤器
# 这个过滤器是 Google 推荐的算法，一般不做修改
-optimizations !code/simplification/arithmetic,!code/simplification/cast,!field/*,!class/merging/*
```

### 3、混淆日志

```
# APK 包内所有 class 的内部结构
-dump proguard/classstructure.txt
# 未混淆的类和成员
-printseeds proguard/unconfused.txt
# 列出从 APK 中删除的代码
-printusage proguard/deletecode.txt
# 混淆前后的映射
-printmapping proguard/confusedmapping.txt
```

### 4、Android 开发不需要混淆的部份

```
# Android 四大组件相关
-keep public class * extends android.app.Activity
-keep public class * extends android.app.Appliction
-keep public class * extends android.app.Service
-keep public class * extends android.content.BroadcastReceiver
-keep public class * extends android.content.ContentProvider
-keep public class * extends android.app.backup.BackupAgentHelper
-keep public class * extends android.preference.Preference
-keep public class * extends android.view.View
-keep public class com.android.vending.licensing.ILicensingService

# 保留support下的所有类及其内部类
-keep class android.support.** {*;}

# 保留 R 下面的资源
-keep class **.R$* {*;}

# 保留本地 native 方法不被混淆
-keepclasseswithmembernames class * {
    native <methods>;
}

# 保留在 Activity 中的方法参数是 vie w的方法，
# 这样以来我们在 layout 中写的 onClick 就不会被影响
-keepclassmembers class * extends android.app.Activity{
    public void *(android.view.View);
}

# 保留枚举类不被混淆
-keepclassmembers enum * {
    public static **[] values();
    public static ** valueOf(java.lang.String);
}

# 保留自定义控件（继承自View）不被混淆
-keep public class * extends android.view.View{
    *** get*();
    void set*(***);
    public <init>(android.content.Context);
    public <init>(android.content.Context, android.util.AttributeSet);
    public <init>(android.content.Context, android.util.AttributeSet, int);
}

# 保留 Parcelable 序列化类不被混淆
-keep class * implements android.os.Parcelable {
  public static final android.os.Parcelable$Creator *;
}

# 保留 Serializable 序列化的类不被混淆
-keepclassmembers class * implements java.io.Serializable {
    static final long serialVersionUID;
    private static final java.io.ObjectStreamField[] serialPersistentFields;
    private void writeObject(java.io.ObjectOutputStream);
    private void readObject(java.io.ObjectInputStream);
    java.lang.Object writeReplace();
    java.lang.Object readResolve();
}

# 对于带有回调函数的 onXXEvent 的，不能被混淆
-keepclassmembers class * {
    void *(**On*Event);
}

# WebView，没有使用 WebView 请注释掉
-keepclassmembers class fqcn.of.javascript.interface.for.webview {
   public *;
}
-keepclassmembers class * extends android.webkit.webViewClient {
    public void *(android.webkit.WebView, java.lang.String, android.graphics.Bitmap);
    public boolean *(android.webkit.WebView, java.lang.String);
}
-keepclassmembers class * extends android.webkit.webViewClient {
    public void *(android.webkit.webView, jav.lang.String);
}
```

### 5、自定义混淆规则
1) JavaBean 实体类不能混淆，一般会将实体类统一放到一个包下，keep public class 后面请改成你自己项目的路径

```
-keep public class com.ljd.example.entity.** {
    public void set*(***);
    public *** get*();
    public *** is*();
}
```





-------------------

## 总结


-------------------