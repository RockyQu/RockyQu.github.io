---
layout: post
title: "使用 FFmpeg 生成 ts 切片并使用 AES-128 加密"
excerpt: "使用 FFmpeg 生成 ts 切片并使用 AES-128 加密"
date: 2018-10-26
categories:
  - FFmpeg
tags:
  - FFmpeg
  - encrypt
  - video
---

## 0x0000 基本操作流程
目的：使用 FFmpeg 将一个 MP4 视频文件切割成多个 ts 切片，并在生成 ts 切片过程中对每一个切片使用 AES-128 加密，最后生成一个包含解密 Key 的 m3u8 视频播放索引文件。这些被加密的 ts 切片无法在其他地方播放，必须使用 m3u8 文件包含的解密 key 才可以。
工具：需要在 PC 配置 FFmpeg 环境，通过 Dos 命令行生成 ts 切片 + m3u8 配置文件，加密 key 文件需要配置 OpenSSL 来生成加密文件或者直接随便手写一段字符串也可以，区别是手写的是明文加密，使用 OpenSSL 生成的加密 key 文件是看不到明文的。

-------------------

## 0x0001 环境配置
* FFmpeg [GitHub](https://github.com/FFmpeg/FFmpeg) [官网](https://ffmpeg.org)  
下载地址 [https://ffmpeg.zeranoe.com/builds/](https://ffmpeg.zeranoe.com/builds/) 点击 [Download Build] 下载，也可以选择其他不同的版本

* 版本说明
版本有三个，分别是 Dev、Shared、Static  
1、Dev 是开发版本，里面包含有库文件（.lib）和头文件（.h），但是没有 exe 文件。
2、Shared 文件夹里面有 ffmpeg.exe、ffplay.exe、ffprobe.exe，除此之外还有一些dll文件，比如说avcodec-58.dll、avdevice-58.dll等。它的exe文件比较小，运行时需要调用dll的功能。
3、Static 文件夹里面只有三个 exe，dll文件被集成在 exe 里面了，所以文件比较大。我们使用这个版本就可以。

* 配置 FFmpeg
下载完成解压进入 bin 目录
![1](/assets/image/2018-10-26-Generate encrypted video 1.png)  

将 bin 目录配置到系统环境变量 Path 里即可，FFmpeg 环境配置结束了
![2](/assets/image/2018-10-26-Generate encrypted video 2.png)  

* 配置 OpenSSL 这个是用来专门生成密码的，如果你是手写的可以不用
