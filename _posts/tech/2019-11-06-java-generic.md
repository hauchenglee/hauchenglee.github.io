---
layout: post
title: Java筆記-Generic 泛型
category: tech
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

![](http://www.hauchenglee.com/assets/images/tech/arraylist-oracle.png)

`ArrayList`就是一個泛型類，`E`就是一個泛型，具體的類型在創建集合對象的時候才確定。

### 泛型方法

> -- [Generic Methods (The Java™ Tutorials > Learning the Java Language > Generics (Updated))](https://docs.oracle.com/javase/tutorial/java/generics/methods.html){:target="_blank"}
>
> -- [Generic Methods (The Java™ Tutorials > Bonus > Generics)](https://docs.oracle.com/javase/tutorial/extra/generics/methods.html){:target="_blank"}
>
> -- [The Basics of Java Generics | Baeldung](https://www.baeldung.com/java-generics){:target="_blank"}
>
> -- [java泛型理解和深入 - 知乎](https://zhuanlan.zhihu.com/p/40925435){:target="_blank"}
>
> -- [java 泛型详解 - s10461的博客 - CSDN博客](https://blog.csdn.net/s10461/article/details/53941091){:target="_blank"}

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
![](http://www.hauchenglee.com/assets/images/tech/generic-extends.jpg)

`List <? super Fruit>`：List中所有元素都是Fruit的父类（包含本身）
![](http://www.hauchenglee.com/assets/images/tech/generic-super.jpg)

## 泛型通配符

以下資料整理自：
> -- [聊一聊 JAVA 泛型中的通配符 T，E，K，V，？ - 知乎](https://zhuanlan.zhihu.com/p/79162771){:target="_blank"}

### 常用的T, E, K, V, ?

### \<T\> <?>

### Class\<T\> 和 Class<?> 區別

---