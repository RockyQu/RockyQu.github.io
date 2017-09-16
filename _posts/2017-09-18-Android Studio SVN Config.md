---
layout: post
title: "关于 Android Studio 使用 SVN 的填坑记录"
date: 2017-9-18
categories:
  - Android Studio
tags:
  - Android Studio
  - SVN
  - TortoiseSVN
---

#### 关于 Android Studio 使用 SVN 的填坑记录
-------------------

### 安装SVN

当安装TortoiseSVN时，command line client tools 这项默认是不会安装的，必须手动选择安装上，否则svn.exe文件不存在，无法在 Android Studio 中进行SVN关联配置

![图一](/assets/image/2017-09-18-Android Studio SVN Config 1.png)  

![图二](/assets/image/2017-09-18-Android Studio SVN Config 2.png)  

![图三](/assets/image/2017-09-18-Android Studio SVN Config 3.png)  

### 关联SVN  

![图三](/assets/image/2017-09-18-Android Studio SVN Config 4.png)  

### 忽略文件


-------------------