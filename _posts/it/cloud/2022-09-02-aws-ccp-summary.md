---
layout: post
title: AWS - CCP Summary
category: operations
tags: [aws]
---

## 云计算

范例：
- IaaS: EC2
- Paas: Elastic Beanstalk
- Saas: Apps

雲計算部署模式：
- 云端部署（Cloud）
- 本地部署（On-premises）（私有雲部署）
- 混合部署（**Hybrid**）

云计算的六大优势：支出、速度、规模、灵活

### 设计原则

AWS 架构完善的框架
- 卓越运营（Operational Excellence）：运行和监控系统
- 安全性（Security）：保护信息和系统（eg. AWS CloudTrail）
- 可靠性（Reliability）：恢复中断系统
- 性能效率（Performance）：有效使用计算资源
- 成本优化：避免或消除不需要的成本或次优资源
- 可持续性：解决对环境、经济和社会的长期影响

## 基础设施

### Region, AZ, Edge Location

三个基础设施
- 区域（Region）：数据中心集群，包含 AWS 资源的地理区域
- 可用区（Availability Zones）：数据中心、容错、相同安全
- 边缘站点（Edge Location）：副本、非灾难恢复、主要城市

区域四项业务因素：
- 合规性（Compliance）（**数据主权**）
- 距离（Proximity）
- 可用服务（Available services）
- 定价（Pricing）

AWS CloudFront: 
- 内容分发服务
- 边缘站点缓存内容

### 预置资源

与AWS服务交互的方式：
- AWS CLI: 通过脚本自动执行 AWS 服务和应用程序的操作
- AWS SDK: 采用受支持的编程语言开发 AWS 应用程序

AWS Outposts: 
- 配置管理服务
- 混合云：将云基础设施和服务扩展到您的**本地数据中心**

AWS CloudFormation: 
- 将**基础设施**视为代码，通过编写代码来构建环境

AWS CodeDeploy：
- 代码部署到任何实例
- 非管理基础设施与构建环境

AWS Ekastic Beanstalk: 
- 提供代码和配置设置

Amazon S3 Transfer Acceleration：
- 长距离文件传输：作用于客户端和 S3 存储桶之间
- 原理：利用全球分布式边缘站点路由优化路径

## 安全性

### 责任共担模式

- 客户：网络、加密数据
- AWS：物理、创建管理程序

共享控制：适用于基础设施层和客户层（apply to both the infrastructure layer and customer layers）
- **补丁管理**：AWS 负责修补和修复基础设施内的缺陷，而客户负责修补其来宾操作系统和应用程序
- **配置管理**：AWS 负责维护基础设施设备的配置，而客户负责配置自己的来宾操作系统、数据库和应用程序
- 认知和培训：AWS 负责培训 AWS 员工，而客户必须负责培训自己的员工

### IAM

用户权限和访问 Identity and Access Management
- IAM 用户组和角色
- IAM 策略
- 多重验证（MFA）

与 IAM 进行交互的方法：
- AWS 管理控制台（AWS Management Console）
- AWS 命令​​行工具（AWS Command Line Tools, CLI）
- AWS 开发工具包（AWS Software Development Kits, SDK）

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

### 拒绝服务攻击

AWS Shield：
- 标准：阻挡DDoS
- 高级：阻挡、诊断、检测、缓解DDoS

### 其他安全服务

- 密钥管理（AWS KMS）：创建与管理加密操作的密钥（eg. MFA）
- 访问秘钥：用于签署对**编程请求**（CLI, API）的长期凭证
- 网络应用程序防火墙（AWS WAF）：监控进入 Web 应用程序的网络请求的服务
- 自动化安全性评估（Amazon Inspector）：检查应用程序的安全漏洞以及偏离最佳实践的情况的服务
- 智能威胁检测（Amazon GuardDuty）

## 联网

### 网关和连接

互联网网关：将 VPC 连接到互联网

Amazon Virtual Private Cloud (Amazon VPC)：
- 虚拟网络隔离区，客户可以完全控制

AWS Direct Connect：
- 在数据中心和 VPC 之间建立专用私有连接

AWS Transit Gateway：
- 网络中转中心
- 简化 VPC 之间的连接管理

### 子网和网络访问控制列表

子网：
- 是一个用于将资源归为一组的独立区域
- 是 VPC 的一部分
- 可以是公有子网，也可以是私有子网

网络访问控制列表 (ACL)：
- 用于在**子网**级别控制入站和出站流量
- 默认允许所有入站和出站流量，并检查所有出入白名单与黑名单是否放行
- 无状态数据包筛选

安全组：
- 用于控制 **Amazon EC2 实例**的入站和出站流量
- 默认拒绝所有入站流量，并允许所有出站流量
- 有状态数据包筛选

### 全球联网

Amazon Route 53：
- DNS Web 管理域名服务
- 可以对 Amazon EC2 实例、 Web 服务器和其他资源执行**健康检查**
- 通过多种路由类型管理全球应用程序流量（跨区域）

## EC2

- 快速预置、启动和随时停止
- 按需使用与付费

### 实例类型

- 通用实例（General Purpose）：均衡计算
- 计算优化实例（Compute Optimized）：提供高性能处理器，适合批次处理
- 内存优化实例（Memory Optimized）：非常适合高性能数据库
- 存储优化实例（Storage Optimized）：适用于数据仓库应用程序

### 定价

- 按需实例（On-Demand Instances）：不能中断、短期、没有前期成本
- Savings Plans：承诺稳定使用 1 年或 3 年期
- 预留实例（Reserved）：
    - 购买 1 年或 3 年期、标准预留实例、可转换预留实例
    - 预付越多，折扣越多（All Upfront）
- Spot实例（Spot Instances）：灵活、可承受中断
- 专用主机（Capacity Reservations）：指 EC2 实例容量完全供您专用的物理服务器。

### Auto Scaling

Amazon EC2 Auto Scaling
- 更好的容错性：通过**跨区域内多个可用区**提供可靠性
- 更好的成本管理：只需要在使用实例时为实际使用的实例付费

限制：
- **不能跨越多个区域**
- 不分配流量（这是 ELB 的工作）
- 不执行任何缓存（这是 CloudFront 的工作）

### ELB

Elastic Load Balancing
- 应用程序负载均衡器：Application Load Balancer 最适合 HTTP 和 HTTPS 流量的负载平衡
- 网络负载均衡器：Network Load Balancer 最适合 TCP 和 TLS 流量的负载均衡

### SNS & SQS

- SNS：发布/订阅服务。
- SQS：消息队列服务。

### Lambda

AWS Lambda
- 无服务
- 按需使用（付费）
- 代码触发
- 任何编程

### Container

- Elastic Container Service (ECS)：with Docker
- Elastic Kubernetes Service (EKS)：with Kubernetes
- AWS Fargate：无服务器计算引擎，允许客户在无需管理服务器或集群的情况下运行容器

## 存储和数据库

summary:
- 实例：丢失
- EBS：块级、单区、读写、持久、配置收费
- S3：对象、文件、数量任意大小限制、99耐用、批量折扣
    - 标准、智能、IA、归档
- EFS：区域、多区、扩容
- RDS：自动管理
    - Aurora：扩实
    - 其他：扩容
- DynamoDB：键值、无服务、扩容
- Storage Gateway：混合、无法自动扩展
- Neptune：图形、无服务

其他：
- Redshift：数据仓库、数据分析
- DMS：迁移

使用 Amazon RDS 属于责任共担模型：
- 客户的责任：
    - 管理数据库设置
    - 构建关系数据库模式
- AWS的责任：
    - 安装数据库软件
    - 执行备份
    - 修补数据库软件

## 监控和分析

summary：
- CloudWatch：图形监控、警报
- CloudTrail：调用日志、为治理、**合规性**和风险审计提供了很好的资源
- Trusted Advisor：成本、性能、安全、容错、配额
- AWS X-Ray：调试服务、方便排除错误

## 定价和支持

### AWS 定价概念

工具 & 服务：
- AWS 定价计算器
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

影响整体成本因素：
- 计算费用（compute charges）
- 数据传出费用（data transfer out charges）

不影响整体成本因素：
- 使用的服务数量（the number of services used）：每项 AWS 服务都有自己的定价细节，其中许多是免费使用的
- 数据传输费用（data transfer in charges）：对于大多数服务，AWS 不对“数据传输”收取任何费用
- 配置的 IAM 角色数量（The number of IAM roles provisioned）：IAM 及其所有功能均可免费使用

影响 EC2 成本：
- 类型
- 区域（非可用区）
- 实例数
- ELB 服务、EBS 卷
- 分配的弹性 IP 地址

不影响 EC2 成本：
- 安全组数：免费使用
- **托管区域数**：不是免费的，但它们与 Amazon EC2 成本无关。托管区域是 Amazon Route 53 成本的因素之一

影响 S3 定价四个因素：
- S3 数据总量
- S3 类型
- 从 S3 传出 AWS 的数据量（Amount of data transferred out of AWS from S3）
- 对 S3 的请求数（Number of requests to S3）

不影响 S3 定价基于四个因素
- 默认加密
- 创建和删除存储桶
- 免费使用访问控制列表 (ACL) 

影响 EBS 定价两个因素：
- 卷的配置容量
- 快照

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

### 其他

AWS Professional Services：
- 帮助客户实现他们期望的业务成果

Amazon Connect：
- 是一种基于云的联络中心解决方案
- 可让您轻松设置和管理客户联络中心，并提供任何规模的可靠客户互动

AWS Marketplace：
- 数字目录软件

AWS Health Dashboard
- 服务运行状况的个性化视图
- 主动通知
- 详细的故障排除指南

## 迁移和创新

### AWS 云采用框架 (AWS CAF)

- 业务：确保您的业务策略和目标与您的 IT 策略和目标保持一致
- 人员：评估组织结构和角色、新技能和流程要求
- 监管：确定和实施 IT 监管的最佳实践，并利用技术为业务流程提供支持
- 平台：帮助您根据业务目标和视角来设计、实施和优化 AWS 基础设施
- 安全：确保组织满足可见性、可审核性、控制和敏捷性等方面的安全目标
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
- 发现现有和已删除的 AWS 资源
- 确定您对规则的整体合规性
- 并随时深入了解资源的配置详细信息

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
- DynamoDB（无服务）
    
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

---