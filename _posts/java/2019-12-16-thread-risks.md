---
layout: post
title: Java筆記 - Risks of Thread 線程的風險
category: java
tags: [java]
---

## 線程的風險

### 線程安全危險

線程安全的問題是微妙且出乎意料的，因為在沒有進行充分同步的情況下，多線程中的各個操作的順序是不可預測的，也就是容易造成線程安全問題。

簡單舉個例子：

下面的程序在單線程中運行是沒有問題的。

```
import org.apache.http.annotation.NotThreadSafe;

@NotThreadSafe
class UnSafeSequence {
    private int value;
    
    public int getNext() {
        return value++;
    }
}
```

但是在多線程環境中，它的`value`值計算就會出現問題。

首先，它共享了`value`這個變量，其次來說`value++`是一個組合的操作（注意，它並非是原子性）

它分成3個獨立的操作：
1. 讀取這個值
2. 使之加1
3. 再寫入新值

以下圖示很好表示線程操作順序：

![](http://www.hauchenglee.com/assets/images/java/not-thread-safe-value-count.png)

- 當線程A讀取到`value`值是9的時候，**同時**線程B也執行此方法並且也是讀取`value`值是9
- 兩個線程的操作都對`value`加上1
- 將計算結果寫入`value`上，但卻是寫入到`value`的結果是10
- 也就是說：兩個線程的互相操作，正確結果應該返回11，而它返回了9，這就是線程不安全

> 圖表中描述了不同線程之間的交替操作。在這些圖表中，時間由左至右發展，每一行表現一個不同線程的活動。
> 這些交替的圖表通常用來描述最壞的情況，目的是表現特定順序下產生錯誤僭越帶來的危險。
>
> 事實上，最壞的情況可能比圖中表現的更糟糕，因為存在重排序的可能。

`UnsafeSequence`闡明了一種常見的並發危險：**競爭條件**（race condition）。

因為線程共享相同的內存地址空間，且並發地運行，它們可能訪問或修改其他線程正在使用的變量。
好處是當數據共享相對於其他的線程間通訊機制都更加簡單，但是這其中也存在著巨大的風險：當數據意外改變時，線程可能會出現混亂。

為了使多線程程序的行為可預見性，訪問共享資源必須經過合理的協調，才不會造成線程相互干擾。

```
import org.apache.http.annotation.GuardedBy;
import org.apache.http.annotation.ThreadSafe;

@ThreadSafe
class Sequence {
    @GuardedBy("this") private int value;

    public synchronized int getNext() {
        return value++;
    }
}
```

### 活躍度危險

在並發開發代碼時，對線程安全的關注是至關重要的：安全不能妥協。安全的重要性不僅僅存在多線程程序中，單線程的程序也必須注意保護安全性和正確性。
然而，線程的引入和使用造成另一形式的活躍度危險（liveness failure），這不會出現在單線程的程序中。

當一個活動進入某種永遠無法再繼續執行的狀態時，活躍度失敗就發生了。

就比如`Servlet`，一個`Servlet`對象可以處理多個請求，所以可以說`Servlet`是支持多線程的程序。

一樣舉個例子：

```
import javax.servlet.*;
import java.io.IOException;

class UnsafeCountingServlet extends GenericServlet implements Servlet {
    private long count = 0;

    public long getCount() {
        return count;
    }

    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        count++;
    }
}
```

依照上面說過的，上面這個類是線程不安全的。最簡單的方式：在`service`方法上加上內置鎖（synchronized），就可以實現線程安全。

```
import javax.servlet.*;
import java.io.IOException;

class UnsafeCountingServlet extends GenericServlet implements Servlet {
    private long count = 0;

    public long getCount() {
        return count;
    }

    @Override
    public synchronized void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        count++;
    }
}
```

雖然實現了線程安全了，但是這會帶來很嚴重的性能問題：

- 每個請求都得等待上一個請求的`service`方法處理了以後才可以執行操作。

也就是我們只是完成一個小小功能，使用多線程目的是想要提高效率，卻沒有把握得當，帶來嚴重的性能問題。

更嚴重的話，還會帶來各種活躍度危險，包括：
- 死鎖（deadlock）
- 飢餓（starvation）
- 活鎖（livelock）

### 性能危險

在設計良好的應用程序中使用線程，能夠獲得純粹的性能收益，但是線程仍然會給運行時帶來一定成程度的開銷。

上下文切換（context switches）————當調度程序臨時掛起當前運行的線程時，另一個線程開始運行————這在多個
線程組成的應用程序中是很頻繁的，並且帶來巨大的系統開銷：保存和恢復線程執行的上下文，離開執行現場，並且CPU
的時間會花費在對線程的調度而不是在運行上。

性能問題涉及很多方面，包括服務時間、響應性、吞吐量、資源消耗或者可伸縮性的不良表現。

## 線程安全危險

### 線程干擾錯誤

線程干擾錯誤（Thread interference errors / Race Conditions）

簡單來說就是多線程操作共享資源，造成數據錯誤。用簡單代碼舉例：

這是簡單`class`：

```java
class Counter {
    int count = 0;

    public void increment() {
        count = count + 1;
    }

    public int getCount() {
        return count;
    }
}
```

現在我們使用多線程調用`increment()`：

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.TimeUnit;

class RaceConditionExample {
    public static void main(String[] args) throws InterruptedException {
        ExecutorService executorService = Executors.newFixedThreadPool(10);

        Counter counter = new Counter();

        for (int i = 0; i < 1000; i++) {
            executorService.submit(() -> counter.increment());
        }
        
        executorService.shutdown();
        executorService.awaitTermination(60, TimeUnit.SECONDS);

        System.out.println("Final count is : " + counter.getCount());
    }
}
```

看起來最後`count`計算後應該是1000，然而每次運行時，它都給出不一致的結果，例如992、996和993。

解法有：
- 加上同步鎖
- 原子性操作

### 內存一致性錯誤

內存一致性錯誤（Memory consistency errors）

說明：

程序在操作數據時是有先後順序的。當一個線程操作更新完共享數據時，此時的操作結果並沒有一併更新到其他線程，導致該線程讀取到舊的數據值，造成錯誤。

發生這個情況有很多複雜原因。除了編譯器對程序進行優化，以提高效能外，它還可能對指令操作進行重排序，達到更好的效能。
例如，處理器可能會從寄存器（包含變量的最後讀取值）而不是主內存器（具有變量的最新值）讀取變量的當前值。

```java
class MemoryConsistencyErrorExample {
    private static boolean sayHello = false;

    public static void main(String[] args) throws InterruptedException {

        Thread thread = new Thread(() -> {
            while (!sayHello) {
            }

            System.out.println("Hello World!");

            while (sayHello) {
            }

            System.out.println("Good Bye!");
        });

        thread.start();

        Thread.sleep(1000);
        System.out.println("Say Hello..");
        sayHello = true;

        Thread.sleep(1000);
        System.out.println("Say Bye..");
        sayHello = false;
    }
}
```

正常情況下，我們預期出現的結果是：

```
# Ideal Output
Say Hello..
Hello World!
Say Bye..
Good Bye!
```

然而，實際上輸出結果卻是：
```
# Actual Output
Say Hello..
Say Bye..
```

除此之外，還可以發現程序一直在運行，無法終止。

原因就是：第一個線程不知道主線程對`sayHello`變量所做的更改。

解決辦法：可以使用`volatile`來避免內存一致性錯誤。

## Reference

- [Java Concurrency issues and Thread Synchronization - CalliCoder](https://www.callicoder.com/java-concurrency-issues-and-thread-synchronization/){:target="_blank"}
- [JVM中内存模型里的『主内存』是不是就是指『堆』，而『工作内存』是不是就是指『栈』？ - 知乎](https://www.zhihu.com/question/43519009){:target="_blank"}

---