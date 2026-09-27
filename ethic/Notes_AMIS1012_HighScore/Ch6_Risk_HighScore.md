# Ch6 Risk Assessment and Trustworthy Computing — 高分版

> **每份考卷的 Q2（25 分），全部是 case study**：TAR Sdn. Bhd.（Oct 2024）、Cybertron（Jan 2025）、PQR Sdn. Bhd.（Oct 2025）、大型 e-commerce 公司（May 2026）。
> 高分关键：**每一句都叫 case 公司的名字，并补上你自己的细节**——系统名称、资产、数字、时间、威胁的具体样子、控制措施的具体设定。只写 "use encryption" 拿不到 apply 的分；写 "encrypt card numbers with AES-256 and show cashiers only the last 4 digits" 才是。

---

## 0. 讲师 tips 的考点 → 本页位置

| Tips 考点 | 本页 |
|---|---|
| General security risk assessment（定义、为什么重要、assets、failure case、**Risk = Probability × Impact**） | §1 ⭐ |
| 8 steps（past year 考两次） | §2 ⭐ |
| Trustworthy computing：security、privacy、reliability、business integrity | §3 |
| **CIA triad** | §4 ⭐⭐ |
| IT security policy（password、remote access、backup & DR…） | §5 ⭐ |
| ISO 27001（14 areas） | §6 |
| NIST CSF（Core 5 functions、Profiles、Tiers） | §7 |

---

## 1. Security Risk Assessment ⭐

**核心点**：一种**有系统的方法**，找出组织 IT 资产（hardware、software、database、network）面对的风险；**用来决定该投资多少时间和金钱去保护**。
- **Asset**：任何有价值的东西（servers、data、applications）。
- **Failure case / loss event**：任何有害的事件（virus、DDoS…）。
- **Risk level = Probability of threat × Impact of threat**。

### 高分工具：一张有数字的 risk table（写进答案立刻变具体）

以 **PQR Sdn. Bhd. / TAR Sdn. Bhd. 的 cloud POS** 为例（1 = 很低，5 = 很高）：

| Asset | Threat (failure case) | Probability | Impact | **Risk = P × I** | 优先级 |
|---|---|---|---|---|---|
| Customer payment data in the cloud POS | Card data stolen through a phishing-compromised cashier login | 4 | 5 | **20** | 🔴 立刻处理 |
| Cloud POS service | DDoS / cloud outage during a mid-year sale | 2 | 4 | **8** | 🟠 |
| Transaction records | Insider changes refund amounts | 3 | 3 | **9** | 🟠 |
| Branch PCs | Macro virus from email attachments | 3 | 2 | **6** | 🟡 |

*看这张表要注意：付款资料那一列最高——正好是老板要求"ensure the security of customer payment data"的理由。*

### ✅ 高分答法 — Oct 2024 Q2a: *Examine why security risk assessment is important for TAR Sdn. Bhd.* (3 marks)
1. It identifies which of TAR's assets are most at risk after the move to a cloud POS — above all the customer payment data flowing from its kitchenware and drinkware counters to the cloud — so protection is focused where it matters (1).
2. By estimating risk as probability × impact, TAR can see that a card-data breach (e.g. probability 4, impact 5, risk 20) is far more urgent than a slow website, and decide how much money and staff time to invest; a breach could cost more than RM500,000 in fines under the PDPA, card-scheme penalties and lost customers (1).
3. It lets TAR choose cost-effective controls (encryption, MFA, staff training) before an incident rather than after, and provides evidence to customers and banks that the new POS is secure (1).

### ✅ 高分答法 — Oct 2025 Q2a: *Explain how the IT team at PQR Sdn. Bhd. would apply the process of a security risk assessment to ensure the security of customer payment data.* (3 marks)
The IT team would first list PQR's key assets — the cloud POS and the database of card payments from customers buying laptops, keyboards and mice (1). It would then identify threats to that data (stolen cashier logins, insecure API between the POS and the payment gateway, insider fraud), rate each by probability and impact, and rank them — e.g. stolen cashier credentials score 4 × 5 = 20 (1). Finally it would compare mitigation options such as tokenisation, MFA for all 25 cashiers and TLS encryption against their cost, implement the ones where the benefit exceeds the cost, and repeat the assessment whenever the POS or payment methods change (1).

### ✅ 高分答法 — Oct 2024 Q2c / Oct 2025 Q2c: *Identify the main asset that appears important to TAR / PQR.* (2 marks)
The **customer payment data** (card numbers and transaction records) processed and stored by the new cloud-based POS system (1). It is the asset the owner specifically asked to secure; a leak would cause direct financial loss to customers, PDPA and bank penalties, and loss of trust in TAR's / PQR's stores (1).

---

## 2. The EIGHT Steps ⭐

```
① Identify assets → ② Loss events/threats → ③ Frequency → ④ Impact
                                                              │
⑧ Decision ← ⑦ Cost-benefit analysis ← ⑥ Feasibility ← ⑤ Mitigation options
     │
     └──► (reassess whenever the system or threats change)
```

### ✅ 高分答法 — Oct 2024 Q2b / Oct 2025 Q2b: *Outline the EIGHT steps in a general security risk evaluation process conducted by the IT team.* (8 marks)

1 分一步——**每步都写 TAR / PQR 的具体内容**：

1. **Identify IT assets** – list the assets of most concern: the cloud POS, the customer payment database, the 25 cashier terminals and the store Wi-Fi.
2. **Identify loss events / threats** – e.g. card data stolen by an attacker using a phished cashier password, a DDoS attack on the POS during a sale, or an employee altering refund records.
3. **Assess the frequency of each event** – phishing emails reach cashiers almost weekly (high), insider refund fraud is possible (medium), a major cloud outage is rare (low).
4. **Determine the impact of each event** – a payment-data breach is critical (fines, lawsuits, loss of customers); a 1-hour POS slowdown is minor.
5. **Identify options to mitigate** – tokenise card numbers, enforce MFA for all cashier and admin logins, run monthly phishing training, enable DDoS protection.
6. **Assess the feasibility of each option** – check whether PQR's 3-person IT team and the cloud vendor can support MFA and tokenisation within the rollout schedule.
7. **Perform a cost–benefit analysis** – MFA and tokenisation cost about RM25,000 a year, while one breach could cost over RM500,000, so the benefit clearly exceeds the cost (reasonable assurance).
8. **Decide on countermeasures** – implement MFA, tokenisation and training now; the premium DDoS package (RM60,000/year) is too costly, so use the cloud provider's built-in protection instead; reassess when a new e-wallet payment option is added.

### ✅ 高分答法 — May 2026 Q2a: *Describe the first two steps of the security risk assessment process for the (large e-commerce) company.* (6 marks)

**Step 1 – Identify the IT assets of most concern (3):** the IT team lists all assets and prioritises those supporting key business objectives. For this e-commerce company these are the **customer records database** (e.g. 2 million accounts with addresses and phone numbers), the **transaction database** holding orders and payment references, the e-commerce website and mobile app, and **internal business systems** such as inventory and HR. The customer and transaction databases rank highest because the business cannot sell without them and a leak would breach the PDPA.

**Step 2 – Identify loss events, risks and threats (3):** for each asset, the team lists what could go wrong. *E.g.* credential-stuffing attacks using passwords leaked from other sites to take over customer accounts; SQL injection through the search box to dump the customer database; a DDoS attack during a flash sale that stops checkout; ransomware locking the inventory system; and an insider exporting customer data to a competitor. These events are then rated for frequency and impact in the next steps.

---

## 3. Trustworthy Computing — Four Pillars

| Pillar | 核心点 | 📈 你的细节 |
|---|---|---|
| **Security** | 系统能**抵抗攻击**，保护资料 | Firewall、antivirus、MFA、每月装 patch |
| **Privacy** | 用户**控制自己的个人资料**；公司依法保护 | 只收必要资料、取得同意、遵守 PDPA；用户可在 app 里删除帐户 |
| **Reliability** | 系统**稳定运作**、可预期 | 99.9% uptime、备援 server、24/7 online banking |
| **Business integrity** | 公司**诚实、负责**地经营 | 透明收费、出事时主动通知客户、不隐瞒 breach |

### ✅ 高分答法 — May 2025 Q2b: *Briefly explain the FOUR pillars of trustworthy computing.* (8 marks)
每柱 2 分（定义 + 具体例子）：
1. **Security** – systems and data are protected against attacks such as malware and unauthorised access. *E.g.* BayarLah requires fingerprint login plus a 6-digit PIN for transfers above RM500 and patches its servers every month.
2. **Privacy** – users control how their personal information is collected and used, and the company protects it in line with the law. *E.g.* KedaiKu collects only the name, phone and delivery address needed for an order, asks consent before sending promotions, and lets customers delete their account in the app, complying with the PDPA 2010.
3. **Reliability** – systems perform consistently and are available when needed. *E.g.* MedikaCare's appointment system runs on two data centres so that if one fails, bookings for its 12 clinics continue within minutes, achieving 99.9% uptime.
4. **Business integrity** – the company acts honestly and responsibly toward customers. *E.g.* when a bug double-charged 1,200 customers, KedaiKu informed them by email within 24 hours and refunded everyone within 3 days instead of hiding the error.

### ✅ 高分答法 — Jan 2025 Q2d: *Discuss how the FOUR pillars can be applied at Cybertron Sdn. Bhd. to enhance IT security.* (4 marks)
1 分一柱，每句都连到 Cybertron（cloud storage；weak access controls；unmonitored employee practices）：
- **Security:** fix the weak access controls by enforcing MFA for all customer and staff logins and encrypting every stored file with AES-256.
- **Privacy:** let customers control who can view their files through sharing permissions and link expiry, and ensure Cybertron staff cannot open customer files without a logged, approved request.
- **Reliability:** replicate each customer's files across two data centres (e.g. Cyberjaya and Johor) with automatic failover so storage remains available during an outage.
- **Business integrity:** monitor employee activity, publish a clear security policy, and notify affected customers within 72 hours if a breach occurs.

### ✅ 高分答法 — May 2026 Q2c: *Explain TWO pillars of trustworthy computing and their relevance to the (large e-commerce) company.* (6 marks)
1. **Privacy** (3) – customers must control their personal data and the company must protect it. *Relevance:* the company holds customer records with names, addresses and purchase histories; it should collect only what orders need, obtain consent before using data for marketing, restrict access to the 20 staff who handle delivery disputes, and comply with the PDPA 2010, so customers keep trusting it with their details and it avoids fines.
2. **Reliability** (3) – systems must work consistently and be available when needed. *Relevance:* during a flash sale traffic can jump to 10 times normal; if checkout crashes for even 2 hours the company may lose hundreds of thousands of ringgit in orders. It should use load balancing, auto-scaling servers and a tested backup site so that transaction and inventory systems keep running.

---

## 4. CIA Triad ⭐⭐

| | 意思 | 威胁 | 控制 |
|---|---|---|---|
| **Confidentiality** | 只有授权的人能存取 | 窃听、外泄、未授权存取、insider | 加密、role-based access、MFA、tokenisation |
| **Integrity** | 资料正确、没被不当修改 | 篡改、message alteration、人为错误 | Hashing / checksum、digital signature、audit log、input validation |
| **Availability** | 需要时能使用 | DoS/DDoS、硬件故障、ransomware、停电 | Redundancy、backup & DR、load balancing、UPS |

### ✅ 高分答法 — Oct 2024 Q2d / Oct 2025 Q2d: *Discuss the THREE CIA triad principles and provide a scenario for each to demonstrate how it should be applied in TAR / PQR Sdn. Bhd.* (12 marks)

4 分一个原则：**定义（1）+ 情境中的威胁（1）+ 具体控制（1）+ 结果（1）**。以 PQR（电脑与周边产品零售，新 cloud POS）为例，TAR 只要换公司名和产品：

1. **Confidentiality** – only authorised people can access sensitive information. *Scenario:* at PQR's Low Yat Plaza branch, a customer pays RM4,999 for a gaming laptop by card. If the full card number were stored in the cloud POS and visible to all 25 cashiers, one dishonest cashier could photograph the screen and sell the numbers. PQR should **tokenise** card numbers so the POS stores only a token and shows the last 4 digits, encrypt data in transit with TLS 1.3 and at rest with AES-256, and require each cashier to log in with their own account plus MFA. As a result, even a stolen cashier password cannot reveal any customer's full card number.
2. **Integrity** – information is accurate and cannot be altered without authorisation. *Scenario:* a staff member could change a completed sale of a RM350 mechanical keyboard into a "refund" and pocket the cash, or malware could change prices in the POS. PQR should allow refunds only with a supervisor's approval code, record every change in an audit log showing who, what and when, and use checksums on price files pushed from head office. At month-end, the manager compares the audit log with daily sales, so any tampered transaction is detected and traced to a specific user.
3. **Availability** – authorised users can access systems whenever needed. *Scenario:* during PQR's "Merdeka Mega Sale", 800 customers queue at the counters; if the cloud POS goes down because of an Internet outage or DDoS attack, no sales can be made and PQR may lose about RM200,000 in a day. PQR should use a cloud provider with a 99.9% uptime SLA and DDoS protection, keep a 4G backup Internet line at each branch, and enable an **offline mode** that stores transactions locally and syncs them when the connection returns, so cashiers can keep selling during an outage.

### ✅ 高分答法 — Jan 2025 Q2c: *Assess the Confidentiality principle and the Availability principle that can be adopted at Cybertron Sdn. Bhd., with an example each.* (8 marks)

1. **Confidentiality** (4) – only authorised parties may access customer files. Cybertron's attackers exploited **weak access controls**, so this principle must be strengthened. *Example:* a customer such as a law firm stores 12,000 client contracts on Cybertron. Cybertron should require MFA for every account, apply least-privilege access so support engineers can reset accounts but cannot open files, encrypt each customer's files with its own key, and log every staff access. If a support engineer's password is phished, the attacker still cannot read the law firm's contracts.
2. **Availability** (4) – customers must be able to reach their files whenever needed. As a cloud storage provider, Cybertron's whole business depends on it. *Example:* if Cybertron's Cyberjaya data centre loses power for 8 hours, 5,000 business customers cannot open their files and may move to competitors. Cybertron should replicate data to a second site in another state with automatic failover, keep daily backups, and use DDoS protection, so requests are redirected to the backup site within minutes.

### ✅ 高分答法 — May 2026 Q2b: *Analyse THREE ways the CIA Triad can be applied to safeguard the (large e-commerce) company's information systems.* (9 marks)

3 分一个：
1. **Confidentiality – protecting customer records.** The company stores about 2 million customers' names, addresses and phone numbers. It should encrypt the customer database, give access only to staff roles that need it (e.g. 20 delivery-dispute officers see addresses but not purchase history), and require MFA for every admin login. This prevents a stolen staff password from exposing millions of records and a PDPA breach.
2. **Integrity – protecting transaction data.** An attacker or insider might change an order total from RM1,200 to RM12 or alter a refund. The company should validate all inputs to block SQL injection, use hashes to detect changes to transaction records, and keep audit logs of every modification so any altered order can be traced and reversed.
3. **Availability – keeping internal business systems and the store running.** A ransomware infection or DDoS attack during a flash sale could stop checkout and the inventory system. The company should keep offline daily backups, use load balancing and auto-scaling, and test a disaster-recovery plan that restores core systems within 4 hours.

---

## 5. IT Security Policy ⭐

**核心点**：**书面文件**，说明组织**如何保护它的 IT 资产**，给员工明确的规则。Tips 的例子：password policy（每 90 天换）、remote access（必须用 VPN）、data backup & disaster recovery。Slide 另列：Acceptable Use Policy、Information Security Policy、Incident Response Policy、BC/DR Policy、SDLC Policy、Change Management Policy。

### ✅ 高分答法 — Jan 2025 Q2a: *Identify the purpose of an IT Security Policy.* (1 mark)
To state in writing how the organisation will protect its IT assets and give every employee clear rules to follow — e.g. how Cybertron staff must handle passwords, customer data and remote access.

### ✅ 高分答法 — Jan 2025 Q2b: *Provide FOUR examples of IT Security Policy that Cybertron Sdn. Bhd. can implement. Explain the importance of each.* (12 marks)

3 分一个（名称 + 具体规定 + 为什么对 Cybertron 重要）——连到 "**weak access controls and unmonitored employee practices**"：

1. **Password and access control policy** – passwords of at least 12 characters, changed every 90 days, never shared, with MFA for all staff and customer accounts and access removed on the day an employee resigns. *Importance:* attackers exploited Cybertron's weak access controls; this policy makes a single guessed or phished password useless and closes accounts of former staff.
2. **Acceptable use policy (AUP)** – staff may not install unapproved software, use personal USB drives on servers, or access customer files without a ticket; all admin actions are logged and reviewed weekly. *Importance:* it directly addresses unmonitored employee practices and gives Cybertron grounds for disciplinary action.
3. **Remote access policy** – staff working from home must connect only through the company VPN with MFA from company-managed laptops; public Wi-Fi access to admin panels is banned. *Importance:* it prevents attackers from intercepting admin sessions or using a staff member's infected home PC as a way in.
4. **Data backup and disaster recovery policy** – customer data is backed up daily to a second data centre, with one weekly offline copy, and a recovery test is done every quarter with a target of restoring service within 4 hours. *Importance:* as a cloud storage provider, Cybertron must never lose customer files, even after ransomware or a data-centre failure.

*（也可写 Incident Response Policy：发现入侵 1 小时内隔离、通知 CISO、72 小时内通知受影响客户。）*

### ✅ 高分答法 — May 2025 Q2c: *Discuss any THREE IT security policies adopted by companies and provide an example for each to demonstrate its application.* (12 marks)

4 分一个（名称 + 目的 + 具体规定 + 情境应用）：
1. **Password policy** – sets rules for creating and managing passwords to prevent unauthorised access. *Application:* KedaiKu requires 12-character passwords with MFA and blocks accounts after 5 wrong attempts; when an attacker tried 2,000 leaked passwords against staff accounts one night, every attempt was blocked and the IT team was alerted.
2. **Remote access policy** – defines how employees may connect to company systems from outside the office. *Application:* MedikaCare doctors checking lab results from home must use the clinic's VPN on clinic laptops; when a doctor tried to log in from a hotel's public Wi-Fi without VPN, the system refused, protecting patient records from interception.
3. **Data backup and disaster recovery policy** – ensures data can be restored and operations continue after a disaster. *Application:* Kopi Kaki backs up sales and inventory data every night to the cloud; when ransomware encrypted the head-office server in March, IT restored the previous night's backup and all 8 outlets were selling again within 3 hours without paying the ransom.

---

## 6. ISO 27001

**核心点**：国际标准，规定 **Information Security Management System (ISMS)** 的要求——有系统地管理敏感资产（people、processes、IT systems）。可通过外部审核取得认证，向客户证明安全管理达国际水准。Slide 的 **14 个 categories**（例如 access control、cryptography、human resource security、physical & environmental security、incident management、compliance…）。

### ✅ 高分答法 — May 2025 Q2a: *Briefly explain ISO 27001 and list any THREE categories.* (5 marks)
ISO 27001 is an international standard from the International Organization for Standardization that specifies the requirements for an **Information Security Management System (ISMS)** — a systematic approach to managing an organisation's sensitive information and assets (people, processes and IT systems) so that risks are controlled. An organisation such as BayarLah can be audited and certified against it to prove to banks and customers that its data is handled securely (2).
Three categories (1 each, add a one-line example for a stronger answer):
- **Access control** – e.g. staff get only the system rights their role needs.
- **Cryptography** – e.g. all customer data is encrypted in transit and at rest.
- **Human resource security** – e.g. new staff receive security training and sign a confidentiality agreement.

---

## 7. NIST Cybersecurity Framework

**核心点（照讲师 tips）**：美国 National Institute of Standards and Technology 制定，帮助组织 **identify、protect、detect、respond、recover**。三部分：
1. **CSF Core – 5 functions**：Identify · Protect · Detect · Respond · Recover（*CSF 2.0 再加 **Govern***）
2. **Organizational Profiles**：Current profile（现在）vs Target profile（目标）
3. **Tiers**：Tier 1（partial）到 Tier 4（adaptive）

### ✅ 高分答法 — May 2026 Q2d: *Explain how the company can use ISO 27001 and the NIST Cybersecurity Framework to improve its cybersecurity practices.* (4 marks)
- **ISO 27001 (2):** the company can build an ISMS following ISO 27001 — assign a security owner, assess risks, and apply controls from its categories such as access control, cryptography and incident management — then pass an external audit for certification, showing partners and customers that its customer and transaction data are managed to an international standard.
- **NIST CSF (2):** the company can map its practices to the five core functions — *Identify* its customer databases and business systems, *Protect* them with MFA and encryption, *Detect* attacks with 24/7 monitoring, *Respond* with an incident plan, and *Recover* using tested backups — then compare its **current profile** with a **target profile** to find gaps, e.g. moving from Tier 2 to Tier 3 within a year. *(CSF 2.0 also adds Govern.)*

---

## 8. 自己练习的新情境题（附高分答案）

**P1. MedikaCare wants to move patient records to the cloud. Explain the integrity principle with a scenario. (4 marks)**
> Integrity means patient data is accurate and cannot be changed without authorisation. *Scenario:* if an attacker or careless clerk changed a patient's recorded allergy from "penicillin" to "none", a doctor at the Georgetown branch could prescribe a drug that causes a severe reaction. MedikaCare should allow only doctors to edit medical fields, keep an audit trail of every change with the user ID and time, and use checksums to detect unauthorised modification, so any wrong change is spotted and reversed before treatment.

**P2. Calculate and interpret the risk of two threats for Kopi Kaki: (a) phishing of the admin account — probability 4, impact 4; (b) fire at the head-office server room — probability 1, impact 5. (4 marks)**
> (a) Risk = 4 × 4 = 16; (b) Risk = 1 × 5 = 5. Phishing has the higher risk level, so Kopi Kaki should first spend on MFA and staff phishing training, while fire is handled more cheaply with cloud backups and insurance.

**P3. Explain the Detect and Recover functions of NIST CSF for an e-wallet. (4 marks)**
> *Detect* – BayarLah monitors logins 24/7 and flags unusual activity, e.g. 50 failed logins from one IP in a minute or a transfer from a new device in another country, alerting the security team within minutes. *Recover* – after an incident, BayarLah restores systems from clean backups, resets affected users' credentials, and updates its procedures so the same attack does not succeed again.

---

## 9. Cheat sheet

- **Risk assessment**：systematic identification of risks to IT assets → decide how much to invest；**Risk = Probability × Impact**；asset、failure case。
- **8 steps**：assets → loss events → frequency → impact → mitigation → feasibility → cost-benefit → decision（+ reassess）。
- **Main asset (TAR / PQR)**：customer payment data in the cloud POS。
- **4 pillars**：security · privacy · reliability · business integrity。
- **CIA**：confidentiality（tokenise、encrypt、MFA、RBAC）· integrity（audit log、approval、checksum）· availability（SLA、backup line、offline mode、DR）。
- **Policies**：password（12 chars、90 days、MFA）· AUP · remote access（VPN）· backup & DR（daily、offline copy、4-hour restore）· incident response（72 hours）· change management。
- **ISO 27001** = ISMS requirements；certification；14 categories。
- **NIST** = Identify, Protect, Detect, Respond, Recover（+ Govern in 2.0）；current vs target profile；Tier 1–4。
- **每个答案**：case 公司名 + 具体资产 + 数字 + 控制设定 + 结果。
