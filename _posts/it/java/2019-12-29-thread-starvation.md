---
layout: post
title: Java - Thread Starvation 飢餓
category: it
tags: [java]
---

## Starvation and Livelock

> Starvation and livelock are much less common a problem than deadlock, but are still problems that every designer of 
> concurrent software is likely to encounter.
>
> Ref: [Starvation and Livelock (The Java™ Tutorials > Essential Classes > Concurrency)](https://docs.oracle.com/javase/tutorial/essential/concurrency/starvelive.html){:target="_blank"}

## Starvation

> *Starvation* describes a situation where a thread is unable to gain regular access to shared resources and is unable to 
> make progress. This happens when shared resources are made unavailable for long periods by "greedy" threads. For 
> example, suppose an object provides a synchronized method that often takes a long time to return. If one thread invokes 
> this method frequently, other that also need frequent synchronized access to the same object will often be blocked.

飢餓描述了一種情況，即線程無法獲得對共享資源的常規訪問並且無法取得進展。當"貪婪"線程使共享資源長時間不可用時，
就會發生這種情況。例如，假設一個對象提供了一個同步方法，該方法通常需要很長時間才能返回。如果一個線程頻繁調用
此方法，則也需要頻繁同步訪問同一對象的其他線程將經常被阻塞。

<br>

> 线程饥饿问题其实指的公平性问题，意思是多个线程都在执行任务，但是只有一个cpu，如果想要大家都有机会执行自己的任务，
> 那么必须是每个人执行一会之后，让出资源让别人执行，谁都不能一直占着cpu，如果某个线程一直占着cpu，
> 那么造成的结果就是别的线程一直没有机会运行，从而导致饿死。
>
> 举个真实世界的例子：
>
> 每到过年的时候，大家都要买火车票回家，但是呢票的总数是有限的，当票一放出来的时候，黄牛通过不正当的方式，一下抢到了80%的票源，
> 从而导致有很大一部分人买不到票，最后无奈要么不回家了，要么改换日期或者其他交通工具，
> 这其实就是一个典型的公平性问题，公平指的是让人人都有机会而不是让某一个人垄断。
>
> Ref: [关于线程死锁，活锁和饥饿问题 - 云+社区 - 腾讯云](https://cloud.tencent.com/developer/article/1161103){:target="_blank"}

<br>

【Java并发编程实战】頁218：

當線程訪問它所需要的資源時卻永久拒絕，以至於不能再繼續進行，這樣就發生了飢餓（starvation）。

- 在Java應用程序中，使用線程的**優先級**不當可能引起飢餓。
- 在鎖中執行**無終止的建構**也可能引起飢餓（無限循環，或者無盡等待資源），因為其他需要這個鎖的線程永遠不可能得到它。

線程API定義的線程優先級僅僅作為調度的參考。通常情況下，抵制住改變線程優先級的誘惑是明智的。
只要你開始改變線程的優先級，程序的行為就變為與平台相關的了，增加平台的依賴性，並且你會引入飢餓發生的風險。

大多數並發應用程序可以對所有線程使用相同的優先級。

---