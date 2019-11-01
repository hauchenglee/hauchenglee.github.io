---
layout: post
title: java筆記-基礎語法
category: java
tags: [java]
---

## 基本數值運算

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

## 操作符

### 算數運算符


* 加法 +
   * 字串相加的情況
   ```
   string  + string = string:
       "3 + 5 = " + 3 + 5 // console: 3 + 5  = 35
   integer + string = string:
       3 + 5 + " = 5 + 5" // console: 8 = 5 + 5
   ```
* 減法 -
* 乘法 *
* 除法 /
* 餘法 %
   * 先不考慮兩個運算元的正負號，直接做餘數運算
   * 被除數的正負號，就是最終結果的正負號
    ```
     17 %  5 =  2
    -17 %  5 = -2
     17 % -5 =  2
    -17 % -5 = -2
    ```
* 自增 ++
   * a++: evaluates a, then increments it (post-incrementation).
   * ++a: increments a, then evaluates it (pre-incrementation).
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
* 自減 --

### 關係運算符

* 相等 ==
* 不相等 !=
* 大於 >
* 小於 <
* 大於或等於 >=
* 小於或等於 <=

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

* and or：
   * and 成立在前後條件式都要成立（true），結果才會為真
   * or  成立在前後條件式只要一個成立（true），結果就會為真
* 短路（&&、||）與非短路（&、|、^）：
   * 短路：The Conditional Operators which exhibit "short-circuiting" behavior.
   * 非短路：The bitwise Operator which is not short-circuiting.
* MySQL AND OR NOT<br>
![](http://www.hauchenglee/assets/images/tech/MySQL_AND_OR_NOT.jpg)

### 賦值運算符

* =
* +=
* -=
* *=
* /=
* %=
* <<=
* \>\>=
* &=
* ^=
* |=

### 條件運算符

`variable = booleanExpression ? valueWhenTrue : valueWhenFalse`

### instanceof 運算


---