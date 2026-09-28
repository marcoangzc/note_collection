# Ch2 Computer and Internet Crime — 高分版

> 考卷 Q1 后半，每份 11–16 分。最常考：**passive vs active attacks**、**virus / worm / trojan / rootkit / DDoS 各配一个例子**、**FIVE types of spam（10 分，考过两次）**、cybercrime 定义与类型、exploit、vulnerability 增加的原因。
> 题目说 "provide an example for each" 时，**一句 "e.g. ILOVEYOU virus" 不够**——要写"谁被攻击、怎么中招、造成什么后果"。

---

## 0. 讲师 tips 的考点 → 本页位置

| Tips 考点 | 本页 |
|---|---|
| What is cybercrime（weapon / target / both；malicious vs accidental） | §1 |
| Computer user expectations（资料多、audit trail、共用密码、复杂度、技术变化快） | §2 |
| Reliance on commercial software（exploit、patch） | §3 |
| Passive vs active attacks（eavesdropping、traffic monitoring；masquerade、replay、alteration、DoS/DDoS） | §4 ⭐ |
| Malware & threats（virus、worm、trojan、botnet、rootkit、spam、phishing） | §5 ⭐ |
| Five types of spam（past year 考两次） | §6 ⭐ |
| Hackers vs crackers；malicious insiders；cybercriminal categories | §7 |

---

## 1. Cybercrime

**核心点**：cybercrime = 透过 Internet / 电脑科技进行的非法活动；电脑可以是 **weapon**（用来攻击）、**target**（被攻击）或 **both**。两类：**malicious harm**（故意）与 **accidental harm**（非故意但仍造成损失）。

### ✅ 高分答法 — Jan 2025 Q1b: *Define cybercrime.* (2 marks)
Cybercrime is any illegal activity carried out using computers or the Internet, where the computer is the **weapon**, the **target**, or both (1). *For example,* a syndicate uses a laptop (weapon) to break into KedaiKu's customer database (target) and steal 50,000 customers' card details (1).

### ✅ 高分答法 — May 2026 Q1d: *Explain TWO types of cybercrime.* (6 marks)

1. **Cybercrime causing malicious harm** (3) – crime committed deliberately to steal, damage or profit, planned against a chosen victim. *Scenario:* a scam syndicate sends 20,000 SMS messages pretending to be KedaiKu offering "RM50 cashback", linking to a fake login page. Within two days 300 customers enter their banking details and lose a total of about RM180,000. The syndicate's acts are cheating under Penal Code s.420 and unauthorised access under the Computer Crimes Act 1997; victims report to the NSRC 997 hotline.
2. **Cybercrime causing accidental harm** (3) – harm that the person did not fully intend but that still disrupts operations and causes financial loss. *Scenario:* Farid, a KedaiKu staff member, plugs a USB drive he found in the car park into his office PC "to find the owner". It contains a worm that spreads to 40 PCs on the office network and shuts down order processing for 5 hours, costing about RM30,000 in lost sales. He did not mean to cause damage, but the loss is real and the company must investigate and clean every PC.

*（另一种可接受的分法：computer as a **target**（hacking、DDoS）vs computer as a **weapon**（phishing、online fraud、cyberbullying）。）*

---

## 2. Computer User Expectations & Rise of Vulnerabilities

**核心点**：
- 储存资料**更快更便宜** → 收集更多资料、更容易追踪用户；**audit trail** 记录每一笔修改 → 资料越多越有价值。
- 问题：用户**共用 login** → 未授权存取；**系统越来越复杂** → 更多漏洞；**技术变化太快** → 难以跟上安全。

### ✅ 高分答法 — Jan 2025 Q1c: *Describe TWO factors that contribute to the rise of computer-related vulnerabilities.* (6 marks)

1. **Increasing complexity of systems** (3) – modern systems connect computers, cloud services, mobile apps, routers and IoT devices through millions of lines of code, and every connection is a possible entry point. *Scenario:* Kopi Kaki connects 8 outlets' POS terminals, 16 smart CCTV cameras and a loyalty app to one network. One camera still uses the default password "admin123"; an attacker logs into it and moves from the camera to the POS network, reading card data. The more devices added, the more doors an attacker can try.
2. **Rapid pace of technological change** (3) – new technologies are adopted faster than security teams can assess them, so new threats appear before defences are ready. *Scenario:* MedikaCare moves its appointment system to a new cloud storage service in two weeks to meet a launch date. The team has not learned the new permission settings, and a storage folder with 3,000 patients' scanned IC copies is left publicly accessible for a month before a security researcher reports it.

*（其他可写：用户期待快速服务 → help desk 跳过身份验证 / 用户共用密码；依赖有已知漏洞的商业软件。）*

---

## 3. Exploits & Patches

**核心点**：**exploit** = 利用系统漏洞（通常因设计或实作不良）的攻击；开发者发现漏洞后开发并发布 **patch**；**用户不装 patch 就有风险**——多数攻击利用的是**已经有 patch** 的漏洞。

### ✅ 高分答法 — Oct 2024 Q1c: *Define "exploits" in the context of a computer attack on a commercial system. Explain how a software developer responds to exploits.* (4 marks)

- **Definition (2):** An exploit is an attack that takes advantage of a flaw (vulnerability) in a system's design or implementation to gain unauthorised access or cause damage. *E.g.* attackers find that KedaiKu's checkout page does not check input, so typing a special SQL string into the voucher-code box returns the whole customer table.
- **Developer's response (2):** The developer quickly analyses the flaw, writes a fix called a **patch** (here, using parameterised queries and input validation), tests it and releases it — for a commercial product, as an automatic update with a security advisory urging users to install it immediately. Systems that delay patching stay exposed: in 2017 the WannaCry ransomware hit organisations that had not installed a Windows patch released two months earlier.

---

## 4. Passive vs Active Attacks ⭐

**判断规则**：**有没有改动资料或影响系统运作？** 没有 → passive；有 → active。

| | **Passive** | **Active** |
|---|---|---|
| 目的 | **偷看 / 偷听资讯**，不改资料 | **修改、冒充、重发、瘫痪** |
| 例子 | Eavesdropping、traffic monitoring | Masquerade、replay、message alteration、DoS/DDoS |
| 侦测 | **很难**（没有留下改动） | 较容易（有明显破坏） |
| 防御重点 | **Prevention**（加密） | **Detection & recovery** |
| 比喻 | 安静的小偷 | 砸窗的窃贼 |

### ✅ 高分答法 — Oct 2024 Q1e: *Identify the main objective of an attacker performing a passive attack on a company.* (1 mark)
To secretly obtain (monitor or eavesdrop on) the company's information — e.g. customers' login details sent over unencrypted Wi-Fi — without altering the data, so the attack goes unnoticed.

### ✅ 高分答法 — May 2025 Q1b: *Discuss (i) passive attacks and (ii) active attacks, and provide an example for each.* (3 + 3 marks)

**(i) Passive attack (3):** an attack in which the attacker only monitors or intercepts information without changing it; its aim is to obtain data, and because nothing is modified it is very hard to detect. *Scenario:* at a café in Bangsar, an attacker sets up a free Wi-Fi hotspot named "KopiKaki_Free". A KedaiKu staff member connects and logs into the company's old admin page, which still uses HTTP. The attacker captures the username and password with a packet sniffer; nothing on the system changes, so no one notices for weeks.

**(ii) Active attack (3):** an attack in which the attacker modifies data, impersonates users or disrupts the system; it causes visible damage and is easier to detect. *Scenario:* using the stolen password, the attacker logs in as the staff member (**masquerade**) and changes the bank account number for KedaiKu's supplier payments to his own. The next RM45,000 supplier payment goes to the attacker's account, which is discovered when the supplier complains of non-payment.

### ✅ 高分答法 — May 2026 Q1c: *Describe passive attacks and active attacks.* (4 marks)
每个 2 分：定义 1 句 + 例子 1 句（用上面两个情境的缩短版）：
- **Passive attacks** only eavesdrop or monitor traffic without altering data, aiming to steal information quietly; *e.g.* capturing a staff member's password over a fake café Wi-Fi hotspot.
- **Active attacks** alter data, impersonate users or disrupt services; *e.g.* logging in with the stolen password and changing KedaiKu's supplier bank account to divert a RM45,000 payment.

### Active attack 的四种（slide）+ 情境

| Type | 意思 | 📈 你的情境 |
|---|---|---|
| **Masquerade** | 冒充另一个人 / 系统 | 攻击者用偷来的 Farid 帐号登入，以管理员身分建立新的 admin 帐号 |
| **Replay** | 截取讯息后**重新发送** | 攻击者截取 BayarLah 用户一笔 "transfer RM200" 的请求，重发 10 次，扣了 RM2,000 |
| **Message alteration** | 修改 / 延迟讯息 | 把网购订单的送货地址从 Johor Bahru 改成攻击者的地址 |
| **Denial of Service (DoS / DDoS)** | 用大量假请求让系统瘫痪 | 11.11 促销时 5 万台被感染的设备同时连线 KedaiKu，结帐页停摆 6 小时 |

### ✅ 高分答法 — Jan 2025 Q1d: *Explain THREE examples of active security attacks.* (9 marks)

1. **Masquerade** (3) – the attacker pretends to be an authorised user or system to gain privileges. *Scenario:* an attacker phones MedikaCare's help desk pretending to be Dr Tan, "locked out before surgery", and the rushed help-desk officer resets the password. Using Dr Tan's account, the attacker opens 500 patient records. The system believes it is the real doctor, so every action appears legitimate.
2. **Replay attack** (3) – the attacker captures a valid message and re-sends it later to repeat its effect. *Scenario:* on an unprotected network, an attacker records a BayarLah user's "pay RM200 to merchant" request and re-sends it 10 times. Because the app does not use one-time tokens or timestamps, the user is charged RM2,000. Adding a unique transaction ID and time limit to each request stops this.
3. **Distributed Denial of Service (DDoS)** (3) – the attacker floods a system with fake requests from many machines so real users cannot be served. *Scenario:* at 12 a.m. on 11.11, a botnet of about 50,000 infected home routers sends millions of requests to KedaiKu's checkout server. Real customers see "Server busy" for 6 hours, and KedaiKu loses around RM120,000 in orders plus customer trust.

*（也可写 message alteration：修改转帐金额或送货地址。）*

---

## 5. Malware & Threats ⭐

| Threat | 核心点 | 📈 你的情境（背一个） |
|---|---|---|
| **Virus** | 附在档案 / 程序上，**使用者开启时**才执行和传播 | 会计部收到 "Invoice_Sept.docm"，打开后 macro virus 感染共享硬碟里 40 台电脑的 Excel 档 |
| **Worm** | **自行**透过网络复制传播，**不需使用者动作** | 一台没装 patch 的伺服器被 worm 感染，一小时内扫描并感染同网段 200 台电脑 |
| **Trojan horse** | **伪装成正常软件**，暗中做坏事；**不会自我复制** | 员工下载"免费 PDF 转换器"，其实装了后门，攻击者可远端控制电脑 |
| **Botnet** | 许多被感染的电脑（zombies）**被攻击者远端控制** | 5 万台家用路由器被感染，被用来发 spam 和 DDoS |
| **Rootkit** | 取得管理员权限并**隐藏自己和攻击者的存在** | Rootkit 修改系统，让恶意程序不出现在 Task Manager 和防毒扫描结果里，攻击者潜伏 3 个月 |
| **Spam** | 不请自来的大量讯息 | 见 §6 |
| **Phishing** | 骗使用者**交出敏感资讯**（密码、卡号） | 假冒银行的 email 说帐户被冻结，要求 24 小时内"验证" |

### ✅ 高分答法 — Oct 2025 Q1b: *Discuss (i) virus, (ii) rootkit, (iii) DDoS attack and provide an example for each.* (2 + 2 + 2 marks)

**(i) Virus (2)** – malicious code that attaches itself to a file or program and runs and spreads only when the user opens that file. *Example:* a clerk at Kopi Kaki's head office opens an email attachment "Supplier_Invoice_Sept.docm" and enables macros; the macro virus copies itself into every Excel file on the shared drive, corrupting the sales reports of all 8 outlets.

**(ii) Rootkit (2)** – a set of programs that gives an attacker administrator-level control of a computer while hiding the attacker's presence and processes from the user and security tools. *Example:* after breaking into KedaiKu's web server, an attacker installs a rootkit that hides his backdoor from Task Manager and antivirus scans; he silently collects customer data for three months before a routine audit finds unusual outbound traffic.

**(iii) DDoS attack (2)** – many compromised computers (a botnet) send a flood of requests to a target at the same time so that legitimate users cannot access the service. *Example:* during BayarLah's RM50 year-end promotion, a botnet of about 50,000 infected IoT cameras floods its payment server; 300,000 users cannot pay for 4 hours and merchants lose sales.

### ✅ 高分答法 — Oct 2024 Q1d: *Discuss (i) trojan horse, (ii) distributed denial of service attack, (iii) rootkit.* (2 + 2 + 2 marks)

**(i) Trojan horse (2)** – a program that appears useful or harmless but secretly carries malicious code; unlike a virus it does not replicate itself. *Example:* Daniel downloads a "free Microsoft Office activator" to his office laptop; it installs a remote-access tool that lets an attacker record his keystrokes, including the company's online-banking password.

**(ii) DDoS (2)** – see above: a botnet floods KedaiKu's checkout server with millions of requests on 11.11 so real customers cannot pay for 6 hours.

**(iii) Rootkit (2)** – see above: it hides the attacker's backdoor on KedaiKu's server from Task Manager and antivirus for three months.

---

## 6. Five Types of Spam ⭐⭐（10 分，考过两次）

每种 2 分：**特点（1）+ 怎么运作 / 你的情境（1）**。

### ✅ 高分答法 — May 2025 Q1c / Oct 2025 Q1c: *Explain FIVE common types of spam — describe their characteristics and how each typically functions.* (10 marks)

1. **Email spam** – unsolicited bulk email, usually advertising, sent to thousands or millions of addresses at almost no cost. It works by using address lists bought or harvested from websites; the same message goes to everyone and often carries a malicious link or attachment. *Example:* 2 million addresses receive "Congratulations! You won an iPhone 16 — claim before midnight", linking to a site that asks for card details.
2. **Social media spam** – spam spread through social networking platforms using fake accounts. Spammers create hundreds of bot profiles that post the same comment or link under popular posts, exploiting users' trust in their friends' feeds. *Example:* under a viral Facebook post by a Malaysian influencer, 300 fake accounts comment "I earned RM3,000 this week with this app!" with a link to a fraudulent investment scheme.
3. **Mobile phone (SMS) spam** – unwanted text messages or app push notifications sent directly to phones, reaching users even without Internet access. *Example:* thousands of Malaysians receive an SMS saying "Your parcel is on hold, pay RM1.50 at [link]"; the fake courier page steals card details.
4. **Text message / instant-messaging spam** – spam broadcast through messaging apps such as WhatsApp or Telegram; it spreads faster than email because messages are forwarded within groups. *Example:* a "Part-time job: like YouTube videos, earn RM300 a day" message is sent to a 250-member university WhatsApp group; students who join are later asked to "deposit" money to unlock payments.
5. **Search engine optimisation (SEO) spam / spam indexing** – misuse of SEO tricks such as keyword stuffing, hidden text and link farms to push a spammer's website to the top of search results. *Example:* a fake website stuffed with "KedaiKu promo code 2026" appears first on Google; shoppers who click are shown pop-up ads and a fake login page instead of real vouchers.

---

## 7. Hackers vs Crackers；Insiders；Cybercriminal Categories

| | 核心点 | 📈 你的情境 |
|---|---|---|
| **Ethical hacker (white hat)** | **合法**测试系统、帮忙改善安全（如 CEH 认证） | BayarLah 付钱请 Hafiz 做 penetration test，他找到 3 个漏洞并写报告 |
| **Cracker (black hat)** | **恶意**入侵系统谋利 | 一名 cracker 入侵 KedaiKu，偷 5 万笔顾客资料在 dark web 卖 RM8,000 |
| **Malicious insider** | 员工或前员工**滥用权限**，常与外人勾结诈骗 | 大学 IT admin 把 3,000 名学生资料卖给补习中心 |
| **Cybercriminal** | 为**钱**（偷卡号、偷钱） | 偷信用卡资料转卖 |
| **Hacktivist** | 为**政治 / 社会理念** | 为抗议某政策，把政府网站首页改成抗议标语 |
| **Cyberterrorist** | 造成**大规模伤害 / 恐慌** | 攻击医院或机场系统，让航班和急诊停摆 |

> **同一个技术，动机不同就是不同类别**——这句写进答案，显示你懂重点。

**📈 高分短答（Tutorial 类题）：Differentiate an ethical hacker and a cracker, and their impact on a business.**
> An ethical hacker is authorised by the organisation to test its systems and report weaknesses: BayarLah hires Hafiz, a CEH-certified tester, who finds that the app's password-reset link never expires and reports it before launch, saving the company from a possible breach. A cracker breaks in without permission for personal gain: a cracker who found the same flaw could take over 300,000 e-wallet accounts, causing financial loss, PDPA penalties and loss of trust.

---

## 8. 自己练习的新情境题（附高分答案）

**P1. Explain phishing with a scenario and TWO ways a company can reduce it. (5 marks)**
> Phishing tricks users into revealing sensitive information by pretending to be a trusted party. *Scenario:* Aina receives an email that looks like it is from KedaiKu's IT department — same logo, sender "it-support@kedaiku-helpdesk.com" — saying her mailbox is full and she must log in within 24 hours. She enters her password on the fake page, and the attacker uses it to send fake invoices to 40 suppliers. *Controls:* (1) run monthly phishing-simulation training so staff learn to check sender domains; (2) enable multi-factor authentication so a stolen password alone cannot log in.

**P2. A worm and a virus both spread. Distinguish them with an example each. (4 marks)**
> A virus needs a user action to spread — it runs only when someone opens the infected file, e.g. a macro virus in an invoice attachment infects the shared drive after a clerk enables macros. A worm spreads by itself across the network without any user action, e.g. a worm exploiting an unpatched Windows service infects 200 PCs in KedaiKu's office within an hour overnight while nobody is working.

**P3. Classify each attacker: (a) a group defaces a ministry website to protest fuel prices; (b) a syndicate steals 10,000 card numbers; (c) an attacker shuts down a hospital's emergency system. (3 marks)**
> (a) Hacktivist – politically motivated; (b) cybercriminal – financially motivated; (c) cyberterrorist – aims to cause large-scale harm and fear.

---

## 9. Cheat sheet

- **Cybercrime** = illegal activity using computers / Internet；computer = weapon / target / both；**malicious vs accidental**。
- **Vulnerabilities 增加**：complexity（更多 entry points）· rapid tech change · user expectations（共用密码、help desk 跳过验证）· commercial software 漏洞。
- **Exploit** = 利用漏洞的攻击 → developer 发 **patch** → 用户要装（WannaCry）。
- **Passive**（eavesdropping、traffic monitoring；难侦测；目的 = 取得资讯）vs **Active**（masquerade、replay、message alteration、DoS/DDoS）。
- **Malware**：virus（开档才传）· worm（自己传）· trojan（伪装、不复制）· botnet（僵尸网络）· rootkit（隐藏 + admin 权限）· phishing（骗资料）。
- **5 spam**：email · social media · mobile/SMS · instant messaging · SEO spam。
- **Ethical hacker vs cracker**；**malicious insider**；**cybercriminal（钱）/ hacktivist（理念）/ cyberterrorist（大规模伤害）**。
- **每个例子都写**：谁、怎么中招、多少台 / 多少钱 / 几小时、后果。
