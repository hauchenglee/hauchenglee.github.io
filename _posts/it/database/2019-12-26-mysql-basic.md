---
layout: post
title: MySQL - SQL Basic 基礎
category: database
tags: [database]
---

## Basic

SQL命令：
- 不分大小寫（Case Insensitive）
- 關鍵字不可以分割成多行
- MySQL單雙引號皆可，但其它database只接受單引號
- 常數（Literal）：不變的數據，不儲存於數據庫內部

Ref:
- [database design - Reason why oracle is case sensitive? - Stack Overflow](https://bit.ly/2QA4ZIf){:target="_blank"}
- [Is SQL is case Sensitive or Insensitive? - Querychat](https://bit.ly/2QqoVx1){:target="_blank"}

## Data Queries

### Queries

<table>
    <thead>
        <tr>
            <th>Order</th>
            <th>SQL</th>
            <th>Desc</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>5</td>
            <td>SELECT</td>
            <td>
            指定查詢項目<br>
            給定特定欄位而非全部查詢
            </td>
        </tr>
        <tr>
            <td>1</td>
            <td>FROM</td>
            <td>數據來源</td>
        </tr>
        <tr>
            <td>2</td>
            <td>WHERE</td>
            <td>
            原始過濾<br>
            不可以有群組函數
            </td>
        </tr>
        <tr>
            <td>3</td>
            <td>GROUP</td>
            <td>
            數據分組<br>
            預設分組小到大
            </td>
        </tr>
        <tr>
            <td>4</td>
            <td>HAVING</td>
            <td>分組過濾</td>
        </tr>
        <tr>
            <td>6</td>
            <td>ORDER BY</td>
            <td>
            數值排序<br>
            預設小到大（ASC）
            </td>
        </tr>
        <tr>
            <td>7</td>
            <td>LIMIT</td>
            <td>
            MySQL特有語法
            </td>
        </tr>        
    </tbody>
</table>

<br>

查詢語法：

```sql
SELECT
  *
  [DISTINCT] column_name
  column_name, column_name, column_name...
  expression [alias]
FROM
  table_name AS [alias];
```

<br>

排除重複數據：

```sql
DESCRIBE table_name;
```

<br>

群組函數（Group / Aggregate Functions）
- `COUNT(column)`
- `MAX(column)`
- `MIN(column)`
- `SUM(column)`
- `AVG(column)`

### Join

連結運算方式（Join Types）：
- 交叉連結 Cross Join
- 內部連結 Inner Join
   - 自然連結 Natural Join
   - 相等連結 Equal Join
   - 不相等連結 Non-Equal Join
- 外部連結 Outer Join
   - 左外部連結 Left Outer Join
   - 右外部連結 Right Outer Join
   - 完全外部連結 Full Outer Join
- 自我連結 Self Join

<br>

**Cross Join**

```sql
SELECT *
FROM emp e CROSS JOIN dept d;
```

Result: total e * d records

<br>

**Inner Join**

```sql
SELECT ...
FROM dept d JOIN emp e ON d.deptno = e.deptno;
```

```sql
SELECT ...
FROM t1 JOIN t2 ON t1.columnA = t2.columnA
        JOIN t3 ON t2.columnB = t3.columnB
        JOIN t4 ON t3.columnB = t4.columnB;
```

<br>

**Natural Join**

```sql
SELECT ...
FROM table_name NATURAL JOIN table_name;
```

- 使用表格內之同名欄位作為連結條件
- 若欄位名稱形態不同時，則產生錯誤訊息（盡量少用`NATURAL JOIN`）

<br>

**Non-Equal Join**

```sql
SELECT a.empno, a.ename, a.sal, b.grade
FROM emp a JOIN salgrade b ON
     (a.sal BETWEEN b.losal AND b.hisal);
```

- 若符合條件的數據超過一筆以上時，數據可能會不正確
- 不好寫，幾乎很少用（95%用不到）

<br>

**Outer JOIN**

```sql
SELECT a.deptno, a.dname, b.empno, b.ename
FROM dept a LEFT OUTER JOIN emp b ON a.deptno = b.deptno;
```

- 指定一數據表，當連結條件符合時則顯示，若與第二個數據表都不符合時則以null value顯示
- `OUTER`可以省略不寫

<br>

**Self Join**

```sql
SELECT ...
FROM table A1 JOIN table A2 ON A1.column1 = A2.column2;
```

- 使用同一個表格做連結運算
- 一定使用表格別名來建立二個相同數據表的連接運算

### Inner Join vs Outer Join

**Inner Join**

![](https://hauchenglee.github.io/assets/images/it/database/inner-vs-outer-join_inner.png)

**Left Outer Join**

![](https://hauchenglee.github.io/assets/images/it/database/inner-vs-outer-join_outer-left.png)

**Right Outer Join**

![](https://hauchenglee.github.io/assets/images/it/database/inner-vs-outer-join_outer-right.png)

**Full Outer Join**

![](https://hauchenglee.github.io/assets/images/it/database/inner-vs-outer-join_full-outer.png)

Ref: [Inner Join vs Outer Join - Difference and Comparison - Diffen](https://www.diffen.com/difference/Inner_Join_vs_Outer_Join){:target="_blank"}

### Sub-Queries



## Transaction

在執行SQL語句的時候，某些業務要求一系列操作必須完整全部執行，而不能僅執行一部分。例如一個轉賬操作：

```sql
-- 
UPDATE accounts SET balance = balance - 100 WHERE id = 1;

-- 
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
```

這兩條SQL語句必須全部執行，或是只要任一條語句失敗，就必須全部撤銷。

數據庫事務具有ACID四個特性：
- 原子性（Atomic）：將所有SQL作為原子工作單位執行，要麼全部執行，要麼全部不執行。
- 一致性（Consistent）：事務完成後，所有數據的狀態都是一致的，即A賬戶只要減去了100，B賬戶必定加上了100.
- 隔離性（Isolation）：如果有多個事務並發執行，每個事務作出的修改必須與其它的事務隔離。
- 持久性（Duration）：即事務完成後，對數據的修改被持久化存儲。

<br>

**隱式事務 & 顯示事務**

【隱式事務（Auto Commit）】：對於單條SQL語句，數據庫系統自動將其作為一個事務執行。

【顯式事務（User Commit）】：手動把多條SQL語句作為一個事務執行，使用`BEGIN`開啟一個事務，使用`COMMIT`提交一個事務。

```sql
BEGIN:
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;
```

<br>

**`COMMIT` & `ROLLBACK`**

【`COMMIT`】：指提交SQL所有事務並持久性保存，如果執行失敗，整個事務也會失敗。

【`ROLLBACK`】：如果執行失敗，則取消或回復事務期間內所有的數據修改。

```sql
BEGIN:
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
ROLLBACK;
```

<br>

**隔離級別**

對於兩個並發執行的事務，如果涉及到操作同一條記錄的時候，可能會發生問題。因為並發操作會帶來數據的不一致性，包括髒讀、不可重複讀、幻讀等。

數據庫系統提供了隔離級別是的可以有針對性地選擇事務的隔離級別，避免數據不一致的問題。

<table>
    <thead>
        <tr>
            <th>Isolation Level</th>
            <th>髒讀<br>Dirty Read</th>
            <th>不可重複讀<br>Non Repeatable Read</th>
            <th>幻讀<br>Phantom Read</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Read Uncommitted</td>
            <td>Yes</td>
            <td>Yes</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>Read Committed</td>
            <td>-</td>
            <td>Yes</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>Repeatable Read</td>
            <td>-</td>
            <td>-</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>Serializable</td>
            <td>-</td>
            <td>-</td>
            <td>-</td>
        </tr>
    </tbody>
</table>

Ref: [事务 - 廖雪峰的官方网站](https://www.liaoxuefeng.com/wiki/1177760294764384/1179611198786848){:target="_blank"}

## DML

Data Manipulation Language:
- INSERT
- UPDATE
- DELETE

常見的錯誤：
- 違反數據檢查條件（Constraint）
   - 鍵值重複：違反主鍵（primary key）或唯一鍵（unique）
   - 違反外鍵（foreign key）
   - 對非空值（not null）的欄位給定空值
- 數據長度過長
- 數據個數不夠
- 數據形態錯誤

<table>
    <thead>
        <tr>
            <th></th>
            <th>父</th>
            <th>子</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>INSERT</td>
            <td>X</td>
            <td>V</td>
        </tr>
        <tr>
            <td>UPDATE</td>
            <td>V</td>
            <td>V</td>
        </tr>
        <tr>
            <td>DELETE</td>
            <td>V</td>
            <td>X</td>
        </tr>
    </tbody>
</table>

## DDL

Data Definition Language:
- CREATE
- ALTER
- DROP

<br>

數據檢查條件（Database Constrain）
- 欄位預設值（Default）
- 不允許空值（NOT NULL）
- 主鍵（Primary Key）：不可重複、不可NULL、只能唯一
- 外鍵（Foreign Key）：可以重複、可以NULL
- 唯一鍵（Unique Key）：不可重複、可以NULL

Ref: [sql - Can a foreign key be NULL and/or duplicate? - Stack Overflow](https://bit.ly/2Q1zxDq){:target="_blank"}

可在建立數據表時指定，或於數據表建立後加入
- 欄位階層（Column Level）
- 數據表階層（Table Level）

<br>

**主鍵（Primary Key）**

Column Level:

```sql
CREATE TABLE table_name
(
DEPTNO SMALLINT(4) PRIMARY KEY,
DNAME VARCHAR(14),
LOCAL VARCHAR(13)
);
```

Table Level:

```sql
CREATE TABLE table_name
(
DEPTNO SMALLINT(4) PRIMARY KEY,
DNAME VARCHAR(14),
LOCAL VARCHAR(13),
CONSTRAINT PK_DEPT122_DEPTNO PRIMARY KEY(DEPTNO)
);
```

- 命名原則：ConstraintType_TableName_ColumnName
- 不命名，系統預設給定。

**外鍵（Foreign Key）**

```sql
CREATE TABLE table_name
(
...,
fk_definition
[ON DELETE {RESTRICT | CASCADE | SET NULL}]
[ON UPDATE {RESTRICT | CASCADE | SET NULL}]
);
```

- ON DELETE：數據刪除時
- ON UPDATE：數據更新時
- RESTRICT：限制（預設）
- CASCADE：相依性（依賴）
- SET NULL：設為空值

<br>

**DELETE & TRUNCATE & DROP**

<table>
    <thead>
        <tr>
            <th>Statement</th>
            <th>DML vs DDL</th>
            <th>COMMIT & ROLLBACK</th>
            <th>Desc</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>WHERE</td>
            <td>DML</td>
            <td>can rollback</td>
            <td>
            刪除整個行以及結構<br>
            可以使用WHERE條件
            </td>
        </tr>
        <tr>
            <td>TRUNCATE</td>
            <td>DDL</td>
            <td>cannot rollback</td>
            <td>
            只會刪除內容，而不是結構<br>
            無法使用WHERE條件
            </td>
        </tr>
        <tr>
            <td>DROP</td>
            <td>DDL</td>
            <td>cannot rollback</td>
            <td>
            從數據庫中刪除表<br>
            所有表的行，索引和權限也將被刪除
            </td>
        </tr>
    </tbody>
</table>

Ref: [Difference between TRUNCATE, DELETE and DROP commands - Oracle FAQ](https://www.orafaq.com/comment/6629){:target="_blank"}

## DCL

Data Control language（DCL）

Level:
- Lv5: God
- Lv4: Server
- Lv3: Database
- Lv2: Table
- Lv1: Column

## Reference

學習路線：

- [MySQL有什么推荐的学习书籍？ - 知乎](https://www.zhihu.com/question/28385400){:target="_blank"}
- [MySQL 学习路线是怎样的？有哪些学习资料或网站推荐？ - 知乎](https://www.zhihu.com/question/20931204){:target="_blank"}

教程：

- [MySQL优化系列（一）--库与表基本操作以及数据增删改_Jack__Frost的博客-CSDN博客](https://blog.csdn.net/jack__frost/article/details/71194208){:target="_blank"}

數據表設計：

- [MySQL 数据库设计总结 - 云+社区 - 腾讯云](https://cloud.tencent.com/developer/article/1004367){:target="_blank"}

---