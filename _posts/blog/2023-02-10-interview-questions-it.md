---
layout: post
title: 面試題（IT）
category: blog
tags: [diary]
---

## Java Basic

java basic：final、static、equal
java advance：线程、内存、传值、collection
spring：aop ioc
设计模式：工厂、单例、spring 用到的
高并发、分布式、微服务
数据库：sql injection
数据结构：

### == equal 差別

==：
- `==`其實是在判斷`stack`內的值，當兩個參考資料型別變數指向同一物件， `==`運算子的結果會為`true`; 且若兩個參考資料型別變數指向不同物件時，結果為`false`。
- 在`String`中使用`==`，因為Java為節省記憶體，會在某一輪調區中維護唯一的`String`物件，所以如果在類別裡使用同一字串，Java只會建立一個唯一的字串而已。

equal：
- 先比較兩物件是否為相同類型的類別後在比較其內容值是否相同，是就回傳`true`，否則回傳`false`。
- `equals()` 不能用于判断基本数据类型的变量，只能用来判断两个对象是否相等。`equals()`方法存在于`Object`类中，而`Object`类是所有类的直接或间接父类，因此所有的类都有`equals()`方法。

简单来说：
- ==：比较其值（如 1=1），和两个对象的内存地址（即使对象的值相同）
- equal：两个对象的值（如new(1) = new (1)），虽然分属于两个不同对象，但其值相同

### final, finally, finalize 差別

- final：
    - 修飾class（類）：表示不能被子類繼承
    - 修飾method（方法）：表示不能被子類改寫
    - 修飾variable（變量）：表示不能被改變其值，並且必須在宣告時或是構造函數初始化
- finally：finally用於部分區塊代碼一定會被執行，無論程序是否發生例外異常，多用在`try-catch`中，用來恢復異常的有效狀態，和清理non-memory資源
- finalize：表示Java `object`類的一個方法，在垃圾回收機制中執行的時候會調用被回收對象的方法，允許回收此前未回收的記憶體垃圾

### 面向对象 封装 继承 多态

- 继承：让某个类型的对象获得另一个类型的对象的属性的方法。继承就是子类继承父类的特征和行为，使得子类对象（实例）具有父类的实例域和方法，或子类从父类继承方法，使得子类具有父类相同的行为。
- 封装：隐藏部分对象的属性和实现细节，对数据的访问只能通过外公开的接口。通过这种方式，对象对内部数据提供了不同级别的保护，以防止程序中无关的部分意外的改变或错误的使用了对象的私有部分。
- 多态：对于同一个行为，不同的子类对象具有不同的表现形式。多态存在的3个条件：1）继承；2）重写；3）父类引用指向子类对象。

Constructor 不能被 override（重写），但是可以 overload（重载），所以你可以看到⼀个类中有多个构造函数的情况。

### interface vs abstract class

interface
- 不能實體化。
- 沒有建構子。
- 預設為`public static`以及`final`，並且使用`implement`來實作。
- 其中的方法必須是`abstract`的。
- 其中的資料成員都為常數。

abstract
- 不能實體化
- 有建構子
- 如同一般類別預設為`default`，並且使用`extends`來繼承。
- 繼承後的類別如果沒有完全實作所以內容的話，則此類別就同為抽象類別。
- 其中的方法可以是`abstract`也可以是一般方法。

使用的時機以例子來舉例。

> 例如鴿子、麻雀以及飛機。<br>
> 三種都實作飛行（`abstract`）的功能。<br>
> 飛機則除了實作飛行外，還繼承機器（`implement`）。<br>
> 鴿子與麻雀則實作飛行外，還繼承動物（`implement`）。<br>
>
> 即當多個類別之間有共同的方法或屬性時，可以使用抽象類別。<br>
> 而若其方法實作的方式有差異，就可以將這樣的方法寫成介面。

### pass by value ＆ pass by reference 差別

值传递。Java 中只有值传递，对于对象参数，值的内容是对象的引用。

[methods - Is Java "pass-by-reference" or "pass-by-value"? - Stack Overflow](https://stackoverflow.com/questions/40480/is-java-pass-by-reference-or-pass-by-value?page=1&tab=scoredesc#tab-top)

### static

- 唯一值：靜態的意思，可以用來宣告一個函數或者變數，當被宣告為`static`時，它就具有唯一值的概念，永遠只佔著那一組記憶體空間，不管該類別被new幾個`object`，該值永遠都會是一樣
- 可以透過類別直接調用：由於`static`方法就是沒有`this`的方法。在`static`方法內部不能調用非靜態方法，反過來是可以的。而且可以在沒有創建任何對象的前提下，僅僅通過類本身來調用static方法

成员变量存在于堆内存中。静态变量存在于方法区中。

成员变量与对象共存亡，随着对象创建而存在，随着对象被回收而释放。静态变量与类共存亡，随着类的加载而存在，随着类的消失而消失。

成员变量所属于对象，所以也称为实例变量。静态变量所属于类，所以也称为类变量。

成员变量只能被对象所调用 。静态变量可以被对象调用，也可以被类名调用。

### string 

[java-string #h-string-in-memory](https://hauchenglee.github.io/java/2019/11/06/java-string.html#h-string-in-memory)

### string stringbuffer stringbuilder

- `String`中的
    - 对象是不可变的，也就可以理解为常量，String 的值被创建后不能修改，任何对 String 的修改都会引发新的 String 对象的生成
    - 线程安全
    - `AbstractStringBuilder`是`StringBuilder`与`StringBuffer`的公共父类，定义了一些字符串的基本操作，如 `expandCapacity`、`append`、`insert`、`indexOf`等公共方法
- `StringBuffer`：线程安全（加了同步锁）
- `StringBuilder`：非线程安全

### arraylist, linkedlist 差別

`arraylist`：
- 查找快
- 修改慢（需要進行擴容操作，實際上就是創建出一個更大的數組，然後將舊數組中的值拷貝到新數組中）

`linkedlist`：
- 查找慢（需要通过for循环进行查找，虽然已经在查找方法上做了优化，比如index < size / 2，则从左边开始查找，反之从右边开始查找，但是还是比ArrayList要慢）
- 修改快

两者都不是线程安全的

### HashSet 是如何保证不重复的？

HashSet 底层使用 `HashMap` 来实现，见下面的源码，元素放在 `HashMap` 的 `key` 里，`value` 为固定的 `Object` 对象。当 `add` 时调用 `HashMap` 的 `put`方法，如果元素不存在，则返回 `null` 表示 `add` 成功，否则 `add` 失败。

```java
private transient HashMap<E,Object> map;
 
// Dummy value to associate with an Object in the backing Map
private static final Object PRESENT = new Object();
 
public boolean add(E e) {
    return map.put(e, PRESENT)==null;
}
```

由于 `HashMap` 的 `Key` 值本身就不允许重复，HashSet 正好利用 `HashMap` 中 key 不重复的特性来校验重复元素

### collection的使用场景与比较 set/list/queue/map

列队、排序：list
- 快速更新：linkedlist
- 快速访问：arraylist

允许将向量视为堆栈：
- `push`, `pop`, `peek()`

[Java集合类：map、set、list、queue、stack的特点与用法 - 掘金](https://juejin.cn/post/7101613196594642952)

### 如何实现多线程

通常来说，可以认为有三种方式：1）继承 `Thread` 类；2）实现 `Runnable` 接口；3）实现 `Callable` 接口。

其中，`Thread` 其实也是实现了 `Runable` 接口。`Runnable` 和 `Callable` 的主要区别在于是否有返回值。

[Java - Thread Objects 線程對象 \| 吹雪](https://hauchenglee.github.io/java/2019/12/15/thread-objects.html)

### Thread 调用 start() 方法和调用 run() 方法的区别

`run()`：普通的方法调用，在主线程中执行，不会新建一个线程来执行。

`start()`：新启动一个线程，这时此线程处于就绪（可运行）状态，并没有运行，一旦得到 CPU 时间片，就开始执行 `run()` 方法。

### 并发与并行

并发：两个或多个事件在同一时间间隔发生。

并行：两个或者多个事件在同一时刻发生。

并行是真正意义上，同一时刻做多件事情，而并发在同一时刻只会做一件事件，只是可以将时间切碎，交替做多件事情。

网上有个例子挺形象的：

你吃饭吃到一半，电话来了，你一直到吃完了以后才去接，这就说明你不支持并发也不支持并行。

你吃饭吃到一半，电话来了，你停了下来接了电话，接完后继续吃饭，这说明你支持并发。

你吃饭吃到一半，电话来了，你一边打电话一边吃饭，这说明你支持并行。

### synchronized 同步机制

[Java - Thread Synchronization 線程同步機制 \| 吹雪](https://hauchenglee.github.io/java/2019/12/20/thread-synchronization.html#h-implementation-principle)

### 线程安全

三种线程安全危险：
1. 线程危险
    - 线程干扰错误
    - 内存一致性错误
2. 活跃度危险
3. 性能危险

预防：
- 使用同步：`synchronized`
- 原子性：`atomic`
- 可見性：`volatile`
- 線程封閉：`ThreadLocal`
- 不變性：`final`
- 線程安全性委託：JDK

[Java - Risks of Thread 線程的風險 \| 吹雪](https://hauchenglee.github.io/java/2019/12/17/thread-risks.html)

### 线程池的使用场景

- 降低资源消耗。通过重复利用已创建的线程，降低线程创建和销毁造成的消耗。
- 提高响应速度。当任务到达时，任务可以不需要等到线程创建就能立即执行。
- 增加线程的可管理型。线程是稀缺资源，使用线程池可以进行统一分配，调优和监控。

### 线程池的核心属性

- `threadFactory`（线程工厂）：用于创建工作线程的工厂。
- `corePoolSize`（核心线程数）：当线程池运行的线程少于 `corePoolSize` 时，将创建一个新线程来处理请求，即使其他工作线程处于空闲状态。
- `maximumPoolSize`（最大线程数）：线程池允许开启的最大线程数。
- `workQueue`（队列）：用于保留任务并移交给工作线程的阻塞队列。
- `handler`（拒绝策略）：往线程池添加任务时，将在下面两种情况触发拒绝策略：1）线程池运行状态不是 `RUNNING`；2）线程池已经达到最大线程数，并且阻塞队列已满时。
- `keepAliveTime`（保持存活时间）：如果线程池当前线程数超过 `corePoolSize`，则多余的线程空闲时间超过 `keepAliveTime` 时会被终止。

### 线程运作流程

Ref: [面试必问的线程池，你懂了吗？_程序员囧辉的博客-CSDN博客](https://joonwhee.blog.csdn.net/article/details/106609583)

### 终止线程

`shutdown`, `shutdownNow`

### Java 8

新特性
- lambda
- stream api
- 日期新格式
- optional

## 设计模式

https://hauchenglee.github.io/architecture/2020/04/16/design-pattern.html

### singleton

[单例模式 \| 菜鸟教程](https://www.runoob.com/design-pattern/singleton-pattern.html)

### factory


### 套件中或项目用过的设计模式


## spring

[面试必问的 Spring，你懂了吗？_程序员囧辉的博客-CSDN博客_面试问简单介绍spring](https://blog.csdn.net/v123411739/article/details/110009966)

### spring 使用場景

Spring是于2003 年兴起的一个轻量级的Java 开发框架

特点：
- IOC容器解耦
- AOP编程支持
- 易于依赖管理（dependence）
    - spring-boot-starter-data-jpa
    - spring-boot-starter-web
- 对应用服务器的原生支持——Servlet 容器
- 对各种优秀框架的集成，如hibernate、struts
- 方便程序的测试，例如：Spring对Junit4支持，可以通过注解方便的测试Spring程序

### spring life time 說明

[Spring中Bean的生命周期是怎样的？ - 知乎](https://www.zhihu.com/question/38597960)

### IOC

可以将对象之间的依赖关系交由Spring进行控制，避免硬编码所造成的过度程序耦合

### AOP



## Database

### Sql injection

因字符相加造成

error

```sql
select * from user where(name = 'input' or 1=1) and (password = 'input' or 1=1)
```

预防：
1. 参数化查询
2. 最小权限原则
3. 合法性、合规性检查
4. 加入其它字符替代、跳脱字节
5. 强化防火墙

### 正规化

1NF：
- 一個欄位儲存多筆資料：a;b;c'1
- 出現意義上重複的欄位：男, man
- 缺乏主鍵(Primary Key)

2NF：
- 消除部分相依：将重复出现的数据独立各自table解耦

Ref: [Day 32 資料庫正規化(一~三) - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天](https://ithelp.ithome.com.tw/articles/10229472)

## Ref

- [Java 基础高频面试题（2022年最新版）_程序员囧辉的博客-CSDN博客](https://blog.csdn.net/v123411739/article/details/115364158)

---