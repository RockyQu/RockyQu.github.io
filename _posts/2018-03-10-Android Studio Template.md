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
### 1、模板的创建
创建模板很简单，你可以直接复制一份已经存在的模板，在 activities 目录下有个叫 EmptyActivity 的模板，里面只有一个类，是最简单的模板，可以以此模板做为基础

![4](/assets/image/2018-03-10-Android Studio Template 4.png)  

目录结构如下图
![3](/assets/image/2018-03-10-Android Studio Template 3.png)  

### 2、template.xml
页面文件，模板配置文件
![5](/assets/image/2018-03-10-Android Studio Template 4.png)  

### 3、recipe.xml.ftl
指定资源文件的路径并相应生成到我们的项目目录
![6](/assets/image/2018-03-10-Android Studio Template 4.png)  

### 4、globals.xml.ftl
定义一些全局变量，并引用了一个内置的通用 globals.xml.ftl
![7](/assets/image/2018-03-10-Android Studio Template 4.png)  

### 5、root 文件夹
里面存放后缀为ftl的java文件和XML文件
![8](/assets/image/2018-03-10-Android Studio Template 4.png)  

-------------------

## 三、使用模板


-------------------