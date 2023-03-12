---
layout: post
title: MySQL - SQL Optimizing 優化
category: it
tags: [database]
---

## Optimization SQL Statements

### 確定`SELECT`字段而不是`SELECT *`

如果表具有許多字段和許多行，則這將查詢大量不需要的數據。

Inefficient:

```sql
SELECT * FROM Customers
```

Efficient:

```sql
SELECT FirstName, LastName, Address FROM Customers
```

### 選擇更多字段以避免`SELECT DISTINCT`

`SELECT DISTINCT`是從查詢中去除重複項的便利方法，它通過對查詢中的所有字段進行`GROUP`來創建不同的結果。然而，為了實現該目標，需要大量的處理能力，並且數據可能會分組到不準確的程度。

Inefficient and inaccurate:

```sql
SELECT DISTINCT FirstName, LastName, Address FROM Customers
```

利用添加更多的字段，無需使用`SELECT DISTINCT`即可返回未重複的記錄。

Efficient and accurate:

```sql
SELECT FirstNanem, LastName, Address, City, State FROM Customers
```

### 使用`INNER JOIN`而不是`WHERE`創建關聯

此種join又稱作`CROSS JOIN`，將會產生A表格 * B表格平方總數的數據

假設，`Customers`有1000筆記錄，`Sales`有1000筆記錄，使用`CROSS JOIN`將產生1,000,000記錄量。

Bad:

```sql
SELECT Customers.CustomerID, Customers.Name, Sales.LastSaleDate
FROM Customers, Sales
WHERE Customers.CustomerID = Sales.CustomerID

```

應改用`INNER JOIN`，僅生成A表格相等的所需記錄。

Good:

```sql
SELECT Customers.CustomerID, Customers.Name, Sales.LasterSaleDate
FROM Customers
INNER JOIN Sales ON Customers.CustomerID = Sales.CustomerID
```

### 使用`WHERE`而不是`HAVING`條件查詢

與上述概念類似，有效的查詢，目標是僅從數據庫中提取所需的記錄。

根據SQL操作順序，在`WHERE`語句之後才計算`HAVING`語句。如果母的是根據條件過濾查詢，則`WHERE`會更有效。

例如，假設在2016年實現了200筆銷售，我們要查詢2016年每位客戶的銷售數量：

```sql
SELECT Customers.CustomerID, Customers.Name, COUNT(Sales.SalesID)
FROM Customers
INNER JOIN Sales ON Customers.CustomerID = Sales.CustomerID
GROUP BY Customers.CustomerID, Customers.Name
HAVING Sales.LastSaleDate BETWEEN #1/1/2016# AND #12/31/2016#
```

該方法從`Sales`表中提取1000條銷售幾率，然後過濾2016年生成的200條記錄，最後對數據進行計數。

使用`WHERE`限制提取的記錄數量：

```sql
SELECT Customers.CustomerID, Customers.Name, COUNT(Sales.SaleID)
FROM Customers
INNER JOIN Sales ON Customers.CustomerID = Sales.CustomerID
WHERE Sales.LastSaleDate BETWEEN #1/1/2016# AND #12/31/2016#
GROUP BY Customers.CustomerID, Customers.Name
```

該方法僅提取符合`WHERE`條件的200筆記錄，然後對數據進行計數。`HAVING`子句中的第一步已被消除。

僅當在聚合字段（aggregated field）進行過濾時，才應使用`HAVING`。例如，使用`HAVING`為銷售額大於5的客戶進行過濾：

```sql
SELECT Customers.CustomerID, Customers.Name, COUNT(Sales.SalesID)
FROM Customers
INNER JOIN Sales ON Customers.CustomerID = Sales.CustomerID
WHERE Sales.LastSaleDate BETWEEN #1/1/2016# AND #12/31/2016#
GROUP BY Customers.CustomerID, Customers.Name
HAVING COUNT(Sales.Sales) > 5
```

### 僅使用右模糊查詢

```sql
SELECT City FROM Customers
WHERE City LIKE '%Char%'
```

Results: **Char**leston, **Char**lotte, and **Char**lton. Also, Cape **Char**les, Crab Or**char**d, and Ri**chard**son.

更好的查詢：

```sql
SELECT City FROM Customers
WHERE City LIKE 'Char%'
```

Results: **Char**leston, **Char**lotte, and **Char**lton.

### 使用`LIMIT`返回指定記錄數

```sql
SELECT Customers.CustomerID, Customers.Name, COUNT(Sales.SalesID)
FROM Customers
INNER JOIN Sales ON Customers.CustomerID = Sales.CustomerID
WHERE Sales.LastSaleDate BETWEEN #1/1/2016# AND #12/31/2016#
GROUP BY Customers.CustomerID, Customers.Name
LIMIT 10
```

在某些DBMS中，`TOP`與`LIMIT`可以交互使用。

## Avoiding Full Table Scans

Avoiding Tables Scans

> A table scan is the reading of every row in a table and is caused by queries that don't properly use indexes. Table scans on large tables take an excessive 
> amount of time and cause performance problems.
>
> Make sure that, for any queries against large tables, **at least** one `WHERE` clause condition:
>
> - refers to an indexed column and 
> - is reasonable selective
>
> You should be concerned primarily with queries against large tables. If you have a table with a few hundred rows, table scans are not a problem and are 
> sometimes faster than indexed access.
>
> Ref: [Avoiding Table Scans - ORACLE](https://docs.oracle.com/cd/E23095_01/Platform.93/ATGInstallGuide/html/s0807avoidingtablescans01.html){:target="_blank"}

### Queries with NULL Conditions

【具有NULL條件的查詢】

Oracle無法使用索引選擇NULL列值，因為NULL未儲存在索引中。以下示例將調用全表掃描：

```sql
select
   emp_name
from
   emp
where
   middle_name IS NULL;
```

通過約定一些特殊含義（例如：`N/A`、`-1`）來代替NULL，避免全表掃描：

```sql
update
   emp
set
   middle_name to ‘N/A’
where
   middle_name IS NULL;
```

```sql
select
   emp_name
from
   emp
where
   middle_name = ‘N/A’;
```

<br>

MySQL:

> If a `WHERE` clause includes a `col_name` `IS NULL` condition for a column that is declared as `NOT NULL`, that expression is optimized away. 
> This optimization does not occur in cases when the column might produce `NULL` anyway (for example, if it comes from a table on the right side of a `LEFT JOIN`).

如果WHERE子句包含聲明為NOT NULL的列的col_name IS NULL條件，則該表達式將被優化。 如果列仍然可能產生NULL（例如，如果它來自LEFT JOIN右側的表），則不會進行此優化。

Ref: [MySQL :: MySQL 8.0 Reference Manual :: 8.2.1.15 IS NULL Optimization](https://dev.mysql.com/doc/refman/8.0/en/is-null-optimization.html)

<br>

結論：將欄位宣告`NOT NULL`，避免出現NULL值。

### Queries Against Unindexed Columns

【針對未索引列的查詢】

可以創建索引來提高查詢效能。

<br>

【`OR`語句使用不當引起全表掃描】

`WHERE`子句中比較的兩個條件，一個有索引，一個沒索引，使用`OR`則會引起全表掃描。例如：

```sql
WHERE A =: 1 OR B =: 2
```

A有索引，B沒有索引，則比較`B =: 2`會重新開始全表掃描。

Ref: [会引起全表扫描的十种SQL语句_sql_码上笔记-CSDN博客](https://blog.csdn.net/ak57193856/article/details/77345512)

### Queries with Like Conditions

【具有Like條件的查詢】

> Queries that use the like clause will invoke a full-table scan if the percent sign 
> mask is used in the leading side of the query. For example, the following clause 
> would not cause a full-table scan, because the like mask begins with characters 
> and the existing index will be able to service the query.

如果在查詢的開頭使用`%`，則使用`like`子句的查詢將調用全表掃描。例如，以下子句不會引起全表掃描，因為`like`查詢以字符（characters ）開頭，並且現有索引將能夠為查詢提供服務。

```sql
select
   ename
   job,
   hiredate
from
   emp
where
   ename like ‘BURLE%’;
```

> However, we run into full-table scans when the `like` mask has the percent sign in the beginning of the mask:

但是，當`like`查詢的開頭具有`%`時，我們將進行全表掃描：

```sql
select
   ename
   job,
   hiredate
from
   emp
where
   ename like ‘%SON’;
```

結論：右模糊會使用索引，左模糊無法使用索引。

可以使用`reverse + function index`的形式，變化成`LIKE '...%'`：

```sql
create index
   ename_reverse_idx
on emp
( reverse(ename) );
```

```sql
select
   ename
   job,
   hiredate
from
   emp
where
   reverse(ename) like ‘NOS%’;
```

### Queries with a Not Equals Condition

【條件不等於的查詢】

可以在Oracle SQL使用以下三種*NOT EQUALS*條件查詢：
- `select ename from emp where job <> 'boss'`
- `select ename from emp where job != 'boss'`
- `select ename from emp where job not in ('boss')`

然而，SQL中不等於操作符會限制索引，引起全表掃描，即使比較的字段上有索引。

解決辦法：通過把`NOT EQUALS`改成`OR`，可以使用索引，避免全表掃描。例如：

`column != 'aaa'`

改成

`column < 'aaa' OR column > 'aaa'`

Ref: [会引起全表扫描的十种SQL语句_sql_码上笔记-CSDN博客](https://blog.csdn.net/ak57193856/article/details/77345512){:target="_blank"}

## 結論

這是其他

1. 正確使用索引（index）
1. 欄位`NOT NULL`，並給出預設值
1. 使用正向且精確的查詢，代替反向查詢
1. 使用`UNION`代替`OR`
1. 盡量不在`WHERE`函數運算
1. `JOIN`查詢比`Sub-Query`快，但`Sub-Query`具有更好的閱讀可能性

子查詢 Performance：
- [sql - Join vs. sub-query - Stack Overflow](https://stackoverflow.com/questions/2577174/join-vs-sub-query){:target="_blank"}
- [SQL Joins Vs SQL Subqueries (Performance)? - Stack Overflow](https://stackoverflow.com/questions/3856164/sql-joins-vs-sql-subqueries-performance){:target="_blank"}

## Reference

Optimization:

- [MySQL :: MySQL 8.0 Reference Manual :: 8 Optimization](https://dev.mysql.com/doc/refman/8.0/en/optimization.html){:target="_blank"}
- [8 Ways to Fine-Tune Your SQL Queries (for Production Databases) - Sisense](https://www.sisense.com/blog/8-ways-fine-tune-sql-queries-production-databases/){:target="_blank"}
- [15個優化你的sql Query的方式 - Davidou的 Blog](http://blog.davidou.org/archives/609){:target="_blank"}

Full Table Scans:

- [MySQL :: MySQL 8.0 Reference Manual :: 8.2.1.23 Avoiding Full Table Scans](https://dev.mysql.com/doc/refman/8.0/en/table-scan-avoidance.html){:target="_blank"}
- [Preventing Unwanted Full-Table Scans - SQL Syntax and Full-Table Scans](http://www.remote-dba.net/t_op_sql_unwanted_table_scan.htm){:target="_blank"}

SQL Optimize Monitoring

- [史上最全SQL优化方案 - 掘金](https://juejin.im/post/5c4eb852e51d4506953e45be){:target="_blank"}
- [数据库优化 - SQL优化 - 掘金](https://juejin.im/post/5dbcf1ed51882522f345eb14){:target="_blank"}

---