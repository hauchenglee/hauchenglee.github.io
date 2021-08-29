---
layout: post
title: Java - Thread Immutable Objects 對象不可改變性
category: java
tags: [java]
comments: true
toc: true
---

## Immutable Objects

> An object is considered *immutable* if its state cannot change after it is constructed. Maximum reliance on immutable 
> objects is widely accepted as a sound strategy for creating simple, reliable code.
>
> Immutable objects are particularly useful in concurrent applications. Since they cannot change state, they cannot be 
> corrupted by thread interference or observed in an inconsistent state.
>
> Ref: [Immutable Objects (The Java™ Tutorials > Essential Classes > Concurrency)](https://docs.oracle.com/javase/tutorial/essential/concurrency/immutable.html){:target="_blank"}

以上開場白出自Java Oracle Tutorial系列教程。

不變性是在多線程環境下的操作，還能維持與確保數據安全方法之一（其他兩個為：不共享變量、使用同步機制）

## A Synchronized Class Example

這是Oracle給出來的範例：

```java
public class SynchronizedRGB {

    // Values must be between 0 and 255.
    private int red;
    private int green;
    private int blue;
    private String name;

    private void check(int red,
                       int green,
                       int blue) {
        if (red < 0 || red > 255
                || green < 0 || green > 255
                || blue < 0 || blue > 255) {
            throw new IllegalArgumentException();
        }
    }

    public SynchronizedRGB(int red,
                           int green,
                           int blue,
                           String name) {
        check(red, green, blue);
        this.red = red;
        this.green = green;
        this.blue = blue;
        this.name = name;
    }

    public void set(int red,
                    int green,
                    int blue,
                    String name) {
        check(red, green, blue);
        synchronized (this) {
            this.red = red;
            this.green = green;
            this.blue = blue;
            this.name = name;
        }
    }

    public synchronized int getRGB() {
        return ((red << 16) | (green << 8) | blue);
    }

    public synchronized String getName() {
        return name;
    }

    public synchronized void invert() {
        red = 255 - red;
        green = 255 - green;
        blue = 255 - blue;
        name = "Inverse of " + name;
    }
}
```

代碼看起來挺簡單的，就是一個簡單的[POJO](https://www.geeksforgeeks.org/pojo-vs-java-beans/){:target="_blank"}，
但是如果在多線程（例如：A、B兩線程）執行以下代碼就必須小心：

```
SynchronizedRGB color =
    new SynchronizedRGB(0, 0, 0, "Pitch Black");
...
int myColorInt = color.getRGB();      //Statement 1
String myColorName = color.getName(); //Statement 2
```

如果這時B線程在A線程執行Statement 1與Statement 1中間調用了`color.set`，將造成`myColorInt`和`myColorName`的值無法匹配。

為了避免出現這樣的線程不安全結果，必須把兩個行為綁定在一起：

```
synchronized (color) {
    int myColorInt = color.getRGB();
    String myColorName = color.getName();
}
```

這種不一致的狀態（inconsistent state）只會發生在可改變對象（mutable objects），對於不可改變的對象是不會有問題的。

## A Strategy for Defining Immutable Objects

Oracle對於如何設計出不可改變對象（Immutable Objects）提出幾點策略建議：

> 1. Don't provide "setter" methods — methods that modify fields or objects referred to by fields.
> 2. Make all fields `final` and `private`.
> 3. Don't allow subclasses to override methods. The simplest way to do this is do declare the class as `final`. A more 
>    sophisticated approach is to make the constructor `private` and construct instances in factory methods.
> 4. If the instance fields include references to mutable objects, don't allow those objects to be changed:
>   - Don't provide methods that modify the mutable objects.
>   - Don't share references to the mutable objects. Never store references to external, mutable objects passed 
>     to the constructor; if necessary, create copies, and store references to the copies. Similarly, create copies of 
>     your internal mutable objects when necessary to avoid returning the originals in your methods.

比較有意思的是第三點的工廠方法（factory methods），以及第四點要稍微注意不要修改（modify）或是共享（share）可變對象（mutable objects）的內存地址引用（reference）。

所以，依據上述策略，可以來改寫先前線程不安全`SynchronizedRGB`的版本：
1. 不提供`setter`方法，杜絕修改字段，以及通過`invert()`返回新的修改後對象，避免了修改現有對象
2. 將所有的字段加上`final`和`private`
3. 在class加上`final`防止繼承，或是將constructor改為`private`並且用工廠模式產生instances
4. 只有一個字段引用一個對象，而該對象本身是不可變的

經過這些更改後，我們有了`ImmutableRGB`：

```java
final public class ImmutableRGB {

    // Values must be between 0 and 255.
    final private int red;
    final private int green;
    final private int blue;
    final private String name;

    private void check(int red,
                       int green,
                       int blue) {
        if (red < 0 || red > 255
                || green < 0 || green > 255
                || blue < 0 || blue > 255) {
            throw new IllegalArgumentException();
        }
    }

    public ImmutableRGB(int red,
                        int green,
                        int blue,
                        String name) {
        check(red, green, blue);
        this.red = red;
        this.green = green;
        this.blue = blue;
        this.name = name;
    }


    public int getRGB() {
        return ((red << 16) | (green << 8) | blue);
    }

    public String getName() {
        return name;
    }

    public ImmutableRGB invert() {
        return new ImmutableRGB(255 - red,
                255 - green,
                255 - blue,
                "Inverse of " + name);
    }
}
```

---