---
layout: post
title: Java - Livelock 活鎖
category: java
tags: [java]
---

## Livelock

> A thread often acts in response to the action of another thread. If the other thread's action is also response to the action 
> of another thread, then *livelock* may result. As with deadlock, livelocked threads are unable to make further progress. 
> However, the threads are not blocked — they are simply too busy responding to each other to resume work. This is 
> comparable to two people attempting to pass each other in a corridor: Alphonse moves to his left to let Gaston pass, while 
> Gaston moves to his right to let Alphonse pass. Seeing that they are still blocking each other, Alphonse moves to his right, 
> while Gaston moves to his left. They're sill blocking each other, so...
>
> Ref: [Starvation and Livelock (The Java™ Tutorials > Essential Classes > Concurrency)](https://docs.oracle.com/javase/tutorial/essential/concurrency/starvelive.html){:target="_blank"}

一個線程通常會響應另一個線程的操作而行動。如果另一個線程的動作也是對另一個線程的動作的響應，則可能會導致活鎖。
與死鎖一樣，活動鎖定的線程無法取得進一步的進展。但是，線程沒有被阻塞-它們只是太忙於彼此響應而無法恢復工作。
這相當於兩個人試圖在走廊中互相經過：阿方斯（Alphonse）向左移動以讓加斯頓（Gaston）通過，而格斯頓（Gaston）向右移動以讓Alphonse通過。
看到他們仍然互相阻擋，Alphonse向右移動，而Gaston向左移動。他們仍然互相阻礙，所以...

<br>

> 活锁与死锁恰恰相反，从字面上理解，死锁是因为两个线程相互等待此时他们的状态都是Blocked，因为阻塞住了所以可以理解成僵死。
> 
> 活锁指的是，两个线程都是处于活越状态（Runnable），但是呢两个线程分别相互推脱任务，导致线程繁忙，最终程序无法继续向前运行。
>
> 举个真实世界的例子：
>
> 两个人在一个狭窄的胡同的相遇，但是呢两个人都很有礼貌，都彼此先让对方先过，所以一直同时从这边移到那边，从那边移到这边，
> 虽然还在活动，但是却忙于彼此礼让对方，最终导致谁也不能干的别的事。
>
> Ref: [关于线程死锁，活锁和饥饿问题 - 云+社区 - 腾讯云](https://cloud.tencent.com/developer/article/1161103){:target="_blank"}

<br>

【Java并发编程实战】頁219：

活鎖（livelock）是線程中活躍度失敗的另一種形式，儘管沒有被阻塞，線程仍然不能繼續，因為它不斷重試相同的操作，卻總是失敗。

解決活鎖的一種方案就是對重試機制引入一些隨機性。

例如：在以太網絡上，兩個機站嘗試使用相同的載波發送數據包，包會發生衝突。機站發現了衝突，並且都在稍後重發。
如果它們都非常精確地在一秒後重發，它們又會發生衝突，並且不斷衝突下去，導致數據包永遠不能發送，即使有大量的寬帶都是閒置的。

為了避免這種情況發生，我們通過使用一個隨機組件讓它們進行等待。（以太協議在重複發生衝突時，同樣包含一個指數的撤銷協議，減少了擁塞和多個衝突的機站重複失敗的風險。）

在並發程序中，通過隨機等待和撤回來進行重試，能夠相當有效地避免活鎖的發生。

---