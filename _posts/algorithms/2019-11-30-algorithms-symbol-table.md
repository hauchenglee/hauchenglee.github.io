---
layout: post
title: Algorithms - 符號表
category: algorithms
tags: [algorithms]
comments: true
toc: true
---

## Symbol table

符號表（Symbol Table）是一個非常常見的數據結構，在現實生活中應用很多。它是一個key-value對應的結構。
在符號表中，存儲的是鍵值對。通過輸入key，查詢對應的value。

### applications

![](https://www.hauchenglee.com/assets/images/algorithms/symbol-table-app.png)

### api

符號表的操作，就是基本的CRUD

![](https://www.hauchenglee.com/assets/images/algorithms/symbol-table-api.png)

<br>

這裡有幾個約定：
- key is not null
- if key is not exist, get() will return null
- pull() will cover old value

### key and value

**Value**

在Java中如果我們想要實現一個符號表，我們希望它是支持所有[泛型](https://www.hauchenglee.com/java-generic/)的。

<br>

**Key**

對於Key，我們希望：
- Key是可比較的，即是`Comparable`的，並且使用`compareTo()`函數
- Key是泛型的
- 能用`equals()`判斷相等，能用`hashCode()`獲取Key（這兩個函數都是Java的內置函數）
- 對於Key最好是使用不可變的類型（`Integer`、`Double`、`String`之類的）

<br>

**相等性測試**

如果我們提到了`equals()`函數，就不得不提Java的相等性測試，Java要求`equals()`滿足：
- 自反性：`x.equals(x)` is true
- 對稱性：`x.equals(y)` is true if and only if `y.equals(x)` is true
- 傳遞性：if `x.equals(y)` and `y.equals(z)` are true, then so is `x.equals(z)`
- 不為`null`：`x.equals(null)` return false

<br>

一般來說，我們做判斷的`(x == y)`這個式子並不做類型檢查。所以，我們在實現`equals()`的時候要特別注意，它看起來很簡單，但是想實現完美比較麻煩。

比如一個日期的class，可能會這樣實現`equals()`

```
public class Date implements Comparable<Date> {
    private final int month;
    private final int day;
    private final int year;

    public Date(int month, int day, int year) {
        this.month = month;
        this.day = day;
        this.year = year;
    }

    public boolean equals(Date that) {
        if (this.day   != that.day  ) return false;
        if (this.month != that.month) return false;
        if (this.year  != that.year ) return false;
        return true;
    }
}
```

<br>

但是這樣實現會更好：
- 用`final`字段，不然可能違反對稱性（繼承）
- 最好用`Object`（不過這個專家們目前還在激烈爭論中）
- 加入自反性，不為`null`的判斷，並且驗證類型一致性
- 關於比較一個類中的不同變量：
   - 如果比較的是一個原始類型，用`==`
   - 如果比較的是一個對象，用`equals()`
   - 如果比較的是一個數組，對`Arrays.equals(a, b)`而不是`a.equals(b)`

```
public boolean equals(Object y) {
    if (y == this)                       return true;
    if (y == null)                       return false;
    if (y.getClass() != this.getClass()) return false;
    Date that = (Date) y;
    if (this.day   != that.day  ) return false;
    if (this.month != that.month) return false;
    if (this.year  != that.year ) return false;
    return true;
}
```

## Unordered linked list

這應該是最簡單的實現方案了，我們只用一個無序鏈錶就可以實現符號表了

![](https://www.hauchenglee.com/assets/images/algorithms/sequential-search.png)

<br>

我們可以分析出這個算法的性能：

![](https://www.hauchenglee.com/assets/images/algorithms/unordered-list-cost.png)

可以看出，性能很差。

## Ordered array

我們還可以用兩個有序數組來實現ST（一個Key數組，一個Value數組，相互對應），
如果數組有序，那麼我們可以考慮用二分查找提高查找效率，這樣查找複雜度可以提高到`logN`

但是有序數組帶來一個問題，插入比較麻煩，插入的時候需要移動交換多次到正確的地方。

![](https://www.hauchenglee.com/assets/images/algorithms/binary-search.png)

<br>

我們可以看出，用有序數組的話，插入效率並沒有提升很多，但是查找效率可以提高到`logN`

![](https://www.hauchenglee.com/assets/images/algorithms/order-array-cost.png)

## Ordered symbol tables

> 上面提到了可以用有序數組實現符號表提高查找效率，以後會有算法能提高插入效率。
> 這裡先引入一個新的概念：有序符號表。

其實也很好理解，就是符號表有序。那有序的符號表有什麼特殊的呢？最大的不一樣就是實現了一些有序操作。

例如以下的API：

![](https://www.hauchenglee.com/assets/images/algorithms/ordered-symbol-table-api.png)

<br>

我們看看如果使用上面兩種初等實現有序表的功能，性能怎麼樣。

![](https://www.hauchenglee.com/assets/images/algorithms/binary-search-cost.png)

可以看出二分查找效率還是高很多。

<br>

**Binary Search**

之所以將Key保留在有序數組中，是為了使我們可以使用數組索引來顯著減少每一搜索所需的比較次數，並使用一種古老的經典算法（*binary search*）。
基本思想很簡單：我們維護已排序key數組中的索引，這些索引界定了可能包含search key的子數組。為了進行搜索，我們將search key與子數組中間的key進行比較。
- 如果search key < the key in the middle → 在子數組的左半部分搜索
- 如果search key > the key in the middle → 在子數組的右半部分搜索
- 否則the key in the middle == search key

![](https://www.hauchenglee.com/assets/images/algorithms/binary-search-rank.png)

## Reference

- [Elementary Symbol Tables](https://algs4.cs.princeton.edu/31elementary/){:target="_blank"}
- [《 常见算法与数据结构》符号表ST（1）——基本介绍 - Voidsky - CSDN博客](https://blog.csdn.net/hk2291976/article/details/51406050){:target="_blank"}
- [《 常见算法与数据结构》符号表ST（2）——初等实现分析和有序符号表 - Voidsky - CSDN博客](https://blog.csdn.net/hk2291976/article/details/51406756){:target="_blank"}

---