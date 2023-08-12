---
layout: post
title: Docker Image
category: it
tags: [devops]
---

提交容器成為一個新的副本

```shell
docker commit -m="提交的描述信息" -a="作者" 容器id [space] 目標鏡像名
```

實戰測試：
1. 啟動一個默認的tomcat
2. 發現這個默認的tomcat是沒有webapps應用，鏡像的原因，官方的鏡像默認從webapps下面是沒有文件的！
3. 我自己拷貝進去了基本的文件
4. 將我們操作過的容器通過`commit`提交為一個鏡像！我們以後就使用我們修改過的鏡像即可，這就是我們自己的一個修改的鏡像。

---