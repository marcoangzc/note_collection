# AMIS1012 — Chapter 2: Computer and Internet Crime

> Slide 列了很多攻击名称，但没有告诉你**怎样分辨 passive 和 active**，Bob–Alice–Darth 的图也没有解释。而且 p14 定义 passive attack 时混进了一句 active attack 的定义（"A successful attack tries to change the system's resources"），很容易读错。spam、virus、worm、trojan、botnet、DDoS、rootkit 是**每年 Q1 的 6–10 分**，slide 却没有给比较表。这份笔记补上：一个判断规则、四张图的解释、malware 比较表、以及 Tutorial 2 全部 10 题的答案。

---

## 0. 一句话总览

**Cybercrime 是用电脑做武器或目标的非法行为；系统越复杂、越依赖商业软件，漏洞（exploit）越多；攻击分两种——passive（只偷看、不改动）和 active（篡改、冒充、瘫痪）；发动攻击的人从 ethical hacker 到 cracker、malicious insider、cybercriminal、hacktivist、cyber terrorist，动机各不相同。**

| Part | 内容 | Slide |
|---|---|---|
| 1 | Introduction to cybercrime（定义、分类、例子） | p4–6 |
| 2 | Computer user expectations + 漏洞增加的原因 | p7–11 |
| 3 | Reliance on commercial software（exploit、patch） | p12 |
| 4 | **Security attacks**：passive vs active；virus, worm, trojan, botnet, DDoS, rootkit, spam, phishing | p13–39 |
| 5 | Hackers and crackers | p40–41 |
| 6 | Malicious insider | p42 |
| 7 | Cybercriminals, hacktivists, cyber terrorists | p43–45 |

🎯 **这章回答的考题**
- Define cybercrime — Jan 2025 Q1b (2)
- TWO types of cybercrime — May 2026 Q1d (6)
- TWO factors → rise of computer-related vulnerabilities — Jan 2025 Q1c (6)
- Define exploits + how developers respond — Oct 2024 Q1c (4)
- Passive attacks / active attacks — May 2025 Q1b (3+3)、May 2026 Q1c (4)、Oct 2024 Q1e 被动攻击的目的 (1)
- Active attacks：trojan / DDoS / rootkit — Oct 2024 Q1d (6)；virus / rootkit / DDoS — Oct 2025 Q1b (6)；THREE examples — Jan 2025 Q1d (9)
- **FIVE types of spam** — May 2025 Q1c (10)、Oct 2025 Q1c (10)

**Prerequisites**：Ch1 的 ethics vs law——这章的人全部已经越过了 law 那条线（ethical hacker 除外）。

---

## Scene：Bob 给 Alice 的一笔转账

Bob 在网上银行转 RM500 给 Alice。中间有一个攻击者 **Darth**（slide 的图就用这三个人）。

- Darth 可以只是**偷听**这笔交易的内容 → 他知道了 Bob 的账号。
- Darth 也可以**把 RM500 改成 RM5,000**，或者**冒充 Bob** 发转账指令，或者**把 Bob 的请求录下来重发十次**，或者**让银行系统瘫痪**让谁都转不了账。

前一种不改变任何东西，后面几种都动了手。这就是 passive 和 active 的分界线。但在讲攻击之前，先问：为什么这种事越来越多？

---

## 1. Introduction to Cybercrime（p4–6）

- **Cybercrime** = a form of **illegal activity that takes place over the Internet**; any criminal operation carried out using modern technology。
- 另一个说法：unlawful Internet operation in which a **device is used as a weapon, a target, or both**。
  - 电脑是 **weapon**：用电脑发 phishing email 骗钱。
  - 电脑是 **target**：入侵银行 server 偷资料。
  - **Both**：用 botnet（一堆被感染的电脑 = 被当成 target）去 DDoS 另一个网站（= 当成 weapon）。
- **两种分类**：
  - **Malicious harm**（恶意伤害）——故意的，如偷资料、散播病毒赚钱。
  - **Accidental harm**（意外伤害）——例：不满的员工写了一个他以为"无害"的 virus，结果造成公司运作中断；虽然没有直接偷钱，但**浪费工时和修复资源**，仍然有财务损失。
- 例子：illegal entry / intrusion / computer abuse（攻击 IT 基础设施）、**data theft**、**cyber bullying**（威胁、强迫、针对受保护群体的仇恨）、**散播 worms/viruses**、为了金钱/个人资料而 hacking、unintentional damage。

> 💬 **答题句 (EN)** — *Cybercrime is any illegal activity carried out using computers or the Internet, in which a device is used as a weapon, a target, or both. It can be classified into cybercrimes that cause malicious harm and those that cause accidental harm.*

### ✅ 满分答法 — Jan 2025 Q1b: Define cybercrime. (2 marks)
Cybercrime is any illegal activity carried out over the Internet or using computer technology (1), in which a computer or device is used as the weapon, the target, or both (1).

### ✅ 满分答法 — May 2026 Q1d: Explain TWO (2) types of cybercrime. (6 marks)

"Types" 最符合 slide 的是 **malicious vs accidental harm**；每个 3 分（定义 + 特点 + 例子）。

1. **Cybercrime that causes malicious harm** – crimes committed deliberately with the intention of damaging, stealing or profiting. The attacker plans the act and targets victims for money, revenge or political aims. *Example:* a cybercriminal hacks an online store's database to steal customers' credit card details and sells them.
2. **Cybercrime that causes accidental harm** – harm that the offender did not fully intend, but which still disrupts operations and causes financial loss through wasted work hours and recovery costs. *Example:* a dissatisfied employee releases a "harmless" prank virus that unexpectedly crashes the company's email server for a day.

（若想用另一种分法，也可写"computer as a target"（hacking, DDoS）vs "computer as a weapon"（phishing, online fraud, cyber bullying）——两种都合理，但 slide 用的是 malicious / accidental。）

---

## 2. Computer User Expectations & 漏洞增加的原因（p7–11）

用户的期待本身就在制造漏洞：

- 电脑让收集资料**更快、更便宜**，所以收集得**更多、更多样**；每一笔修改都能记录，**audit trail metadata** 变得非常大 → 资料越多，被偷的价值越高。
- 用户希望问题**马上解决** → IT help desk 压力大，**有时跳过身份验证或权限确认**（social engineering 的机会）。
- 用户把自己的 login credentials **借给同事** → 未授权存取。

**Factors contributing to the spike in computer-related vulnerability incidents（p10–11）**：

1. **Increasing complexity increases vulnerability** — 数以亿计的 code 连接电脑、网络、OS、软件、网页、switches、routers、gateways；**连接的设备越多，entry points 越多**；系统不断扩张和改变，就不断带来新风险。
2. **Rapid pace of technological change** — 要跟上技术变化、持续评估新的安全威胁、并实施应对方法，**极其困难**。
3. （p9 延伸）**Higher user expectations** — 为了快速服务而忽略验证、共用密码。
4. （p12 延伸）**Reliance on commercial software with known vulnerabilities** — 见下一节。

> 💬 **答题句 (EN)** — *Computer-related security incidents have increased because growing system complexity creates more entry points for attackers, the rapid pace of technological change makes it hard to assess and respond to new threats, users' expectation of fast support leads help desks to skip identity checks, and organisations rely on commercial software with known vulnerabilities.*

### ✅ 满分答法 — Jan 2025 Q1c: Describe TWO factors that contribute to the rise of computer-related vulnerabilities. (6 marks)

1. **Increasing complexity of computing environments** – modern systems link computers, networks, operating systems, applications, web pages, routers and gateways through hundreds of millions of lines of code. The more devices connected, the more potential entry points an attacker can exploit, and every expansion or change introduces new risks. *Example:* a company that connects IoT cameras to its network adds devices with weak default passwords.
2. **The rapid pace of technological change** – new technologies and attack methods appear faster than organisations can evaluate them. Continuously assessing new threats and implementing defences is extremely difficult, so gaps remain. *Example:* a company adopts a new cloud service before its security team understands how to configure it, leaving storage publicly accessible.

（其他可接受的 factor：users' expectations of fast help-desk service / password sharing；reliance on commercial software with known vulnerabilities。）

---

## 3. Reliance on Commercial Software：Exploit 与 Patch（p12）

- **Exploit** = a computer attack that **takes advantage of a flaw (vulnerability) in the system**；常见原因是 **poor system design or implementation**。
- 发现 security flaw 后，software engineers 会**迅速开发并发布 fix / patch**。**如果用户不安装 patch，就有被入侵的风险。**
- 补充（extra，不在 slide）：还没有 patch 时就被利用的漏洞叫 **zero-day exploit**——这时开发者要先发 workaround / advisory。

### ✅ 满分答法 — Oct 2024 Q1c / Tutorial 2 Q4: Define "exploits" and explain how a software developer responds to exploits. (4 marks)

- **Definition (2):** An exploit is a computer attack that takes advantage of a vulnerability (flaw) in a system's design or implementation in order to gain unauthorised access or cause damage.
- **Response (2):** When a flaw is discovered, software developers quickly analyse it, develop a fix called a **patch**, and distribute it to users (e.g. through automatic updates), advising them to install it immediately, because systems that are not patched remain exposed to a security breach.

---

## 4. Security Attacks（p13–39）⭐ 本章重心

### 4.1 Passive vs Active：一条判断规则

> **有没有改动资料或影响系统运作？** 没有 → **passive**（只看/只听）；有 → **active**（改、冒充、重发、瘫痪）。

| | Passive attack | Active attack |
|---|---|---|
| 做什么 | 取得/使用资讯，**不伤害系统资源** | **篡改 data stream 或制造假的 stream**，影响系统 |
| 目的 | **Intercept information**（窃取资讯） | 修改、冒充、破坏、阻断服务 |
| 受害者知道吗 | 通常**不知道**，很难察觉（资料没变） | 通常会察觉（资料变了/服务停了） |
| 防御重点 | **Prevention (avoidance)**，如 **encryption / cryptography** | **Detection & recovery** |
| 例子 | Eavesdropping, traffic monitoring (traffic analysis) | Masquerade, replay, message alteration, DoS；virus, worm, trojan, botnet, DDoS, rootkit, phishing |

⚠️ **Slide p14 的陷阱**：在 passive attack 的定义里，slide 写了 "A successful attack tries to change the system's resources or disrupt its activity"——这句其实是 **active attack** 的定义，被误放在这里。考试**不要**把这句写进 passive 的答案里。

> 💬 **答题句 (EN)** — *A passive attack attempts to learn or use information from a system without altering system resources, so it is difficult to detect and is best handled by prevention such as encryption. An active attack modifies the data stream or creates a false stream, affecting system resources or operations, e.g. masquerade, replay, message modification and denial of service.*

### 4.2 Passive attacks（p14–17）

**(a) Eavesdropping（窃听）** — Darth 偷听 Bob 与 Alice 之间的电话、email、传输的档案。

```
                        [ Darth ]
                           ^
                           |  reads contents of
                           |  message from Bob to Alice
  [ Bob ] ------------> ( Internet or Other ) ------------> [ Alice ]
                        (  Comms Facility   )
```
*看图重点：箭头从中间"分叉"到 Darth，但 Bob→Alice 的讯息完全没被改变——所以是 passive。*

**(b) Traffic monitoring / traffic analysis（流量监测）** — 即使内容被加密，Darth 仍能观察**谁在传、传给谁、多频繁、多久**。

```
                        [ Darth ]
                           ^
                           |  observe PATTERN of
                           |  messages from Bob to Alice
  [ Bob ] ------------> ( Internet or Other ) ------------> [ Alice ]
                        (  Comms Facility   )
```
*看图重点：Darth 看的是"模式"（时间、频率、身份），不是内容。例如公司高层突然和某律师事务所密集通讯，可能暗示收购。*

- 很难察觉（资料没变）；sender/receiver 不知道有第三者。
- **Cryptography** 可以防止成功（至少保护内容）。

**Passive attack 的主要目的（Oct 2024 Q1e，1 分）**：*to intercept / obtain the information being transmitted (e.g. confidential company data) without being detected.*

### 4.3 Active attacks — 4 groups（p18–21）

**(a) Masquerade（冒充）** — 一个实体假装是另一个（例：权限低的用户冒充权限高的用户来取得更多 privileges）。

```
                        [ Darth ]
                           |
                           |  message from Darth
                           |  that APPEARS to be from Bob
                           v
  [ Bob ]              ( Internet or Other ) ------------> [ Alice ]
                       (  Comms Facility   )
```
*看图重点：箭头方向反过来——是 Darth **发出**讯息，Alice 以为来自 Bob。*

**(b) Replay（重放）** — 被动地录下资料，**之后重新发送**以得到未授权的结果。

```
                        [ Darth ]
                        ^       \
     (1) capture message|        \ (2) later replay
         from Bob to    |         \    message to Alice
         Alice          |          v
  [ Bob ] ------------> ( Internet or Other ) ------------> [ Alice ]
                        (  Comms Facility   )
```
*看图重点：先 passive 地录（1），再 active 地重发（2）。Bob 转账 RM500 一次，Darth 重发十次 → Alice 收到十笔。*

**(c) Message alteration（修改讯息）** — 合法讯息的一部分被修改，或被**延迟、重新排序**（例："Allow John to read file X" 改成 "Allow Darth to read file X"；RM500 改成 RM5,000）。

**(d) Denial-of-service (DoS)** — 用大量假请求/资料淹没系统，让服务、网站或 server **无法使用**。

### 4.4 Malware 与其他 active attacks（p22–31）

先看比较表，再逐个解释：

| Attack | 需要 host program？ | 会自我复制？ | 需要人类动作传播？ | 主要行为 |
|---|---|---|---|---|
| **Virus** | ✅ 附在档案/程式上 | ✅ | ✅ 用户打开档案/附件 | 执行 **payload**：从烦人到删档、造成 DoS |
| **Worm** | ❌ 独立程式 | ✅ 在 active memory 复制 | ❌ 自动传（如 email 自己） | 利用漏洞或 social engineering 快速扩散 |
| **Trojan horse** | 伪装成合法程式 | ❌ **不会**自我复制 | ✅ 骗用户安装 | 安装后破坏、窃取、开后门 |
| **Botnet** | — | — | — | 被感染（常经 Trojan）的电脑群，被 **command & control** 远程操控 |
| **DDoS** | — | — | — | 用 botnet 大量流量淹没目标 |
| **Rootkit** | — | — | — | 一组工具让攻击者**远程存取和操控**设备（执行程式、看 log、监视、改设定） |

**Virus（p22–24）**
- 一段让电脑以**意外、通常不愉快的方式**运作的 code；多数带有 **payload**（恶意内容）。
- 传播：打开被感染的 email 附件、安装被感染的程式、访问被感染的网站；通过网络、磁碟、file sharing 传给其他电脑——**靠被感染电脑用户的动作**传播。
- 例子：**Macro virus**——用应用程式的 macro language（Visual Basic, VBScript）写的；感染应用程式后，**插入之后每一份用它开的文件**；收件人点开可能删除硬碟资料。

**Worm（p25）**
- 在电脑 **active memory 中复制自己**的有害软件。
- 与 virus 的差别：**不需要人帮忙、不需要 host program**，可以自己把 copy email 给其他电脑；利用目标设备的漏洞，或用 social engineering 让用户执行。

**Trojan horse（p26）**
- **伪装成真的程式或档案**，骗你安装；之后执行它被设计的目的（破坏、干扰、窃取）。
- **不能自我复制**（和 virus、worm 不同）。
- 例：一个"免费 PDF 转换器"其实会偷浏览器存的密码。

**Botnet（p27）**
- 一群**连上 Internet、被 malware 感染、可被 hacker 控制**的电脑。
- Cybercriminal 用特定 **Trojan** 感染，再装 **command and control (C&C)** 软件来进行大规模破坏。
- 每一台叫 **bot / zombie**。

**DDoS — Distributed Denial of Service（p28–30）**
- 协调地用**大量 Internet traffic 淹没**目标 server/网络或其周边网络，扰乱正常运作。
- 流量来自**大量被入侵的机器**（包括 IoT 设备）组成的 botnet；攻击者远程下指令，每个 bot 向目标 IP 发请求 → 超载 → DoS。
- **难以分辨攻击流量和正常流量**，因为每个 bot 都是真实的 Internet 设备。

```
  [Attacker PC]──┐
  [Attacker PC]──┤ (malicious traffic)            ┌─[Server]
                 ├──────> [ Internet ] ──────────>├─[Server]  !! overloaded
  [Actual user]──┤ (normal traffic)               └─[Server]
  [Actual user]──┘
```
*看图重点（p30 GlobalSign 图）：攻击流量和正常用户流量混在一起到达 target server——server 分不出来，所以正常用户也被拒绝服务。*

**DoS vs DDoS**：DoS 来自一个来源；DDoS 来自**分布在各地的很多来源（botnet）**，更难阻挡。

**Rootkit（p31）**
- 一组软件工具，让 threat actor **远程存取和操控**设备。
- 可执行程式、存取 logs、监视用户活动、改电脑设定。
- 补充（extra）：rootkit 通常会**隐藏自己和其他 malware**，一般 antivirus 难以发现，常需重装 OS。

### ✅ 满分答法 — Oct 2024 Q1d: Discuss (i) trojan horse (ii) DDoS attack (iii) rootkit. (2 marks each)

- **(i) Trojan horse** – malware disguised as a legitimate program or file that tricks the user into installing it; once installed it damages, steals or disrupts data. Unlike viruses and worms, it cannot replicate itself. *e.g.* a fake "free game" that installs a keylogger.
- **(ii) DDoS attack** – a coordinated attempt to make a server or network unavailable by flooding it with traffic from a large number of compromised machines (a botnet). Because every bot is a real device, attack traffic is hard to distinguish from normal traffic. *e.g.* thousands of infected IoT cameras flood an online bank's website during a sale.
- **(iii) Rootkit** – a set of software tools that allows an attacker to gain remote, hidden access to and control of a computer, so they can run programs, access logs, monitor user activity and change settings. *e.g.* an attacker installs a rootkit on a company server to secretly monitor administrators' activity.

### ✅ 满分答法 — Oct 2025 Q1b: Discuss (i) Virus (ii) Rootkit (iii) DDoS with an example each. (2 marks each)

- **(i) Virus** – malicious code attached to a file or program that spreads when the user opens or runs the infected item, and carries a payload that makes the computer behave abnormally (from annoying messages to deleting data). *Example:* a macro virus hidden in a Word attachment deletes files when the recipient opens it.
- **(ii) Rootkit** – see above. *Example:* an attacker uses a rootkit to remotely change a victim's firewall settings and read their logs without being noticed.
- **(iii) DDoS** – see above. *Example:* a botnet sends millions of requests to an e-commerce site so real customers cannot check out.

### ✅ 满分答法 — Jan 2025 Q1d: Explain THREE examples of active security attacks. (9 marks)

3 × 3 分（what it is + how it works + example）。挑最好写的三个：

1. **Masquerade** – one entity pretends to be another in order to gain privileges it is not entitled to. The attacker sends messages that appear to come from a trusted user. *Example:* an attacker uses a stolen staff ID and password to log in as an HR manager and approve fake payroll changes.
2. **Replay** – the attacker passively captures a valid data transmission and retransmits it later to produce an unauthorised effect. *Example:* capturing a customer's "transfer RM500" request and replaying it several times so the money is transferred repeatedly.
3. **Denial of Service (DoS / DDoS)** – the attacker floods a system with fake requests so that the service becomes unavailable to legitimate users; in a DDoS the traffic comes from a botnet of compromised machines. *Example:* a botnet floods a university's online registration server on enrolment day.

（也可用 message alteration、virus、worm、trojan、rootkit。）

### 4.5 Spam（p32–35）⭐ 10 分题

- **Spam** = **unrequested (unsolicited)**、通常是**推广性质**、**大量发送**的讯息——不管你有没有要求都会收到。
- 它为什么被归在 "active attacks"：spam 常被用来**散播 malware 和 phishing link**，而且主动地塞进你的收件箱（这是 slide 的分类；严格说 spam 本身是一种滥用，不是传统的 active attack）。

**五种 spam（slide 刚好五种，考试照这五种写）**：

| Type | 特点 | 怎么运作 | 例子 |
|---|---|---|---|
| **1. Email spam** | 最标准的 spam | 大量发送 unsolicited email，塞满邮箱、分散注意力；常夹带广告、恶意附件、phishing link | "You've won an iPhone! Click here" |
| **2. Social media spam** | 利用社交网络的"社交性" | 用**假帐号 (fake profiles)** 在热门社交平台发贴文、留言、私讯 | 假帐号在 Instagram 贴文下面留"投资赚钱"连结 |
| **3. Mobile phone spam** | SMS 形式 + **push alerts** | 发 SMS；有些用 app 的 push notification 吸引注意 | SMS："RM0 Loan approved, reply YES" |
| **4. Text message (instant messaging) spam** | 像 email spam 但**快得多** | 通过 **WhatsApp、Telegram** 等 IM app 群发 | WhatsApp 群组突然收到陌生人的兼职广告 |
| **5. SEO spam (spam indexing)** | 针对搜寻引擎 | 用 **SEO 技巧**（关键字堆砌、垃圾外链、隐藏文字）把 spammer 的网站**在搜寻结果中排高** | 搜"cheap flights"排第一的是骗局网站 |

⚠️ Slide 的 "mobile phone spam" 和 "text message spam" 很像。区分：**mobile phone spam = SMS + push alerts（电讯商/手机系统层面）**；**text message spam = 透过 IM apps（WhatsApp/Telegram）**。写答案时把这个区别写出来。

> 💬 **答题句 (EN)** — *Spam is unsolicited, usually promotional, messages sent in bulk to a large number of recipients. Common types are email spam, social media spam, mobile phone (SMS/push) spam, instant-messaging text spam, and SEO spam (spam indexing).*

### ✅ 满分答法 — May 2025 Q1c / Oct 2025 Q1c: List and describe / Explain FIVE common types of spam (characteristics + how it functions). (10 marks)

5 × 2 分（特点 1 + 运作方式/例子 1）：

1. **Email spam** – the standard form of spam: unsolicited bulk emails, usually advertisements. Spammers send the same message to millions of addresses at almost no cost; it clogs the inbox, distracts users from wanted emails and often carries malicious links or attachments.
2. **Social media spam** – spam spread through social networking sites. Spammers create fake profiles and use them to post comments, messages or links on popular platforms, taking advantage of the social, trusting nature of these sites.
3. **Mobile phone spam** – spam delivered as SMS messages to mobile phones; some spammers also use app push notifications to draw attention to their offers. Users receive it directly on their phone whether they asked for it or not.
4. **Text message (instant messaging) spam** – similar to email spam but much faster; spammers use instant-messaging apps such as WhatsApp and Telegram to broadcast promotional or scam messages to individuals and groups.
5. **Search engine optimisation (SEO) spam / spam indexing** – the misuse of SEO techniques (e.g. keyword stuffing, link farms) to push the spammer's website to the top of search results, so users are misled into visiting low-quality or malicious sites.

### 4.6 Phishing（p36–39）

- **Phishing** = 一种 **social engineering** 攻击，骗取敏感资讯（login password、信用卡资料）。
- **Social engineering** = hacker 扮成**可信的人**，说服受害者开帐号、发讯息等。

**Phishing 的技巧（p37–38）** —— Tutorial 2 Q9 "TWO techniques to create convincing phishing emails"：

1. **Phishing scams through email (mimicking a real business)**
   - 用**一样的措辞、字体、logo、签名**，看起来像真的公司 email；
   - 制造 **sense of urgency**（"24 小时内不验证就冻结帐户"）；
   - 讯息里的连结**模仿真网址**，但**拼错 domain** 或加多余 subdomain（`maybank2u.com.secure-login.xyz`）。
2. **Spear phishing（phishing with a specific purpose）**
   - 针对**单一个人或公司**；需要深入了解该组织的权力结构（谁批准付款），所以更可信。

**Phishing kit 的生命周期（p39，图）**：

```
 (1) Legitimate website     (2) Login page changed to     (3) Modified files bundled
     is cloned        ───>      point to a credential- ───>   into a ZIP file to make
                                stealing script                a phishing kit
                                                                      │
                                                                      v
 (5) Emails sent with links  <───  (4) Phishing kit uploaded to a hacked
     pointing to the new               website; files are unzipped
     spoofed website
```
*看图重点：攻击者不是自己架站，而是**入侵别人的网站**来放假登入页——所以网址看起来"正常"。*

---

## 5. Hackers and Crackers（p40–41）

| | Ethical hacker | Cracker (malicious hacker) |
|---|---|---|
| 目的 | 测试网络安全、找出漏洞，**帮公司保护资料** | 恶意入侵、破坏、牟利 |
| 授权 | **有授权**，与公司合作 | **未授权** |
| 资格 | 持有 license/证书如 **CEH (Certified Ethical Hacker)** | 无 |
| 发现漏洞后 | 报告并协助修补 | 删除/破坏记录、拒绝合法用户服务 |
| 对企业的影响 | ✅ 提前发现弱点、减少被攻击风险、符合法规 | ❌ 资料损失、停机、财务损失、名誉受损、法律责任 |

### ✅ 满分答法 — Tutorial 2 Q8: Distinguish between an ethical hacker and a cracker, and how each impacts a business.
An **ethical hacker** is an authorised security professional (often CEH-certified) who uses hacking tools to test an organisation's systems, find vulnerabilities and report them so they can be fixed; this strengthens the business's security and prevents future losses. A **cracker** is a malicious hacker who breaks into systems without authorisation to destroy records, deny service to legitimate users or profit illegally; this causes data loss, downtime, financial loss and reputational damage to the business.

---

## 6. Malicious Insider（p42）

- **现任或前任员工、承包商、商业伙伴**，**故意超越或滥用被授权的存取权限**，危害资料的 **confidentiality, integrity, or availability**（→ Ch6 的 CIA triad）。
- 诈骗常需要**内部人士与外部第三方**勾结。
- 为什么危险：他们**本来就有合法权限**，防火墙挡不住。→ 对应 Ch4 的 TechNova 和 BrightHome 场景（员工把客户资料拷到 USB）。

---

## 7. Cybercriminals, Hacktivists, Cyber Terrorists（p43–45）

| 类型 | 动机 | 手法/例子 |
|---|---|---|
| **Cybercriminal** | **金钱** | 入侵企业 server 转帐；偷卖信用卡号、姓名、手机 ID；花大钱**收买不诚实的内部人员** |
| **Hacktivist** | **政治或社会目标** | 用电脑攻击（slide 说 malware）推动政治/社会议程，威胁或逼迫一个国家；如 defacing 政府网站、DDoS 抗议 |
| **Cyber terrorist** | **更激进**：造成伤害、恐慌 | 摧毁或中断网络以造成破坏（而非收集情报）；例：入侵**水坝、航空交通管制**，威胁生命与国家安全 |

⚠️ Slide p45 说 "there has yet to be a single case of true cyber terrorism"——这是课本较旧的说法，学界对"是否已发生"有争议；考试可以照写，不要当成定论。

### ✅ 满分答法 — Tutorial 2 Q10: Is there any difference between a hacktivist and a cybercriminal?
Yes — the difference is **motivation**. A cybercriminal attacks systems mainly for **financial gain**, e.g. stealing and reselling credit card data or transferring funds. A hacktivist attacks systems to promote a **political or social cause**, e.g. defacing a government website or launching a DDoS against a company whose policies they oppose. Both may use similar techniques and both are illegal, but a hacktivist's goal is publicity and pressure rather than money.

---

## Tutorial 2 — 其余题目答案

**Q1. What is cybercrime?** → 见 §1 答题句。

**Q2. How can cybercrime be classified? Examples.** → Malicious harm（偷卡号、勒索）vs accidental harm（员工的"恶作剧"病毒导致停机）。

**Q3. Why have computer-related security incidents increased?** → 见 §2（complexity、rapid change、user expectations、reliance on commercial software with vulnerabilities）。

**Q5. Differentiate passive and active attacks, one example each.** → 见 §4.1 表格：passive = eavesdropping；active = masquerade / DDoS。

**Q6. Classify (i) Botnet (ii) Eavesdropping.**
- (i) **Botnet → active**：被感染的电脑被远程控制去发动攻击（如 DDoS、发 spam），会改变/影响系统。
- (ii) **Eavesdropping → passive**：只是偷听，不改动资料。

**Q7. Describe FIVE types of spam.** → §4.5。

**Q9. TWO techniques for convincing phishing emails.** → §4.6（mimic real business with same wording/logos/urgency/look-alike links；spear phishing with in-depth knowledge of the target）。

---

## Closing the loop

回到 Bob、Alice 和 Darth：Darth 只偷听是 passive，改金额、冒充、重放、瘫痪都是 active；他可能用 phishing 骗 Bob 的密码、用 botnet 发动 DDoS。他可能是为钱（cybercriminal）、为理念（hacktivist），也可能根本是银行的内部员工（malicious insider）。

下一章问：**法律怎么管这些事？** 美国、欧洲、马来西亚分别用哪些 cyber security laws 来保护资料和惩罚这些人。

---

## ⚠️ Where the slides mislead

| Slide | Slide 说 | 更准确的理解 |
|---|---|---|
| p14 | Passive attack 定义中写 "A successful attack tries to change the system's resources or disrupt its activity" | 这句是 **active** attack 的特征；passive 攻击**不**改变系统资源 |
| p32–39 | Spam 和 phishing 放在 "active attacks" 标题下 | 这是 slide 的分类；严格来说 spam 是滥用通讯，phishing 是 social engineering。考试照 slide 写，但不要说 spam "modifies data" |
| p34 | Mobile phone spam vs text message spam 定义几乎重叠 | 分成 SMS/push（mobile）与 WhatsApp/Telegram（IM text）两类来写 |
| p44 | Hacktivism is the use of **malware** | 更准确是 "use of hacking / computer attacks"（DDoS、defacement 不一定用 malware） |
| p45 | 还没有一宗真正的 cyber terrorism | 旧说法，有争议；照写但知道是 textbook 观点 |

考试策略：若题目引用 slide 原句，照 slide 答，再补一句更准确的说明。

---

## Term table

| English | 中文 | 一句话说明 |
|---|---|---|
| Cybercrime | 网络犯罪 | 用电脑/网络进行的非法活动 |
| Vulnerability | 漏洞 | 系统的弱点 |
| Exploit | 漏洞利用 | 利用漏洞的攻击 |
| Patch | 修补程式 | 修补漏洞的更新 |
| Passive attack | 被动攻击 | 只窃取资讯、不改动系统 |
| Active attack | 主动攻击 | 篡改、冒充、破坏 |
| Eavesdropping | 窃听 | 偷听通讯内容 |
| Traffic analysis / monitoring | 流量分析 | 观察通讯模式 |
| Masquerade | 冒充 | 假装是别人 |
| Replay | 重放 | 录下再重发 |
| Message alteration | 讯息篡改 | 修改/延迟/重排讯息 |
| Denial of Service (DoS) | 阻断服务 | 让服务无法使用 |
| DDoS | 分散式阻断服务 | 用 botnet 发动 DoS |
| Virus / Worm / Trojan horse | 病毒 / 蠕虫 / 木马 | 见比较表 |
| Payload | 有效载荷 | 恶意程式真正做坏事的部分 |
| Botnet / bot / zombie | 殭尸网络 / 殭尸电脑 | 被远程控制的感染电脑群 |
| Rootkit | Rootkit | 让攻击者远程隐藏控制的工具组 |
| Spam | 垃圾讯息 | 未经请求的大量讯息 |
| SEO spam / spam indexing | 搜寻引擎垃圾 | 作弊提高搜寻排名 |
| Phishing / spear phishing | 网络钓鱼 / 鱼叉式钓鱼 | 骗取敏感资料；针对特定对象 |
| Social engineering | 社交工程 | 利用人性骗取资讯 |
| Ethical hacker / Cracker | 道德黑客 / 骇客 | 授权测试 vs 恶意入侵 |
| Malicious insider | 恶意内部人员 | 滥用合法权限的员工/伙伴 |
| Hacktivist | 黑客行动主义者 | 为政治/社会目的攻击 |
| Cyber terrorist | 网络恐怖分子 | 为造成恐慌与破坏而攻击 |

---

## Cheat sheet

- **Cybercrime**：device = weapon / target / both；分 **malicious harm** vs **accidental harm**。
- **漏洞增加**：complexity ↑ entry points；rapid technical change；user expectations（help desk 跳过验证、共用密码）；commercial software flaws。
- **Exploit** = 利用漏洞的攻击；对策 = **patch**，不装就危险。
- **Passive**（不改动，目的 = intercept，靠 **encryption** 预防）：eavesdropping, traffic monitoring。
- **Active 4 groups**：masquerade, replay, message alteration, DoS。
- Virus（需 host + 人）/ Worm（独立、自传）/ Trojan（伪装、**不复制**）/ Botnet（zombies + C&C）/ DDoS（botnet 淹没，难分辨）/ Rootkit（远程隐藏控制）。
- **5 spam**：email, social media, mobile phone (SMS/push), text message (WhatsApp/Telegram), SEO spam。
- **Phishing**：email 仿冒（same look + urgency + misspelled link）；spear phishing（针对个人/公司）；phishing kit 5 步。
- **Hackers**：ethical hacker（CEH，授权）vs cracker（恶意）。**Insider**：滥用合法权限 → 危害 CIA。
- **Motives**：cybercriminal = money；hacktivist = political/social；cyber terrorist = damage/fear。

---

## Practice (answers included)

### A. MCQ

1. Which malware disguises itself as a legitimate program and cannot replicate itself?
   (a) Worm (b) Virus (c) Trojan horse (d) Botnet
2. An attacker records a valid login message and sends it again later. This is:
   (a) Masquerade (b) Replay (c) Eavesdropping (d) Traffic analysis
3. Which spam type manipulates search rankings?
   (a) Email spam (b) Social media spam (c) SEO spam (d) Mobile spam
4. The main defence against passive attacks focuses on:
   (a) detection (b) prevention, e.g. encryption (c) recovery (d) patching
5. A former employee uses an account that was never disabled to copy customer data. This person is a:
   (a) hacktivist (b) ethical hacker (c) malicious insider (d) cyber terrorist

**Answers:** 1-c, 2-b, 3-c, 4-b, 5-c

### B. Short answer

**B1. Differentiate a virus from a worm. (4 marks)**
A virus attaches itself to a host file or program and spreads only when a user opens or runs the infected item. A worm is a standalone program that replicates itself in memory and spreads automatically across networks (e.g. by emailing copies of itself) without human action or a host program, exploiting system flaws.

**B2. Explain how a botnet is used to launch a DDoS attack. (4 marks)**
The attacker infects many Internet-connected devices (PCs, IoT) with Trojan malware and installs command-and-control software, turning them into bots. On command, all bots send large numbers of requests to the victim's IP address simultaneously, overwhelming the server or network so that legitimate users are denied service; since each bot is a genuine device, the traffic is hard to filter.

**B3. What is the main objective of a passive attack, and why is it difficult to detect? (3 marks)**
Its objective is to intercept or obtain the information being transmitted. It is hard to detect because the attacker does not alter the data or the system, so neither sender nor receiver notices anything unusual.

### C. Application (case style)

*Case: QuickMart Sdn. Bhd. runs an online grocery platform.*

**C1.** Customers receive emails that look exactly like QuickMart's, warning "Your account will be suspended in 24 hours — verify now", with a link to `quickmart-verify.com`. Identify the attack and TWO techniques used. (5 marks)
Phishing (a social engineering attack) (1). Techniques: (i) mimicking QuickMart's email with the same wording, logo and signature so it looks genuine (2); (ii) creating a sense of urgency and using a look-alike domain in the link to trick customers into entering their credentials (2).

**C2.** During a festive sale, QuickMart's site becomes unreachable because of millions of requests from thousands of IP addresses worldwide. Identify and explain the attack, and whether it is active or passive. (4 marks)
A DDoS attack — an active attack (1). A botnet of compromised devices floods QuickMart's servers with traffic, exhausting resources so genuine customers cannot access the site (2); it is active because it disrupts system operation/availability (1).

**C3.** QuickMart's app has a known vulnerability; the developer released a fix two weeks ago but many staff laptops are not updated. Explain the risk. (3 marks)
The vulnerability can be targeted by an exploit. The developer has released a patch, but unpatched laptops remain exposed, so attackers can use the known flaw to gain access; staff must install the patch immediately.

### D. Thinking

**D1.** Is an ethical hacker who tests a company's system without permission still "ethical"? (4 marks)
No. Although the intention may be to help, testing without authorisation is illegal (e.g. unauthorised access under the Computer Crimes Act 1997) and breaks the professional principle that ethical hackers work with the organisation's consent. A true ethical hacker obtains written permission and agreed scope first.

**D2.** Why are malicious insiders often more dangerous than external crackers? (4 marks)
Insiders already have legitimate credentials and knowledge of systems and where valuable data is stored, so perimeter defences such as firewalls do not stop them; their activity may look normal and go undetected; and they can collude with external parties. Organisations therefore need least-privilege access, monitoring and prompt removal of access when staff leave.

---

## Slide index

| Note section | Slides |
|---|---|
| 1 Cybercrime | p4–6 |
| 2 User expectations & vulnerability factors | p7–11 |
| 3 Exploit & patch | p12 |
| 4.1–4.3 Passive / active attacks | p13–21 |
| 4.4 Virus, worm, trojan, botnet, DDoS, rootkit | p22–31 |
| 4.5 Spam | p32–35 |
| 4.6 Phishing | p36–39 |
| 5 Hackers & crackers | p40–41 |
| 6 Malicious insider | p42 |
| 7 Cybercriminals | p43–45 |

## Links to other chapters
- Laws against these crimes (CFAA, Computer Crimes Act 1997, Act 854) → **Ch3**
- Insider threat & whistle-blowing → **Ch4**
- CIA triad, risk assessment against DDoS/insider theft → **Ch6**
- Cybersquatting & identity-theft domains → **Ch8**
