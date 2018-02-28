---
layout: post
title: "AndroidStudio NDK 开发简单实例教程"
excerpt: "简单实现了Java与C之间的互相调用实例"
date: 2018-1-18
categories:
  - Android NDK
tags:
  - AndroidStudio NDK
  - NDK
  - JNI
---

## 一、所需工具
> Android Studio 2.3.2、NDK、CMake、LLDB

## 二、概念说明
### 1、NDK（WNative Development Kit） 
NDK 是一系列工具的集合，帮助开发者快速开发 C/C++ 的动态库。Android程序运行在Dalvik虚拟机中，NDK允许用户使用类似 C/C++ 之类的原生代码语言执行部分程序。NDK包括从 C/C++ 生成原生代码库所需要的工具和build files。
将一致的原生库嵌入可以在Android设备上部署的应用程序包文件（application packages files ，即.apk文件）中。支持所有未来Android平台的一系列原生系统头文件和库

**为何要用到NDK?**
概括来说主要分为以下几种情况：
- 代码的保护。由于 apk 的 java 层代码很容易被反编译，而 C/C++ 库反汇难度较大。
- 可以方便地使用现存的开源库。大部分现存的开源库都是用 C/C++ 代码编写的。
- 提高程序的执行效率。将要求高性能的应用逻辑使用 C 开发，从而提高应用程序的执行效率。
- 便于移植。用 C/C++ 写得库可以方便在其他的嵌入式平台上再次使用。

摘自[参考](https://baike.baidu.com/item/NDK/5967464?fr=aladdin)

### 2、CMake

### 3、LLDB

-------------------

## 开始搭建（Windows）

**1、安装Git**  
[Git Download](https://git-scm.com/downloads)

 **2、安装Github客户端**  
为了方便提交到Github， 可以安装这个Github可视化操作客户端，也可以直接用Git上传，如果你对Git比较熟悉。  
[Github Desktop](https://desktop.github.com/)

**3、注册Github账号并新建一个仓库**  
- 如何注册略过，主要讲一下新建仓库。

![233](/assets/image/2017-09-10-GitHub Pages + Markdown + Jekyll Build Blog 233.png)  

- 仓库的名称需要命名为`用户名.github.io`

![231](/assets/image/2017-09-10-GitHub Pages + Markdown + Jekyll Build Blog 231.png)  

- 以这种方式新建好仓库后，在`Settings`的`GitHub Pages` 下你的Blog已经生成出来了。

![232](/assets/image/2017-09-10-GitHub Pages + Markdown + Jekyll Build Blog 232.png)  

- 地址就是`https://用户名.github.io/`，网上大部分老教程讲的这个地方需要再设置一下`Launch automatic page generator`，新版的其实已经不用了

**4、上传`Jekyll`博客模板**  
- 打开`Github Desktop`客户端，登录自己的`Github`账号

![234](/assets/image/2017-09-10-GitHub Pages + Markdown + Jekyll Build Blog 234.png)  

- Clone刚才新建好的`用户名.github.io`项目到本地，删除所有文件，只保留`.git`

![235](/assets/image/2017-09-10-GitHub Pages + Markdown + Jekyll Build Blog 235.png) 

- 去[Jekyll模板](http://jekyllthemes.org/)找一套你喜欢的模板下载，解压后将根目录所有文件复制到项目的根目录


- 使用Github Desktop Push到Github，此时你的博客已经建立完毕了！

-------------------

## Jekyll基本教程
> 网上教程大部份只是讲了上面的如何建立博客，当你完成后你会发现不知道如何编辑一个博客，下面介绍关于`Jekyll`如果编辑博客的简单教程。
有些教程让你安装Jekyll环境，目的就是为了可以在本地生成页面，实时浏览，方便调试，如果嫌麻烦，也可以用以下方法，Jekyll编辑一篇博客使用Markdown，有很多在线Markdown编辑，推荐[Markdown在线编辑](https://maxiang.io/)，效果实时预览的，很方便。

**1、Jekyll文件结构**

- `_config.yml` 用于保存配置（jekyll会自动加载这些配置）
- `_includes` 存放可以重复利用的文件，可以被其他的文件包含
- `_layouts` 存放模板文件
- `_posts` 存放实际的博客文章内容（文件名格式：年-月-日-标题.md）
- `assets` 存放实际的博客用到的图片、音频等

**2、编辑博客**  
直接在_posts文件夹下找一篇复制一份，用[Markdown在线编辑](https://maxiang.io/)修改里面的内容，改好了再复制回去，用Github Desktop Push到Github，即可看到了  

**3、其他更多教程**   
[Jekyll教程](http://jekyll.com.cn/docs/home/)  
[Markdown教程](http://www.appinn.com/markdown/)

## 反馈与建议
- [Issues](https://github.com/DesignQu/DesignQu.github.io/issues)
- <quhaifeng2017@gmail.com>

-------------------

感谢你阅读这份教程

-------------------