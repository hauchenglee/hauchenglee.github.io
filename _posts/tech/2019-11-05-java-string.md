---
layout: post
title: Java筆記-String 字符串
category: tech
tags: [java]
---

## what is string

Here is Oracle's description:
> -- [String (Java Platform SE 7 )](https://docs.oracle.com/javase/7/docs/api/java/lang/String.html){:target="_blank"}

> ```
> public final class String
> extends Object
> implements Serizlizable, Comparable<String>, CharSequence   
> ```
>
> The String class represents character strings. All string literals in Java programs, such as "abc",
> are implemented as instances of this class.
>
> String are constants; **their values cannot be changed after they are created**. String buffers support
> mutable strings. Because String objects are immutable they can be shared. For example:
>
> `String str = "abc";`
>
> is equivalent to:
>
> ```
> char data[] = {'a', 'b', 'c'};
> String str = new String(data);
> ```
>
> Here ae some more examples of how strings can be used:
> ```
> System.out.println("abc");
> String cde = "cde";
> System.out.println("abc" + "cde");
> String c = "abc".substring(2, 3);
> String d = cde.substring(1, 2);
> ```
>
> ...
>
> The Java language provides special support for the string concatenation operator (+). and fro conversion of
> other objects to strings. String concatenation is implemented through the `StringBuilder` (or `StringBuffer`)
> class and its append method.

Above all, there have some emphasis about string, such as:
1. string literal vs string object?
2. why string is final, and it cannot be changed?
3. different between char and string?
4. how about string append to other string?
5. different between `StringBuilder` and `StringBuffer`?

## final string in java

### final vs immutability

**final**: In Java, final is a modifier which is used for class, method and variable also. When a variable is declared
 with final keyword, it's value can't be modified, essentially, a constant.
 
 **immutability**: In simple terms, immutability means unchanging over time or unable to be changed. In Java, we know
 that String objects are immutable means we cant change anything to the existing String objects.

Here is differences between final and immutability:

 <table>
     <thead>
         <tr>
             <th></th>
             <th>final</th>
             <th>immutability</th>
         </tr>
     </thead>
     <tbody>
         <tr>
             <td>reference</td>
             <td>x</td>
             <td>v</td>
         </tr>
         <tr>
             <td>value</td>
             <td>v</td>
             <td>x</td>
         </tr>
         <tr>
             <td>modifier applicable for</td>
             <td>variable but not for objects</td>
             <td>an object but not for variables</td>
         </tr>
     </tbody>
 </table>

For example:

```
final StringBuffer sb = new StringBuffer("Hello");

// Even though reference variable sb is final
// We can perform any changes
sb.append("GFG");

// Here we will get compile time error
// Because reassignment is not possible for final variable
sb = new StringBuffer("Hello World");
```

Pictorial Representation of the above Program

![](http://www.hauchenglee.com/assets/images/tech/final-vs-immutability.png)

> -- [final vs Immutability in Java - GeeksforGeeks](https://www.geeksforgeeks.org/final-vs-immutability-java/){:target="_blank"}

### why string is final

> -- [Why String is Immutable or Final in Java](https://javarevisited.blogspot.com/2010/10/why-string-is-immutable-or-final-in-java.html){:target="_blank"}

## string in memory

JVM divides the allocated memory to a Java program into two parts. One is **stack** and another one is **heap**. 
Stack is used for execution purpose and heap is used for storage purpose. In that heap memory, JVM allocates some memory specially meant for string literals. 
This part of the heap memory is called **String Constant Pool**.

(See more information in [java-memory](http://www.hauchenglee.com/tech/2019/12/04/java-memory.html) 

In the other hand:
- create a string object using string literal: object is stored in the **string constant pool**.
- create a string object using new keyword: object is stored in the **heap memory**.

<table>
    <thead>
        <tr>
            <th></th>
            <th>string literal</th>
            <th>new keyword</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>stored in</td>
            <td>string constant pool</td>
            <td>heap memory</td>
        </tr>
        <tr>
            <td>exapmle</td>
            <td>String s1 = "abc"</td>
            <td>String s2 = new String("def");</td>
        </tr>
    </tbody>
</table>

This is how String Constant Pool looks like in the memory:
![](http://www.hauchenglee.com/assets/images/tech/string-in-memory-allotment.png)

> When you create a string object using string literal, JVM first checks the content of be created object. If there exist an
> object in the pool with the same content, then the reference of that object. It does not create new object. If the content
> is different from the existing objects then only it creates new objects.
>
> But, when you create string objects using new keyword, a new object is created whether the content is same or not.
>
> --[How The Strings Are Stored In The Memory?](https://javaconceptoftheday.com/how-the-strings-are-stored-in-the-memory/){:target="_blank"}

See another information: [Guide to Java String Pool - Baeldung](https://www.baeldung.com/java-string-pool){:target="_blank"}

Prove:
```
String s1 = "abc";
String s2 = "abc";
System.out.println(s1 == s2);

String s3 = new String("abc");
String s4 = new String("abc");
System.out.println(s3 == s4);
```

The console is:
```
true
false
```

## string in concurrency

See: [Java Concurrency]()

## scanner

### the scanner bug

Problem:

The following code fragment asks users for their name and age:

```
System.out.print("What is your name? ");
name = in.nextLine();
System.out.print("What is your age? ");
age = in.nextInt();
Sysout.out.printf("Hello %s, age %d\n", name, age);
```

The output might look something like this:

`Hello Grace Hopper, age 45`

When you read a `String` followed by an `int`, everything works just fine. But when you read an `int` followed by a `String`, 
something strange happens.

```
System.out.print("What is your age? ");
age = in.nextInt();
System.out.print("What is your name? ");
name = in.nextLine();
Sysout.out.printf("Hello %s, age %d\n", name, age);
```

Try running this example code. It doesn't let tou input name, and it immediately displays the output:

`What is you name? Hello , age 45`

To understand what is happening, you have to understand that the `Scanner` doesn't see input as multiple lines, like we do.

![](http://www.hauchenglee.com/assets/images/tech/the-scanner-bug.png)

When you call `nextInt`, it reads characters until it gets to a non-digit. At this point, `nextInt` return 45. 
The program then displays the prompt "`What is you name? `" and calls `nextLine`, which reads characters until it gets to a **newline**. 
But since the next character is already a newline, `nextLine` returns the empty string "".

To solve this problem, you need an extra `nextLine` after `nextInt`.

```
System.out.print("What is your age? ");
age = in.nextInt();
in.nextLine(); // read the newLine
System.out.print("What is your name? ");
name = in.nextLine();
Sysout.out.printf("Hello %s, age %d\n", name, age);
```

This technique is common when reading `int` or `double` values that appear on their own line. First you read the number, 
and then you read the rest of the line, which is just a newline character.

> --Think Java / by Allen B. Downey and Chris Mayfield

### next and nextLine

next():
- only reads the characters until it encounters a blank space.
- `next()` places the cursor in the same line after reading the input.

`"Hello World"` → `"Hello"`

nextLine():
- reads input including space between the words till end of line \n.
- once the input is read, `nextLine()` positions the cursor in the next line.
- for reading the entire line, it is better to use `nextLine()`

`"Hello World!"` → `"Hello World"`

---