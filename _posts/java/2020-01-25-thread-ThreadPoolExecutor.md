---
layout: post
title: Java - ThreadPoolExecutor
category: java
tags: [java]
---

## ThreadPoolExecutor

在`java.uitl.concurrent.ThreadPoolExecutor`類是Executor框架中最核心的類。

先來看頂部注釋：

![](https://www.hauchenglee.com/assets/images/java/thread-ThreadPoolExecutor-jdk-detail.png)

Ref: [线程池你真不来了解一下吗？ - Java3y - 博客园](https://www.cnblogs.com/Java3y/p/8996365.html){:target="_blank"}

### 構造方法與參數說明

ThreadPoolExecutor有四個構造方法，前三個都是基於第四個實現。

![](https://www.hauchenglee.com/assets/images/java/thread-ThreadPoolExecutor-constructor-arch.png)

第四個構造方法定義如下：

七個參數重點：
1. 指定核心線程數量
2. 指定最大線程數量
3. 允許線程空閒時間
4. 時間對象
5. 阻塞列隊
6. 線程工廠
7. 任務拒絕策略

```
/**
  * 用给定的初始参数创建一个新的ThreadPoolExecutor。

  * @param keepAliveTime 当线程池中的线程数量大于corePoolSize的时候，如果这时没有新的任务提交，
  * 核心线程外的线程不会立即销毁，而是会等待，直到等待的时间超过了keepAliveTime；
  * @param unit  keepAliveTime参数的时间单位
  * @param workQueue 等待队列，当任务提交时，如果线程池中的线程数量大于等于corePoolSize的时候，把该任务封装成一个Worker对象放入等待队列；
  * 
  * @param threadFactory 执行者创建新线程时使用的工厂
  * @param handler RejectedExecutionHandler类型的变量，表示线程池的饱和策略。
  * 如果阻塞队列满了并且没有空闲的线程，这时如果继续提交任务，就需要采取一种策略处理该任务。
  * 线程池提供了4种策略：
     1.AbortPolicy：直接抛出异常，这是默认策略；
     2.CallerRunsPolicy：用调用者所在的线程来执行任务；
     3.DiscardOldestPolicy：丢弃阻塞队列中靠最前的任务，并执行当前任务；
     4.DiscardPolicy：直接丢弃任务；
  */
 public ThreadPoolExecutor(int corePoolSize,
                           int maximumPoolSize,
                           long keepAliveTime,
                           TimeUnit unit,
                           BlockingQueue<Runnable> workQueue,
                           ThreadFactory threadFactory,
                           RejectedExecutionHandler handler) {
     if (corePoolSize < 0 ||
         maximumPoolSize <= 0 ||
         maximumPoolSize < corePoolSize ||
         keepAliveTime < 0)
         throw new IllegalArgumentException();
     if (workQueue == null || threadFactory == null || handler == null)
         throw new NullPointerException();
     this.corePoolSize = corePoolSize;
     this.maximumPoolSize = maximumPoolSize;
     this.workQueue = workQueue;
     this.keepAliveTime = unit.toNanos(keepAliveTime);
     this.threadFactory = threadFactory;
     this.handler = handler;
 }
```

【參數說明】：

- `corePoolSize`: 線程池的基本線程數。這個參數跟後面講述的線程池的實現原理有非常大的關係。在創建了線程池後，默認情況下，
  線程池中並沒有任何線程，而是等待有任務到來才創建線程去執行任務，除非調用了`prestartAllCoreThreads()`或者`prestartCoreThread()`方法，
  從這兩個方法的名字就可以看出，是預創建線程的意思，即在沒有任務到來之前就創建corePoolSize個線程或者一個線程。默認情況下，
  在創建了線程池後，線程池中的線程數為0，當有任務來之後，就會創建一個線程去執行任務，當線程池中的線程數目達到corePoolSize後，
  就會把到達的任務放到緩存列隊當中。
- `maximumPoolSize`: 線程池允許創建的最大線程數。如果列隊滿了，並且已創建的線程數小於最大線程數，則線程池會再創建新的線程執行任務。
  值得注意的是如果使用了無界的任務列隊，這個參數就沒什麼效果。
- `keepAliveTime`: 線程活動保持時間。線程池的工作線程空閒後，保持存活的時間，所以如果任務很多，並且每個任務執行時間比較短，可以調大這個時間，提高線程的利用率。
- `unit`: 參數keepAliveTime的時間單位，有七種取值。
   - DAYS
   - HOURS
   - MINUTES
   - MILLISECONDS
   - MICROSECONDS
   - NANOSECONDS
- `workQueue`: 任務列隊。用於保存等待執行的任務的阻塞列隊，可以選擇以下幾個阻塞列隊：
   - `ArrayBlockingQueue`: 是一個基於數組結構的有界阻塞列隊，此列隊按FIFO（先進先出）原則對元素進行排序。
   - `LinkedBlockingQueue`: 一個基於鏈表結果的阻塞列隊，此列隊按照FIFO（先進先出）排序元素，吞吐量通常高於ArrayBlockingQueue。
     靜態工廠方法`Executors.newFixedThreadPool()`使用了這個列隊。
   - `SynchronousQueue`: 一個不儲存元素的阻塞列隊。每個插入操作必須等到另一個線程調用移除操作，否則插入操作一直處於阻塞狀態。吞吐量通常要高於LinkedBlockingQueue。
     靜態工廠方法`Executors.newCachedThreadPool`使用了這個列隊。
   - `PriorityBlockingQueue`: 一個具有優先級的無限阻塞列隊。
- `threadFactory`: 創建線程的工廠。可以通過線程工廠給每個創建出來的線程設置更有意義的名字。
- `handler`: 飽和策略。當列隊和線程都滿了，說明線程池處於飽和狀態，那麼必須採取一種策略處理移交的新任務。這個策略默認情況下是AbortPolicy，表示無法處理新任務時拋出異常。
  以下是JDK1.5提供的四種策略：
   - `AbortPolicy`: 直接拋出異常。
   - `CallerRunsPolicy`: 只用調用者所在線程來運行任務。
   - `DiscardOldestPolicy`: 不處理，丟棄掉。
   - 當然也可以根據應用場景需求來實現`RejectedExecutionHandler`接口自定義策略。如記錄日誌或持久化不能處理的任務。

<br>

![](https://www.hauchenglee.com/assets/images/java/thread-ThreadPoolExecutor-summary.png)

Ref:
- [线程池你真不来了解一下吗？ - Java3y - 博客园](https://www.cnblogs.com/Java3y/p/8996365.html){:target="_blank"}
- [当面试官问线程池时，你应该知道些什么？ - 知乎](https://zhuanlan.zhihu.com/p/62132884){:target="_blank"}

### 線程池狀態

線程池的5種狀態：Running、ShutDown、Stop、Tidying、Terminated

<br>

![](https://www.hauchenglee.com/assets/images/java/thread-ThreadPoolExecutor-state-doc.png)

> 变量ctl定义为AtomicInteger，记录了“线程池中的任务数量”和“线程池的状态”两个信息。

![](https://www.hauchenglee.com/assets/images/java/thread-ThreadPoolExecutor-state-code.png)

Ref: [线程池你真不来了解一下吗？ - Java3y - 博客园](https://www.cnblogs.com/Java3y/p/8996365.html){:target="_blank"}

線程池各個狀態切換框架圖：

![](https://www.hauchenglee.com/assets/images/java/thread-ThreadPoolExecutor-state-arch.jpg)

Ref: [Java多线程线程池（4）--线程池的五种状态_Runner的博客-CSDN博客](https://blog.csdn.net/L_kanglin/article/details/57411851){:target="_blank"}

線程池的狀態：
> - `RUNNING`: 线程池能够接受新任务，以及对新添加的任务进行处理。
> - `SHUTDOWN`: 线程池不可以接受新任务，但是可以对已添加的任务进行处理。
> - `STOP`: 线程池不接收新任务，不处理已添加的任务，并且会中断正在处理的任务。
> - `TIDYING`: 当所有的任务已终止，ctl记录的"任务数量"为0，线程池会变为TIDYING状态。当线程池变为TIDYING状态时，会执行钩子函数`terminated()`。
>   `terminated()`在ThreadPoolExecutor类中是空的，若用户想在线程池变为TIDYING时，进行相应的处理；可以通过重载`terminated()`函数来实现。
> - `TERMINATED`: 线程池彻底终止的状态。

整理成表格：

<table>
    <thead>
        <tr>
            <th>狀態</th>
            <th>高三位</th>
            <th>未添加的任務</th>
            <th>工作列隊workers中的任務</th>
            <th>阻塞列隊worksQueue中的任務</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>RUNNING</td>
            <td>111</td>
            <td>添加</td>
            <td>繼續處理</td>
            <td>繼續處理</td>
        </tr>
        <tr>
            <td>SHUTDOWN</td>
            <td>000</td>
            <td>不添加</td>
            <td>繼續處理</td>
            <td>繼續處理</td>
        </tr>
        <tr>
            <td>STOP</td>
            <td>001</td>
            <td>不添加</td>
            <td>嘗試中斷</td>
            <td>不處理</td>
        </tr>
        <tr>
            <td>TIDYING</td>
            <td>010</td>
            <td>不添加</td>
            <td>處理完了</td>
            <td>
            如果由SHUTDOWN -> TIDYING，那就是處理完了<br>
            如果由STOP -> TIDYING，那就是不處理
            </td>
        </tr>
        <tr>
            <td>TERMINATED</td>
            <td>011</td>
            <td>同TIDYING</td>
            <td>同TIDYING</td>
            <td>同TIDYING</td>
        </tr>        
    </tbody>
</table>

Ref: [线程池你真不来了解一下吗？ - Java3y - 博客园](https://www.cnblogs.com/Java3y/p/8996365.html){:target="_blank"}

## 線程池方法介紹

### execute

`execute()`方法實際上是Executor中聲明的方法，在ThreadPoolExecutor進行了具體的實現，這個方法是ThreadPoolExecutor的核心方法，
通過這個方法可以向線程池提交一個任務，交由線程池去執行。

慣例來看JDK：

```
    /**
     * Executes the given task sometime in the future.  The task
     * may execute in a new thread or in an existing pooled thread.
     *
     * If the task cannot be submitted for execution, either because this
     * executor has been shutdown or because its capacity has been reached,
     * the task is handled by the current {@code RejectedExecutionHandler}.
     *
     * @param command the task to execute
     * @throws RejectedExecutionException at discretion of
     *         {@code RejectedExecutionHandler}, if the task
     *         cannot be accepted for execution
     * @throws NullPointerException if {@code command} is null
     */
    public void execute(Runnable command) {
        if (command == null)
            throw new NullPointerException();
        /*
         * Proceed in 3 steps:
         *
         * 1. If fewer than corePoolSize threads are running, try to
         * start a new thread with the given command as its first
         * task.  The call to addWorker atomically checks runState and
         * workerCount, and so prevents false alarms that would add
         * threads when it shouldn't, by returning false.
         *
         * 2. If a task can be successfully queued, then we still need
         * to double-check whether we should have added a thread
         * (because existing ones died since last checking) or that
         * the pool shut down since entry into this method. So we
         * recheck state and if necessary roll back the enqueuing if
         * stopped, or start a new thread if there are none.
         *
         * 3. If we cannot queue task, then we try to add a new
         * thread.  If it fails, we know we are shut down or saturated
         * and so reject the task.
         */
        int c = ctl.get();
        if (workerCountOf(c) < corePoolSize) {
            if (addWorker(command, true))
                return;
            c = ctl.get();
        }
        if (isRunning(c) && workQueue.offer(command)) {
            int recheck = ctl.get();
            if (! isRunning(recheck) && remove(command))
                reject(command);
            else if (workerCountOf(recheck) == 0)
                addWorker(null, false);
        }
        else if (!addWorker(command, false))
            reject(command);
    }
```

這個方法中的重點都在方法裡的注釋中：

```
/*
 * Proceed in 3 steps:
 *
 * 1. If fewer than corePoolSize threads are running, try to
 * start a new thread with the given command as its first
 * task.  The call to addWorker atomically checks runState and
 * workerCount, and so prevents false alarms that would add
 * threads when it shouldn't, by returning false.
 *
 * 2. If a task can be successfully queued, then we still need
 * to double-check whether we should have added a thread
 * (because existing ones died since last checking) or that
 * the pool shut down since entry into this method. So we
 * recheck state and if necessary roll back the enqueuing if
 * stopped, or start a new thread if there are none.
 *
 * 3. If we cannot queue task, then we try to add a new
 * thread.  If it fails, we know we are shut down or saturated
 * and so reject the task.
 */
```

就不翻譯說明了，對照上面的資訊，思考後理解。

### submit

`submit()`方法是在ExecutorService中聲明的方法，在AbstractExecutorService就已經有了具體的實現，在ThreadPoolExecutor中並沒有對其進行重寫。

這個方法也是用來向線程池提交任務的，但是它和`execute()`方法不同，它能夠返回任務執行的結果，
去看`submit()`方法的實現，會發現它實際上還是調用`execute()`方法，只不過它利用了Future來獲取任務執行結果。

```
    /**
     * @throws RejectedExecutionException {@inheritDoc}
     * @throws NullPointerException       {@inheritDoc}
     */
    public Future<?> submit(Runnable task) {
        if (task == null) throw new NullPointerException();
        RunnableFuture<Void> ftask = newTaskFor(task, null);
        execute(ftask);
        return ftask;
    }

    /**
     * @throws RejectedExecutionException {@inheritDoc}
     * @throws NullPointerException       {@inheritDoc}
     */
    public <T> Future<T> submit(Runnable task, T result) {
        if (task == null) throw new NullPointerException();
        RunnableFuture<T> ftask = newTaskFor(task, result);
        execute(ftask);
        return ftask;
    }

    /**
     * @throws RejectedExecutionException {@inheritDoc}
     * @throws NullPointerException       {@inheritDoc}
     */
    public <T> Future<T> submit(Callable<T> task) {
        if (task == null) throw new NullPointerException();
        RunnableFuture<T> ftask = newTaskFor(task);
        execute(ftask);
        return ftask;
    }
```

### shutdown & shutdownNow

引用自：[线程池你真不来了解一下吗？ - Java3y - 博客园](https://www.cnblogs.com/Java3y/p/8996365.html){:target="_blank"}

【shutdown()】

![](https://www.hauchenglee.com/assets/images/java/thread-shutdown-jdk.png)

【shutdownNow()】

![](https://www.hauchenglee.com/assets/images/java/thread-shutdown-now-jdk.png)

<br>

> 区别：
> - 调用`shutdown()`后，线程池状态立刻变为`SHUTDOWN`，而调用`shutdownNow()`，线程池状态立刻变为`STOP`。
> - `shutdown()`等待任务执行完才中断线程，而`shutdownNow()`不等任务执行完就中断了线程。

## Executors vs ThreadPoolExecutor

這裡先簡單的說明使用兩者創建線程池有啥區別：
- Executors: 簡便的線程池工廠方法
- ThreadPoolExecutor: 較複雜，但可以細節調控

Oracle官方建議使用`Executors`創建線程池，但《阿里巴巴Java开发手册》卻建議使用`ThreadPoolExecutor`創建線程池，差別在於可以更細節調控效率效能。

Ref: 
- [ThreadPoolExecutor (Java Platform SE 8 )](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ThreadPoolExecutor.html){:target="_blank"}
- [Java Thread Pool - ThreadPoolExecutor Example - HowToDoInJava](https://howtodoinjava.com/java/multi-threading/java-thread-pool-executor-example/){:target="_blank"}
- [JUC学习笔记--从阿里Java开发手册学习线程池的正确创建方法 - JavaNoob - 博客园](https://www.cnblogs.com/javanoob/p/threadpool.html){:target="_blank"}

## Reference

- [线程池你真不来了解一下吗？ - Java3y - 博客园](https://www.cnblogs.com/Java3y/p/8996365.html){:target="_blank"}
- [当面试官问线程池时，你应该知道些什么？ - 知乎](https://zhuanlan.zhihu.com/p/62132884){:target="_blank"}
- [Java多线程学习（八）线程池与Executor 框架 - 知乎](https://zhuanlan.zhihu.com/p/37524061){:target="_blank"}

---