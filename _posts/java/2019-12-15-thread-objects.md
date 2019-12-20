---
layout: post
title: Java - Thread Objects 線程對象
category: java
tags: [java]
---

## Create and Starting a Thread

### extends Thread

```java
class ThreadExample extends Thread {
    public static void main(String[] args) {
        System.out.println("Inside : " + Thread.currentThread().getName());

        System.out.println("Creating thread...");
        Thread thread = new ThreadExample();

        System.out.println("Starting thread...");
        thread.start();
    }

     // run() method contains the code that is executed by the thread.
    @Override
    public void run() {
        System.out.println("Inside : " + Thread.currentThread().getName());
    }    
}
```

```
# Output
Inside : main
Creating thread...
Starting thread...
Inside : Thread-0
```

- `Thread.currentThread()` return a reference to the thread that is currently executing.
- `getName()` method to print the name of the current thread.

### implement Runnable

```java
class RunnableExample implements Runnable {
    public static void main(String[] args) {
        System.out.println("Inside : " + Thread.currentThread().getName());

        System.out.println("Creating Runnable...");
        Runnable runnable = new RunnableExample();

        System.out.println("Creating Thread...");
        Thread thread = new Thread(runnable);

        System.out.println("Starting Thread...");
        thread.start();
    }

    @Override
    public void run() {
        System.out.println("Inside : " + Thread.currentThread().getName());
    }
}
```

```
# Output
Inside : main
Creating Runnable...
Creating Thread...
Starting Thread...
Inside : Thread-0
```

<br>

You can also use [anonymous class](https://docs.oracle.com/javase/tutorial/java/javaOO/anonymousclasses.html){:target="_blank"} syntax.

```java
class RunnableExampleAnonymousClass implements Runnable {
    public static void main(String[] args) {
        System.out.println("Inside : " + Thread.currentThread().getName());

        System.out.println("Creating Runnable...");
        Runnable runnable = new Runnable() {
            @Override
            public void run() {
                System.out.println("Inside : " + Thread.currentThread().getName());
            }
        };

        System.out.println("Creating Thread...");
        Thread thread = new Thread(runnable);

        System.out.println("Starting Thread...");
        thread.start();
    }

    @Override
    public void run() {
        System.out.println("Inside : " + Thread.currentThread().getName());
    }
}
```

<br>

The above example can be made even shorter by using Java 8's [lambda expression](https://www.callicoder.com/java-lambda-expression-tutorial/){:target="_blank"}

```java
class RunnableExampleLambdaExpression implements Runnable {
    public static void main(String[] args) {
        System.out.println("Inside : " + Thread.currentThread().getName());

        System.out.println("Creating Runnable...");
        Runnable runnable = () -> System.out.println("Inside : " + Thread.currentThread().getName());

        System.out.println("Creating Thread...");
        Thread thread = new Thread(runnable);

        System.out.println("Starting Thread...");
        thread.start();
    }

    @Override
    public void run() {
        System.out.println("Inside : " + Thread.currentThread().getName());
    }
}
```

## Analysis of source code

`Runnable`僅定義了一個抽象方法`run()`

```java
package java.lang;

/**
 * The <code>Runnable</code> interface should be implemented by any
 * class whose instances are intended to be executed by a thread. The
 * class must define a method of no arguments called <code>run</code>.
 * <p>
 * This interface is designed to provide a common protocol for objects that
 * wish to execute code while they are active. For example,
 * <code>Runnable</code> is implemented by class <code>Thread</code>.
 * Being active simply means that a thread has been started and has not
 * yet been stopped.
 * <p>
 * In addition, <code>Runnable</code> provides the means for a class to be
 * active while not subclassing <code>Thread</code>. A class that implements
 * <code>Runnable</code> can run without subclassing <code>Thread</code>
 * by instantiating a <code>Thread</code> instance and passing itself in
 * as the target.  In most cases, the <code>Runnable</code> interface should
 * be used if you are only planning to override the <code>run()</code>
 * method and no other <code>Thread</code> methods.
 * This is important because classes should not be subclassed
 * unless the programmer intends on modifying or enhancing the fundamental
 * behavior of the class.
 *
 * @author  Arthur van Hoff
 * @see     java.lang.Thread
 * @see     java.util.concurrent.Callable
 * @since   JDK1.0
 */
@FunctionalInterface
public interface Runnable {
    /**
     * When an object implementing interface <code>Runnable</code> is used
     * to create a thread, starting the thread causes the object's
     * <code>run</code> method to be called in that separately executing
     * thread.
     * <p>
     * The general contract of the method <code>run</code> is that it may
     * take any action whatsoever.
     *
     * @see     java.lang.Thread#run()
     */
    public abstract void run();
}
```

目的：
- 限定`Thread`構造方法的形參類型（針對方式2`implement Runnable`）
- 將`run()`向上抽取，做成抽象方法，讓實現類去重寫（為什麼？）

<br>

來看看`Thread`類的源碼解析：

```java
public class Thread implements Runnable {
    /**
     * Allocates a new {@code Thread} object. This constructor has the same
     * effect as {@linkplain #Thread(ThreadGroup,Runnable,String) Thread}
     * {@code (null, null, gname)}, where {@code gname} is a newly generated
     * name. Automatically generated names are of the form
     * {@code "Thread-"+}<i>n</i>, where <i>n</i> is an integer.
     */
    public Thread() {
        init(null, null, "Thread-" + nextThreadNum(), 0);
    }

    /**
     * Allocates a new {@code Thread} object. This constructor has the same
     * effect as {@linkplain #Thread(ThreadGroup,Runnable,String) Thread}
     * {@code (null, target, gname)}, where {@code gname} is a newly generated
     * name. Automatically generated names are of the form
     * {@code "Thread-"+}<i>n</i>, where <i>n</i> is an integer.
     *
     * @param  target
     *         the object whose {@code run} method is invoked when this thread
     *         is started. If {@code null}, this classes {@code run} method does
     *         nothing.
     */
    public Thread(Runnable target) {
        init(null, target, "Thread-" + nextThreadNum(), 0);
    }

    /**
     * Causes this thread to begin execution; the Java Virtual Machine
     * calls the <code>run</code> method of this thread.
     * <p>
     * The result is that two threads are running concurrently: the
     * current thread (which returns from the call to the
     * <code>start</code> method) and the other thread (which executes its
     * <code>run</code> method).
     * <p>
     * It is never legal to start a thread more than once.
     * In particular, a thread may not be restarted once it has completed
     * execution.
     *
     * @exception  IllegalThreadStateException  if the thread was already
     *               started.
     * @see        #run()
     * @see        #stop()
     */
    public synchronized void start() {
        /**
         * This method is not invoked for the main method thread or "system"
         * group threads created/set up by the VM. Any new functionality added
         * to this method in the future may have to also be added to the VM.
         *
         * A zero status value corresponds to state "NEW".
         */
        if (threadStatus != 0)
            throw new IllegalThreadStateException();

        /* Notify the group that this thread is about to be started
         * so that it can be added to the group's list of threads
         * and the group's unstarted count can be decremented. */
        group.add(this);

        boolean started = false;
        try {
            start0();
            started = true;
        } finally {
            try {
                if (!started) {
                    group.threadStartFailed(this);
                }
            } catch (Throwable ignore) {
                /* do nothing. If start0 threw a Throwable then
                  it will be passed up the call stack */
            }
        }
    }

    /**
     * If this thread was constructed using a separate
     * <code>Runnable</code> run object, then that
     * <code>Runnable</code> object's <code>run</code> method is called;
     * otherwise, this method does nothing and returns.
     * <p>
     * Subclasses of <code>Thread</code> should override this method.
     *
     * @see     #start()
     * @see     #stop()
     * @see     #Thread(ThreadGroup, Runnable, String)
     */
    @Override
    public void run() {
        if (target != null) {
            target.run();
        }
    }    
}
```

結合創建線程的兩種方法，背後程序運行的方式：

![](http://www.hauchenglee.com/assets/images/java/thread-01.jpg)

![](http://www.hauchenglee.com/assets/images/java/thread-02.jpg)

<br>

所以，該何如理解上面那兩句話呢？

### reason 1

- 限定`Thread`構造方法的形參類型（針對方式2`implement Runnable`）

把`Runnable`想成教派（形而上的教派），只有我大R家的人，才是一家人，一家人就是要整整齊齊。看看下面的代碼：

```
Runnable runnable = new RunnableExample();
Thread thread = new Thread(runnable);

// Thread's constructor
public Thread(Runnable target) {
    init(null, target, "Thread-" + nextThreadNum(), 0);
}

thread.start();
```

- 首先，R家建立一個任務（task）叫作`runnable`，任務內容寫在`run()`裡面
- 他派出手下的`Thread`交給他這個task（使用`constructor`傳遞任務）
- 手下的`Thread`收到後先檢查是不是R家的任務，形參必須是`Runnable`或其底下家人才能收下
- 執行`thread.start()`後將會調用`Thread`自家的`run()`方法

> Thread的有參構造函數**只允許**接受Runnable的實現類對象，包括Thread子類對象<br>
>
> 觀察源碼，我們發現Thread也實現了Runnable

![](http://www.hauchenglee.com/assets/images/java/thread-03.jpg)

就像我先前說的，`Thread`這個家族入口很嚴格，只有身為`Runnable`宗族的人，才能進來的。

<br>

### reason 2

- 將`run()`向上抽取，做成抽象方法，讓實現類去重寫（為什麼？）

![](http://www.hauchenglee.com/assets/images/java/thread-04.jpg)

觀察上圖，可以發現：

- 一個線程運行，總是由`start()`開始，因為它才是開啟線程的鑰匙，並且開始後JVM會調用`run()`方法執行

```
* Causes this thread to begin execution; the Java Virtual Machine calls the <code>run</code> method of this thread.
```

- `run()`的本質只是為了"包裹"需要線程執行的代碼塊

```
* If this thread was constructed using a separate Runnable run object, then that Runnable object's run method is called;
*
* otherwise, this method does nothing and returns.
*
* Subclasses of Thread should override this method.
```

來解讀上面那段話：

> 如果這個線程由`Runnable`建立，則調用`Runnable`對象的`run`方法；
>
> 否則（如果是`Thread`建立的線程），則`override`父類的`run`方法

結合圖片以及說明，可以發現在上編程時，執行線程的包裹只有黃色虛線框內的代碼，當線程被`start()`喚醒後，只會去調用`Thread`的`run()`：
- 這個`run()`可能來自`Thread`類（方式2）
- 也可能來自`Thread`的子類對象（方式1）

換言之，`Thread`類（及其子類）是線程運行的入口，沒了`Thread`以後，`Runnable`及其實現類（`implement class`）就無法調用`run`方法。

![](http://www.hauchenglee.com/assets/images/java/thread-05.jpg)

> `Thread`类及其子类永远是入口，方式2写在`Runnable`实现类中代码之所以能被执行到，仅仅是因为`Thread`的`run()`中调用了`target.run()`。

## Compare

所以說，該如何選擇呢？

1. 通過`Thread`類來擴展（`extends`）線程沒有充分運用面向對象的思想，因為一旦從`Thead`繼承了，就無法再繼承其他類（Java不允許多重繼承）。

2. 如果遵循良好的設計規範，繼承（`extends`）是用於擴展父類的功能，但是當我們創建線程時，我們不會去擴展`Thread`類的功能，而只是使用提供的`run()`方法的實現。

3. 解耦執行者（`thread`）與被執行者（`run()`）兩者，將【待執行代碼】移到`Runnable`實現類中，達到【資源共享】的目的。

![](http://www.hauchenglee.com/assets/images/java/thread-06.jpg)

> 注意，继承`Thread`方式並沒有做到资源共享，因为每个子类对象都有各自的一份`run()`，各玩各的。

<br>

多個線程共享資源：

```java
public class ThreadForIncrease {
    private static int cnt = 0; // public data

    public static void main(String[] args) throws InterruptedException {
        Runnable runnable = () -> {
            int n = 10000;
            while (n > 0) {
                cnt++;
                n--;
            }
        };

        Thread thread1 = new Thread(runnable);
        Thread thread2 = new Thread(runnable);
        Thread thread3 = new Thread(runnable);
        Thread thread4 = new Thread(runnable);
        Thread thread5 = new Thread(runnable);
        thread1.start();
        thread2.start();
        thread3.start();
        thread4.start();
        thread5.start();

        Thread.sleep(1000);
        System.out.println(cnt);
    }
}
```

```
# Output
第一次執行：17727
第二次執行：16156
第三次執行：16290
第四次執行：15378
第五次執行：17358
```

## Reference

- [Java Thread and Runnable Tutorial - CalliCoder](https://www.callicoder.com/java-multithreading-thread-and-runnable-tutorial/){:target="_blank"}
- [多线程初级（上） - 知乎](https://zhuanlan.zhihu.com/p/56518031){:target="_blank"}

---