---
layout: post
title: Clean Code 無暇的程式碼- Ch8 邊界
category: arch
tags: [arch]
---

重點：如何花最少時間學習與使用第三方API

有以下方式可供快速使用：
- 封裝泛型細節，使用者只要知道我需要的結果就好，不需要知道到底傳入什麼泛型
- apache log4j
- 單元測試（unit test）

第一點說明：

```
public class Sensors {
    private Map sensors = new HashMap();

    public Sensor getById(String id) {
        return (Sensor) sensor.get(id);
    }
}
```

---