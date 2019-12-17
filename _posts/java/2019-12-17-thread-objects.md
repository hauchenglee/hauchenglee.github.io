---
layout: post
title: Java筆記 - Thread Objects 線程對象
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
> 因為觀察源碼，我們發現Thread也實現了Runnable

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




![](http://www.hauchenglee.com/assets/images/java/thread-05.jpg)



![](http://www.hauchenglee.com/assets/images/java/thread-06.jpg)



## Compare


## Reference

---