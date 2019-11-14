---
layout: post
title: Java筆記-Date Time 日期時間
category: tech
tags: [java]
---

> -- [Date-Time Design Principles (The Java™ Tutorials > Date Time > Date-Time Overview)](https://docs.oracle.com/javase/tutorial/datetime/overview/design.html){:target="_blank"}

## Date-Time Design Principles

The Date-Time API was developed using several design principles.

### Clear

The methods in the API are well defined and their behavior is clear and expected. For example, invoking a Date-Time method will a `null` parameter value typically triggers a `NullPointerException`.

### Fluent

The Date-Time API provides a fluent interface, making the code easy to read. Because most methods do not allow parameters with a `null` value and do not return a `null` value, method calls can be
 chained together and the resulting code can be quickly understood. For example:

```
LocalDate today = LocalDate.now();
LocalDate payDay = today.with(TemporalAdjusters.lastDayOfMonth()).minusDays(2);
```

### Immutable

Most of the classes in the Date-Time API create objects that are **immutable**, meaning that, after the object is created, it cannot be modified. To alter the value of an immutable object, a new
 object must be constructed as a modified copy of the original. This also means that the Date-Time API is, by definition, **thread-safe**. This affects the API in that most of the methods used to
 crate date or time objects are prefixed with `of`, `from`, or `with`, rather than constructors, and there are no `set` methods. For example:

```
LocalDate dateOfBirth = LocalDate.of(2012, Month.May, 14);
LocalDate firstBirthday = dateOfBirth.plusYears(1);
```

### Extensible

The Date-Time API is extensible wherever possible. For example, you can define your own time adjusters and queries, or build your own calendar system.

## Java 8 new Date and Time

(coming soon...)

## Excel

- Rule of excel datetime:
[Excel: convert text to date and number to date](https://www.ablebits.com/office-addins-blog/2015/03/26/excel-convert-text-date/){:target="_blank"}

- Deal with read excel datetime value problem by Java POI:
[Java處理Excel中的日期格式 - IT閱讀](https://www.itread01.com/content/1549318509.html){:target="_blank"}

- How to fix:
[Converting Number representation of Date in excel to Date in java - Stack Overflow](https://stackoverflow.com/questions/19028192/converting-number-representation-of-date-in-excel-to-date-in-java){:target="_blank"}

---