---
layout: post
title: Java - Thread Publication and Escape 對象的發佈與逸出
category: java
tags: [java]
---

## 對象的發佈與逸出

- 發佈（publishing）：能夠被當前作用域外的代碼所使用
- 逸出（escape）：一個對象在尚未準備好時就將它發佈

常見*逸出*的有下面幾種方式：
- 靜態域逸出
- `public`修飾的`get`方法
- 方法參數傳遞
- 隱式的`this`

以下分別說明：

- 靜態域逸出

```
// public修飾的靜態域，相當於發佈了這個對象
// person對象也間接發佈了
public static Set<Person> allPerson;

public void init() {
    allPerson = new HashSet<>();
}

// 在未調用init的時候，外面的代碼已經是可以使用allPerson這個對象了，所以已經逸出了
```

- public修飾get方法

```
class Unsafe {
    private String[] states = {};

    public String[] getStates() {
        return states;
    }

    // 本應該是私有的數據，被public修飾的get方法發佈出去了。
    // 任何人都可以通過get方法獲取states（states變量已經超出了當前作用域的範圍 -> 逸出）
    // 也就是說：state已不再安全
}
```

- 方法參數傳遞

因為把對象傳遞過去給另外的方法，已經是逸出了。

- this逸出

```
import java.util.ArrayList;
import java.util.List;

class ThisEscape {
    private final List<Event> listOfEvents;

    public ThisEscape(EventSource source) {
        source.registerListener(new EventListener() {
            @Override
            public void onEvent(Event event) {
                // 在構造方法還沒有完全結束的時候
                // 就調用了該對象的方法doSomething(event)
                doSomething(event);
            }
        });

        // lambda -> method reference
        source.registerListener(this::doSomething);

        // 此時對象還沒有初始化
        listOfEvents = new ArrayList<>();

    }

    void doSomething(Event event) {
        listOfEvents.add(event);
    }

    // 結果，在多線程的環境下，可能還沒有完全初始化listOfEvents對象，
    // 另一個線程就調用了doSomething()方法了，這是不合理的！
    // 這裡就是隱式地this引用逸出
    interface EventSource {
        void registerListener(EventListener eventListener);
    }

    interface EventListener {
        void onEvent(Event event);
    }

    interface Event { }
}
```

## 安全發佈對象

安全發佈對象有幾種常見的方式：
- 在靜態域中直接初始化：`public static Person = new Person();`
   - 靜態初始化由JVM在類的初始階段就執行了，JVM內部存在著同步機制，致使這種方式可以安全發佈對象
- 對應的引用保存到volatile或者AtomicReference引用中
   - 保證了該對象的引用可見性和原子性
- 由final修飾
   - 該對象是不可變的，那麼線程就一定是安全的
- 由鎖來保護
   - 發佈和使用的時候都需要加鎖，這樣才能保證能夠該對象不會逸出

---