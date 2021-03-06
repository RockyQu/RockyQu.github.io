---
layout: post
title: "基于 Android Studio Template 开发一款 MVP 代码自动生成模板"
excerpt: "适用于 MVPFrames 开发框架的代码自动生成模板"
date: 2018-3-10
categories:
  - Template
tags:
  - Android Studio Template
  - Template
  - 模板
---

-------------------

## 0x00 概述
> Android Studio Template 是基于 [FreeMarker](https://baike.baidu.com/item/freemarker/9489366?fr=aladdin) 来开发的，Android Studio 已经预设一些常用 Template，如下图，这些模板包括图片、XML、类都可以用自定义形式方便添加各种效果，减少了模板式代码编写，提高工作效率

![1](/assets/image/2018-03-10/2018-03-10-Android Studio Template 1.png)  

这些模板文件被存放在 Android Studio 安装目录 \plugins\android\lib\templates 文件夹下，本教程以 Activity Template 为核心来讲解

![2](/assets/image/2018-03-10/2018-03-10-Android Studio Template 2.png)  

-------------------

## 0x01 模板的创建与制作  
### 1、模板的创建
创建模板很简单，你可以直接复制一份已经存在的模板，在 activities 目录下有个叫 EmptyActivity 的模板，里面只有一个类，是最简单的模板，可以以此模板做为基础

![4](/assets/image/2018-03-10/2018-03-10-Android Studio Template 4.png)

### 2、模板的目录结构
![3](/assets/image/2018-03-10/2018-03-10-Android Studio Template 3.png)  

### 3、template.xml
页面文件，模板配置文件，这是我自己已经写好的模板代码如下，注释已非常详细

```
<?xml version="1.0"?>
<template
    format="5"
    revision="1" // 版本号，当你对已经发布的模板进行更新时版本号应该有所变化
    name="MVPFrames" // 模板显示的名称
    minApi="9" // 模板所需的最小API值，IDE将确保在实例化模板之前，目标工程的 minSdkVersion 不低于这个值
    minBuildApi="15" // 模板所需的最小编译API，IDE将确保在实例化模板之前，项目工程的 API 等级大于或等于这个值
    description="快速一键生成 MVP 模板，适用于 MVPFrames 开发框架"> // 模板描述信息

    <category value="Frames" /> // 模板类型，用于在菜单栏 File-New 下显示
    <formfactor value="Mobile" /> // 如同我们在创建 module 时所显示的类型

    <!-- 模板类型 -->
    <parameter
        id="templateType" // 唯一Id，在ftl文件中可以用 ${templateType} 获取参数值
        name="Template Type" // 创建模板时在文本框左边或上边显示的该文本框名称
        type="enum" // 类型，enum 代表是一个下拉选择框
        default="templateTypeActivity" // 默认值
        help="模板类型 Activity、Fragment" > // 显示在窗口最下面的帮助提示信息
        <option id="templateTypeActivity">Activity</option> // 选项
        <option id="templateTypeFragment">Fragment</option>
    </parameter>

    <!-- 模块名称 -->
    <parameter
        id="moduleName"
        name="Module Name"
        type="string"
        constraints="class|unique|nonempty"  // 约束类型，可用 | 符号联合使用
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
        suggest="_${classToResource(moduleName)}" // 自动提示信息，classToResource 在下面会介绍
        visibility="generateLayout" // 是否显示这个控件
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

    <globals file="globals.xml.ftl" /> // 定义全局变量和引入一些共通的包
    <execute file="recipe.xml.ftl" /> // 开始执行生成模板

</template>
```

关于 suggest 字段  
* suggest="_${classToResource(moduleName)}"  
classToResource 的作用是 moduleName 输入 User 的值 转换为 user。    
* suggest="_${underscoreToCamelCase(moduleName)}"  
underscoreToCamelCase 的作用和 classToResource 正好相反，当 moduleName 输入 user 的值会转换为 User 即驼峰命名方法。

### 3、recipe.xml.ftl
涉及的几个重要参数  
* copy：复制，将 from 中的文件复制到 to 路径下。  
* merge：合并，将 from 中的文件合并到 to 路径下的文件中。  
* instantiate：和 copy 类似，也是将from中的文件复制到to路径下，它会将 ftl 文件中的类似为 ${moduleName} 会转换为我们需要的类名。  
* open：模板代码生成后，打开 file 中指定的文件。  
  
示例代码
```
<instantiate from="root/src/app_package/BaseSimpleActivity.java.ftl"
                   to="${escapeXmlAttribute(srcOut)}/${moduleName}Activity.java" />
<open file="${escapeXmlAttribute(srcOut)}/${moduleName}Activity.java" />
```

关于 escapeXmlAttribute 字段  
* ${escapeXmlAttribute(srcOut)} 当前包名路径  
* ${escapeXmlAttribute(resOut)} res 根目录  

### 4、globals.xml.ftl
添加全局常量用的，这里我没有用到，只是引入了一个共通的文件
```
<?xml version="1.0"?>
<globals>
    <#include "../common/common_globals.xml.ftl" />
</globals>
```

### 5、root 文件夹
存放后缀为 ftl 的 java 文件和 XML 文件用来生成最终的模板文件

-------------------

## 0x02 使用模板
模板制作完成后，将整个文件夹放到 activities 文件夹下
![2](/assets/image/2018-03-10/2018-03-10-Android Studio Template 2.png)  

重启 Android Studio，即可看到模板了
![6](/assets/image/2018-03-10/2018-03-10-Android Studio Template 6.png)  

-------------------

## 0x03 总结
已上模板代码已全部开源到 [GitHub](https://github.com/RockyQu/FramesTemplate) 欢迎共同学习交流(゜▽゜)つロ