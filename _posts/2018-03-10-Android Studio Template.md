---
layout: post
title: "基于 Android Studio Template 开发一款 MVP 代码自动生成模板"
excerpt: "适用于 MVPFrames 框架的代码自动生成模板"
date: 2018-3-10
categories:
  - Template
tags:
  - Android Studio Template
  - Template
  - 模板
  - 插件
---

## 一、概述
> Android Studio Template 是基于 [FreeMarker](https://baike.baidu.com/item/freemarker/9489366?fr=aladdin) 来开发的，Android Studio 已经预设一些常用 Template，如下图，这些模板包括图片、XML、类都可以用自定义形式方便添加各种效果，减少了模板式代码编写，提高工作效率

![1](/assets/image/2018-03-10-Android Studio Template 1.png)  

这些模板文件被存放在 Android Studio 安装目录 \plugins\android\lib\templates 文件夹下，本教程以 Activity Template 为核心来讲解

![2](/assets/image/2018-03-10-Android Studio Template 2.png)  

-------------------

## 二、模板的创建与制作  
> 模板的创建很简单，你可以直接复制一份已经存在的模板，在 activities 目录下有个叫 EmptyActivity 的模板，里面只有一个类，是最简单的模板
![3](/assets/image/2018-03-10-Android Studio Template 3.png)  



![1](/assets/image/2018-03-10-IntelliJ IDEA Development basis 1.png)  



-------------------

## 三、配置 IntelliJ Platform Plugin SDK

选择右下角 Configure | Project Defaults | Project Structure 打开配置窗口
![2](/assets/image/2018-03-10-IntelliJ IDEA Development basis 2.png)  

选择 IntelliJ Platform Plugin SDK
![4](/assets/image/2018-03-10-IntelliJ IDEA Development basis 4.png)  

选择默认的 IntelliJ IDEA 文件夹即可配置完毕
![3](/assets/image/2018-03-10-IntelliJ IDEA Development basis 3.png)  

-------------------

## 四、 初始化项目

![6](/assets/image/2018-03-10-IntelliJ IDEA Development basis 6.png)  

![7](/assets/image/2018-03-10-IntelliJ IDEA Development basis 7.png)  

![8](/assets/image/2018-03-10-IntelliJ IDEA Development basis 8.png)  

-------------------

## 五、 项目结构
> src 目录存放的插件对应的 Java 源码，resources/META-INF/plugin.xml 配置 Action 的文件，可以看成是插件的配置文件

![9](/assets/image/2018-03-10-IntelliJ IDEA Development basis 9.png) 

配置文件 plugin.xml，如下图
![10](/assets/image/2018-03-10-IntelliJ IDEA Development basis 10.png) 

-------------------

## 附录
[官方教程](http://www.jetbrains.org/intellij/sdk/docs/welcome.html)

-------------------