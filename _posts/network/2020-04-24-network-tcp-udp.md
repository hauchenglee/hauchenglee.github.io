---
layout: post
title: Network - TCP / UDP
category: network
tags: [network]
---

## TCP

TCP / IP:

1. Transmission Control Protocol (TCP): is responsible for data delivery once that IP address has been found.
2. Internet Protocol (IP): is the part that obtains the address to which data is sent.

->

TCP decides the way to send; IP decide location to send.

TCP 幾個特點：
1. 面向連接協議（connection-oriented）
2. 具有可靠性

### TCP segment structure

![](https://hauchenglee.github.io/assets/images/network/tcp-SegmentHeader-1.png)

1. Source Port Address: 傳送來源
2. Destination Port Address: 傳送對象
3. Sequence Number: 序列號一串數字，用來表明接收後封包的重組順序
4. Acknowledgment Number: 確認號碼，告知接收者期待下次要接收的信息，也就是告知接收的狀態如何
5. Header Length (HLEN): 標頭長度
6. Control flags: 包含六個1 bit長度的信息，用於控制connection建立、終止、中斷、流的控制、傳輸模式等
   1. URG: Urgent pointer is valid
   2. ACK: Acknowledgment number is valid (used in case of cumulative acknowledgment)
   3. PSH: Request for push
   4. RST: Reset the connection
   5. SYN: Synchronize sequence numbers
   6. FIN: Terminate the connection
7. Window size: 傳送的TCP視窗（窗口）大小
8. Checksum: 用於錯誤控制的檢驗值，此值在TCP是必須的，但在UDP非必要存在
9. Urgent pointer: 僅當URG控制標誌存在而使用，用於指示需求最早傳送的數據

### Protocol operation

【TCP State】

<table>
    <thead>
        <tr>
            <th style="width:22%">State</th>
            <th style="width:18%">Subject</th>
            <th>Desc</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>LISTEN</td>
            <td>server</td>
            <td>等待（監聽）連線的TCP與端口</td>
        </tr>
        <tr>
            <td>SYN-SENT</td>
            <td>client</td>
            <td>發送連線請求（request）後並等待連線響應</td>
        </tr>
        <tr>
            <td>SYN-RECEIVED</td>
            <td>server</td>
            <td>連線確認</td>
        </tr>
        <tr>
            <td>ESTABLISHED</td>
            <td>both server and client</td>
            <td>連線成功並開啟狀態，可以互相傳送數據</td>
        </tr>
        <tr>
            <td>FIN-WAIT-1</td>
            <td>both server and client</td>
            <td>等待終止連線的請求（from remote TCP & previous request）</td>
        </tr>
        <tr>
            <td>FIN-WAIT-2</td>
            <td>both server and client</td>
            <td>等待終止連線的請求（from remote TCP）</td>
        </tr>
        <tr>
            <td>CLOSE-WAIT</td>
            <td>both server and client</td>
            <td>等待終止連線的確認（from local user）</td>
        </tr>
        <tr>
            <td>CLOSING</td>
            <td>both server and client</td>
            <td>等待終止連線的確認（from remote TCP）</td>
        </tr>
        <tr>
            <td>LAST-ACK</td>
            <td>both server and client</td>
            <td>最後終止連線的確認</td>
        </tr>
        <tr>
            <td>TIME-WAIT</td>
            <td>either server or client</td>
            <td>等待足夠的時候以獲得連線終止請求與確認的響應</td>
        </tr>
        <tr>
            <td>CLOSED</td>
            <td>both server and client</td>
            <td>無任何連線</td>
        </tr>
    </tbody>
</table>

<br>

【Connection establishment】

三次握手建立連接

![](https://hauchenglee.github.io/assets/images/network/tcp-connection-establishment.jpg)

過程：

> 首先 B 处于 LISTEN（监听）状态，等待客户的连接请求。
>
> 1. A 向 B 发送连接请求报文，SYN=1，ACK=0，选择一个初始的序号 x。
> 2. B 收到连接请求报文，如果同意建立连接，则向 A 发送连接确认报文，SYN=1，ACK=1，确认号为 x+1，同时也选择一个初始的序号 y。
> 3. A 收到 B 的连接确认报文后，还要向 B 发出确认，确认号为 y+1，序号为 x+1。
>
> B 收到 A 的确认后，连接建立。

原因：

1. 為了保證正確的連線傳送到server，避免錯誤的連線請求的後果
2. 每次握手都會確認對方是否正確響應，也會表明自己狀態正常
3. 只有三次最低限度才能保證連線是正確無誤的

<br>

【Connection termination】

四次揮手關閉連接

![](https://hauchenglee.github.io/assets/images/network/tcp-connection-termination.jpg)

過程：

> 1. 客户端发送一个 FIN 段，并包含一个希望接收者看到的自己当前的序列号 K. 同时还包含一个 ACK 表示确认对方最近一次发过来的数据。
> 2. 服务端将 K 值加 1 作为 ACK 序号值，表明收到了上一个包。这时上层的应用程序会被告知另一端发起了关闭操作，通常这将引起应用程序发起自己的关闭操作。
> 3. 服务端发起自己的 FIN 段，ACK=K+1, Seq=L。
> 4. 客户端确认。进入 TIME-WAIT 状态，等待 2 MSL（最大报文存活时间）后释放连接。ACK=L+1。

原因：

1. 在連線狀態時只要成功後，都可以同時向對方互相傳送數據，但是當一方要關閉連接時，需要雙方的互相確認
2. 一方先發起信息告知要關閉了，但是另一方仍可以繼續傳送數據（可能數據尚未傳送完畢）
3. 等待數據傳送都結束時，傳送端會發送FIN標誌來關閉連接，接收端發送ACK確認關閉連接

<br>

【TIME_WAIT】

當收到server FIN標誌後會進入此狀態，而不是直接終止（CLOSED），有以下理由：

1. 確保最後一個確認報文能夠正確送達，若在一定時間內都沒有收到確認報文，那麼將會重新發送請求報文
2. 為了將本連接內的所有數據、報文全部處理完畢，並在網絡上清零，使下次全新的連接不會有上次連線的干擾

### TCP Sequence and Acknowledgement



### TCP Flow Control

to avoid buffer overflow

### TCP Window Control



### TCP Retransmission



### Vulnerabilities

1. Denial of service: 通過發送錯誤或是欺騙性的標誌來消耗、攻擊系統
2. Connection hijacking: 竊聽、劫持TCP連線，並獲得永久連接的控制
3. TCP veto: 竊聽TCP連線，但不會破壞中止現有連接

## UDP

User Datagram Protocol (UDP):

1. 沒有握手的流程，與不保存UDP發送消息的狀態特性，因此具有不可靠性
2. 它是無狀態、無重傳延遲，因此適合串流應用
3. 適用於不需要在程序執行中進行錯誤檢測與糾錯的目的，達到減少開銷的效能

## Compare

1. TCP是面向連接的協議，而UDP是無連接協議。
2. TCP的速度較慢，而UDP的速度較快
3. TCP使用SYN，SYN-ACK，ACK等握手協議，而UDP不使用握手協議
4. TCP執行錯誤檢查並進行錯誤恢復，另一方面，UDP執行錯誤檢查，但會丟棄錯誤的數據包。
5. TCP具有確認段，但是UDP沒有任何確認段。
6. TCP是重量級的，而UDP是輕量級的。

<table>
    <thead>
        <tr>
            <th></th>
            <th>TCP</th>
            <th>UDP</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td></td>
            <td>Reliable</td>
            <td>Unreliable</td>
        </tr>
        <tr>
            <td></td>
            <td>Sequenced</td>
            <td>Unsequenced</td>
        </tr>
        <tr>
            <td></td>
            <td>Connection-oriented</td>
            <td>Connectionless</td>
        </tr>
        <tr>
            <td></td>
            <td>Virtual circuit</td>
            <td>Low Overhead</td>
        </tr>
        <tr>
            <td></td>
            <td>Acknowledgement</td>
            <td>No Acknowledgement</td>
        </tr>
        <tr>
            <td></td>
            <td>Window flow control</td>
            <td>No any type control</td>
        </tr>
    </tbody>
</table>

## Reference

Wiki:

- [Transmission Control Protocol - Wikipedia](https://en.wikipedia.org/wiki/Transmission_Control_Protocol){:target="_blank"}
- [User Datagram Protocol - Wikipedia](https://en.wikipedia.org/wiki/User_Datagram_Protocol){:target="_blank"}

TCP:
- [What is TCP/IP? \| How the Model & Protocols Work \| Avast](https://www.avast.com/c-what-is-tcp-ip){:target="_blank"}

TCP segment structure:

- [Services and Segment structure in TCP - GeeksforGeeks](https://www.geeksforgeeks.org/services-and-segment-structure-in-tcp/){:target="_blank"}
- [TCP segment structure](http://www.cs.umd.edu/~shankar/417-F01/Slides/chapter3b/sld002.htm){:target="_blank"}

Protocol operation:

- [Manual:Connection oriented communication - MikroTik Wiki](https://wiki.mikrotik.com/wiki/Manual:Connection_oriented_communication_(TCP/IP)){:target="_blank"}

Compare:

- [TCP vs UDP: What's the Difference?](https://www.guru99.com/tcp-vs-udp-understanding-the-difference.html){:target="_blank"}
- [TCP vs UDP - Difference and Comparison \| Diffen](https://www.diffen.com/difference/TCP_vs_UDP){:target="_blank"}
- [TCP vs. UDP: Understanding the Difference](https://www.privateinternetaccess.com/blog/tcp-vs-udp-understanding-the-difference/){:target="_blank"}

Interview:

- [一文搞定 UDP 和 TCP 高频面试题！ - 知乎](https://zhuanlan.zhihu.com/p/108822858){:target="_blank"}

---