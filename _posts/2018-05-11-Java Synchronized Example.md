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

-------------------

## 0x00 前言
> synchronized 是 Java语言的关键字，当它用来修饰一个方法或者一个代码块的时候，能够保证在同一时刻最多只有一个线程执行该段代码，而这段代码也被称为临界区。

## 0x01 几种应用方式
* 修饰代码块，被修饰的代码块称为同步语句块，其作用的范围是大括号{}括起来的代码，作用的对象是调用这个代码块的对象
* 修饰方法，被修饰的方法称为同步方法，其作用的范围是整个方法，作用的对象是调用这个方法的对象
* 修饰静态方法，其作用的范围是整个静态方法，作用的对象是这个类的所有对象
* 修饰类，其作用的范围是synchronized后面括号括起来的部分，作用主的对象是这个类的所有对象

## 0x02 修饰代码块
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
![5](/assets/image/2018-05-11/2018-05-11-Java synchronized Example 1.png)

把上面的调用代码改成如下代码，发现 thread3 和 thread4 同时在执行。这是因为 synchronized 只锁定对象，每个对象只有一个锁（lock）与之相关联这时会有两把锁分别锁定 runnable1 对象和 runnable2 对象，而这两把锁是互不干扰的，不形成互斥，所以两个线程可以同时执行。
```
SyncRunnable runnable1 = new SyncRunnable();
SyncRunnable runnable2 = new SyncRunnable();
Thread thread3 = new Thread(runnable1);
Thread thread4 = new Thread(runnable2);
thread3.start();
thread4.start();
```

指定某个对象加锁
```
class Counter {

    String name;
    int count;

    public Counter(String name, int count) {
        this.name = name;
        this.count = count;
    }

    public void add(int count) {
        this.count += count;
        try {
            Thread.sleep(300);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    public void sub(int count) {
        this.count -= count;
        try {
            Thread.sleep(300);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    public int getCount() {
        return count;
    }
}

class CounterOperator implements Runnable {

    private final Counter counter;

    public CounterOperator(Counter counter) {
        this.counter = counter;
    }

    public void run() {
        synchronized (counter) {// 可以去掉此行代码，查看不同的结果
            counter.add(500);
            counter.sub(500);
            Logg.e(Thread.currentThread().getName() + ":" + counter.getCount());
        }
    }
}
```

调用代码
```
Counter account = new Counter("Counter", 1000);
CounterOperator counterOperator = new CounterOperator(account);
for (int i = 0; i < 5; i++) {
    Thread thread = new Thread(counterOperator, "Thread" + i);
    thread.start();
}
```

在 CounterOperator 类中的 run 方法里，我们用 synchronized 给 Counter 对象加锁。这时，当一个线程访问 Counter 对象时，其他试图访问 Counter 对象的线程将会阻塞，直到该线程访问 Counter 对象结束。也就是说谁拿到那个锁谁就可以运行它所控制的那段代码。当没有明确的对象作为锁，只是想让一段代码同步时，可以创建一个特殊的对象来充当锁
```
byte[] lock = new byte[0]
```

注意创建这种类型锁时可以使用 byte[] lock = new byte[0] 零长度的 byte 数组 Object lock = new Object() 更经济

## 0x03 修饰方法
将最开始的 run() 方法改成如下代码，既是修饰一个方法，修饰方法和修饰代码块是等价的，只是作用范围不一样，修饰代码块是大括号括起来的范围，而修饰方法范围是整个函数
```
public synchronized void run() {
    for (int i = 0; i < 3; i++) {
        try {
            Logg.e(i);
            Thread.sleep(100);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

方法修饰需要注意几点
* synchronized 关键字不能继承。如果在父类中的某个方法使用了 synchronized 关键字，而在子类中覆盖了这个方法，在子类中的这个方法默认情况下并不是同步的，而必须在子类的这个方法中也加上 synchronized 才可以。或者还可以在子类方法中调用父类中相应的方法，这样虽然子类中的方法不是同步的，但子类调用了父类的同步方法，因此，子类的方法也就相当于同步了
* 在定义接口方法时不能使用 synchronized 关键字
* 构造方法不能使用 synchronized 关键字，但可以使用 synchronized 代码块来进行同步

### 修饰静态方法
静态方法是属于类的而不属于对象的。同样 synchronized 修饰的静态方法锁定的是这个类的所有对象
```
static class SyncStaticRunnable implements Runnable {

    public synchronized void run() {
        method();
    }

    public synchronized static void method() {
        for (int i = 0; i < 3; i++) {
            try {
                Logg.e(i);
                Thread.sleep(200);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
```

调用代码
```
SyncStaticRunnable runnable1 = new SyncStaticRunnable();
SyncStaticRunnable runnable2 = new SyncStaticRunnable();
Thread thread3 = new Thread(runnable1);
Thread thread4 = new Thread(runnable2);
thread3.start();
thread4.start();
```

runnable1 和 runnable2 是两个对象，但在 thread3 和 thread4 并发执行时却保持了线程同步。这是因为 run 中调用了静态方法，而静态方法是属于类的，所以相当于用了同一把锁

## 0x04 修饰类
把刚才上面的类修改一下如下代码
```
static class SyncStaticRunnable implements Runnable {

    public synchronized void run() {
        method();
    }

    public static void method() {
    	synchronized(SyncStaticRunnable.class) {
            for (int i = 0; i < 3; i++) {
                try {
                    Logg.e(i);
                    Thread.sleep(200);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
``` 

效果是一样的，synchronized 作用于一个类时，类所有对象用的是同一把锁

-------------------

## 0x05 总结
* synchronized 关键字加在方法上或对象上，如果它作用的对象是非静态的，则它取得的锁是对象。如果 synchronized 作用的对象是一个静态方法或一个类，则它取得的锁是对类，该类所有的对象同一把锁
* 每个对象只有一个锁与之相关联，谁拿到这个锁谁就可以运行它所控制的那段代码
* 实现同步是要很大的系统开销作为代价的，甚至可能造成死锁，所以尽量避免无谓的同步控制