---
layout: post
title: AWS - Cloud Practitioner Essentials Detail (Official)
category: it-operations
tags: [aws]
---

## AWS Summary

AWS Cloud Practitioner Essentials:
- [Self-paced digital training on AWS - AWS Skill Builder](https://explore.skillbuilder.aws/learn/mycourses)

resource:
- [AWS CCP證照準備紀錄與心得分享 \| Terahake](https://terahake.in/post/aws-ccp-certified-exp/)
- [【考試篇】三個月考過 AWS Solution Architect Associate (SAA-C02)—考試重點與心得、線上考試註意事項](shorturl.at/crvw8)
- [【考試篇】AWS 證照線上考試報名 — 圖文詳細教學 - Sunny』s Life in CMU 🔅 - Medium](https://medium.com/cloud-guru-%E7%9A%84%E5%BE%81%E9%80%94/%E8%80%83%E8%A9%A6%E7%AF%87-aws-%E8%AD%89%E7%85%A7%E7%B7%9A%E4%B8%8A%E8%80%83%E8%A9%A6%E5%A0%B1%E5%90%8D-%E5%9C%96%E6%96%87%E8%A9%B3%E7%B4%B0%E6%95%99%E5%AD%B8-e48ecd861328)

## 雲計算

定義：通過雲服務平臺經由Internet 按需提供計算能力、數據庫、存儲、應用程序及其他IT 資源，並且采用隨用隨付的定價方式。

類型：
- Infrastructure as a Service (Laas):
    - Provide building blocks for cloud IT
    - Provide networking, computers, data storage space
    - Highest level of flexibility
    - Easy parallel with traditional on-premises IT
- Platform as a Service (Paas):
    - Removes the need for your organization to manage the underlying infrastructure
    - Focus on the deployment and management of your applications
- Software as a Service (Saas):
    - Completed product that is run and managed by the service provider

範例：
- Infrastructure as a Service:
    - **Amazon EC2 (on AWS)**
    - GCP, Azure, Rackspace, Digital Ocean, Linode
- Platform as a Service:
    - **Elastic Beanstalk (on AWS)**
    - Heroku, Google Aapp Engine (GCP), Windows Azure (Microsoft)
- Sofeware as a Service:
    - Many AWS services (ex: Rekognition for Machine Learning)
    - Google Apps (Gmail), Dropbox, Zoom

全球服務：
- IAM
- AWS Health Dashboard

雲計算部署模式：
- 雲端部署（Cloud）
- 本地部署（On-premises）（私有雲部署）
- 混合部署（**Hybrid**）

雲計算的六大優勢：
- 將前期費用轉變為可變支出。（作為**運營成本**而不是資本支出產生）
- 實現**規模經濟效益**。
- 無需猜測容量。
- 提高速度和敏捷性。
- 不再花費資金來運行和維護數據中心。
- 數分鐘內實現全球化部署。

其他考試提到的優勢：
- AWS 繼續為其客戶降低雲計算成本

### 設計原則

AWS 架構完善的框架
- 卓越運營（Operational Excellence）：運行和監控系統
- 安全性（Security）：保護信息和系統（eg. AWS CloudTrail）
- 可靠性（Reliability）：恢復中斷系統
- 性能效率（Performance）：有效使用**計算資源**
- 成本優化：避免或消除不需要的成本或次優資源
- 可持續性：解決對環境、經濟和社會的長期影響

Ref:
- [AWS Well-Architected – 構建安全、高效的雲應用程序](https://aws.amazon.com/cn/architecture/well-architected/)
- [Design Principles - Operational Excellence Pillar](https://docs.aws.amazon.com/wellarchitected/latest/operational-excellence-pillar/design-principles.html)
- [Security Foundations - Security Pillar](https://docs.aws.amazon.com/wellarchitected/latest/security-pillar/security.html)
- [Design Principles - Reliability Pillar](https://docs.aws.amazon.com/wellarchitected/latest/reliability-pillar/design-principles.html)
- [Design Principles - Performance Efficiency Pillar](https://docs.aws.amazon.com/wellarchitected/latest/performance-efficiency-pillar/design-principles.html)

## 基礎設施

### Region, AZ, Edge Location

三個基礎設施
- 區域（Region）：數據中心集群，包含 AWS 資源的地理區域
- 可用區（Availability Zones）：
    - 一個區域內的單個數據中心或一組數據中心
    - 提高應用程序的容錯能力
    - 所有可用區都具有**相同的安全級別**
    - **與降低延遲無關**，尤其是當這些可用區位於同一區域時
- 邊緣站點（Edge Location）：
    - 將緩存的內容**副本**存儲在更靠近客戶的位置以加快分發速度
    - 邊緣位置**不用於災難恢復**
    - 邊緣站點位於世界上大多數**主要城市**。邊緣站點**可能存在也可能不存在**於給定的 AWS **區域**中

區域四項業務因素：
- 合規性（Compliance）（**數據主權**）
- 距離（Proximity）
- 可用服務（Available services）
- 定價（Pricing）

AWS CloudFront: 
- 內容分發服務
- 它利用邊緣站點網絡來緩存內容並將其分發給全球客戶
- 與 AWS Shield 和 AWS WAF 集成以防止網絡和應用程序層 DDoS 攻擊

### 預置資源

與AWS服務交互的方式：
- AWS CLI: 通過腳本自動執行 AWS 服務和應用程序的操作
- AWS SDK: 采用受支持的編程語言開發 AWS 應用程序

AWS Outposts: 
- 是一種**配置管理服務**，提供 Chef 和 Puppet 的托管實例。**Chef 和 Puppet 是自動化平臺**，允許您使用代碼來自動化服務器的配置
- 將 AWS 基礎設施和服務擴展到**您的本地數據中心**
- 這項服務讓您能夠采用**混合雲**方法運行基礎設施

AWS CodeDeploy：
- 可自動將應用程序代碼部署到任何實例（包括 Amazon EC2 實例和本地運行的實例）
- 可以更輕松地快速發布新功能，幫助避免部署期間的停機，並處理更新應用程序的復雜性
- **不能**用於管理 AWS **基礎設施**（無法使用 AWS CodeDeploy 構建和自動化 AWS 環境）

Amazon S3 Transfer Acceleration：
- 可在您的客戶端和 S3 存儲桶之間實現快速、輕松且安全的長距離文件傳輸。
- Transfer Acceleration 利用 Amazon CloudFront 的全球分布式邊緣站點。
- 當數據到達邊緣位置時，數據會通過優化的網絡路徑路由到 Amazon S3

AWS Ekastic Beanstalk: 
- 提供代碼和配置設置

AWS CloudFormation: 
- 將**基礎設施**視為代碼，通過編寫代碼來構建環境

## 安全性

### 責任共擔模式

- 客戶：雲中的安全
    - 構建應用程序架構
    - **分析網絡性能**
    - 配置安全組和網絡 ACL
    - **加密**其數據
- AWS：雲的安全性
    - 數據中心的物理安全
    - **創建管理程序**
    - 更換舊磁盤驅動器
    - 基礎設施的補丁管理

繼承控製：
- 物理控製
- 環境控製 v
- 題目選項：數據中心安全控製 v

共享控製：
- 定義：適用於基礎設施層和客戶層（apply to both the infrastructure layer and customer layers）
- 包括：
    - **補丁管理**：AWS 負責修補和修復基礎設施內的缺陷，而客戶負責修補其來賓操作系統和應用程序
    - **配置管理**：AWS 負責維護基礎設施設備的配置，而客戶負責配置自己的來賓操作系統、數據庫和應用程序
    - 認知和培訓：AWS 負責培訓 AWS 員工，而客戶必須負責培訓自己的員工

- [責任共擔模式 – Amazon Web Services (AWS)](https://aws.amazon.com/cn/compliance/shared-responsibility-model/)

### IAM

用戶權限和訪問 Identity and Access Management
- IAM 用戶組和角色
- IAM 策略
- 多重驗證（MFA）

與 IAM 進行交互的方法：
- AWS 管理控製臺（AWS Management Console）
- AWS 命令​​行工具（AWS Command Line Tools, CLI）
- AWS 開發工具包（AWS Software Development Kits, SDK）

以下需要訪問密鑰 ID 和秘密訪問密鑰才能以編程方式長期訪問 AWS 資源：
- IAM account root user
- IAM user

- IAM Credentials Report (account-level)
    - a report that lists all your account's users and the status of their various credentials

- IAM Access Advisor (user-level)
    - Access advisor shows the service permissions granted to a user and when those services were last accessed.
    - You can use this information to revise your policies

保護權限措施：
- 更改root用戶帳戶的電子郵件地址和密碼
- 在 root 用戶帳戶上啟用 MFA
- 輪換（更改）所有帳戶的所有訪問密鑰
- 更改所有IAM用戶的用戶名和密碼
- 在所有 IAM 用戶賬戶上啟用 MFA

選項不正確：
- 刪除所有 IAM 賬戶並重新創建它們：會導致運營中斷
- 在安全的地方下載所有附加的策略：這只是備份，沒太大用處

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

Ref: [Getting credential reports for your AWS account - AWS Identity and Access Management](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_getting-report.html)

### 拒絕服務攻擊

AWS Shield：
- 標準：阻擋DDoS
- 高級：阻擋、診斷、檢測、緩解DDoS

### 其他安全服務

- 加密密鑰（AWS KMS）：使客戶能夠輕松創建和控製用於**加密操作的密鑰**（eg. MFA）
- 訪問秘鑰：訪問密鑰是可用於簽署對 AWS 的**編程請求**（CLI, API）的長期憑證
- 網絡應用程序防火墻（AWS WAF）：監控進入 Web 應用程序的網絡請求的服務
- 自動化安全性評估（Amazon Inspector）：檢查應用程序的安全漏洞以及偏離最佳實踐的情況的服務
- 智能威脅檢測（Amazon GuardDuty）：持續監控 AWS 基礎設施並幫助檢測攻擊者偵查或賬戶泄漏等威脅

## 聯網

### 網關和連接

互聯網網關：
- 將 VPC 連接到互聯網

Amazon Virtual Private Cloud (Amazon VPC)：
- 在 AWS 雲中預置出隔離的部分。在這個隔離的部分中，您可以在自己定義的虛擬網絡中啟動各種資源
- 子網是 VPC 的一部分，可以包含 Amazon EC2 實例等資源
- 客戶可以**完全控製** VPC 虛擬網絡環境，也就是所有管理和配置細節

AWS Direct Connect：
- 在數據中心和 VPC 之間建立專用私有連接
- 可以降低網絡成本，同時增加通過網絡傳輸的帶寬

AWS Transit Gateway：
- 網絡中轉中心：每個網絡只需連接到 Transit Gateway，而無需連接到其他所有網絡
- 簡化 VPC 之間的連接管理：每個 VPC 只需連接到 Transit Gateway 而不必連接到其他所有 VPC

### 子網和網絡訪問控製列表

子網：
- 是一個用於將資源歸為一組的獨立區域
- 是 VPC 的一部分
- 可以是公有子網，也可以是私有子網

網絡訪問控製列表 (ACL)：
- 網絡訪問控製列表 (ACL) 是一種虛擬防火墻，用於在**子網**級別控製入站和出站流量
- 默認允許所有入站和出站流量，並檢查所有出入白名單與黑名單是否放行
- 無狀態數據包篩選

安全組：
- 安全組是一種虛擬防火墻，用於控製 **Amazon EC2 實例**的入站和出站流量
- 默認拒絕所有入站流量，並允許所有出站流量
- 有狀態數據包篩選

### 全球聯網

Amazon Route 53：
- 是一項 DNS Web 服務
- 是管理域名的 DNS 記錄
- 可以對 Amazon EC2 實例、 Web 服務器和其他資源執行**健康檢查**
- 配置運行狀況檢查以僅將流量路由到運行狀況良好的端點
- 通過多種路由類型管理全球應用程序流量（跨區域）。
## EC2

- 快速預置、啟動和隨時停止
- 按需使用與付費

### 實例類型

- 通用實例（General Purpose）：均衡計算
- 計算優化實例（Compute Optimized）：提供高性能處理器，適合批次處理
- 內存優化實例（Memory Optimized）：非常適合高性能數據庫
- 存儲優化實例（Storage Optimized）：適用於數據倉庫應用程序

### 定價

- 按需實例（On-Demand Instances）：非常適合不能中斷的短期無規律工作負載。沒有前期成本或最低費用要求。
- Savings Plans：通過承諾在 1 年期或 3 年期內保持穩定的計算使用量來降低計算成本。
- 預留實例（Reserved）：購買 1 年期或 3 年期標準預留實例和可轉換預留實例，也可以購買 1 年期計劃預留實例。
    - 預留實例（無預付費用）：無預付款選項不需要任何預付款，並在期限內提供打折的小時費率
    - 預留實例（部分預付）：部分預付選項比無預付選項更具成本效益（預付越多，節省的越多）
    - 預留實例（**全部預先 All Upfront**）：你預付的錢越多，你得到的折扣就越多
- Spot實例（Spot Instances）：非常適合開始時間和結束時間靈活的工作負載，以及可以承受中斷的工作負載。
- 專用**主機**（Capacity Reservations）：指 EC2 實例容量完全供您專用的物理服務器。

Ref:
- [Dedicated Instances - Amazon Elastic Compute Cloud](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/dedicated-instance.html)
- [Amazon EC2專用主機_AWS EC2虛擬服務器_雲服務器 -AWS雲服務](https://aws.amazon.com/cn/ec2/dedicated-hosts/)

### Auto Scaling

Amazon EC2 Auto Scaling
- 更好的容錯性：
    - Amazon EC2 Auto Scaling 可以檢測實例何時運行狀況不佳、終止它並啟動一個實例來替換它
    - Amazon EC2 Auto Scaling 能夠通過**跨區域內多個可用區**的 Auto Scaling 組來利用地理冗余的安全性和可靠性
    - 當一個可用區變得不正常或不可用時，Auto Scaling 會在未受影響的可用區中啟動新實例
    - 當不正常的可用區恢復到正常狀態時，Auto Scaling 會自動在所有指定的可用區中均勻地重新分配應用程序實例
- 更好的成本管理：只需要在使用實例時為實際使用的實例付費

限製：
- 不能跨越多個區域
- 不分配流量（這是 ELB 的工作）
- 不執行任何緩存（這是 CloudFront 的工作）

### ELB

Elastic Load Balancing
- 應用程序負載均衡器：Application Load Balancer 最適合 HTTP 和 HTTPS 流量的負載平衡
- 網絡負載均衡器：Network Load Balancer 最適合 TCP 和 TLS 流量的負載均衡
- 網關負載均衡器
- 經典負載均衡器

### SNS & SQS

SNS
- Amazon Simple Notification Service (Amazon SNS) 是一項發布/訂閱服務。
- 發布者使用 Amazon SNS 主題將消息發布給訂閱者。
- 這與咖啡店類似：收銀員向製作咖啡的咖啡師提供咖啡訂單。

SQS
- Amazon Simple Queue Service (Amazon SQS) 是一項消息隊列服務。
- 借助 Amazon SQS，您可以在軟件組件之間發送、存儲和接收消息，而且不會丟失消息，也不需要使用其他服務。

### Lambda

無服務器：
- 指您的代碼在服務器上運行，但您無需預置或管理這些服務器
- 您可以將更多精力放在新產品和功能創新上，而不是放在維護服務器上

AWS Lambda
- 是一種讓您能夠在無需預置或管理服務器的情況下運行代碼的服務（例如**容量管理**）
- 使用 AWS Lambda 時，您只需為使用的計算時間付費
- Lambda 只在被觸發時才運行代碼（例如 AWS 服務、移動應用程序或 HTTP 終端節點）
- 原生 Lambda 可以使用 API 支持**任何編程語言**（eg. Java、Go、PowerShell、Node.js、C#、Python）

### Container

- Elastic Container Service (ECS)：with Docker
- Elastic Kubernetes Service (EKS)：with Kubernetes
- AWS Fargate：無服務器計算引擎，允許客戶在無需管理服務器或集群的情況下運行容器

## 存儲和數據庫

### 實例存儲 & EBS

實例存儲：
- 實際掛載到 EC2 實例宿主機的磁盤存儲，因而具有與該實例相同的生命周期
- 當實例終止時，實例存儲中的所有數據都將**丟失**

Amazon Elastic Block Store (Amazon EBS)：
- 塊級存儲：適用於數據庫和動態網站，**非計算服務**
- **單個**可用區中
- 高讀寫需求、需要頻繁和精細更新的數據、持久保存數據
- 生命周期獨立：即使停止或終止 Amazon EC2 實例，掛載的 EBS 卷上的所有數據仍然可用（仍然繼續付費）
- 使用量付費
- **Amazon RDS 數據庫實例使用的主要存儲服務是 EBS**
- 一個卷只能附加到**一個**計算資源

Amazon EBS 快照：
- 增量備份
- 對卷進行的第一次備份會復製所有數據。後續備份則只保存自最近一次快照以來發生更改的數據塊

Ref:
- [Amazon Elastic Block Store (Amazon EBS) - Amazon Elastic Compute Cloud](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AmazonEBS.html)
- [塊存儲、文件存儲、對象存儲這三者的本質差別是什麽？ - 知乎](https://www.zhihu.com/question/21536660)

## S3

對象存儲 Amazon Simple Storage Service (Amazon S3)：
- Amazon S3 是一種對象存儲，用於存儲和檢索來自任何地方的任意數量的數據
- Amazon S3 可以上傳任何類型的文件 ，例如圖片、視頻、文本文件等
- Amazon S3 存儲的**數據總量**和**對象數量**不受限製，但每個對象都有大小限製。單個 Amazon S3 對象的大小範圍可以從最小 0 字節到最大 5 TB
- 提供 99.999999999% 的耐用性
- 為實際使用量付費（**批量折扣**）

S3補充說明：
- S3 設計初衷就是為任何 Internet 應用程序處理流量
- S3 的大規模允許平均分配負載（Load Balance）:
    - 沒有單個應用程序受到流量峰值的影響
    - 無需創建多個 S3 存儲桶並在它們之間分配流量
    - 無需任何幹預即可處理任意數量的流量

S3通用：
- S3標準（S3 Standard）：
    - **頻繁訪問**
    - [托管靜態網站](https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteHosting.html)
    - 較低的延遲和較高的吞吐量性能
    - 支持傳輸中數據 SSL 和靜態數據加密
- S3智能分層（S3 Intelligent-Tiering）：
    - **監控對象的訪問模式**
    - 適合存儲訪問模式未知或不斷變化的數據
    - 支付少量的對象監控和自動化費用
- 不頻繁訪問：
    - S3標準 IA（S3 Stander IA）：不常訪問、但在需要時要求快速訪問的數據，檢索價格較高
    - S3**單區** IA（S3 One Zone IA）：將數據存儲在單個可用區中
- 歸檔（數據存檔設計）：
    - S3 Glacier 即時檢索：檢索時間最快
    - S3 Glacier 靈活檢索：檢索時間幾分鐘
    - S3 Glacier Deep Archive：檢索時間為 12 小時以內（12 至 48 小時），成本最低

S3 ACL：
- Amazon S3 訪問控製列表 (ACL) 使您能夠管理對存儲桶和對象的訪問。每個存儲桶和對象都有一個作為子資源附加到它的 ACL。您可以使用 ACL 向其他 AWS 賬戶授予基本讀/寫權限。當收到針對資源的請求時，Amazon S3 會檢查相應的 ACL 以驗證請求者是否具有必要的訪問權限
- Amazon S3 ACL 不同於網絡 ACL。網絡訪問控製列表 (Network ACL) 是您的 VPC 的可選安全層，它充當防火墻，用於控製進出一個或多個子網的流量
- 使用 Amazon S3 ACL **不收取額外費用**

Ref:
- [Amazon S3 雲存儲_對象存儲_雲存儲服務-AWS雲服務](https://aws.amazon.com/cn/s3/)
- [Amazon S3存儲類別_AWS雲存儲服務-AWS雲服務](https://aws.amazon.com/cn/s3/storage-classes/)

### Elastic File System

文件存儲 Amazon Elastic File System (Amazon EFS)：
- 是一項**區域性**服務
- 將數據存儲在**多個**可用區中
- 添加和刪除文件時，Amazon EFS 會**自動擴展和縮減**
- 文件存儲非常適合用於大量服務和資源需要**同時訪問**相同數據

### RDS

Amazon Relational Database Service (Amazon RDS)：
- 提供經濟高效且**可調整大小的容量**
- 自動執行耗時的管理任務，例如**硬件配置、數據庫設置、修補和備份。**
- **專註於應用程序**

Amazon Relational Database Service (Amazon RDS)
- Amazon Aurora：將傳統企業數據庫的性能和可用性與開源數據庫的簡單性和成本效益相結合
- PostgreSQL
- MySQL
- MariaDB
- Oracle Database
- Microsoft SQL Server

使用 Amazon RDS 屬於責任共擔模型：
- 客戶的責任：
    - 管理數據庫設置
    - 構建關系數據庫模式
- AWS的責任：
    - 安裝數據庫軟件
    - 執行備份
    - 修補數據庫軟件

### Amazon DynamoDB

Amazon DynamoDB
- 是一項鍵值對和文檔數據庫服務（**非結構化 / 非存儲服務**）。
- **無（實例）服務器架構**
- **低延遲**：可以在任意規模實現不超過 10 毫秒的延遲
- **自動擴展容量**

### Amazon Redshift

Amazon Redshift 
- 是一項數據倉庫服務，可用於進行大數據分析
- 它讓您能夠從多個來源收集數據，並幫助您了解數據中的關系和趨勢

### DMS

AWS Database Migration Service (AWS DMS) 
- 讓您能夠遷移關系數據庫、非關系數據庫和其他類型的數據存儲
- 源數據庫和目標數據庫可以是相同的類型，也可以是不同的類型

### 其他DB

Amazon ElastiCache：
- 提供內存數據庫存儲服務
- 提高 web 應用程序性能

AWS Storage Gateway：
- **混合**存儲服務，可讓**本地**應用程序無縫使用 AWS 雲存儲
- 可以使用該服務進行備份和歸檔、災難恢復、雲數據處理、存儲分層和遷移
- **成本效益大於 S3**
- 為使用的容量付費，並且可以根據需要進行擴展（**無法自動擴展**）

Amazon Neptune：
- 完全托管的圖形數據庫服務（完全托管並處理耗時的任務，例如預置、修補、備份、恢復、故障檢測和修復）
- 可以輕松構建和運行與高度連接的數據集（例如社交網絡、推薦引擎和知識圖譜）一起使用的應用程序

## 監控和分析

### Amazon CloudWatch

- 實時監控 AWS 基礎設施和資源
- 查看指標和圖形以監控資源的性能
- 配置自動操作和**警報**來響應指標

### AWS CloudTrail

- 自動檢測異常賬戶活動
- 篩選日誌以幫助執行操作分析和故障排除
- 為治理、**合規性**和風險審計提供了很好的資源

### AWS Trusted Advisor

- 成本優化：對可消除且提供成本節省的未使用和空閑的資源的各種檢查
- 性能：
    - 通過建議如何利用預置的吞吐量來幫助您提升服務的性能
    - 對服務限製和過度使用的實例的檢查
- 安全性：查看權限以及識別要啟用的 AWS 安全功能的各種檢查
- 容錯能力：提高應用程序的可用性和冗余性的各種檢查
- 服務配額：
    - 服務配額是可以在 AWS 賬戶中創建的最大資源數。
    - AWS 實施配額，旨在為所有客戶提供高度可用且可靠的服務，並避免您支付意外費用

### 其他

AWS X-Ray：
- 是一種調試服務，可幫助開發人員分析和**調試**生產或開發中的分布式應用程序
- 借助 X-Ray，您可以**了解、跟蹤**您的應用程序及其底層服務的執行情況，以便識別和排除性能問題和錯誤的根本原因

## 定價和支持

### AWS 免費套餐

- 永久免費：
    - AWS Lambda 每月提供 100 萬個免費請求和長達 320 萬秒的計算時間
    - Amazon DynamoDB 每月提供 25GB 的免費存儲空間
- 12 個月免費：
    - 特定數量的 Amazon S3 標準存儲
    - 每月 Amazon EC2 計算時間的閾值
    - Amazon CloudFront 數據傳出量
- 試用：
    - Amazon Inspector 提供 90 天免費試用
    - Amazon Lightsail（一項讓您能夠運行虛擬專用服務器的服務）在 30 天內提供 750 小時的免費使用

### AWS 定價概念

定價原理
- 按需付費
- 預留容量，付費更少
- 使用越多，折扣越多

工具 & 服務：
- AWS 定價計算器：AWS 定價計算器只是一種根據您的預期使用情況估算每月 AWS 賬單的工具（估算用）
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

Ref: [Cloud Financial Management - Overview of Amazon Web Services](https://docs.aws.amazon.com/whitepapers/latest/aws-overview/aws-cost-management.html#aws-cost-usage)

EC2 實例定價取決於許多變量：
- 購買選項（按需、儲蓄計劃、預留、現貨、專用）
- 選定的實例類型
- 選定區域
    - Amazon EC2 實例的價格可能會因預置實例的區域而異。
    - 在同一區域內的**不同可用區**中預置的 Amazon EC2 實例具有相同的價格
- 實例數
- **負載均衡**
- 分配的彈性 IP 地址
    - 私有 IP 不收費。
    - 分配的彈性 IP 數量是可能影響 Amazon EC2 費用的因素。為確保有效使用彈性 IP 地址，如果彈性 IP 地址未與正在運行的實例相關聯，或者與已停止的實例相關聯，AWS 會按小時收取少量費用。在實例運行時，您無需為與該實例關聯的一個彈性 IP 地址付費，但額外的彈性 IP 不是免費的。

減少 EC2 成本：
- 刪除所有彈性負載均衡器
- 終止（terminate）所有未使用的 EC2 實例
- 刪除他們不需要的附加 **EBS 卷**
- 釋放所有未使用的彈性 IP

Ref: [Amazon EC2實例價格_EC2虛擬雲服務器托管價格 - AWS雲服務](https://aws.amazon.com/cn/ec2/pricing/on-demand/)

不影響 EC2 成本：
- 安全組數：免費使用
- **托管區域數**：不是免費的，但它們與 Amazon EC2 成本無關。托管區域是 Amazon Route 53 成本的因素之一

影響 Lambda 成本：
- 計算時間消耗
- 函數的請求數
- x 存儲消耗、卷數：Lambda 不是存儲服務。它是一種運行應用程序的計算服務

影響整體成本因素：
- 計算費用（compute charges）
- 數據傳出費用（data transfer out charges）

不影響整體成本因素：
- 使用的服務數量（the number of services used）：您使用多少 AWS 服務並不重要。每項 AWS 服務都有自己的定價細節，其中許多是免費使用的
- 數據傳輸費用（data transfer in charges）：對於大多數服務，AWS 不對「數據傳輸」收取任何費用
- 配置的 IAM 角色數量（The number of IAM roles provisioned）：IAM 及其所有功能均可免費使用

影響 S3 定價四個因素：
- 存儲在 S3 上的數據總量（以 GB 為單位）
- 存儲類（S3 Standard、S3 Intelligent-Tiering、S3 Standard-Infrequent Access、S3 One Zone-IA、S3 Glacier 或 S3 Glacier Deep Archive）
- 從 S3 傳出 AWS 的數據量（Amount of data transferred out of AWS from S3）
- 對 S3 的請求數（Number of requests to S3）

不影響 S3 定價基於四個因素
- 對任意數量的 S3 存儲桶使用默認加密：對 S3 存儲桶使用默認加密不會產生額外費用
- 創建和刪除 S3 存儲桶：創建或刪除 S3 存儲桶是免費的，但您需要為存儲在這些存儲桶中的數據付費
- 附加到 S3 存儲桶的訪問控製列表 (ACL) 的數量：使用 Amazon S3 ACL 不收取額外費用。

影響 EBS 定價兩個因素：
- 卷（Volumes）：所有 EBS 卷類型的卷存儲按您每月配置的 GB 量收費，直到您釋放存儲。 
- 快照（Snapshots）：
    - 快照存儲基於您的數據在 Amazon S3 中消耗的空間量
    - 由於 Amazon EBS 不保存空塊，因此快照大小可能會大大小於您的卷大小
    - 復製 EBS 快照是根據跨區域傳輸的數據量收費的

降低 EBS 成本：
- 刪除未附加的 EBS 卷：當 EC2 實例停止或終止時，附加的 EBS 卷**不會自動刪除，並且會繼續產生費用**，因為它們仍在運行
- 調整大小或更改 EBS 卷類型：識別未充分利用的卷並縮小它們的大小或更改卷類型
- 刪除過時的 Amazon EBS 快照：將其刪除以降低存儲成本
- x 使用預留：Amazon EBS 中沒有獨立於 Amazon EC2 的預留（reservations）

影響 CloudCront 成本：
- 請求數
- 流量分布
- x 實例類型：實例類型是影響 Amazon EC2 成本而非 Amazon CloudFront 成本的因素

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

基於 AWS Trusted Advisor 不同的支持計劃：
- 僅提供訪問6 項核心安全檢查：基本、開發
- 所有115 項 檢查：業務、企業

其他：
- AWS Professional Services：幫助客戶實現他們期望的業務成果
- Amazon Connect：是一種基於雲的聯絡中心解決方案。Amazon Connect 可讓您輕松設置和管理客戶聯絡中心，並提供任何規模的可靠客戶互動

Ref:
- [AWS Support 計劃比較 \| 開發人員、業務、企業、Enterprise On-Ramp | AWS Support](https://aws.amazon.com/cn/premiumsupport/plans/)

### AWS Marketplace

是一種數字目錄，其中包含獨立軟件供應商提供的上千款軟件產品。您可以使用 AWS Marketplace 查找、測試和購買在 AWS 上運行的軟件

優勢：
- 它通過**靈活的定價選項**和多種部署方法簡化了軟件許可和采購。靈活的定價選項包括免費試用、每小時、每月、每年、多年和 BYOL。
- 客戶只需點擊幾下即可**快速啟動預配置軟件**，並選擇 AMI 和 SaaS 格式以及其他格式的軟件解決方案。
- 它確保**定期掃描產品**以查找已知漏洞、惡意軟件、默認密碼和其他與安全相關的問題。

不正確：
- 提供在 AWS 或*任何其他雲供應商上*運行的軟件解決方案：AWS Marketplace 提供**僅在 AWS 上運行的軟件**解決方案

### AWS Health Dashboard

- 服務運行狀況的個性化視圖
- 主動通知
- 詳細的故障排除指南

### AWS 支持

- AWS Security team
- AWS Concierge team
- AWS Customer Service team
- AWS Abuse team

## 遷移和創新

### AWS 雲采用框架 (AWS CAF)

- 側重於業務能力：業務、人員和監管視角
    - 業務：確保您的業務策略和目標與您的 IT 策略和目標保持一致
    - 人員：評估組織結構和角色、新技能和流程要求
    - 監管：
        - 使 IT 策略與業務策略保持一致的技能和流程
        - 確定和實施 IT 監管的最佳實踐，並利用技術為業務流程提供支持
- 側重於技術能力：平臺、安全和運維視角
    - 平臺：
        - 實施新解決方案以及將本地工作負載遷移到雲的原則和模式
        - 幫助您根據業務目標和視角來設計、實施和優化 AWS 基礎設施
    - 安全：
        - 確保組織滿足可見性、可審核性、控製和敏捷性等方面的安全目標
        - 安排控製措施的選擇和實施
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
- 發現現有和已刪除的 AWS 資源，並隨時深入了解資源的配置詳細信息
- 確定您對規則的整體合規性
- 無法用於**監控或設置 CPU 使用閾值**

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
-  DynamoDB（無服務）
    
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

會自動跨多可用區備份復製數據：
- Amazon Aurora
- S3

免費：
- AWS 公告（AWS Bulletins）
- AWS 安全博客（AWS Security Blog）

---