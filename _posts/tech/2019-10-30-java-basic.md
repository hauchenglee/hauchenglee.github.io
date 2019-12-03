---
layout: post
title: Java筆記-基礎語法
category: tech
tags: [java]
---

## Variable

### byte

bytes (2^n) → 字节（位元組） / bit → 字元（位元）

type|desc
---|---|
byte|1|
boolean|1|
char|2
int|4
float|4
long|8
double|8
string|depend on charset like ASCII of UTF-8 lead to different bytes

### conversion

- 小 &rarr; 大：隱性轉型，大 &rarr; 小：強制轉型

```
// large = small
double d = float f;
float f = (float) double d;
```

- 數值可以跟字元相互轉換
- 布林值不可以相互轉換
- Java 5 之後會自動轉換包裝類別

```
int a = 20;
Integer b = new Integer(3);

// converting int into Integer (primitive -> reference)
Integer i = Integer.valueOf(a);

// autoboxing, now compiler will write Integer.valueOf(a) internally
Integer j = a;

// converting Integer into int (reference -> primitive)
int x = b.intValue();

// unboxing, now compiler will write a.intValue() internally
int y = b;
```

### 作用域（scope）

<table>
    <thead>
        <tr>
            <th></th>
            <th>in method</th>
            <th>in block</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>static variable</td>
            <td>can access</td>
            <td>can access</td>
        </tr>
        <tr>
            <td>instance variable</td>
            <td>can access</td>
            <td>can access</td>
        </tr>
        <tr>
            <td>local variable</td>
            <td>can access</td>
            <td>can access until block need inside in method</td>
        </tr>
        <tr>
            <td>local variable in block</td>
            <td>N/A</td>
            <td>this variable only live within the block. example: if, for block</td>
        </tr>
    </tbody>
</table>

## Operator

### 算數運算符

- 加法 +
   - 字串相加的情況
   ```
   string  + string = string: "3 + 5 = " + 3 + 5 // console: 3 + 5 = 35
   integer + string = string: 3 + 5 + " = 5 + 5" // console: 8 = 5 + 5
   ```
- 減法 -
- 乘法 *
- 除法 /
- 餘法 %
   - 先不考慮兩個運算元的正負號，直接做餘數運算
   - 被除數的正負號，就是最終結果的正負號
    ```
    17 % 5 = 2
    -17 % 5 = -2
    ```
- 自增 ++
   - a++: evaluates a, then increments it (post-incrementation).
   - ++a: increments a, then evaluates it (pre-incrementation).
- 自減 \-\-
    ```
    int a = 1;
    int b = a++; // b = 1, a = 2
  
    a = 1;
    b = ++a; // b = 2, a = 2
  
    int x = 5, y = 5;
    System.out.println(++x); // console 6
    System.out.println(x);   // console 6
    System.out.println(y++); // console 5
    System.out.println(y);   // console 6
    ```

### 關係運算符

- 相等 ==
- 不相等 !=
- 大於 >
- 小於 <
- 大於或等於 >=
- 小於或等於 <=

### 位運算符

> Java定义了位运算符，应用于整数类型(int)，长整型(long)，短整型(short)，字符型(char)，和字节型(byte)等类型。
>
> 位运算符作用在所有的位上，并且按位运算。假设a = 60，b = 13;它们的二进制格式表示将如下：
> ```
> A = 0011 1100
> B = 0000 1101
> -----------------
> A&B = 0000 1100
> A | B = 0011 1101
> A ^ B = 0011 0001
> ~A= 1100 0011
> ```

### 邏輯運算符

- and or：
   - and 成立在前後條件式都要成立（true），結果才會為真
   - or  成立在前後條件式只要一個成立（true），結果就會為真
- 短路（&&、\|\|）與非短路（&、\|、^）：
   - 短路：The Conditional Operators which exhibit "short-circuiting" behavior.
   - 非短路：The bitwise Operator which is not short-circuiting.
- MySQL and or not：
<table>
    <thead>
        <tr>
            <th></th>
            <th colspan="4" style="text-align:center">b</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan="4" style="font-weight:bold">A</td>
            <td></td>
            <td>true</td>
            <td>false</td>
            <td>null</td>
        </tr>
        <tr>
            <td>true</td>
            <td>t</td>
            <td>f</td>
            <td>n</td>
        </tr>
        <tr>
            <td>false</td>
            <td>f</td>
            <td>f</td>
            <td>f</td>
        </tr>
        <tr>
            <td>null</td>
            <td>n</td>
            <td>f</td>
            <td>n</td>
        </tr>
    </tbody>
</table>

### 賦值運算符

- =
- +=
- -=
- *=
- /=
- %=
- <<=
- \>\>=
- &=
- ^=
- |=

### 條件運算符

`variable = booleanExpression ? valueWhenTrue : valueWhenFalse`

### instanceof 運算符

說明：

`object instanceof class` → return object is-a the class or not


使用格式：

`(object reference variable) instanceof (class/interface type)`

簡單例子：

```
String name = "miku"
boolean result = name instanceof String; // console: true
```

詳細例子：
```
class Vehicle {}

public class Car extends Vehicle {
    public static void main(String[] args){
        Vehicle a = new Car();
        boolean result =  a instanceof Car; // console: true
    }
}
```

## Traversal

### 時間複雜度（time complexity）

- see: [algorithms time complexity](http://hauchenglee.com/tech/2019/11/13/algorithms-time-complexity.html) - 時間複雜度

### 遞歸（recursive function）

```
// 1. sum of 1 + 2 + 3 + 4 + .... + 18 + 19 + 20
// 2. sum of 1 + 3 + 5 + .... + 17 + 19
// 3. sum of 2 + 4 + 6 + .... + 18 + 20

public class Recursive {
    private static int incrementRecursive(int x) {
        if (x == 1) {
            return 1;
        }
        return incrementRecursive(x - 1) + x;
    }

    private static int oddRecursive(int x) {
        if (x == 1) {
            return 1;
        }
        return oddRecursive(x - 2) + x;
    }

    private static int evenRecursion(int x) {
        if (x == 2) {
            return 2;
        }
        return evenRecursion(x -2) + x;
    }
}
```

## Array

### 一維數組

- 基本語法：
`type[] array_name = new type[]`

- Array → List：
```
Integer[] array = new Integer[] {1, 2, 3};
List<Integer> list = new Arrays.asList(array);
```

- List → Array：
```
List<String> list = new ArrayList<>();
list.add("element1");
list.add("element2");
String[] array = new String[list.size()];
array = list.toArray(array);
```

### 二維數組

- 基本語法：
`type[][] array_name = new type[row][column]`
- 二維數組：
   - 優點：簡單、易懂，在二維關係比較滿的時候省空間
   - 缺點：在二維關係不滿的時候，浪費空間

---