---
layout: post
title: Java 8 - Stream Creation 建立Stream流
category: it
tags: [java]
---

## Stream Creation

- 創建`stream`的實例後，不會修改元數據
- 因此允許從單個元數據創建多個實例

Ref: [The Java 8 Stream API Tutorial - Baeldung](https://www.baeldung.com/java-8-streams){:target="_blank"}

## Empty Stream

Syntax:

```
static <T> Stream<T> empty()
```

Return Value: empty stream

Note: An empty stream might be useful to **avoid null pointer exceptions** while callings methods with stream parameters.

Example:

```java
class GFG {
    public static void main(String[] args) {
        // Creating an empty Stream
        Stream<String> stringStream = Stream.empty();
        
        // Display elements in Stream
        stringStream.forEach(System.out::println);
    }
}
```

Ref: [Stream empty() in Java with Examples - GeeksforGeeks](https://www.geeksforgeeks.org/stream-empty-java-examples/){:target="_blank"}

## Stream of Collection `stream()`

### Array.asList()

Syntax

```
public static List asList(T... a)
```

- 參數：數組`Array`
- 回傳值：集合`List`
- 範例：

```java
class GFG {
    public static void main(String[] args) {
        // creating Arrays of String type
        String[] stringArray = new String[]{"A", "B", "C", "D"};

        // getting the list view of Array
        List<String> list = Arrays.asList(stringArray);
        
        // printing the list
        System.out.println("The list is: " + list);
    }
}
```

Output:

```console
The list is: [A, B, C, D]
```

Ref: [Arrays asList() method in Java with Examples - GeeksforGeeks](https://www.geeksforgeeks.org/arrays-aslist-method-in-java-with-examples/){:target="_blank"}

### Stream of Collection

可以創建任何類型的Collection（`Collection`、`List`、`Set`）

Example 1:

```
Collection<String> collection = Arrays.asList("a", "b", "c"); // creating a List instenceof Collection
String<String> streamOfCollection = collection.stream();
```

Example 2:

```java
import java.util.ArrayList;
import java.util.List;
import java.util.stream.Stream;

public class StreamBuilders {
    public static void main(String[] args) {
        List<Integer> list = new ArrayList<Integer>();

        for (int i = 1; i < 10; i++) {
            list.add(i);
        }

        Stream<Integer> stream = list.stream();
        stream.forEach(System.out::println);
    }
}
```

## Stream of Array `of()`

### Stream.of(val1, val2, val3...)

```java
import java.util.stream.Stream;

public class StreamBuilders {
    public static void main(String[] args) {
        Stream<Integer> stream = Stream.of(1, 2, 3, 4, 5, 6, 7, 8, 9);
        stream.forEach(System.out::println);
    }
}
```

### Stream.of(array)

```java
import java.util.stream.Stream;

public class StreamBuilders {
    public static void main(String[] args) {
        Integer[] intArray = new Integer[]{1, 2, 3, 4, 5, 6, 7, 8, 9};
        Stream<Integer> stream = Stream.of(intArray);
        stream.forEach(System.out::println);
    }
}
```

### Arrays.stream(array)

Syntax:

```
Arrays.stream（array）
Arrays.stream（array，start，end）
```

```java
import java.util.Arrays;
import java.util.stream.Stream;

public class StreamBuilders {
    public static void main(String[] args) {
        String[] arr = new String[]{"a", "b", "c"};
        Stream<String> streamOfArrayFull = Arrays.stream(arr);
        Stream<String> streamOfArrayPart = Arrays.stream(arr, 1, 3);
    }
}
```

### Compare `Stream.of()` & `Arrays.stream()`

- 對於基本數組（如`int[]`、`long[]`等），`Stream.of()`和`Arrays.stream()`具有不同的返回值。

這是`Stream.of()`的源碼：返回值是`Stream<T>`

```
    /**
     * Returns a sequential {@code Stream} containing a single element.
     *
     * @param t the single element
     * @param <T> the type of stream elements
     * @return a singleton sequential stream
     */
    public static<T> Stream<T> of(T t) {
        return StreamSupport.stream(new Streams.StreamBuilderImpl<>(t), false);
    }
```

```
    static final class StreamBuilderImpl<T>
            extends AbstractStreamBuilderImpl<T, Spliterator<T>>
            implements Stream.Builder<T> {

        /**
         * Constructor for a singleton stream.
         *
         * @param t the single element
         */
        StreamBuilderImpl(T t) {
            first = t;
            count = -2;
        }    
}

// p.s.
// >= 0 when building, < 0 when built
// -1 == no elements
// -2 == one element, held by first
// -3 == two or more elements, held by buffer
int count;

// The first element in the stream
// valid if count == 1
T first;
```

這是`Arrays.stream()`的源碼：返回值是`IntStream`

```
    /**
     * Returns a sequential {@link IntStream} with the specified array as its
     * source.
     *
     * @param array the array, assumed to be unmodified during use
     * @return an {@code IntStream} for the array
     * @since 1.8
     */
    public static IntStream stream(int[] array) {
        return stream(array, 0, array.length);
    }
```

```
    /**
     * Returns a sequential {@link IntStream} with the specified range of the
     * specified array as its source.
     *
     * @param array the array, assumed to be unmodified during use
     * @param startInclusive the first index to cover, inclusive
     * @param endExclusive index immediately past the last index to cover
     * @return an {@code IntStream} for the array range
     * @throws ArrayIndexOutOfBoundsException if {@code startInclusive} is
     *         negative, {@code endExclusive} is less than
     *         {@code startInclusive}, or {@code endExclusive} is greater than
     *         the array size
     * @since 1.8
     */
    public static IntStream stream(int[] array, int startInclusive, int endExclusive) {
        return StreamSupport.intStream(spliterator(array, startInclusive, endExclusive), false);
    }
```

這是範例：

```java
// Java program to demonstrate return type 
// of Arrays.stream() and Stream.of() method 
// for primitive arrays 

import java.util.Arrays;
import java.util.stream.IntStream;
import java.util.stream.Stream;

class GFG {
    public static void main(String[] args) {
        // Creating an integer array 
        int[] arr = {1, 2, 3, 4, 5};

        // --------- Using Stream.of() ---------

        // to convert int array into Stream
        Stream<int[]> stream = Stream.of(arr);

        // Displaying elements in Stream
        stream.forEach(System.out::print);

        System.out.println("\n");
        // --------- Using Arrays.stream() --------- 

        // to convert int array into Stream 
        IntStream intStream = Arrays.stream(arr);

        // Displaying elements in Stream 
        intStream.forEach(System.out::print);
    }
}
```

Output:

```console
[I@448139f0

12345
```

<br>

- 如果需要處理特殊的原始類型stream（例如`IntStream`、`LongStream`等），使用`Stream.of()`需要為其展開攤平（flattening ）

```java
// Java program to demonstrate need of flattenning
// Stream.of() method returned type for primitive arrays

import java.util.Arrays;
import java.util.stream.IntStream;
import java.util.stream.Stream;

class GFG {
    public static void main(String[] args) {
        // Creating an integer array
        int[] arr = {1, 2, 3, 4, 5};

        // --------- Using Arrays.stream() ---------

        // to convert int array into Stream
        IntStream intStream = Arrays.stream(arr);

        // Displaying elements in Stream
        intStream.forEach(System.out::print);

        System.out.println("\n");
        // --------- Using Stream.of() ---------

        // to convert int array into Stream
        Stream<int[]> stream = Stream.of(arr);

        // ***** Flattening of Stream<int[]> into IntStream *****

        // flattenning Stream<int[]> into IntStream
        // using flatMapToInt()
        IntStream intStreamNew = stream.flatMapToInt(Arrays::stream);

        // Displaying elements in IntStream
        intStreamNew.forEach(System.out::print);
    }
}
```

<br>

- `Stream.of()`是通用的（Generic），然而`Arrays.stream`只能處理原始類型

> `Arrays.stream()` method only works for primitive arrays of `int[]`, `long[]`, and 
> `double[]` type, and returns `IntStream`, `LongStream` and `DoubleStream` 
> respectively. For other primitive types, `Arrays.stream()` won't work. 
> On the other hand, `Stream.of()` returns a generic Stream of type `T (Stream)`. 
>
> Hence, it can be used with any type.

`Arrays.stream()`方法僅適用於`int[]`、`long[]`和`double[]`類型的原始數組，並分別返回`IntStream`、`LongStream`和`DoubleStream`。對於其他原始類型，`Arrays.stream()`將不起作用。

另一方面，`Stream.of()`返回類型`T (Stream)`的通用Stream。因此，它可以與任何類型一起使用。

<br>

`Stream.of()`用法：

```java
// Java program to demonstrate return type
// of Stream.of() method
// for primitive arrays of char

import java.util.stream.Stream;

class GFG {
    public static void main(String[] args) {
        // Creating a character array
        char[] arr = {'1', '2', '3', '4', '5'};

        // --------- Using Stream.of() ---------
        // Will work efficiently

        // to convert int array into Stream
        Stream<char[]> stream = Stream.of(arr);

        // Displaying elements in Stream
        stream.forEach(System.out::print);
    }
}
```

`Arrays.stream()`用法

```java
// Java program to demonstrate return type
// of Arrays.stream() method
// for primitive arrays of char

import java.util.Arrays;

class GFG {
    public static void main(String[] args) {
        // Creating a character array
        char[] arr = {'1', '2', '3', '4', '5'};

        // --------- Using Arrays.stream() ---------
        // This will throw error

        // to convert char array into Stream
        Arrays.stream(arr);
    }
}
```

Output:

```console
Compilation Error in java code :-
prog.java:20: error: no suitable method found for stream(char[])
Arrays.stream(arr);
^
```

Ref:
- [Difference between Stream.of() and Arrays.stream() method in Java - GeeksforGeeks](https://www.geeksforgeeks.org/difference-between-stream-of-and-arrays-stream-method-in-java/){:target="_blank"}
- [Arrays.stream() vs Stream.of()](http://mail.openjdk.java.net/pipermail/lambda-dev/2013-August/010771.html){:target="_blank"}

## Stream `builder()`

Syntax:

```
static <T> Stream.Builder<T> builder()
```

> Returns an instance of `Stream.Builder`, which can be used to add elements individually.

返回Stream.Builder的實例，該實例可用於單獨添加元素。

使用`builder`時候必須在其（`builder()`）右側指定的泛型形態，不然`build()`方法會創建通用的`Object`泛型。

好處：避免額外創建`ArrayList`的開銷。（without the copying overhead that comes from using an `ArrayList` as a temporary buffer.）

Example 1:

```
Stream<String> streamBuilder = Stream
    .<String>builder()
    .add("a")
    .add("b")
    .add("c")
    .build();
```

Example 2:

```java
import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.Stream;

public class BuilderExample {
    public static void main(String[] args) {
        Stream.Builder<String> builder = Stream.builder();
        Stream<String> stream = builder
                .add("one")
                .add("two")
                .add("three")
                .build();
        List<String> list = stream.map(String::toUpperCase)
                .collect(Collectors.toList());
        System.out.println(list);
    }
}
```

Example 3:

```java
// Java code for Stream builder() 

import java.util.stream.Stream;

class GFG {
    // Driver code 
    public static void main(String[] args) {
        // Using Stream builder() 
        Stream.Builder<String> builder = Stream.builder();

        // Adding elements in the stream of Strings 
        Stream<String> stream = builder.add("Geeks").build();

        // Displaying the elements in the stream 
        stream.forEach(System.out::println);
    }
}
```

Ref:
- [Stream.Builder (Java Platform SE 8 )](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.Builder.html){:target="_blank"}
- [Java 8 Streams - Stream.builder Examples](https://www.logicbig.com/how-to/code-snippets/jcode-java-8-streams-stream-builder.html){:target="_blank"}
- [Stream builder() in Java with Examples - GeeksforGeeks](https://www.geeksforgeeks.org/stream-builder-java-examples/){:target="_blank"}

## Stream `generate()`

Syntax:

```
static<T> Stream<T> generate(Supplier<T> s)
```

> This factory method returns an infinite sequential unordered stream, where each element is generated by the provide `Supplier`.

此工廠方法返回無限順序無序流，其中每個元素由`Supplier`形態的參數產生而來。

使用`generate()`必須指定Steam流的大小，否則`generate()`將創建直至達到內存限制（work until it reaches the memory limit）

Example 1:

```
Stream<String> streamGenerated =
  Stream.generate(() -> "element").limit(10);
```

Example 2:

```java
import java.util.stream.Stream;

public class GenerateExample {
    public static void main(String[] args) {
        Stream<String> stream = Stream.generate(() ->
                Double.toString(Math.random() * 1000)).limit(10);

        stream.forEach(System.out::println);
    }
}
```

Ref:
- [Java 8 Streams - Stream.generate Examples](https://www.logicbig.com/how-to/code-snippets/jcode-java-8-streams-stream-generate.html){:target="_blank"}

## Stream `iterate()`

創建無限流的另一種方法是使用`iterate()`方法

Example:

```java
import java.util.stream.Stream;

public class StreamIterateDemo {
    public static void main(String[] args) {
        Stream.iterate(0, n -> n + 1)
                .limit(10)
                .forEach(System.out::print);
    }
}
```

Output:

```console
0123456789
```

Ref:
- [Java 8 Stream.iterate examples – Mkyong.com](https://mkyong.com/java8/java-8-stream-iterate-examples/){:target="_blank"}
- [Java Streams - Stream.iterate()](http://www.java2s.com/Tutorials/Java_Streams/Tutorial/Streams/Stream_iterate_.htm){:target="_blank"}

## Stream of Primitives

> Java 8 offers a possibility to create streams out of three primitive types: `int`, `long` and `double`.
> As `Stream<T>` is a generic interface and there is no way to use primitives as a type parameter 
> with generics, there new special interfaces were created: **`IntStream`, **`LongStream`**, **`DoubleStream`**.
>
> Using the new interfaces alleviates unnecessary auto-boxing allows increased productivity.

因為`Stream<T>`無法傳入基本形態的泛型參數，為了減少不必要的裝箱、拆箱，因此創建了*IntStream*、*LongStream*、*DoubleStream*.



- IntStream:

```java
class IntStreamDemo {
    public static void main(String[] args) {
        System.out.println("--Using IntStream.rangeClosed--");
        IntStream.rangeClosed(2, 5).map(n -> n * n).forEach(s -> System.out.print(s + "\t"));

        System.out.println("\n--Using IntStream.range--");
        IntStream.range(2, 5).map(n -> n * n).forEach(s -> System.out.print(s + "\t"));

        System.out.println("\n--Sum of range 1 to 10--");
        System.out.println(IntStream.rangeClosed(1, 10).sum());

        System.out.println("\n--Sorted number--");
        IntStream.of(12, 14, 15, 2, 8).sorted().forEach(s -> System.out.print(s + "\t"));
    }
}
```

Output:

```console
--Using IntStream.rangeClosed--
4	9	16	25
--Using IntStream.range--
4	9	16 
--Sum of range 1 to 10--
55
--Sorted number--
2 4 8 13 15  
```

- LongStream
- DoubleStream

Methods are same like IntStream.

Ref: 
- [Java 8 IntStream, LongStream and DoubleStream Example](https://www.concretepage.com/java/jdk-8/java-8-intstream-longstream-doublestream-example){:target="_blank"}
- [A Guide to Streams in Java 8: In-Depth Tutorial with Examples](https://stackify.com/streams-guide-java-8/){:target="_blank"} #Stream-Specializations

## Stream of String

`String`也可以作為Steam流的元數據。

1. 通過`chars()`生成：

```
IntStream streamOfChars = "abc".chars();
```

2. 使用其他APIs，例如`spitAsStream()`：

```
Stream<String> streamOfStream =
  Pattern.compile(", ").splitAsStream("a. b. c");
```

## Stream of File

> Java NIO class `Files` allows to generate a `Stream<String>` of a text file through the `lines()` 
> method. Every line of the text becomes an element of the stream:

Java NIO类文件允许通过方法`lines()`生成文本文件的`Stream<String>`。文本的每一行都会变成stream的一个元素：

```
Path path = Paths.get("C:\\file.txt");
Stream<String> streamOfString = Files.lines(path);
Stream<String> streamWithCharset = Files.lines(path, Charset.forName("utf-8"));
```

---