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

## 0x0000 基本流程
* 目的：使用 FFmpeg 将一个 MP4 视频文件切割成多个 ts 切片，并在生成 ts 切片过程中对每一个切片使用 AES-128 加密，最后生成一个包含解密 Key 的 m3u8 视频播放索引文件。这些被加密的 ts 切片无法在其他地方播放，必须使用 m3u8 文件包含的解密 key 才可以。  
* 工具：需要在 PC 配置 FFmpeg 环境，通过 Dos 命令行生成 ts 切片 + m3u8 配置文件，加密 key 文件需要配置 OpenSSL 来生成加密文件或者直接随便手写一段字符串也可以，区别是手写的是明文加密，使用 OpenSSL 生成的加密 key 文件是看不到明文的。

-------------------

## 0x0001 环境配置
* FFmpeg [GitHub](https://github.com/FFmpeg/FFmpeg) [官网](https://ffmpeg.org)  
下载地址 [https://ffmpeg.zeranoe.com/builds/](https://ffmpeg.zeranoe.com/builds/) 点击 [Download Build] 下载，也可以选择其他不同的版本

* 版本说明  
1、Dev 是开发版本，里面包含有库文件（.lib）和头文件（.h），但是没有 exe 文件。  
2、Shared 文件夹里面有 ffmpeg.exe、ffplay.exe、ffprobe.exe，除此之外还有一些dll文件，比如说avcodec-58.dll、avdevice-58.dll等。它的exe文件比较小，运行时需要调用dll的功能。  
3、Static 文件夹里面只有三个 exe，dll文件被集成在 exe 里面了，所以文件比较大。我们使用这个版本就可以。

* 配置 FFmpeg
下载完成解压进入 bin 目录，将 bin 目录配置到系统环境变量 Path 里即可
![1](/assets/image/2018-10-26-Generate encrypted video 1.png)  
![2](/assets/image/2018-10-26-Generate encrypted video 2.png)  

* 配置 OpenSSL 这个是用来专门生成密码的，如果你是手写的可以不用  
下载地址 [https://slproweb.com/products/Win32OpenSSL.html](https://slproweb.com/products/Win32OpenSSL.html)  
安装成功后使用同样的方法，将 bin 目录配置到系统环境变量。

-------------------

## 0x0002 使用 OpenSSL 生成密钥
* 生成密钥文件
```
openssl rand 16 > [你的密钥文件路径] 例 D:\encrypt.key
```

* 生成 IV
```
openssl rand -hex 16
```

-------------------

## 0x0003 使用 FFmpeg 生成加密文件
* 一个行命令同时生成 ts 切片，m3u8 索引播放文件并加密

```
ffmpeg -y -i [原始视频文件路径] -c:v libx264 -c:a copy -f hls -hls_time 180 -hls_list_size 0 -hls_key_info_file [密钥文件路径] -hls_playlist_type vod -hls_segment_filename [切片文件路径] [索引文件路径]
```

* 以上命令的部分参数说明

| ------ | ------ |
| -y | 直接覆盖已经存在的输出文件 |
| -c | 指定输入输出的解码编码器 |
| -f | 强制设置输入输出的文件格式，默认情况下ffmpeg会根据文件后缀名判断文件格式 |
| -hls_time | 指定生成 ts 视频切片的时间长度s |
| -hls_list_size 0 | 索引播放列表的最大列数 默认5，0 为不限制 |
| -hls_playlist_type vod | 表示当前的视频流并不是一个直播流，而是点播流 |
| -hls_segment_filename | 输出 ts m3u8 文件路径 |

* [原始文件路径] 例 D:\test.mp4  
* [密钥文件路径] 文件后缀名需为 .keyinfo 否则可能会找不到解密文件，例 D:\key.keyinfo
* [切片文件路径] 例 D:\test_%03d.ts
* [索引文件路径] 例 D:\test.m3u8 

## 0x0004 相关参数与格式说明

* m3u8 部分字段意义

| ------ | ------ |
| #EXTM3U | 每个M3U文件第一行必须是这个tag |
| #EXTINF:<duration>,<title> | duration表示持续的时间（秒）必须是整数，如果版本在3以上可以是浮点数 |
| #EXTINF | 指定每个媒体段(ts)的持续时间，这个仅对其后面的URI有效，每两个媒体段URI间被这个tag分隔开 |
| #EXT-X-TARGETDURATION | 指定最大的媒体段时间长（秒）。所以#EXTINF中指定的时间长度必须小于或是等于这个最大值。这个tag在整个PlayList文件中只能出现一次（在嵌套的情况下，一般有真正ts url的m3u8才会出现该tag）  格式如下：#EXT-X-TARGETDURATION:<s>：s表示最大的秒数 |
| #EXT-X-MEDIA-SEQUENCE | 每一个media URI 在 PlayList中只有唯一的序号，相邻之间序号+1 |
| #EXT-X-KEY | 表示怎么对media segments进行解码。其作用范围是下次该tag出现前的所有media URI  格式如下：#EXT-X-KEY:<attribute-list>：NONE 或者 AES-128。如果是NONE，则URI以及IV属性必须不存在，如果是AES-128(Advanced Encryption Standard)，则URI必须存在，IV可以不存在。对于AES-128的情况，keytag和URI属性共同表示了一个key文件，通过URI可以获得这个key，如果没有IV（Initialization Vector）,则使用序列号作为IV进行编解码，将序列号的高位赋到16个字节的buffer中，左边补0；如果有IV，则将改值当成16个字节的16进制数。 |
| #EXT-X-ALLOW-CACHE： | 是否允许做cache，这个可以在PlayList文件中任意地方出现，并且最多出现一次，作用效果是所有的媒体段  格式如下：#EXT-X-ALLOW-CACHE:<YES|NO> |

* key.keyinfo 文件规则，该文件内容一共为三行

| ------ | ------ |
解密与加密 key 文件可以使用同一个| 第一行 | 
 decrypt.key 文件路径 |
| 第二行 | 加密 encrypt.key 文件路径 |
| 第三行 | IV 非必须 |

解密与加密 key 文件可以使用同一个