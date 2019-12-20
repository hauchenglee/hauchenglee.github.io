---
layout: post
title: Java筆記 - Synchronization 線程同步機制
category: java
tags: [java]
---

## 同步

同步，用最基本生活中的話來說，就是將每個人要操作前的狀態都同步到最新。<br>
（想想A、B倆對象的交易行為就明白了）

同步的含義：
- 原子性（行為不可分割）
- 內存可見性（一個線程修改對象的狀態後，其他線程可看到狀態的改變）
- 有序性（在本線程內的所有操作都是天然有序的）

## Atomic 原子性



## synchronized 同步鎖

### 何謂鎖




### 同步用法

 


### 實現原理

在Java中，`synchronized`有兩種使用形式，同步方法和同步代碼塊：

```java
public class SynchronizedDemo {
    // sync method
    public synchronized void doSth1() {
        System.out.println("Hello World");
    }

    // sync block
    public void doSth2() {
        synchronized (SynchronizedDemo.class) {
            System.out.println("Hello World 2");
        }
    }
}
```

使用`javap`來反編譯以上代碼：

```
javac SynchronizedDemo.java
javas -c SynchronizedDemo

```

or

```
javac SynchronizedDemo.java
javap -verbose SynchronizedDemo
```

p.s. `javap`相關用法：
- [javap反编译java字节码文件 - 立足未来](https://blog.csdn.net/haovip123/article/details/52220405){:target="_blank"}
- [【synchronized关键词】从字节码层面解析 - 扬帆舟的博客 - CSDN博客](https://blog.csdn.net/zhoufanyang_china/article/details/54615857){:target="_blank"}
- [[碼農之路] JAVA 開發環境的設定(for Windows 10)](https://zerotowhole.blogspot.com/2017/05/java-for-windows-10.html){:target="_blank"}

編譯結果如下：

```console
Microsoft Windows [版本 10.0.17763.864]
(c) 2018 Microsoft Corporation. 著作權所有，並保留一切權利。

C:\java>javac SynchronizedDemo.java

C:\java>javap -verbose SynchronizedDemo
Classfile /C:/java/SynchronizedDemo.class
  Last modified 2019/12/20; size 586 bytes
  MD5 checksum 477ff2d007653ed9e6f73ccd887ab85a
  Compiled from "SynchronizedDemo.java"
public class SynchronizedDemo
  minor version: 0
  major version: 52
  flags: ACC_PUBLIC, ACC_SUPER
Constant pool:
   #1 = Methodref          #6.#19         // java/lang/Object."<init>":()V
   #2 = Fieldref           #20.#21        // java/lang/System.out:Ljava/io/PrintStream;
   #3 = String             #22            // Hello World
   #4 = Methodref          #23.#24        // java/io/PrintStream.println:(Ljava/lang/String;)V
   #5 = Class              #25            // SynchronizedDemo
   #6 = Class              #26            // java/lang/Object
   #7 = Utf8               <init>
   #8 = Utf8               ()V
   #9 = Utf8               Code
  #10 = Utf8               LineNumberTable
  #11 = Utf8               doSth
  #12 = Utf8               doSth1
  #13 = Utf8               StackMapTable
  #14 = Class              #25            // SynchronizedDemo
  #15 = Class              #26            // java/lang/Object
  #16 = Class              #27            // java/lang/Throwable
  #17 = Utf8               SourceFile
  #18 = Utf8               SynchronizedDemo.java
  #19 = NameAndType        #7:#8          // "<init>":()V
  #20 = Class              #28            // java/lang/System
  #21 = NameAndType        #29:#30        // out:Ljava/io/PrintStream;
  #22 = Utf8               Hello World
  #23 = Class              #31            // java/io/PrintStream
  #24 = NameAndType        #32:#33        // println:(Ljava/lang/String;)V
  #25 = Utf8               SynchronizedDemo
  #26 = Utf8               java/lang/Object
  #27 = Utf8               java/lang/Throwable
  #28 = Utf8               java/lang/System
  #29 = Utf8               out
  #30 = Utf8               Ljava/io/PrintStream;
  #31 = Utf8               java/io/PrintStream
  #32 = Utf8               println
  #33 = Utf8               (Ljava/lang/String;)V
{
  public SynchronizedDemo();
    descriptor: ()V
    flags: ACC_PUBLIC
    Code:
      stack=1, locals=1, args_size=1
         0: aload_0
         1: invokespecial #1                  // Method java/lang/Object."<init>":()V
         4: return
      LineNumberTable:
        line 1: 0

  public synchronized void doSth();
    descriptor: ()V
    flags: ACC_PUBLIC, ACC_SYNCHRONIZED
    Code:
      stack=2, locals=1, args_size=1
         0: getstatic     #2                  // Field java/lang/System.out:Ljava/io/PrintStream;
         3: ldc           #3                  // String Hello World
         5: invokevirtual #4                  // Method java/io/PrintStream.println:(Ljava/lang/String;)V
         8: return
      LineNumberTable:
        line 3: 0
        line 4: 8

  public void doSth1();
    descriptor: ()V
    flags: ACC_PUBLIC
    Code:
      stack=2, locals=3, args_size=1
         0: ldc           #5                  // class SynchronizedDemo
         2: dup
         3: astore_1
         4: monitorenter
         5: getstatic     #2                  // Field java/lang/System.out:Ljava/io/PrintStream;
         8: ldc           #3                  // String Hello World
        10: invokevirtual #4                  // Method java/io/PrintStream.println:(Ljava/lang/String;)V
        13: aload_1
        14: monitorexit
        15: goto          23
        18: astore_2
        19: aload_1
        20: monitorexit
        21: aload_2
        22: athrow
        23: return
      Exception table:
         from    to  target type
             5    15    18   any
            18    21    18   any
      LineNumberTable:
        line 7: 0
        line 8: 5
        line 9: 13
        line 10: 23
      StackMapTable: number_of_entries = 2
        frame_type = 255 /* full_frame */
          offset_delta = 18
          locals = [ class SynchronizedDemo, class java/lang/Object ]
          stack = [ class java/lang/Throwable ]
        frame_type = 250 /* chop */
          offset_delta = 4
}
SourceFile: "SynchronizedDemo.java"

C:\java>
```

<br>

這裡簡單的說明，更詳細的說明可以參考下面的鏈接，或是JVM也看過才會有比較清晰的認識。

- 對於同步方法，JVM採用`ACC_SYNCHRONIZED`標記符來實現同步（標記後需要先獲得鎖才能執行該方法）。
   1. 當某個線程要訪問某個方法的時候，會檢查是否有`ACC_SYNCHRONIZED`
   2. 如果有設置，則需要先獲得監視器（`monitor`），然後開始執行方法，方法執行完之後再釋放鎖
   3. 這時候如果其他線程來請求執行方法，會因為無法獲得監視器鎖而被阻斷
   4. 如果在方法執行中發生異常，並且沒有處理異常，那麼在異常被拋到方法外面之前監視器鎖會被自動釋放

- 對於同步代碼塊，JVM採用`monitorenter`（加鎖）、`monitorexit`（解鎖）來實現同步。
   1. 可以把`monitorenter`指令理解為加鎖，`monitorexit`理解為釋放鎖
   2. 每個對象維護這一個記錄被鎖次數的計數器
   3. 未被鎖定的計數器預設為0，當同一個線程多次獲得該對象鎖都會+1；反之，（同一個線程多次釋放）則-1
   4. 當計數器為0的時候，鎖將被釋放，其他線程便可以獲得鎖

Ref:
- [深入理解多线程（一）——Synchronized的实现原理-HollisChuang's Blog](http://www.hollischuang.com/archives/1883){:target="_blank"}
- [深入理解多线程（四）—— Moniter的实现原理-HollisChuang's Blog](http://www.hollischuang.com/archives/2030)
- [Chapter 2. The Structure of the Java Virtual Machine](https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-2.html#jvms-2.11.10){:target="_blank"}

### synchronized & 原子性



### synchronized & 可見性



### synchronized & 有序性



### 缺失



## volatile 可見性

### volatile & 有序性



### volatile & 可見性



### volatile & 原子性

## synchronized vs volatile




## Reference

- [Java多線程-同步與鎖機制 - 每日頭條](https://kknews.cc/code/4k64rvx.html){:target="_blank"}
- [Java线程(二)：线程同步synchronized和volatile · Java线程 · 看云](https://www.kancloud.cn/digest/java-thread/107457){:target="_blank"}
- [内存可见性和原子性：Synchronized和Volatile的比较 - pan_jinquan的博客](https://blog.csdn.net/guyuealian/article/details/52525724){:target="_blank"}
- [既然synchronized是"万能"的，为什么还需要volatile呢？ - 掘金](https://juejin.im/post/5d5c9fbce51d4561cd246641){:target="_blank"}
- [](){:target="_blank"}

---