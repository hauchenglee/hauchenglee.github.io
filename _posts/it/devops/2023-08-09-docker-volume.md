---
layout: post
title: Docker Volumn
category: it
tags: [devops]
---

## 什麼是容器數據卷

**Docker的理念回饋**

將應用和環境打包成一個鏡像！

數據？如果數據都在容器中，那麼我們容器刪除，數據就會丟失！ *需求：數據可以持久化

MySQL，容器刪了，刪庫跑路！ *需求：MySQL數據可以存儲在本地！ *

容器之間可以有一個數據共享的技術！ Docker容器中產生的數據，同步到本地！

這就是卷技術！目錄的掛載，將我們容器內的目錄，掛載到Linux上面！

**總結一句話：容器的持久化和同步操作！容器間也是可以數據共享的！ **

## 使用數據卷

查看所有的volumn情況

```shell
docker volume ls
```

### 方法一：直接使用命令來掛載 -v

```shell
docker run -it -v 主機目錄:容器內目錄
```

測試

```shell
docker run -it -v /home/ceshi:/home centos /bin/bash
```

啟動起來時候我們可以通過`docker inspect 容器id`查看

好處：我們以後修改只需要在本地修改即可，容器會自動同步（停止容器，容器內的數據依舊是同步的！）

### 具名掛載

```shell
docker run -it -v 卷名:容器內目錄
```

所有的docker容器內的捲，沒有指定目錄的情況下都是在`/var/lib/docker/volumes/xxx/...data`

我們通過具名掛載可以方便的找到我們的一個卷，大多數情況在使用**具名掛載**

### 比較

如何確定是具名掛載還是匿名掛載，還是指定路徑掛載！

```shell
-v 容器內路徑 # 匿名掛載
-v 卷名:容器內路徑 # 具名掛載
-v /宿主機路徑:容器內路徑 # 指定路徑掛載
```

### 方法二：初識Dockerfile

Dockerfile就是用來構建docker鏡像的構建文件！命令腳本！

通過這個腳本可以生成鏡像，鏡像是一層一層的，腳本一個個的命令，每個命令都是一層！

創建一個dockerfile文件，名字可以隨機 建議Dockerfile

文件中的內容、指令（大寫）、參數：

```shell
FROM centos

VOLUMN ["volume01","volumn2"]

CMD echo "----end----"

CMD /bin/bash
```

這裡的每個命令，就是鏡像的一層！

這種方式我們未來使用的十分多，因為我們通常會構建自己的鏡像！

假設構建鏡像時候沒有掛載卷，要手動鏡像掛載 `-v 卷名:容器內路徑`

## 數據卷容器

（略）

---