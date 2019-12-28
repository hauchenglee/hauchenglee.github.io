---
layout: post
title: Java - Thread Deadlock 死鎖
category: java
tags: [java]
---

## 死鎖

【Java并发编程实战】頁205：

> 當一個線程永遠佔有一個鎖，而其他線程嘗試去獲得這個鎖，那麼它們將永遠被阻塞。

<br>

[Deadlock (The Java™ Tutorials > Essential Classes > Concurrency)](https://docs.oracle.com/javase/tutorial/essential/concurrency/deadlock.html){:target="_blank"}

> *Deadlock* describe a situation where two or more threads are blocked forever, waiting for each other.

<br>

與許多其他的並發危險相同，死鎖很少能被立即發現。一個類如果有發生死鎖的潛在可能並不意味著死鎖**每次都將**發生，它只發生在該發生的時候。
當死鎖出現的時候，往往是遇到了最不幸的時候————在高負載之下。

## 各種死鎖

### 鎖順序死鎖

鎖順序死鎖（Lock-ordering deadlocks）原因：兩個線程試圖通過不同的順序獲得多個相同的鎖。如果請求鎖的順序相同，就不會出現循環的鎖依賴對象，也就不會產生死鎖了。

例如：若能夠保證同時請求鎖`L`和鎖`M`的每一個線程，都是按照從`L`到`M`的順序，那麼就不會發生死鎖了。

DeadLock Example:

```java
public class LeftRightDeadlock {
    private final Object left = new Object();
    private final Object right = new Object();

    public void leftRight() {
        synchronized (left) {
            synchronized (right) {
                // doSth();
            }
        }
    }

    public void rightLeft() {
        synchronized (right) {
            synchronized (left) {
                // doSth();
            }
        }
    }

}
```

- 線程A調用`leftRight()`方法，得到`left`鎖
- **同時**線程B調用`rightLeft()`方法，得到`right`鎖
- 線程A和線程B都繼續執行：
   - 此時線程A需要`right`鎖才能繼續往下執行
   - 此時線程B需要`left`鎖才能繼續往下執行
- 但是：
   - 線程A的`left`鎖並沒有得到釋放
   - 線程B的`right`鎖也沒有釋放
- 所以他們都只能等待，而這種等待是無期限的 -> 永久等待 -> 死鎖

![](http://www.hauchenglee.com/assets/images/java/unlucky-timing-in-left-right-deadlock.png)

### 動態的鎖順序死鎖

動態的鎖順序死鎖（Dynamic lock order deadlocks）

有一個需求：把資金從一個賬戶轉入另一個賬戶。在執行轉賬前要獲得`Account`對象的鎖，確保操作是原子性，以及保護業務邏輯（例如賬戶餘額不能為負數）。

```java
class DynamicLockOrderDeadlocks {
    public void transferMoney(Account fromAccount, Account toAccount, DollarAmount amount) throws InsufficientFundsException {
        synchronized (fromAccount) {
            synchronized (toAccount) {
                if (fromAccount.getBalance().compareTo(amount) < 0) throw new InsufficientFundsException();
                else {
                    fromAccount.debit(amount);
                    toAccount.credit(amount);
                }
            }
        }
    }
}
```

事實上，上鎖的順序取決於傳遞給`transferMoney`的參數順序，這個參數的順序實際上又取決於外部輸入的順序。

- 如果兩個線程**同時**調用`transferMoney`
- 線程A：從`X`向`Y`轉賬
- 線程B：從`Y`向`X`轉賬
- 那麼就會發生死鎖。

```
A: transferMoney(myAccount, yourAccount, 10);
B: transferMoney(yourAccount, myAccount, 20);
```

### 協作對象間的死鎖

Deadlocks between cooperating objects:

```java
import net.jcip.annotations.GuardedBy;

import java.util.HashSet;
import java.util.Set;

public class CooperatingDeadlock {
    // Warning: deadlock-prone!
    class Taxi {
        @GuardedBy("this")
        private Point location, destination;
        private final Dispatcher dispatcher;

        public Taxi(Dispatcher dispatcher) {
            this.dispatcher = dispatcher;
        }

        public synchronized Point getLocation() {
            return location;
        }

        // setLocation 需要Taxi内置锁
        public synchronized void setLocation(Point location) {
            this.location = location;
            if (location.equals(destination))
                // 调用notifyAvailable()需要Dispatcher内置锁
                dispatcher.notifyAvailable(this);
        }

        public synchronized Point getDestination() {
            return destination;
        }

        public synchronized void setDestination(Point destination) {
            this.destination = destination;
        }
    }

    class Dispatcher {
        @GuardedBy("this")
        private final Set<Taxi> taxis;
        @GuardedBy("this")
        private final Set<Taxi> availableTaxis;

        public Dispatcher() {
            taxis = new HashSet<Taxi>();
            availableTaxis = new HashSet<Taxi>();
        }

        public synchronized void notifyAvailable(Taxi taxi) {
            availableTaxis.add(taxi);
        }

        // 调用getImage()需要Dispatcher内置锁
        public synchronized Image getImage() {
            Image image = new Image();
            for (Taxi t : taxis)
                // 调用getLocation()需要Taxi内置锁
                image.drawMarker(t.getLocation());
            return image;
        }
    }

    class Image {
        public void drawMarker(Point p) {
        }
    }

    private class Point {

    }
}
```

> `Taxi`中的同步方法`setLocation`调用了`Dispatcher`中的同步方法`notifyAvailable`，`Dispatcher`中的同步方法`getImage`调用了`Taxi`中的同步方法`getLocation`。
> 当两个线程同时分别从`taxi`和`dispatcher`中进行调用时，会产生死锁。
>
> ————————————————<br>
> 版权声明：本文为CSDN博主「赱乂」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。<br>
> 原文链接：[https://blog.csdn.net/u014653854/article/details/80660155](https://blog.csdn.net/u014653854/article/details/80660155)<br>

上面的`getImage()`和`setLocation(Point location)`都需要獲取兩個鎖，並且在操作途中是沒有釋放鎖。

這就是隱式獲取兩個鎖（對象之間協作），這種方式也很容易造成死鎖。

## 避免和診斷死鎖

避免死鎖可以概括成三種方法：
- 固定加鎖的順序（針對鎖順序死鎖）
- 開放調用（針對對象之間協作造成的死鎖）
- 使用定時鎖 -> `tryLock()` -> 如果等待獲取鎖時間超時，則拋出異常而不是一直等待

### 固定鎖順序避免死鎖

發生原因：加鎖順序不一致

解決辦法：

> 如果請求鎖的順序相同，就不會出現循環的鎖依賴對象，也就不會產生死鎖了。

```java
public class InduceLockOrder {

    // 额外的锁、避免两个对象hash值相等的情况(即使很少)
    private static final Object tieLock = new Object();

    public void transferMoney(final Account fromAcct,
                              final Account toAcct,
                              final DollarAmount amount)
            throws InsufficientFundsException {
        class Helper {
            public void transfer() throws InsufficientFundsException {
                if (fromAcct.getBalance().compareTo(amount) < 0)
                    throw new InsufficientFundsException();
                else {
                    fromAcct.debit(amount);
                    toAcct.credit(amount);
                }
            }
        }
        // 得到锁的hash值
        int fromHash = System.identityHashCode(fromAcct);
        int toHash = System.identityHashCode(toAcct);

        // 根据hash值来上锁
        if (fromHash < toHash) {
            synchronized (fromAcct) {
                synchronized (toAcct) {
                    new Helper().transfer();
                }
            }

        } else if (fromHash > toHash) { // 根据hash值来上锁
            synchronized (toAcct) {
                synchronized (fromAcct) {
                    new Helper().transfer();
                }
            }
        } else { // 额外的锁、避免两个对象hash值相等的情况(即使很少)
            synchronized (tieLock) {
                synchronized (fromAcct) {
                    synchronized (toAcct) {
                        new Helper().transfer();
                    }
                }
            }
        }
    }
}
```

### 開放調用避免死鎖

【Java并发编程实战】頁211：

> 當調用的方法不需要持有鎖時，這被稱為開放調用（open call），依賴於開放調用的類會具有更好的行為，並且比那些需要獲得鎖才能調用的方法相比，更容易與其他的類合作。

改進：

使用開放調用，縮小加鎖粒度，同步代碼塊最好僅被用於保護那些設計【共享變量（狀態）】的操作。

```java
import net.jcip.annotations.GuardedBy;
import net.jcip.annotations.ThreadSafe;

import java.util.HashSet;
import java.util.Set;

class CooperatingNoDeadlock {
    @ThreadSafe
    class Taxi {
        @GuardedBy("this")
        private Point location, destination;
        private final Dispatcher dispatcher;

        public Taxi(Dispatcher dispatcher) {
            this.dispatcher = dispatcher;
        }

        public synchronized Point getLocation() {
            return location;
        }

        public synchronized void setLocation(Point location) {
            boolean reachedDestination;

            // 加Taxi内置锁
            synchronized (this) {
                this.location = location;
                reachedDestination = location.equals(destination);
            }
            // 执行同步代码块后完毕，释放锁


            if (reachedDestination)
                // 加Dispatcher内置锁
                dispatcher.notifyAvailable(this);
        }

        public synchronized Point getDestination() {
            return destination;
        }

        public synchronized void setDestination(Point destination) {
            this.destination = destination;
        }
    }

    @ThreadSafe
    class Dispatcher {
        @GuardedBy("this")
        private final Set<Taxi> taxis;
        @GuardedBy("this")
        private final Set<Taxi> availableTaxis;

        public Dispatcher() {
            taxis = new HashSet<Taxi>();
            availableTaxis = new HashSet<Taxi>();
        }

        public synchronized void notifyAvailable(Taxi taxi) {
            availableTaxis.add(taxi);
        }

        public Image getImage() {
            Set<Taxi> copy;

            // Dispatcher内置锁
            synchronized (this) {
                copy = new HashSet<Taxi>(taxis);
            }
            // 执行同步代码块后完毕，释放锁

            Image image = new Image();
            for (Taxi t : copy)
                // 加Taxi内置锁
                image.drawMarker(t.getLocation());
            return image;
        }
    }

    class Image {
        public void drawMarker(Point p) {
        }
    }

    private class Point {

    }
}
```

### 使用定時鎖

使用顯示Lock鎖，在獲取鎖時使用`tryLock()`方法。當等待超過時限的時候，`tryLock()`不會一直等待，而是返回錯誤訊息。

### 死鎖檢測

雖然造成死鎖的原因是因為我們設計得不夠好，但是可能寫代碼的時候不知道哪裡發生了死鎖。

JDK提供了兩種方式檢測：
- JConsoleJDK自帶的圖形化界面工具，使用JDK的工具JConsole
- JStackJDK自帶的命令行工具，主要用於線程Dump分析

具體可以參考：
- [Java线程死锁查看分析方法 - bijian1013 - 博客园](https://www.cnblogs.com/flyingeagle/articles/6853167.html){:target="_blank"}

## Reference

- [多线程之死锁就是这么简单](https://bit.ly/3613mK8){:target="_blank"}

---