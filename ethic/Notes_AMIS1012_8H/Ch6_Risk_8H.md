# Ch6 风险评估与可信赖运算 · Risk Assessment & Trustworthy Computing

> ⏱ **建议 1 小时 50 分** · 📝 **每份 past year 的 Q2（25 分）都是这一章**，而且都是 case study（TAR、Cybertron、PQR、e-commerce 公司）
> 📖 范围 = 讲师 Revision Notes 的 Ch6 全部 6 节 + **8 steps**（讲师没写，但考过 3 次）
> **怎么读**：每一节先看 ❓，自己想 5 秒 → 读"大白话" → 看例子 → 背 📖 讲师原句和 💬 答题句 → 最后看 ✅ 真题满分答案。

---

## 🗺️ 全章地图（Tree map）

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
*看图重点：① 是"找出危险在哪里"；②③ 是"保护到什么程度才算好"；④⑤⑥ 是"用什么规则和标准去保护"。*

## 🎯 考试怎么问（五份 past year 的 Q2）

| 考点 | Oct 2024 (TAR) | Jan 2025 (Cybertron) | May 2025 | Oct 2025 (PQR) | May 2026 (e-commerce) |
|---|---|---|---|---|---|
| 为什么要做 risk assessment / 怎么应用 | 2a (3) | | | 2a (3) | |
| 8 steps / 前两步 | 2b (8) | | | 2b (8) | 2a (6) |
| Main asset | 2c (2) | | | 2c (2) | |
| **CIA + 情境** | 2d (12) | 2c C + A (8) | | 2d (12) | 2b (9) |
| Trustworthy computing | | 2d (4) | 2b (8) | | 2c (6) |
| IT security policy | | 2a (1) + 2b (12) | 2c (12) | | |
| ISO 27001 / NIST | | | 2a (5) | | 2d (4) |

👉 **CIA 五份里考了四份，分数最高（8–12 分）**。这一章最先要搞懂的就是 CIA。

---

## 1. General Security Risk Assessment 一般安全风险评估

### ❓ 1.1 公司的钱和时间有限，怎么知道要先保护哪一样东西？

**大白话**
想象你家要装防盗设备，但钱只够买两样。你会先想：家里有什么值钱的？（电脑、珠宝、护照）小偷最可能从哪里进来？（后门没锁）哪样东西被偷最惨？（护照丢了出不了国）然后才决定先装后门锁，还是先买保险箱。

公司也一样。公司有很多 IT 东西：电脑、server、资料库、网络。不可能每样都花大钱保护。**Risk assessment（风险评估）就是有系统地把这些东西列出来，找出哪里最危险，再决定钱和时间先花在哪里。**

**为什么重要？** 没有做评估，公司可能花大钱保护不重要的东西（例如公开的产品目录），却忽略最重要的（例如顾客的信用卡资料）。

### 🔑 关键词

| English | 中文 | 一句话 |
|---|---|---|
| **Risk assessment** | 风险评估 | 有系统地找出 IT 系统面对的风险 |
| **Asset** | 资产 | 任何有价值的东西：servers、data、applications |
| **Failure case**（又叫 loss event） | 失败事件 / 损失事件 | 任何对资产有害的事件：virus infection、DDoS attack |
| **Threat** | 威胁 | 可能造成伤害的东西（骇客、病毒、员工偷资料） |
| **Probability** | 机率 | 这件事有多大可能发生 |
| **Impact** | 冲击 | 发生了会有多严重 |

📖 **讲师原句（Revision Notes）**
- *Definition:* "A **systematic way to identify risks** to an organization's IT systems (**hardware, software, databases, networks**)."
- *Why important:* "Helps decide **how much money and time should be invested** in protecting the organization."
- "**Assets**: Anything valuable (e.g., servers, data, applications)."
- "**Failure Case**: Any harmful event (e.g., virus infection, DDoS attack)."

> 💬 **答题句 (EN)** — *A security risk assessment is a systematic way to identify the risks to an organisation's IT systems — hardware, software, databases and networks. It is important because it helps the company decide how much money and time should be invested in protecting the organisation.*

### ❓ 1.2 两个风险，怎么比较哪一个比较危险？

**大白话**
看两件事：**它有多常发生（probability）**，和**发生了有多惨（impact）**。两个相乘，就是风险有多高。

**Risk Level = Probability of Threat × Impact of Threat**

生活例子：
- 下雨忘了带伞：很常发生（机率高），但只是淋湿（冲击低）→ 风险不高。
- 家里失火：很少发生（机率低），但损失所有东西（冲击极高）→ 风险仍然很高，所以要买保险、装烟雾警报器。

**用 1 到 5 分来算（1 = 很低，5 = 很高）**，以一家零售公司的云端收银系统为例：

| Asset 资产 | Failure case 会出什么事 | P 机率 | I 冲击 | **Risk = P × I** | 结论 |
|---|---|---|---|---|---|
| 顾客付款资料 | 收银员被 phishing，骇客用他的帐号偷卡号 | 4 | 5 | **20** | 🔴 第一优先 |
| 交易纪录 | 员工偷偷改退款金额 | 3 | 3 | **9** | 🟠 第二 |
| 收银系统 | 促销时被 DDoS | 2 | 4 | **8** | 🟠 第三 |
| 公开的产品目录网页 | 被人乱改图片 | 4 | 1 | **4** | 🟢 可以接受 |

*看表重点：**Impact 很高的，就算机率低，也要优先处理**；公开资料就算常出事，影响不大，可以 accept。*

📖 **讲师原句**："**Risk Level = Probability of Threat × Impact of Threat**"

🌰 **讲师的例子**：医院的 **patient database**（asset）被 **ransomware** 攻击（failure case）。Impact 很高（病人有生命危险、医院会吃官司），**就算 probability 只是中等，risk 也是 critical**。

> 💬 **答题句 (EN)** — *Risk level is calculated as the probability of a threat multiplied by its impact. For example, if a hospital's patient database is hit by ransomware, the impact is very high (lives at risk and legal issues), so even with a moderate probability the risk is critical.*

### ❓ 1.3 如果要你"一步一步"做一次风险评估，要怎么做？（8 steps ⭐）

讲师的 Revision Notes 没写这 8 步，但 **Oct 2024、Oct 2025 各考 8 分，May 2026 考前两步 6 分**，一定要背。

**大白话**：先找出**有什么要保护** → 再找出**会出什么事** → 算**多常发生**、**多严重** → 想**有什么办法** → 看**做不做得到** → 算**划不划算** → 最后**决定**。系统或威胁一改变，就**重新做一次**。

```
① Identify assets          找出要保护的资产
   ↓
② Specify loss events      找出会发生什么坏事（威胁）
   ↓
③ Frequency of events      多常发生？        ← 这就是 Probability
   ↓
④ Impact of events         发生了多严重？    ← 这就是 Impact
   ↓
⑤ Options to mitigate      有什么办法降低风险？
   ↓
⑥ Feasibility of options   这些办法做得到吗？（人手、技术、时间）
   ↓
⑦ Cost/benefit analysis    划算吗？（保护的成本不能超过可能的损失）
   ↓
⑧ Decision                 决定做哪些、不做哪些
   ↓
   Reassessment ──→ 有新系统、新威胁时，回到 ①
```
*记忆法：**资产 → 坏事 → 多常 → 多惨 → 办法 → 做得到？ → 划算？ → 决定**。③×④ 就是 Risk = P × I。*

| Step | 中文 | 在做什么（大白话） |
|---|---|---|
| 1 **Identify assets** | 找出资产 | 列出公司最在乎的 IT 资产，先排重要的（支撑主要业务的） |
| 2 **Specify loss events / threats** | 找出损失事件 | 每样资产可能遇到什么坏事：外面的骇客、里面的员工 |
| 3 **Assess frequency of events** | 估计频率 | 每件坏事大概多常发生 |
| 4 **Determine impact of events** | 判断冲击 | 发生了是小麻烦，还是让公司很久不能运作 |
| 5 **Identify options to mitigate** | 找缓解方案 | 怎样让它**比较不会发生**，或**发生了伤害比较小** |
| 6 **Assess feasibility of options** | 评估可行性 | 公司的人手、技术、时间做得到吗 |
| 7 **Perform cost/benefit analysis** | 成本效益分析 | 保护的成本 vs 避免的损失；**成本不应超过收益**（reasonable assurance） |
| 8 **Decide** | 做决定 | 决定实施哪些对策；太贵的就换比较便宜的方案 |

> 💬 **答题句 (EN)** — *The eight steps of a general security risk assessment are: identify the IT assets of most concern; specify the loss events or threats; assess the frequency of each event; determine the impact of each event; identify options to mitigate each threat; assess the feasibility of each option; perform a cost–benefit analysis; and decide which countermeasures to implement. The process is repeated whenever the system or the threats change.*

---

### ✅ 真题满分答案

**Oct 2024 Q2a** — *TAR Sdn. Bhd. sells kitchenware, storage and drinkware and has upgraded its point-of-sale (POS) system to a cloud-based solution. Examine why security risk assessment is important for TAR Sdn. Bhd.* **(3 marks)**

> 1. A security risk assessment is a systematic way to identify the risks to TAR's IT systems — its new cloud-based POS system, the customer payment database, the store network and the staff computers. Since TAR has just moved its POS to the cloud, it faces new threats that it has never assessed before. (1)
> 2. It helps TAR decide how much money and time to invest in protection. By rating each threat as probability × impact, TAR can see that a theft of customer card data (high probability, critical impact) is far more urgent than a slow product website, so its limited budget is spent on the biggest risk first. (1)
> 3. It protects customer trust and helps TAR avoid losses. A payment-data breach could lead to fines under the Personal Data Protection Act 2010, compensation to customers and loss of reputation; identifying and treating the risk early is much cheaper than recovering from a breach. (1)

**Oct 2025 Q2a** — *PQR Sdn. Bhd. sells laptops, keyboards and mice and has adopted a cloud-based POS system. The owner wants to ensure the security of customer payment data. Explain how the IT team would apply the process of a security risk assessment to achieve this goal.* **(3 marks)**

> The IT team would first identify PQR's key assets — the cloud-based POS system and the customer payment data it processes and stores (1). It would then identify the threats to this data, such as a cashier's password being stolen through phishing, an insider copying card numbers, or a DDoS attack on the POS, and rate each threat by probability and impact, e.g. stolen cashier credentials = 4 × 5 = 20, the highest risk (1). Finally, it would compare mitigation options such as encryption, multi-factor authentication and staff training, check that they are feasible and cost less than the loss they prevent, implement the chosen controls, and repeat the assessment whenever the POS or payment methods change (1).

**Oct 2024 Q2b / Oct 2025 Q2b** — *Outline the EIGHT steps in a general security risk evaluation process conducted by the IT team.* **(8 marks)**

> 1. **Identify assets** – identify the IT assets the company is most concerned about, especially those supporting key business goals: the cloud-based POS system and the customer payment database. (1)
> 2. **Specify loss events / threats** – list the events that could harm these assets, e.g. hackers stealing card data, an insider altering refund records, or a DDoS attack stopping the POS. (1)
> 3. **Assess the frequency of events** – estimate how likely each event is, e.g. phishing emails reach cashiers almost every week (high), while a full cloud outage is rare (low). (1)
> 4. **Determine the impact of events** – judge how severe each event would be, e.g. a card-data breach is critical (fines, lawsuits, loss of customers), while a one-hour website slowdown is minor. (1)
> 5. **Identify options to mitigate** – decide how each threat can be made less likely or less damaging, e.g. encrypting card data, enabling multi-factor authentication, installing antivirus and training staff. (1)
> 6. **Assess the feasibility of options** – check whether each option can actually be implemented with the company's IT staff, technology and time, e.g. whether the cloud POS vendor supports MFA. (1)
> 7. **Perform a cost–benefit analysis** – compare the cost of each control with the loss it prevents; e.g. MFA and encryption cost about RM20,000 a year while one breach could cost over RM500,000, so the benefit exceeds the cost. (1)
> 8. **Decide** – decide which countermeasures to implement; if a control is too expensive, choose a cheaper alternative. The assessment is repeated whenever the system or threats change, e.g. when a new e-wallet payment method is added. (1)

**May 2026 Q2a** — *A large e-commerce company stores customer records, transaction data and internal business systems. Describe the first two steps of the security risk assessment process for the company.* **(6 marks)**

> **Step 1 – Identify assets (3):** The IT team identifies and lists the IT assets that the company is most concerned about and ranks them by how important they are to the business. For this e-commerce company, the most important assets are the customer records database (names, addresses, phone numbers), the transaction database (orders and payment references), the e-commerce website and mobile app, and internal business systems such as inventory. The customer and transaction databases rank highest because the company cannot sell without them, and a leak would breach the Personal Data Protection Act 2010.
>
> **Step 2 – Specify loss events / threats (3):** For each asset, the team identifies the harmful events that could happen, from both outside and inside the company. For example, hackers could use SQL injection to steal customer records; a DDoS attack during a big sale could stop customers from checking out; ransomware could lock the inventory system; and a dishonest employee could export customer data to sell to a competitor. These events are then rated for frequency and impact in the next steps.

**Oct 2024 Q2c / Oct 2025 Q2c** — *Identify the main asset that appears important to TAR / PQR.* **(2 marks)**

> The main asset is the **customer payment data** (card numbers and transaction records) that is processed and stored by the new **cloud-based POS system** (1). The owner specifically asked to secure it, and if it were stolen, customers would suffer fraud and the company would face fines, compensation claims and loss of trust (1).

---

## 2. Trustworthy Computing 可信赖运算

### ❓ 为什么你敢把钱放在网上银行？你到底在信任它什么？

**大白话**
你愿意用网上银行，是因为你相信四件事：
1. 骇客**攻不进去**（安全）
2. 银行**不会乱用**你的个人资料（隐私）
3. 你要付账时，app **一定打得开**（可靠）
4. 如果出事，银行**会老实告诉你、负责处理**（商业诚信）

这四件事少了一样，你就不敢用了。**Trustworthy computing（可信赖运算）**就是让科技同时做到这四样，**建立客户的信心**。

```
          ┌──────────────────────────────────────────────┐
          │           TRUSTWORTHY COMPUTING               │  ← 屋顶 = 客户的信任
          └──────────────────────────────────────────────┘
            │ Security │ │ Privacy │ │Reliability│ │ Business  │
            │  安全     │ │  隐私    │ │  可靠      │ │ Integrity │
            │          │ │         │ │           │ │  商业诚信  │
          ══════════════════════════════════════════════════
```
*看图重点：四根柱子一起撑住屋顶；少一根，信任就会塌。*

📖 **讲师原句**
- *Definition:* "An approach ensuring technology is **secure, private, reliable, and ethical**."
- *Goal:* "**Build confidence** in computing for customers and businesses."

| Pillar | 中文 | 📖 讲师定义 | 🌰 讲师例子 | 大白话 |
|---|---|---|---|---|
| **Security** | 安全 | Systems **resist attacks** | **Firewalls and antivirus** | 骇客攻不进来 |
| **Privacy** | 隐私 | Users **control their personal data** | **GDPR compliance** in Europe | 我的资料由我决定给不给、给谁 |
| **Reliability** | 可靠 | Systems **work consistently** | **Online banking available 24/7** | 什么时候要用都能用、结果都正确 |
| **Business Integrity** | 商业诚信 | Companies **act ethically** | **Transparent billing** by cloud providers | 公司诚实、负责，不隐瞒 |

💡 马来西亚的例子：Privacy 可以写 **Personal Data Protection Act (PDPA) 2010**。

> 💬 **答题句 (EN)** — *Trustworthy computing is an approach that ensures technology is secure, private, reliable and ethical, in order to build confidence in computing for customers and businesses. Its four principles are security (systems resist attacks), privacy (users control their personal data), reliability (systems work consistently) and business integrity (companies act ethically).*

### ✅ 真题满分答案

**May 2025 Q2b** — *Briefly explain the FOUR pillars of trustworthy computing.* **(8 marks)**

> 1. **Security** – systems are designed to resist attacks, so data and services are protected from hackers and malware. *Example:* an online bank uses firewalls, antivirus and multi-factor authentication so that attackers cannot break into customer accounts. (2)
> 2. **Privacy** – users control their personal data, and the organisation protects it and uses it only with their consent. *Example:* a shopping app asks users before sending marketing messages, lets them delete their account, and complies with the PDPA 2010 (or GDPR in Europe). (2)
> 3. **Reliability** – systems work consistently and are available whenever they are needed. *Example:* an online banking system is available 24/7 and continues to work during peak periods because it has backup servers. (2)
> 4. **Business integrity** – the company acts ethically and is honest and responsible towards its customers. *Example:* a cloud provider uses transparent billing with no hidden charges, and informs customers quickly if a problem affects their data. (2)

**Jan 2025 Q2d** — *Cybertron Sdn. Bhd. is a cloud storage provider. Attackers exploited its weak access controls and unmonitored employee practices. Discuss how the FOUR pillars of trustworthy computing can be applied at Cybertron to enhance IT security.* **(4 marks)**

> - **Security:** Cybertron should fix its weak access controls by requiring multi-factor authentication for every customer and staff login and by encrypting all stored files, so attackers cannot break in again. (1)
> - **Privacy:** Cybertron should make sure customers control who can see their files, and that employees cannot open customer files without an approved and logged request. (1)
> - **Reliability:** Cybertron should copy customers' files to a second data centre with automatic failover, so the cloud storage keeps working even during an attack or outage. (1)
> - **Business integrity:** Cybertron should monitor employee practices, be honest with customers about the attack, notify those affected quickly and explain what it is doing to fix the problem. (1)

**May 2026 Q2c** — *Explain TWO pillars of trustworthy computing and their relevance to the e-commerce company.* **(6 marks)**

> 1. **Security** – systems must resist attacks. *Relevance:* the company stores customer records and transaction data that are very attractive to cyber criminals. It should use firewalls, encryption, multi-factor authentication and regular patching, so that attacks such as SQL injection or account takeover do not succeed and customers' data stays safe. (3)
> 2. **Privacy** – customers must be able to control their personal data. *Relevance:* the company collects names, addresses, phone numbers and purchase histories. It should collect only what is needed for orders, ask for consent before using data for marketing, allow only authorised staff to view customer records, and comply with the PDPA 2010. This makes customers willing to keep shopping and avoids legal penalties. (3)

---

## 3. CIA Triad ⭐⭐（最常考、分数最高）

### ❓ 3.1 "保护资料"听起来很抽象，其实是在保护哪三件事？

**大白话**
用你的**网上银行帐户**来想：
- **别人看不到**你的余额和交易 → 这叫 **Confidentiality（机密性）**
- 你转 RM100，**不会被人偷偷改成** RM10,000 → 这叫 **Integrity（完整性）**
- 你凌晨两点要付账，app **打得开、用得到** → 这叫 **Availability（可用性）**

这三个字的第一个字母合起来就是 **CIA**。讲师说它是 **"the foundation of cybersecurity"**（网络安全的基础）：几乎所有保护措施，都是为了守住这三样的其中一样。

```
                    Confidentiality
                   只有授权的人能看
                          /\
                         /  \
                        / CIA\
                       /______\
            Integrity            Availability
          资料不能被乱改          需要时能用得到
```

### 🔑 关键词

| English | 中文 | 📖 讲师定义 | 🌰 讲师例子 |
|---|---|---|---|
| **Confidentiality** | 机密性 | **Only authorized people** can access data | **Password-protected medical records** |
| **Integrity** | 完整性 | Data **cannot be altered improperly** | **Digital signatures** to prevent tampering |
| **Availability** | 可用性 | Systems must be **accessible when needed** | **E-commerce sites working during Black Friday** |

### ❓ 3.2 每一个要用什么方法保护？（写情境题一定要用）

| | 会被什么破坏？ | 用什么保护？ |
|---|---|---|
| **Confidentiality** | 骇客偷看、员工偷资料、密码被盗 | **Encryption**（加密）、**password + MFA**（多重验证）、**access control**（按职位给权限） |
| **Integrity** | 骇客改资料、员工改金额、病毒破坏档案 | **Digital signature / hashing**（数码签名、杂凑）、**audit log**（记录谁改了什么）、只准主管修改 |
| **Availability** | **DDoS**、server 坏掉、停电、ransomware | **Backup**（备份）、**redundant servers**（备用伺服器）、**DDoS protection**、disaster recovery plan |

⚠️ **别搞混**：Availability **不是**"只有授权的人能用"，那是 Confidentiality。Availability 的重点是**需要的时候用得到**。

> 💬 **答题句 (EN)** — *The CIA triad is the foundation of cybersecurity. Confidentiality means only authorised people can access data (e.g. password-protected medical records); integrity means data cannot be altered improperly (e.g. digital signatures prevent tampering); availability means systems are accessible when needed (e.g. an e-commerce site keeps working during Black Friday).*

### ❓ 3.3 情境题怎么写才拿满分？

每个原则写 4 件事：**① 定义 → ② 在这家公司会发生什么坏事 → ③ 公司要怎么做 → ④ 结果**。一定要**叫出公司名字**，也要写**具体的系统和资料**。

### ✅ 真题满分答案

**Oct 2024 Q2d / Oct 2025 Q2d** — *Discuss the THREE CIA triad principles and provide a scenario for each to demonstrate how it should be applied in TAR Sdn. Bhd. / PQR Sdn. Bhd.* **(12 marks)**

（以 PQR 为例；TAR 只要把公司名和商品换成 kitchenware / drinkware。）

> **1. Confidentiality (4)** – Confidentiality ensures that only authorised people can access data. *Scenario:* when a customer buys a RM4,999 laptop at a PQR branch and pays by card, the card number passes through the cloud-based POS. If a cashier's password is stolen through a phishing email, an outsider could log in and read customers' full card numbers. PQR should encrypt the card data both when it is sent to the cloud and when it is stored, show cashiers only the last four digits, and require every cashier to log in with their own password plus multi-factor authentication. As a result, even a stolen password cannot reveal customers' card details, and PQR avoids a data breach under the PDPA 2010.
>
> **2. Integrity (4)** – Integrity ensures that data cannot be altered improperly. *Scenario:* a dishonest employee could change a completed sale of a RM350 keyboard into a "refund" and keep the cash, or malware could change prices in the POS. PQR should allow refunds only with a supervisor's approval, keep an audit log that records who changed what and when, and use digital signatures or hashing on transaction records so that any change is detected. At the end of each month the manager checks the audit log against daily sales, so any tampered transaction is found and traced to the person responsible.
>
> **3. Availability (4)** – Availability ensures that systems are accessible when needed. *Scenario:* during PQR's year-end sale, hundreds of customers queue at the counters. If a DDoS attack or an Internet outage stops the cloud-based POS, cashiers cannot take any payment and PQR could lose a whole day of sales. PQR should choose a cloud provider with backup servers and DDoS protection, keep a backup Internet line at each branch, and use an offline mode that stores sales locally and uploads them when the connection returns. This way, the stores can keep selling even during an attack or outage.

**Jan 2025 Q2c** — *Assess the Confidentiality principle and the Availability principle that can be adopted at Cybertron Sdn. Bhd. (cloud storage provider), with an example each.* **(8 marks)**

> **1. Confidentiality (4)** – Only authorised people should be able to access customers' stored files. Cybertron's attackers exploited its weak access controls, so this principle must be strengthened. *Example:* a law firm stores thousands of client contracts on Cybertron. Cybertron should require multi-factor authentication for every account, give support staff only the rights they need (they can reset a password but cannot open files), encrypt each customer's files, and log every staff access. Then, even if a staff password is phished, the attacker still cannot read the law firm's contracts.
>
> **2. Availability (4)** – Customers must be able to reach their files whenever they need them; for a cloud storage provider, this is its whole business. *Example:* if Cybertron's main data centre loses power for eight hours, thousands of business customers cannot open their files and may move to a competitor. Cybertron should copy all data to a second data centre in another location with automatic failover, keep daily backups, and use DDoS protection, so customer requests are redirected to the backup site within minutes.

**May 2026 Q2b** — *Analyse THREE ways the CIA Triad can be applied to safeguard the e-commerce company's information systems.* **(9 marks)**

> **1. Confidentiality – protecting customer records (3).** The company stores millions of customers' names, addresses and phone numbers. It should encrypt the customer database, give access only to staff whose jobs need it (for example, delivery staff see addresses but not purchase history), and require multi-factor authentication for all admin logins. This prevents a stolen staff password from exposing customer data and breaching the PDPA 2010.
>
> **2. Integrity – protecting transaction data (3).** An attacker or dishonest employee might change an order total from RM1,200 to RM12 or create a fake refund. The company should validate all input on the website to block SQL injection, use digital signatures or hashing to detect any change to transaction records, and keep audit logs of every modification, so altered orders can be detected, traced and corrected.
>
> **3. Availability – keeping the website and business systems running (3).** A DDoS attack or ransomware during a big sale could stop checkout and the inventory system. The company should use backup servers and load balancing, keep daily offline backups, and test a disaster recovery plan that restores key systems within a few hours. This keeps sales going and protects customer confidence.

---

## 4. IT Security Policy IT 安全政策

### ❓ 员工不知道"可以做什么、不可以做什么"，公司怎么办？

**大白话**
学校有校规，公司的 IT 也要有"IT 规矩"。**IT security policy 就是公司写下来的一份规定，说明公司怎样保护它的 IT 资产**：密码要怎么设、在家怎么连公司系统、资料多久备份一次……写成白纸黑字，员工就知道要怎么做；有人违反，公司也有依据处理。

📖 **讲师原句**
- *Definition:* "A **written plan** on how a company **protects its IT assets**."
- *Examples of policies:*
  - "**Password policies** (e.g., must **change every 90 days**)."
  - "**Remote access rules** (**VPN required**)."
  - "**Data backup and disaster recovery** procedures."

| Policy | 中文 | 规定什么（大白话） | 🌰 具体例子 |
|---|---|---|---|
| **Password policy** | 密码政策 | 密码要多长、多久换、不能共用 | 至少 12 个字、**每 90 天换**、要用 MFA |
| **Remote access policy** | 远端存取政策 | 在公司外面怎么连回公司系统 | **一定要用 VPN**，不能用公共 Wi-Fi 直接登入 |
| **Data backup & disaster recovery policy** | 备份与灾难复原政策 | 资料多久备份、出事后怎么恢复 | 每天备份到另一个地点，出事 4 小时内恢复 |
| **Acceptable use policy (AUP)** | 可接受使用政策 | 员工可以怎样用公司电脑和网络 | 不能装未经批准的软件、不能共用帐号 |
| **Incident response policy** | 事件应变政策 | 被攻击时要怎么处理 | 1 小时内隔离中毒的电脑、通知主管和客户 |

（前三个是讲师写的例子；后两个是 slide 上的，题目要 THREE 或 FOUR 个时可以用。）

> 💬 **答题句 (EN)** — *An IT security policy is a written plan that states how a company protects its IT assets. Examples include a password policy (passwords must be changed every 90 days), remote access rules (a VPN is required), and data backup and disaster recovery procedures.*

### ✅ 真题满分答案

**Jan 2025 Q2a** — *Identify the purpose of an IT Security Policy.* **(1 mark)**

> The purpose of an IT security policy is to set out, in a written plan, how the company protects its IT assets, so that every employee has clear rules to follow. (1)

**Jan 2025 Q2b** — *Provide FOUR examples of IT Security Policy that Cybertron Sdn. Bhd. can implement. Explain the importance of each.* **(12 marks)**

> 1. **Password policy (3)** – passwords must be at least 12 characters, changed every 90 days, never shared, and combined with multi-factor authentication; accounts are closed on the day an employee leaves. *Importance:* attackers exploited Cybertron's weak access controls, and this policy makes a guessed or stolen password much less useful.
> 2. **Acceptable use policy (3)** – employees may not install unapproved software, share accounts, or open customer files without an approved request, and admin activity is logged and reviewed every week. *Importance:* it directly fixes Cybertron's unmonitored employee practices and gives the company a basis for disciplinary action.
> 3. **Remote access policy (3)** – staff working from home must connect only through the company VPN with multi-factor authentication, using company laptops; logging in to admin systems over public Wi-Fi is not allowed. *Importance:* it stops attackers from intercepting staff sessions or entering through an infected home computer.
> 4. **Data backup and disaster recovery policy (3)** – customer data is backed up every day to a second data centre, with regular recovery tests. *Importance:* as a cloud storage provider, Cybertron must never lose customer files, even after ransomware or a data-centre failure.

**May 2025 Q2c** — *Discuss any THREE IT security policies adopted by companies and provide an example for each to demonstrate its application.* **(12 marks)**

> 1. **Password policy (4)** – it sets rules for creating and managing passwords to prevent unauthorised access, e.g. passwords must be changed every 90 days and must not be shared. *Application:* an online retailer requires 12-character passwords with multi-factor authentication and locks an account after five wrong attempts. When an attacker tried thousands of leaked passwords against staff accounts one night, every attempt was blocked and the IT team was alerted.
> 2. **Remote access policy (4)** – it states how employees may connect to company systems from outside the office, e.g. a VPN is required. *Application:* doctors at a clinic chain who check lab results from home must use the clinic's VPN on clinic laptops. When one doctor tried to log in from a hotel's public Wi-Fi without the VPN, the system refused, protecting patients' records from being intercepted.
> 3. **Data backup and disaster recovery policy (4)** – it makes sure data is copied regularly and can be restored so the business keeps running after a disaster. *Application:* a café chain backs up its sales and inventory data every night. When ransomware locked its head-office server, the IT team restored the previous night's backup and all outlets were selling again within three hours, without paying the ransom.

---

## 5. ISO 27001

### ❓ 公司说"我们的资料很安全"，客户凭什么相信？

**大白话**
就像食物上有"Halal"标志，客户一看就放心。资讯安全也有国际认可的标准：**ISO 27001**。公司照这个标准，建立一套**管理资讯安全的制度（ISMS）**，再请外面的审核员来检查，通过了就拿到证书。客户看到证书，就知道这家公司的资料处理达到国际水准。

📖 **讲师原句**
- "A **global standard** for **Information Security Management Systems (ISMS)**."
- "Covers **14 areas**, such as: **Access control** (who can access what) · **Cryptography** (data encryption) · **Human resource security** (training employees) · **Compliance** with laws."
- 🌰 "A company **certified under ISO 27001** can **prove to customers** that their data is handled securely."

| Area（讲师举的 4 个） | 中文 | 大白话 |
|---|---|---|
| **Access control** | 存取控制 | 谁可以看什么资料 |
| **Cryptography** | 密码学 / 加密 | 把资料加密，别人偷了也看不懂 |
| **Human resource security** | 人力资源安全 | 训练员工、签保密协议 |
| **Compliance** | 合规 | 遵守法律（例如 PDPA 2010） |

> 💬 **答题句 (EN)** — *ISO 27001 is a global standard for Information Security Management Systems (ISMS). It covers 14 areas, such as access control, cryptography, human resource security and compliance with laws. A company certified under ISO 27001 can prove to customers that their data is handled securely.*

### ✅ 真题满分答案

**May 2025 Q2a** — *Briefly explain ISO 27001 and list any THREE categories.* **(5 marks)**

> ISO 27001 is a global standard for Information Security Management Systems (ISMS). It gives organisations a structured way to manage the security of their information, covering 14 areas of control. A company that is certified under ISO 27001 can prove to its customers that their data is handled securely. (2)
>
> Three categories (1 each):
> - **Access control** – controlling who can access what, e.g. staff get only the system rights their job needs.
> - **Cryptography** – protecting data through encryption, e.g. customer data is encrypted when stored and when sent.
> - **Human resource security** – training employees, e.g. new staff receive security awareness training and sign a confidentiality agreement.

---

## 6. NIST Cybersecurity Framework (CSF)

### ❓ 面对网络攻击，公司在"之前、当下、之后"各要做什么？

**大白话**
想象你是一栋大楼的保安：
1. 先**认识**大楼有哪些值钱的东西、哪里容易被闯入 → **Identify**
2. 装锁、装闸门、训练员工 → **Protect**
3. 装监视器，**早点发现**有人闯入 → **Detect**
4. 发现了就**马上处理**，把损失控制住 → **Respond**
5. 事后把东西**修好、恢复正常** → **Recover**

这 5 个动作就是美国 **NIST**（National Institute of Standards and Technology）的网络安全框架的核心。

📖 **讲师原句**
- "Created by: **U.S. National Institute of Standards and Technology**."
- "Purpose: Helps organizations **identify, protect, detect, respond, and recover** from cyber threats."
- "**Three Main Parts**: 1. **CSF Core** (5 Functions) · 2. **Organizational Profiles** · 3. **CSF Tiers**"

```
 1. CSF CORE（5 functions）
    Identify ─→ Protect ─→ Detect ─→ Respond ─→ Recover
    认识        保护        发现        回应        复原

 2. ORGANIZATIONAL PROFILES
    Current Profile（现在在哪） ──找出差距──→ Target Profile（想去哪）

 3. CSF TIERS（成熟度等级）
    Tier 1 ─→ Tier 2 ─→ Tier 3 ─→ Tier 4
    低                              adaptive & advanced（能自我调整、最先进）
```

| Part | 中文 | 📖 讲师 | 大白话 |
|---|---|---|---|
| **Identify** | 识别 | Know your **assets and risks** | 知道自己有什么、弱点在哪 |
| **Protect** | 保护 | **Safeguards** (firewalls, training) | 装防护、训练员工 |
| **Detect** | 侦测 | Find security incidents **early** | 早点发现入侵 |
| **Respond** | 回应 | Take action to **contain impact** | 出事时控制损失 |
| **Recover** | 复原 | **Restore** systems and operations | 恢复系统和业务 |
| **Current Profile** | 现况 | **Where we are now** | 公司现在做到什么程度 |
| **Target Profile** | 目标 | **Where we want to be** | 公司想做到什么程度 |
| **Tiers** | 等级 | **Tier 1 = low, Tier 4 = adaptive & advanced** | 安全管理有多成熟 |

🌰 **讲师的例子**：*A **bank** might be at **Tier 3** (repeatable, well-documented security) and aim for **Tier 4**.*

⚠️ 讲师写 **5 functions**，但 slide 用的是新版 CSF 2.0，多了一个 **Govern**（治理：订策略、分配责任）。考试**先写讲师的 5 个**，最后加一句 *"(CSF 2.0 also adds Govern.)"*

> 💬 **答题句 (EN)** — *The NIST Cybersecurity Framework, created by the U.S. National Institute of Standards and Technology, helps organisations identify, protect, detect, respond and recover from cyber threats. It has three parts: the CSF Core (five functions: Identify, Protect, Detect, Respond, Recover), Organizational Profiles (a Current Profile showing where the organisation is now and a Target Profile showing where it wants to be), and CSF Tiers (from Tier 1, low, to Tier 4, adaptive and advanced).*

### ✅ 真题满分答案

**May 2026 Q2d** — *Explain how the company can use ISO 27001 and the NIST Cybersecurity Framework to improve its cybersecurity practices.* **(4 marks)**

> - **ISO 27001 (2):** The company can build an Information Security Management System (ISMS) following ISO 27001, applying controls in areas such as access control, cryptography and human resource security to protect its customer records and transaction data. It can then be certified, which proves to customers and business partners that their data is handled securely.
> - **NIST CSF (2):** The company can organise its security work around the five core functions — identify its customer databases and business systems, protect them with firewalls and staff training, detect attacks early through monitoring, respond to contain incidents, and recover systems from backups. It can compare its Current Profile with a Target Profile to find gaps, and use the Tiers to move, for example, from Tier 2 to Tier 3. (CSF 2.0 also adds Govern.)

---

## 🔁 自测：我问，你先想，答案在下面

1. 讲师对 risk assessment 的定义？它为什么重要？
2. Risk level 的公式？
3. 8 steps 按顺序说出来。
4. 讲师给 trustworthy computing 的 4 个 principles，各配一个讲师例子。
5. "E-commerce 网站在 Black Friday 不能当机"是 CIA 的哪一个？
6. "数码签名防止资料被篡改"是哪一个？
7. 写 CIA 情境题，每个原则要写哪 4 件事？
8. IT security policy 的定义？讲师举的 3 个例子？
9. ISO 27001 是关于什么系统的标准？有几个 area？举 3 个。
10. NIST CSF 的 3 个部分？Core 的 5 个 functions？

.

.

.

.

**答案**
1. **A systematic way to identify risks to an organisation's IT systems (hardware, software, databases, networks)**；helps decide **how much money and time to invest** in protection
2. **Risk Level = Probability of Threat × Impact of Threat**
3. **Identify assets → Specify loss events → Frequency → Impact → Options to mitigate → Feasibility → Cost/benefit analysis → Decision**（→ reassessment）
4. **Security**（firewalls & antivirus）· **Privacy**（GDPR compliance）· **Reliability**（online banking 24/7）· **Business Integrity**（transparent billing）
5. **Availability**
6. **Integrity**
7. **定义 → 公司会发生什么坏事 → 公司要怎么做 → 结果**（全部叫出公司名字）
8. **A written plan on how a company protects its IT assets**；**password policy（90 days）· remote access（VPN）· data backup & disaster recovery**
9. **ISMS**；**14 areas**；access control、cryptography、human resource security（或 compliance）
10. **CSF Core · Organizational Profiles · CSF Tiers**；**Identify, Protect, Detect, Respond, Recover**

---

## 🧾 一页背诵（考前 5 分钟看）

- **Risk assessment** = systematic way to identify risks to IT systems (HW, SW, DB, network) → decide **money & time** to invest
- **Asset** = anything valuable (servers, data, apps) · **Failure case** = any harmful event (virus, DDoS) · **Risk = Probability × Impact**（hospital + ransomware = critical）
- **8 steps**：Identify assets → Loss events → Frequency → Impact → Mitigation options → Feasibility → Cost/benefit → Decision（→ Reassess）
- **Main asset (TAR / PQR)**：customer payment data in the cloud-based POS
- **Trustworthy computing** = secure, private, reliable, ethical → build confidence：**Security**（resist attacks; firewall, antivirus）· **Privacy**（control own data; GDPR / PDPA）· **Reliability**（work consistently; banking 24/7）· **Business Integrity**（act ethically; transparent billing）
- **CIA**：**C** = only authorised（password, encryption, MFA）· **I** = not altered improperly（digital signature, audit log）· **A** = accessible when needed（backup, redundant server, DDoS protection; Black Friday）
- **CIA 情境公式**：定义 → 坏事 → 做法 → 结果 + 公司名
- **IT security policy** = written plan to protect IT assets：password（90 days）· remote access（VPN）· backup & DR（+ AUP、incident response）
- **ISO 27001** = global standard for **ISMS** · **14 areas**（access control, cryptography, HR security, compliance）· certification proves security to customers
- **NIST CSF**：Core **Identify-Protect-Detect-Respond-Recover** · Profiles **Current → Target** · Tiers **1 (low) → 4 (adaptive)**（bank Tier 3 → 4）·（CSF 2.0 + Govern）

➡️ 下一章：**Ch7 Software Development**（很多漏洞在写程式的时候就产生了）
