---
layout: post
title: Java - Thread Synchronization 線程同步機制
category: it
tags: [java]
---

同步是一種概念，用於防止多個線程輸入代碼的特定部分，這又是避免線程問題的最基本概念。

> Synchronization is a concept to prevent more than one thread from entering a specific part of code, 
> which is - again - the most basic concept of avoiding threading issues.）
>
> Ref: [multithreading - Synchronization in java - Can we set priority to a synchronized access in java? - Stack Overflow](https://bit.ly/2PGluTv){:target="_blank"}

同步，用最基本生活中的話來說，就是將每個人要操作前的狀態都同步到最新。<br>
（想想A、B倆對象的交易行為就明白了）

同步的含義：
- 原子性（行為不可分割）
- 內存可見性（一個線程修改對象的狀態後，其他線程可看到狀態的改變）
- 重排序（有些文章叫作有序性）

## synchronized

### synchronized usage

The `synchronized` keyword can be used on different levels:
- Instance methods
- Static methods
- Code blocks

The default method and test:

```
import org.junit.jupiter.api.Test;

import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.TimeUnit;
import java.util.stream.IntStream;

import static org.junit.jupiter.api.Assertions.assertEquals;

public class SynchronizedMethods {

    private int sum = 0;

    public void calculate() {
        setSum(getSum() + 1);
    }

    // standard setters and getters
    public int getSum() {
        return sum;
    }

    public void setSum(int sum) {
        this.sum = sum;
    }

    @Test
    public void givenMultiThread_whenNonSyncMethod() throws InterruptedException {
        ExecutorService service = Executors.newFixedThreadPool(3);
        SynchronizedMethods summation = new SynchronizedMethods();

        IntStream.range(0, 1000)
                .forEach(count -> service.submit(summation::calculate));

        service.awaitTermination(1000, TimeUnit.MILLISECONDS);
        assertEquals(1000, summation.getSum());
    }
}
```

<br>

*Synchronized Instance Methods*

we add `synchronized` in the method declaration:

```
public synchronized void synchronisedCalculate() {
    setSum(getSum() + 1);
}
```

<br>

*Synchronized Static Methods*

```
public static synchronized void syncStaticCalculate() {
    setSum(getSum() + 1);
}
```

These methods are `synchronized` on the `Class` object associated with the class and since only 
one `Class` object exists per JVM per class, only one thread can execute inside a `static synchronized` 
method per class, irrespective of the number of instances it has.

翻譯一下：

> 這些方法在與該類關聯的Class對像上同步，並且由於每個JVM每個類僅存在一個Class對象，因此每個類
> 在一個靜態同步方法內只能執行一個線程，而不管它有多少實例。

<br>

*Synchronized Blocks Within Methods*

Sometimes we do not want to synchronized the entire method but only some instructions within it.

```
public void performSynchrinisedTask() {
    synchronized (this) {
        setSum(getSum() + 1);
    }
}
```

Notice, that we passed a parameter `this` to the `synchronized` block. This is the monitor object, the 
code inside the block get synchronized on the monitor object. Simply put, only one thread per monitor 
object can execute inside that block of code.

In case the method is `static`, we would pass class name in place of the object reference. And the class 
would be a monitor for synchronization of the block:

翻譯一下：

> 注意，我們將參數`this`傳遞給了同步塊。這是監視對象，塊內的代碼在監視對像上同步。
> 簡而言之，每個監視對像只有一個線程可以在該代碼塊內執行。
>
> 如果方法是靜態的，我們將傳遞類名代替對象引用。該類將成為該塊同步的監視器：

```
public static void performStaticSyncTask() {
    synchronized (SynchroniseBlocks.class) {
        setStaticCount(getStaticCount() + 1);
    }
}
```

<br>

Ref:
- [Guide to the Synchronized Keyword in Java - Baeldung](https://www.baeldung.com/java-synchronized){:target="_blank"}
- [Java Synchronization Tutorial : What, How and Why?](https://javarevisited.blogspot.com/2011/04/synchronization-in-java-synchronized.html){:target="_blank"}
- [Synchronized in Java - GeeksforGeeks](https://www.geeksforgeeks.org/synchronized-in-java/){:target="_blank"}

### implementation principle

以下為`synchronized`同步方法和同步代碼塊：

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
- [深入理解多线程（四）—— Moniter的实现原理-HollisChuang's Blog](http://www.hollischuang.com/archives/2030){:target="_blank"}
- [Chapter 2. The Structure of the Java Virtual Machine](https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-2.html#jvms-2.11.10){:target="_blank"}

### synchronized & 原子性

被`synchronized`修飾過的代碼片段，在進入之前加了鎖，只要它沒有執行完，其他線程是無法獲得執行這段代碼片段的，就可以保證它內部的代碼可以全部被執行，進而保證原子性。

> 线程1在执行`monitorenter`指令的时候，会对Monitor进行加锁，加锁后其他线程无法获得锁，
> 除非线程1主动解锁。即使在执行过程中，由于某种原因，比如CPU时间片用完，线程1放弃了
> CPU，但是，他并没有进行解锁。而由于`synchronized`的锁是可重入的，下一个时间片还是只能
> 被他自己获取到，还是会继续执行代码。直到所有代码执行完。这就保证了原子性。

### synchronized & 可見性

`synchronized`可以保證可見性。

從[java.util.concurrent (Java Platform SE 7 )](https://bit.ly/35DeEns){:target="_blank"}官方文件中說明了怎樣的操作才能保證【Memory Consistency Properties】，其中有一段文字：

> Because the happens-before relation is transitive, all actions of a thread prior to unlocking happen-before all actions subsequent to any thread locking that monitor.

翻譯一下：

> 由於事前發生關係是可傳遞的，因此在解鎖之前，線程的所有操作都發生在監視該線程的所有線程之後的所有操作之前

這個翻譯太玄幻，我也看不懂在翻譯什麼 - -

我修飾一下：

> 由於先行發生原則的關係具有傳遞性，因此【該線程在解鎖之前的所有操作】都發生在【監視器執行監控全部線程的所有操作】之前

<br>

其他多線程保證可見性（Memory Consistency Properties）的方法還包括：
- 使用`volatile`關鍵字，但不要求互斥鎖定（but do *not* mutual exclusion locking）
- 在啟動執行線程中任何操作之前（*happens-before*），都確保`Thread.start()`對線程的調用
- 在`join`返回（`return`）之前，執行線程中的所有操作
- 使用`java.util.concurrent`提供更高級別的同步（higher-level synchronization）

### synchronized & 有序性

`synchronized`不可以保證【線程訪問有序性】，同步將以任何亂數次序（random order, or who is the fastest to grab the Lock）訪問，如何訪問取決於JVM的實現，在某些情況甚至會發生**飢餓**（starve）。

Ref: [multithreading - Ensure Java synchronized locks are taken in order? - Stack Overflow](https://bit.ly/34HixGL){:target="_blank"}

<br>

`synchronized`【無法對訪問設置優先權（priority）】，僅提供基於公平（fairness）的同步。

Ref: [multithreading - Synchronization in java - Can we set priority to a synchronized access in java? - Stack Overflow](https://bit.ly/34FUB6K){:target="_blank"}

<br>

`synchronized`關於【重排序（reordering）】的規則：
- 當且僅當所有順序一致的執行都沒有數據爭用時，程序才能正確同步。（A program is *correctly synchronized* if and only if all sequentially consistent executions are free of data races.）
- 如果程序正確同步，則該程序的所有執行將看起來是順序一致的。（If a program is correctly synchronized, then all executions of the program will appear to be sequentially consistent.）

Ref:
- [Chapter 17. Threads and Locks](https://docs.oracle.com/javase/specs/jls/se7/html/jls-17.html#jls-17.4.5){:target="_blank"}
- [multithreading - Does synchronized keyword prevent reordering in Java? - Stack Overflow](https://bit.ly/2SbYZYl){:target="_blank"}
- [multithreading - Synchronized and Code reorder in java - Stack Overflow](https://bit.ly/34DO3W1){:target="_blank"}

## volatile

### volatile & 有序性



### volatile & 可見性



### volatile & 原子性



## synchronized vs volatile



Ref:
- [既然synchronized是"万能"的，为什么还需要volatile呢？ - 掘金](https://juejin.im/post/5d5c9fbce51d4561cd246641){:target="_blank"}

## Reference

- [Java线程(二)：线程同步synchronized和volatile · Java线程 · 看云](https://www.kancloud.cn/digest/java-thread/107457){:target="_blank"}
- [内存可见性和原子性：Synchronized和Volatile的比较 - pan_jinquan的博客](https://blog.csdn.net/guyuealian/article/details/52525724){:target="_blank"}
- [既然synchronized是"万能"的，为什么还需要volatile呢？ - 掘金](https://juejin.im/post/5d5c9fbce51d4561cd246641){:target="_blank"}

---