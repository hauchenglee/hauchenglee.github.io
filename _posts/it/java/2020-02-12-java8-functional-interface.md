---
layout: post
title: Java 8 - Functional Interface 函數式接口
category: java
tags: [java]
---

## Overview

函數式接口（Functional interface）是那些有，且只有顯示定義一個方法的接口。

函數式接口可以被隱式轉換為lambda表達式。

範例：

```java
public interface MyFunctionalInterface {
    public void execute();
}
```

> The above counts as a functional interface in Java because it only contains a 
> single method, and that method has no implementation. Normally a Java 
> interface does not contain implementation of the methods it declares, but it 
> can contain implementation in default methods, or in static methods. Below is 
> another example of a Java functional interface, with implementations of some 
> of the methods.
>
> Ref: [Java Functional Interfaces](http://tutorials.jenkov.com/java-functional-programming/functional-interfaces.html){:target="_blank"}


能成為函數式接口（functional interface）的情況：
- 只有一個方法，並且該方法沒有被實作

可以包含的方法，也就是不影響函數式接口：
- default method(s)
- static method(s)
- public method(s) instanceof Object class

不成立函數式接口的情況：
- 有多個一般方法
- 繼承的`Object`方法不是`public`

```java
public interface MyFunctionalInterface {
    /**
     * only contains a signal method in interface is functional interface
     */
    public void execute();

    /**
     * default method is acceptable in functional interface
     *
     * @param text use string param to do sth
     */
    public default void print(String text) {
        // do sth
    }

    /**
     * static method is also acceptable in functional interface
     *
     * @param i use int param i to do sth
     */
    public static void print(int i) {
        // do sth
    }

    /**
     * methods that are instance Object class, note: must be public method in Object
     *
     * @return Object
     */
    @Override
    String toString();

    @Override
    int hashCode();

    @Override
    boolean equals(Object object);

    /**
     * if there have some methods that is not public of Object class,
     * it is not functional interface. for example, clone() method is
     * protected modifier inside Object class instead of public.
     *
     * @return Object
     */
    Object clone();
}
```

## Reason

> 为什么会单单从接口中定义出此类接口呢？原因是在Java Lambda的实现中， 开发组不想再为Lambda表达式单独定义
> 一种特殊的Structural函数类型，称之为箭头类型（arrow type）， 依然想采用Java既有的类型系统(class, interface, method等)，
> 原因是增加一个结构化的函数类型会增加函数类型的复杂性，破坏既有的Java类型，并对成千上万的Java类库造成严重的影响。
> 权衡利弊，因此最终还是利用SAM接口作为Lambda表达式的目标类型。
>
> Ref: [Java 8函数式接口functional interface的秘密 \| 鸟窝](https://colobu.com/2014/10/28/secrets-of-java-8-functional-interface/){:target="_blank"}

## @FunctionalInterface

此註解功能類似於`@Override`，可以幫助我們檢查是否為函數式接口（functional interface），如果不符合定義就會編譯報錯。

例如以上述代碼為例：

```java
@FunctionalInterface
public interface MyFunctionalInterface {
    public void execute();

    public default void print(String text) {
        // do sth
    }

    public static void print(int i) {
        // do sth
    }

    @Override
    String toString();

    @Override
    int hashCode();

    @Override
    boolean equals(Object object);
}
```

在上述代碼中，並未有：
1. 一個以上的一般方法
2. 非`public`的`Object class`方法

原因是可以保護函數式接口能成功轉換為lambda表達式，如果想要通過lambda表達式來實作此接口，請添加此安全性注釋。

<br>

如果改成以下這樣，就會報錯：

```java
@FunctionalInterface
public interface MyFunctionalInterface {
    public void execute();

    public void secondExecute();
}
```

或是：

```java
@FunctionalInterface
public interface MyFunctionalInterface {
    public void execute();

    Object clone();
}
```

錯誤訊息為：

```console
Multiple non-override abstract methods found in interface
```

Ref:
- [FunctionalInterface (Java Platform SE 8 )](https://docs.oracle.com/javase/8/docs/api/java/lang/FunctionalInterface.html){:target="_blank"}
- [lambda - What are functional interfaces used for in Java 8? - Stack Overflow](https://bit.ly/3bCJ6lg){:target="_blank"}
- [functional interface - Why to use @FunctionalInterface annotation in Java 8 - Stack Overflow](https://bit.ly/2vukD0y){:target="_blank"}

---