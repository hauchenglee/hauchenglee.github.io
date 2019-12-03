---
layout: post
title: Algorithms筆記-時間複雜度
category: tech
tags: [algorithms]
---

## Overview

算法效率與函數的漸進增長影響：
1. 算法公式
2. 編譯產生的速度
3. 問題的輸入規模
4. 機器執行的速度

推導時間複雜度（Big-O notation）：
1. 找出跟問題規模有關的算法
2. 用常數1取代運行時間中的所有加法常數
3. 在修改後的運行次數函數中，只保留最高階項
4. 如果最高階存在且不是1，則去除與這個項相乘的常數
5. 得到的結果就是`Big-O`

時間複雜度速率圖：

> 14AnalysisOfAlgorithms.pdf page40

or

![](http://www.hauchenglee.com/assets/images/tech/classifications.png)

<br>

更詳細的時間複雜度分析表：
> -- [Big-O Algorithm Complexity Cheat Sheet (Know Thy Complexities!) @ericdrowell](https://www.bigocheatsheet.com/){:target="_blank"}

## Binary vs Linear Search

- Side by Side look

![](http://www.hauchenglee.com/assets/images/tech/binary-and-linear-search-animations.gif)

<br>

- Best Case Binary

![](http://www.hauchenglee.com/assets/images/tech/linear-vs-binary-search-best-case.gif)

<br>

- Worst Case Binary

![](http://www.hauchenglee.com/assets/images/tech/linear-vs-binary-search-worst-case.gif)

<br>

refer: [Binary Vs Linear Search Animated Gifs](https://www.mathwarehouse.com/programming/gifs/binary-vs-linear-search.php){:target="_blank"}

## Usually time complexity

以下筆記整理自：
> -- [如何清晰的理解算法中的时间复杂度？ - 知乎](https://www.zhihu.com/question/20196775/answer/693388880){:target="_blank"}

### O(1)

無論代碼執行了多少行，其他區域不會影響到操作，這個代碼的時間複雜度都是`O(1)`：

```
void swap(int &a, int &b) {
    int temp = a;
    a = b;
    b = temp;
}
```

### O(n)

在下面這段代碼，`for`循環裡面的代碼會執行n遍，因此它消耗的時間是隨著n的變化而變化的，因此可以用`O(n)`來表示它的時間複雜度：

```
int sum(int n) {
    int ret = 0;
    for (int i = 0; i <= n; i++) {
        ret += i;
    }
    return ret;
}
```

### O(n²)

當存在雙重循環的時候，即把`O(n)`的代碼再嵌套循環一遍，它的時間複雜度就是`O(n²)`了。

```
void selectionSort(int arr[], int n) {
    for (int i = 0; i < n; i++) {
        int minIndex = i;
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[minIndex]) {
                minIndex = j;
            }
        }
        swap(arr[i], arr[minIndex]);
    }
}
```

這裡簡單的推導一下：

- 當`i = 0`時，第二重循環需要運行`(n - 1)`次
- 當`i = 1`時，第二重循環需要運行`(n - 2)`次
- ...

不難得到公式：（`底 * 高 / 2`）

```
(n - 1) + (n - 2) + (n - 3) + ... + 0
= ((n - 1) + 0) * n / 2
= O(n^2)
```

### O(log n)

在二分查找法的代碼中，通過`while`循環，成2倍數的縮減搜索範圍，也就是說需要經過`log2^n`次即可跳出循環。

```
int binarySearch(int arr[], int n, int target) {
    int left = 0, right = n - 1;
    while (left <= r) {
        int mid = left + (right - 1) / 2;
        if      (arr[mid] > target) right = mid - 1;
        else if (arr[mid] < target) left = mid + 1;
        else                        return mid;
    }
    return -1;
}
```

更詳細的公式推導過程：
> -- [时间复杂度 O(log n) 意味着什么？ - 后端 - 掘金](https://juejin.im/entry/593f56528d6d810058a355f4){:target="_blank"}

### O(n log n)

將時間複雜度為`O(log n)`的代碼循環n遍的話，那麼它的時間複雜度就是`n * O(log n)`，也就是`O(n log n)`：

```
void nlogn() {
    for (int m = 1; m < n; m++) {
        i = 1;
        while (i < n) {
            i = i * 2;
        }
    }
}
```

## Recursive algorithm

如果遞歸函數中，只進行一次遞歸調用，遞歸深度為depth；在每個遞歸的遞歸的函數中，時間複雜度為T，則總體的時間複雜度為`O(T * depth)`。

在前面章節中，歸並排序與快速排序都帶有遞歸的思想，並且時間複雜度都是`O(n log n)`，但並不是有遞歸的函數就一定是`O(n log n)`，例如以下情況：

<table>
    <thead>
        <tr>
            <th>算法</th>
            <th>時間複雜度</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>二分查找法</td>
            <td>O(log n)</td>
        </tr>
        <tr>
            <td>求和</td>
            <td>O(n)</td>
        </tr>
        <tr>
            <td>求冪</td>
            <td>O(log n)</td>
        </tr>
        <tr>
            <td>歸並排序</td>
            <td>O(n log n)</td>
        </tr>
    </tbody>
</table>

## Advanced time complexity

以下筆記整理自：
> -- [算法复杂度分析（下）：最好、最坏、平均、均摊等时间复杂度概述 - Jonins - 博客园](https://www.cnblogs.com/jonins/p/9956752.html){:target="_blank"}

在數組`array`中尋找變量`x`第一次出現的位置，若沒有找到，則返回-1，否則返回位置下標。

```
int find(int[] array, int n, int x) {
    for (int i = 0; i < n; i++) {
        if (array[i] == x) {
            return i;
        }
    }
    return -1;
}
```

這段代碼根據`x`值的不同，時間複雜度也有區別：
1. `x = 1`：時間複雜度是`O(1)`
2. `1 <= x <= n`：時間複雜度是**不確定**的值，取決於`x`的值
3. `x > n`：時間複雜度是`O(n)`

這段代碼在不同情況下，其時間複雜度是不一樣的，所以為了描述在不同情況下的不同時間複雜度，有了**最好、最壞、平均時間複雜度**
- 最好情況的時間複雜度 best case time complexity
- 最壞情況的時間複雜度 worst case time complexity
- 平均時間的時間複雜度 average case time complexity
- 均攤時間複雜度 amortized time complexity

### best and worse case

最好、最壞情況時間複雜度指的是特殊情況下的時間複雜度。

> 最好情況時間複雜度就是在最理想情況下執行代碼的時間複雜度，它的時間是最短的；
>
> 最壞情況時間複雜度就是在最糟糕情況下執行代碼的時間複雜度，它的時間是最長的。

在這裡當數組中第一個元素就是要找的`x`時，時間複雜度是`O(1)`，也就是`x = 1`

而當數組中最後一個元素才是`x`時，時間複雜度是`O(n)`，也就是`n < x`

### average case

最好、最壞時間複雜度反應的是極端條件下的複雜度，發生的概率不大，不能代表平均水平。那麼，為了更好的表示平均情況下的算法複雜度，就需要引入平均時間複雜度。
平均情況時間複雜度可用代碼再所有可能情況下執行次數的加權平均值表示。

還是以上述[`find`](#best-and-worse-case-time-complexity)函數為例，從概率的角度看，`x`在數組中每一個位置的可能性是相同的，為`1 / n`，那麼平均情況時間複雜度就可以用下面的方式計算：

`((1 + 2 + 3 + ... + n) / n + n) / 2 = (3n + 1) / 4`

因此，`find`函數的平均時間複雜度為`O(n)`。

> 更詳細的公式推導過程：
>
> -- [算法复杂度分析（下）：最好、最坏、平均、均摊等时间复杂度概述 - Jonins - 博客园](https://www.cnblogs.com/jonins/p/9956752.html){:target="_blank"}

<br>

多數情況下，我們**不需要**區分最好、最壞、平均情況的時間複雜度，只有同一塊代碼在**不同情況下時間複雜度有量級差距**，我們才會區分3種情況，為了是**更有效的描述**代碼的時間複雜度。

### amortized time complexity

> 均摊复杂度是一个更加高级的概念，它是一种特殊的情况，应用的场景也更加特殊和有限。

> 摊还分析法
>
> 分析上述示例的平均复杂度分析并不需要如此复杂，无需引入概率论的知识。
>
> 因为通过分析可以看出，上述示例代码复杂度大多数为`O(1)`，极端情况下复杂度才较高为`O(n)`。同时复杂度遵循一定的规律，一般为1个`O(n)`，和n个`O(1)`。针对这样一种特殊场景使用更简单的分析方法：摊还分析法。
> 通过摊还分析法得到的时间复杂度为均摊时间复杂度。
>
> 大致思路：每一次`O(n)`都会跟着n次`O(1)`，所以把耗时多的复杂度均摊到耗时低的复杂度。得到的均摊时间复杂度为`O(1)`。
>
> 应用场景：均摊时间复杂度和摊还分析应用场景较为特殊，对一个数据进行连续操作，大部分情况下时间复杂度都很低，只有个别情况下时间复杂度较高。而这组操作其存在前后连贯的时序关系。
> 这个时候我们将这一组操作放在一起分析，将高复杂度均摊到其余低复杂度上，所以一般均摊时间复杂度就等于最好情况时间复杂度。
>
> 注意：均摊时间复杂度是一种特殊的平均复杂度（特殊应用场景下使用），掌握分析方式即可。

---