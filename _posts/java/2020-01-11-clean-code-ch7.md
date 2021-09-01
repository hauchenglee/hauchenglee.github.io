---
layout: post
title: Clean Code 無暇的程式碼- Ch7 異常處理
category: architecture
tags: [design-pattern]
---

## Use `try-catch`

重點總結：
- 使用異常處理，而非錯誤碼
- 在最外層定義好`try-catch`，讓調用者決定如何處理異常
- 使用`unchecked`異常，原因是維持封閉性原則
- 提供異常的詳細資訊
- 打包被調用者，封裝異常

第四點詳細說明：

這是沒修飾過的一般代碼：

```
LDAP ldap = new LDAP("username", "password");
try {
    ldap.execute();
} catch(TimeOutException e) {
    log.error("TimeOutException: " + e);
    report(e);
} catch(ErrorURL e) {
    log.error("ErrorURL: " + e);
    report(e);
} catch(ErrorUser e) {
    log.error("ErrorUser: " + e);
    report(e);
} catch(Exception e) {
    log.error("Exception: " + e);
    report(e);
}
```

我們可以打包異常處理：

```
public class LDAPUtil {
    private LDAP ldap;

    // constructor
    public LDAPUtil(String username, String pwd) { }

    public void execute() {
        try {
            ldap.execute();
        } catch(TimeOutException e) {
            throw TimeOutException(e);
        } catch( // other exception write here) { }
    }
}
```

封裝好了API的異常處理，以後調用只要寫共同的異常就好

```
LDAPUtil ldap = new LDAPUtil("username", "password");
try {
    ldap.execute();
} catch(Exception) {
    log.error(e);
    report(e);
}
```

個人評：

看起來只是把原本的method再封裝一層而已，並沒有什麼特殊之處，使用AOP（Aspect-oriented programming）可以更好解決這個問題。

## About `null`

重點總結：
- 不要傳遞`null`
- 解決NPE四個方法：
   - Empty List
   - Java 8 `Optional`
   - 封裝空指針對象（也就是`new`一個特殊對象，用這對象帶代表`null`對象）
   - 拋出異常（當沒有返回值時使用異常的決定高度取決於應用程序的性質）

### do not pass and return `null`

編程的人應該都聽過一句話：【Null Pointer References: The Billion Dollar Mistake】，這是從空指針的發明人Tony Hoare之口說出的。

為了這個`null`，我們在編程時多少次遇到了空指針異常，導致系統崩潰；<br>
為了這個`null`，Java 8特地引入了`Optional`讓我們在調用前強制檢查空指針，避免錯誤<br>
為了這個`null`，Kotlin甚至直接將檢查NPE當作預設屬性了，強制執行檢查機制

為了避免發生`NullPointerException`，我們必須在每次調用時都確認一次，這是很煩人的。

舉一個例子。例如，如果我要用Java定義一個返回`Collection`的方法，那麼我通常希望返回一個空`collection`（即`Collections.emptyList()`）而不是`null`，因為這意味著我的代碼可以更加簡潔：

```
Collection<? extends Item> c = getItems(); // Will never return null.

for (Item item : c) { // Will not enter the loop if c is empty.
  // Process item.
}
```

...比以下更清潔：

```
Collection<? extends Item> c = getItems(); // Could potentially return null.

// Two possible code paths now so harder to test.
if (c != null) {
  for (Item item : c) {
    // Process item.
  }
}
```

### Summary

在Robert Martin的【Clean Code】中寫道：
【當您可以返回空數組時，返回`null`是錯誤的設計。由於預期結果是數組，為什麼不呢？它使您無需任何額外條件即可遍歷結果。如果是整數，則可能為0，如果是散列，則為空散列。等等】

在【Effective Java】item 43 p. 201, Joshua Bloch說道：【沒有任何理由應該從`array`或是`collection`返回一個`null`值】。
（There is no reason ever to return `null` from an array or collection valued method.）

他認為應該返回一個長度為零的數組（`array.length() == 0`），或是一個空集合（`collection.empty()`）以表示沒有數據。因為，讓調用者每次調用都要檢查所有內容是痛苦的。

每個API設計人員都應牢記最小驚喜的原則，並努力做到簡單。

Ref:
- [Stop Returning Null in Java - Code by Amir](https://www.codebyamir.com/blog/stop-returning-null-in-java){:target="_blank"}
- [Is it better to return NULL or empty values from functions/methods where the return value is not present? - Software Engineering Stack Exchange](https://bit.ly/2QOquoP){:target="_blank"}
- [oop - Is returning null bad design? - Stack Overflow](https://bit.ly/2ua8y03){:target="_blank"}
- [java - DAO with Null Object Pattern - Stack Overflow](https://bit.ly/35eBMI7){:target="_blank"}

---
