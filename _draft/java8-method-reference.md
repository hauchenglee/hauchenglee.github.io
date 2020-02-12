---
layout: post
title: Java 8 - Method Reference
category: java
tags: [java]
---

## Overview

在之前lambda expression章節中，如果要調用method，我們可以這樣寫：

```
str -> System.out.println(str)
```

你可以使用method reference（方法參考/方法引用）：

```
System.out::println
```

可以看到`::`操作符，是lambda expression的特殊用法，也是語法糖的一種。

<br>

這裡有四種method references（方法參考/引用）：

- static methods: `Class::staticMethod`
- instance methods: `object::instanceMethod`
- constructor: `Class::new`


這是範例：

```java
import java.util.Arrays;
import java.util.Comparator;

public class MethodReference {
    public static void main(String[] args) {
        String[] stringArray = {"C", "D", "A", "E", "B"};

        // anonymous class
        Arrays.sort(stringArray, new Comparator<String>() {
            @Override
            public int compare(String o1, String o2) {
                return o1.compareTo(o2);
            }
        });

        // lambda expression
        Arrays.sort(stringArray, (o1, o2) -> o1.compareTo(o2));

        // method reference
        Arrays.sort(stringArray, String::compareTo);
    }
}
```

其實我覺得method reference並不直觀，有點太過語法糖了~ 編程不只是簡潔，有時也是需要兼顧閱讀性~

## Reference

- [Java 8 Method Reference](https://beginnersbook.com/2017/10/method-references-in-java-8/){:target="_blank"}
- [Method References in Java \| Baeldung](https://www.baeldung.com/java-method-references){:target="_blank"}
- [菜鳥工程師 肉豬: Java 8 Method References（方法參考/方法引用）的使用時機](https://matthung0807.blogspot.com/2018/08/java-8-method-references.html){:target="_blank"}

---