---
layout: post
title: "使用 Ubuntu 编译 ijkplayer 源码"
excerpt: "使用 Ubuntu 编译 ijkplayer 源码，基于版本 0.8.8"
date: 2018-4-18
categories:
  - ijkplayer
tags:
  - ijkplayer
---

## 0x0000 安装 Ubuntu
> 我用的是 Oracle VM VirtualBox [下载地址](https://www.virtualbox.org/) 虚拟机来安装 Ubuntu [下载地址](https://www.ubuntu.com/download)，不会对已安装的系统造成什么影响。在新建的虚拟机时配置内存要选用大一点的，第一次我安装全是默认项，卡的要死，建议分配内存4G，硬盘20G以上，还有下面我遇到的一个小坑。

### 报错：不能为虚拟电脑  XXX 打开一个新任
* 此时需要通过安装 “Oracle VM VirtualBox Extension Pack” 扩展包解决此问题 [下载地址](https://www.virtualbox.org/wiki/Downloads)

![1](/assets/image/2018-04-18-Compile ijkplayer 1.png)  

* 安装菜单 管理 → 全局设定 → 扩展

![2](/assets/image/2018-04-18-Compile ijkplayer 2.png)  

-------------------

## 0x0001 安装相关工具

### 1.作用与意义 
1.首先先去下载androidNDK 以及SDK
android NDK选择Linux的
android SDK选择一个高一点的Linux版本就好。
下载完成之后打开终端
Ctrl+Alt+T
我们在home\Downloads目录下会看到我们下载的ndk和sdk压缩包我们把它们解压出来，一个是.zip的另一个是.tgz的。
cd ~/Downloads
unzip xxx.zip
tar zxvf xxx.taz
将两个压缩文件解压到当前目录即可。
2.下载openjdk
sudo apt-get install openjdk-8-jre-headless
之后会自动安装好。
3.配置SDK和NDK环境变量
新下载好的linux版本的sdk缺少一点东西，需要我们自己去下，好在官方readme中已经说明了打开和windows中的sdkmanager一样的东西，在Linux中不叫这个名字，叫做android.sh 在tools目录下。说明如下
To start the SDK Manager,please execute the program "android".
启动android sdk manager
sh ~/Downloads/android-sdk-linux/tools/android
之后就是和windows一样了。
我们下载最新的Android SDK Tools和Android SDK Platform-tools以及Android SDK Build-tools，在下载一个最新的Android SDK Platform即可。
配置环境变量
sudo gedit /etc/profile
/etc/profile 的文件是让所有用户都可用。
在配置文件末尾加入如下部分并保存：
export PATH=~/Downloads/android-sdk-linux/platform-tools:$PATH
export PATH=~/Downloads/android-sdk-linux/tools:$PATH
export ANDROID_NDK=~/Downloads/android-ndk-r14b
export PATH=~/Downloads/android-ndk-r14b:$PATH
让环境变量立即生效
source /etc/profile
你可以用简单的命令来测试一下是否生效了如
adb -version

3.安装一些软件
sudo apt-get update
sudo apt-get install git
sudo apt-get install yasm
4.下载ijkplayer-android
git clone https://github.com/Bilibili/ijkplayer.git ijkplayer-android
5.开始编译
cd ijkplayer-android
先做初始化
./init-android.sh
初始化从网上提取ffmpeg库，有段时间，可以先去泡杯茶。
如果视频播放需要支持Https协议的还需要执行一遍如下初始化脚本。
./init-android-openssl.sh


作者：Ggx的代码之旅
链接：https://www.jianshu.com/p/f2ae594942d9
來源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

-------------------




## 总结


-------------------

## 相关链接


-------------------