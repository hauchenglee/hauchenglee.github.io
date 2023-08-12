---
layout: post
title: Docker Dockerfile
category: it
tags: [devops]
---

## 介紹

Dockerfile是用來構建Docker鏡像的文件！命令參數腳本！

構建步驟
1. 編寫一個Dockerfile文件
2. `docker build` 構建成為一個鏡像
3. `docker run` 運行鏡像
4. `docker push` 發布鏡像（DockerHub）

很多官方鏡像都是基礎包，很多功能都沒有，我們通常會自己搭建自己的鏡像！

## 構建過程

基礎知識：
1. 每個保留關鍵字（指令）都是必須是大寫字母
2. 執行從上到下順序執行
3. `#` 表示註解
4. 每一個指令都會創建提交一個新的鏡像層，並提交！

![](https://hauchenglee.github.io/assets/images/it/devops/dockerfile_commit.png)

dockerfile是面向開發的，我們以後要發布項目、做鏡像，就需要編寫dockerfile文件。

- DockerFile：構建文件，定義類一切的步驟，源代碼
- DockerImages：通過DockerFile構建生成的鏡像，最終發布和運行的產品
- Docker容器：容器就是鏡像運行起來提供服務

## 指令說明

![](https://hauchenglee.github.io/assets/images/it/devops/dockerfile_cmd1.png)

![](https://hauchenglee.github.io/assets/images/it/devops/dockerfile_cmd2.png)

---