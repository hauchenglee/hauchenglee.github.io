---
layout: post
title: 計算機概論
category: it
tags: [computer-science]
---

## 數字系統

數字表示、轉換：
- [第二章　電腦工作原理](http://www.chwa.com.tw/TResource/HS/book1/ch2/ch2-3-1.htm)
- [數字系統](http://www.chwa.com.tw/TResource/VS/book1/ch2/2-5.htm)

補數：
- [一補數與二補數](https://noob.tw/complements/)

補數計算：
- 正＋正  應為正數（負數為異常）
- 負＋負  應為負數（正數為異常）
- 正＋負  可正可負

補數異常：（overflow）：兩個負數相加，最左邊的符號位元相加結果變成 1

## 資料的表示方法

### 資料的儲存單位

電腦的資料單位：
1. bit：0 1
2. byte: 
    - 1 byte = 8 bit
    - 英文 = 1 byte / 中文 = 2 byte
3. word

電腦的容量單位：
- 1kb = 2^10 bytes = 10^3 bytes
- 1mb = 2^20 bytes = 10^6 bytes
- 1gb = 2^30 bytes = 10^9 bytes
- 1tb = 2^40 bytes = 10^12 bytes
- 1pb = 2^10 TB = 2^20 GB

109身心5：
> 4 大數據處理資料量已進入 PB 級容量單位，它等於 2 的 10 次方個 TB，也等於 2 的 20 次方個 GB，而 1GB > 大約是 10 的 9 次方位元組（Byte），那麼 1PB 可以概算為 10 的幾次方位元組？
>  
> (A) 9<br>
> (B) 12<br>
> (C) 15<br>
> (D) 18

109身心5：
> 25 某計算機有 64 GB（Gigabytes）記憶體，若一個 word 由 8 個位元組（byte）所構成，則此計算機需要幾位> 元的位址（address）才能定位記憶體中單一的 word？
>  
> (A) 33<br>
> (B) 34<br>
> (C) 35<br>
> (D) 36

### 文字資料的表示法

ASCII:
- 是生活中電腦最常見的編碼方式，也是鍵盤使用的編碼（109身心5）

同位檢查：
- 奇同位檢查：在每個資料最左邊位數加上0 or 1，使編碼中的1維持奇數
- 偶同位檢查：在每個資料最左邊位數加上0 or 1，使編碼中的1維持偶數

### 數值資料的表示法

浮點數

## 程式開發

### 流程圖


### 邏輯與布林

邏輯運算：
1. not: 相反
2. and: 同true為 1
3. or: 同false為 0
4. xor: tt ff 為 0
5. eqv: tt ff 為 1
6. imp: tf 為 0

NOT：
1. 產生一個與輸入相反的輸出信號，運算符號為上橫線
2. 上橫線A為A的補數
3. 上1 = 0，上0 = 1

XOR、XNOR：
- XOR：0110
- XNOR：1001

定理：
1. 單一律、交換律、結合律、分配律
2. 補數定律
3. 吸收定律
4. 對偶律

109關務4：
> 16 布林函數 A+BC 等於： <br>
> (A) (A+B)C<br>
> (B) AB+AC<br>
> (C) AB+AB+BC<br>
> (D) (A+B)(A+C) 

TODO 卡諾圖

## 資料處理

複雜度等級：
1. O(1)：常數時間(constant time)
2. O(log n)：次線性時間(sub-linear)
3. O(n)：線性時間(linear)
4. O(n log n)
5. O(n2)：平方時間(quadratic)
6. O(n3)：立方時間(cubic)
7. O(2n)：指數時間(exponential)
8. O(n!)：階乘時間(factorial)

時間複雜度：
- 選擇排序法：最佳O(n^2) ，最差O(n^2)，平均O(n^2)
- 插入排序法：最佳O(n)   ，最差O(n^2)，平均O(n^2)
- 氣泡排序法：最佳O(n)   ，最差O(n^2)，平均O(n^2) -> 左右兩兩比較
- 快速排序法：最佳O(n log n)，最差O(n^2)，平均O(n log n)

BST：
- 前序(Preorder)，搜索順序 根-左-右：1245367
- 中序(Inorder)，搜索順序 左-根-右：4251637
- 後序(Postorder)，搜索順序 左-右-根 4526731

遞歸：

109身心5：
> 36 給予一個如下演算法 A： <br>
> 則 A（5）的回傳值何者正確？ 
>
> ANS：
> A(0) = 1
> A(1) = 1
> A(2) = 2*A(1) + A(0) = 3
> A(3) = 2*A(2) + A(1) = 7
> A(4) = 2*A(3) + A(2) = 17
> A(5) = 2*A(4) + A(3) = 41 (答案)

## 計算機的基本結構

### 五大單元

1. 輸入
2. 記憶
    - 主記憶體：暫存器、RAM、ROM
    - 次記憶體：磁盤、光碟
3. 控制
4. 算術邏輯
5. 輸出

### 記憶體

依其容量大小與存儲速度：
1. 主記憶體
    - 暫存器（Register）
        - **程式計數器**：目前正在或即將被執行的指令的位址（110地特4）
        - 一般暫存器：是中央處理器內用來暫存指令、數據和位址的電腦記憶體
    - 半導體記憶體：
        - 隨機存取記憶體（RAM）
        - 唯讀記憶體（ROM）
2. 次記憶體
    - 磁盤
    - 光碟

隨機存取記憶體（RAM）
- 特性：揮發性記憶體
- 種類：
    - SRAM：快取記憶體
    - DRAM：需充電

存取速度快到慢：
1. 暫存器（最快）
2. 快取(L1>L2>L3)
3. DRAM(DDR3>DDR2>DDR>SDRAM)
4. ROM(Flash)
5. 硬碟(固態硬碟>混合碟>傳統硬碟)
6. 光碟
7. 軟碟
8. 磁帶

共享記憶體多處理器（shared memory multiprocessor）（110地特4）：
> - (A) 提供單一實體記憶體位置空間（address space）給多處理器使用
> - (B) 於多處理器上的所有程序（process）必須共享同一虛擬記憶體位置空間（address space）
> - (C) 若任一處理器存取任一記憶體中的一個字組（word），所花費的時間皆相同，則稱之為一致的記憶體存取> （uniform memory access, UMA）
> - (D) 若不同處理器存取記憶體中的同一個字組（word），所花費的時間可能不同，則稱之為非一致的記憶體存取（nonuniform memory access, NUMA）

### 電腦效能

- 時脈：指電腦內部一個計數器，每計數1次即是一個時脈週期
- 時脈週期：指CPU執行一個程序所花費的時脈週期
- 時脈速度：時脈計數的速度，單位為MHz or GHz

CPU時間 = CPU時脈週期 x 時脈週期時間

**時脈週期越大，速度越慢**

110地特4:
> CPU execution time 又稱為 CPU time。<br>
> CPU time = Clock cycles * Clock cycle time 。
>
> Clock cycles = ( 程式指令數 * 每個指令時脈數 )。<br>
> Clock cycle time = ( 1 / 時脈頻率 )。
>
> CPU time = ( 程式指令數  * 每個指令時脈數 ) * ( 1 /  時脈頻率 )

### 編程語言

由低到高：
1. 機器語言
2. 組合語言
3. 高階語言
4. 極高階語言
5. 自然語言

編譯器：
1. 組譯 assembler
2. 編譯 compiler
3. 解譯 interpreter

## 其他設備

### 匯流排

[匯流排 (數據) - 維基百科，自由的百科全書](https://zh.wikipedia.org/zh-tw/%E6%80%BB%E7%BA%BF)

### 傳輸媒介

1. USB
2. SCSI
3. RS-232

### 磁碟

RAID:
- [5張圖輕鬆介紹RAID磁碟陣列](https://raidnas.tw/hsinchu-nas-raid-explain-rescue/)

儲存架構：
1. SAN(storages area network)：儲存區域網路，爲專屬獨立的高速網路，連結多部伺服器，並提供共用的儲存裝置區，如同直接安裝在伺服器上的磁碟機
2. NAS(Network attached storage)：網路儲存設備=私有雲（home server），有自己的CPU/MB/RAM等基本伺服器架構，功能多元
3. DAS(Direct attached storage)：直接與伺服器相連的儲存裝置（外接硬碟）
4. Host attached storage：透過I/O port 直接存取的儲存裝置，例如，hard disk driver,CD DVD driver,RAID arrays

### 其他

- 磁帶
- 光碟
- 隨身碟：以下速度由快到慢
    - Thunderbolt 3
    - USB 3.1
    - SATA III
- 印表機：
    - CMYK：Cyan 青色、Magenta 洋紅色、Yellow 黃色、blacK 黑色
    - HSB：色相、飽和、亮度
    - RGB：紅綠藍

## 作業系統

### 資料處理系統迭代

1. 早起批次系統：單一user使用
2. 整批批次處理系統：累計一定的量批次處理
3. 連線作業處理系統：各終端機連線到中央處理器運算
4. 即時作業處理系統：一定是連線作業處理系統
5. 分時作業處理系統：二個user以上同時使用同一台處理速度很快的主機
6. 多元作業處理系統
7. 多處理系統
8. 分散處理系統

### 處理器之指令流

處理器之指令流與資料流分類：
1. SISD（Single Instruction stream, Single Data streams）
2. SIMD（Single Instruction stream, Multiple Data streams）
2. MIMD（Multiple Instruction streams, Multiple Data streams）

MIMD處理器可充分利用資料層級平行性（datalevel parallelism），因此當程式中有很多 case 或是 switch 敘述時，此類型處理器表現最好

### 多功能處理

109關務4：
> 3 硬體多緒處理（hardware multithreading）允許多個執行緒（threads）有效率地共用一個處理器。要允許上述的共用，處理器必須要支援可以迅速切換執行緒的能力。下列何者為處理器在進行執行緒切換時，所需要保存的個別執行緒的狀態？
>  
> (A) 快取記憶體的資料<br>
> (B) 記憶體的資料<br>
> (C) 暫存器與程式計數器（program counter）的資料<br>
> (D) 算數運算器的資料
>
> Ans：
> 不同的 Threads 會共用 Code、Data、File Section、OS Resource，而不同Threads則有自己獨立的 Register、Stack

### 常見系統問題

1. CPU優先權：
    - FSFS = FIFO 先到先做（不會造成飢餓現象）
    - SJF 最短工作先做，處理時間最短會被搶先（不會造成飢餓現象）
    - SRTF 最短剩餘時間優先，處理時間最短會被搶先（有飢餓可能）
    - SSTF 最短尋找樓層時間優先演算法（有飢餓可能）
    - Priority 設定每項任務優先順序
    - Round-Robin(RR) 所有的process皆使用一固定的CPU執行時間，若超過此一時間則被迫放棄CPU，等待下一個循環
    - SJF、SRTF：SJF的程序一但被CPU服務後就不會被其他程序插隊(non- preemptive)，而SRTF的程序即使已經進入執行狀態，仍可能被準備狀態的程序插隊(preemptive)。
2. 死鎖： -> 有搶先機制（例如xx優先）就有可能造成永遠排不到執行
    - 互斥
    - 佔用與等待
    - 禁止搶先
    - 循環等待
3. 分頁錯誤

(A)週期盜取(Cycle Stealing)：
- 當週邊設備要求進行主記憶體存取時，它中斷中央處理器， 用不著儲存中央處理器狀態，並使中央處理器延遲一個記憶體週期(Memory Cycle)，週邊設備利用這極短的時間 ，至主記憶體內存取一或二個位元組(Bytes)。而直接記憶體存取可以由多個週期盜取組合來實現

(B)輾轉現象(thrashing)：
- 是指行程(process)分頁替換的頻率相當高，於是系統的時間相耗費在輸入輸出(I/O)的動作，使CPU的使用率(utilization)低落，作業系統會認為需要增加多工的程度(the degree of multiprogramming)，而又將行程加入系統中，每個行程所分配到的實體記憶體又更少，因而惡性循環
- 資料來源：https://blog.pulipuli.info/2006/04/922-2.html?m=1

(C)貝勒地異常（Belady’s anomaly）：
- 一般來說在同一種分頁替換演算法當中，將記憶體容量增加以增加分頁可用欄位的時候，應該會減少分頁錯誤的次數；可是有時因為可用欄位的增加而降低了CPU的使用率，OS因而加入了更多分頁，導致可用欄位相對不足，分頁錯誤便沒有減少。這種違反一般趨勢的現象，即稱為布雷第異常現象。
- 資料來源：https://blog.pulipuli.info/2006/04/922-1.html?m=1

(D)內部碎裂 (Internal Fragmentation)：
- 作業系統配置給 process 的 memory 空間大於 process 真正所需的空間，這些多出來的空間該 process 用不到，而且也沒辦法供其他 process 使用，形成浪費。
- 資料來源：https://mropengate.blogspot.com/2015/01/operating-system-ch8-memory-management.html?m=1

## 資訊安全

對稱式加密：加解密都使用同一把金鑰，所以安全性較低

## 資料庫

- 正規化
- no sql

## 管理資訊系統

## 雲端

- 3種服務模式：SaaS、PaaS、IaaS
- 4種部署模式：公有雲、混合雲、私有雲、社群雲

物聯網：感知、網絡、應用

---