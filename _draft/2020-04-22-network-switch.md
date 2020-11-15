---
layout: post
title: Network - Switch
category: tech
tags: [tech]
---

## Device

### Repeater

### Hub

### Bridge

bridge & switch 其實是一樣的東西，所以算域的時候算法一樣

### Switch

switch：全雙工
半雙工：會有collision，所以有 CSMA/CD 避免掉包（舊有技術，因為現在設備都是全雙工）

## Collision Domain and Broadcast Domain

無論多少個Hub，只要Hub直接相連，就是一個衝突域，最終都會產生碰撞掉包，但是若沒有直接相連（例如中間有路由），就是不同的衝突域 

switch 的每一個端口都是衝突域，但是一台交換機默認只有一個廣播域（直連狀態一樣一個廣播域），分割衝突域

路由器有幾個端口，就有幾個廣播域，路由是用來分割廣播域的

## Switching Basic

交換機如何工作的

內部有一張表 mac table，記錄鏈接在這台交換機的mac地址

交換機根據硬件ASICs進行數據轉發

Hardware-based bridging (ASICs)
- Wire speed
- Low latency
- Low cost

二層重點：
- How to learn MAC address 如何學習MAC地址，如何生成MAC表格
- How to forward frame 如何進行數據轉發
- **How to prevent looping** 怎樣防止環路

## Switching Address Learning

交換機收到一個未知單播幀（不知道目的地MAC地址）：廣播

## Switching Forwarding


## VLAN

使用技術虛擬出多個局域網

好處：
- 邏輯分割
- 增加安全性
- 局域網模塊化（也就是雖然局域網數量變多，但因為總數一致，相對應每個局域網內部的host減少，能更好定位問題）

## Access and Trunk Port

Access Port
- Belongs to and carries the traffic of only one VLAN
- Traffic is both received and sent in native formats with no VLAN information (tagging) whatsoever

Trunk Port
- Carries the traffic of multiple VLANs

## DTP Dynamic Trunk Protocol

---