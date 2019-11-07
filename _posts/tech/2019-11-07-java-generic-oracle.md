---
layout: post
title: Java筆記-Generic 泛型 - Oracle Tutorial
category: tech
tags: [java]
---

> -- [Generic Methods (The Java™ Tutorials > Learning the Java Language > Generics](https://docs.oracle.com/javase/tutorial/java/generics/types.html){:target="_blank"}

## why use generics?

In a nutshell, generics enable *types* (classes and interfaces) to be parameters when defining classes, interfaces and methods. Much like the more familiar *formal parameters*
 used in method declarations, type parameters provide a way for you to re-use the same code with different inputs. The difference is that the inputs to formal parameters are
 values, while the inputs to type parameters are types.

Code that uses generics has many benefits over ono-generic code:

- Stronger type checks at compile time.<br>
  A Java compiler applies strong type checking to generic code and issues errors if the code violates type safety. Fixing compile-time errors is easier than fixing runtime
  errors, which can be difficult to find.
- Elimination of casts.<br>
  The following code snippet without generics requires casting:
 
 ```
 List list = new ArrayList();
 list.add("Hello");
 String s = (String) list.get(0);
 ```

When re-written generics, the code does not require casting:
```
List<String> list = new ArrayList<String>();
list.add("Hello");
String s = list.get(0); // no cast
```

- Enabling programmers to implement generic algorithms.<br>
  By using generics, programmers can implement generic algorithms that work on collections of different types, can be customized, and are type safe and easier to read.

## generic types

A *generic type* is generic class or interface that is parameterized over types. The following `Box` class will be modified to demonstrate the concept.

### a simple Box class

Begin by examining a non-generic `Box` class that operates on objects of any type. It needs only to provide two methods: `set`, which adds an object to the `Box`, and `get`,
 which retrieves it:

```
public class Box {
    private Object object;
    
    public void set(Object object) { this.object = object; }
    public Object get() { return object; }
}
```

Since its methods accept or return an `Object`, you are free to pass in whatever you want, provided that it is not one of the primitive types. There is no way to verify, at
 compile time, how the class is used. One part of the code may place an `Integer` in the box and expect to get `Integer`s out of it, while another part of the code may mistakenly
 pass in a `String` resulting in a runtime error.

### a generic version of the Box class

A generic class is defined with the following format:

`class name<T1, T2, ..., Tn> { /* ... */ }`

The type parameter section, delimited by angle brackets(<>), follows the class name, It specified the *type parameters* (also called *type variables*) `T1`, `T2`, ..., and `Tn`.

To update the `Box` class to use generic, you create a *generic type declaration* by changing the code "`public class Box`" to "`public class Box<T>`". This introduces the
 type variable, `T`, that can be used anywhere inside the class.

With this change, the `Box` class becomes:

```
/**
 * Generic version of the Box class
 * @param <T> the type of the value being boxed
 */
public class Box<T> {
    // T stands for "Type"
    private T t;
    
    public void set(T t) { this.t = t; }
    public T get() { return t; }
}
```

As you can see, all occurrences of `Object` are replaced by `T`. A type can be any **non-primitive** type you specify: any class type, any interface type, any array type, or
 even another type variable.

This same technique can be applied to create generic interfaces.

### type parameter naming conventions

By conventions, type parameter names are single, uppercase letters. This stands in sharp contrast to the variable naming conventions that you already know about, and with good
 reason: Without this convention, it would be difficult to tell the difference between a type variable and an ordinary class or interface name.

The most commonly used type parameter names are:

- E - Element (used extensively by the Java Collections Framework)
- K - Key
- N - Number
- T - Type
- V - Value
- S,U,V etc. - 2nd, 3rd, 4th types

You'll see these names used throughout the Java SE API and the rest of this lesson.

### invoking and instantiating a generic type

To reference the generic `Box` class form within your code, you must perform a *generic type invocation*, which replaces `T` with some concrete value, such as `Integer`:

`Box<Integer> integerBox;`

You can think of a generic type invocation as being similar to an ordinary method invocation, but instead of passing an argument to a method, you are passing a *type argument
 Integer* in this case -- to the `Box` class itself.

> **Type Parameter and Type Argument Terminology**: Many developers use the terms "type parameter" and "type argument" interchangeably, but these terms are not the same. When
 coding, one provides type arguments in order to create a parameterized type. Therefore, the `T` in `Foo<T>` is a type parameter and the `String` in `Foo<String> f` is a type
 argument. This lesson observes this definition when using these terms.

Like any other variable declaration, this code does not actually create a new `Box` object. It simply declares that `integerBox` will hold a reference to a `Box of Integer`,
 which is how `Box<Integer>` is read.

An invocation of a generic type is generally known as a *parameterized type*.

To instantiate this class, use the `new` keyword, as usually, but place <Integer> between the class name and the parenthesis:

`Box<Integer> integerBox = new Box<Integer>();`

### the diamond

(skip)

### multiple type parameters

As mentioned previously, a generic class can have multiple type parameters. For example, the generic `OrderedPair` class, which implements the generic `Pair` interface:

```
public interface Pair<K, V> {
    public K getKey();
    public V getValue();
}

public class OrderedPair<K, V> implements Pair<K, V> {
    
    private K key;
    private V value;

    public OrderedPair(K key, V value) {
        this.key = key;
        this.value = value;
    }

    @Override
    public K getKey() { return key; }

    @Override
    public V getValue() { return value; }
}
```

The following statements create two instantiations of the `OrderedPair` class:

```
Pair<String, Integer> p1 = new OrderedPair<String, Integer>("Even", 8);
Pair<String, String> p2 = new OrderedPair<String, String>("hello", "world");
```

The code, `new OrderedPair<String, Integer>`, instantiated `K` as a `String` and `V` as a `Integer`. Therefore, the parameter types of `OrderedPair`'s constructor
 are `String` and `Integer`, respectively. Due to [autoboxing](https://docs.oracle.com/javase/tutorial/java/data/autoboxing.html), it is valid to pass a `String`
 and an `int` to the class.

As mentioned in [The Diamond](#the-diamond), because a Java compiler can infer the `K` and `V` types from the declaration `OrderedPair<String, Integer>`, there
 statements can be shortened using diamond notation:

```
OrderedPair<String, Integer> p1 = new OrderedPair<>("Even", 8);
OrderedPair<String, String>  p2 = new OrderedPair<>("hello", "world");
```

To create a generic interface, follow the same conventions as for creating a generic class.

### parameterized types

You can also substitute a type parameter (i.e., `K` or `V`) with a parameterized type (i.e., List<String>). For example, using the `OrderedPair<K, V>` example:

`OrderedPari<String, Box<Integer>>` p = new OrderedPair<>("primes", new Box<Integer>(...));