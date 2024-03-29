---
layout: post
title: Java - Thread Callable & Future
category: it
tags: [java]
---

## Callable & Future

> There are two wats of creating threads – one by extending the Thread class and other by creating a thread with a Runnable. 
> However, one feature lacking in Runnable is that we cannot make a thread return result when it terminates, i.e. when `run()` completes. 
> For supporting this feature, the Callable interface is present in Java.
>
> Ref: [Callable and Future in Java - GeeksforGeeks](https://www.geeksforgeeks.org/callable-future-java/){:target="_blank"}

有兩種創建線程的方法-一種是通過擴展Thread類，另一種是通過使用Runnable創建線程。
但是，Runnable缺少的一項功能是，當線程終止（即`run()`完成）時，我們無法使線程返回結果。
為了支持此功能，Java中提供了Callable接口。

### Runnable JDK

其中Runnable應該是我們最熟悉的接口，它只有一個`run()`方法，用於執行線程任務，該函數沒有返回值。

Thread類在呼叫`start()`函數後，執行的是Runnable`run()`函數。

```java
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

### Callable JDK

> `Callable` interface has the `call()` method. In this method, we have to implement the logic of a task. The `Callable` 
> interface is a parameterized interface, meaning we have to indicate the type of data the `call()` method will return.

`Callable`接口定義了`call()`方法。在這個方法中，我們西部實現任務的邏輯。該`Callable`接口是一個參數化接口，這意味著我們必須指示該`call()`方法將返回的數據類型。

<br>

Callable與Runnable的功能大致相似，Callable中有一個`call()`函數，但是`call()`函數有返回值，而Runnable的`run()`函數不能將結果返回給使用者程序。

用簡單話來說，Callable就是Runnable具有返回值的擴展。

這是一個泛型接口，`call()`返回的型別就是使用者傳遞來的`V`型別。

```java
package java.util.concurrent;

/**
 * A task that returns a result and may throw an exception.
 * Implementors define a single method with no arguments called
 * {@code call}.
 *
 * <p>The {@code Callable} interface is similar to {@link
 * java.lang.Runnable}, in that both are designed for classes whose
 * instances are potentially executed by another thread.  A
 * {@code Runnable}, however, does not return a result and cannot
 * throw a checked exception.
 *
 * <p>The {@link Executors} class contains utility methods to
 * convert from other common forms to {@code Callable} classes.
 *
 * @see Executor
 * @since 1.5
 * @author Doug Lea
 * @param <V> the result type of method {@code call}
 */
@FunctionalInterface
public interface Callable<V> {
    /**
     * Computes a result, or throws an exception if unable to do so.
     *
     * @return computed result
     * @throws Exception if unable to compute a result
     */
    V call() throws Exception;
}
```

### Future JDK

> `Future` interface has methods to obtain the result generated by a `Callable` object and to manage its state.

`Future`接口具有獲取對像生成的結果`Callable`並管理其狀態的方法。

<br>

Executor就是Runnable和Callable的排程容器，Future就是對於具體的Runnable或者Callable任務的執行結果進行取消、查詢是否完成、獲取結果、設定結果操作。

主要作用：獲取任務執行結果，中斷任務等（代表的是任务的生命周期）。

在Future中有三個比較重要的方法：
- `public boolean cancel(boolean mayInterruptIfRunning)`: 用於停止任務。
   - 如果尚未啟動，它將停止執行任務。
   - 如果已經啟動，則僅在`mayInterruptIfRunning`為true時才會中斷任務。
- `public Object get() throws InterruptedException, ExecutionException`: 用於獲取任務的結果。
   - 如果任務完成，它將立即返回結果，
   - 否則將等待任務完成，然後返回結果。
- `public boolean isDone()`: 如果任務完成，則返回true，否則返回false。

```java
package java.util.concurrent;

/**
 * A {@code Future} represents the result of an asynchronous
 * computation.  Methods are provided to check if the computation is
 * complete, to wait for its completion, and to retrieve the result of
 * the computation.  The result can only be retrieved using method
 * {@code get} when the computation has completed, blocking if
 * necessary until it is ready.  Cancellation is performed by the
 * {@code cancel} method.  Additional methods are provided to
 * determine if the task completed normally or was cancelled. Once a
 * computation has completed, the computation cannot be cancelled.
 * If you would like to use a {@code Future} for the sake
 * of cancellability but not provide a usable result, you can
 * declare types of the form {@code Future<?>} and
 * return {@code null} as a result of the underlying task.
 *
 * <p>
 * <b>Sample Usage</b> (Note that the following classes are all
 * made-up.)
 * <pre> {@code
 * interface ArchiveSearcher { String search(String target); }
 * class App {
 *   ExecutorService executor = ...
 *   ArchiveSearcher searcher = ...
 *   void showSearch(final String target)
 *       throws InterruptedException {
 *     Future<String> future
 *       = executor.submit(new Callable<String>() {
 *         public String call() {
 *             return searcher.search(target);
 *         }});
 *     displayOtherThings(); // do other things while searching
 *     try {
 *       displayText(future.get()); // use future
 *     } catch (ExecutionException ex) { cleanup(); return; }
 *   }
 * }}</pre>
 *
 * The {@link FutureTask} class is an implementation of {@code Future} that
 * implements {@code Runnable}, and so may be executed by an {@code Executor}.
 * For example, the above construction with {@code submit} could be replaced by:
 *  <pre> {@code
 * FutureTask<String> future =
 *   new FutureTask<String>(new Callable<String>() {
 *     public String call() {
 *       return searcher.search(target);
 *   }});
 * executor.execute(future);}</pre>
 *
 * <p>Memory consistency effects: Actions taken by the asynchronous computation
 * <a href="package-summary.html#MemoryVisibility"> <i>happen-before</i></a>
 * actions following the corresponding {@code Future.get()} in another thread.
 *
 * @see FutureTask
 * @see Executor
 * @since 1.5
 * @author Doug Lea
 * @param <V> The result type returned by this Future's {@code get} method
 */
public interface Future<V> {

    /**
     * Attempts to cancel execution of this task.  This attempt will
     * fail if the task has already completed, has already been cancelled,
     * or could not be cancelled for some other reason. If successful,
     * and this task has not started when {@code cancel} is called,
     * this task should never run.  If the task has already started,
     * then the {@code mayInterruptIfRunning} parameter determines
     * whether the thread executing this task should be interrupted in
     * an attempt to stop the task.
     *
     * <p>After this method returns, subsequent calls to {@link #isDone} will
     * always return {@code true}.  Subsequent calls to {@link #isCancelled}
     * will always return {@code true} if this method returned {@code true}.
     *
     * @param mayInterruptIfRunning {@code true} if the thread executing this
     * task should be interrupted; otherwise, in-progress tasks are allowed
     * to complete
     * @return {@code false} if the task could not be cancelled,
     * typically because it has already completed normally;
     * {@code true} otherwise
     */
    boolean cancel(boolean mayInterruptIfRunning);

    /**
     * Returns {@code true} if this task was cancelled before it completed
     * normally.
     *
     * @return {@code true} if this task was cancelled before it completed
     */
    boolean isCancelled();

    /**
     * Returns {@code true} if this task completed.
     *
     * Completion may be due to normal termination, an exception, or
     * cancellation -- in all of these cases, this method will return
     * {@code true}.
     *
     * @return {@code true} if this task completed
     */
    boolean isDone();

    /**
     * Waits if necessary for the computation to complete, and then
     * retrieves its result.
     *
     * @return the computed result
     * @throws CancellationException if the computation was cancelled
     * @throws ExecutionException if the computation threw an
     * exception
     * @throws InterruptedException if the current thread was interrupted
     * while waiting
     */
    V get() throws InterruptedException, ExecutionException;

    /**
     * Waits if necessary for at most the given time for the computation
     * to complete, and then retrieves its result, if available.
     *
     * @param timeout the maximum time to wait
     * @param unit the time unit of the timeout argument
     * @return the computed result
     * @throws CancellationException if the computation was cancelled
     * @throws ExecutionException if the computation threw an
     * exception
     * @throws InterruptedException if the current thread was interrupted
     * while waiting
     * @throws TimeoutException if the wait timed out
     */
    V get(long timeout, TimeUnit unit)
        throws InterruptedException, ExecutionException, TimeoutException;
}
```

## Example

這是一個Java Callable任務的簡單示例，該示例返回一秒鐘後執行該任務的線程的名稱。我們使用Executor框架並行執行100個任務，
並使用Java Future獲取提交的任務的結果。

示例出處：[Java Callable Future Example - JournalDev](https://www.journaldev.com/1090/java-callable-future-example){:target="_blank"}

```java
import java.util.ArrayList;
import java.util.Date;
import java.util.List;
import java.util.concurrent.*;

public class MyCallable implements Callable<String> {

    @Override
    public String call() throws Exception {
        Thread.sleep(1000);

        // return the thread name executing this callable task
        return Thread.currentThread().getName();
    }

    public static void main(String[] args) {
        // Get ExecutorService from Executors utility class, thread pool size is 10
        ExecutorService executor = Executors.newFixedThreadPool(10);

        // create a list to hold the Future object associated with Callable
        List<Future<String>> list = new ArrayList<Future<String>>();

        // Create MyCallable instance
        Callable<String> callable = new MyCallable();

        for (int i = 0; i < 100; i++) {
            // submit Callable tasks to be executed by thread pool
            Future<String> future = executor.submit(callable);

            // add Future to the list, we can get return value using Future
            list.add(future);
        }
        for (Future<String> fut : list) {
            try {
                // print the return value of Future, notice the output delay in console 
                // because Future.get() waits for task to get completed
                System.out.println(new Date() + "::" + fut.get());
            } catch (InterruptedException | ExecutionException e) {
                e.printStackTrace();
            }
        }
        // shut down the executor service now
        executor.shutdown();
    }

}
```

> Once we execute the above program, you will notice the delay in output because java Future `get()` method waits for the java callable task to complete. 
> Also notice that there are only 10 threads executing these tasks.

一旦執行了上述程序，您將注意到輸出的延遲，因為java Future `get()`方法等待java可調用的任務完成。另請注意，只有10個線程執行這些任務。

console:

```console
Mon Dec 31 20:40:15 PST 2012::pool-1-thread-1
Mon Dec 31 20:40:16 PST 2012::pool-1-thread-2
Mon Dec 31 20:40:16 PST 2012::pool-1-thread-3
Mon Dec 31 20:40:16 PST 2012::pool-1-thread-4
Mon Dec 31 20:40:16 PST 2012::pool-1-thread-5
Mon Dec 31 20:40:16 PST 2012::pool-1-thread-6
Mon Dec 31 20:40:16 PST 2012::pool-1-thread-7
Mon Dec 31 20:40:16 PST 2012::pool-1-thread-8
Mon Dec 31 20:40:16 PST 2012::pool-1-thread-9
Mon Dec 31 20:40:16 PST 2012::pool-1-thread-10
Mon Dec 31 20:40:16 PST 2012::pool-1-thread-2
...
```

## Reference

- [Callable and Future in Java - GeeksforGeeks](https://www.geeksforgeeks.org/callable-future-java/){:target="_blank"}
- [Java Callable Future Example - JournalDev](https://www.journaldev.com/1090/java-callable-future-example){:target="_blank"}
- [Java Callable Future Example - HowToDoInJava](https://howtodoinjava.com/java/multi-threading/java-callable-future-example/){:target="_blank"}
- [Java中的Runnable，Callable，Future，FutureTask的比較 - 程式前沿](https://bit.ly/2tOqFsz){:target="_blank"}

---