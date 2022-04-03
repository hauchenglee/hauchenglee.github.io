---
layout: post
title: Java 8 - Default Method 預設方法
category: java
tags: [java]
---

## Default Method

> Before Java 8, interfaces could have only abstract methods. The implementation of 
> these methods has to be provides in a separate class. So, if a new method is to be 
> added in an interface, then its implementation code has to provided in the class 
> implementing the same interface. To overcome this issue, Java 8 has introduced 
> the concept of default methods which allow the interfaces to have methods with 
> implementation without affecting the classes that implement the interface.

在Java 8之前，接口只能具有抽象方法。這些方法的實現必須在單獨的類中提供。
因此，如果要在接口中添加新方法，則必須在實現相同接口的類中提供其實現代碼。
為克服此問題，Java 8引入了默認方法的概念，該方法允許接口具有可實現的方法，而不會影響實現接口的類。

> The default methods were introduced to provide backward compatibility so that 
> existing interfaces can use the lambda expressions without implementing the 
> methods in the implementation class.

引入了默認方法以提供向後兼容性，以便現有接口可以使用lambda表達式而無需在實現類中實現這些方法。

> Default methods are also known as **defender methods** or **virtual extension methods**

默認方法也稱為"防御者方法"或"虛擬擴展方法"。

<br>

直上代碼~

```java
@FunctionalInterface
public interface MyFunctionalInterface {
    public void execute();

    /**
     * this is default method
     */
    default void defaultMethod() {
    }

    /**
     * this is static method
     */
    static void staticMethod() {
    }
}

class MyMainDefaultMethod implements MyFunctionalInterface {
    public static void main(String[] args) {
        // static method executed
        MyFunctionalInterface.staticMethod();
    }

    @Override
    public void execute() {
        // override
    }

    @Override
    public void defaultMethod() {
        // not necessary, you can also override this default method
    }
}
```

## Static method

【Override】

First, static method cannot be overridden in implementing the class.

```java
interface NewInterface {
    static void hello() {
        System.out.println("Hello, New Static Method here");
    }

    void overrideMethod(String str);
}

public class InterfaceDemo implements NewInterface {
    public static void main(String[] args) {
        InterfaceDemo interfaceDemo = new InterfaceDemo();

        // Calling the static method of interface
        NewInterface.hello();

        // Calling the abstract method of interface
        interfaceDemo.overrideMethod("Hello, Override Method here");
    }

    @Override
    public void overrideMethod(String str) {
        System.out.println(str);
    }
}
```

Output:

```console
Hello, New Static Method Here
Hello, Override Method here
```

【Scope】

Second, the scope of the static method definition is within the interface only.

```java
interface PrintDemo {
    static void hello() {
        System.out.println("Called from Interface PrintDemo");
    }
}

public class InterfaceDemo implements PrintDemo {
    public static void main(String[] args) {
        // Call Interface method as Interface
        // name is proceeding with method
        PrintDemo.hello();

        // Call Class static method
        hello();
    }

    // Class static method is defined
    static void hello() {
        System.out.println("Called from Class");
    }
}
```

Output:

```console
Called from Interface PrintDemo
Called from Class
```

【Invoke】

Third, different calling the method:
- access static methods using the interface name
- to call a method you need to use the object of the implementing class

```java
interface MyInterface {
    public void abstractMethod();

    public static void staticMethod() {
        System.out.println("This is a static method");
    }
}

public class InterfaceDemo implements MyInterface {
    public static void main(String[] args) {
        InterfaceDemo obj = new InterfaceDemo();
        obj.abstractMethod();
        MyInterface.staticMethod();
    }

    @Override
    public void abstractMethod() {
        System.out.println("This is the implementation of the abstractMethod");
    }
}
```

Output:

```console
This is the implementation of the abstractMethod
This is a static method
```

Ref: 
- [Static method in Interface in Java - GeeksforGeeks](https://www.geeksforgeeks.org/static-method-in-interface-in-java/){:target="_blank"}
- [Default method vs static method in an interface in Java?](https://www.tutorialspoint.com/default-method-vs-static-method-in-an-interface-in-java){:target="_blank"}

## Why we need static method

這個問題有點難以回答。。。

先跳過，留下相關鏈接，未來再細細閱讀~

Ref:
- [Why can't I define a static method in a Java interface? - Stack Overflow](https://bit.ly/2SnoLsn){:target="_blank"}
- [class - What is the purpose of a static method in interface from Java 8? - Stack Overflow](https://bit.ly/39tbOmw){:target="_blank"}
- [Why were default and static methods added to interfaces in Java 8 when we already had abstract classes? - Software Engineering Stack Exchange](https://bit.ly/2wjKyZD){:target="_blank"}
- [java - Is "static interface" a good practice? - Software Engineering Stack Exchange](https://bit.ly/38xG2Vi){:target="_blank"}

## Default Method and Multiple Inheritance

我們都知道，Java是禁止多重繼承的（Multiple Inheritance），如下所示：

```java
class A {
    void show() {

    }
}

class B {
    void show() {

    }
}

class InterfaceDemo extends A, B {
    public static void main(String[] args) {

    }
}
```

Java禁止多重繼承，因為開放多重繼承（例如C++）會造成[Diamond Problem](http://www.lambdafaq.org/what-about-the-diamond-problem/){:target="_blank"}的現象。

就是說如果class C inherit class A 和class B，而A、B中有一個完全一樣的方法`show()`；
這個時候class C一個instance叫做`obj`， 當`obj.show()`的時候，它就不知道去invoke哪個`show()`。

簡單來說，這是因為我們在接口中都有相同的方法，並且編譯器不確定要調用哪個方法。

Java只能多重接口繼承。

如下範例，A、B都有一個抽象方法`show()`，並且接口A、B都被`InterfaceDemo`實作，這時在`InterfaceDemo`必須去override這個`show()`方法，
因為在接口A、B僅僅是宣告方法`show()`，並沒有實際指示該如何運行的細節，也就是去定義（define）它，
如果將來要調用`show()`，也是調用在`InterfaceDemo`實例化的`obj`並且override後的`show()`方法，所以這時候是不會有衝突的。

```java
interface A {
    void show();
}

interface B {
    void show();
}

class InterfaceDemo implements A, B {
    public static void main(String[] args) {
        InterfaceDemo obj = new InterfaceDemo();

        // invoke method inside class InterfaceDemo
        obj.show();
    }

    @Override
    public void show() {

    }
}
```

但是Java 8加入Default Method一切都不一樣了。

```java
interface A {
    default void show() {
        System.out.println("A");
    }
}

interface B {
//    default void show() {
//        System.out.println("B");
//    }
}

class InterfaceDemo implements A, B {
    public static void main(String[] args) {
        InterfaceDemo obj = new InterfaceDemo();

        // invoke method inside
        obj.show();
    }

//    @Override
//    public void show() {
//        System.out.println("InterfaceDemo");
//    }
}
```

我們發現，如果把接口B的同名Default Method`show()`註解掉，是不會有問題的。然而如果有兩個同名Default Method，並且又被多重繼承，跟之前的Diamond Problem很相似，也會錯誤：

```java
interface A {
    default void show() {
        System.out.println("A");
    }
}

interface B {
    default void show() {
        System.out.println("B");
    }
}

class InterfaceDemo implements A, B {
    public static void main(String[] args) {
        InterfaceDemo obj = new InterfaceDemo();

        // invoke method inside
        obj.show();
    }

//    @Override
//    public void show() {
//        System.out.println("InterfaceDemo");
//    }
}
```

錯誤訊息為：`InterfaceDemo inherits unrelated default for show() from type A and B`

解決方法也很簡單，在實作多重繼承接口的class中override該Default Method就好：

```java
interface A {
    default void show() {
        System.out.println("A");
    }
}

interface B {
    default void show() {
        System.out.println("B");
    }
}

class InterfaceDemo implements A, B {
    public static void main(String[] args) {
        InterfaceDemo obj = new InterfaceDemo();

        // invoke method inside
        obj.show();
    }

    @Override
    public void show() {
        System.out.println("InterfaceDemo");
    }
}
```

InterfaceDemo的實例化對象在調用時`obj.show()`會出現怎樣的情況？答案是輸出`InterfaceDemo`，因为class比interface有更多的力量（power），
這個時候的class InterfaceDemo中的`show()`會屏蔽掉interface中的`show()`。

Output:

```console
InterfaceDemo
```

Ref:
- [Java 8 Interface Changes – default method and static method](https://beginnersbook.com/2017/10/java-8-interface-changes-default-method-and-static-method/){:target="_blank"}
- [Interface in Java 8(Default/Static methods)_Stan的专栏-CSDN博客](https://blog.csdn.net/stanxl/article/details/53512142){:target="_blank"}

## Reference

- [Default Methods (The Java™ Tutorials > Learning the Java Language > Interfaces and Inheritance)](https://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html){:target="_blank"}
- [Default Methods In Java 8 - GeeksforGeeks](https://www.geeksforgeeks.org/default-methods-java/){:target="_blank"}
- [Don'tCare: 簡介 Java 8 的 default method](http://blog.dontcareabout.us/2013/03/java-8-default-method.html){:target="_blank"}
- [Java 8 默认方法（Default Methods） - Ebn's Blog](http://ebnbin.com/2015/12/20/java-8-default-methods/){:target="_blank"}

---