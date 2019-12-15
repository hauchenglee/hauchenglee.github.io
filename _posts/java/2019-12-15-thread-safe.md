---
layout: post
title: Java筆記-Concurrency 線程安全
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

## 對象的發佈與逸出

- 發佈（publishing）：能夠被當前作用域外的代碼所使用
- 逸出（escape）：一個對象在尚未準備好時就將它發佈

常見*逸出*的有下面幾種方式：
- 靜態域逸出
- `public`修飾的`get`方法
- 方法參數傳遞
- 隱式的`this`

以下分別說明：

- 靜態域逸出

```
// public修飾的靜態域，相當於發佈了這個對象
// person對象也間接發佈了
public static Set<Person> allPerson;

public void init() {
    allPerson = new HashSet<>();
}

// 在未調用init的時候，外面的代碼已經是可以使用allPerson這個對象了，所以已經逸出了
```

- public修飾get方法

```
class Unsafe {
    private String[] states = {};

    public String[] getStates() {
        return states;
    }

    // 本應該是私有的數據，被public修飾的get方法發佈出去了。
    // 任何人都可以通過get方法獲取states（states變量已經超出了當前作用域的範圍 -> 逸出）
    // 也就是說：state已不再安全
}
```

- 方法參數傳遞

因為把對象傳遞過去給另外的方法，已經是逸出了。

- this逸出

```
import java.util.ArrayList;
import java.util.List;

class ThisEscape {
    private final List<Event> listOfEvents;

    public ThisEscape(EventSource source) {
        source.registerListener(new EventListener() {
            @Override
            public void onEvent(Event event) {
                // 在構造方法還沒有完全結束的時候
                // 就調用了該對象的方法doSomething(event)
                doSomething(event);
            }
        });

        // lambda -> method reference
        source.registerListener(this::doSomething);

        // 此時對象還沒有初始化
        listOfEvents = new ArrayList<>();

    }

    void doSomething(Event event) {
        listOfEvents.add(event);
    }

    // 結果，在多線程的環境下，可能還沒有完全初始化listOfEvents對象，
    // 另一個線程就調用了doSomething()方法了，這是不合理的！
    // 這裡就是隱式地this引用逸出
    interface EventSource {
        void registerListener(EventListener eventListener);
    }

    interface EventListener {
        void onEvent(Event event);
    }

    interface Event { }
}
```

### 安全發佈對象

安全發佈對象有幾種常見的方式：
- 在靜態域中直接初始化：`public static Person = new Person();`
   - 靜態初始化由JVM在類的初始階段就執行了，JVM內部存在著同步機制，致使這種方式可以安全發佈對象
- 對應的引用保存到volatile或者AtomicReference引用中
   - 保證了該對象的引用可見性和原子性
- 由final修飾
   - 該對象是不可變的，那麼線程就一定是安全的
- 由鎖來保護
   - 發佈和使用的時候都需要加鎖，這樣才能保證能夠該對象不會逸出
   
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

![](http://www.hauchenglee.com/assets/images/java/atomic-jdk.PNG)

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

参考资料：
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

![](http://www.hauchenglee.com/assets/images/java/lock-jdk.PNG)

## 參考

- [多线程基础必要知识点！看了学习多线程事半功倍](https://bit.ly/2RRZ57q){:target="_blank"}

---