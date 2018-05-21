---
layout: post
title: "Java synchronized 用法小结"
excerpt: "Java 线程同步 synchronized 即同步锁使用方法总结"
date: 2018-5-11
categories:
  - Java
tags:
  - Java
  - synchronized
  - 线程同步
---

## 0x0000 前言
> synchronized 是 Java语言的关键字，当它用来修饰一个方法或者一个代码块的时候，能够保证在同一时刻最多只有一个线程执行该段代码，而这段代码也被称为临界区。

## 0x0001 应用方式
* 修饰代码块，被修饰的代码块称为同步语句块，其作用的范围是大括号{}括起来的代码，作用的对象是调用这个代码块的对象
* 修饰方法，被修饰的方法称为同步方法，其作用的范围是整个方法，作用的对象是调用这个方法的对象
* 修饰静态方法，其作用的范围是整个静态方法，作用的对象是这个类的所有对象
* 修饰类，其作用的范围是synchronized后面括号括起来的部分，作用主的对象是这个类的所有对象

### 修饰代码块
```
class SyncRunnable implements Runnable {

    public void run() {
        synchronized (this) {// 可以去掉此行代码，查看不同的结果
            for (int i = 0; i < 3; i++) {
                try {
                    Logg.e(i);
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

调用代码
```
SyncRunnable runnable = new SyncRunnable();
Thread thread1 = new Thread(runnable);
Thread thread2 = new Thread(runnable);
thread1.start();
thread2.start();
```

运行结果
![5](/assets/image/2018-05-11-Java synchronized Example 1.png)

把上面的调用代码改成如下代码，发现 thread3 和 thread4 同时在执行。这是因为 synchronized 只锁定对象，每个对象只有一个锁（lock）与之相关联这时会有两把锁分别锁定 runnable 对象和 runnable 对象，而这两把锁是互不干扰的，不形成互斥，所以两个线程可以同时执行。
```
SyncRunnable runnable1 = new SyncRunnable();
SyncRunnable runnable2 = new SyncRunnable();
Thread thread3 = new Thread(runnable1);
Thread thread4 = new Thread(runnable2);
thread3.start();
thread4.start();
```

### 修饰方法


### 修饰静态方法


### 修饰类

-------------------





-------------------
