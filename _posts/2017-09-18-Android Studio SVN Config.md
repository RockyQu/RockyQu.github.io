---
layout: post
title: "关于 Android Studio 使用 SVN 的填坑记录"
excerpt: "Android Studio 使用 SVN 的曾经遇到的问题记录"
date: 2017-9-18
categories:
  - Android Studio
tags:
  - Android Studio
  - SVN
  - TortoiseSVN
---

## 0x0000 安装SVN

当安装TortoiseSVN时，command line client tools 这项默认是不会安装的，必须手动选择安装上，否则svn.exe文件不存在，无法在 Android Studio 中进行SVN关联配置

![img1](/assets/image/2017-09-18/2017-09-18-Android Studio SVN Config 1.png)  

![img2](/assets/image/2017-09-18/2017-09-18-Android Studio SVN Config 2.png)  

![img3](/assets/image/2017-09-18/2017-09-18-Android Studio SVN Config 3.png)  

-------------------

## 0x0001 关联SVN  

![img4](/assets/image/2017-09-18/2017-09-18-Android Studio SVN Config 4.png)  

-------------------

## 0x0002 忽略文件

![img5](/assets/image/2017-09-18/2017-09-18-Android Studio SVN Config 5.png) 

![img6](/assets/image/2017-09-18/2017-09-18-Android Studio SVN Config 6.png) 

![img7](/assets/image/2017-09-18/2017-09-18-Android Studio SVN Config 7.png) 

> 配置忽略文件必须在Share到SVN之前进行，忽略文件只需初次Share Project之前配置一次，其他人Check后不需要配置

-------------------

## 0x0003 附件
[clean-svn-folders](https://github.com/RockyQu/RockyQu.github.io/tree/master/assets/file) 批量删除项目下的 .svn 文件的脚本