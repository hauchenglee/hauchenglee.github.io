---
layout: post
title: Java - Thread Life Cycle 線程生命週期
category: it
tags: [java]
---

## Overview

In the Java language, multithreading is driven by the core concept of a Thread. 
During their lifecycle, threads go through various states:

![](https://hauchenglee.github.io/assets/images/it/java/Life_cycle_of_a_Thread_in_Java.jpg)

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

```
NEW
```

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

```
RUNNABLE
```

### Blocked

> 處於運行狀態（RUNNABLE）的線程由於某種原因，暫時放棄對CPU的使用權，停止執行，此時進入阻塞狀態，知道其進入到就緒狀態，才有機會再次被CPU調用以進入到運行狀態。
>
> 阻塞狀態分類
> 1. 等待阻塞：運行狀態中的線程執行`wait()`方法，使本線程進入到等待阻塞狀態
> 2. 線程在獲取`synchronized`同步鎖失敗（因為鎖被其它線程佔用），它會進入到同步阻塞狀態
> 3. 其它阻塞：通過調用線程的`sleep()`或`join()`或發出I/O請求時，線程會進入到阻塞狀態。
>    當`sleep()`狀態超時、`join()`等待線程終止或者超時、或者I/O處理完畢時，線程重新轉入就緒狀態。

```
public class BlockedState {
    public static void main(String[] args) throws InterruptedException {
        Thread t1 = new Thread(new DemoThreadB());
        Thread t2 = new Thread(new DemoThreadB());

        t1.start();
        t2.start();

        Thread.sleep(1000);

        System.out.println(t2.getState());
        System.exit(0);
    }
}

class DemoThreadB implements Runnable {

    @Override
    public void run() {
        commonResource();
    }

    public static synchronized void commonResource() {
        while (true) {
            // Infinite loop to mimic heavy processing 't1'
            // won't leave this method
            // when 't2' try to enters this
        }
    }
}
```

1. 我們創建了兩個不同的線程– `t1`和`t2`
2. `t1`開始並進入同步的`commonResource()`方法；這意味著只有一個線程可以訪問它；嘗試訪問此方法的所有其他後續線程將被阻止進一步執行，直到當前線程將完成處理為止
3. 當`t1`進入此方法時，它會無限循環。這只是為了模仿繁重的處理，以便所有其他線程無法進入此方法
4. 現在，當我們啟動t2時，它將嘗試輸入`commonResource()`方法，該方法已被`t1`訪問，因此，`t2`將保持在`BLOCKED`狀態

console:

```
BLOCKED
```

### Waiting

A thread is in *WAITING* state when it's waiting for some other thread to perform a particular action. 
[According to JavaDocs](https://docs.oracle.com/javase/9/docs/api/java/lang/Thread.State.html#WAITING){:target="_blank"}, 
any thread can enter this state by calling any one of following three methods:

1. `Object.wait()`
2. `thread.join()`
3. `LockSupport.park()`

```
public class WaitingState implements Runnable {
    public static Thread t1;

    public static void main(String[] args) {
        t1 = new Thread(new WaitingState());
        t1.start();
    }

    @Override
    public void run() {
        Thread t2 = new Thread(new DemoThreadWS());
        t2.start();

        try {
            t2.join();
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
            System.out.println("Thread interrupted: " + e);
        }
    }
}

class DemoThreadWS implements Runnable {

    @Override
    public void run() {
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
            System.out.println("Thread interrupted: " + e);
        }

        System.out.println(WaitingState.t1.getState());
    }
}
```

1. 我們已經創建並啟動了`t1`
2. `t1`創建一個`t2`並啟動它
3. 在繼續執行`t2`的過程中，我們調用`t2.join()`，這會將`t1`置於*WAITING*狀態，直到`t2`完成執行
4. 由於`t1`等待`t2`完成，因此我們從`t2`調用`t1.getState()`

console:

```
WAITING
```

### Timed Waiting

A thread is in *TIMED_WAITING* state when it's waiting for another thread to perform a particular action within a stipulated amount of time.

[According to JavaDocs](https://docs.oracle.com/javase/9/docs/api/java/lang/Thread.State.html#TIMED_WAITING){:target="_blank"}, 
there are five ways to put a thread on *TIMED_WAITING* state:

1. thread.sleep(long millis)
2. wait(int timeout) or wait(int timeout, int nanos)
3. thread.join(long millis)
4. LockSupport.parkNanos
5. LockSupport.parkUntil

```
public class TimedWaitingState {
    public static void main(String[] args) throws InterruptedException {
        DemoThread obj1 = new DemoThread();
        Thread t1 = new Thread(obj1);
        t1.start();

        // The following sleep will give enough time for ThreadScheduler
        // to start processing of thread t1
        Thread.sleep(1000);
        System.out.println(t1.getState());
    }
}

class DemoThread implements Runnable {

    @Override
    public void run() {
        try {
            Thread.sleep(5000);
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
            System.out.println("Thread interrupted: " + e);
        }
    }
}
```

在這裡，我們創建並啟動了線程`t1`，該線程進入睡眠狀態的超時時間為5秒。

console:

```
TIMED_WAITING
```

### Terminated

This is the state of a dead thread. It's in the *TERMINATED* state when is has either finished execution or was terminated abnormally.

In this article: ([How to Kill a Java Thread](https://www.baeldung.com/java-thread-stop){:target="_blank"} that discusses different ways of stopping the thread.

```
public class TerminatedState implements Runnable {
    public static void main(String[] args) throws InterruptedException {
        Thread t1 = new Thread(new TerminatedState());
        t1.start();

        // The following sleep method will give enough time for
        // thread t1 to complete
        Thread.sleep(1000);
        System.out.println(t1.getState());
    }

    @Override
    public void run() {
        // No processing in this block
    }
}
```

在這裡，當我們啟動線程`t1`下一個語句`Thread.sleep(1000)`為`t1`完成提供了足夠的時間，因此該程序將輸出顯示為：

```
TERMINATED
```

## Reference

- [Life Cycle of a Thread in Java - Baeldung](https://www.baeldung.com/java-thread-lifecycle){:target="_blank"}
- [JAVA线程生命周期 - 简书](https://www.jianshu.com/p/19228c30ffed){:target="_blank"}

---