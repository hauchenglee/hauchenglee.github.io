---
layout: post
title: Java - Class and Object 面向對象
category: java
tags: [java]
comments: true
toc: true
---

## Reference

Reference types hold reference to objects and provide a means to access those objects stored somewhere in memory. All reference types are a subclass of type `java.lang.Object`. 

<table>
    <thead>
        <tr>
            <th>Reference type</th>
            <th>Brief description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Annotation</td>
            <td>Provides a way to associate metadata (data about data) with program elements.</td>
        </tr>
        <tr>
            <td>Array</td>
            <td>Provides a fixed-size data structure that stores data elements of the same type.</td>
        </tr>
        <tr>
            <td>Class</td>
            <td>Designed to provide inheritance, polymorphism, and encapsulation. 
             Usually models somethings in the real world and consists of a set of values that holds data and a set of methods that operates on the data.</td>
        </tr>
        <tr>
            <td>Enumeration</td>
            <td>A reference for a set of objects that represents a related set of choices.</td>
        </tr>
        <tr>
            <td>Interface</td>
            <td>Provides a public API and is "implemented by Java classes."</td>
        </tr>
    </tbody>
</table>

> - [4. Reference Types - Java 8 Pocket Guide [Book]](https://www.oreilly.com/library/view/java-8-pocket/9781491901083/ch04.html){:target="_blank"}

<br>

In Java or C#, primitive variables area created in the stack while reference variable is created in heap space, which is managed by garbage collector.
Memory allocation in terms of stack and heap is not specified in the C++ standard.

> - [Difference between Primitive and Reference variable in Java](https://javarevisited.blogspot.com/2015/09/difference-between-primitive-and-reference-variable-java.html){:target="_blank"}
> - [Confused about Stack and Heap? - Fhinkel - Medium](https://medium.com/fhinkel/confused-about-stack-and-heap-2cf3e6adb771){:target="_blank"}

## class

> "class" is (approximately) synonymous with "type." -- on java 8 page 38

<br>

[what does `.class` mean in Java?](https://stackoverflow.com/questions/15078935/what-does-class-mean-in-java/15078951){:target="_blank"}

> When you write `.class` after a class name, it references the class literal - `java.lang.Class` object 
> that represents information *about* given class.

- 因為Java所有操作都是基於對象，故如果要操作`class`必須先把`class`類別實例化
- 當此類尚未實例化，使用`.class`
- 當此類已經實例化，使用`.getClass()`

> For example, if your class is `Print`, then `Print.class` is an object that represents the class `Print` 
> on runtime. It is the same object that is returned by the `getClass()` method of any (direct) instance of 
> `Print`.

- 如果您的類是`Print`，那麼`Print.class`是一個`Print`在運行時表示該類的對象
- 此`getClass()`與`Print.class`返回的對象是同一個，並且由`getClass()`獲得的對象與`Print`具有同個實例化對象

```
Print myPrint = new Print();
System.out.println(Print.class.getName());
System.out.println(myPrint.getClass().getName());
```

## constructor

- This guarantees that the object is properly initialized you can get your hands on it.
- Priority order:
   1. specific super constructor
   2. superclass' constructor without parameters
   3. constructor by programmer in superclass
   4. constructor by default in superclass

![](https://www.hauchenglee.com/assets/images/java/invoke_constructor_order.png)

## modifier

### static

Static is a non-access modifier in Java. Any static member can be accessed before any objects of its class are created, and without reference to any object. 
It is applicable for the following:
1. blocks
2. variables
3. methods
4. nested classes

Static block:
- It can be used for static initializations of a class.
- It will executed exactly **only once** when the class is **first loaded**.
- Static block

```
class Demo {
    static int i;
    static {
        System.out.println("static 1");
    }
    Demo() {
        System.out.println("static constructor");
    }
    static {
        System.out.println("static 2, i = " + i);
    }
}

public class TestDemo {
    public static void main(String[] args) {
        Demo con = new Demo();
        System.out.println("main method");
        System.out.println("Demo.i = " + Demo.i);
    }
}

// static 1
// static 2, i = 0
// static constructor
// main method
// Demo.i = 0
```

Static variables:
- It is **class-level** variable instead of local variable.
- All instances of the class share the same static variable.

Static methods:
- They can only directly call other static methods.
- They can only directly access static data.
- They cannot refer to **this** or **super** in any way.

Static nested classes:
- Java allows us to define a class within another class which is called a nested class. The class which enclosed nested class is known as Outer class.
- In java, we can't make Top level class static. **Only nested classes can be static.**

> - [static keyword in java - GeeksforGeeks](https://www.geeksforgeeks.org/static-keyword-java/){:target="_blank"}

### final

Final data:
- final primitive: final makes the value a constant
- final reference: Once the reference is initialized to an object, it can **never be changed** to point to another object. 
However, the object itself can be modified.

Blank finals:
- Blank finals are final fields without initialization values. The compiler ensures that blank finals are initialized before use.
- You're forced to perform assignments to finals either with an expression at the point of definition of the field or in every **constructor**. 
This guarantees that the final fields is always initialized before use.

Final arguments:
- Inside the method you can **read and return** the argument, but you **can't change** it.
- `i + 1;` ok
- `i++;` not allow
- `final object = new Object();` not allow

Final methods
- The first reason is to put a "lock" on the method to **prevent an inheriting class from changing that method meaning by overriding it**. 
This is done for design reasons when you want to make sure that a method's behavior is retained during inheritance.
- The second reason is that final were suggested in the past is efficiency.

Final classes
- When you say that an entire class is final (by preceding its definition with the final keyword), you're **preventing all inheritance** from this class.
- The fields of a final class can be final or not, as you choose. The same rules apply to final for fields regardless of whether the class is defined as final. 
However, because it prevents inheritance, all methods in final class are **implicitly final**, since there's no way to override them. 
**You can include the final specifier to a method in a final class, but it doesn't add any meaning.**

> - On Java 8
> - See more information: [final vs immutability in java-string](https://www.hauchenglee.com/java-string/#final-vs-immutability)

## feature

### pass by value

Java is always **pass-by-value**.

> When an argument is passed by value, a copy of the argument’s value is passed to the
> called method. The called method works exclusively with the copy. Changes to the called
> method’s copy **do not affect** the original variable’s value in the caller.

```
public static void main(String[] args) {
    Dog aDog = new Dog("Max");
    Dog oldDog = aDog;

    // we pass the object to foo
    foo(aDog);

    // aDog variable is still pointing to the "Max" dog when foo(...) returns
    aDog.getName().equals("Max"); // true
    aDog.getName().equals("Fifi"); // false
    aDog = oldDog; // true    
}

public static void foo(Dog d) {
    d.getName().equals("Max"); // true

    // change d inside of foo() to point to a new Dog instance "Fifi"
    d = new Dog("Fifi");
    d.getName().equals("Fifi"); // true
}
```

In the example above `aDog.getName()` will return `"Max"`. The value `aDog` within `main` is not changed in the function `foo` with the `Dog Fifi` as the 
object reference is passed by value. If it were passed by reference, then the `aDog.getName()` in `main` would return `Fifi` after the call to `foo`.

Likewise:

```
public static void main(String[] args) {
    Dog aDog = new Dog("Max");
    Dog oldDog = aDog;

    foo(aDog);

    // when foo(...) returns, the name of the dog has been changed to "Fifi"
    aDog.getName().equals("Fifi"); // true

    // but it is still the same dog:
    aDog = oldDog; // true
}

public static void foo(Dog d) {
    d.getName().equals("Max"); // true

    // this changes the name of d to be "Fifi"
    d.setName("Fifi");
}
```

In the above example, `Fifi` is the dog's name after call to `foo(aDog)` because the object's name was set inside of `foo(...)`. Any operations 
that `foo` performs on `d` are such that, for all practical purposes, they are performed on `aDog`, but it is **not** possible to change the value of the variable 
`aDog` itself.

> - [methods - Is Java "pass-by-reference" or "pass-by-value"? - Stack Overflow](https://bit.ly/33g4W9q){:target="_blank"}

### equals and ==

<table>
    <thead>
        <tr>
            <th></th>
            <th>equals()</th>
            <th>==</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>primary</td>
            <td>x</td>
            <td>value</td>
        </tr>
        <tr>
            <td>reference</td>
            <td>object equals(): same as ==</td>
            <td>address comparsion</td>
        </tr>
        <tr>
            <td></td>
            <td>string equals(): content comparsion</td>
            <td></td>
        </tr>
    </tbody>
</table>

> - [Difference between == and .equals() method in Java - GeeksforGeeks](https://www.geeksforgeeks.org/difference-equals-method-java/){:target="_blank"}

---