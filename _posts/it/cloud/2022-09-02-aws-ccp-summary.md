---
layout: post
title: AWS - CCP Summary
category: operations
tags: [aws]
---

## 雲計算

範例：
- IaaS: EC2
- Paas: Elastic Beanstalk
- Saas: Apps

雲計算部署模式：
- 雲端部署（Cloud）
- 本地部署（On-premises）（私有雲部署）
- 混合部署（**Hybrid**）

雲計算的六大優勢：支出、速度、規模、靈活

### 設計原則

AWS 架構完善的框架
- 卓越運營（Operational Excellence）：運行和監控系統
- 安全性（Security）：保護信息和系統（eg. AWS CloudTrail）
- 可靠性（Reliability）：恢復中斷系統
- 性能效率（Performance）：有效使用計算資源
- 成本優化：避免或消除不需要的成本或次優資源
- 可持續性：解決對環境、經濟和社會的長期影響

## 基礎設施

### Region, AZ, Edge Location

三個基礎設施
- 區域（Region）：數據中心集群，包含 AWS 資源的地理區域
- 可用區（Availability Zones）：數據中心、容錯、相同安全
- 邊緣站點（Edge Location）：副本、非災難恢復、主要城市

區域四項業務因素：
- 合規性（Compliance）（**數據主權**）
- 距離（Proximity）
- 可用服務（Available services）
- 定價（Pricing）

AWS CloudFront: 
- 內容分發服務
- 邊緣站點緩存內容

### 預置資源

與AWS服務交互的方式：
- AWS CLI: 通過腳本自動執行 AWS 服務和應用程序的操作
- AWS SDK: 采用受支持的編程語言開發 AWS 應用程序

AWS Outposts: 
- 配置管理服務
- 混合雲：將雲基礎設施和服務擴展到您的**本地數據中心**

AWS CloudFormation: 
- 將**基礎設施**視為代碼，通過編寫代碼來構建環境

AWS CodeDeploy：
- 代碼部署到任何實例
- 非管理基礎設施與構建環境

AWS Ekastic Beanstalk: 
- 提供代碼和配置設置

Amazon S3 Transfer Acceleration：
- 長距離文件傳輸：作用於客戶端和 S3 存儲桶之間
- 原理：利用全球分布式邊緣站點路由優化路徑

## 安全性

### 責任共擔模式

- 客戶：網絡、加密數據
- AWS：物理、創建管理程序

共享控製：適用於基礎設施層和客戶層（apply to both the infrastructure layer and customer layers）
- **補丁管理**：AWS 負責修補和修復基礎設施內的缺陷，而客戶負責修補其來賓操作系統和應用程序
- **配置管理**：AWS 負責維護基礎設施設備的配置，而客戶負責配置自己的來賓操作系統、數據庫和應用程序
- 認知和培訓：AWS 負責培訓 AWS 員工，而客戶必須負責培訓自己的員工

### IAM

用戶權限和訪問 Identity and Access Management
- IAM 用戶組和角色
- IAM 策略
- 多重驗證（MFA）

與 IAM 進行交互的方法：
- AWS 管理控製臺（AWS Management Console）
- AWS 命令​​行工具（AWS Command Line Tools, CLI）
- AWS 開發工具包（AWS Software Development Kits, SDK）

### Organization

AWS Organizations
- 在一個中心位置整合和管理多個 AWS 賬戶
- 使用服務控製策略 (Service control policies, SCPs) **集中控製組織中所有賬戶**的權限
- SCP 可以應用於以下身份和資源：
    - 單個成員賬戶
    - 組織單位（OU）

### 合規性

AWS Artifact：
- 按需訪問 AWS 安全性與合規性報告
- 審核、接受並管理與 AWS 簽訂的協議
- 允許客戶下載 **AWS SOC** 和 **PCI** 報告
- AWS 賬戶所有者可以從此處獲得其賬戶中所有用戶的列表，包括其 AWS 憑證的狀態

AWS Certificate Manager (ACM)：
- 提供服務器證書
- 快速購買和部署 SSL/TLS 證書
- 可以使用 ACM 或 IAM 來存儲和部署服務器證書

### 拒絕服務攻擊

AWS Shield：
- 標準：阻擋DDoS
- 高級：阻擋、診斷、檢測、緩解DDoS

### 其他安全服務

- 密鑰管理（AWS KMS）：創建與管理加密操作的密鑰（eg. MFA）
- 訪問秘鑰：用於簽署對**編程請求**（CLI, API）的長期憑證
- 網絡應用程序防火墻（AWS WAF）：監控進入 Web 應用程序的網絡請求的服務
- 自動化安全性評估（Amazon Inspector）：檢查應用程序的安全漏洞以及偏離最佳實踐的情況的服務
- 智能威脅檢測（Amazon GuardDuty）

## 聯網

### 網關和連接

互聯網網關：將 VPC 連接到互聯網

Amazon Virtual Private Cloud (Amazon VPC)：
- 虛擬網絡隔離區，客戶可以完全控製

AWS Direct Connect：
- 在數據中心和 VPC 之間建立專用私有連接

AWS Transit Gateway：
- 網絡中轉中心
- 簡化 VPC 之間的連接管理

### 子網和網絡訪問控製列表

子網：
- 是一個用於將資源歸為一組的獨立區域
- 是 VPC 的一部分
- 可以是公有子網，也可以是私有子網

網絡訪問控製列表 (ACL)：
- 用於在**子網**級別控製入站和出站流量
- 默認允許所有入站和出站流量，並檢查所有出入白名單與黑名單是否放行
- 無狀態數據包篩選

安全組：
- 用於控製 **Amazon EC2 實例**的入站和出站流量
- 默認拒絕所有入站流量，並允許所有出站流量
- 有狀態數據包篩選

### 全球聯網

Amazon Route 53：
- DNS Web 管理域名服務
- 可以對 Amazon EC2 實例、 Web 服務器和其他資源執行**健康檢查**
- 通過多種路由類型管理全球應用程序流量（跨區域）

## EC2

- 快速預置、啟動和隨時停止
- 按需使用與付費

### 實例類型

- 通用實例（General Purpose）：均衡計算
- 計算優化實例（Compute Optimized）：提供高性能處理器，適合批次處理
- 內存優化實例（Memory Optimized）：非常適合高性能數據庫
- 存儲優化實例（Storage Optimized）：適用於數據倉庫應用程序

### 定價

- 按需實例（On-Demand Instances）：不能中斷、短期、沒有前期成本
- Savings Plans：承諾穩定使用 1 年或 3 年期
- 預留實例（Reserved）：
    - 購買 1 年或 3 年期、標準預留實例、可轉換預留實例
    - 預付越多，折扣越多（All Upfront）
- Spot實例（Spot Instances）：靈活、可承受中斷
- 專用主機（Capacity Reservations）：指 EC2 實例容量完全供您專用的物理服務器。

### Auto Scaling

Amazon EC2 Auto Scaling
- 更好的容錯性：通過**跨區域內多個可用區**提供可靠性
- 更好的成本管理：只需要在使用實例時為實際使用的實例付費

限製：
- **不能跨越多個區域**
- 不分配流量（這是 ELB 的工作）
- 不執行任何緩存（這是 CloudFront 的工作）

### ELB

Elastic Load Balancing
- 應用程序負載均衡器：Application Load Balancer 最適合 HTTP 和 HTTPS 流量的負載平衡
- 網絡負載均衡器：Network Load Balancer 最適合 TCP 和 TLS 流量的負載均衡

### SNS & SQS

- SNS：發布/訂閱服務。
- SQS：消息隊列服務。

### Lambda

AWS Lambda
- 無服務
- 按需使用（付費）
- 代碼觸發
- 任何編程

### Container

- Elastic Container Service (ECS)：with Docker
- Elastic Kubernetes Service (EKS)：with Kubernetes
- AWS Fargate：無服務器計算引擎，允許客戶在無需管理服務器或集群的情況下運行容器

## 存儲和數據庫

summary:
- 實例：丟失
- EBS：塊級、單區、讀寫、持久、配置收費
- S3：對象、文件、數量任意大小限製、99耐用、批量折扣
    - 標準、智能、IA、歸檔
- EFS：區域、多區、擴容
- RDS：自動管理
    - Aurora：擴實
    - 其他：擴容
- DynamoDB：鍵值、無服務、擴容
- Storage Gateway：混合、無法自動擴展
- Neptune：圖形、無服務

其他：
- Redshift：數據倉庫、數據分析
- DMS：遷移

使用 Amazon RDS 屬於責任共擔模型：
- 客戶的責任：
    - 管理數據庫設置
    - 構建關系數據庫模式
- AWS的責任：
    - 安裝數據庫軟件
    - 執行備份
    - 修補數據庫軟件

## 監控和分析

summary：
- CloudWatch：圖形監控、警報
- CloudTrail：調用日誌、為治理、**合規性**和風險審計提供了很好的資源
- Trusted Advisor：成本、性能、安全、容錯、配額
- AWS X-Ray：調試服務、方便排除錯誤

## 定價和支持

### AWS 定價概念

工具 & 服務：
- AWS 定價計算器
- 賬單控製面板（AWS Billing Dashboard）：查看 AWS 賬單的詳細信息，但無法設置警報
- 整合賬單：在組織的賬戶中共享批量折扣定價、Savings Plans 和預留實例
- AWS 預算：在服務使用量超出您定義的閾值時接收提醒
    - 成本預算：成本超過（或預計超過）他們的預算金額時
    - **使用量預算**：使用量超過（或預計超過）定義的閾值時
    - **預訂預算**：設置預訂利用率或覆蓋目標，在其利用率低於他們定義的閾值時
- AWS Cost Explorer：
    - 根據**過去使用情況**（非預期使用情況）直觀查看、了解和管理隨時間變化的 AWS 成本和使用情況
    - 提供有關 EC2 **預留實例**利用率和覆蓋率報告
    - 不提供**本地歷史**支出報告
- AWS Cost and Usage Reports：數據比 AWS Cost Explorer 提供的數據更詳細

影響整體成本因素：
- 計算費用（compute charges）
- 數據傳出費用（data transfer out charges）

不影響整體成本因素：
- 使用的服務數量（the number of services used）：每項 AWS 服務都有自己的定價細節，其中許多是免費使用的
- 數據傳輸費用（data transfer in charges）：對於大多數服務，AWS 不對「數據傳輸」收取任何費用
- 配置的 IAM 角色數量（The number of IAM roles provisioned）：IAM 及其所有功能均可免費使用

影響 EC2 成本：
- 類型
- 區域（非可用區）
- 實例數
- ELB 服務、EBS 卷
- 分配的彈性 IP 地址

不影響 EC2 成本：
- 安全組數：免費使用
- **托管區域數**：不是免費的，但它們與 Amazon EC2 成本無關。托管區域是 Amazon Route 53 成本的因素之一

影響 S3 定價四個因素：
- S3 數據總量
- S3 類型
- 從 S3 傳出 AWS 的數據量（Amount of data transferred out of AWS from S3）
- 對 S3 的請求數（Number of requests to S3）

不影響 S3 定價基於四個因素
- 默認加密
- 創建和刪除存儲桶
- 免費使用訪問控製列表 (ACL) 

影響 EBS 定價兩個因素：
- 卷的配置容量
- 快照

### AWS Support 計劃

基本支持計劃
- 免費
- 包括訪問白皮書、文檔和支持社區
- 可以使用**有限的 AWS Trusted Advisor** 檢查功能
- AWS 知識中心（AWS Knowledge Center）：幫助回答 AWS 客戶最常提出的問題

開發人員計劃（Developer Support）：12小時回應服務
- 成本最低
- 最佳實踐指導
- 客戶端診斷工具
- 構建塊架構支持，其中包括有關如何結合使用 AWS 產品、功能和服務的指導

業務計劃（Business Support）：1小時回應服務
- 成本居中
- 使用案例指導，用於確定最適合您的特定需求的 AWS 產品、功能和服務
- **所有 AWS Trusted Advisor 檢查**
- 針對第三方軟件（例如常見的操作系統和應用程序堆棧組件）的有限支持
- 為技術支持工程師提供 **24x7** 全天候訪問

企業支持計劃（Enterprise Support）：15分鐘回應服務
- 成本最高
- 功能最全：包含基本、開發人員和業務支持計劃中所有功能
- 應用程序架構指導：可支持公司的特定使用案例和應用程序的咨詢關系
- 基礎設施事件管理：通過 AWS Support 展開短期合作 → 了解使用案例、提供架構和擴展指導
- **技術客戶經理（TAM）**：
    - TAM 是您在 AWS 的主要聯系人
    - 他們可以在您計劃、部署和優化應用程序時提供指導和架構審核，並與您的公司保持溝通
- **AWS Support Concierge**：
    - 僅適用於 AWS Enterprise 或 Enterprise On-Ramp 支持計劃
    - 專門處理企業賬戶的 AWS **計費和賬戶**專家

### 其他

AWS Professional Services：
- 幫助客戶實現他們期望的業務成果

Amazon Connect：
- 是一種基於雲的聯絡中心解決方案
- 可讓您輕松設置和管理客戶聯絡中心，並提供任何規模的可靠客戶互動

AWS Marketplace：
- 數字目錄軟件

AWS Health Dashboard
- 服務運行狀況的個性化視圖
- 主動通知
- 詳細的故障排除指南

## 遷移和創新

### AWS 雲采用框架 (AWS CAF)

- 業務：確保您的業務策略和目標與您的 IT 策略和目標保持一致
- 人員：評估組織結構和角色、新技能和流程要求
- 監管：確定和實施 IT 監管的最佳實踐，並利用技術為業務流程提供支持
- 平臺：幫助您根據業務目標和視角來設計、實施和優化 AWS 基礎設施
- 安全：確保組織滿足可見性、可審核性、控製和敏捷性等方面的安全目標
- 運維：運營和恢復 IT 工作負載，以滿足業務利益相關方的要求

### 遷移策略

- 重新托管
- 更換平臺
- 重構/重新設計架構
- 重新購買
- 保留
- 停用

### AWS Snow 系列

- AWS Snowcone：具有 2 個 CPU、4GB 內存和 8TB 的可用存儲容量
- AWS Snowball：內置計算能力，允許客戶在本地處理數據
    - Snowball Edge Storage Optimized：80TB 硬盤驅動器 (HDD) 容量
    - Snowball Edge Compute Optimized：42TB 的可用 HDD 容量
- AWS Snowmobile：80 - 100PB 的數據

### 其他遷移

- AWS Migration Evaluator：構建雲規劃和遷移創建定向業務案例
- AWS Migration Hub：提供單一位置來跟蹤跨多個 AWS 和合作夥伴解決方案的應用程序遷移進度
- Database Migration Service (AWS DMS)：在最廣泛使用的商業和開源數據庫之間遷移數據

### 利用 AWS 進行創新

- Amazon Transcribe 將語音轉換為文本
- **Amazon Comprehend** 自然語言處理 (NLP) 服務
- Amazon Fraud Detector 識別潛在的欺詐性在線活動
- Amazon Lex 構建語音和文本聊天機器人
- Amazon SageMaker 構建、訓練和部署機器學習模型
- **Amazon Rekognition** 基於深度學習的視覺搜索和圖像分類檢測圖像中的對象、場景和面部

## 其他筆記

AWS Service Catalog：
- 創建和集中管理常用部署的 IT 服務
- 幫助現一致的治理、快速部署，以及滿足合規性要求

AWS Config：
- 發現現有和已刪除的 AWS 資源
- 確定您對規則的整體合規性
- 並隨時深入了解資源的配置詳細信息

AWS Cloud9：
- 基於雲的集成開發環境 (IDE)，只要瀏覽器即可編寫、運行和調試代碼
- Cloud9 預裝了適用於流行編程語言（包括 JavaScript、Python、PHP 等）的基本工具

Amazon Chime：
- 用於在線會議的通信服務

Amazon Cognito：
- 讓客戶可以快速輕松地將用戶註冊、登錄和訪問控製添加到他們的 Web 和移動應用程序
- 支持通過 SAML 2.0 使用社交身份提供商（例如 Facebook、Google 和 Amazon）以及企業身份提供商進行登錄

Amazon CloudSearch：
- 是 AWS 雲中的一項托管服務
- 可讓輕松且經濟高效地為您的網站或應用程序設置、管理和擴展搜索解決方案

Amazon Athena：
- 交互式查詢服務，可以使用標準 SQL 輕松分析 Amazon S3 中的數據

AWS Global Accelerator：
- 是一項網絡服務，可提高您向全球用戶提供的應用程序的可用性和性能

Amazon Elastic Map Reduce (Amazon EMR)：
- 是一項 Web 服務，使企業、研究人員、數據分析師和開發人員能夠輕松且經濟高效地處理大量數據
- Amazon EMR 讓您可以專註於處理或分析數據，而不必擔心 Hadoop 集群的耗時設置、管理或調整或它們所在的計算容量

Amazon Simple Email Service (Amazon SES)：
- 是一種基於雲的電子郵件發送服務，旨在幫助數字營銷人員（digital marketers）和應用程序開發人員（application developers）發送營銷、通知和交易電子郵件

## 考試

可以自動擴展實例（instances auto-scaling）：
- EC2（須先設定Auto Scaling）
- Lambda
- Amazon Aurora

可以自動擴展容量（storage auto-scaling）：
- EFS
- S3
- 除 Amazon Aurora 之外的 RDS
- DynamoDB（無服務）
    
不能自動擴展：
- EBS
- Storage Gateway

無服務：
- AWS Lambda
- AWS Fargate
- AWS DynamoDB
- AWS Neptune

可以用作計算資源：
- EC2
- Lambda

---