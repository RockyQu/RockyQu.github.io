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

```
<?xml version="1.0"?>
<template
    format="5"
    revision="1" 
    name="MVPFrames" 
    minApi="9"
    minBuildApi="15"
    description="快速一键生成 MVP 模板，适用于 MVPFrames 开发框架">

    <!-- 主分类 -->
    <category value="Frames" />
    <!-- 设备 -->
    <formfactor value="Mobile" />

    <!-- 模板类型 -->
    <parameter
        id="templateType"
        name="Template Type"
        type="enum"
        default="templateTypeActivity"
        help="模板类型 Activity、Fragment" >
        <option id="templateTypeActivity">Activity</option>
        <option id="templateTypeFragment">Fragment</option>
    </parameter>

    <!-- 模块名称 -->
    <parameter
        id="moduleName"
        name="Module Name"
        type="string"
        constraints="class|unique|nonempty"
        help="模块名称 例：UserActivity 填写 User 即可" />

    <!-- 模块子目录名称 -->
    <parameter
        id="moduleFolder"
        type="string"
        suggest="${classToResource(moduleName)}"
        visibility="false" />

    <!-- 是否创建布局 -->
    <parameter
        id="generateLayout"
        name="Generate Layout File"
        type="boolean"
        default="true"
        help="此选项将创建一个对应的布局文件" />

    <!-- 布局文件名称，如果选择是才会显示出来 -->
    <parameter
        id="layoutName"
        name="Layout Name"
        type="string"
        constraints="layout|unique|nonempty"
        suggest="_${classToResource(moduleName)}"
        visibility="generateLayout"
        help="创建一个与模块对应的布局文件" />

    <!-- 引入 R 文件时所用的包名 -->
    <parameter
        id="package"
        name="Package Name"
        type="string"
        default="com." 
        help="引入 R 文件，以及创建 Entity 时所用的包名，例：com.mycompany.myapp"/>

    <!-- Presenter -->
    <parameter
        id="presenterName"
        name="Presenter Name"
        type="string"
        suggest="${moduleName}Presenter"
        help="Presenter 名称"/>

    <!-- Repository -->
    <parameter
        id="repositoryName"
        name="Repository Name"
        type="string"
        suggest="${moduleName}Repository"
        help="Repository 名称"/>

    <!-- 是否创建 Entity -->
    <parameter
        id="isEntity"
        name="Generate Entity File"
        type="boolean"
        default="false"
        help="此选项将创建一个对应的 Entity 文件" />

    <!-- Entity -->
    <parameter
        id="entityName"
        name="Entity Name"
        type="string"
        visibility="isEntity"
        suggest="${moduleName}"
        help="Entity 名称"/>

    <parameter
        id="packageName"
        name="Directory"
        type="string"
        constraints="package"
        default="com.mycompany.myapp" />

    <globals file="globals.xml.ftl" />
    <execute file="recipe.xml.ftl" />

</template>
```

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