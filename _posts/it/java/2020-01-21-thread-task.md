---
layout: post
title: Java - Thread Task Execution 任務執行
category: java
tags: [java]
---

## 任務（Tasks）

大多數並發應用程序是圍繞執行任務（task）進行管理的。任務時邏輯上的工作單元，線程是使任務異步執行的機制。

任務（task）的定義：
- 是抽象、離散的工作單元（unit of work）
- 是獨立的工作，它的工作並不依賴其他任務的狀態、結果或邊界效應（side effect）

任務的邊界（task boundaries）：
- Web服務器
- 郵件服務器
- 文件服務器
- EJB容器
- 數據庫服務器

例如：向郵件服務器提交一個消息後產生的結果，並不會被其他正在同時處理的消息所影響。

### 單線程執行任務

```
import java.io.IOException;
import java.net.ServerSocket;
import java.net.Socket;

class SingleThreadWebServer {
    public static void main(String[] args) throws IOException {
        ServerSocket socket = new ServerSocket(80);
        while (true) {
            Socket connection = socket.accept();
            handleRequest(connection);
        }
    }

    private static void handleRequest(Socket connection) {
        // do sth in detail
    }
}
```

`SingleThreadedWebServer`很簡單，它在理論上是正確的，但是它一次只能處理一個請求：
- 主線程不斷在【接受連接】與【處理相關請求】之間交替運行
- 並且直到主線程完成了當前的請求（處理完任務）並再次調用`accept`，此前新的請求都必須等待

造成結果：在生產環境中的執行效率很糟糕，資源利用率非常低。

### 多線程執行任務

為了提供更好的響應性，可以為每個服務請求創建一個新的線程。

```
import java.io.IOException;
import java.net.ServerSocket;
import java.net.Socket;

class ThreadPerTaskWebServer {
    public static void main(String[] args) throws IOException {
        ServerSocket socket = new ServerSocket(80);
        while (true) {
            final Socket connection = socket.accept();
            Runnable task = () -> {
                handleRequest(connection);
            };
            new Thread(task).start();
        }
    }

    private static void handleRequest(Socket connection) {
        // do sth in detail
    }
}
```

與單線程執行的差別：
- 相同：主線程仍然不斷地交替運行【接受外部連接】與【轉發請求】
- 不同：主循環為每個連接都創建一個新線程以處理請求（任務），而不是在主循環的內部處理它

結論：
- 執行任務的負載已經脫離了主線程，這讓主循環能夠更迅速地重新開始等待下一個連接並且接受新的請求，從而提高了響應。
- 並行處理任務，這使得多個請求可以同時得到服務。如果有多個處理器，或者出於I/O未完成、鎖請求以及資源可用性等任何因素需要阻塞任務時，
  程序的吞吐量會得到提高。
- 任務處理代碼必須是線程安全（*thread safe*）的，因為有多個任務會並發地調用它。

### 無限制創建線程的缺點

主要有三：

- 【線程生命週期的開銷】：線程的創建與關閉都需要時間與開銷，這會帶來處理請求的延遲，並且每個請求創建一個新線程的做法會消耗大量的計算資源。
- 【資源消耗量】：線程在執行時會消耗系統資源，尤其是內存，無論是線程閒置，還是線程競爭CPU資源，都會造成資源浪費產生額外的性能開銷。
- 【穩定性】：應該限制可創建線程的數目，如果打破了這些限制，最可能的結果是`OutOfMemoryError`。

每個任務每個線程（thread-per-task）方法的問題在於它沒有對已創建線程的數量進行任何限制，除非對客戶端能夠拋出的請求速率進行限制。

像其他的並發危險（想一想，有哪些？）一樣，無限制創建線程的行為可能在原型和開發階段還能表現良好，而當部署後並運行於高負載下，它的問題才會曝露出來。

---