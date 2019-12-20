---
layout: post
title: Java - Concurrency 並發編程
category: java
tags: [java]
---

## 基礎知識

用多線程只有一個目的：更好的利用cpu資源。

更詳細的說：
- 系統需要多功能處理，開啟進程非常耗費資源
- 進程間的通訊很不方便，因為大多數操作系統不允許進程訪問其他進程的內存空間
- 因為多線程程序是亂序執行，每次執行的結果都是隨機的。因此，只有亂序執行的代碼才有必要設計為多線程。

關於線程的一些概念：

- cpu時間片：我們操作系統看起來可以多個程序同時運行，分時操作系統，將系統劃分成相同的時間區域，並分配給一個線程使用。
  當線程還沒有結束，時間片已經過去，該線程只有先停止，等待下一個時間片。cpu運行很快，中間的停頓感覺不出來。
- 多線程：指的是這個程序（一個進程）運行時產生了不止一個線程（比如：下載程序，開啟多個線程同時運行）。
- 並行：多個cpu實例或者多台機器同時執行一段處理邏輯，是真正的同時。
- 並發：通過cpu調度算法，讓用戶看上去同時執行，實際上從cpu操作層面不是真正的同時。並
  發往往在場景中有公用的資源，那麼針對這個公用的資源往往產生瓶頸，我們會用TPS（Transactions Per Second）或QPS（Query Per Second）來反應這個系統的處理能力。
  （See [【Teradata】系统吞吐量重要参数QPS（TPS）、并发数、响应时间 - 李子恒 - 博客园](https://www.cnblogs.com/badboy200800/p/11178044.html){:target="_blank"}）
- 線程安全：經常用來描繪一段代碼。指在並發的情況下，該代碼經過多線程使用，線程的調度順序不影響任何結果。這個時候使用多線程，
  我們只需要關注系統的內存，cpu是不是夠用即可。反過來，線程不安全意味著線程的調度順序會影響最終結果。
- 同步：通過人為的控制和調度，保證共享資源的多線程訪問成為線程安全，來保證結果的準確。

線程安全的優先級 > 性能。

Ref: [Java多线程的实现 - - SegmentFault 思否](https://segmentfault.com/a/1190000014543489){:target="_blank"}

## 並發與並行

並發，指的是多個事情，在同一時間段內同時發生。<br>
並行，指的是多個事情，在同一時間點上同時發生。<br>

並發的多個任務之間是互相搶佔資源。<br>
並行的多個任務之間是不互相搶佔資源。<br>

只有在多cpu的情況中，才會發生並行。否則，看似同時發生的事情，其實都是並發執行的。

![](http://www.hauchenglee.com/assets/images/java/concurrent-parallel.png)

Ref: [面试必考的：并发和并行有什么区别？](https://mp.weixin.qq.com/s?__biz=MzI4Njg5MDA5NA==&mid=2247485178&idx=1&sn=0fc0fd1bb7e8b41ec8770ac5a62f8287&chksm=ebd747fbdca0ceedc6e0dab24d449ecdfa5b4e582d2acdc62c234b299c5828abe047f76196f4&token=1230572157&lang=zh_CN###rd){:target="_blank"}

## 進程與線程

進程：

進程是程序的一次執行過程，是系統運行程序的基本單位，因此進程是動態的。系統運行一個程序即是一個進程從創建、運行到消亡的
過程。簡單來說，一個進程就是一個執行中的程序，它在計算機中一個指令接著一個指令的運行著，同時，每個進程還佔有某些系統資
源如cpu時間、內存空間、文件設備的使用權等等。換句話說，當程序在執行時，將會被操作系統載入內存。

線程：

線程與進程相似，但線程是一個比進程更小的執行單位，一個進程在其執行的過程中可以產生多個線程。與進程不同的是同類的多個線
程共享同一塊內存空間和一組系統資源，所以系統在產生一個線程，或是在各個線程之間作切換工作時，負擔要比進程小得多。

用比喻的方式說明：

> 1。单进程单线程：一个人在一个桌子上吃菜。<br>
> 2。单进程多线程：多个人在同一个桌子上一起吃菜。<br>
> 3。多进程单线程：多个人每个人在自己的桌子上吃菜。<br>
>
> 多线程的问题是多个人同时吃一道菜的时候容易发生争抢，例如两个人同时夹一个菜，一个人刚伸
> 出筷子，结果伸到的时候已经被夹走菜了。。。此时就必须等一个人夹一口之后，在还给另外一个
> 人夹菜，也就是说资源共享就会发生冲突争抢。
>

Ref: [多线程有什么用？ - 知乎](https://www.zhihu.com/question/19901763){:target="_blank"}

## 同步與異步

消息通信機制：
- 同步（synchronous communication）：需要等待request回調
- 異步（asynchronous communication）：不需等待request回調

Ref: [怎样理解阻塞非阻塞与同步异步的区别？ - 知乎](https://www.zhihu.com/question/19732473){:target="_blank"}

## Java與多線程關係

單線程方法調用鏈（main）

```
class Demo1 {
    public static void main(String[] args) {
        m1();
    }

    public static void m1() {
        m2();
        m3();
    }

    public static void m2() {
    }

    public static void m3() {
    }
}
```

![](http://www.hauchenglee.com/assets/images/java/java-disable-thread.png)

<br>

開啟多線程

```
class Demo2 implements Runnable {
    public static void main(String[] args) {
        Demo2 demo2 = new Demo2();
        Thread thread = new Thread(demo2);
        thread.start();
    }

    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName()); // Thread-0
    }
}
```

![](http://www.hauchenglee.com/assets/images/java/java-enable-thread.png)

## 參考

架構：

- [Lesson: Concurrency (The Java™ Tutorials > Essential Classes)](https://docs.oracle.com/javase/tutorial/essential/concurrency/index.html){:target="_blank"}
- [Java Concurrency / Multithreading Basics - CalliCoder](https://www.callicoder.com/java-concurrency-multithreading-basics/){:target="_blank"}
- [《Java并发编程实战》高清PDF电子书 百度云网盘免费下载 - Java SE免费电子书 - Java电子书大全 - Powered by Discuz!](https://www.ebook23.com/thread-78-1-1.html){:target="_blank"}

思維導圖：

- [java并发编程思维导图 - 简书](https://www.jianshu.com/p/5b60f89ad7fd){:target="_blank"}
- [Java初学有必要深入多线程编程吗？ - 知乎](https://www.zhihu.com/question/60148851/answer/670934093){:target="_blank"}
- [JavaGuide/多线程系列.md at master · Snailclimb/JavaGuide](https://github.com/Snailclimb/JavaGuide/blob/master/docs/java/%E5%A4%9A%E7%BA%BF%E7%A8%8B%E7%B3%BB%E5%88%97.md){:target="_blank"}

基礎：

- [多线程初级（上） - 知乎](https://zhuanlan.zhihu.com/p/56518031){:target="_blank"}
- [多线程初级（中） - 知乎](https://zhuanlan.zhihu.com/p/57482990){:target="_blank"}
- [Java 程序中的多线程](https://www.ibm.com/developerworks/cn/java/multithreading/){:target="_blank"}

線程池：

- [线程池你真不来了解一下吗？ - Java3y - 博客园](https://www.cnblogs.com/Java3y/p/8996365.html){:target="_blank"}
- [理解ThreadPoolExecutor线程池的corePoolSize、maximumPoolSize和poolSize - FrankYou - 博客园](https://www.cnblogs.com/frankyou/p/10135212.html){:target="_blank"}
- [当面试官问线程池时，你应该知道些什么？ - 知乎](https://zhuanlan.zhihu.com/p/62132884){:target="_blank"}
- [Java多线程学习（八）线程池与Executor 框架 - 知乎](https://zhuanlan.zhihu.com/p/37524061){:target="_blank"}

面試題目：

- [《面试-多线程》 - java思维导图社区](https://www.java-mindmap.com/view/71){:target="_blank"}

---