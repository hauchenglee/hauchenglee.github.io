---
layout: post
title: Java - Executors 線程池
category: java
tags: [java]
---

## Thread Pool in Oracle tutorial

【[Thread Pools (The Java™ Tutorials > Essential Classes > Concurrency)](https://docs.oracle.com/javase/tutorial/essential/concurrency/pools.html){:target="_blank"}】

Most of the executor implementations in `java.util.cuncurrent` use *thread pools*, which consist of *worker threads*. 
This kind of thread exists separately from the `Runnable` and `Callable` tasks it executes and is often used to execute 
multiple tasks.

Using worker threads minimizes the overhead due to thread creation. Thread objects use a significant amount of memory, 
and in a large-scale application, allocating and deal locating many thread creates a significant memory 
management overhead.

One common type of thread pool is the *fixed thread pool*. This type of pool always has a specified number of threads 
running; if a thread is somehow terminated while it is still in use, it is automatically replaced with a new thread. Tasks are 
submitted to the pool via an internal queue, which holds extra tasks whenever there are more active tasks than threads.

An important advantage of the fixed thread pool is that applications using it *degrade gracefully*. To understand this, 
consider a web server application where each HTTP request is handled by a separate thread. If the application simply 
creates a new thread for every new HTTP request, and the system receives more requests than is can handle immediately, 
the application will suddenly stop responding to *all* requests when the overhead of all those threads exceed the capacity of 
the system. With a limit on the number of the threads that can be created, the application will not be servicing HTTP 
requests as quickly as they come in, but it will be servicing them as quickly as the system can sustain.

A simple way to create an executor that uses a fixed thread pool is to invoke the [`newFixedThreadPool`](https://bit.ly/2RQTT24){:target="_blank"} factory method 
in [`java.util.concurrent.Executors`](https://bit.ly/2ROmvsB){:target="_blank"}. This class also provides the following factory methods:

- The [`newCachedThreadPool`](https://bit.ly/310rL0v){:target="_blank"} method creates an executor with an expandable thread pool. This executor is 
suitable for applications that launch many short-lived tasks.
- The [`newSingleThreadExecutor`](https://bit.ly/2TUdHnP){:target="_blank"} method creates an executor that executes a single task at a time.
- Several factory methods are `ScheduledExecutorService` versions of the above executors.

If none of the executors provided by the above factory methods meet your needs, constructing instances of 
[`java.util.concurrent.ThreadPoolExecutor`](https://bit.ly/2RR1Fc7){:target="_blank"} or [`java.util.concurrent.ScheduledThreadPoolExecutor`](https://bit.ly/2tI1yYB){:target="_blank"}

<br>

大多數執行程序實現都在`java.util.concurrent`使用線程池，線程池由工作線程組成。
此類線程與其執行的`Runnable`和`Callable`任務分開存在，通常用於執行多個任務。

使用工作線程可以最大程度地減少線程創建所帶來的開銷。
線程對象使用大量內存，在大型應用程序中，分配和取消分配許多線程對象會產生大量內存管理開銷。

線程池的一種常見類型是固定線程池。這種類型的池始終具有指定數量的正在運行的線程。
如果某個線程在仍在使用時以某種方式終止，則它將自動替換為新線程。
任務通過內部隊列提交到池中，該內部隊列在活動任務多於線程時容納額外的任務。

固定線程池的一個重要優點是使用該線程池的應用程序可以正常降級。
為了理解這一點，請考慮一個Web服務器應用程序，其中每個HTTP請求都由一個單獨的線程處理。
如果應用程序只是為每個新的HTTP請求創建一個新線程，並且系統收到的請求超出了立即處理的請求，
那麼當所有這些線程的開銷超出系統容量時，應用程序將突然停止響應所有請求。
由於可以創建的線程數受到限制，因此應用程序將不會盡快處理HTTP請求，但是會盡可能快地為系統提供服務。

創建使用固定線程池的執行程序的一種簡單方法是調用`newFixedThreadPool`的factory方法。`java.util.concurrent.Executors`此類還提供以下工廠方法：

- 該`newCachedThreadPool`方法創建具有可擴展線程池的執行程序。該執行程序適用於啟動許多短期任務的應用程序。
- 該`newSingleThreadExecutor`方法創建一個執行程序，一次執行一個任務。
- 幾種工廠方法是`ScheduledExecutorService`上述執行程序的版本。

如果上述工廠方法提供的執行程序都不滿足您的需求，則構造實例`java.util.concurrent.ThreadPoolExecutor`或`java.util.concurrent.ScheduledThreadPoolExecutor`將為您提供其他選擇。

## Thread Pool

【線程池是什麼】

- 【Java并发编程实战】頁119：

> 正如名稱中所稱的那樣，線程池管理一個工作者線程的同構池（homogeneous pool）。
> 線程池是與工作列隊（work queue）緊密綁定的。所謂工作列隊，其作用是持有所有等待執行的任務。
> 工作者線程的生活從此輕鬆起來：它從工作列隊中獲取下一個任務，執行它，然後回來繼續等待另一個線程。

- 【[线程池你真不来了解一下吗？ - Java3y - 博客园](https://www.cnblogs.com/Java3y/p/8996365.html){:target="_blank"}】

> 線程池可以看做是**線程的集合**。在沒有任務時線程處於空閒狀態，當請求到來：線程池給這個請求分配一個空閒的線程，
> 任務完成後回到線程池中等待下次任務（而不是銷毀-創建新線程）。這樣就**實現了線程的重用**。

用圖示來說明如下：

![](https://hauchenglee.github.io/assets/images/java/thread-pool-baeldung.png)

Ref: [Introduction to Thread Pools in Java - Baeldung](https://www.baeldung.com/thread-pool-java-and-guava){:target="_blank"}

<br>

【線程池好處】

通過Executor來啟動並使用線程池，比使用`Thread`的`start()`方法更好。（想一想，why?）

- 降低線程創建-銷毀過程的系統資源開銷
- 使用已存在的線程，不必等待線程創建的時間，提高響應速度
- 統一管理線程分配、調優和監控，並且具有更多功能方法實現

【線程池概念】

- 任務：線程需要執行的代碼，也就是Runnable
- 任務列隊：線程滿了，將後續任務放入任務列隊里等待，等其他任務在線程裡執行完，這個線程就空出來了，任務列隊就將最早來的未執行的任務放入線程執行
- 核心線程：線程池一直存在的線程
- 最大線程數量：指的是線程池所容納的最多的線程數量

## Executors

- 【[并发新特性—Executor 框架与线程池 - Java 并发编程 - 极客学院Wiki](https://wiki.jikexueyuan.com/project/java-concurrency/executor.html){:target="_blank"}】

> Executors 提供了一系列工厂方法用于创先线程池，返回的线程池都实现了 ExecutorService 接口。

<br>

- 【[Java 8 并发教程 Threads 和 Executors - Java 8 教程汇总](https://bit.ly/2RrCqxX){:target="_blank"}】

> 并发API引入了ExecutorService作为一个在程序中直接使用Thread的高层次的替换方案。Executors支持运
> 行异步任务，通常管理一个线程池，这样一来我们就不需要手动去创建新的线程。在不断地处理任务的过程中，
> 线程池内部线程将会得到复用，因此，在我们可以使用一个executor service来运行和我们想在我们整个程序
> 中执行的一样多的并发任务。
>
> Executors类提供了便利的工厂方法来创建不同类型的 executor services。
>
> Java进程从没有停止！Executors必须显式的停止-否则它们将持续监听新的任务。

### JDK

```java
package java.util.concurrent;
import java.util.*;
import java.util.concurrent.atomic.AtomicInteger;
import java.security.AccessControlContext;
import java.security.AccessController;
import java.security.PrivilegedAction;
import java.security.PrivilegedExceptionAction;
import java.security.PrivilegedActionException;
import java.security.AccessControlException;
import sun.security.util.SecurityConstants;

/**
 * Factory and utility methods for {@link Executor}, {@link
 * ExecutorService}, {@link ScheduledExecutorService}, {@link
 * ThreadFactory}, and {@link Callable} classes defined in this
 * package. This class supports the following kinds of methods:
 *
 * <ul>
 *   <li> Methods that create and return an {@link ExecutorService}
 *        set up with commonly useful configuration settings.
 *   <li> Methods that create and return a {@link ScheduledExecutorService}
 *        set up with commonly useful configuration settings.
 *   <li> Methods that create and return a "wrapped" ExecutorService, that
 *        disables reconfiguration by making implementation-specific methods
 *        inaccessible.
 *   <li> Methods that create and return a {@link ThreadFactory}
 *        that sets newly created threads to a known state.
 *   <li> Methods that create and return a {@link Callable}
 *        out of other closure-like forms, so they can be used
 *        in execution methods requiring {@code Callable}.
 * </ul>
 *
 * @since 1.5
 * @author Doug Lea
 */
```

### Example

Steps to be followed

> 1. Create a task(Runnable Object) to execute
> 2. Create Executor Pool using Executors
> 3. Pass tasks to Executor Pool
> 4. Shutdown the Executor Pool

```java
import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

// Task class to be executed (Step 1) 
class Task implements Runnable {
    private String name;

    public Task(String s) {
        name = s;
    }

    // Prints task name and sleeps for 1s 
    // This Whole process is repeated 5 times 
    @Override
    public void run() {
        try {
            for (int i = 0; i <= 5; i++) {
                if (i == 0) {
                    Date d = new Date();
                    SimpleDateFormat ft = new SimpleDateFormat("hh:mm:ss");
                    System.out.println("Initialization Time for"
                            + " task name - " + name + " = " + ft.format(d));
                    //prints the initialization time for every task  
                } else {
                    Date d = new Date();
                    SimpleDateFormat ft = new SimpleDateFormat("hh:mm:ss");
                    System.out.println("Executing Time for task name - " +
                            name + " = " + ft.format(d));
                    // prints the execution time for every task  
                }
                Thread.sleep(1000);
            }
            System.out.println(name + " complete");
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}

public class ThreadPoolDemo {
    // Maximum number of threads in thread pool 
    static final int MAX_T = 3;

    public static void main(String[] args) {
        // creates five tasks 
        Runnable r1 = new Task("task 1");
        Runnable r2 = new Task("task 2");
        Runnable r3 = new Task("task 3");
        Runnable r4 = new Task("task 4");
        Runnable r5 = new Task("task 5");

        // creates a thread pool with MAX_T no. of  
        // threads as the fixed pool size(Step 2) 
        ExecutorService pool = Executors.newFixedThreadPool(MAX_T);

        // passes the Task objects to the pool to execute (Step 3) 
        pool.execute(r1);
        pool.execute(r2);
        pool.execute(r3);
        pool.execute(r4);
        pool.execute(r5);

        // pool shutdown ( Step 4) 
        pool.shutdown();
    }
}
```

Ref: [Thread Pools in Java - GeeksforGeeks](https://www.geeksforgeeks.org/thread-pools-java/){:target="_blank"}

## Executors Thread Pool

類庫提供了一個靈活的線程池實現和一些有用的預設配置。你可以通過調用`Executors`中的某個靜態工廠方法來創建一個線程池：
- `newFixedThreadPool`: 創建一個定長的線程池，每當提交一個任務就創建一個線程，直到達到池的最大長度，這時線程池會保持長度不再變化
  （如果一個線程由於非預期的Exception而結束，線程池會補充一個新的線程）。
- `newCachedThreadPool`: 創建一個可緩存的線程池，如果當前線程池的長度超過了處理的需要時，它可以靈活地回收空閒的線程，
  當需求增加時，它可以靈活地添加新的線程，而並不會對池的長度作任何限制。
- `newSingleThreadExecutor`: 創建一個單線程化的executor，它只創建唯一的工作者線程來執行任務，如果這個線程異常結束，會有另一個取代它。
  executor會保證任務依照任務列隊鎖規定的順序（FIFO、LIFO、優先級）執行。
- `newScheduleThreadPool`: 創建一個定長的線程池，而且支持定時的以及週期性的任務執行，類似於Timer。

`newFixedThreadPool`和`newCachedThreadPool`兩個工廠方法返回通用目的的`ThreadPoolExecutor`實例。
直接使用`ThreadPoolExecutor`，也能創建更滿足某些專有領域的executor。

Ref:
- 【Java并发编程实战】頁120
- [Java多线程学习（八）线程池与Executor 框架 - 知乎](https://zhuanlan.zhihu.com/p/37524061)

## Risks in using Thread Pools

使用線程池的風險

- 死鎖（Deadlock）：儘管死鎖可以在任何多線程程序中發生，但是線程池會引入另一種死鎖情況，在這種情況下，由於線程無法執行，所有正在執行的線程都在等待隊列中被阻塞線程的結果。
- 線程洩漏（Thread Leakage）：如果從線程池中刪除線程以執行任務，但任務完成後沒有返迴線程，則會發生線程洩漏。
  例如，如果線程引發異常，並且池類沒有捕獲此異常，則線程將直接退出，從而將線程池的大小減小一個。如果重複多次，則該池最終將變為空，並且沒有線程可用於執行其他請求。
- 資源釋放（Resource Thrashing）：如果線程池很大，那麼浪費時間在線程之間進行上下文切換。如所解釋的，具有比最佳數量更多的線程可能會導致飢餓問題，從而導致資源崩潰。

重要事項

- 不要將同時等待其他任務結果的任務排隊。如上所述，這可能導致死鎖的情況。
- 使用線程進行長壽命操作時要小心。它可能導致線程永遠等待，並最終導致資源洩漏。
- 線程池必須在末尾顯式結束。如果未完成，則程序將繼續執行，並且永遠不會結束。在池上調用`shutdown()`以結束執行程序。
  如果您嘗試在關閉後將另一個任務發送給執行器，它將拋出`RejectedExecutionException`。
- 需要了解任務以有效地調整線程池。如果任務之間有很大差異，則有必要對不同類型的任務使用不同的線程池，以便對其進行適當的調整。

Ref: [Thread Pools in Java - GeeksforGeeks](https://www.geeksforgeeks.org/thread-pools-java/){:target="_blank"}

## Reference

- [线程池你真不来了解一下吗？ - Java3y - 博客园](https://www.cnblogs.com/Java3y/p/8996365.html){:target="_blank"}
- [理解ThreadPoolExecutor线程池的corePoolSize、maximumPoolSize和poolSize - FrankYou - 博客园](https://www.cnblogs.com/frankyou/p/10135212.html){:target="_blank"}
- [当面试官问线程池时，你应该知道些什么？ - 知乎](https://zhuanlan.zhihu.com/p/62132884){:target="_blank"}
- [Java多线程学习（八）线程池与Executor 框架 - 知乎](https://zhuanlan.zhihu.com/p/37524061){:target="_blank"}

---