---
layout: post
title: Java - Generic 泛型
category: java
tags: [java]
---

## 泛型基本

用法：限定傳入參數形態

最基本的用法

```
ArrayList<String> list = new ArrayList<>();
list.add("a");
list.add("b");
list.add("c");
list.add(1); // error
```

## 泛型進階

### 泛型類

API 中的示例：

![](https://hauchenglee.github.io/assets/images/it/java/arraylist-oracle.png)

`ArrayList`就是一個泛型類，`E`就是一個泛型，具體的類型在創建集合對象的時候才確定。

### 泛型方法

Generic methods are those that are written with a single method declaration and can be called with arguments of different types. 
The compiler will ensure the correctness of whichever type is used. These are some properties of generic methods:

- Generic methods have a type parameter (the diamond operator enclosing the type) before the return type of the method declaration
- Type parameters can be bounded
- Generic methods can have different type parameters separated by commas in the method signature
- Method body for a generic method is just like a normal method

An example of defining a generic method to convert an array to a list
```
public <T> List<T> fromArrayToList(T[] a) {
    return Arrays.stream(a).collect(Collectors.toList());
}
```

In the previous example, the `<T>` in the method signature implies that the method will be dealing with generic type `T`. 
This is needed even if the method is returning void.

As mentioned above, the method can deal with more than one generic type, where this is the case, all generic types muse be added 
to the method signature, for example, if we want to modify the above method to deal with type `T` and type `G`, it should be 
written like this:

```
public static <T, G> List<G> fromArrayToList(T[] a, Function<T, G> mapperFunction) {
    return Arrays.stream(a)
      .map(mapperFunction)
      .collect(Collectors.toList());
}
```

We're passing a function that converts an array with the elements of type `T` to list with elements of type `G`. An example would be 
to convert `Integer` to its `String` representation:

```
@Test
public void givenArrayOfIntegers_thanListOfStringReturnOK() {
    Integer[] intArray = {1, 2, 3, 4, 5};
    List<String> stringList = Generics.fromArrayToList(intArray, Object::toString);
    
    assertThat(stringList, hasItems("1", "2", "3", "4", "5"));
}
```

It is worth noting that Oracle recommendation is to use an uppercase letter to represent a generic type and to choose a more descriptive letter 
to represent formal types, for example in Java Collections `T` is used for type, `K` for key, `V` for value.

> - [The Basics of Java Generics - Baeldung](https://www.baeldung.com/java-generics){:target="_blank"}

Here have more generics information:

> - [java泛型理解和深入 - 知乎](https://zhuanlan.zhihu.com/p/40925435){:target="_blank"}
> - [java 泛型详解 - s10461的博客 - CSDN博客](https://blog.csdn.net/s10461/article/details/53941091){:target="_blank"}

### 泛型接口

泛型接口：
```
public interface Response<T> {
    T getBeanResponseBody();

    T getListResponseBody();

    T getMapResponseBody();

    T getJSONResponseBody();

    T getStringResponseBody();

    T getExceptionResponseBody();
}
```

實現接口的時候，泛型種類不確定，那麼實現類也不確定
```
public class ResponseFactory<T> implements Response<T> {
    @Override
    public T getBeanResponseBody() {
        return (T) new BeanResponseBody();
    }

    @Override
    public T getListResponseBody() {
        return (T) new ListResponseBody();
    }

    @Override
    public T getMapResponseBody() {
        return (T) new MapResponseBody();
    }

    @Override
    public T getJSONResponseBody() {
        return (T) new JSONResponseBody();
    }

    @Override
    public T getStringResponseBody() {
        return (T) new StringResponseBody();
    }

    @Override
    public T getExceptionResponseBody() {
        return (T) new ExceptionResponseBody();
    }
}
```

使用`Autowired`創建對象，並傳入泛型指定形態：
```
@Autowired
private Response<BeanResponseBody> beanFactory;

@Autowired
private Response<ListResponseBody> listFactory;

BeanResponseBody responseBody = beanFactory.getBeanResponseBody();

ListResponseBody responseBody = listFactory.getListResponseBody();
```

## 泛型界限

`List <? extends Fruit>`：`List`中所有元素都是`Fruit`的子类（包含本身）
![](https://hauchenglee.github.io/assets/images/it/java/generic-extends.jpg)

<br>

`List <? super Fruit>`：List中所有元素都是Fruit的父类（包含本身）
![](https://hauchenglee.github.io/assets/images/it/java/generic-super.jpg)

> - [Java 泛型 <? super T> 中 super 怎么 理解？与 extends 有何不同？ - 知乎](https://www.zhihu.com/question/20400700){:target="_blank"}

## 泛型通配符

以下資料整理自：
> - [聊一聊 JAVA 泛型中的通配符 T，E，K，V，？ - 知乎](https://zhuanlan.zhihu.com/p/79162771){:target="_blank"}

### 常用的T, E, K, V, ?

### 泛型 \<T\> 和 泛型 <?>

### Class\<T\> 和 Class<?>

## Reference

> - [8. Generics - Learning Java, 4th Edition [Book]](https://www.oreilly.com/library/view/learning-java-4th/9781449372477/ch08.html){:target="_blank"}

---