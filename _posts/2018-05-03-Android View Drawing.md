---
layout: post
title: "Android View 绘制流程分析"
excerpt: "结合源码分析从一个 View 被声明到 XML 开始到最终是如何显示到页面上的"
date: 2018-5-3
categories:
  - Android Source
tags:
  - View
  - 绘制流程
---

## 0x0000 前言
> 此篇博文讨论的技术是在 XML 里声明的 View 是如何最终显示到页面上的。并且你可能需要查看一些 Android Open Source，这里推举一个在线看源码的网站 [androidxref](http://androidxref.com/)，所展示的源码来自 [Android 8.0](http://androidxref.com/8.0.0_r4/)。因为涉及知识点多，所以此篇幅非常长，请耐心阅读~

-------------------





-------------------
