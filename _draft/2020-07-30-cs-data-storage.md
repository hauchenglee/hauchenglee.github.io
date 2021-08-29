---
layout: post
title: CS - Data Storage
category: tech
tags: [tech]
---

## Summary

[計算機概論 - 臺大開放式課程 (NTU OpenCourseWare)](http://ocw.aca.ntu.edu.tw/ntu-ocw/index.php/ocw/cou/101S210){:target="_blank"}

- 單元1‧Introduction & Data Storage
- 單元2‧Data Manipulation
- 單元3‧Operating Systems
- 單元4‧Networking and the Internet
- 單元5‧Algorithms
- 單元6‧Programming Languages
- 單元7‧Data Abstractions
- 單元8‧Database Systems 
- 單元9‧Computer Graphics
- 單元10‧Artificial Intelligence 
- 單元11‧Theory of Computations 

Sources:
- [參考資料](https://www.hauchenglee.com/assets/docs/ntu-ocw/101S210_CA01R01.doc){:target="_blank"}
- [教材檔 - Data Storage](https://www.hauchenglee.com/assets/docs/ntu-ocw/101S210_CS01L01.pdf){:target="_blank"}

## Binary

### Boolean operations & gates

![](https://www.hauchenglee.com/assets/images/tech/cs/AND-OR-XOR-NOT.png)

### Binary

binary → decimal

![](https://www.hauchenglee.com/assets/images/tech/101S210_CS01L01/101S210_CS01L01-18.png)

![](https://www.hauchenglee.com/assets/images/tech/101S210_CS01L01/101S210_CS01L01-19.png)

decimal → binary

![](https://www.hauchenglee.com/assets/images/tech/101S210_CS01L01/101S210_CS01L01-20.png)

### Hexadecimal

![](https://www.hauchenglee.com/assets/images/tech/101S210_CS01L01/101S210_CS01L01-7.png)

## Binary Operation

### Addition

![](https://www.hauchenglee.com/assets/images/tech/101S210_CS01L01/101S210_CS01L01-23.png)

### Subtraction

负数表示法有二种：
1. Two's complement notation: 二补数法
2. Excess: 偏移表示法

**Two's complement notation**:

加法直接用，比大小重新设计

![](https://www.hauchenglee.com/assets/images/tech/101S210_CS01L01/101S210_CS01L01-24.png)

![](https://www.hauchenglee.com/assets/images/tech/101S210_CS01L01/101S210_CS01L01-26.png)

![](https://www.hauchenglee.com/assets/images/tech/cs/Properties+of+Two’s+Complement+Notation.jpg)

Ref: [Chapter 5 Data representation. - ppt video online download](https://slideplayer.com/slide/6174101/){:target="_blank"}

**Excess**:

加法对人类不直接、对电脑直觉；比大小直接用

![](https://www.hauchenglee.com/assets/images/tech/101S210_CS01L01/101S210_CS01L01-27.png)

**Bin Pattern**

![](https://www.hauchenglee.com/assets/images/tech/cs/bin-pattern.png)

### Overflow

Desc: 电脑在运算加减乘除，其结果超过该表的表示范围，导致换算成Binary时查表后无法正确响应。

![](https://www.hauchenglee.com/assets/images/tech/101S210_CS01L01/101S210_CS01L01-28.png)

### Fraction

小数有两种表示法：
1. Fixed-Point 固定小数点
2. Float-Point 浮点数

**Fixed-Point**

不容易科学记号表示

![](https://www.hauchenglee.com/assets/images/tech/101S210_CS01L01/101S210_CS01L01-29.png)

**Float-Point**

![](https://www.hauchenglee.com/assets/images/tech/101S210_CS01L01/101S210_CS01L01-30.png)

![](https://www.hauchenglee.com/assets/images/tech/101S210_CS01L01/101S210_CS01L01-31.png)

截断错误：Truncation Errors
规范化形式：Normalized Form

## Memory & Storage

### Random Access Memory (RAM)

非揮發性記憶體主要有以下類型：

- ROM（Read-only memory，唯讀記憶體）
   - PROM（Programmable read-only memory，可規化唯讀記憶體）
   - EAROM （Electrically alterable read only memory，電可改寫唯讀記憶體）
   - EPROM（Erasable programmable read only memory，可擦可規化唯讀記憶體）
   - EEPROM（Electrically erasable programmable read only memory，電可擦可規化唯讀記憶體）
- Flash memory（快閃記憶體）

隨機存取記憶體(RAM)：可以隨時更改記憶體內的資料非永久性質。主要可分為下列幾種:
- 靜態隨機存取記憶體 (SRAM): 用於速度較快的快取記憶體
- 動態隨機存取記憶體 (DRAM): 主要用於電腦的主記憶體
- 視訊隨機存取記憶體 (VRAM): 加速螢幕的顯示速度

### Capacity
1. Kilobyte: 2^10 bytes = 1,024 bytes ~= 10^3 bytes.
2. Megabyte: 2^20 bytes = 1,048,576 bytes ~= 10^6 bytes.
3. Gigabyte: 2^30 bytes = 1,073,741,824 bytes ~= 10^9 bytes.

example: 32 bits pc 最多可以有 2^30 * 2^2 = 1G * 4 = 4G RAM

<br>

- [108經濟部所屬事業機構新進職員甄試A_資訊_計算機原理、網路概論#80782](https://bit.ly/3fgI3Yv){:target="_blank"}

2\. 若要定址32M記憶體，最少需使用幾條位址線？<br>
(A) 25<br>
(B) 26<br>
(C) 27<br>
(D) 28.

Ans:

32M = 32 \* 2^20 bytes = 25 \* 2^20 = 2^25 (需25位元來定址每一個位元組)

---