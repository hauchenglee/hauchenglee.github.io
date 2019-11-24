---
layout: post
title: Java筆記-Exception 異常處理
category: tech
tags: [java]
---

## exception architecture

![](http://www.hauchenglee.com/assets/images/tech/throwable.png)

- `Error`：程序無法處理的錯誤。
- `Exception`：程序本身可以處理的異常。
   - unchecked (`RuntimeException`): All exception types that are direct or indirect subclasses of RuntimeException are unchecked exceptions.
   - checked: Java compiler enforces special requirements for checked exceptions (discussed momentarily). 
     An exception's type determines whether it's checked or unchecked.

<table>
    <thead>
        <tr>
            <th></th>
            <th>unchecked</th>
            <th>checked</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>mandatory inspection</td>
            <td>x</td>
            <td>v</td>
        </tr>
        <tr>
            <td>spring auto rollback</td>
            <td>v</td>
            <td>x</td>
        </tr>
        <tr>
            <td>example exception</td>
            <td>NullPointerException</td>
            <td>FileNotFoundException</td>
        </tr>
    </tbody>
</table>

## try-catch-finally

try-catch-finally 用法：
- try：用以捕獲異常，如果沒有`catch`塊，則必須跟一個`finally`塊
- catch：用以處理`try`捕獲的異常，可以接零個或多個`catch`塊
- finally：
   - 無論是否捕獲或處理異常，`finally`塊里的語句都會被執行
   - 當在`try`塊或`catch`塊遇到`return`語句時，`finally`語句將在**方法返回之前**執行，並且`finally`語句的返回值將會覆蓋原始的返回值

```
public static int f(int value) {
    try {
        return value * value;
    } finally {
        if (value == 2) {
            return 0;
        }
    }
}
```

如果調用`f(2)`，返回值將是0，而不是4，因為`finally`語句的返回值覆蓋了`try`語句塊的返回值

## try-with-resources

后用先关，先用后关

## override exception

- 父類別`throws`例外範圍 >= 子類別`throws`例外範圍
- 子類別可以丢`IOException`，但子類別不可以丢`Exception`，因為`Exception` > `IOException`

## exception message

Get exception message:
1. `public string getMessage()`: simple message
2. `public string toString()`: detailed message
3. `public string getLocalizedMessage`:
   - override this method with a subclass of `Throwable` to generate localized information
   - if the subclass does not override the method, it is the same as the `getMessage()` message
4. `public void printStackTrace()`: print the exception information encapsulated by the `Throwable` object

Send a stacktrace to log4j: `log.error( "failed!", e );`

---