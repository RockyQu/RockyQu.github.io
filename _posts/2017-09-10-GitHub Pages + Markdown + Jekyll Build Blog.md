---
layout: post
title: "使用GitHub Pages + Markdown + Jekyll搭建自己的博客"
excerpt: "这里只是介绍一种快速搭建个人博客的方式"
date: 2017-9-10
categories:
  - 笔记
tags:
  - GitHub Pages
  - Markdown
  - Jekyll
  - Build Blog
---

-------------------

## 0x00 先了解一下一些相关概念说明

- **What is GitHub Pages?**
> Github Pages可以认为是用户编写的、托管在github上的静态网页，支持自带主题，并且可以使用Jekyll或者Hexo等静态博客框架进行管理。    —— [GithubPages](https://pages.github.com/)

- **What is Markdown?**
> 是一种轻量级标记语言，它允许人们使用易读易写的纯文本格式编写文档，然后转换成格式丰富的HTML页面。    —— [维基百科](https://zh.wikipedia.org/wiki/Markdown)

- **What is  Jekyll?**
> [Jekyll](https://github.com/jekyll)是一个简单的免费的静态网页生成工具，不需要数据库支持。但是可以配合第三方服务，最关键的是jekyll可以免费部署在Github上，而且可以绑定自己的域名。    —— [百度百科](https://baike.baidu.com/item/Jekyll)

-------------------

## 0x01 开始搭建（Windows）

**1、安装Git**  
[Git Download](https://git-scm.com/downloads)

 **2、安装Github客户端**  
为了方便提交到Github， 可以安装这个Github可视化操作客户端，也可以直接用Git上传，如果你对Git比较熟悉。  
[Github Desktop](https://desktop.github.com/)

**3、注册Github账号并新建一个仓库**  
- 如何注册略过，主要讲一下新建仓库。

![233](/assets/image/2017-09-10/2017-09-10-GitHub Pages + Markdown + Jekyll Build Blog 233.png)  

- 仓库的名称需要命名为`用户名.github.io`

![231](/assets/image/2017-09-10/2017-09-10-GitHub Pages + Markdown + Jekyll Build Blog 231.png)  

- 以这种方式新建好仓库后，在`Settings`的`GitHub Pages` 下你的Blog已经生成出来了。

![232](/assets/image/2017-09-10/2017-09-10-GitHub Pages + Markdown + Jekyll Build Blog 232.png)  

- 地址就是`https://用户名.github.io/`，网上大部分老教程讲的这个地方需要再设置一下`Launch automatic page generator`，新版的其实已经不用了

**4、上传`Jekyll`博客模板**  
- 打开`Github Desktop`客户端，登录自己的`Github`账号

![234](/assets/image/2017-09-10/2017-09-10-GitHub Pages + Markdown + Jekyll Build Blog 234.png)  

- Clone刚才新建好的`用户名.github.io`项目到本地，删除所有文件，只保留`.git`

![235](/assets/image/2017-09-10/2017-09-10-GitHub Pages + Markdown + Jekyll Build Blog 235.png) 

- 去[Jekyll模板](http://jekyllthemes.org/)找一套你喜欢的模板下载，解压后将根目录所有文件复制到项目的根目录

- 使用Github Desktop Push到Github，此时你的博客已经建立完毕了！

-------------------

## 0x02 Jekyll基本教程
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