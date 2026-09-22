# AMIS1012 — Chapter 6: Risk Assessment and Trustworthy Computing

> **每一份 past year 的 Q2（25 分）都出自这一章**。但 slide 最核心的内容——risk assessment 八步、trustworthy computing 四根柱子、NIST CSF 的 core/profiles/tiers——**全部是图片**，只看文字会以为只有标题。Slide 也没有示范怎样写 "CIA + scenario"（12 分），而且 p17 把 availability 写成 "data is accessible to only authorized users and not to unapproved individuals"，这其实是 **confidentiality** 的定义。这份笔记把每张图按 slide 的版面重画，用一家零售公司贯穿全章，并为五份考卷的 20 多个小题都写好满分答法。

---

## 0. 一句话总览

**公司先做 security risk assessment（8 步：找资产 → 找威胁 → 估机率与冲击 → 找缓解方案 → 评可行性 → cost-benefit → 决定 → 持续 reassess），用 CIA triad（confidentiality, integrity, availability）作为保护目标，用 trustworthy computing 的四根柱子（security, privacy, reliability, business integrity）赢得信任，再用 IT security policies 和国际框架（ISO 27001、NIST CSF 2.0）把这一切标准化。**

| Part | 内容 | Slide |
|---|---|---|
| 1 | General security risk assessment：asset、loss event、**8 steps**、risk tolerance = probability × impact、决策例子 | p4–11 |
| 2 | **Trustworthy computing** 四根柱子 | p12–14 |
| 3 | **CIA triad** | p15–17 |
| 4 | **IT security policy**（6 个例子） | p18–21 |
| 5 | **ISO 27001**（ISMS + 14 categories） | p22–24 |
| 6 | **NIST CSF 2.0**（core 6 functions、organizational profiles、tiers） | p25–30 |

🎯 **这章回答的考题（Q2 每年必考）**

| 题 | Oct 2024 (TAR) | Jan 2025 (Cybertron) | May 2025 | Oct 2025 (PQR) | May 2026 (e-commerce) |
|---|---|---|---|---|---|
| Why risk assessment / apply the process | 2a (3) | — | — | 2a (3) | — |
| First two steps | — | — | — | — | 2a (6) |
| **EIGHT steps** | 2b (8) | — | — | 2b (8) | — |
| Main asset | 2c (2) | — | — | 2c (2) | — |
| **CIA** + scenario | 2d (12) | 2c C & A (8) | — | 2d (12) | 2b THREE ways (9) |
| **Trustworthy computing** | — | 2d (4) | 2b (8) | — | 2c TWO pillars (6) |
| IT security policy | — | 2a purpose (1), 2b FOUR (12) | 2c THREE (12) | — | — |
| ISO 27001 / NIST | — | — | 2a ISO (5) | — | 2d ISO + NIST (4) |

（Tutorial 6 的 6 题也全在这章。）

**Prerequisites**：Ch2 的攻击（DDoS、insider theft、virus）= 这章的 "loss events"；Ch3 的 cyber security、PDPA。

---

## Scene：TAR Sdn. Bhd. 的新收银系统

TAR Sdn. Bhd. 卖厨房用品、收纳盒、杯子。它刚把 **point-of-sale (POS)** 系统升级成**云端方案**。老板对 IT team 说："做一次 **security risk assessment**，特别要确保**顾客的付款资料**安全。"

IT team 面对的问题：
- 我们有哪些东西要保护？哪个最重要？
- 会出什么事？多常发生？发生了有多严重？
- 钱有限，先处理哪个？花多少钱才值得？

这就是 risk assessment。本章用 TAR（也等同 PQR Sdn. Bhd. 那一题，情节一样）贯穿到底。

---

## 1. General Security Risk Assessment（p4–11）⭐

### 1.1 定义

- **Risk assessment** = a method for determining the **security risks** that an organisation's systems and networks face from **both internal and external threats**。
- **Risk management 的目的**：决定**哪些时间和资金投入**最能保护公司免受**最可能、最严重**的威胁。
- **Asset** = any **hardware, software, computer system, network, or database** used by the enterprise to accomplish its business goals。
- **Loss event（slide 叫 failure case）** = any event that has a **detrimental effect** on an asset（例：设备中毒、网站被 DDoS）。

### ✅ 满分答法 — Oct 2024 Q2a: Examine why security risk assessment is important for TAR Sdn. Bhd. (3 marks)
1. It identifies the security risks that TAR's systems and networks face from both internal and external threats — especially to customer payment data in the new cloud-based POS system.
2. It helps TAR prioritise, so that limited time and money are spent on protecting against the most likely and most damaging threats (e.g. a payment-data breach) rather than minor ones.
3. It supports decisions on cost-effective controls, protecting customer trust and helping TAR comply with legal and payment-industry requirements, avoiding fines and reputational damage.

### ✅ 满分答法 — Oct 2025 Q2a: Explain how the IT team at PQR would apply the process of a security risk assessment to achieve this goal (securing customer payment data). (3 marks)
The IT team would first identify the cloud POS system and the customer payment database as the key assets, then identify threats to them (e.g. hacking, insider theft, DDoS on the POS) and estimate how likely and how severe each is. Based on this, they would choose, evaluate and cost suitable controls (e.g. encryption, MFA, access control) and decide which to implement, reassessing regularly as the system changes.

### 1.2 八个步骤（p6–10 的图）

Slide 用一张"阶梯"图，每一步向下一层，所有步骤都流进底部的 **Reassessment**：

```
 [1 Identify Assets]──┐
 ^                    v
 │      [2 Specify Loss Events]──┐
 │                               v
 │          [3 Frequency of Events]──┐
 │                                   v
 │ Anticipated    [4 Impact of Events]──┐
 │ or actual                            v
 │ change          [5 Options to Mitigate]──┐
 │                                          v
 │                  [6 Feasibility of Options]──┐
 │                                              v
 │                     [7 Cost/Benefit Analysis]──> < 8 DECISION >
 │                                                         │
 │     (steps 1–7 also feed down into Reassessment)        │
 └──────────────────── REASSESSMENT <──────────────────────┘
```
*看图重点：这不是一次性的直线——**任何预期或实际的改变**（新系统、新威胁）都会触发 reassessment，回到 step 1。*

| Step | Slide 的说法 | TAR Sdn. Bhd. 例子 |
|---|---|---|
| **1. Identify assets** | 找出公司**最担心的 IT 资产**；优先保护支持组织**议程与关键业务目标**的资产 | Cloud POS system、**customer payment database**、网店、员工电脑、网络 |
| **2. Specify loss events / threats** | 找出可能的**损失事件、风险与威胁**，如 **DDoS** 或 **insider theft** | 骇客偷取信用卡资料；员工盗取资料；POS 被 DDoS；云端服务中断；ransomware |
| **3. Frequency of events** | 估计每种威胁的**次数或机率**；某些（如 insider theft）比其他更可能发生 | Phishing 尝试：几乎每周；insider theft：中；云端大规模中断：低 |
| **4. Impact of events** | 判断每种危害的**严重性**：小影响，还是让公司**长时间**无法运作？ | 付款资料外泄：**critical**（罚款、诉讼、失去信任）；网店慢 1 小时：低 |
| **5. Options to mitigate** | 决定能否**降低发生机率**或**降低冲击**（如全面装 antivirus 让中毒机率大降）。因时间预算有限，多数公司先处理**高频率、高冲击**的风险 | Encryption、tokenisation、MFA、DDoS protection、员工 access control、备份 |
| **6. Feasibility of options** | 评估**实施缓解方案的可行性** | TAR 的 IT 人手与技术能否支援？云端供应商是否提供？ |
| **7. Cost/benefit analysis** | 做 **cost–benefit study**；没有任何金额能保证完美，要权衡**安全漏洞的可能性 vs 防范成本**；**reasonable assurance**：控制成本**不应超过**系统的收益或威胁造成的损失 | MFA + 加密每年 RM20k vs 一次资料外泄可能损失 RM500k → 值得 |
| **8. Decision** | 决定**是否实施**某一对策；若决定不实施，想想威胁是否真的严重，若严重就选**较便宜的方案** | 实施加密与 MFA；DDoS 高阶防护太贵 → 选云端内建的基本防护 |
| **(Reassessment)** | 任何预期或实际的变化 → 重新评估 | 加入新的电子钱包付款方式时重新做一次 |

> 💬 **答题句 (EN)** — *The eight steps of a general security risk assessment are: identify the IT assets of most concern; identify potential loss events, risks and threats; assess the frequency (probability) of each event; determine the impact of each event; determine how each threat can be mitigated; assess the feasibility of implementing the mitigation options; perform a cost–benefit analysis; and decide whether to implement each countermeasure — with reassessment whenever there is an anticipated or actual change.*

### ✅ 满分答法 — Oct 2024 Q2b / Oct 2025 Q2b: Outline the EIGHT steps in a general security risk evaluation process conducted by the IT team. (8 marks)

1 分一步，每步套用 TAR/PQR：

1. **Identify assets** – identify the IT assets the company is most concerned about, prioritising those that support key business objectives, e.g. the cloud-based POS system and the customer payment database.
2. **Specify loss events (threats)** – identify potential loss events and threats to these assets, such as hacking of payment data, insider theft or a DDoS attack on the POS.
3. **Assess the frequency of events** – estimate the probability of each threat occurring, e.g. phishing attempts on staff are frequent while a cloud provider outage is rare.
4. **Determine the impact of events** – determine how severe each event would be, e.g. a payment-data breach would be critical (fines, lawsuits, loss of trust) whereas a brief website slowdown is minor.
5. **Identify options to mitigate** – decide how each threat can be made less likely or less damaging, e.g. encrypting card data, enabling MFA, installing antivirus.
6. **Assess the feasibility of options** – evaluate whether each mitigation option can practically be implemented with the company's staff, technology and time.
7. **Perform a cost–benefit analysis** – compare the cost of each control with the expected loss it prevents, ensuring the cost of control does not exceed the benefit (reasonable assurance).
8. **Make a decision** – decide whether to implement each countermeasure; if a serious threat's control is too costly, choose a less costly alternative. The whole process is reassessed whenever the system or threats change.

### ✅ 满分答法 — May 2026 Q2: Describe the first two steps of the security risk assessment process for the (e-commerce) company. (6 marks)
3 分一步（what + why + scenario example）：
1. **Identify assets** – the IT team identifies and lists the IT assets the company is most concerned about, and prioritises those that support its key business objectives. For the e-commerce company, the most important assets are the customer records database, transaction data, the e-commerce website and the internal business systems, because a failure or breach of these would directly stop sales and harm customers.
2. **Specify loss events (identify threats)** – the team identifies the potential loss events, risks and threats that could harm each asset, both external and internal. For example, hackers stealing customer and transaction data, a DDoS attack making the website unavailable during a sale, ransomware encrypting internal systems, or an insider stealing customer records.

### ✅ 满分答法 — Oct 2024 Q2c / Oct 2025 Q2c: Identify the main asset that appears important to the company. (2 marks)
The **customer payment data** (1) stored and processed by the new **cloud-based POS system** (1) — the owner specifically emphasised its security, and a breach would cause financial loss, legal penalties and loss of customer trust.

### 1.3 Risk Tolerance Level = Probability × Impact（p7, p11）

Slide 把 step 3（frequency）和 step 4（impact）合起来：

> **Risk Tolerance Level = Probability × Impact**
> （课本一般叫 *risk level / risk score*；这里照讲师的名称。）

用 1–5 分数举例（*extra*，方便理解与计算）：

| Threat | Probability (1–5) | Impact (1–5) | Risk = P × I | 优先度 |
|---|---|---|---|---|
| Payment-data breach by hackers | 3 | 5 | **15** | 最高 |
| Phishing on staff | 5 | 3 | **15** | 最高 |
| Insider theft | 3 | 4 | **12** | 高 |
| DDoS on POS | 2 | 4 | **8** | 中 |
| Public product-catalogue page defaced | 4 | 1 | **4** | 低 → accept |

**Slide 的两个决策例子（p11）**：

| Scenario | Probability | Impact | Decision |
|---|---|---|---|
| 1. 存放**公开资讯**的资料库，控制较少 | **High** | **Non-critical**（资料本来就公开） | **Accept the risk**，维持现有控制 |
| 2. 存放**客户敏感资料**的资料库，控制非常严密 | **Low**（控制严密） | **Critical**（名誉、诉讼、巨额损失） | **Prioritise the risk**：采取所有必要行动避免发生 |

*看表重点：机率低不代表可以不理——**impact critical 的就要优先**。*

### ✅ 满分答法 — Tutorial 6 Q2
- **(a) TWO IT assets:** the customer database (containing personal and payment data) and the company's servers/network infrastructure (or the e-commerce web application).
- **(b) TWO pieces of information to establish risk tolerance level:** (1) the **probability** (frequency) that a threat will occur, e.g. how often phishing attempts happen; (2) the **impact** (severity) of the event on the business, e.g. financial loss, legal penalties and reputational damage. Risk tolerance level = probability × impact.
- **(c) Scenario:** a database containing only publicly available product information has a high probability of breach but non-critical impact, so the company **accepts the risk** with current controls; whereas a database of customers' sensitive data has low probability but critical impact, so the company **prioritises the risk** and takes all necessary actions to prevent it.

---

## 2. Trustworthy Computing（p12–14）⭐

- **Trustworthy computing** = a computing approach that provides **stable, confidential and consistent computing interactions** based on **sound business practices**。
- 是电脑业（软硬件设计者、承包商、程式员）客户的**首要要求**；例：**Microsoft** 推出 trustworthy computing 计划，提高人们对其产品的信心。

**四根柱子（p13–14，神殿图）**：

```
                  _______________________________
                 /                               \
                /      TRUSTWORTHY COMPUTING      \
               /___________________________________\
               |___________________________________|
                 |      |   |      |   |      |   |      |
                 |Secu- |   |Pri-  |   |Relia-|   |Busi- |
                 |rity  |   |vacy  |   |bility|   |ness  |
                 |      |   |      |   |      |   |Integ-|
                 |      |   |      |   |      |   |rity  |
                 |______|   |______|   |______|   |______|
```
*看图重点：屋顶（trust）要四根柱子一起撑；少了任何一根，客户就不会信任。*

| Pillar | Slide 定义（p14） | TAR 例子 |
|---|---|---|
| **Security** | 系统能**抵抗攻击**，系统与资料的 **confidentiality, integrity, availability** 受保护 | POS 用加密与 firewall，防骇客窃取付款资料 |
| **Privacy** | 人们能**控制自己的个人资讯**，使用资讯的组织**忠实保护**它 | 只收集必要的顾客资料，说明用途，不卖给第三方，遵守 PDPA |
| **Reliability** | 系统**可靠**，**需要时可用**，**按预期**并在适当水平运作 | 云端 POS 在节庆促销高峰也不当机，有备份与 failover |
| **Business integrity** | 公司对客户**负责**，帮客户找合适的解决方案、处理产品/服务的问题，并**开放地与客户互动** | 出现资料外泄时如实通知顾客、处理投诉、补救 |

> 💬 **答题句 (EN)** — *Trustworthy computing rests on four pillars: security (the system is resilient to attack and the confidentiality, integrity and availability of the system and its data are protected), privacy (individuals can control their personal information and organisations protect it faithfully), reliability (the system is dependable, available when needed and performs as expected), and business integrity (the company acts responsibly towards customers, helps solve their problems and is open in its dealings).*

### ✅ 满分答法 — May 2025 Q2b / Tutorial 6 Q3: Briefly explain the FOUR pillars of trustworthy computing. (8 marks)
2 分一柱（定义 + 例子）：
1. **Security** – the system is resilient to attack and protects the confidentiality, integrity and availability of the system and its data. *e.g.* an online bank uses encryption, firewalls and intrusion detection.
2. **Privacy** – individuals can control their personal information, and organisations that use it protect it faithfully. *e.g.* an app asks for consent before collecting location data and does not sell it.
3. **Reliability** – the system is dependable, available when needed and performs as expected at appropriate levels. *e.g.* a payment system continues operating during peak shopping periods with backup servers.
4. **Business integrity** – the company is responsible to its customers, helps them find appropriate solutions, addresses problems with its products or services, and is open in its interactions with customers. *e.g.* a software vendor promptly discloses a vulnerability and releases a patch.

### ✅ 满分答法 — Jan 2025 Q2d: Discuss how the FOUR pillars can be applied at Cybertron (cloud storage provider) to enhance IT security. (4 marks)
1 分一柱，每句都连到 Cybertron：
- **Security** – fix the weak access controls: enforce MFA, role-based access and encryption of stored customer files.
- **Privacy** – ensure customers control their stored data; staff cannot view customer files, and data is not shared without consent.
- **Reliability** – keep the cloud service available and dependable through redundant servers, backups and monitoring.
- **Business integrity** – be transparent with customers about the vulnerabilities found, notify affected users, and respond openly to their concerns.

### ✅ 满分答法 — May 2026 Q2c: Explain TWO pillars of trustworthy computing and their relevance to the (e-commerce) company. (6 marks)
3 分一柱（定义 + 为什么与公司相关 + 做法）：
1. **Security** – the system must resist attacks and protect the confidentiality, integrity and availability of systems and data. It is relevant because the company stores customer records and transaction data that attract cyber criminals; it should apply encryption, firewalls, MFA and regular patching so that attacks do not succeed.
2. **Privacy** – customers must be able to control their personal information and the company must protect it faithfully. It is relevant because an e-commerce company collects names, addresses and payment details; it should collect only what is needed, obtain consent, restrict staff access and comply with the PDPA, which builds customer trust and repeat business.
（也可选 reliability：网站在促销高峰要可用；business integrity：出事时透明处理。）

---

## 3. CIA Triad（p15–17）⭐⭐ 最高分的一题

| Principle | 定义 | Slide 的网购例子 |
|---|---|---|
| **Confidentiality** | **只有获授权的人**能存取敏感资讯 | 网购用的**信用卡号**必须保密，未授权者无法取得；工具从**加密卡号的软件**到**锁住 server 机房的门锁** |
| **Integrity** | 资讯**正确**，**没有**被未授权的人或恶意软件**修改** | 攻击者把购买金额从 **$10,000.00 改成 $1.00** 就违反了 integrity |
| **Availability** | 授权用户在需要时**能够持续存取**资产 | 网购订单的数量必须让**仓库员工看得到**，才能出正确的货 |

⚠️ **Slide p17 的错误**：availability 的描述写 "Availability ensures that data is accessible to only authorized users and not to unapproved individuals"——后半句（"not to unapproved individuals"）是 **confidentiality** 的意思。Availability 的重点是**授权用户在需要时能用**（uptime、不被 DoS、有备份）。考试**不要**这样写 availability。

**每个原则常用的控制措施（写 scenario 时用）**：

| Principle | 威胁 | 控制措施 |
|---|---|---|
| Confidentiality | 窃听、资料外泄、未授权存取、insider | **Encryption**（at rest & in transit）、**access control / least privilege**、**MFA**、tokenisation、physical locks |
| Integrity | 篡改、message alteration、malware、人为错误 | **Hashing / checksums**、**digital signatures**、input validation、audit logs、version control、role-based edit rights |
| Availability | **DoS/DDoS**、硬件故障、停电、ransomware、灾难 | **Redundancy / failover**、**backups & disaster recovery**、DDoS protection、load balancing、UPS、patching |

> 💬 **答题句 (EN)** — *The CIA triad consists of confidentiality (only authorised parties can access sensitive information), integrity (information is accurate and has not been altered by unauthorised persons or malicious software), and availability (authorised users have reliable, timely access to information and systems when needed).*

### ✅ 满分答法 — Oct 2024 Q2d / Oct 2025 Q2d: Discuss the THREE CIA triad principles and provide a scenario for each to demonstrate how it should be applied in TAR / PQR Sdn. Bhd. (12 marks)

4 分一个原则：**定义 (1) + 场景中的威胁 (1) + 应用的控制 (1–2)**。

1. **Confidentiality** – ensures that only authorised people can access sensitive information.
   *Scenario:* customers' card numbers pass through the new cloud POS and are stored in the cloud. If they are intercepted or accessed by unauthorised staff, customers could suffer fraud. TAR should encrypt card data in transit (TLS) and at rest, tokenise card numbers so cashiers never see the full number, and use role-based access with MFA so that only authorised finance staff can view payment records.
2. **Integrity** – ensures that information is accurate and has not been altered by unauthorised persons or malicious software.
   *Scenario:* an attacker or dishonest employee could change a transaction amount (e.g. from RM1,000 to RM1) or alter refund records in the POS. TAR should use input validation, hashing/checksums to detect changes, audit logs that record who modified each transaction, and restrict edit rights so that only supervisors can approve price overrides and refunds.
3. **Availability** – ensures that authorised users can access systems and data whenever needed.
   *Scenario:* during a year-end sale, a DDoS attack or cloud outage could stop the POS from processing payments, so cashiers cannot serve customers and sales are lost. TAR should choose a cloud provider with a high-uptime SLA, enable DDoS protection and load balancing, keep regular backups with a disaster-recovery plan, and provide an offline fallback mode that syncs transactions when the connection returns.

### ✅ 满分答法 — Jan 2025 Q2c: Assess the Confidentiality and Availability principles that can be adopted at Cybertron (cloud storage provider), with an example each. (8 marks)
4 分一个：
1. **Confidentiality** – only authorised parties may view information. Cybertron's attackers exploited **weak access controls**, so customers' stored files are at risk of exposure. Cybertron should enforce strong authentication (MFA), least-privilege role-based access for employees, and encrypt customer files at rest with keys that staff cannot misuse. *Example:* a support engineer can reset a customer's account but cannot open the customer's stored files.
2. **Availability** – authorised users must have reliable access to their data when needed. As a cloud storage provider, if Cybertron's service goes down, customers cannot reach their files and may leave. Cybertron should use redundant data centres with automatic failover, regular backups, DDoS protection and continuous monitoring. *Example:* if one data centre fails, customer requests are automatically redirected to a replica in another region.

### ✅ 满分答法 — May 2026 Q2b: Analyse THREE ways the CIA triad can be applied to safeguard the company's information systems. (9 marks)
3 分一个：用上面 TAR 答案的结构，把场景换成 e-commerce（customer records、transaction data、internal business systems）：
1. **Confidentiality** – encrypt customer records and transaction data, apply role-based access and MFA so only authorised staff access customer information; this prevents data theft and PDPA breaches.
2. **Integrity** – protect transaction records from unauthorised modification through audit trails, hashing and restricted update rights, and validate inputs to the website to prevent injection attacks; this ensures orders and payments are accurate.
3. **Availability** – keep the e-commerce site and internal systems running through redundant servers, backups, disaster-recovery plans and DDoS protection; this prevents lost sales and customer frustration during outages or attacks.

### ✅ 满分答法 — Tutorial 6 Q4: Explain the CIA triad with ONE example each in an e-banking system.
- **Confidentiality:** a customer's account balance and transaction history are visible only after login with password + OTP, and data is encrypted between the app and the bank.
- **Integrity:** a transfer of RM500 cannot be altered to RM5,000 in transit; the bank uses digital signatures/hashing and logs every transaction.
- **Availability:** the e-banking service is available 24/7 through redundant data centres and DDoS protection, so customers can pay bills when needed.

---

## 4. IT Security Policy（p18–21）

- **IT Security Policy** = a **written document** that states **how an organisation plans to protect its IT assets**。
- **Purpose（Jan 2025 Q2a，1 分）**：*to state, in writing, how the organisation will protect its information technology assets, giving employees clear rules and direction.*

**六个例子（非完整清单）**：

| Policy | 说明 | 例子（Cybertron / 一般公司） |
|---|---|---|
| **Acceptable Use Policy (AUP)** | 给网络与系统用户**清楚指示**：资讯资源**可以怎样用** | 禁止共用帐号、禁止安装未授权软件、禁止用公司网络做私人生意 |
| **Information Security Policy** | 为整个 security program 提供**高层授权与指导** | 管理层批准：所有客户资料必须分级并加密 |
| **Incident Response Policy** | 描述组织**如何回应**安全事件 | 发现入侵后 1 小时内隔离系统、通知 CISO、72 小时内通报当局与客户 |
| **Business Continuity & Disaster Recovery (BC/DR) Policy** | 确保灾难**期间与之后**关键业务**持续运作**、资料与资产**被恢复与保护** | 每日异地备份；主数据中心失效时 4 小时内切换到备援站 |
| **Software Development Life Cycle (SDLC) Policy** | 建立维护软件的流程与标准，确保**每个开发阶段都考虑并整合安全** | 上线前必须 code review 与 security testing |
| **Change Management & Change Control Policy** | 描述如何**审查、批准、实施**资讯系统的变更，同时管理网络安全与营运风险 | 任何 firewall 规则变更须经 change advisory board 批准并记录 |

> 💬 **答题句 (EN)** — *An IT security policy is a written document that states how an organisation plans to protect its information technology assets.*

### ✅ 满分答法 — Jan 2025 Q2b: Provide FOUR examples of IT security policy that Cybertron can implement and explain the importance of each. (12 marks)
3 分一个（名称 + 内容 + 对 Cybertron 的重要性，连到 "weak access controls and unmonitored employee practices"）：
1. **Acceptable Use Policy (AUP)** – gives employees clear direction on the permissible use of information resources (e.g. no password sharing, no personal use of admin accounts, no unauthorised software). It is important because Cybertron's attackers exploited **unmonitored employee practices**; clear rules, acknowledged by staff and backed by monitoring, reduce risky behaviour and provide grounds for disciplinary action.
2. **Information Security Policy** – provides high-level authority and guidance for the whole security program, including access-control rules such as least privilege and MFA. It is important because it fixes the **weak access controls** at the root, with management backing, so that every system follows the same security standard.
3. **Incident Response Policy** – describes how Cybertron will detect, contain, investigate and recover from security incidents and whom to notify. It is important because a cloud provider holds many customers' data; a fast, planned response limits damage and ensures customers and regulators are informed on time.
4. **Business Continuity and Disaster Recovery Policy** – sets procedures to keep essential services running during and after a disaster and to recover data. It is important because customers depend on Cybertron's availability; backups and failover ensure that an attack or outage does not permanently lose customer data.
（也可写 Change Management Policy：防止未经审查的设定变更造成新的漏洞；SDLC Policy：确保新功能开发时就考虑安全。）

### ✅ 满分答法 — May 2025 Q2c: Discuss any THREE IT security policies adopted by companies, with an example of each to demonstrate its application. (12 marks)
4 分一个（定义 2 + 应用例子 2）：
1. **Acceptable Use Policy** – provides users with clear direction on how the company's network, systems and information may be used. *Application:* a bank's AUP states that staff may not install software, share passwords or access social media on work terminals; staff sign it on joining, and violations are disciplined.
2. **Incident Response Policy** – describes how the organisation will respond to security incidents, including roles, reporting and recovery steps. *Application:* when an online retailer detects ransomware, the policy requires the IT team to isolate infected machines, inform the incident response team and management, restore from backup, and notify affected customers and authorities.
3. **Change Management and Change Control Policy** – describes how proposed changes to information systems are reviewed, approved and implemented while managing security and operational risk. *Application:* before an e-commerce company updates its payment gateway, the change is documented, tested in a staging environment, approved by a change board and scheduled with a rollback plan.

### ✅ 满分答法 — Tutorial 6 Q5（CISO 角色）：两个 policy — 用 AUP + Incident Response（或 BC/DR），各自说"内容 + 为什么需要"。

---

## 5. ISO 27001（p22–24）

- **ISO（International Organization for Standardization）**：国际**非政府**组织，由各国标准机构组成；制定了大量 cybersecurity 标准。
- **ISO 27001** = a standard that provides **requirements for an Information Security Management System (ISMS)**。
- **ISMS** = a **systematic approach to managing sensitive assets** so that they remain secure；这些 assets 包括用来管理风险的 **people, processes, and IT systems**。
- *Extra*：组织可以通过外部审核取得 **ISO 27001 certification**，向客户证明其资讯安全管理达到国际标准。

**14 categories of control objectives（p23–24）**：

| # | Category | # | Category |
|---|---|---|---|
| 1 | Information security policies | 8 | Operations security |
| 2 | Organisation of information security | 9 | Communications security |
| 3 | Human resource security | 10 | System acquisition, development & maintenance |
| 4 | Asset management | 11 | Supplier relationships |
| 5 | **Access control** | 12 | Information security incident management |
| 6 | **Cryptography** | 13 | Information security aspects of business continuity management |
| 7 | Physical & environmental security | 14 | Compliance (internal policies & external laws) |

*记忆法（按顺序）*：**P**olicy → **O**rganisation → **H**uman → **A**sset → **A**ccess → **C**rypto → **P**hysical → **O**perations → **C**ommunications → **S**ystem dev → **S**upplier → **I**ncident → **B**usiness continuity → **C**ompliance。

⚠️ *Extra*：这 14 类是 **ISO/IEC 27001:2013 Annex A** 的结构；**2022 版**改为 4 个 themes（Organisational、People、Physical、Technological）共 93 controls。考试照 slide 的 14 类写。

### ✅ 满分答法 — May 2025 Q2a: Briefly explain ISO 27001 and list any THREE categories. (5 marks)
**ISO 27001** is an international standard published by the International Organization for Standardization that specifies the requirements for an **Information Security Management System (ISMS)** — a systematic approach to managing an organisation's sensitive assets (people, processes and IT systems) so that they remain secure and risks are managed. Organisations can be certified against it to demonstrate good security management. (2)
Three categories (1 each): **Access control**; **Cryptography**; **Information security incident management** (or any three of the 14).

---

## 6. NIST Cybersecurity Framework（p25–30）

- **NIST（National Institute of Standards and Technology）**：隶属**美国商务部**，使命是透过推进测量科学、标准与技术来促进美国创新与产业竞争力。
- **NIST CSF** = 一套指引，帮助**私人企业识别、侦测、回应**网络攻击，也包括**预防与复原**的指引。最新版本 **NIST CSF 2.0**。
- CSF 分为**三个基本部分**：

### 6.1 CSF Core（p26）

```
  Cybersecurity Framework Core
  ┌───────────┐     ┌────────────┐     ┌───────────────┐
  │ Functions │ ──> │ Categories │ ──> │ Subcategories │
  ├───────────┤     └────────────┘     └───────────────┘
  │ Govern    │
  │ Identify  │     (each function splits into categories,
  │ Protect   │      each category into subcategories =
  │ Detect    │      specific outcomes)
  │ Respond   │
  │ Recover   │
  └───────────┘
```
*看图重点：Core 定义的是"要达成什么结果"——6 个 function → categories → subcategories，由粗到细。*

**六个 Functions**（slide 叫你看 NIST 文件 p3–4；以下为整理）：

| Function | 意思 | 电商公司例子 |
|---|---|---|
| **Govern (GV)** | 建立、沟通、监督组织的**网络安全风险管理策略、期望与政策**（2.0 新增，居中） | 董事会批准安全策略、指定 CISO、明确角色与责任 |
| **Identify (ID)** | 了解目前的网络安全风险：**资产**、供应商、风险评估、改进机会 | 列出所有 server、资料库、云端服务，做 risk assessment |
| **Protect (PR)** | 使用**防护措施**管理风险：身份与存取管理、**awareness training**、资料安全、平台安全 | MFA、加密、员工培训、patching |
| **Detect (DE)** | **发现并分析**可能的攻击与入侵 | SIEM 监控、入侵侦测系统、异常登入警报 |
| **Respond (RS)** | 对侦测到的事件**采取行动**：管理、分析、沟通、缓解 | 启动 incident response、隔离受感染机器、通知客户 |
| **Recover (RC)** | **恢复**受事件影响的资产与运作 | 从备份还原、检讨改进、对外沟通复原进度 |

### 6.2 CSF Organizational Profiles（p27–29）

- 描述组织**目前的网络安全状况**与**迈向 CSF 的路线图**；像是组织为 NIST CSF 所做一切的**executive summary**；让组织看到每一步的弱点，缓解后可升到更高的 tier。
- 每个 profile 包含一个或两个：
  - **Current Profile**：组织**目前达成（或尝试达成）**的 Core outcomes 及程度。
  - **Target Profile**：组织**想要达成并排定优先**的 outcomes；考虑预期变化（新要求、新技术、威胁趋势）。

**建立与使用 Profile 的五步（p29 图）**：
1. **Scope** the Organizational Profile
2. **Gather** needed information
3. **Create** the Organizational Profile
4. **Analyse gaps** and create an action plan（Current vs Target 的差距）
5. **Implement** action plan and **update** Profile → *Repeat…*

### 6.3 CSF Tiers（p30）

```
                                      ┌────────┐
                             ┌────────┤ Tier 4 │
                    ┌────────┤ Tier 3 │Adaptive│
           ┌────────┤ Tier 2 │Repeat- │        │
  ┌────────┤ Tier 1 │ Risk-  │ able   │        │
  │ Tier 1 │Partial │Informed│        │        │
  └────────┴────────┴────────┴────────┴────────┘
     weaker  ─────────────────────────>  stronger
```
*看图重点：阶梯越高 = 网络安全风险管理越成熟、越"compliant"。*

| Tier | 意思（*extra* 简述） |
|---|---|
| **1 Partial** | 临时、被动 (ad hoc)，风险意识有限 |
| **2 Risk Informed** | 管理层有批准风险做法，但尚未成为全组织政策 |
| **3 Repeatable** | 有正式批准、全组织执行、定期更新的政策 |
| **4 Adaptive** | 根据经验与预测指标**持续调整**、即时应对新威胁 |

### ✅ 满分答法 — May 2026 Q2d: Explain how the company can use ISO 27001 and the NIST CSF to improve its cybersecurity practices. (4 marks)
- **ISO 27001 (2):** the company can build an Information Security Management System following ISO 27001 — assessing risks and applying controls across categories such as access control, cryptography, supplier relationships and incident management — and seek certification to show customers and partners that its security is managed to an international standard.
- **NIST CSF (2):** the company can use the six CSF Core functions (Govern, Identify, Protect, Detect, Respond, Recover) to organise its security activities, create a Current and a Target Profile to find gaps and build an action plan, and use the Tiers (Partial → Adaptive) to measure and improve its maturity over time.

### ✅ 满分答法 — Tutorial 6 Q6: List and briefly explain TWO industry-standard security frameworks.
1. **ISO 27001** – an international standard specifying the requirements for an Information Security Management System (ISMS), with control objectives in 14 categories (e.g. access control, cryptography, incident management); organisations can be certified.
2. **NIST Cybersecurity Framework (CSF 2.0)** – guidelines from the US National Institute of Standards and Technology to help organisations identify, protect against, detect, respond to and recover from cyber attacks, structured as the CSF Core (six functions), Organizational Profiles and Tiers.

### Tutorial 6 Q1: List the general steps to conduct a security risk assessment. → §1.2 的八步。

---

## Closing the loop

TAR 的 IT team 现在有了一套完整做法：用**八步**找到最重要的资产（顾客付款资料）和最大的风险；用 **CIA** 决定要保护什么（保密、正确、可用）；用四根 **trustworthy computing** 柱子赢得顾客信任；用 **IT security policies** 规范员工；再用 **ISO 27001 / NIST CSF** 让整套做法有国际标准可循。

但很多漏洞在系统**被写出来的时候**就已经存在了。下一章 **Software Development** 讲：怎样用对的开发方法、测试和品质保证，在出货前就把缺陷找出来。

---

## ⚠️ Where the slides mislead

| Slide | Slide 说 | 更准确的理解 |
|---|---|---|
| p17 | Availability "ensures that data is accessible to **only** authorized users and **not to unapproved individuals**" | 这是 **confidentiality**；availability 是授权用户**在需要时能存取** |
| p5 | "A failure case is any event…" | 图上与课本叫 **loss event**；两个名称都可 |
| p7 | **Risk Tolerance Level** = Probability × Impact | 课本通常叫 **risk level / risk score**；risk tolerance 通常指"公司愿意接受多少风险"。考试照 slide 公式写 |
| p23 | ISO 27001 有 14 categories | 是 **27001:2013** 的 Annex A；2022 版改为 4 themes / 93 controls |
| p6–10, 13–14, 26–30 | 关键内容全是图片 | 本笔记已把每张图重画并解释 |

考试策略：若题目照 slide 字眼问，照 slide 答；availability 那一句**不要照抄**。

---

## Term table

| English | 中文 | 一句话说明 |
|---|---|---|
| Risk assessment | 风险评估 | 找出系统面对的内外部安全风险 |
| Risk management | 风险管理 | 决定花时间金钱防哪些威胁 |
| Asset | 资产 | 支援业务目标的软硬件、系统、网络、资料库 |
| Loss event / failure case | 损失事件 | 对资产有害的事件 |
| Threat | 威胁 | 可能造成损失的事情 |
| Frequency / probability | 频率 / 机率 | 事件发生的可能性 |
| Impact | 冲击 | 事件的严重程度 |
| Mitigation | 缓解 | 降低机率或冲击的措施 |
| Feasibility | 可行性 | 能否实际实施 |
| Cost–benefit analysis | 成本效益分析 | 控制成本 vs 避免的损失 |
| Reasonable assurance | 合理保证 | 控制成本不应超过其收益 |
| Countermeasure | 对策 | 防范措施 |
| Reassessment | 重新评估 | 变化时重新做 |
| Risk tolerance level | 风险容忍度（讲师用法） | Probability × Impact |
| Accept / prioritise the risk | 接受 / 优先处理风险 | 两种决策 |
| Trustworthy computing | 可信赖运算 | Security, privacy, reliability, business integrity |
| CIA triad | CIA 三要素 | Confidentiality, integrity, availability |
| Encryption / hashing | 加密 / 杂凑 | 保密 / 检查完整性 |
| Redundancy / failover | 冗余 / 故障切换 | 保持可用 |
| IT security policy | IT 安全政策 | 书面说明如何保护 IT 资产 |
| AUP | 可接受使用政策 | 资源允许的用途 |
| Incident response | 事件应变 | 如何回应安全事件 |
| BC/DR | 业务持续 / 灾难恢复 | 灾难中与后维持运作 |
| SDLC policy | 软件开发生命周期政策 | 每阶段纳入安全 |
| Change management | 变更管理 | 审查批准系统变更 |
| ISO / ISO 27001 / ISMS | 国际标准化组织 / 资讯安全管理系统 | 国际安全标准 |
| NIST / CSF | 美国国家标准与技术研究院 / 网络安全框架 | Core, Profiles, Tiers |
| Govern / Identify / Protect / Detect / Respond / Recover | 治理 / 识别 / 保护 / 侦测 / 回应 / 复原 | CSF 2.0 六功能 |
| Current / Target Profile | 现况 / 目标概况 | 找差距 |
| Tier: Partial, Risk Informed, Repeatable, Adaptive | 部分、风险知情、可重复、自适应 | 成熟度 |

---

## Cheat sheet

- **Risk assessment**：找内外部威胁；目的 = 把有限资源用在最可能、最严重的威胁上。
- **Asset** = HW/SW/system/network/DB；**loss event** = 对资产有害的事件。
- **8 steps**：① Identify assets ② Specify loss events ③ Frequency ④ Impact ⑤ Options to mitigate ⑥ Feasibility ⑦ Cost/benefit ⑧ Decision → Reassessment。
- **Risk tolerance = Probability × Impact**；公开资料（高机率、低冲击）→ **accept**；敏感资料（低机率、critical）→ **prioritise**。
- **Trustworthy computing 4 pillars**：Security（抗攻击、CIA）｜Privacy（个人控制资料）｜Reliability（可靠、需要时可用）｜Business integrity（对客户负责、开放）。
- **CIA**：C = 只有授权者看（encryption, access control, MFA）｜I = 正确未被篡改（$10,000 → $1；hashing, audit log）｜A = 需要时可用（warehouse staff；backup, redundancy, DDoS protection）。
- **IT security policy** = 书面说明如何保护 IT 资产；6 例：AUP, Information Security, Incident Response, BC/DR, SDLC, Change Management。
- **ISO 27001** = ISMS 要求；14 categories（Policy, Organisation, HR, Asset, Access, Crypto, Physical, Operations, Communications, System dev, Supplier, Incident, BC, Compliance）。
- **NIST CSF 2.0**：Core（**GV, ID, PR, DE, RS, RC**）｜Profiles（Current vs Target；5 步 scope→gather→create→gap→implement）｜Tiers（Partial, Risk Informed, Repeatable, Adaptive）。

---

## Practice (answers included)

### A. MCQ

1. An attacker changes an online order total from RM2,000 to RM20. Which principle is violated?
   (a) Confidentiality (b) Integrity (c) Availability (d) Privacy
2. Which step of risk assessment compares the cost of a control with the loss it prevents?
   (a) Options to mitigate (b) Feasibility of options (c) Cost/benefit analysis (d) Identify assets
3. "The system is dependable, available when needed and performs as expected" describes which pillar?
   (a) Security (b) Privacy (c) Reliability (d) Business integrity
4. Which NIST CSF 2.0 function was newly added and sets strategy and policy?
   (a) Identify (b) Govern (c) Protect (d) Recover
5. A database of publicly available information has a high probability but non-critical impact of breach. The decision is to:
   (a) prioritise the risk (b) accept the risk (c) shut down the database (d) buy insurance

**Answers:** 1-b, 2-c, 3-c, 4-b, 5-b

### B. Short answer

**B1. Calculate and rank: Threat A (P = 4, I = 2), Threat B (P = 2, I = 5), Threat C (P = 3, I = 3). (3 marks)**
A = 4 × 2 = 8; B = 2 × 5 = 10; C = 3 × 3 = 9. Rank: B (10) > C (9) > A (8). B is handled first even though it is least likely, because its impact is critical.

**B2. Differentiate a Current Profile and a Target Profile in NIST CSF. (4 marks)**
A Current Profile specifies the Core outcomes an organisation is currently achieving (or attempting to achieve) and to what extent. A Target Profile specifies the desired outcomes the organisation has selected and prioritised for its risk-management objectives, considering expected changes such as new requirements, technologies and threats. Comparing the two reveals gaps for an action plan.

**B3. Explain why the slide's description of availability is inaccurate. (3 marks)**
The slide says availability ensures data is accessible only to authorised users and not to unapproved individuals — restricting who can access is confidentiality. Availability means authorised users can access systems and data reliably whenever they need them, e.g. protected against DoS and failures through redundancy and backups.

### C. Application

*Case: SwiftPay, a Malaysian e-wallet company, recently suffered a two-hour outage and discovered that some staff share admin passwords.*

**C1.** Apply steps 1–4 of the risk assessment to SwiftPay. (8 marks)
1. Identify assets — e-wallet transaction servers, customer account database, mobile app, admin console. 2. Specify loss events — service outage/DDoS, account takeover via shared admin passwords, data breach, fraudulent transactions. 3. Frequency — shared-password misuse: likely (practice is ongoing); DDoS: moderate; full data-centre failure: rare. 4. Impact — outage: high (users cannot pay, loss of trust, regulatory attention); admin account misuse: critical (could alter balances or steal data).

**C2.** Recommend ONE control for each CIA principle for SwiftPay. (6 marks)
Confidentiality — eliminate shared admin accounts; individual admin accounts with MFA and least privilege. Integrity — transaction signing/hashing and immutable audit logs of all balance changes. Availability — redundant servers across data centres with automatic failover and DDoS protection to prevent another outage.

**C3.** Which TWO IT security policies should SwiftPay introduce first? Justify. (6 marks)
Acceptable Use/Information Security Policy — to forbid password sharing and require individual accountable accounts, directly addressing the staff practice. Business Continuity and Disaster Recovery Policy — to define backup, failover and recovery-time targets so that an outage like the two-hour one is prevented or recovered quickly.

### D. Thinking

**D1.** "A company should always eliminate every risk it finds." Discuss. (4 marks)
No. No amount of money can guarantee perfect security; the cost–benefit step requires that the cost of a control should not exceed the loss it prevents (reasonable assurance). Low-impact risks may be accepted (e.g. public data), while high-impact risks are prioritised. The goal is to manage risk to an acceptable level, reassessing as conditions change.

**D2.** Why does trustworthy computing include business integrity, not just technical security? (4 marks)
Because customers' trust depends on how the company behaves, not only on technology. Even a secure system loses trust if the company hides breaches, ignores complaints or misleads customers. Business integrity — responsibility, openness and helping customers solve problems — completes the technical pillars of security, privacy and reliability.

---

## Slide index

| Note section | Slides |
|---|---|
| 1 Risk assessment (definitions) | p4–5 |
| 1.2 Eight steps diagram | p6–10 |
| 1.3 Risk tolerance & decisions | p7, p11 |
| 2 Trustworthy computing | p12–14 |
| 3 CIA triad | p15–17 |
| 4 IT security policy | p18–21 |
| 5 ISO 27001 | p22–24 |
| 6 NIST CSF | p25–30 |

## Links to other chapters
- Loss events (DDoS, insider theft, virus) → **Ch2**
- Cyber security subdomains, PDPA, Act 854 risk assessments for NCII → **Ch3**
- ICT code of conduct vs AUP → **Ch4**
- Cloud security in healthcare / SmartRetail → **Ch5**
- SDLC policy, testing and quality → **Ch7**
