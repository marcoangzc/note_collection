# Ch6 风险评估与可信赖运算 · Risk Assessment & Trustworthy Computing

> ⏱ **建议 1 小时 40 分** · 📝 **每份 past year 的 Q2（25 分）都是这一章**，而且是 case study（TAR、PQR、Cybertron、e-commerce 公司）
> 📖 范围 = 讲师 Revision Notes 的 Ch6（6 节）+ **8 steps**（tips 没写，但考过两次，各 8 分）
> 用法：每一节先看 ❓，自己想 5 秒，再往下读。英文 **粗体** 是考试要写的字。

---

## 🗺️ 先看全章地图（Tree map）

```
                    Ch6 Risk Assessment & Trustworthy Computing
                                     │
   ┌──────────────┬──────────────┬───┴──────────┬───────────────┬──────────────┐
   ▼              ▼              ▼              ▼               ▼              ▼
 ① Risk         ② Trustworthy  ③ CIA Triad    ④ IT Security  ⑤ ISO 27001    ⑥ NIST CSF
 Assessment     Computing                     Policy
 · Assets       · Security     · Confident-   · written plan  · ISMS         · Core: 5 functions
 · Failure case · Privacy        iality       · password      · 14 areas     · Profiles:
 · Risk =       · Reliability  · Integrity    · remote access                 Current/Target
   P × Impact   · Business     · Availability · backup & DR                 · Tiers 1–4
 · 8 steps ⭐     Integrity
```
*看图重点：①是"找出危险"，②③是"要保护成什么样"，④⑤⑥是"用什么规则 / 标准去保护"。*

---

## 🎯 考试怎么问（五份 past year）

| 考点 | 考过哪几份 | 分数 | 重要度 |
|---|---|---|---|
| **CIA + 每个给情境** | Oct 2024、Jan 2025、Oct 2025、May 2026 | 8–12 | ⭐⭐⭐ |
| **8 steps / 前两步** | Oct 2024、Oct 2025、May 2026 | 6–8 | ⭐⭐⭐ |
| **Trustworthy computing 4 pillars** | Jan 2025、May 2025、May 2026 | 4–8 | ⭐⭐⭐ |
| **IT security policy** | Jan 2025、May 2025 | 12 | ⭐⭐ |
| Why risk assessment + main asset | Oct 2024、Oct 2025 | 3 + 2 | ⭐⭐ |
| ISO 27001 / NIST CSF | May 2025、May 2026 | 4–5 | ⭐ |

---

## 1. General Security Risk Assessment 一般安全风险评估

### ❓ 公司的钱和时间有限，要先保护哪一样东西？

这就是 risk assessment 要回答的问题。大白话：**先把"可能出事的地方"列出来，再算哪个最危险，钱先花在最危险的地方。**

### 🔑 关键词

| English | 中文 | 一句话 |
|---|---|---|
| **Risk assessment** | 风险评估 | 有系统地找出 IT 系统面对的风险 |
| **Asset** | 资产 | 任何有价值的东西：servers、data、applications |
| **Failure case** (= loss event) | 失败事件 / 损失事件 | 任何有害的事件：virus infection、DDoS attack |
| **Probability** | 机率 | 这件事有多大可能发生 |
| **Impact** | 冲击 / 影响 | 发生了有多严重 |
| **Risk level** | 风险等级 | **Probability × Impact** |

📖 **讲师原句（Revision Notes）**
- *Definition:* "A **systematic way to identify risks** to an organization's IT systems (**hardware, software, databases, networks**)."
- *Why important:* "Helps decide **how much money and time should be invested** in protecting the organization."
- "**Risk Level = Probability of Threat × Impact of Threat**"

🌰 **讲师的例子**：医院的 patient database（asset）被 ransomware 攻击（failure case）。Impact 很高（有生命危险、法律问题），**就算 probability 只是中等，risk 也是 critical**。

### 🧮 算一次你就懂（1 = 很低，5 = 很高）

| Asset | Failure case | P | I | **Risk = P × I** | 先做哪个？ |
|---|---|---|---|---|---|
| 顾客付款资料 | 员工被 phishing，骇客偷卡号 | 4 | 5 | **20** | 🔴 第一 |
| 网站 | 促销时被 DDoS | 2 | 4 | **8** | 🟠 |
| 公开的产品目录 | 被改图 | 4 | 1 | **4** | 🟢 可以接受 |

*看表重点：**Impact 高的就算机率低，也要优先**；公开资料就算常出事，也可以 accept。*

> 💬 **答题句 (EN)** — *A security risk assessment is a systematic way to identify the risks to an organisation's IT assets (hardware, software, databases and networks). It helps the company decide how much money and time to invest in protection, where risk level = probability of the threat × impact of the threat.*

### 1.1 八个步骤 8 Steps ⭐（tips 没写，但考过 2 次 × 8 分）

### ❓ 如果要你一步一步做风险评估，第一步做什么？最后一步做什么？

记法：**先找东西 → 再找危险 → 算多常、多严重 → 想办法 → 看做不做得到 → 算划不划算 → 决定**。

```
① Identify assets          找资产
   ↓
② Specify loss events      找会出什么事（威胁）
   ↓
③ Frequency of events      多常发生？（Probability）
   ↓
④ Impact of events         多严重？（Impact）
   ↓
⑤ Options to mitigate      有什么办法降低？
   ↓
⑥ Feasibility of options   做得到吗？（人、技术、时间）
   ↓
⑦ Cost/benefit analysis    划算吗？（成本不能超过损失）
   ↓
⑧ Decision                 决定做 / 不做
   ↓
   Reassessment ──→ 系统或威胁一有改变，就回到 ①
```
*看图重点：③×④ 就是 Risk = P × I；最后会**重新评估**，不是做一次就算。*

| Step | 中文 | 一句话（套 TAR 零售公司的云端 POS） |
|---|---|---|
| 1 **Identify assets** | 找出资产 | cloud POS、**customer payment database** |
| 2 **Specify loss events / threats** | 找出威胁 | 骇客偷卡号、insider theft、DDoS |
| 3 **Frequency of events** | 估计频率 | phishing 几乎每周；云端大当机很少 |
| 4 **Impact of events** | 判断冲击 | 卡号外泄 = critical；网站慢 1 小时 = minor |
| 5 **Options to mitigate** | 缓解方案 | encryption、MFA、antivirus、staff training |
| 6 **Feasibility of options** | 可行性 | IT 团队只有 3 人，做得来吗？ |
| 7 **Cost/benefit analysis** | 成本效益 | MFA 一年 RM20k vs 一次外泄损失 RM500k → 值得 |
| 8 **Decision** | 决定 | 实施 MFA + 加密；太贵的换便宜的方案 |

### 📝 真题：Oct 2024 / Oct 2025 Q2b — *Outline the EIGHT steps in a general security risk evaluation process.* (8 marks)
✅ 写法：**1 步 1 分**。每一步 = 英文步骤名 + 半句 case 例子（像上表）。

### 📝 真题：May 2026 Q2a — *Describe the first two steps of the security risk assessment process for the company.* (6 marks)
✅ 写法：两步各 3 分 = **做什么 + 为什么 + 这家公司的例子**。
- **Identify assets** – list the IT assets the company is most concerned about and prioritise those supporting key business goals, e.g. the **customer records database**, **transaction data** and the e-commerce website, because the business cannot sell without them.
- **Specify loss events** – identify internal and external threats to each asset, e.g. hackers stealing customer data, a **DDoS** attack during a sale, ransomware, or an **insider** copying customer records.

### 📝 真题：Oct 2024 / Oct 2025 Q2a + Q2c
- *Why is risk assessment important for TAR / how would PQR apply it?* (3) → 用 📖 讲师的两句（systematic way to identify risks + decide how much money and time）+ 一句套公司：先保护 **customer payment data**。
- *Identify the main asset.* (2) → **Customer payment data** stored in the new **cloud-based POS system** (1) — the owner asked specifically to secure it, and a leak causes financial loss and loss of trust (1).

---

## 2. Trustworthy Computing 可信赖运算

### ❓ 为什么你敢把钱放在网上银行？你信任它什么？

你相信它：①不会被骇客攻破、②不会乱用你的资料、③随时都能用、④出事会老实告诉你。这四样就是 trustworthy computing 的 **4 根柱子**。

```
          ┌──────────────────────────────────────┐
          │        TRUSTWORTHY COMPUTING          │   ← 屋顶 = 客户的信任
          └──────────────────────────────────────┘
            │ Security │ Privacy │Reliability│ Business  │
            │  安全     │  隐私    │  可靠      │ Integrity │
            │          │         │           │  商业诚信  │
          ════════════════════════════════════════
```
*看图重点：少一根柱子，屋顶（信任）就会塌。*

📖 **讲师原句**：*"An approach ensuring technology is **secure, private, reliable, and ethical**. Goal: **Build confidence** in computing for customers and businesses."*

| Pillar | 中文 | 📖 讲师定义 | 🌰 讲师例子 |
|---|---|---|---|
| **Security** | 安全 | Systems **resist attacks** | Firewalls and antivirus |
| **Privacy** | 隐私 | Users **control their personal data** | GDPR compliance（马来西亚可写 **PDPA 2010**） |
| **Reliability** | 可靠 | Systems **work consistently** | Online banking **available 24/7** |
| **Business Integrity** | 商业诚信 | Companies **act ethically** | **Transparent billing** by cloud providers |

> 💬 **答题句 (EN)** — *Trustworthy computing is an approach that ensures technology is secure, private, reliable and ethical, to build customers' confidence. Its four principles are security (systems resist attacks), privacy (users control their personal data), reliability (systems work consistently) and business integrity (the company acts ethically and openly with customers).*

### 📝 真题
- **May 2025 Q2b** — *Briefly explain the FOUR pillars.* (8) → 每根 2 分 = **定义 + 例子**（用上表）。
- **Jan 2025 Q2d** — *How can the FOUR pillars be applied at Cybertron (cloud storage)?* (4) → 每根 1 句，**一定叫 Cybertron**：
  Security – enforce MFA and encrypt stored files, fixing Cybertron's weak access controls · Privacy – staff cannot open customers' files without consent · Reliability – backup servers so the cloud service stays online · Business integrity – tell customers honestly about the vulnerabilities found.
- **May 2026 Q2c** — *Explain TWO pillars and their relevance to the company.* (6) → 挑 **Security + Privacy**，每个 3 分 = 定义 + 为什么跟这家公司有关 + 做法。

---

## 3. CIA Triad ⭐⭐（最常考、分数最高）

### ❓ 保护资料，其实是在保护哪三件事？

用你的**网上银行**想：
- 别人**看不到**你的余额 → **C**onfidentiality
- 你转 RM100，**不会被改成** RM10,000 → **I**ntegrity
- 你要付账时，app **打得开** → **A**vailability

```
                 Confidentiality
                  只有授权的人能看
                       /\
                      /  \
                     / CIA\
                    /______\
          Integrity          Availability
        不能被乱改            需要时能用
```

📖 **讲师原句**：*"The **foundation of cybersecurity**."*

| | 中文 | 📖 讲师定义 | 🌰 讲师例子 | 🔧 常用做法 |
|---|---|---|---|---|
| **Confidentiality** | 机密性 | **Only authorized people** can access data | **Password-protected** medical records | encryption、MFA、access control |
| **Integrity** | 完整性 | Data **cannot be altered improperly** | **Digital signatures** to prevent tampering | hashing、audit log、只准主管修改 |
| **Availability** | 可用性 | Systems must be **accessible when needed** | E-commerce sites working during **Black Friday** | backup、redundant server、DDoS protection |

⚠️ **别混淆**：Availability **不是**"只有授权的人能用"——那是 Confidentiality。Availability = **需要的时候用得到**。

> 💬 **答题句 (EN)** — *The CIA triad is the foundation of cybersecurity: confidentiality means only authorised people can access data; integrity means data cannot be altered improperly; availability means systems are accessible when needed.*

### 📝 真题：Oct 2024 / Oct 2025 Q2d — *Discuss the THREE CIA principles and provide a scenario for each to show how it should be applied in TAR / PQR.* (12 marks)
✅ 写法：每个 4 分 = **定义 (1) + 情境里会出什么事 (1) + 怎么做 (2)**。以 PQR 云端 POS 为例：

1. **Confidentiality** – only authorised people can access data. *Scenario:* customers' card numbers pass through PQR's cloud POS; if a cashier's password is phished, an outsider could read them. PQR should **encrypt** card data, show cashiers only the **last 4 digits**, and require **MFA** so only finance staff can view full payment records.
2. **Integrity** – data cannot be altered improperly. *Scenario:* a dishonest staff member changes a sale from **RM1,000 to RM1**, or edits refund records. PQR should keep an **audit log** of every change, use **digital signatures / hashing** on transactions, and let only supervisors approve refunds.
3. **Availability** – systems are accessible when needed. *Scenario:* during a year-end sale, a **DDoS** attack or cloud outage stops the POS, so cashiers cannot take payment and sales are lost. PQR should use a provider with **backup servers**, enable **DDoS protection**, and keep an **offline mode** that syncs later.

### 📝 真题
- **Jan 2025 Q2c** — *Confidentiality and Availability at Cybertron, with an example each.* (8) → 同样结构，各 4 分，情境换成**云端档案**（weak access controls → MFA + least privilege；服务中断 → 备援数据中心）。
- **May 2026 Q2b** — *THREE ways the CIA triad can safeguard the company's information systems.* (9) → 每个 3 分：定义 + 做法 + 结果（防资料外泄 / 订单金额正确 / 促销时网站不当机）。

---

## 4. IT Security Policy IT 安全政策

### ❓ 员工不知道"可以做什么、不可以做什么"，公司怎么办？

写成**白纸黑字的规定**。这就是 IT security policy。

📖 **讲师原句**：*"A **written plan** on how a company **protects its IT assets**."*

| 讲师的例子 | 中文 | 🌰 具体写法 |
|---|---|---|
| **Password policy** | 密码政策 | must **change every 90 days** |
| **Remote access rules** | 远端存取规定 | **VPN required** |
| **Data backup and disaster recovery procedures** | 备份与灾难复原 | 每天异地备份，当机 4 小时内恢复 |

Slide 还有这几种（past year 要 3–4 个时可以用）：

| Policy | 中文 | 一句话 |
|---|---|---|
| **Acceptable Use Policy (AUP)** | 可接受使用政策 | 员工**可以怎样用**公司的电脑和网络 |
| **Incident Response Policy** | 事件应变政策 | 出事时**怎么回应**（隔离、通报、复原） |
| **Business Continuity & Disaster Recovery Policy** | 业务持续与灾难复原 | 灾难**期间和之后**继续运作、恢复资料 |
| **Change Management Policy** | 变更管理政策 | 系统改动要**先审核、批准**才做 |

> 💬 **答题句 (EN)** — *An IT security policy is a written plan that states how a company protects its IT assets, for example a password policy requiring passwords to be changed every 90 days, remote-access rules requiring a VPN, and data backup and disaster recovery procedures.*

### 📝 真题
- **Jan 2025 Q2a** — *Purpose of an IT security policy.* (1) → 用 📖 讲师原句。
- **Jan 2025 Q2b** — *FOUR examples for Cybertron and the importance of each.* (12) → 每个 3 分 = **名字 + 内容 + 为什么 Cybertron 需要**（题目说 Cybertron 有 *weak access controls and unmonitored employee practices* → Password policy、AUP、Remote access (VPN)、Backup & DR）。
- **May 2025 Q2c** — *THREE policies with an example each.* (12) → 每个 4 分 = 定义 2 + 例子 2。

---

## 5. ISO 27001

### ❓ 公司怎么向客户"证明"自己的资料安全做得好？

去拿一张国际认可的证书：**ISO 27001**。

📖 **讲师原句**：*"A **global standard** for **Information Security Management Systems (ISMS)**. Covers **14 areas**."*

| 讲师举的 4 个 area | 中文 | 意思 |
|---|---|---|
| **Access control** | 存取控制 | 谁能看什么 |
| **Cryptography** | 密码学 | 资料加密 |
| **Human resource security** | 人力资源安全 | 训练员工 |
| **Compliance** | 合规 | 遵守法律 |

🌰 讲师例子：*A company **certified** under ISO 27001 can **prove to customers** that their data is handled securely.*

> 💬 **答题句 (EN)** — *ISO 27001 is a global standard for Information Security Management Systems (ISMS). It covers 14 areas such as access control, cryptography, human resource security and compliance, and a certified company can prove to customers that their data is handled securely.*

---

## 6. NIST Cybersecurity Framework (CSF)

### ❓ 被攻击的"之前、当下、之后"，公司各要做什么？

NIST CSF 用 **5 个动作**回答：**认识 → 保护 → 发现 → 回应 → 复原**。

📖 **讲师原句**：*"Created by the U.S. **National Institute of Standards and Technology**. Helps organizations **identify, protect, detect, respond, and recover** from cyber threats."* 分 **3 个部分**：

```
 1. CSF CORE（5 functions）
    Identify ─→ Protect ─→ Detect ─→ Respond ─→ Recover
    认识资产    防护      早点发现    控制影响    恢复运作

 2. ORGANIZATIONAL PROFILES
    Current Profile（现在在哪） ──找差距──→ Target Profile（想去哪）

 3. CSF TIERS（成熟度）
    Tier 1 ─→ Tier 2 ─→ Tier 3 ─→ Tier 4
     low                          adaptive & advanced
```

| Function | 中文 | 📖 讲师一句话 |
|---|---|---|
| **Identify** | 识别 | Know your **assets and risks** |
| **Protect** | 保护 | **Safeguards** (firewalls, training) |
| **Detect** | 侦测 | Find security incidents **early** |
| **Respond** | 回应 | Take action to **contain impact** |
| **Recover** | 复原 | **Restore** systems and operations |

🌰 讲师例子：*A bank might be at **Tier 3** (repeatable, well-documented security) and aim for **Tier 4**.*

⚠️ 讲师写 **5 functions**；slide 用的是 CSF 2.0，多了一个 **Govern**。考试**先写讲师的 5 个**，最后加一句 *"(CSF 2.0 also adds Govern.)"*

### 📝 真题
- **May 2025 Q2a** — *Briefly explain ISO 27001 and list THREE categories.* (5) → 定义 2 分 + Access control / Cryptography / Human resource security（各 1）。
- **May 2026 Q2d** — *How can the company use ISO 27001 and NIST CSF to improve its cybersecurity?* (4) → ISO：建立 ISMS、拿认证向客户证明（2）｜NIST：用 5 functions 安排工作、用 Current → Target Profile 找差距、用 Tiers 往上升（2）。

---

## 🔁 自测：我问，你先想，答案在下面

1. Risk level 的公式是什么？
2. 8 steps 的第 1 步和第 8 步？
3. 讲师给 trustworthy computing 的 4 个 principle？
4. "E-commerce 网站在 Black Friday 不能当机"是 CIA 的哪一个？
5. "数码签名防止资料被篡改"是哪一个？
6. IT security policy 的定义（讲师原句）？
7. ISO 27001 是关于什么系统的标准？有几个 area？
8. NIST CSF 的三个部分？Core 有哪 5 个 function？

.

.

.

.

**答案**
1. **Risk Level = Probability of Threat × Impact of Threat**
2. ① **Identify assets** · ⑧ **Decision**（之后 reassessment）
3. **Security, Privacy, Reliability, Business Integrity**
4. **Availability**
5. **Integrity**
6. *A **written plan** on how a company **protects its IT assets**.*
7. **ISMS**（Information Security Management System）· **14 areas**
8. **CSF Core、Organizational Profiles、CSF Tiers** · Core = **Identify, Protect, Detect, Respond, Recover**

---

## 🧾 一页背诵（考前 5 分钟看）

- **Risk assessment** = systematic way to identify risks to IT systems (HW, SW, DB, network) → decide **money & time** to invest
- **Asset** = anything valuable · **Failure case** = any harmful event · **Risk = P × I**
- **8 steps**：Identify assets → Loss events → Frequency → Impact → Mitigate options → Feasibility → Cost/benefit → Decision（→ Reassess）
- **Trustworthy computing** = secure, private, reliable, ethical → build confidence：**Security**（resist attacks）· **Privacy**（control own data）· **Reliability**（work consistently, 24/7）· **Business Integrity**（act ethically, transparent billing）
- **CIA**：**C** only authorised (password, encryption) · **I** not altered (digital signature) · **A** accessible when needed (Black Friday)
- **IT security policy** = written plan to protect IT assets：password (90 days) · remote access (VPN) · backup & DR（+ AUP, incident response, change management）
- **ISO 27001** = global standard for **ISMS**，**14 areas**：access control, cryptography, HR security, compliance
- **NIST CSF**：Core **I-P-D-R-R** · Profiles **Current → Target** · Tiers **1 → 4**（bank Tier 3 → 4）

➡️ 下一章：**Ch7 Software Development**（很多漏洞在写程式时就产生了）
