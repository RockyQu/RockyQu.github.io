---
layout: post
title: "基于 IntelliJ IDEA 开发一款代码生成插件（入门基础）"
excerpt: "适用于 Android Studio 代码自动生成插件"
date: 2018-3-10
categories:
  - Git
tags:
  - IntelliJ IDEA
  - IntelliJ
  - IDEA
  - 插件
---

## 一、简介
> IDEA 全称 IntelliJ IDEA，是java语言开发的集成环境，IntelliJ在业界被公认为最好的java开发工具之一，尤其在智能代码助手、代码自动提示、重构、J2EE支持、各类版本工具(git、svn、github等)、JUnit、CVS整合、代码分析、 创新的GUI设计等方面的功能可以说是超常的。IDEA是JetBrains公司的产品，这家公司总部位于捷克共和国的首都布拉格，开发人员以严谨著称的东欧程序员为主。它的旗舰版本还支持HTML，CSS，PHP，MySQL，Python等。免费版只支持Java等少数语言。

[百度百科](https://baike.baidu.com/item/IntelliJ%20IDEA/9548353?fr=aladdin)

-------------------

## 二、下载安装

[下载地址](https://www.jetbrains.com/idea/download/#section=windows)

> 注意：Idea要基于jdk1.8，若本地没有1.8的环境会在此步骤多出勾选框，勾选Download and install JRE x86 by JetBrains（会自动下载安装JetBrains版的jre环境）

![1](/assets/image/2018-03-10-IntelliJ IDEA Development basis 1.png)  

## 三、配置 IntelliJ Platform Plugin SDK

选择右下角 Configure | Project Defaults | Project Structure 打开配置窗口
![2](/assets/image/2018-03-10-IntelliJ IDEA Development basis 2.png)  

选择 IntelliJ Platform Plugin SDK
![4](/assets/image/2018-03-10-IntelliJ IDEA Development basis 4.png)  

选择默认的 IntelliJ IDEA 文件夹即可配置完毕
![3](/assets/image/2018-03-10-IntelliJ IDEA Development basis 3.png)  

## 四、 Hello World

![6](/assets/image/2018-03-10-IntelliJ IDEA Development basis 6.png)  

![7](/assets/image/2018-03-10-IntelliJ IDEA Development basis 7.png)  

![8](/assets/image/2018-03-10-IntelliJ IDEA Development basis 8.png)  



[参考](http://www.ruanyifeng.com/blog/2015/12/git-cheat-sheet.html)

-------------------

## 三、	附录
[官方教程](http://www.jetbrains.org/intellij/sdk/docs/welcome.html)

-------------------