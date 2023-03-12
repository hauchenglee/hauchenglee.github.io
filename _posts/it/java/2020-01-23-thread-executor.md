---
layout: post
title: Java - Thread Executor 框架
category: it
tags: [java]
---

## Executor

Executor框架是Java5之後引進的，存在於`java.util.concurrent`包下，
雖然Executor只是個簡單的接口，卻為一個靈活而且強大的框架創造了基礎，並且是【線程池】的基礎。

### 解耦的機制

-> Executor提供了一種將【任務提交】與【任務執行】分離開來的機制（解耦）

`TaskExecutionWebServer`使用`Executor`代替了硬編碼（hard code）創建的線程。
在這個例子中，我們用到了`Executor`標準實現之一，一個定長的線程池，可以容納100個線程。

```
import java.io.IOException;
import java.net.ServerSocket;
import java.net.Socket;
import java.util.concurrent.Executor;
import java.util.concurrent.Executors;

class TaskExecutionWebServer {
    private static final int NTHREADS = 100;
    private static final Executor executor = Executors.newFixedThreadPool(NTHREADS);

    public static void main(String[] args) throws IOException {
        ServerSocket socket = new ServerSocket(80);
        while (true) {
            final Socket connection = socket.accept();
            Runnable task = () -> {
                // handleRequest(connection);
            };
            executor.execute(task);
        }
    }
}
```

通過使用`Executor`，將處理請求任務的提交與它的執行體進行了解耦。

【只要替換一個不同的Executor實現，就可以改變服務器的行為】。改變Executor的實現或者配置，所產生的的影響遠遠小於直接改變任務的執行方式。

這就是解耦。

### 執行策略

【Java并发编程实战】頁118 & 頁119：

將任務的提交與任務的執行體進行解耦，它的價值在於讓你可以簡單地為一個類給定的任務制定執行策略，並且保證後續的修改不至於太困難。

一個執行策略指明了任務執行的【what, where, when, how】幾個因素，具體包括：

- what: 任務在什麼線程中執行
- what: 任務以什麼順序執行（FIFO、LIFO、優先級？）
- how many: 可以有多少個任務並發執行
- how many: 可以有多少個任務進入等待執行列隊
- which: 如果系統過載，需要放棄一個任務，應該挑選哪一個
- how: 另外，如何通知應用程序知道這一切（放棄一個任務）
- what: 在一個任務的執行前與結束後，應該做什麼處理

執行策略是資源管理工具。

最佳策略取決於可用的計算資源和你對服務質量（quality-of-service）的需求。
通過限制並發任務的數量，你能夠確保應用程序不會由於資源耗盡而失敗，大量任務也不會在爭奪稀缺資源時出現性能問題。

將任務的提交與任務的執行策略進行分離，有助於在部署階段選擇一個與當前硬件最匹配的執行策略。

> 無論何時，當你看到這種形式的代碼：
> `new Thread(runnable).start()`
> 並且你可能最終希望獲得一個更加靈活的執行策略時，請使用`Executor`代替`Thread`

## Executor JDK UML

![](https://hauchenglee.github.io/assets/images/it/java/thread-executor-arch.png)

Ref: [线程池你真不来了解一下吗？ - Java3y - 博客园](https://www.cnblogs.com/Java3y/p/8996365.html){:target="_blank"}

### Executor

![](https://hauchenglee.github.io/assets/images/it/java/thread-Executor-jdk.png)

一個接口（interface）內部定義了一個參數為`Runnable`對象的方法`executor`。它用來執行一個任務（task），任務就是一個實現（implement）`Runnable`接口的對象。

一般執行線程任務的方法：

```
Runnable runnable = () -> {};
new Thread(runnableTask).start();
```

但是在Executor中，可以使用Executor而不用顯示地建立線程：

```
executor.execute(new RunnableTask())
```

### ExecutorService

![](https://hauchenglee.github.io/assets/images/it/java/thread-ExecutorService-jdk.png)

> ExecutorService 接口继承自 Executor 接口，它提供了更丰富的实现多线程的方法，比如，ExecutorService 提供了关闭自己的方法，以及可为跟踪一个或多个异步任务执行状况而生成 Future 的方法。
>
> 可以调用 ExecutorService 的 `shutdown()` 方法来平滑地关闭 ExecutorService，调用该方法后，
> 将导致 ExecutorService 停止接受任何新的任务且等待已经提交的任务执行完成。已经提交的任务会分两类：
> - 一类是已经在执行的
> - 另一类是还没有开始执行的
>
> 当所有已经提交的任务执行完毕后将会关闭 ExecutorService。因此我们一般用该接口来实现和管理多线程。
>
> Ref: [并发新特性—Executor 框架与线程池 - Java 并发编程 - 极客学院Wiki](https://wiki.jikexueyuan.com/project/java-concurrency/executor.html){:target="_blank"}

### ExecutorService生命週期

ExecutorService生命週期有三種狀態：
- 運行（running）
- 關閉（shutting down）
- 終止（terminated）

![](https://hauchenglee.github.io/assets/images/it/java/thread-ExecutorService-life-cycle.png)

- `shutdown()`會啟動一個平緩的關閉過程：停止接受新的任務，同時等待已經提交的任務完成，包括尚未開始執行的任務。
- `shutdownNow()`會啟動一個強制的關閉過程：嘗試取消所有運行中的任務和排在列隊中尚未開始的任務。

在關閉後提交到ExecutorService中的任務，會被**拒絕執行處理器**（rejected execution handler）處理。
可能只是簡單地放棄任務，也可能會引起`execute()`拋出一個未檢查（unchecked）的`RejectedExecutionException`。

- 可以調用`awaitTermination()`等待ExecutorService到達終止狀態，通常`shutdown()`會緊隨`awaitTermination()`之後，這樣可以產生同步地關閉ExecutorService的效果。
- 也可以輪流檢查`isTerminated()`判斷是否ExecutorService終止

### Executor JDK

【AbstractExecutorService】：

![](https://hauchenglee.github.io/assets/images/it/java/thread-AbstractExecutorService-jdk.png)

<br>

【ScheduledExecutorService】：

![](https://hauchenglee.github.io/assets/images/it/java/thread-ScheduledExecutorService-jdk.png)

<br>

【ThreadPoolExecutor】：

![](https://hauchenglee.github.io/assets/images/it/java/thread-ThreadPoolExecutor-jdk.png)

<br>

【ScheduledThreadPoolExecutor】：

![](https://hauchenglee.github.io/assets/images/it/java/thread-ScheduledThreadPoolExecutor-jdk.png)

## Compare

### Executor vs ExecutorService

<table>
    <thead>
        <tr>
            <th></th>
            <th>Executor</th>
            <th>ExecutorService</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>繼承關係不同</td>
            <td>父接口</td>
            <td>子接口</td>
        </tr>
        <tr>
            <td>方法參數與返回值不同</td>
            <td>void execute(Runnable task)</td>
            <td>
            Future submit(Callable task);<br>
            Future submit(Runnable task);
            </td>
        </tr>
        <tr>
            <td>是否能控制線程池</td>
            <td>僅有提交任務的功能（execute）</td>
            <td>還具有關閉線程池的功能（shutdown）</td>
        </tr>
    </tbody>
</table>

### Executor vs new Thread()

`new Thread`的缺點如下：
- 每次`new Thread`新建對象效能差
- 線程缺乏統一管理，可能無限制新建線程，造成相互競爭，並且可能佔用過多系統資源，或是`OutOfMemory`
- 缺乏更多功能，如定時、定期執行，或是線程中斷

Java提供的Executor線程池好處：
- 重用存在的線程，減少創建、銷毀的開銷
- 可有效控制最大並發線程數，提高系統資源使用率，同時避免過多資源競爭，避免阻塞
- 提供更多的線程功能，例如排程、並發數控制等功能

## Reference

- [线程池你真不来了解一下吗？ - Java3y - 博客园](https://www.cnblogs.com/Java3y/p/8996365.html){:target="_blank"}
- [java併發程式設計：Executor、Executors、ExecutorService - 程式前沿](https://bit.ly/2NW1Vp9){:target="_blank"}

---