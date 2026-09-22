# AMIS1012 — Chapter 3: Organisation Privacy (Part I) — Cyber Security & Laws

> 这一章 slide 基本上是一张**法律清单**：US 五条、Europe/UK/China/India 各一条、马来西亚十一条，每条一两句。它没有告诉你**这些法律之间怎么分**（SCA vs ECPA 最容易混）、**马来西亚的法律各管什么**（slide 叫你"自己上网看"），而且 slide 说 Cyber Security Act 2024 "has not yet been enforced"——这已经过时。Tutorial 3 和 BrightHome 情境题还问到 identity theft、workplace monitoring、consumer profiling，这些属于 **Part II**，但文件夹里**没有 Part II 的 slide**。这份笔记把 laws 整理成"管什么资料 / 管谁"的对照表，补上马来西亚主要法律的一句话说明，并用标注为 *extra* 的内容回答 Part II 的题目。

---

## 0. 一句话总览

**Information system 由 people, procedures, software, hardware, data, Internet 组成；cyber security 保护它们的 confidentiality、integrity 与运作；各国用 cyber security / privacy laws 来保护敏感资料、预防犯罪、保障隐私和国家安全——美国按行业分散立法，欧盟用一部全面的 GDPR，马来西亚则由 NACSA 统筹、以 Computer Crimes Act 1997、PDPA 2010 和 Cyber Security Act 2024 为核心。**

| Part | 内容 | Slide |
|---|---|---|
| 0 | Parts of an information system | p2–4 |
| 1 | Cyber security 定义 + 7 subdomains | p7–9 |
| 2 | Cyber security laws 的目的；US laws（CFAA, SCA, ECPA, HIPAA, CCPA, GLBA） | p10–19 |
| 3 | GDPR, UK DPA 2018, China PIPL, India DPDP | p20–22 |
| 4 | Malaysia：NACSA + Malaysian cyber laws（PDPA 2010, CSA 2024） | p23–25 |
| 5 | *Extra (Part II，slide 缺)*：identity theft、workplace monitoring、consumer profiling | Tutorial 3 Q10–13 |

🎯 **这章回答的考题**
- 五份 past year **没有一题直接考 Ch3**。但 Tutorial 3（13 题）和 **BrightHome Bank 情境题**（Act 854、NACSA、CISO 建议）都以这章为主，而且 2026 年的新题型（scenario + KBAT）很可能把这章带进考卷。→ 读懂概念，准备好 BrightHome 的答案。

**Prerequisites**：Ch2 的攻击（hacking、insider、phishing）——这章讲的是用法律对付它们。

---

## Scene：BrightHome Bank 的六个月

BrightHome Bank 推出 Fresh Graduate Mortgage Campaign，网上收集申请人的 **MyKad 号码、收入、雇佣记录、产业文件**。六个月后：
1. 一名**有授权的员工**在辞职前把数千笔客户资料下载到**私人 USB**；
2. 同时，客户收到**冒充银行的 phishing email**，网上银行帐户被入侵；
3. 被偷的资料被卖给营销公司，**未经同意**用于定向广告；
4. 银行属于马来西亚的**关键金融基础设施**，事件被报告给 **NACSA**。

问题：谁的错？违反了哪些法律？以后怎么防？要回答这些，需要先知道 cyber security 保护什么、法律怎么分工。

---

## 0'. 回到基本：Information System 的六个部分（p2–4）

| Part | 说明 | BrightHome 例子 |
|---|---|---|
| **People** | 使用系统的 end users | 银行职员、申请人 |
| **Procedures** | 使用软硬件与资料的规则/指引 | "离职员工当天收回权限"（银行显然没有做到） |
| **Software** | 一步步指示电脑工作的程式；把 **data 转成 information** | Mortgage application portal |
| **Hardware** | 处理资料的设备（手机、键盘、显示器、system unit…）；由 software 控制 | Server、员工电脑、**USB 端口** |
| **Data** | 原始、未处理的事实（文字、数字、影像、声音） | MyKad 号码、收入 |
| **Internet** | 让电脑连接、分享资讯 | 网上申请、网上银行 |

Slide 的例子：payroll program 把"工作时数"(data) × "时薪"(data) → "周薪"(information)。

> BrightHome 的问题**不只是技术**：People（内鬼）+ Procedures（没有 USB 政策、离职流程）+ Data（没有加密）同时失败。

---

## 1. Cyber Security（p7–9）

- 专家担心针对 ICT 网络的攻击会在规模和强度上**持续升级**。
- **Cyber security** = the **methods, techniques, and processes** used to secure the **confidentiality, integrity, and functioning** of information systems, networks, and data against **cyber attacks or unauthorized access**。

**为什么重要？**（Tutorial 3 Q1）：保护敏感资料（个人、财务、商业机密）、确保业务持续运作、维持客户信任与声誉、遵守法律（避免罚款）、保护国家关键基础设施与国家安全。

**Cyber security 的七个 subdomains（p8–9）**：

| Subdomain | 做什么 | BrightHome 如果做好了… |
|---|---|---|
| **Application security** | 安全的应用架构、**secure coding**、严格的 input 验证，防止未授权进入或修改 | Portal 防止 SQL injection |
| **Data protection & identity management** | **Identity**：只让合法人员存取系统；**Data**：保证资料 at rest 与 in transit 的完整性 | 员工只能看自己负责的申请；资料加密 |
| **Network security** | 用软硬件保护网络与设施，防未授权存取、延迟、滥用 | Firewall、IDS |
| **Mobile security** | 保护手机/平板/笔电上的公司与个人资料，防未授权进入、故障、失窃、ransomware | 员工手机上的银行资料可远程删除 |
| **Cloud security** | 为使用 AWS/Google/Azure 的企业建立安全的云架构 | 客户文件存云端时正确设定权限 |
| **Disaster Recovery & Business Continuity (DR&BC)** | 程序、通报、预警、准备，让关键服务在**危机中与危机后**持续，并恢复系统 | 被攻击后网上银行快速恢复 |
| **Security awareness & training** | 正式教育员工安全议题、最佳实践、公司流程与法规，并追踪/举报恶意活动 | 员工认得出 phishing email |

### ✅ 满分答法 — Tutorial 3 Q2: Identify and describe TWO cyber security subdomains.
1. **Data protection and identity management** – identity management provides mechanisms and procedures that allow only legitimate people to access an organisation's information systems, while data protection ensures data integrity at rest and in transit, e.g. through encryption and access control.
2. **Security awareness and training** – formal training that educates employees on security issues, industry best practices, company procedures and regulations, so they can recognise and report malicious activity such as phishing emails.

---

## 2. Cyber Security Laws：目的与 US 法律（p10–19）

**Cyber security laws** = 政府/组织建立的规章与法律框架，保护数位资讯、电脑系统、网络与整体网络环境。

**五个目的（Tutorial 3 Q3）**：
1. **Protection of sensitive data** — 个人、财务、医疗资料；
2. **Prevention of cybercrime** — 把 hacking、fraud 定为犯罪并惩罚；
3. **Data privacy** — 规定组织怎样收集、使用、分享个人资料；
4. **National security** — 保护关键基础设施；
5. **Consumer trust** — 人们敢上网交易。

### 2.1 美国法律：按"资料类型/行业"分开立法

**最容易混的一组：SCA vs ECPA**

```
  Data-in-transit  ─────────────>   Data-at-rest
  (moving)                          (stored)
  emails being sent                 emails stored in a database
  phone calls, text messages        text / IM stored in a database
  upload phone → cloud              data in cloud storage
  hard drive → computer transfer    data on a hard drive
        │                                  │
        v                                  v
     ECPA (Title I: wiretaps)       SCA (= ECPA Title II)
```
*看图重点：同一封 email，"正在寄"时归 ECPA 管，"寄到后存在 server 上"时归 SCA 管。SCA 其实是 ECPA 的第二部分。*

| Law | 管什么 | 重点 |
|---|---|---|
| **Computer Fraud and Abuse Act (CFAA)**（18 U.S.C. §1030） | **Cybercrime** | 保护**联邦政府电脑、银行电脑、连上 Internet 的电脑**，禁止 hacking、online fraud；p11 图列出七种行为：入侵政府电脑、入侵取得政府/金融资讯、用 worm/virus/DoS 破坏、以未授权存取进行诈骗、威胁破坏电脑、**贩卖密码**、为间谍目的存取电脑 |
| **Stored Communications Act (SCA)** | **Data-at-rest** | 保护**储存**中的电子通讯；§2701 禁止未授权存取储存的通讯；§2702 禁止服务商（ECS/RCS，如电讯、email、社交平台、云端）**主动披露**储存的通讯；§2703 政府要用 warrant / court order / subpoena 才能**强制**服务商披露 |
| **Electronic Communications Privacy Act (ECPA)** | **Data-in-transit** | 禁止**故意拦截传输中的电子通讯**（限制 wiretap）。三个 Title：**I** 禁止窃听、禁止政府用非法取得的通讯作证据；**II** = SCA；**III** 政府装 pen register / trap-and-trace 设备须经授权 |
| **HIPAA (1996)** | **医疗资料** | **Privacy Rule** 保护 **PHI**（姓名、地址、SSN、出生日期、病历、保险资料），任何形式（电子、书面、口头）；**Security Rule** 是 Privacy Rule 的子集，只管 **ePHI**，要求保证 CIA。Covered entities：医疗提供者、保险公司、healthcare clearinghouses、**business associates**（第三方承包商） |
| **California Consumer Privacy Act (CCPA)** | **州级**消费者隐私 | 加州消费者四项权利：**right to know**（收集了什么、怎么用和分享）、**right to delete**、**right to opt-out** of sale/sharing、**right to non-discrimination**（行使权利不被差别对待） |
| **Gramm-Leach-Bliley Act (GLBA)** | **金融机构** | 提供贷款、理财、保险的公司必须**向客户说明资料分享做法**并**保护敏感资料**；p19 图：Annual Notice、Secure、Dispose、Access（只给需要的员工）、Train 员工、Manage 第三方风险 |

### ✅ 满分答法 — Tutorial 3 Q4: ONE example of a US employee violating the CFAA.
An employee at a US bank uses a colleague's password to log in to the bank's customer database, which he is not authorised to access, and downloads customer account details to sell them. This is unauthorised access to a bank computer to commit fraud, which the CFAA prohibits.

### ✅ 满分答法 — Tutorial 3 Q5a: Differentiate the SCA and the ECPA.

| | Stored Communications Act (SCA) | Electronic Communications Privacy Act (ECPA) |
|---|---|---|
| Protects | **Stored** electronic communications (**data-at-rest**) | Communications **in transit** (**data-in-transit**) |
| Main prohibition | Unauthorised access to, and providers' voluntary disclosure of, stored communications | Intentional interception of communications, e.g. wiretapping |
| Examples of data | Emails/texts stored in a database, cloud storage, hard drive | Emails being sent, phone calls, data being uploaded |
| Relationship | Title II of the ECPA | The umbrella act (Titles I–III) |

### ✅ 满分答法 — Tutorial 3 Q5b: ONE example of non-compliance with each.
- **SCA:** a cloud email provider allows an employee to read and hand over a customer's stored emails to a private investigator without the customer's consent or a court order.
- **ECPA:** a company secretly installs software to intercept and record employees' personal phone calls and WhatsApp messages as they are being transmitted, without consent.

---

## 3. Europe 与其他国家（p20–22）

**GDPR — General Data Protection Regulation（EU）**
- **"Toughest privacy and security law in the world"**。
- 由 EU 通过，但**对全世界的组织都有义务**——只要它们**针对或收集 EU 人民的资料**（extraterritorial）；组织必须**证明** compliance。
- **2018 年 5 月 25 日**生效。
- 罚款可达**数千万欧元**（extra：最高 €20 million 或全球年营业额 4%，取较高者）。
- **Personal data** = 任何能**直接或间接识别**一个人的资讯：姓名、email、位置、种族、性别、**biometric data**、宗教信仰、**web cookies**、**IP address**、健康与基因资料、政治意见。

| Country | Law | 重点 |
|---|---|---|
| UK | **Data Protection Act 2018** | UK 版本的 GDPR |
| China | **Personal Information Protection Law (PIPL)** | 保护中国公民个人资讯、规范**跨境资料传输**、确保组织负责任地处理资料 |
| India | **Digital Personal Data Protection Bill 2023** | 适用于印度境内的数位个人资料（线上收集，或线下收集后数位化）；也适用于**境外**、为在印度提供商品服务而进行的处理 |

### ✅ 满分答法 — Tutorial 3 Q6: Which is the toughest privacy and security law in the world? Explain.
The **EU General Data Protection Regulation (GDPR)**, in effect since 25 May 2018. It is considered the toughest because (1) it applies to any organisation in the world that targets or collects data about people in the EU, not only EU companies; (2) it defines personal data very broadly, including IP addresses, cookies and biometric data; (3) organisations must be able to prove compliance; and (4) it imposes very heavy fines, reaching tens of millions of euros (up to €20 million or 4% of global annual turnover).

---

## 4. Malaysia：NACSA 与 Malaysian Cyber Laws（p23–25）

### 4.1 NACSA — National Cyber Security Agency

- **2017 年 2 月**成立，是马来西亚**网络安全事务的国家领导机构**。
- 目标：**保障并加强马来西亚面对网络攻击的韧性 (resilience)**，方法是**协调和整合**国家在网络安全领域最好的专家与资源。
- *Extra*：在 Cyber Security Act 2024 下，NACSA 的 Chief Executive 负责监督 **NCII（National Critical Information Infrastructure）** 实体、发布 code of practice、处理事件通报、发放 cyber security service provider 执照。

### ✅ 满分答法 — Tutorial 3 Q7: Discuss the role of NACSA.
NACSA, established in February 2017, is Malaysia's national lead agency for cyber security. Its role is to secure and strengthen Malaysia's resilience against cyber attacks by coordinating and consolidating the nation's best experts and resources, developing national cyber security policies and strategies, and (under the Cyber Security Act 2024) overseeing national critical information infrastructure entities, receiving reports of cyber security incidents and licensing cyber security service providers.

### 4.2 Malaysian cyber security laws（NACSA 列表，p24–25）

Slide 只列名字、叫你自己上网看。下面是每条的一句话重点（*extra，根据法律本身*）：

| Law | 管什么 |
|---|---|
| **Computer Crimes Act 1997** | 把**未授权存取电脑**、未授权存取以进一步犯罪、**未授权修改电脑内容**、错误传播密码/存取码定为犯罪 → 马来西亚版的 CFAA |
| **Copyright (Amendment) Act 1997** | 把 copyright 保护扩展到数位环境（电脑程式、网上传播）→ Ch8 |
| **Digital Signature Act 1997** | 承认**数位签名**的法律效力，规范 certification authorities |
| **Telemedicine Act 1997** | 规范远程医疗的执业（谁可以提供）→ Ch5 |
| **Communications and Multimedia Act 1998** | 规范电讯、广播、网络业；禁止用网络设施发送**不当、虚假、威胁**内容（s.233） |
| **Electronic Commerce Act 2006** | 承认**电子讯息和电子合约**的法律效力 |
| **Electronic Government Activities Act 2007** | 承认政府电子交易与电子文件 |
| **Personal Data Protection Act 2010 (PDPA)** | 规范**商业交易中个人资料的处理**；7 principles：General、Notice & Choice、Disclosure、Security、Retention、Data Integrity、Access |
| **Penal Code** | 一般刑法（诈骗、criminal breach of trust、欺骗）也适用于网络犯罪 |
| **Anti-Fake News (Repeal) Act 2020** | **废除**了 2018 年的 Anti-Fake News Act |
| **Cyber Security Act 2024 (Act 854)** | 见下 |

⚠️ Slide 写 "Communications and Multimedia Act **1988**"——正确是 **1998**。

⚠️ Slide 写 Cyber Security Act 2024 "gazetted on 26 June 2024, **it has not yet been enforced**"——**已过时**。*Extra*：Act 854 已于 **2024 年 8 月 26 日生效**（请以 NACSA 官网为准）。

*Extra*：**PDPA (Amendment) Act 2024** 加入了：**强制 data breach notification**（通知 Commissioner 和受影响的 data subjects）、**必须委任 Data Protection Officer (DPO)**、**data portability** 权利、data processors 直接受 Security Principle 约束、提高罚款。

### 4.3 Cyber Security Act 2024 (Act 854) — 目的（Tutorial 3 Q9，*extra*，整理自 NACSA 网站）

- **加强国家网络安全**，保护 **National Critical Information Infrastructure (NCII)**——对国家安全、经济、公共卫生安全至关重要的系统（如银行金融、政府、能源、交通、医疗、电讯等 sectors）。
- 设立 **National Cyber Security Committee**，并规定 **NACSA Chief Executive** 的职责与权力。
- **NCII sector leads 与 NCII entities 的义务**：遵守 code of practice、进行 **cyber security risk assessment 与 audit**、**向 NACSA 通报 cyber security incidents**。
- **Cyber security service providers 必须持有执照**（如 managed SOC、penetration testing 服务）。
- 违反者可被罚款和/或监禁。

### ✅ 满分答法 — Tutorial 3 Q8: List and describe TWO Malaysian cyber security laws.
1. **Computer Crimes Act 1997** – makes it an offence to gain unauthorised access to computer material, to gain access with intent to commit a further offence, and to make unauthorised modifications to computer contents; it protects systems against hacking and malware.
2. **Personal Data Protection Act 2010** – regulates how organisations process personal data in commercial transactions; organisations must obtain consent, inform individuals why data is collected, keep it secure and accurate, not retain it longer than necessary, and allow individuals to access and correct their data.

---

## 5. *Extra (Part II)*：Identity Theft、Workplace Monitoring、Consumer Profiling

> Tutorial 3 Q10–13 问这些，但本文件夹只有 **Part I** 的 slide。以下根据一般教科书（Reynolds, *Ethics in Information Technology*）整理，**不是 slide 内容**；若之后拿到 Part II slide，以 slide 为准。

### 5.1 Identity theft
**Identity theft** = 盗用别人的个人识别资料（IC 号码、银行帐号、密码），冒充对方取得金钱、贷款或服务。

**三种方法（Tutorial 3 Q10）**：
1. **Data breaches** — 入侵公司资料库偷大量客户资料（或内鬼拷走，如 BrightHome 的 USB 事件）。
2. **Phishing / smishing / vishing** — 冒充银行的 email、SMS、电话，骗受害者输入资料（BrightHome 的 phishing email）。
3. **Spyware / keyloggers** — 恶意软件记录键盘输入或窃取浏览器储存的密码。
（其他：偷手机/钱包、购买被盗资料、shoulder surfing。）

**两种缓解措施（Tutorial 3 Q11）**：
1. **Organisation**：加密个人资料、least-privilege 存取、**禁用 USB/DLP (data loss prevention)**、员工离职时立刻撤销权限、监控异常下载。
2. **Individual/customer**：启用 **multi-factor authentication (MFA)**、不点击 email/SMS 中的连结、直接用官方 app/网址、定期查看银行交易；银行也应进行 **customer awareness** 宣导。

### 5.2 Workplace monitoring（Tutorial 3 Q12）
- **目的**：确保员工把时间用在工作上（productivity）、防止资料外泄与滥用 IT 资源、遵守法规、调查不当行为、保护公司免受骚扰诉讼。
- **主要隐私问题**：监控可能**过度**（读私人讯息、全天定位、键盘记录），员工**不知情或没有同意**，收集的资料被用于工作以外的目的——损害员工的隐私、信任与士气。Ch1 p16 也说：仅凭"访问了某网站"无法判断动机。
- **平衡**：有书面、公开的 monitoring policy；只监控与工作相关的活动；与目的相称 (proportionate)；保护收集到的资料。

### 5.3 Consumer profiling（Tutorial 3 Q13）
- **目的**：公司收集消费者的购买记录、浏览行为、位置、cookies 等资料，建立**profile**，用于**个人化推荐、定向广告、市场分析、定价**。
- **主要隐私问题**：消费者**不知道**被收集了多少、怎么被使用；资料被**卖给或分享给第三方**（如 BrightHome 被偷资料被卖给营销公司）；profile 可能不准确或被用于**歧视性定价**；资料集中也增加 breach 的风险。
- **平衡**：明确告知（notice）、取得同意（consent）、允许 opt-out、只收集必要资料——即 PDPA/GDPR/CCPA 的要求。

---

## 6. BrightHome Bank 情境题 — 满分答法

### ✅ Question 1: Analyse the cyber security weaknesses, identify the stakeholders affected, and evaluate the impacts on the bank, customers and country.

**Weaknesses**
1. **Weak insider-threat controls / excessive access** – an employee with authorised access could download thousands of records, meaning there was no least-privilege access, no data loss prevention and no monitoring of bulk downloads (data protection & identity management failure).
2. **No control over removable media** – USB ports were not blocked or encrypted, allowing data to leave the network.
3. **Poor offboarding procedure** – the resigning employee's activity and access were not reviewed before departure.
4. **Weak customer authentication and awareness** – phishing emails led to account takeovers, suggesting reliance on passwords alone (no MFA) and insufficient customer and staff security awareness.
5. **Sensitive data not protected at rest** – MyKad numbers and income details could be copied in usable form (no encryption/masking).

**Stakeholders:** customers/applicants, the bank (management, employees, shareholders), regulators (Bank Negara Malaysia, Personal Data Protection Commissioner, NACSA), the marketing companies that received the data, and the country/public.

**Impacts**
- **Bank:** financial loss (compensation, investigation, fines under PDPA/Act 854), reputational damage and loss of customer trust, possible legal action, regulatory scrutiny.
- **Customers:** identity theft, financial fraud from hijacked accounts, unwanted targeted marketing, stress and loss of privacy.
- **Country:** as part of critical financial infrastructure, the incident undermines confidence in Malaysia's banking system and digital economy and may be exploited by criminals for wider fraud, affecting national economic security.

### ✅ Question 2: Evaluate whether BrightHome fulfilled its responsibilities in protecting customer information, based on Act 854 and other laws.

**No, the bank did not fully fulfil its responsibilities.**
- **Cyber Security Act 2024 (Act 854):** as an NCII entity in the banking and finance sector, the bank must implement measures under the code of practice, conduct cyber security risk assessments and audits, and report incidents to NACSA. Reporting the incident to NACSA was a correct step, but the fact that an insider and phishing attacks succeeded shows that risk assessment and controls were inadequate.
- **PDPA 2010 – Security Principle:** a data user must take practical steps to protect personal data from loss, misuse, unauthorised access and disclosure. Allowing bulk download to a USB drive breaches this principle.
- **PDPA 2010 – Disclosure / Notice and Choice Principles:** customers' data was used by third parties for advertising without consent. Although the bank did not sell it intentionally, it failed to prevent this.
- **Computer Crimes Act 1997 / Penal Code:** the employee and phishers committed offences (unauthorised access, criminal breach of trust, cheating); the bank should support prosecution.
- **Conclusion:** partially compliant (incident reported) but overall failed its duty of care; it must strengthen controls to meet Act 854 and PDPA obligations.

### ✅ Question 3: As CISO, develop FIVE recommendations.
1. **Least-privilege access and DLP** – restrict customer data to staff who need it; deploy data loss prevention to block and alert on bulk downloads and uploads.
2. **Disable or control USB and removable media** – block USB storage on staff devices or allow only encrypted, company-issued drives, with logging.
3. **Strong offboarding and user monitoring** – review access and activity of staff who give notice; revoke all access on the last working day; use user-behaviour analytics to detect unusual activity.
4. **Multi-factor authentication and anti-phishing measures** – require MFA/secure-device binding for online banking; implement email authentication (SPF, DKIM, DMARC) to reduce spoofing; run customer awareness campaigns.
5. **Encryption and regular risk assessment** – encrypt personal data at rest and in transit, and conduct regular cyber security risk assessments, audits and staff training in line with Act 854 and ISO 27001 (→ Ch6).

---

## Closing the loop

BrightHome 的事件同时违反了技术层面（subdomains）、法律层面（PDPA、Act 854）和人的层面（insider）。法律告诉公司"必须保护资料"，但**公司内部的伦理文化**——员工会不会把资料拷走、看到问题会不会举报、管理层会不会压下来——决定了法律能不能落实。这就是下一章 **Ethics of IT Organisation**：organisational ethics、whistle-blowing、corporate accountability。

---

## ⚠️ Where the slides mislead

| Slide | Slide 说 | 更准确的理解 |
|---|---|---|
| p24 | Communications and Multimedia Act **1988** | 正确是 **1998** |
| p25 | Cyber Security Act 2024 "has not yet been enforced" | 已于 **2024 年 8 月 26 日**生效（extra，以 NACSA 为准） |
| p25 | 马来西亚法律只有名字，叫学生自己看 | 本笔记 §4.2 补上每条的一句话说明 |
| p6/p23 | "Part I" | 本文件夹缺 **Part II**（identity theft、workplace monitoring、consumer profiling），§5 用 extra 内容补上 |

---

## Term table

| English | 中文 | 一句话说明 |
|---|---|---|
| Information system | 资讯系统 | People, procedures, software, hardware, data, Internet |
| Data vs Information | 资料 vs 资讯 | 原始事实 vs 处理后的结果 |
| Cyber security | 网络安全 | 保护 CI 与系统运作的方法与流程 |
| Application security | 应用安全 | Secure coding、input validation |
| Identity management | 身份管理 | 只让合法的人存取 |
| DR&BC | 灾难恢复与业务持续 | 危机中与危机后维持服务 |
| Data-at-rest / in-transit | 静态 / 传输中资料 | SCA vs ECPA |
| CFAA | 电脑诈骗与滥用法 | 美国 cybercrime 法 |
| SCA / ECPA | 储存通讯法 / 电子通讯隐私法 | 储存中 vs 传输中的通讯 |
| HIPAA, PHI, ePHI | 健康保险可携与责任法 | 医疗资料隐私与安全 |
| CCPA | 加州消费者隐私法 | Know, delete, opt-out, non-discrimination |
| GLBA | 金融服务现代化法 | 金融机构资料分享与保护 |
| GDPR | 欧盟一般资料保护规则 | 全球最严，域外适用 |
| PIPL | 中国个人资讯保护法 | 含跨境资料规范 |
| NACSA | 国家网络安全局 | 马来西亚网络安全领导机构 |
| PDPA 2010 | 个人资料保护法 | 马来西亚商业个人资料处理 |
| Cyber Security Act 2024 (Act 854) | 2024 网络安全法 | NCII 保护、事件通报、服务商执照 |
| NCII | 国家关键资讯基础设施 | 银行、能源、政府等关键系统 |
| Identity theft | 身份盗用 | 冒用他人身份资料 |
| Workplace monitoring | 职场监控 | 监控员工 IT 使用 |
| Consumer profiling | 消费者画像 | 分析消费者资料作营销 |

---

## Cheat sheet

- IS 六部分：people, procedures, software, hardware, data, Internet。
- **Cyber security** = methods/techniques/processes 保护 **confidentiality, integrity, functioning**。
- **7 subdomains**：application, data protection & identity mgmt, network, mobile, cloud, DR&BC, awareness & training。
- **Law 目的（5）**：sensitive data, prevent cybercrime, privacy, national security, consumer trust。
- **US**：CFAA（hacking）｜**SCA = at rest**｜**ECPA = in transit**（Title I wiretap, II = SCA, III pen register）｜HIPAA（PHI/ePHI, Privacy vs Security Rule）｜CCPA（know/delete/opt-out/non-discrimination）｜GLBA（金融机构）。
- **GDPR**：最严、2018-05-25、域外适用、须证明合规、巨额罚款、personal data 含 IP/cookies/biometric。
- UK DPA 2018｜China PIPL（跨境）｜India DPDP 2023（域外）。
- **NACSA**：2017-02 成立，国家领导机构，协调专家资源。
- **Malaysia**：Computer Crimes Act 1997、PDPA 2010、**CSA 2024 (Act 854)**、CMA 1998、Digital Signature Act 1997、E-Commerce Act 2006…

---

## Practice (answers included)

### A. MCQ

1. A company's backup server containing old emails is breached. Which US law most directly protects this data?
   (a) ECPA Title I (b) SCA (c) CCPA (d) GLBA
2. Which law applies to a Malaysian online shop that sells to customers in France?
   (a) HIPAA (b) GDPR (c) CFAA (d) GLBA
3. Which cyber security subdomain covers keeping services online during and after a crisis?
   (a) Network security (b) Application security (c) DR&BC (d) Mobile security
4. Under HIPAA, the Security Rule specifically protects:
   (a) all PHI in any form (b) ePHI (c) financial records (d) student records
5. NACSA was established in:
   (a) 1997 (b) 2010 (c) 2017 (d) 2024

**Answers:** 1-b, 2-b, 3-c, 4-b, 5-c

### B. Short answer

**B1. State FIVE purposes of cyber security laws. (5 marks)** Protection of sensitive data; prevention of cybercrime; data privacy; national security; consumer trust.

**B2. List the four consumer rights under the CCPA. (4 marks)** Right to know what personal information is collected and how it is used/shared; right to delete; right to opt out of the sale or sharing of personal information; right to non-discrimination for exercising these rights.

**B3. Why is GDPR relevant to a Malaysian company? (3 marks)** Because GDPR applies to any organisation anywhere that targets or collects data about people in the EU; a Malaysian company serving EU customers must comply and prove compliance or face heavy fines.

### C. Application

*Case: HealthPlus, a Malaysian clinic chain, stores patient records in the cloud and has a mobile app.*

**C1.** Recommend TWO cyber security subdomains HealthPlus should prioritise and justify. (6 marks)
**Cloud security** – patient records are hosted on a cloud platform, so it needs secure cloud architecture, correct access configuration and encryption to prevent exposure. **Mobile security** – patients and doctors use the app on phones, so data on mobile devices must be protected against theft, loss and malware (e.g. app PIN, remote wipe, encrypted storage).

**C2.** A staff member emails patient lists to her personal email. Which Malaysian law and principle are breached? (3 marks)
Personal Data Protection Act 2010 — the Security Principle (failure to protect personal data from unauthorised disclosure) and the Disclosure Principle (disclosed without consent for another purpose).

**C3.** Explain the purpose of the Cyber Security Act 2024 for an organisation like a national hospital group. (4 marks)
If designated as an NCII entity (healthcare sector), it must follow NACSA's code of practice, conduct regular cyber security risk assessments and audits, and report cyber security incidents to NACSA; the Act aims to protect critical national systems and strengthen Malaysia's resilience against cyber threats.

### D. Thinking

**D1.** The US uses many sector-specific laws while the EU uses one comprehensive law. Which is better? (4 marks)
A comprehensive law (GDPR) gives consistent protection for all personal data and is easier for individuals to understand, but may be costly for small businesses. Sector-specific laws (HIPAA, GLBA) are tailored to each industry but leave gaps — much data held by private companies is not covered. Overall, a comprehensive baseline with sector rules on top gives the best protection.

**D2.** Is workplace monitoring ethical? (4 marks)
It can be, if it has a legitimate purpose (security, productivity, compliance), is disclosed to employees in a written policy, is limited to work-related activity and is proportionate. It becomes unethical when it is secret, excessive (e.g. reading personal messages) or used for purposes other than those stated.

---

## Slide index

| Note section | Slides |
|---|---|
| 0' Parts of IS | p2–4 |
| 1 Cyber security & subdomains | p7–9 |
| 2 Law purposes & US laws | p10–19 |
| 3 GDPR & others | p20–22 |
| 4 Malaysia | p23–25 |
| 5 Part II topics | *not in slides* (Tutorial 3 Q10–13) |
| 6 BrightHome | *Scenario Based question.pdf* |

## Links to other chapters
- Attacks these laws target → **Ch2**
- Organisation rules, whistle-blowing, Whistleblower Protection Act 2010 → **Ch4**
- Privacy of health data, cookies, facial recognition, FERPA → **Ch5**
- CIA triad, risk assessment, ISO 27001 / NIST → **Ch6**
- Copyright Act, cybersquatting → **Ch8**
