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

```
class FanXing<T> {
    public void a1() { }

    public void a2(T a) { }

    public void a3(T[] a) { }

    public <BB> void b(BB b) { }

    public static <BB> void s1(BB a) { }

    // FanXing.this cannot be referenced from a static content
    public static void s2(T a) { }

    public <BB> BB[] arr1(BB... r) { return r; }
}
```

說明：
> d方法，e方法 都是静态方法，但是一个可以一个不可以，为啥呢？因为静态方法在类被加载的时候就会被放入方法区了，
> e方法的BB参数，在类加载的时候也没有确定，无所谓的。但是d方法的AA参数，在这个方法被加载的时候，很可能还没有创建对象，
> 那么AA的类型还没有确定呢!所以直接写肯定是不行

> --[java泛型理解和深入 - 知乎](https://zhuanlan.zhihu.com/p/40925435){:target="_blank"}

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
![](http://www.hauchenglee.com/assets/images/tech/generic-extends.png)

`List <? super Fruit>`：List中所有元素都是Fruit的父类（包含本身）
![](http://www.hauchenglee.com/assets/images/tech/generic-super.png)

---