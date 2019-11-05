---
layout: post
title: Java筆記-String
category: tech
tags: [java]
---

## what is string

Here is Oracle's description:
> -- [String (Java Platform SE 7 )](https://docs.oracle.com/javase/7/docs/api/java/lang/String.html)

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
class Final {
    public static void main(String[] args){
        final StringBuffer sb = new StringBuffer("Hello");
        
        // Even though reference variable sb is final
        // We can perform any changes
        sb.append("GFG");
        
        // Here we will get compile time error
        // Because reassignment is not possible for final variable
        sb = new StringBuffer("Hello World");
    }
}
```

Pictorial Representation of the above Program

![](http://www.hauchenglee.com/assets/images/tech/final-vs-immutability.png)

> -- [final vs Immutability in Java - GeeksforGeeks](https://www.geeksforgeeks.org/final-vs-immutability-java/)

### final string



## string in memory



## string in concurrency

see: [Java Concurrency]()

## scanner

---