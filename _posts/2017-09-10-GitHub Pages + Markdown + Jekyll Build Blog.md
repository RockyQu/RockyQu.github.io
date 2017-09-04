---
layout: post
title: "使用GitHub Pages + Markdown + Jekyll搭建自己的博客"
date: 2017-9-10
categories:
  - 笔记
tags:
  - GitHub Pages
  - Jekyll
  - Build Blog
---

### 这里只介绍一种快速搭建个人博客的方式

### 一、先了解一下一些相关概念说明

- **What is GitHub Pages?**
> Github Pages可以认为是用户编写的、托管在github上的静态网页，支持自带主题，并且可以使用Jekyll或者Hexo等静态博客框架进行管理。    —— [GithubPages](https://pages.github.com/)

- **What is Markdown?**
> 是一种轻量级标记语言，它允许人们使用易读易写的纯文本格式编写文档，然后转换成格式丰富的HTML页面。    —— [维基百科](https://zh.wikipedia.org/wiki/Markdown)

- **What is  Jekyll?**
> [Jekyll](https://github.com/jekyll)是一个简单的免费的静态网页生成工具，不需要数据库支持。但是可以配合第三方服务，最关键的是jekyll可以免费部署在Github上，而且可以绑定自己的域名。    —— [百度百科](https://baike.baidu.com/item/Jekyll)

-------------------

### 二、开始搭建（Windows）

**1、安装Git**
[Git Download](https://git-scm.com/downloads)

 **2、安装Github客户端**
为了方便提交到Github， 可以安装这个Github可视化操作客户端，也可以直接用Git上传，如果你对Git比较熟悉。
[Github Desktop](https://desktop.github.com/)

**3、注册一个Github账号并新建一个仓库**
如何注册略过，主要讲一下新建仓库。
仓库的名称需要命名为`用户名.github.io`
![项目菜单](./_screenshots/2.3.1.png)
![screenshots1](Logg/ImageFolder/screenshots1.png "screenshots1")
![设计页面](DesignQu.github.io/_screenshots/2.3.1.png)

当以这种方式新建好仓库后，在`Settings`的`GitHub Pages` 下你会发现你的Blog已经生成出来了。

[Github Desktop](https://desktop.github.com/)

-------------------

## Markdown简介

> Markdown 是一种轻量级标记语言，它允许人们使用易读易写的纯文本格式编写文档，然后转换成格式丰富的HTML页面。    —— [维基百科](https://zh.wikipedia.org/wiki/Markdown)

正如您在阅读的这份文档，它使用简单的符号标识不同的标题，将某些文字标记为**粗体**或者*斜体*，创建一个[链接](http://www.example.com)或一个脚注[^demo]。下面列举了几个高级功能，更多语法请按`Ctrl + /`查看帮助。 

### 代码块
``` python
@requires_authorization
def somefunc(param1='', param2=0):
    '''A docstring'''
    if param1 > param2: # interesting
        print 'Greater'
    return (param2 - param1 + 1) or None
class SomeClass:
    pass
>>> message = '''interpreter
... prompt'''
```

### 表格
| Item      |    Value | Qty  |
| :-------- | --------:| :--: |
| Computer  | 1600 USD |  5   |
| Phone     |   12 USD |  12  |
| Pipe      |    1 USD | 234  |

### 离线存储
**马克飞象**使用浏览器离线存储将内容实时保存在本地，不必担心网络断掉或浏览器崩溃。为了节省空间和避免冲突，已同步至印象笔记并且不再修改的笔记将删除部分本地缓存，不过依然可以随时通过`文档管理`打开。

> **注意：**虽然浏览器存储大部分时候都比较可靠，但印象笔记作为专业云存储，更值得信赖。以防万一，**请务必经常及时同步到印象笔记**。

## 关于收费

**马克飞象**为新用户提供 10 天的试用期，试用期过后需要[续费](maxiang.info/vip.html)才能继续使用。未购买或者未及时续费，将不能同步新的笔记。之前保存过的笔记依然可以编辑。


## 反馈与建议
- 微博：[@马克飞象](http://weibo.com/u/2788354117)，[@GGock](http://weibo.com/ggock "开发者个人账号")
- 邮箱：<hustgock@gmail.com>

---------
感谢阅读这份帮助文档。请点击右上角，绑定印象笔记账号，开启全新的记录与分享体验吧。
