---
layout: post
title: Java筆記- Polymorphism 多態
category: java
tags: [java]
---

## overload

Overloading allows different methods to have the same name, but different signatures where the signature can differ by the **number of** input parameters 
or **type of** input parameters or both.

We can overload:
- common method
- static method
- main() method

We cannot overload:
- methods on return type
- methods that differ only by static keyword

> - [Overloading in Java - GeeksforGeeks](https://www.geeksforgeeks.org/overloading-in-java/){:target="_blank"}

## override

Overriding is a feature that allows a subclass or child class to provide a specific implementation of a method that is already provided by one of its 
super-classes or parent classes. When a method in a subclass has the **same name, same parameters or signature and same return type(or sub-type)** as a method 
in its super-class, then the method in the subclass is said to *override* the method in the super-class.

![](http://www.hauchenglee.com/assets/images/java/overriding-in-java.png)

The version of a method that is executed will be **determined by the object that is used to invoke it**. If an object of a *parent class* is used to 
invoke the method, then the version in the parent class will be executed, but if an object of the *subclass* is used to invoke the method, then the version 
in the child class will be executed. In other words, **it is the type of the object being referred to ((not the type of the reference variable))** 
that determines which version of an overridden method will be executed.

\* Tips: 決定 override version 的調用規則 → 形態看左邊（父），運行看右邊（子）

-  Rules for method overriding:
   - subclass has the same name, same parameters or signature and same return type(or sub-type)
   - without override, it cannot invoke the unique method in subclass with the type which referred to parent class
   - the access modifier for an overriding **can allow more, but not less**, access than the overridden method
   - we can call parent class method in overriding method using **super** keyword
   - abstract methods in an interface or abstract class are meant to be overridden in derived concrete classes
- Reason:
   - subclass has unique feature method
   - this is one of the way by which java achieve Run Time Polymorphism
- Cannot be override:
   - private modifier
   - final methods
   - static methods
- Exception handling: see [override exception in java-except](http://www.hauchenglee.com/java/2019/11/03/java-except.html#override-exception)

See more information:<br>
> - [Overriding in Java - GeeksforGeeks](https://www.geeksforgeeks.org/overriding-in-java/){:target="_blank"}

## up casting

Upcasting is casting to a supertype, while downcasting is casting to subtype.
Upcasting is always allowed, but downcasting involves a type check and can throw a `ClassCastException`.

Example:
```
Animal animal = new Dog(); // this is upcasting
Dog dog = (Dog) animal; // this is downcasting
```

ClassCastException:
```
Animal animal = new Animal();
Cat cat = (Cat) animal; // ClassCastException
```

> - [java - What is the difference between up-casting and down-casting with respect to class variable - Stack Overflow](https://bit.ly/33cj3g0){:target="_blank"}


## polymorphism

Objects of different kinds can adhere to the same interface or set of operations.

polymorphism:
- 事物在運行過程中存在不同的狀態。（使用一個父類，可以操作多個子類）
- 具有 is-a 關係

實施前提:
- 要有繼承關係
- 子類要重寫父類的方法
- 父類引用指向子类别

Example:
```
class Animal {
    int num = 10;
    static int age = 20;

    public void eat() {
        System.out.println("animal eat");
    }

    public static void sleep() {
        System.out.println("animal sleep in static");
    }

    public void run() {
        System.out.println("animal run");
    }
}

class Cat extends Animal {
    int num = 80;
    static int age = 90;
    String name = "tomcat";

    public void eat() {
        System.out.println("cat eat");
    }

    public static void sleep() {
        System.out.println("cat sleep in static");
    }

    public void catchMouse() {
        System.out.println("cat catch mouse");
    }
}
```

TestUnit Demo:
```
class Demo_Test1 {
    public static void main(String[] args){
        Animal animal = new Cat();
        animal.eat();
        animal.sleep();
        animal.run();

        // animal.catchMouse();
        // log.info(animal.name)

        log.info(animal.num);
        log.info(animal.age);
    }
}
```

Console:
```
cat eat
animal sleep in static
animal run
10
20
```

以上的三段代码充分体现了多态的三个前提，即：
1. 存在继承关系：
Cat类继承了Animal类
2. 子类要重写父类的方法：
子类重写（override）了父类的两个成员方法eat()，sleep()。其中eat()是非静态的，sleep()是静态的（static）。
3. 父类数据类型的引用指向子类对象：
测试类Demo_Test1中`Animal animal = new Cat();`语句在堆内存中开辟了子类（Cat）的对象，并把栈内存中的父类（Animal）的引用指向了这个Cat对象

观察总结：
- 成员变量：编译看左边(父类)，运行看左边(父类)。
- 成员方法：编译看左边(父类)，运行看右边(子类)。动态绑定
- 静态方法：编译看左边(父类)，运行看左边(父类)。
(静态和类相关，算不上重写，所以，访问还是左边的)
只有非静态的成员方法，编译看左边，运行看右边

多态缺点：多态后不能使用子类特有的属性和方法

解决方法：强制转型`Cat cat = (Cat) animal;`

> - [JAVA的多态用几句话能直观的解释一下吗？ - 知乎](https://www.zhihu.com/question/30082151){:target="_blank"}

---