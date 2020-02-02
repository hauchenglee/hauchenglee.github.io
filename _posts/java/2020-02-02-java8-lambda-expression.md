---
layout: post
title: Java 8 - Lambda Expression
category: java
tags: [java]
---

## Lambda Expression

細數Java 8最大的更動就屬於【Lambda Expression】吧（個人感覺）！

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
> Ref: [Why do we need Lambda in Java?](https://www.programcreek.com/2014/01/why-lambda-java-8/){:target="_blank"}

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
> Ref: [Java 8: Lambdas, Part 1](https://www.oracle.com/technical-resources/articles/java/architect-lambdas-part1.html){:target="_blank"}

因此，像下面的作法就會報錯，因為在同層級的作用域中已經宣告同名稱的變量，因此也不能在lambda表達式中引入此類變量：

```
String s1 = "Lambda";
// Error
Comparator<String> comp = (s1, s2) -> s1.length() - s2.length();
```

錯誤訊息為：`Labmda expression's parameter s1 cannot redeclare another local variable defined in an enclosing scope.`

關於同個作用域底下lambda表達式中的`this`關鍵字，代表創建lambda的方法的`this`參數（有點繞口）。例如：

```
public class Application() {
    public void doWork() {
        Runnable runner = () -> { ...; Sysout.out.println(this.toString()); ... };
    }
}
```

在這表達式中的`this.toString()`代表調用`toString()`這個方法的`Application`對象，而不是`Runnable`實例，並沒有什麼特別的地方。

可以這麼說，`this`在這個方法裡的任何地方含義都相同，沒有任何差異。

### Accessing enclosing scope variable

如何從lambda表達式中的封閉方法（enclosing method）或者類（class）訪問變量？

```
public class RepeatMessage {
    public static void repeatMessage(String text, int count) {
        Runnable runnable = () -> {
            for (int i = 0; i < count; i++) {
                System.out.println(text);
            }
        };
        new Thread(runnable).start();
    }

    public static void main(String[] args) {
        repeatMessage("Hello", 5); // // Prints Hello 5 times in a separate thread
    }
}
```

考慮上面代碼，先複習lambda表達式分為三部分：
- 一段代碼（A block of code）
- 參數（Parameters）
- 自由變量的值（values for the *free* variables）-> 意思是該變量值不是參數，也不是在lambda中宣告的

在我們的範例中，有兩個自由變量：`text` & `count`，這兩個值被lambda所捕獲（capture）並且存儲這些變量值（示例中是`Hello` & `5`）

```
class RepeatMessage {
    public static void repeatMessage(String text, int count) {
        Runnable runnable = () -> {
            for (int i = 0; i < count; i++) {
                System.out.println(text);
            }
        };
        new Thread(runnable).start();

        for (int i = 0; i < count; i++) {
            new Thread(() -> System.out.println(i)); // error

            // error: Variable used in lambda expression should be final or effectively final
        }
    }

    public static void main(String[] args) {
        repeatMessage("Hello", 5);
    }
}
```

我們發現`new Thread(() -> System.out.println(i));`在lambda表達式中是編譯失敗的，
錯誤提示我們必須在存在於表達式中的變量加上`final`或是變量是有效地最終變量（effectively final）。

從編譯錯誤給出來的訊息`final`，有很明顯的暗示：可能會引起並發問題（只要看到`final`就要聯想到並發）！並且在JLS在官方文件中[§15.27.2](https://docs.oracle.com/javase/specs/jls/se10/html/jls-15.html#jls-15.27.2){:target="_blank"}中提到了原因

> Any local variable, formal parameter, or exception parameter used but not declared in a lambda expression must either be declared final or be effectively final, 
> or a compile-time error occurs the use is attempted.
>
> Any local variable used but not declared in a lambda body must be definitely assigned before the lambda body, or a compile-time error occurs.

已使用但未在lambda表達式中聲明的任何局部變量，形式參數或異常參數都必須聲明final或有效地是最終的，否則在嘗試使用時會發生編譯時錯誤。

必須在lambda主體之前明確分配任何已使用但未在lambda主體中聲明的局部變量，否則會發生編譯時錯誤。

（以上為google翻譯）

<br>

回到剛剛的`for`循環範例，`i`值無法保證在多線程環境下是線程安全的，而規則是lambda表達式只能訪問封閉範圍內的*有效*（effectively）局部變量。
有效的`final`變量永遠不會被修改 —> 它應該本質上為`final`或可以被聲明為`final`的變量。

官方說的更明白：

> The restriction to effectively final variables includes standard loop variables, but not enhanced-for loop variables, 
> which are treated as distinct for each iteration of the loop.

對有效最終變量的限制包括標準循環變量，但不包括增強for循環變量，對於循環的每次迭代，它們都被視為不同的變量。

Ref:
- [java - Variable used in lambda expression should be final or effectively final - Stack Overflow](https://bit.ly/39v91dd){:target="_blank"}
- [Why Do We Need Effectively Final? - Baeldung](https://www.baeldung.com/java-lambda-effectively-final-local-variables){:target="_blank"}

## Reference

- [Variable Scope in Java Lambda Expression - KnpCode](https://knpcode.com/java/variable-scope-java-lambda-expression/){:target="_blank"}
- [3.7 Lambda Expressions and Variable Scope - InformIT](http://www.informit.com/articles/article.aspx?p=2303960&seqNum=7){:target="_blank"}

---