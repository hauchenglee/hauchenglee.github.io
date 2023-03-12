---
layout: post
title: Clean Code 無暇的程式碼- Ch2 有意義的命名
category: it
tags: [design-pattern]
---

## 有意義名稱

Bad:

```
int priceTable[] = new int[16];
```

Good:

```
static final int PRICE_TABLE_MAX = 16;
int price Table[] = new int[PRICE_TABLE_MAX];
```

原則上 0 不用於魔法值，這是因為 0 經常被用作數組的最小下標或者變量初始化的預設值。

原文網址：[https://kknews.cc/tech/2ymnqnz.html](https://kknews.cc/tech/2ymnqnz.html){:target="_blank"}

### 符合意圖

Bad:

```
int d; // elapsed time in days
```

Good:

```
int elapsedTimeInDays;
```

<br>

Bad:

```
public List<int[]> getThem() {
    List<int[]> list1 = new ArrayList<>();
    for (int[] x : theList) {
        if (x[0] == 4) list1.add(x);
    }
    return list1;
}
```

Good:

```
public List<Cell> getFlaggedCells() {
    List<Cell> flaggedCells = new ArrayList<>();
    for (Cell cell : gameBoard) {
        if (cell.isFlagged) flaggedCells.add(cell);
    }
    return flaggedCells;
}
```

<br>

Bad:

```
public static void copyChars(char a1[], char a2[]) {
    // do sth
}
```

Good:

```
public static void copyChars(char source, char destination) {
    // do sth
}
```

### 避免編碼

Bad:
- 避免在命名裡加入數據形態（例如：`phoneString`）
- `interface`不用特別強調（例如：`IShapeFactory`），這是多餘、讓人分心的。選擇實作編碼命名比較好（例如：`ShapeFactoryImpl`）

### 能發音名稱

Bad:

```
Date genymdhms; // generation year, month, day, hour, miniute, seconds
```

Good:

```
Date generationTimestamp;
```

### 有意義區別

Bad:
- `getActiveAccount()`
- `getActiveAccounts()`
- `getActiveAccountInfo()`

（無法區別要調用哪個）

## 統一字詞

【命名一致】 -> 選擇一個命名

- fetch（取得）
- get（取得）
- retrieve（取得）

<br>

【含義一致】 -> 避免雙關語造成含義混淆、誤導判斷

- add（加入）
- insert（插入）
- append（附加）

## 有意義上下文

Bad:

```
class Bad {
    private void printGuessStatistics(char candidate, int count) {
        String number;
        String verb;
        String pluralModifier;

        if (count == 0) {
            number = "no";
            verb = "are";
            pluralModifier = "s";
        } else if (count == 1) {
            number = "1";
            verb = "is";
            pluralModifier = "";
        } else {
            number = Integer.toString(count);
            verb = "are";
            pluralModifier = "s";
        }
        String guessMessage = String.format(
                "There %s %s %s%s", verb, number, candidate, pluralModifier
        );
        print(guessMessage);
    }
    
    private void print(String guessMessage) {
        System.out.println(guessMessage);
    }
}
```

Good:

```
class GuessStatisticsMessage {
    private String number;
    private String verb;
    private String pluralModifier;

    private String make(char candidate, int count) {
        createPluralDependentMessageParts(count);
        return String.format("There %s %s %s%s", verb, number, candidate, pluralModifier);
    }

    private void createPluralDependentMessageParts(int count) {
        if (count == 0) {
            thereAreNoLetters();
        } else if (count == 1) {
            thereIsOneLetter();
        } else {
            thereAreManyLetters(count);
        }
    }

    private void thereAreNoLetters() {
        // initial variable
    }

    private void thereIsOneLetter() {
        // initial variable
    }

    private void thereAreManyLetters(int count) {
        // initial variable
    }
}
```

---
