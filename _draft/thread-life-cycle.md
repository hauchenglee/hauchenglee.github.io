---
layout: post
title: Java - Thread Life Cycle 線程生命週期
category: java
tags: [java]
---

## Overview

In the Java language, multithreading is driven by the core concept of a Thread. 
During their lifecycle, threads go through various states:

![](http://www.hauchenglee.com/assets/images/java/Life_cycle_of_a_Thread_in_Java.jpg)

Ref: [Life Cycle of a Thread in Java - Baeldung](https://www.baeldung.com/java-thread-lifecycle)

## Life Cycle of a Thread

The `java.lang.Thread` class contains a *static State enum* – which defines its potential states. 
During any given point of time, the thread can only be in one of these states:

1. **NEW**: newly created thread that has not yet started the execution
2. **RUNNABLE**: either running or ready for execution but it's waiting for resource allocation
3. **BLOCKED**: waiting to acquire a monitor lock to enter or re-enter a synchronized block/method
4. **WAITING**: waiting for some other thread to perform a particular action without any time limit
5. **TIMED_WAITING**: waiting for some other thread to perform a specific action for a specific period
6. **TERMINATED**: has completed its execution

### New

> 當線程對象創建後，並且尚未啟動線程，即進入新建狀態，直到使用`start()`方法啟動它為止。

```
Runnable runnable = new NewState();
Thread t = new Thread(runnable);
log.info(t.getState());
```

console:

```NEW```

### Runnable

> 當調用線程對象的`start()`方法時，線程即進入就緒狀態（`RUNNABLE`）。處於就緒狀態的線程只是說明此線程已經做好準備，
隨時等待CPU調度執行，並不是說執行了`start()`方法就立刻執行。

In a multi-threaded environment, the Thread-Scheduler (which is part of JVM) allocates a fixed 
amount of time to each thread. So it runs for a particular amount of time, then relinquishes the 
control to other *RUNNABLE* threads.

在多線程環境中，線程調度程序（JVM的一部分）為每個線程分配固定的時間量。因此它運行特定的時間，然後將控件放棄給其他RUNNABLE線程。

```
Runnable runnable = new NewState();
Thread t = new Thread(runnable);
t.start();
log.info(t.getState());
```

console:

```RUNNABLE```

### Blocked

> 

```BLOCKED```

### Waiting



### Timed Waiting



### Terminated



## Reference

- [Life Cycle of a Thread in Java - Baeldung](https://www.baeldung.com/java-thread-lifecycle){:target="_blank"}
- [JAVA线程生命周期 - 简书](https://www.jianshu.com/p/19228c30ffed){:target="_blank"}

---