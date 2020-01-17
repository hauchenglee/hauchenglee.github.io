---
layout: post
title: Web App Server Deploy 部署
category: java
tags: [server]
---

## install

### Windows10

安裝maven在Windows10，需要兩樣：Java JDK、Maven。

1. 下載並設定好Java JDK（7+）
2. 在[此處](https://maven.apache.org/download.cgi){:target="_blank"}，選擇**Binary zip archive**
3. 解壓縮到想放的位置
4. 開始環境變數的設定（System variables）

需要的環境變數有：

Variable name|Variable value
---|---
JAVA_HOME|安裝Java的路徑（C:\Program Files\Java\jdk1.8.0_191）
M2_HOME|解壓縮的路徑（C:\Users\Documents\opt\apache-maven-3.6.3）
MAVEN_HOME|解壓縮的路徑，與M2_HOME相同（C:\Users\Documents\opt\apache-maven-3.6.3）
Path|新增value：%M2_HOME%\bin

最後，開啟terminal，輸入以下指令：

```console
mvn –v
```

console:

```console
C:\Users>mvn -v
Apache Maven 3.6.3 (cecedd343002696d0abb50b32b541b8a6ba2883f)
Maven home: C:\Users\Documents\opt\apache-maven-3.6.3\bin\..
Java version: 1.8.0_191, vendor: Oracle Corporation, runtime: C:\Program Files\Java\jdk1.8.0_191\jre
Default locale: zh_TW, platform encoding: MS950
OS name: "windows 10", version: "10.0", arch: "amd64", family: "windows"
```

Ref:
- [How to install Maven on Windows 10 - Development notes](https://dev-pages.info/how-to-install-maven-on-windows-10/){:target="_blank"}
- [Windows 10 之 JDK、MAVEN 和 Gradle 下載安裝與環境建置 - 第一次學app就上手 - 點部落](https://dotblogs.com.tw/starhao/2016/10/18/004646){:target="_blank"}

### Linux



## directory

maven項目的目錄結構：

![](http://www.hauchenglee.com/assets/images/java/maven-dirctory-structure.png)

source: [Maven Directory Structure - Dinesh on Java](https://www.dineshonjava.com/maven-directory-structure/){:target="_blank"}

注意上面帶粗體的目錄名，maven項目採用"約定優於配置"的原則，
- `src/main/java` 約定用於存放源代碼
- `src/main/test` 約定用於存放單元測試代碼
- `src/target` 約定用於存放編譯、打包後的輸出文件

Ref:
- [maven学习（上）- 基本入门用法 - 菩提树下的杨过 - 博客园](https://www.cnblogs.com/yjmyzz/p/3495762.html){:target="_blank"}

## pom

- groupId：是項目建立團隊或組織的唯一標識符，通常是域名倒寫，與artifactId被統稱為**坐標**，保證了項目的唯一性
- artifactId：定義了依賴jar包在**存儲庫**的地址，是這些jar包的**坐標**唯一標識符，也是一個項目將要產生的文件
- packaging：artifactId打包的方式，表示項目最終產生何種extension的打包文件，如`jar`、`maven-plugin`、`ejb`、`war`、`ear`、`rar`。如果沒有聲明packaging，則預設包裝為`jar`

Ref:
- [Maven – POM Reference](http://maven.apache.org/pom.html#Maven_Coordinates){:target="_blank"}
- [介绍maven的作用、核心概念(Pom、Repositories、Artifact、Goal)、用法、常用参数和命令以及简单故障排除、扩展及配置](https://www.trinea.cn/android/maven/){:target="_blank"}

## 依賴



## 繼承



## 聚合



## Reference

- [maven使用指南 - maven使用指南](https://www.ibofine.com/mavenbook/index.html){:target="_blank"}
- [Maven Tutorials - HowToDoInJava](https://howtodoinjava.com/maven){:target="_blank"}
- [你分得清楚Maven的聚合和继承吗？ - 知乎](https://zhuanlan.zhihu.com/p/57384561){:target="_blank"}

---