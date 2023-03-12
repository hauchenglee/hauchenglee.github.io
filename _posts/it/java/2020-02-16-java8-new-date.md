---
layout: post
title: Java 8 - New Date
category: it
tags: [java]
---

## Issues

1. 非線程安全：`Date` & `Calendar` classes 兩者都不是線程安全，造成在多線程環境難以調試。這是最大的問題
1. 架構設計不當：Java的日期/時間類的定義並不一致，在`java.util`和`java.sql`的包中都有日期類，此外用於格式化和解析的類在`java.text`包中定義
1. 時區處理麻煩：在Java 8以前的日期類並不提供國際化，沒有時區支持，所以開發者必須撰寫額外邏輯去處理時區問題，即使引入了`java.util.Calendar` & `java.util.TimeZone`類，
   仍然存在同樣上述問題，並且難以使用。而使用新的API，可以使用`Local`和`ZonedDate` / `Time` API完成時區的處理

## LocalDate, LocalTime and LocalDateTime

比較：

<table>
    <thead>
        <tr>
            <th></th>
            <th>LocalDate</th>
            <th>LocalTime</th>
            <th>LocalDateTime</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ISO format</td>
            <td>yyyy-MM-dd</td>
            <td>HH:mm</td>
            <td>yyyy-MM-dd HH:mm:ss</td>
        </tr>
    </tbody>
</table>

幾個比較常用的方法：

建立與解析DateTime：

```
LocalDate.now();

LocalDate.of(2020, 2, 17);

LocalDate.of(2020, Month.FEBRUARY, 17);
```

日期時間加減：(`plus*()` & `minus*()`)

```
localDate.plusDay(int dateCount);

localDate.plusMonths(int monthCount);

localDate.minusDays(int minCount);

localDate.minusWeeks(int weekCount);
```

比較時間先後：

```
date1.isAfter(date2);

date1.isBefore(date2);
```

使用ISO格式轉換：

```
LocalDateTime now = LocalDateTime.now();

now.format(DateTimeFormatter.ISO_DATE); // "2018-09-26"

now.format(DateTimeFormatter.ISO_DATE_TIME); // "2018-09-26T22:58:32.2290963"

now.format(DateTimeFormatter.ISO_LOCAL_DATE_TIME); // "2018-09-26T22:58:32.2290963"

now.format(DateTimeFormatter.BASIC_ISO_DATE); // "20180926"
```

使用自定義格式轉換：

```
LocalDateTime now = LocalDateTime.now(); // 2016-11-09T11:44:44.797

DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");

String formatDateTime = now.format(formatter); // 2016-11-09 11:44:44
```

解析：

```
String date = "2018-09-26";

String dateTime = "2018-09-26T22:24:33";

LocalDate localDate = LocalDate.parse(date);

LocalDateTime localDateTime = LocalDateTime.parse(dateTime);
```

Ref:
- [Working With LocalDate, LocalTime, and LocalDateTime - DZone Java](https://dzone.com/articles/working-with-localdate-localtime-and-localdatetime){:target="_blank"}
- [Java 8 – How to format LocalDateTime – Mkyong.com](https://mkyong.com/java8/java-8-how-to-format-localdatetime/){:target="_blank"}

## ZonedDateTime

建立特定時區：

```
ZoneId zoneId = ZoneId.of("[Asia/Shanghai]");
```

建立當地時區：

```
ZoneId currentZone = ZoneId.systemDefault();
```

取得所有時區的ID集合：

```
Set<String> allZoneIds = ZoneId.getAvailableZoneIds();
```

使用`parse()`取得時區的特定日期時間：

```
ZonedDateTime date1 = ZonedDateTime.parse("2015-12-03T10:15:30+05:30[Asia/Shanghai]");
```

將`LocalDateTime`轉換為特定時區：

```
ZonedDateTime zonedDateTime = ZonedDateTime.of(localDateTime, zoneId);
```

使用`OffsetDateTime`設定與UTC/Greenwich的時間偏移量：

> ZoneOffset 是一段时间内代表格林尼治时间/世界同一时间与时区之间的差异。他能表示一个特殊的差异时区偏移量

> OffsetDateTime 是一个带有偏移量的时间日期类。如果你的服务器处在不同的时区，他可以存入数据库中也可以用来记录某个准确的时间点

```
LocalDateTime localDateTime = LocalDateTime.of(2015, Month.FEBRUARY, 20, 06, 30);

ZoneOffset offset = ZoneOffset.of("+02:00");
 
OffsetDateTime offSetByTwo = OffsetDateTime.of(localDateTime, offset);
```

## Period and Duration

Period API 說明：

>  * A date-based amount of time in the ISO-8601 calendar system,<br>
>  * such as '2 years, 3 months and 4 days'.<br>
>  * <p><br>
>  * This class models a quantity or amount of time in terms of years, months and days.<br>
>  * See {@link Duration} for the time-based equivalent to this class.<br>
>  * <p>

Duration API 說明：

>  * A time-based amount of time, such as '34.5 seconds'.<br>
>  * <p><br>
>  * This class models a quantity or amount of time in terms of seconds and nanoseconds.<br>
>  * It can be accessed using other duration-based units, such as minutes and hours.<br>
>  * In addition, the {@link ChronoUnit#DAYS DAYS} unit can be used and is treated as<br>
>  * exactly equal to 24 hours, thus ignoring daylight savings effects.<br>
>  * See {@link Period} for the date-based equivalent to this class.<br>
>  * <p>

簡單來說，Period提供日期的持續時間偏移，而Duration提供一段時間的偏移。

```
LocalDate initialDate = LocalDate.parse("2007-05-10");

LocalDate finalDate = initialDate.plus(Period.ofDays(5));

LocalTime finalTime = initialTime.plus(Duration.ofSeconds(30));
```

兩個日期或時間的偏移量：

```
long five = ChronoUnit.DAYS.between(initialDate, finalDate);

long thirty = ChronoUnit.SECONDS.between(initialTime, finalTime);
```

## Date and Calendar

將`class` & `Calendar`轉換成new Date Time API：

```
LocalDateTime.ofInstant(date.toInstant(), ZoneId.systemDefault());
LocalDateTime.ofInstant(calendar.toInstant(), ZoneId.systemDefault());
```

將`Calendar`實例，並且提供時區ID，轉換為`ZonedDateTime`實例：

```
Calendar now = Calendar.getInstance();
ZonedDateTime zdt = ZonedDateTime.ofInstant(now.toInstant(), ZoneId.systemDefault());
```

`Date` & `Instant`之間的轉換：

```
Instant inst = date.toInstant();
Date newDate = Date.from(inst);
```

OR

Ref:
- [旧版日期时间代码 \| JAVA8 官网笔记教程](https://bit.ly/320pZgp){:target="_blank"}

## Reference

- [Java SE 8 Date and Time](https://www.oracle.com/technical-resources/articles/java/jf14-date-time.html){:target="_blank"}
- [Introduction to the Java 8 Date/Time API \| Baeldung](https://www.baeldung.com/java-8-date-time-intro){:target="_blank"}
- [Java 8 日期时间 API - Java 基础教程 - 简单教程，简单编程](https://www.twle.cn/l/yufei/java/java-basic-java8-datetime-api.html){:target="_blank"}
- [Java 8 日期时间 API 指南 \| Java 8 教程汇总](https://bit.ly/2HmJx5d){:target="_blank"}
- [Java SE 8 新的时间和日期 API \| Java 8 教程汇总](https://bit.ly/2OVlWN4){:target="_blank"}

---