---
layout: post
title: Docker Command Line
category: it
tags: [devops]
---


## Docker 的常用命令

### 幫助命令

```shell
docker version
docker info
docker xxx --help
```

```shell
systemctl start docker
```

### 官方命令集

[Reference documentation \| Docker Documentation](https://docs.docker.com/reference/)

## 鏡像命令

### 查看所有本地的主機上的鏡像

```shell
docker images
-a # 列出所有鏡像
-q # 只顯示鏡像的id
```

### 搜索鏡像

```shell
docker search mysql
-filteSTARS=3000 # 搜索出來的鏡像就是STARS大於3000的
```

### 下載鏡像

```shell
docker pull mysql
using default tag: latest # 如果不寫tag，默認是latest
xxx: Pull complete # 分層下載，docker image的核心 聯合文件系統
...
docker.io/library/mysql: latest # 真實地址

// same 等價
// docker pull mysql = docker pull docker.io/library/mysql: latest

[optional]:
-a
-q
```

### 刪除鏡像

```shell
docker rmi -f 鏡像id # 刪除指定的鏡像
docker rmi -f 鏡像id 鏡像id 鏡像id # 刪除多個鏡像
docker rmi -f $(docker images -aq) # 刪除全部的鏡像
```

## 容器指令

說明：我們有了鏡像才可以創建容器，linux，下載一個 centos 鏡像來測試

```shell
docker pull centos
```

### 新建容器並啟動

```shell
docker run [可選參數] image

# 參數說明
--name="Name" #容器名字 tomcat01 tomcat02 用來區分容器
-d                           #後台方式運行
-it                           #使用交付方式運行，進入容器查看內容
-p                           #指定容器的端口 -p 8080:8080
    -p ip:主機端口:容器端口
    -p 主機端口:容器端口 #（常用）
    -p 容器端口
    容器端口
-P                           #隨機指定端口
--name                  #給容器命名
```

### 啟動並進入容器

```shell
docker run -it centos /bin/bash
[root@e4eccc01b495] ls # 查看容器內的centos
```

### 列出所有的運行的容器

```shell
docker ps  # 列出當前正在運行

[optional]
-a # 列出當前正在運行+帶出歷史運行過的容器
-n=? # 顯示最近創建的容器
-q # 只顯示容器的編號
```

### 退出容器

```shell
exit # 直接容器停止並退出
ctrl + P + Q # 容器不停止退出
```

### 刪除容器

```shell
docker rmi 容器id # 刪除指定的容器，不能刪除正在運行的容器，如果要強制刪除 rm -f
docker rmi -f $(docker ps -aq) # 刪除全部的鏡像
docker ps -a -q|xargs docker rm # 刪除所有的容器
```

### 啟動和停止容器的操作

```shell
docker start 容器id # 啟動容器
docker restart 容器id # 重啟容器
docker stop 容器id # 停止當前正在運行的容器
docker kill 容器id # 強制停止當前容器
```

## 常用其他命令

### 後台啟動容器

```shell
docker run -d 鏡像名
docker run -d centos
```

問題：`docker ps`發現 centos 停止了

常見的坑，docker容器使用後台運行，就必須要有一個前台流程，docker發現沒有應用，就會自動停止。
容器啟動後，發現自己沒有提供服務，就會立刻停止，就是沒有程序了。

### 查看日誌

自己編寫一段shell腳本

```shell
docker run -d centos /bin/sh -c "while true;do echo kuangshen;sleep 1;done"
```

顯示日誌

```shell
docker logs -tf --tail [number] 容器id

[optional]
-tf # 顯示日誌
--tail number # 要顯示日誌條數
```

### 查看容器中進行信息

```shell
docker top 容器id

# ppid是父進程id，pid是進程id
```

### 查看鏡像的元數據

```shell
docker inspect 容器id
```

### 進入當前正在運行的容器

我們通常容器都是使用後台方式運行的，需要進入容器，修改一些配置

方法1：進入容器後開啟一個新的終端，可以在裡面操作（常用）

```shell
docker exec -it 容器id bashShell
```

方法2：進入容器正在執行的終端，不會啟動新的進程

```shell
docker attach 容器id
```

### 從容器內拷貝文件到主機上

```shell
docker cp 容器id:容器內路徑 [space] 目的地主機路徑
```

拷貝是一個手動過程，未來使用 `-v` 卷的技術，可以實現

## 小結

![](https://hauchenglee.github.io/assets/images/it/devops/docker_command_line.png)

---