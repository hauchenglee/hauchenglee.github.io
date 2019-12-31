---
layout: post
title: Java 8 - Lambda Expression
category: java
tags: [java]
---

## Lambda Expression

細數Java 8最大的更動就屬於【Lambda Expression】吧！

> In programming, a Lambda expression (or function) is just an anonymous function, i.e., a function with no name and without being bounded to an identifier.

在編程中，Lambda表達式（或函數）只是一個匿名函數，即不帶名稱且不受標識符限制的函數。

> In other words, lambda expressions are nameless functions given as constant values, and written exactly in the place where it's needed, typically as a 
> parameter to some other function.

換句話說，lambda表達式是作為常量值給出的無名函數，並精確地寫在需要的地方，通常作為其他函數的參數。

## Syntax

基本語法：

```
(parameters) -> expression

(parameters) -> { statements; }

() -> expression
```

For examples:

```
(int a, int b) -> a * b // takes two integers and return their multiplication

(a, b) -> a - b // takes two numbers and returns their difference

() -> 99 // takes no values and returns 99

(String a) -> System.out.println(a) // takes a string, prints it value to the console, and returns nothing

c -> { //some complex statements } // takes a collection and do some procesing
```

詳細說明：

這是完整形式的語法：

`(int a, int b) -> {return a + b; }`

在函數返回值的情況下，由於Java 8增強的類型推斷功能，可以通過刪除`int type`，`{ }`，`return`關鍵字和`;`來簡化代碼：

`(a, b) -> a + b`

此外，在只有一個參數的情況下，可以省略`( )`：

`a -> a * a`

最後，如果沒有參數，則只需要將`( )`留空，如下所示，這在替換`Runnable`實現或其他無參數方法時很常見。

`() -> { print("done"); }`

<br>

比對以前的寫法：

```
for (Shape s : shapes) {
    if (s.getColor() == BLUE)
        s.setColor(RED);
}
```

In Java 8:

```
shapes.forEach(s -> {
    if (s.getColor() == BLUE)
        s.setColor(RED);
});
```

簡化很多

## Why we need lambda expression?

最簡單的一段話來解釋就是：在OOP概念下想要實作`interface`很繁瑣，所以才有從`anonymous class`，再精簡到`lambda expression`

> A lambda expression is a block of code that can be passed around to execute. It is a common feature for 
> some programming languages, such as Lisp, Python, Scala, etc. But before Java 8, we can not do the same in Java. 
> If we want a block of code to be executed, we need to create an object and pass the object around. 
> From JAva 8, lambda expressions enable us to treat functionality as method argument and pass a block of code around.
>
> Ref: [Why do we need Lambda in Java?](https://www.programcreek.com/2014/01/why-lambda-java-8/)

Lambda表達式是可以傳遞執行的代碼塊。這是某些編程語言（例如Lisp，Python，Scala等）的通用功能。
但是在Java 8之前，我們無法在Java中做到這一點。
如果要執行代碼塊，則需要創建一個對象並傳遞該對象。從Java 8開始，lambda表達式使我們能夠將功能視為方法參數並傳遞代碼塊。

<br>

用多線程`Runnable`來舉例：

這是最基本的，使用OOP面向對象的寫法：

```
public class RunnableLambdaExpression implements Runnable {
    public static void main(String[] args) {
        RunnableLambdaExpression expression = new RunnableLambdaExpression();
        expression.run();
    }

    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName());
    }
}
```

我們可以看到如果要調用`interface`的方法，在OOP的觀念下必須每次都要先實作`interface`的實例（instance），才能用此對象調用並實作`interface`的抽象方法。

後來，改成匿名函數的寫法：

```
public class RunnableLambdaExpression {
    public static void main(String[] args) {
        Runnable runnable1 = new Runnable() {
            @Override
            public void run() {
                System.out.println(Thread.currentThread().getName());
            }
        };

        runnable1.run();
    }
}
```

使用Lambda Expression的寫法：

```
public class RunnableLambdaExpression {
    public static void main(String[] args) {
        Runnable runnable2 = () -> {
            System.out.println(Thread.currentThread().getName());
        };

        runnable2.run();
    }
}
```

如果Lambda Expression的statements只有一行，還可以簡化，把`{ }`去掉：

```
public class RunnableLambdaExpression {
    public static void main(String[] args) {
        Runnable runnable2 = () -> System.out.println(Thread.currentThread().getName());

        runnable2.run();
    }
}
```

## Scope

### Scope of a Lambda Expression

Java Lambda表達式沒有自己的作用域，它與外層共享同個的作用域。

> Lambdas, however, are *lexically scoped*, meaning that a lambda recognizes the immediate environment around its definition as the next outermost scope.
>
> Ref: [Java 8: Lambdas, Part 1](https://www.oracle.com/technical-resources/articles/java/architect-lambdas-part1.html)

因此，像下面的作法就會報錯，因為在同層級的作用域中已經宣告同名稱的變量，因此也不能在lambda表達式中引入此類變量：

```
String s1 = "Lambda";
// Error
Comparator<String> comp = (s1, s2) -> s1.length() - s2.length();
```

錯誤訊息為：`Labmda expression's parameter s1 cannot redeclare another local variable defined in an enclosing scope.`

關於同個作用域底下lambda表達式中的`this`關鍵字，代表創建lambda的方法的`this`參數（有點繞口）。例如：

```

```

### Accessing enclosing scope variable



### Reference

- [Variable Scope in Java Lambda Expression - KnpCode](https://knpcode.com/java/variable-scope-java-lambda-expression/)
- [3.7 Lambda Expressions and Variable Scope - InformIT](http://www.informit.com/articles/article.aspx?p=2303960&seqNum=7)

---