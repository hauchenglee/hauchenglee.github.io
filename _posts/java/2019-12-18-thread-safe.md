---
layout: post
title: Java - Thread Safe 線程安全
category: java
tags: [java]
---

## 何謂線程安全

參考【Java并发编程实战】頁17 & 頁18的定義：

> 線程安全，一個類是線程安全的，是指在被多個線程訪問時，類可以持續進行正確的行為

> 當多個線程訪問一個類時，如果不用考慮這些線程在運行時環境下單調度和交替執行，
> 並且不需要額外的同步措施，及在調用方代碼不必作其他的協調，這個類的行為仍然是
> 正確的，那麼這個類就是線程安全的

<br>

換句話說，就是多線程操作共享數據時，容易造成數據危險。重點在於【多線程】與【共享數據】。

我們可以從上面對於線程安全定義，來推導如何維持最基本的修復：
- 不要跨線程共享數據
- 將狀態變量為不可變

以及最重要的：
- 在任何訪問狀態變量的時候使用**同步**機制

Java中受邀的同步機制是`synchronized`關鍵字，它提供了獨佔鎖。除此之外，術語【同步】還包括`volatile`變量、顯示鎖和原子變量的使用。

總結下，保護線程安全的本質（方法）：
- 不在線程之間共享該狀態變量
- 將狀態變量修改為不可變的變量
- 在訪問狀態變量時使用同步

<br>

名詞定義：
- 對象狀態（state）：就是它的數據，存儲在狀態變量中（state variables）中。一個對象的狀態包含了任何會對它外部可見行為產生影響的數據。
- 共享：指一個變量可以被多個線程訪問
- 可變：指變量的值在其生命週期內可以改變的

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

## 多線程的三個特性

總結：
- 可見性 → 解決【緩存一致性問題】
- 原子性 → 解決【處理器優化問題】
- 重排序 → 解決【指令重排序問題】

Ref:
- [Java的并发编程中的多线程问题到底是怎么回事儿？-HollisChuang's Blog](http://www.hollischuang.com/archives/2618){:target="_blank"}

### 原子性

原子性（atomicity）：指操作是否具有一致性、不可分割性，像原子一樣，既不被中斷操作，要不執行完成，要不就不執行。

例如，之前舉的例子：

```java
import org.apache.http.annotation.NotThreadSafe;

@NotThreadSafe
class UnSafeSequence {
    private int value;
    
    public int getNext() {
        return value++;
    }
}
```

我們可以這樣說：
- `count++`不是原子操作，這是一個讀-改-寫（read-modify-write）的操作
- `a = 1`是原子操作，已經是不可分割的狀態

原子性是有背後的原理在的。

之前提到過【時間片】的概念，這裡貼過來：

> cpu時間片：我們操作系統看起來可以多個程序同時運行，分時操作系統，將系統劃分成相同的時間區域，並分配給一個線程使用。
  當線程還沒有結束，時間片已經過去，該線程只有先停止，等待下一個時間片。cpu運行很快，中間的停頓感覺不出來。

如下如所示：

![](http://www.hauchenglee.com/assets/images/java/thread-time-slice.png)

所以在多線程場景下，由於時間片在線程間輪換，就會發生原子性的問題。

> 假設有操作A和B，如果從執行A的線程的角度看，當其他線程執行B時，要麼B全部執行完成，要麼一點都沒有執行，這樣A和B互為**原子**操作。
>
> 一個原子操作是指：該操作對於所有的操作，包括它自己，都是滿足前面描述的狀態。
>
> 【Java并发编程实战】頁22

<br>

另外，在【Java并发编程实战】頁21也有提到其他例子：惰性初始化（lazy initialization）

以下是`@NotThreadSafe`的寫法：

```java
import org.apache.http.annotation.NotThreadSafe;

import java.util.ArrayList;

@NotThreadSafe
class LazyInitRace {
    private ArrayList instance = null;

    public ArrayList getInstance() {
        if (instance == null) {
            instance = new ArrayList();
        }
        return instance;
    }
}
```

發生原因在於【競爭條件】，也就是後線程的操作，必須依賴於前線程的時序操作，如果後線程檢查`instance`為`null`，兩個`getInstance()`的調用者會得到不同的結果。

<br>

更深入的內容：
- [再有人问你Java内存模型是什么，就把这篇文章发给他。-HollisChuang's Blog](http://www.hollischuang.com/archives/2550){:target="_blank"}
- [深入探讨原子性 - Blog](https://bit.ly/2PLaJzD){:target="_blank"}

### 可見性

可見性（visibility）：指當多個線程訪問同一個變量時，一個線程修改了這個變量的值，其他線程能夠立即看得到修改的值。

Java線程通信是通過共享內存的方式進行通信的，而為了加快執行的速度，線程一般是不會直接操作主內存的，而是操作緩存，也就是自己的工作內存。

![](http://www.hauchenglee.com/assets/images/java/java-memory-model.png)

- JMM中的變量指的是線程共享變量（實例變量instance variable、static字段和數組元素），不包括線程私有變量（局部變量local variable和方法參數）。
- JMM規定線程對變量的寫操作都在自己的本地內存對副本進行，不能直接寫主存中的對應變量。
- 主內存是所有線程共享的Java堆（heap），而工作內存中的共享變量的副本是從主內存拷貝過去的，是線程私有的局部變量，位於Java棧（stack）中。

如果線程對變量的操作沒有刷寫回主內存的話，僅僅改變了自己的工作內存的變量的副本，那麼對於其他線程來說是不可見的。
而如果另一個變量沒有讀取主內存中的新的值，而是使用舊的值，同樣的也可以列為不可見。

綜上，JMM通過控制主存和每個線程的本地工作內存的數據交互，保證一致的內存可見性。

控制何時刷寫到主內存的機制，是基於Java的[happens-before](#happens-before-relationship)原則（先行發生原則），這也跟多線程的有序性相關。

Ref:
- [JAVA多线程——线程安全之原子性，有序性和可见性 - a60782885的博客](https://blog.csdn.net/a60782885/article/details/77803757){:target="_blank"}
- [从多线程的三个特性理解多线程开发 - bigfan - 博客园](https://www.cnblogs.com/dafanjoy/p/10020225.html){:target="_blank"}

### 重排序

重排序（reordering）：指程序執行的時候，程序的代碼執行順序和語句的順序是一致的。

在JMM中為了執行效率與優化考量，編譯器和處理器允許對指令進行重排序，但是重排序過程不會影響到單線程程序的執行，卻會影響到多線程並發執行的正確性。

可以參考Oracle官方對於Happens-before Order: Example 17.4.5-1. Happens-before Consistency舉的例子：[Chapter 17. Threads and Locks](https://docs.oracle.com/javase/specs/jls/se7/html/jls-17.html#jls-17.4.5){:target="_blank"}

### Happens-Before Relationship

Happens-Before，官方簡稱HB。

> 多线程的可见性与有序性之间其实是有联系的，如果程序没有按你希望的顺序执行，那么可见性也就无从谈起。
> JMM(Java 线程内存模型) 中的 happen-before规则，该规则定义了 Java 多线程操作的有序性和可见性，防止了编译器重排序对程序结果的影响。
>
> Ref: [从多线程的三个特性理解多线程开发 - bigfan - 博客园](https://www.cnblogs.com/dafanjoy/p/10020225.html){:target="_blank"}

理解這個還要搭配Java Memory Model（JMM）的內存模型，這部分過於高級，我也無法領悟，先放一些參考鏈接在此，盼以後奇跡能頓悟~
- [Chapter 17. Threads and Locks](https://docs.oracle.com/javase/specs/jls/se7/html/jls-17.html#jls-17.4){:target="_blank"}
- [Java - Understanding Happens-before relationship](https://www.logicbig.com/tutorials/core-java-tutorial/java-multi-threading/happens-before.html){:target="_blank"}
- [Handling Java Memory Consistency with happens-before relationship](https://bit.ly/35IYwAY){:target="_blank"}

## 解決線程安全的問題

使用多線程就一定要保證我們的線程是安全的，這是最重要的地方！

在Java中，我們一般會有下面這麼幾種辦法來實現線程安全：

- 無狀態：沒有共享變量：
- 不變性：使用final使該引用變量不可變（如果該對象引用也引用了其他的對象，那麼無論是發佈或者使用時都需要加鎖）
- 加鎖：內置鎖、顯示Lock鎖
- 使用JDK提供的類來實現線程安全
   - 原子性：使用Atomic來實現原子性
   - 容器：`ConcurrentHashMap`等等
- ...等等

### 原子性

原子性就是執行某一個操作是不可分割的

比如上面所說的`value++`操作，它就不是一個原子性的操作，它是分成三個步驟來完成。
如果該操作是原子性的，那麼就可以說線程安全（因為沒有中間的三部環節，一步到位【原子性】）

JDK中有`atomic`包提供給我們實現原子性操作。

![](http://www.hauchenglee.com/assets/images/java/atomic-jdk.png)

### 可見性

對於可見性，Java提供了一個關鍵字：`volatile`

`volatile`僅僅用來保證該變量對所有線程的可見性，但不保證原子性

- 保證該變量對所有線程的可見性：在多線程的環境下，當這個變量修改時，所有的線程都會知道該變量被修改了，也就是所謂的"可見性"
- 不保證原子性：修改變量（賦值）實質上是在JVM中分了好幾步，而在這幾步內（從加載變量到修改），它是不安全的

使用`volatile`修飾的變量保證了三點：
- 一旦你完成寫入，任何訪問這個字段的線程將會得到最新的值。
- 在你寫入前，會保證所有之前發生的事已經發生，並且任何更新過的數據值也是可見的，因為內存屏障會把之前的寫入值都刷新到緩存。
- `volatile`可以防止重排序。重排序意思是，程序執行的時候，CPU、編譯器可能會對執行順序做一些調整，導致執行的順序並不是從上往下的，
  從而出現了一些意想不到的效果。而如果聲明了`volatile`，那麼CPU、編譯器就會知道這個變量是共享的，不會被緩存在寄存器或者其他不可見的地方。

一般來說，`volatile`大多用於標誌位上（判斷操作），滿足下面的條件才應使用`volatile`修飾變量：
- 修飾變量時不依賴變量的當前值（因為`volatile`是不保證原子性的）
- 該變量不會納入到不變性條件中（該變量是可變的）
- 在訪問變量的時候不需要加鎖（加鎖就沒必要使用`vocatile`這種輕量級同步機制了）

Ref:
- [http://www.cnblogs.com/Mainz/p/3556430.html](http://www.cnblogs.com/Mainz/p/3556430.html){:target="_blank"}
- [https://www.cnblogs.com/Mainz/p/3546347.html](https://www.cnblogs.com/Mainz/p/3546347.html){:target="_blank"}
- [http://www.dataguru.cn/java-865024-1-1.html](http://www.dataguru.cn/java-865024-1-1.html){:target="_blank"}

### 線程封閉

在多線程的環境下，只要我們不使用共享數據，那麼就不會出現線程安全的問題。

以`Servlet`為例，每個線程都擁有自己的變量，互不干擾，並且沒有共享變量，只要保證不在棧（方法）上發佈對象，那麼就是線程安全的。

```
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

class MyServlet extends HttpServlet {
    // 沒有共享變量
    
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 數據全在棧上（方法上）
        super.doGet(req, resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doPost(req, resp);
    }
}
```

### 不變性

【不可變對象一定是線程安全的】

上面我們共享的變量都是可變的，正由於是可變的才會出現線程安全問題。如果該狀態是不可變的，那麼隨便多個線程訪問都是沒有問題的！

Java提供了`final`修飾符給我們使用，但值得說明的是：`final`僅僅是不能修改該變量的引用，但是引用裡邊的數據是可以改的。

舉例來說：

`final HashMap<Person> hashMap = new HashMap<>();`

`HashMap`用`final`修飾了，但是它僅僅保證了該對象引用`hashMap`變量所指向是不可變的，但是`hashMap`內部的數據是可變的，
也就是說：可以`add`、`remove`等等操作到集合中。

不可變的對象引用在使用的時候還是需要加鎖的

- 或者`Person`也設計成是一個線程安全的類
- 因為內部的狀態時可變的，不加鎖或者`Person`不是線程安全類，操作都是有危險的

要想將對象設計成不可變對象，那麼要滿足下面三個條件：
- 對象創建後狀態就不能更改
- 對象所有的域都是`final`修飾的
- 對象是正確創建的（沒有`this`引用逸出）

### 線程安全性委託

有個原則：能使用JDK提供的線程安全機制，就使用JDK的。

![](http://www.hauchenglee.com/assets/images/java/lock-jdk.png)

## Reference

- [Java Concurrency issues and Thread Synchronization - CalliCoder](https://www.callicoder.com/java-concurrency-issues-and-thread-synchronization/){:target="_blank"}
- [JVM中内存模型里的『主内存』是不是就是指『堆』，而『工作内存』是不是就是指『栈』？ - 知乎](https://www.zhihu.com/question/43519009){:target="_blank"}
- [多线程基础必要知识点！看了学习多线程事半功倍](https://bit.ly/2RRZ57q){:target="_blank"}

---