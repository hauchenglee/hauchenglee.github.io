---
layout: post
title: AWS - Cloud Practitioner Essentials (Official)
category: operations
tags: [aws]
---

## AWS Summary

AWS Cloud Practitioner Essentials:
- [Self-paced digital training on AWS - AWS Skill Builder](https://explore.skillbuilder.aws/learn/mycourses)

resource:
- [AWS CCP證照準備紀錄與心得分享 \| Terahake](https://terahake.in/post/aws-ccp-certified-exp/)
- [【考試篇】三個月考過 AWS Solution Architect Associate (SAA-C02)—考試重點與心得、線上考試注意事項](shorturl.at/crvw8)
- [【考試篇】AWS 證照線上考試報名 — 圖文詳細教學 - Sunny’s Life in CMU 🔅 - Medium](https://medium.com/cloud-guru-%E7%9A%84%E5%BE%81%E9%80%94/%E8%80%83%E8%A9%A6%E7%AF%87-aws-%E8%AD%89%E7%85%A7%E7%B7%9A%E4%B8%8A%E8%80%83%E8%A9%A6%E5%A0%B1%E5%90%8D-%E5%9C%96%E6%96%87%E8%A9%B3%E7%B4%B0%E6%95%99%E5%AD%B8-e48ecd861328)

## 云计算

定義：通过云服务平台经由Internet 按需提供计算能力、数据库、存储、应用程序及其他IT 资源，并且采用随用随付的定价方式。

类型：
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

范例：
- Infrastructure as a Service:
    - **Amazon EC2 (on AWS)**
    - GCP, Azure, Rackspace, Digital Ocean, Linode
- Platform as a Service:
    - **Elastic Beanstalk (on AWS)**
    - Heroku, Google Aapp Engine (GCP), Windows Azure (Microsoft)
- Sofeware as a Service:
    - Many AWS services (ex: Rekognition for Machine Learning)
    - Google Apps (Gmail), Dropbox, Zoom

全球服务：
- IAM
- AWS Health Dashboard

雲計算部署模式：
- 云端部署（Cloud）
- 本地部署（On-premises）（私有雲部署）
- 混合部署（**Hybrid**）

云计算的六大优势：
- 将前期费用转变为可变支出。（作为**运营成本**而不是资本支出产生）
- 实现**规模经济效益**。
- 无需猜测容量。
- 提高速度和敏捷性。
- 不再花费资金来运行和维护数据中心。
- 数分钟内实现全球化部署。

其他考试提到的优势：
- AWS 继续为其客户降低云计算成本

### 设计原则

AWS 架构完善的框架
- 卓越运营（Operational Excellence）：运行和监控系统
- 安全性（Security）：保护信息和系统（eg. AWS CloudTrail）
- 可靠性（Reliability）：恢复中断系统
- 性能效率（Performance）：有效使用**计算资源**
- 成本优化：避免或消除不需要的成本或次优资源
- 可持续性：解决对环境、经济和社会的长期影响

Ref:
- [AWS Well-Architected – 构建安全、高效的云应用程序](https://aws.amazon.com/cn/architecture/well-architected/)
- [Design Principles - Operational Excellence Pillar](https://docs.aws.amazon.com/wellarchitected/latest/operational-excellence-pillar/design-principles.html)
- [Security Foundations - Security Pillar](https://docs.aws.amazon.com/wellarchitected/latest/security-pillar/security.html)
- [Design Principles - Reliability Pillar](https://docs.aws.amazon.com/wellarchitected/latest/reliability-pillar/design-principles.html)
- [Design Principles - Performance Efficiency Pillar](https://docs.aws.amazon.com/wellarchitected/latest/performance-efficiency-pillar/design-principles.html)

## 基础设施

### Region, AZ, Edge Location

三个基础设施
- 区域（Region）：数据中心集群，包含 AWS 资源的地理区域
- 可用区（Availability Zones）：
    - 一个区域内的单个数据中心或一组数据中心
    - 提高应用程序的容错能力
    - 所有可用区都具有**相同的安全级别**
    - **与降低延迟无关**，尤其是当这些可用区位于同一区域时
- 边缘站点（Edge Location）：
    - 将缓存的内容**副本**存储在更靠近客户的位置以加快分发速度
    - 边缘位置**不用于灾难恢复**
    - 边缘站点位于世界上大多数**主要城市**。边缘站点**可能存在也可能不存在**于给定的 AWS **区域**中

区域四项业务因素：
- 合规性（Compliance）（**数据主权**）
- 距离（Proximity）
- 可用服务（Available services）
- 定价（Pricing）

AWS CloudFront: 
- 内容分发服务
- 它利用边缘站点网络来缓存内容并将其分发给全球客户
- 与 AWS Shield 和 AWS WAF 集成以防止网络和应用程序层 DDoS 攻击

### 预置资源

与AWS服务交互的方式：
- AWS CLI: 通过脚本自动执行 AWS 服务和应用程序的操作
- AWS SDK: 采用受支持的编程语言开发 AWS 应用程序

AWS Outposts: 
- 是一种**配置管理服务**，提供 Chef 和 Puppet 的托管实例。**Chef 和 Puppet 是自动化平台**，允许您使用代码来自动化服务器的配置
- 将 AWS 基础设施和服务扩展到**您的本地数据中心**
- 这项服务让您能够采用**混合云**方法运行基础设施

AWS CodeDeploy：
- 可自动将应用程序代码部署到任何实例（包括 Amazon EC2 实例和本地运行的实例）
- 可以更轻松地快速发布新功能，帮助避免部署期间的停机，并处理更新应用程序的复杂性
- **不能**用于管理 AWS **基础设施**（无法使用 AWS CodeDeploy 构建和自动化 AWS 环境）

Amazon S3 Transfer Acceleration：
- 可在您的客户端和 S3 存储桶之间实现快速、轻松且安全的长距离文件传输。
- Transfer Acceleration 利用 Amazon CloudFront 的全球分布式边缘站点。
- 当数据到达边缘位置时，数据会通过优化的网络路径路由到 Amazon S3

AWS Ekastic Beanstalk: 
- 提供代码和配置设置

AWS CloudFormation: 
- 将**基础设施**视为代码，通过编写代码来构建环境

## 安全性

### 责任共担模式

- 客户：云中的安全
    - 构建应用程序架构
    - **分析网络性能**
    - 配置安全组和网络 ACL
    - **加密**其数据
- AWS：云的安全性
    - 数据中心的物理安全
    - **创建管理程序**
    - 更换旧磁盘驱动器
    - 基础设施的补丁管理

继承控制：
- 物理控制
- 环境控制 v
- 题目选项：数据中心安全控制 v

共享控制：
- 定义：适用于基础设施层和客户层（apply to both the infrastructure layer and customer layers）
- 包括：
    - **补丁管理**：AWS 负责修补和修复基础设施内的缺陷，而客户负责修补其来宾操作系统和应用程序
    - **配置管理**：AWS 负责维护基础设施设备的配置，而客户负责配置自己的来宾操作系统、数据库和应用程序
    - 认知和培训：AWS 负责培训 AWS 员工，而客户必须负责培训自己的员工

- [责任共担模式 – Amazon Web Services (AWS)](https://aws.amazon.com/cn/compliance/shared-responsibility-model/)

### IAM

用户权限和访问 Identity and Access Management
- IAM 用户组和角色
- IAM 策略
- 多重验证（MFA）

与 IAM 进行交互的方法：
- AWS 管理控制台（AWS Management Console）
- AWS 命令​​行工具（AWS Command Line Tools, CLI）
- AWS 开发工具包（AWS Software Development Kits, SDK）

以下需要访问密钥 ID 和秘密访问密钥才能以编程方式长期访问 AWS 资源：
- IAM account root user
- IAM user

- IAM Credentials Report (account-level)
    - a report that lists all your account's users and the status of their various credentials

- IAM Access Advisor (user-level)
    - Access advisor shows the service permissions granted to a user and when those services were last accessed.
    - You can use this information to revise your policies

保护权限措施：
- 更改root用户帐户的电子邮件地址和密码
- 在 root 用户帐户上启用 MFA
- 轮换（更改）所有帐户的所有访问密钥
- 更改所有IAM用户的用户名和密码
- 在所有 IAM 用户账户上启用 MFA

选项不正确：
- 删除所有 IAM 账户并重新创建它们：会导致运营中断
- 在安全的地方下载所有附加的策略：这只是备份，没太大用处

### Organization

AWS Organizations
- 在一个中心位置整合和管理多个 AWS 账户
- 使用服务控制策略 (Service control policies, SCPs) **集中控制组织中所有账户**的权限
- SCP 可以应用于以下身份和资源：
    - 单个成员账户
    - 组织单位（OU）

### 合规性

AWS Artifact：
- 按需访问 AWS 安全性与合规性报告
- 审核、接受并管理与 AWS 签订的协议
- 允许客户下载 **AWS SOC** 和 **PCI** 报告
- AWS 账户所有者可以从此处获得其账户中所有用户的列表，包括其 AWS 凭证的状态

AWS Certificate Manager (ACM)：
- 提供服务器证书
- 快速购买和部署 SSL/TLS 证书
- 可以使用 ACM 或 IAM 来存储和部署服务器证书

Ref: [Getting credential reports for your AWS account - AWS Identity and Access Management](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_getting-report.html)

### 拒绝服务攻击

AWS Shield：
- 标准：阻挡DDoS
- 高级：阻挡、诊断、检测、缓解DDoS

### 其他安全服务

- 加密密钥（AWS KMS）：使客户能够轻松创建和控制用于**加密操作的密钥**（eg. MFA）
- 访问秘钥：访问密钥是可用于签署对 AWS 的**编程请求**（CLI, API）的长期凭证
- 网络应用程序防火墙（AWS WAF）：监控进入 Web 应用程序的网络请求的服务
- 自动化安全性评估（Amazon Inspector）：检查应用程序的安全漏洞以及偏离最佳实践的情况的服务
- 智能威胁检测（Amazon GuardDuty）：持续监控 AWS 基础设施并帮助检测攻击者侦查或账户泄漏等威胁

## 联网

### 网关和连接

互联网网关：
- 将 VPC 连接到互联网

Amazon Virtual Private Cloud (Amazon VPC)：
- 在 AWS 云中预置出隔离的部分。在这个隔离的部分中，您可以在自己定义的虚拟网络中启动各种资源
- 子网是 VPC 的一部分，可以包含 Amazon EC2 实例等资源
- 客户可以**完全控制** VPC 虚拟网络环境，也就是所有管理和配置细节

AWS Direct Connect：
- 在数据中心和 VPC 之间建立专用私有连接
- 可以降低网络成本，同时增加通过网络传输的带宽

AWS Transit Gateway：
- 网络中转中心：每个网络只需连接到 Transit Gateway，而无需连接到其他所有网络
- 简化 VPC 之间的连接管理：每个 VPC 只需连接到 Transit Gateway 而不必连接到其他所有 VPC

### 子网和网络访问控制列表

子网：
- 是一个用于将资源归为一组的独立区域
- 是 VPC 的一部分
- 可以是公有子网，也可以是私有子网

网络访问控制列表 (ACL)：
- 网络访问控制列表 (ACL) 是一种虚拟防火墙，用于在**子网**级别控制入站和出站流量
- 默认允许所有入站和出站流量，并检查所有出入白名单与黑名单是否放行
- 无状态数据包筛选

安全组：
- 安全组是一种虚拟防火墙，用于控制 **Amazon EC2 实例**的入站和出站流量
- 默认拒绝所有入站流量，并允许所有出站流量
- 有状态数据包筛选

### 全球联网

Amazon Route 53：
- 是一项 DNS Web 服务
- 是管理域名的 DNS 记录
- 可以对 Amazon EC2 实例、 Web 服务器和其他资源执行**健康检查**
- 配置运行状况检查以仅将流量路由到运行状况良好的端点
- 通过多种路由类型管理全球应用程序流量（跨区域）。
## EC2

- 快速预置、启动和随时停止
- 按需使用与付费

### 实例类型

- 通用实例（General Purpose）：均衡计算
- 计算优化实例（Compute Optimized）：提供高性能处理器，适合批次处理
- 内存优化实例（Memory Optimized）：非常适合高性能数据库
- 存储优化实例（Storage Optimized）：适用于数据仓库应用程序

### 定价

- 按需实例（On-Demand Instances）：非常适合不能中断的短期无规律工作负载。没有前期成本或最低费用要求。
- Savings Plans：通过承诺在 1 年期或 3 年期内保持稳定的计算使用量来降低计算成本。
- 预留实例（Reserved）：购买 1 年期或 3 年期标准预留实例和可转换预留实例，也可以购买 1 年期计划预留实例。
    - 预留实例（无预付费用）：无预付款选项不需要任何预付款，并在期限内提供打折的小时费率
    - 预留实例（部分预付）：部分预付选项比无预付选项更具成本效益（预付越多，节省的越多）
    - 预留实例（**全部预先 All Upfront**）：你预付的钱越多，你得到的折扣就越多
- Spot实例（Spot Instances）：非常适合开始时间和结束时间灵活的工作负载，以及可以承受中断的工作负载。
- 专用**主机**（Capacity Reservations）：指 EC2 实例容量完全供您专用的物理服务器。

Ref:
- [Dedicated Instances - Amazon Elastic Compute Cloud](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/dedicated-instance.html)
- [Amazon EC2专用主机_AWS EC2虚拟服务器_云服务器 -AWS云服务](https://aws.amazon.com/cn/ec2/dedicated-hosts/)

### Auto Scaling

Amazon EC2 Auto Scaling
- 更好的容错性：
    - Amazon EC2 Auto Scaling 可以检测实例何时运行状况不佳、终止它并启动一个实例来替换它
    - Amazon EC2 Auto Scaling 能够通过**跨区域内多个可用区**的 Auto Scaling 组来利用地理冗余的安全性和可靠性
    - 当一个可用区变得不正常或不可用时，Auto Scaling 会在未受影响的可用区中启动新实例
    - 当不正常的可用区恢复到正常状态时，Auto Scaling 会自动在所有指定的可用区中均匀地重新分配应用程序实例
- 更好的成本管理：只需要在使用实例时为实际使用的实例付费

限制：
- 不能跨越多个区域
- 不分配流量（这是 ELB 的工作）
- 不执行任何缓存（这是 CloudFront 的工作）

### ELB

Elastic Load Balancing
- 应用程序负载均衡器：Application Load Balancer 最适合 HTTP 和 HTTPS 流量的负载平衡
- 网络负载均衡器：Network Load Balancer 最适合 TCP 和 TLS 流量的负载均衡
- 网关负载均衡器
- 经典负载均衡器

### SNS & SQS

SNS
- Amazon Simple Notification Service (Amazon SNS) 是一项发布/订阅服务。
- 发布者使用 Amazon SNS 主题将消息发布给订阅者。
- 这与咖啡店类似：收银员向制作咖啡的咖啡师提供咖啡订单。

SQS
- Amazon Simple Queue Service (Amazon SQS) 是一项消息队列服务。
- 借助 Amazon SQS，您可以在软件组件之间发送、存储和接收消息，而且不会丢失消息，也不需要使用其他服务。

### Lambda

无服务器：
- 指您的代码在服务器上运行，但您无需预置或管理这些服务器
- 您可以将更多精力放在新产品和功能创新上，而不是放在维护服务器上

AWS Lambda
- 是一种让您能够在无需预置或管理服务器的情况下运行代码的服务（例如**容量管理**）
- 使用 AWS Lambda 时，您只需为使用的计算时间付费
- Lambda 只在被触发时才运行代码（例如 AWS 服务、移动应用程序或 HTTP 终端节点）
- 原生 Lambda 可以使用 API 支持**任何编程语言**（eg. Java、Go、PowerShell、Node.js、C#、Python）

### Container

- Elastic Container Service (ECS)：with Docker
- Elastic Kubernetes Service (EKS)：with Kubernetes
- AWS Fargate：无服务器计算引擎，允许客户在无需管理服务器或集群的情况下运行容器

## 存储和数据库

### 实例存储 & EBS

实例存储：
- 实际挂载到 EC2 实例宿主机的磁盘存储，因而具有与该实例相同的生命周期
- 当实例终止时，实例存储中的所有数据都将**丢失**

Amazon Elastic Block Store (Amazon EBS)：
- 块级存储：适用于数据库和动态网站，**非计算服务**
- **单个**可用区中
- 高读写需求、需要频繁和精细更新的数据、持久保存数据
- 生命周期独立：即使停止或终止 Amazon EC2 实例，挂载的 EBS 卷上的所有数据仍然可用（仍然继续付费）
- 使用量付费
- **Amazon RDS 数据库实例使用的主要存储服务是 EBS**
- 一个卷只能附加到**一个**计算资源

Amazon EBS 快照：
- 增量备份
- 对卷进行的第一次备份会复制所有数据。后续备份则只保存自最近一次快照以来发生更改的数据块

Ref:
- [Amazon Elastic Block Store (Amazon EBS) - Amazon Elastic Compute Cloud](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AmazonEBS.html)
- [块存储、文件存储、对象存储这三者的本质差别是什么？ - 知乎](https://www.zhihu.com/question/21536660)

## S3

对象存储 Amazon Simple Storage Service (Amazon S3)：
- Amazon S3 是一种对象存储，用于存储和检索来自任何地方的任意数量的数据
- Amazon S3 可以上传任何类型的文件 ，例如图片、视频、文本文件等
- Amazon S3 存储的**数据总量**和**对象数量**不受限制，但每个对象都有大小限制。单个 Amazon S3 对象的大小范围可以从最小 0 字节到最大 5 TB
- 提供 99.999999999% 的耐用性
- 为实际使用量付费（**批量折扣**）

S3补充说明：
- S3 设计初衷就是为任何 Internet 应用程序处理流量
- S3 的大规模允许平均分配负载（Load Balance）:
    - 没有单个应用程序受到流量峰值的影响
    - 无需创建多个 S3 存储桶并在它们之间分配流量
    - 无需任何干预即可处理任意数量的流量

S3通用：
- S3标准（S3 Standard）：
    - **频繁访问**
    - [托管静态网站](https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteHosting.html)
    - 较低的延迟和较高的吞吐量性能
    - 支持传输中数据 SSL 和静态数据加密
- S3智能分层（S3 Intelligent-Tiering）：
    - **监控对象的访问模式**
    - 适合存储访问模式未知或不断变化的数据
    - 支付少量的对象监控和自动化费用
- 不频繁访问：
    - S3标准 IA（S3 Stander IA）：不常访问、但在需要时要求快速访问的数据，检索价格较高
    - S3**单区** IA（S3 One Zone IA）：将数据存储在单个可用区中
- 归档（数据存档设计）：
    - S3 Glacier 即时检索：检索时间最快
    - S3 Glacier 灵活检索：检索时间几分钟
    - S3 Glacier Deep Archive：检索时间为 12 小时以内（12 至 48 小时），成本最低

S3 ACL：
- Amazon S3 访问控制列表 (ACL) 使您能够管理对存储桶和对象的访问。每个存储桶和对象都有一个作为子资源附加到它的 ACL。您可以使用 ACL 向其他 AWS 账户授予基本读/写权限。当收到针对资源的请求时，Amazon S3 会检查相应的 ACL 以验证请求者是否具有必要的访问权限
- Amazon S3 ACL 不同于网络 ACL。网络访问控制列表 (Network ACL) 是您的 VPC 的可选安全层，它充当防火墙，用于控制进出一个或多个子网的流量
- 使用 Amazon S3 ACL **不收取额外费用**

Ref:
- [Amazon S3 云存储_对象存储_云存储服务-AWS云服务](https://aws.amazon.com/cn/s3/)
- [Amazon S3存储类别_AWS云存储服务-AWS云服务](https://aws.amazon.com/cn/s3/storage-classes/)

### Elastic File System

文件存储 Amazon Elastic File System (Amazon EFS)：
- 是一项**区域性**服务
- 将数据存储在**多个**可用区中
- 添加和删除文件时，Amazon EFS 会**自动扩展和缩减**
- 文件存储非常适合用于大量服务和资源需要**同时访问**相同数据

### RDS

Amazon Relational Database Service (Amazon RDS)：
- 提供经济高效且**可调整大小的容量**
- 自动执行耗时的管理任务，例如**硬件配置、数据库设置、修补和备份。**
- **专注于应用程序**

Amazon Relational Database Service (Amazon RDS)
- Amazon Aurora：将传统企业数据库的性能和可用性与开源数据库的简单性和成本效益相结合
- PostgreSQL
- MySQL
- MariaDB
- Oracle Database
- Microsoft SQL Server

使用 Amazon RDS 属于责任共担模型：
- 客户的责任：
    - 管理数据库设置
    - 构建关系数据库模式
- AWS的责任：
    - 安装数据库软件
    - 执行备份
    - 修补数据库软件

### Amazon DynamoDB

Amazon DynamoDB
- 是一项键值对和文档数据库服务（**非结构化 / 非存储服务**）。
- **无（实例）服务器架构**
- **低延迟**：可以在任意规模实现不超过 10 毫秒的延迟
- **自动扩展容量**

### Amazon Redshift

Amazon Redshift 
- 是一项数据仓库服务，可用于进行大数据分析
- 它让您能够从多个来源收集数据，并帮助您了解数据中的关系和趋势

### DMS

AWS Database Migration Service (AWS DMS) 
- 让您能够迁移关系数据库、非关系数据库和其他类型的数据存储
- 源数据库和目标数据库可以是相同的类型，也可以是不同的类型

### 其他DB

Amazon ElastiCache：
- 提供内存数据库存储服务
- 提高 web 应用程序性能

AWS Storage Gateway：
- **混合**存储服务，可让**本地**应用程序无缝使用 AWS 云存储
- 可以使用该服务进行备份和归档、灾难恢复、云数据处理、存储分层和迁移
- **成本效益大于 S3**
- 为使用的容量付费，并且可以根据需要进行扩展（**无法自动扩展**）

Amazon Neptune：
- 完全托管的图形数据库服务（完全托管并处理耗时的任务，例如预置、修补、备份、恢复、故障检测和修复）
- 可以轻松构建和运行与高度连接的数据集（例如社交网络、推荐引擎和知识图谱）一起使用的应用程序

## 监控和分析

### Amazon CloudWatch

- 实时监控 AWS 基础设施和资源
- 查看指标和图形以监控资源的性能
- 配置自动操作和**警报**来响应指标

### AWS CloudTrail

- 自动检测异常账户活动
- 筛选日志以帮助执行操作分析和故障排除
- 为治理、**合规性**和风险审计提供了很好的资源

### AWS Trusted Advisor

- 成本优化：对可消除且提供成本节省的未使用和空闲的资源的各种检查
- 性能：
    - 通过建议如何利用预置的吞吐量来帮助您提升服务的性能
    - 对服务限制和过度使用的实例的检查
- 安全性：查看权限以及识别要启用的 AWS 安全功能的各种检查
- 容错能力：提高应用程序的可用性和冗余性的各种检查
- 服务配额：
    - 服务配额是可以在 AWS 账户中创建的最大资源数。
    - AWS 实施配额，旨在为所有客户提供高度可用且可靠的服务，并避免您支付意外费用

### 其他

AWS X-Ray：
- 是一种调试服务，可帮助开发人员分析和**调试**生产或开发中的分布式应用程序
- 借助 X-Ray，您可以**了解、跟踪**您的应用程序及其底层服务的执行情况，以便识别和排除性能问题和错误的根本原因

## 定价和支持

### AWS 免费套餐

- 永久免费：
    - AWS Lambda 每月提供 100 万个免费请求和长达 320 万秒的计算时间
    - Amazon DynamoDB 每月提供 25GB 的免费存储空间
- 12 个月免费：
    - 特定数量的 Amazon S3 标准存储
    - 每月 Amazon EC2 计算时间的阈值
    - Amazon CloudFront 数据传出量
- 试用：
    - Amazon Inspector 提供 90 天免费试用
    - Amazon Lightsail（一项让您能够运行虚拟专用服务器的服务）在 30 天内提供 750 小时的免费使用

### AWS 定价概念

定价原理
- 按需付费
- 预留容量，付费更少
- 使用越多，折扣越多

工具 & 服务：
- AWS 定价计算器：AWS 定价计算器只是一种根据您的预期使用情况估算每月 AWS 账单的工具（估算用）
- 账单控制面板（AWS Billing Dashboard）：查看 AWS 账单的详细信息，但无法设置警报
- 整合账单：在组织的账户中共享批量折扣定价、Savings Plans 和预留实例
- AWS 预算：在服务使用量超出您定义的阈值时接收提醒
    - 成本预算：成本超过（或预计超过）他们的预算金额时
    - **使用量预算**：使用量超过（或预计超过）定义的阈值时
    - **预订预算**：设置预订利用率或覆盖目标，在其利用率低于他们定义的阈值时
- AWS Cost Explorer：
    - 根据**过去使用情况**（非预期使用情况）直观查看、了解和管理随时间变化的 AWS 成本和使用情况
    - 提供有关 EC2 **预留实例**利用率和覆盖率报告
    - 不提供**本地历史**支出报告
- AWS Cost and Usage Reports：数据比 AWS Cost Explorer 提供的数据更详细

Ref: [Cloud Financial Management - Overview of Amazon Web Services](https://docs.aws.amazon.com/whitepapers/latest/aws-overview/aws-cost-management.html#aws-cost-usage)

EC2 实例定价取决于许多变量：
- 购买选项（按需、储蓄计划、预留、现货、专用）
- 选定的实例类型
- 选定区域
    - Amazon EC2 实例的价格可能会因预置实例的区域而异。
    - 在同一区域内的**不同可用区**中预置的 Amazon EC2 实例具有相同的价格
- 实例数
- **负载均衡**
- 分配的弹性 IP 地址
    - 私有 IP 不收费。
    - 分配的弹性 IP 数量是可能影响 Amazon EC2 费用的因素。为确保有效使用弹性 IP 地址，如果弹性 IP 地址未与正在运行的实例相关联，或者与已停止的实例相关联，AWS 会按小时收取少量费用。在实例运行时，您无需为与该实例关联的一个弹性 IP 地址付费，但额外的弹性 IP 不是免费的。

减少 EC2 成本：
- 删除所有弹性负载均衡器
- 终止（terminate）所有未使用的 EC2 实例
- 删除他们不需要的附加 **EBS 卷**
- 释放所有未使用的弹性 IP

Ref: [Amazon EC2实例价格_EC2虚拟云服务器托管价格 - AWS云服务](https://aws.amazon.com/cn/ec2/pricing/on-demand/)

不影响 EC2 成本：
- 安全组数：免费使用
- **托管区域数**：不是免费的，但它们与 Amazon EC2 成本无关。托管区域是 Amazon Route 53 成本的因素之一

影响 Lambda 成本：
- 计算时间消耗
- 函数的请求数
- x 存储消耗、卷数：Lambda 不是存储服务。它是一种运行应用程序的计算服务

影响整体成本因素：
- 计算费用（compute charges）
- 数据传出费用（data transfer out charges）

不影响整体成本因素：
- 使用的服务数量（the number of services used）：您使用多少 AWS 服务并不重要。每项 AWS 服务都有自己的定价细节，其中许多是免费使用的
- 数据传输费用（data transfer in charges）：对于大多数服务，AWS 不对“数据传输”收取任何费用
- 配置的 IAM 角色数量（The number of IAM roles provisioned）：IAM 及其所有功能均可免费使用

影响 S3 定价四个因素：
- 存储在 S3 上的数据总量（以 GB 为单位）
- 存储类（S3 Standard、S3 Intelligent-Tiering、S3 Standard-Infrequent Access、S3 One Zone-IA、S3 Glacier 或 S3 Glacier Deep Archive）
- 从 S3 传出 AWS 的数据量（Amount of data transferred out of AWS from S3）
- 对 S3 的请求数（Number of requests to S3）

不影响 S3 定价基于四个因素
- 对任意数量的 S3 存储桶使用默认加密：对 S3 存储桶使用默认加密不会产生额外费用
- 创建和删除 S3 存储桶：创建或删除 S3 存储桶是免费的，但您需要为存储在这些存储桶中的数据付费
- 附加到 S3 存储桶的访问控制列表 (ACL) 的数量：使用 Amazon S3 ACL 不收取额外费用。

影响 EBS 定价两个因素：
- 卷（Volumes）：所有 EBS 卷类型的卷存储按您每月配置的 GB 量收费，直到您释放存储。 
- 快照（Snapshots）：
    - 快照存储基于您的数据在 Amazon S3 中消耗的空间量
    - 由于 Amazon EBS 不保存空块，因此快照大小可能会大大小于您的卷大小
    - 复制 EBS 快照是根据跨区域传输的数据量收费的

降低 EBS 成本：
- 删除未附加的 EBS 卷：当 EC2 实例停止或终止时，附加的 EBS 卷**不会自动删除，并且会继续产生费用**，因为它们仍在运行
- 调整大小或更改 EBS 卷类型：识别未充分利用的卷并缩小它们的大小或更改卷类型
- 删除过时的 Amazon EBS 快照：将其删除以降低存储成本
- x 使用预留：Amazon EBS 中没有独立于 Amazon EC2 的预留（reservations）

影响 CloudCront 成本：
- 请求数
- 流量分布
- x 实例类型：实例类型是影响 Amazon EC2 成本而非 Amazon CloudFront 成本的因素

### AWS Support 计划

基本支持计划
- 免费
- 包括访问白皮书、文档和支持社区
- 可以使用**有限的 AWS Trusted Advisor** 检查功能
- AWS 知识中心（AWS Knowledge Center）：帮助回答 AWS 客户最常提出的问题

开发人员计划（Developer Support）：12小时回应服务
- 成本最低
- 最佳实践指导
- 客户端诊断工具
- 构建块架构支持，其中包括有关如何结合使用 AWS 产品、功能和服务的指导

业务计划（Business Support）：1小时回应服务
- 成本居中
- 使用案例指导，用于确定最适合您的特定需求的 AWS 产品、功能和服务
- **所有 AWS Trusted Advisor 检查**
- 针对第三方软件（例如常见的操作系统和应用程序堆栈组件）的有限支持
- 为技术支持工程师提供 **24x7** 全天候访问

企业支持计划（Enterprise Support）：15分钟回应服务
- 成本最高
- 功能最全：包含基本、开发人员和业务支持计划中所有功能
- 应用程序架构指导：可支持公司的特定使用案例和应用程序的咨询关系
- 基础设施事件管理：通过 AWS Support 展开短期合作 → 了解使用案例、提供架构和扩展指导
- **技术客户经理（TAM）**：
    - TAM 是您在 AWS 的主要联系人
    - 他们可以在您计划、部署和优化应用程序时提供指导和架构审核，并与您的公司保持沟通
- **AWS Support Concierge**：
    - 仅适用于 AWS Enterprise 或 Enterprise On-Ramp 支持计划
    - 专门处理企业账户的 AWS **计费和账户**专家

基于 AWS Trusted Advisor 不同的支持计划：
- 仅提供访问6 项核心安全检查：基本、开发
- 所有115 项 检查：业务、企业

其他：
- AWS Professional Services：帮助客户实现他们期望的业务成果
- Amazon Connect：是一种基于云的联络中心解决方案。Amazon Connect 可让您轻松设置和管理客户联络中心，并提供任何规模的可靠客户互动

Ref:
- [AWS Support 计划比较 \| 开发人员、业务、企业、Enterprise On-Ramp | AWS Support](https://aws.amazon.com/cn/premiumsupport/plans/)

### AWS Marketplace

是一种数字目录，其中包含独立软件供应商提供的上千款软件产品。您可以使用 AWS Marketplace 查找、测试和购买在 AWS 上运行的软件

优势：
- 它通过**灵活的定价选项**和多种部署方法简化了软件许可和采购。灵活的定价选项包括免费试用、每小时、每月、每年、多年和 BYOL。
- 客户只需点击几下即可**快速启动预配置软件**，并选择 AMI 和 SaaS 格式以及其他格式的软件解决方案。
- 它确保**定期扫描产品**以查找已知漏洞、恶意软件、默认密码和其他与安全相关的问题。

不正确：
- 提供在 AWS 或*任何其他云供应商上*运行的软件解决方案：AWS Marketplace 提供**仅在 AWS 上运行的软件**解决方案

### AWS Health Dashboard

- 服务运行状况的个性化视图
- 主动通知
- 详细的故障排除指南

### AWS 支持

- AWS Security team
- AWS Concierge team
- AWS Customer Service team
- AWS Abuse team

## 迁移和创新

### AWS 云采用框架 (AWS CAF)

- 侧重于业务能力：业务、人员和监管视角
    - 业务：确保您的业务策略和目标与您的 IT 策略和目标保持一致
    - 人员：评估组织结构和角色、新技能和流程要求
    - 监管：
        - 使 IT 策略与业务策略保持一致的技能和流程
        - 确定和实施 IT 监管的最佳实践，并利用技术为业务流程提供支持
- 侧重于技术能力：平台、安全和运维视角
    - 平台：
        - 实施新解决方案以及将本地工作负载迁移到云的原则和模式
        - 帮助您根据业务目标和视角来设计、实施和优化 AWS 基础设施
    - 安全：
        - 确保组织满足可见性、可审核性、控制和敏捷性等方面的安全目标
        - 安排控制措施的选择和实施
    - 运维：运营和恢复 IT 工作负载，以满足业务利益相关方的要求

### 迁移策略

- 重新托管
- 更换平台
- 重构/重新设计架构
- 重新购买
- 保留
- 停用

### AWS Snow 系列

- AWS Snowcone：具有 2 个 CPU、4GB 内存和 8TB 的可用存储容量
- AWS Snowball：内置计算能力，允许客户在本地处理数据
    - Snowball Edge Storage Optimized：80TB 硬盘驱动器 (HDD) 容量
    - Snowball Edge Compute Optimized：42TB 的可用 HDD 容量
- AWS Snowmobile：80 - 100PB 的数据

### 其他迁移

- AWS Migration Evaluator：构建云规划和迁移创建定向业务案例
- AWS Migration Hub：提供单一位置来跟踪跨多个 AWS 和合作伙伴解决方案的应用程序迁移进度
- Database Migration Service (AWS DMS)：在最广泛使用的商业和开源数据库之间迁移数据

### 利用 AWS 进行创新

- Amazon Transcribe 将语音转换为文本
- **Amazon Comprehend** 自然语言处理 (NLP) 服务
- Amazon Fraud Detector 识别潜在的欺诈性在线活动
- Amazon Lex 构建语音和文本聊天机器人
- Amazon SageMaker 构建、训练和部署机器学习模型
- **Amazon Rekognition** 基于深度学习的视觉搜索和图像分类检测图像中的对象、场景和面部

## 其他笔记

AWS Service Catalog：
- 创建和集中管理常用部署的 IT 服务
- 帮助现一致的治理、快速部署，以及满足合规性要求

AWS Config：
- 发现现有和已删除的 AWS 资源，并随时深入了解资源的配置详细信息
- 确定您对规则的整体合规性
- 无法用于**监控或设置 CPU 使用阈值**

AWS Cloud9：
- 基于云的集成开发环境 (IDE)，只要浏览器即可编写、运行和调试代码
- Cloud9 预装了适用于流行编程语言（包括 JavaScript、Python、PHP 等）的基本工具

Amazon Chime：
- 用于在线会议的通信服务

Amazon Cognito：
- 让客户可以快速轻松地将用户注册、登录和访问控制添加到他们的 Web 和移动应用程序
- 支持通过 SAML 2.0 使用社交身份提供商（例如 Facebook、Google 和 Amazon）以及企业身份提供商进行登录

Amazon CloudSearch：
- 是 AWS 云中的一项托管服务
- 可让轻松且经济高效地为您的网站或应用程序设置、管理和扩展搜索解决方案

Amazon Athena：
- 交互式查询服务，可以使用标准 SQL 轻松分析 Amazon S3 中的数据

AWS Global Accelerator：
- 是一项网络服务，可提高您向全球用户提供的应用程序的可用性和性能

Amazon Elastic Map Reduce (Amazon EMR)：
- 是一项 Web 服务，使企业、研究人员、数据分析师和开发人员能够轻松且经济高效地处理大量数据
- Amazon EMR 让您可以专注于处理或分析数据，而不必担心 Hadoop 集群的耗时设置、管理或调整或它们所在的计算容量

Amazon Simple Email Service (Amazon SES)：
- 是一种基于云的电子邮件发送服务，旨在帮助数字营销人员（digital marketers）和应用程序开发人员（application developers）发送营销、通知和交易电子邮件

## 考试

可以自动扩展实例（instances auto-scaling）：
- EC2（须先设定Auto Scaling）
- Lambda
- Amazon Aurora

可以自动扩展容量（storage auto-scaling）：
- EFS
- S3
- 除 Amazon Aurora 之外的 RDS
-  DynamoDB（无服务）
    
不能自动扩展：
- EBS
- Storage Gateway

无服务：
- AWS Lambda
- AWS Fargate
- AWS DynamoDB
- AWS Neptune

可以用作计算资源：
- EC2
- Lambda

会自动跨多可用区备份复制数据：
- Amazon Aurora
- S3

免费：
- AWS 公告（AWS Bulletins）
- AWS 安全博客（AWS Security Blog）

---