# Ch2 电脑与网络犯罪 · Computer and Internet Crime

> ⏱ **建议 1 小时 15 分** · 📝 **Q1 后半（约 14 分）**：attacks（passive / active、virus、DDoS、rootkit）和 **FIVE types of spam（10 分，考了两次）**
> 📖 范围 = 讲师 Revision Notes 的 Ch2 全部 8 节 + **5 types of spam**（讲师只写一行，但 May 2025、Oct 2025 各考 10 分）
> **怎么读**：每一节先看 ❓，自己想 5 秒 → 读"大白话" → 看例子 → 背 📖 讲师原句和 💬 答题句 → 最后看 ✅ 真题满分答案。

---

## 🗺️ 全章地图（Tree map）

```
                               Ch2 Computer and Internet Crime
                                            │
   ┌──────────────┬───────────────┬─────────┴──────┬────────────────┬──────────────────┐
   ▼              ▼               ▼                ▼                ▼                  ▼
 ① What is      ② User          ③ Commercial    ④ Security       ⑤ Malware &       ⑥ Who attacks?
   Cybercrime     Expectations     Software         Attacks          Threats          · Hackers vs
 · weapon /     · more data     · exploit        · Passive:       · Virus            Crackers
   target /     · audit trail   · patch            eavesdropping  · Worm           · Malicious
   both         · share login                      traffic mon.   · Trojan horse     insiders
 · malicious /  · complexity                     · Active:        · Botnet         · Cybercriminal /
   accidental   · rapid change                     masquerade,    · Rootkit          Hacktivist /
                                                   replay,        · Spam (5 types)   Cyberterrorist
                                                   alteration,    · Phishing
                                                   DoS/DDoS
```
*看图重点：①②③ 讲"为什么会被攻击"；④⑤ 讲"怎么被攻击"；⑥ 讲"谁在攻击"。*

## 🎯 考试怎么问（五份 past year 的 Q1 后半）

| 考点 | Oct 2024 | Jan 2025 | May 2025 | Oct 2025 | May 2026 |
|---|---|---|---|---|---|
| Define cybercrime / 2 types | | 1b (2) | | | 1d (6) |
| 2 factors → more vulnerabilities | | 1c (6) | | | |
| Exploit + developer response | 1c (4) | | | | |
| **Passive / active** | 1e (1) | | 1b (6) | | 1c (4) |
| **Active attacks / malware** | 1d (6) | 1d (9) | | 1b (6) | |
| **5 types of spam** | | | 1c (10) | 1c (10) | |

👉 **5 种 spam 考两次，每次 10 分；passive / active 和 malware 几乎每年都考。**

---

## 1. What is Cybercrime? 什么是网络犯罪

### ❓ 用电脑犯罪时，电脑是"武器"，还是"受害者"？

**大白话**
**Cybercrime（网络犯罪）就是利用网络做的违法活动。** 电脑在里面可以扮演三种角色：
- **Weapon（武器）**：犯人**用**电脑去犯罪，例如用电脑发假的银行 email 骗钱。
- **Target（目标）**：电脑**本身被攻击**，例如骇客入侵银行的 server。
- **Both（两者都是）**：例如骇客先入侵几千台电脑（这些电脑是 target），再用它们一起去攻击另一个网站（这时它们变成 weapon）。

网络犯罪造成的伤害又分两种：
- **Malicious harm（恶意伤害）**：**故意**的，例如故意写病毒、偷资料去卖。
- **Accidental harm（意外伤害）**：**不是故意**的，但还是造成损失，例如员工**不小心**点了钓鱼连结，让骇客进入公司网络。

📖 **讲师原句**
- "**Illegal activity using the Internet.**"
- "A computer can be: **Weapon** (used to launch attack), **Target** (hacked), or **both**."
- "Two types: 1. **Malicious harm** (e.g., stealing data) 2. **Accidental harm** (e.g., employee accidentally spreading virus)."
- 🌰 "**Creating a virus** vs. an employee **'accidentally' introducing malware**."
- 🌰 "**Clicking a phishing email link** → **accidentally exposes** company network to hackers."

| English | 中文 | 🌰 例子 |
|---|---|---|
| **Weapon** | 电脑当武器 | 用电脑发 phishing email 骗钱 |
| **Target** | 电脑是目标 | 骇客入侵银行 server |
| **Both** | 两者 | 被感染的电脑（target）组成 botnet 去攻击别的网站（weapon） |
| **Malicious harm** | 恶意伤害 | 故意创造病毒、偷资料 |
| **Accidental harm** | 意外伤害 | 员工不小心点了 phishing link |

> 💬 **答题句 (EN)** — *Cybercrime is illegal activity using the Internet, in which a computer can be a weapon (used to launch an attack), a target (hacked), or both. There are two types: cybercrime causing malicious harm, such as deliberately stealing data or creating a virus, and cybercrime causing accidental harm, such as an employee accidentally spreading a virus by clicking a phishing link.*

### ✅ 真题满分答案

**Jan 2025 Q1b** — *Define cybercrime.* **(2 marks)**

> Cybercrime is any illegal activity carried out using the Internet or computer technology (1), in which a computer can be used as a weapon to launch an attack, as a target that is hacked, or as both (1).

**May 2026 Q1d** — *Explain TWO types of cybercrime.* **(6 marks)**

> 1. **Cybercrime that causes malicious harm (3):** the offender acts deliberately, with the aim of stealing, damaging or making money. The attack is planned and targets victims on purpose. *Example:* a criminal creates a virus, or hacks an online store's database to steal customers' credit card details and sells them.
> 2. **Cybercrime that causes accidental harm (3):** the person did not intend to cause damage, but the action still disrupts the company and causes losses in time and repair costs. *Example:* an employee clicks a link in a phishing email by mistake, which installs malware and accidentally exposes the company network to hackers.

---

## 2. Computer User Expectations 用户的期待

### ❓ 电脑越来越方便，为什么被攻击的机会反而越来越多？

**大白话**
1. **资料越存越多**：储存资料变得又快又便宜，所以公司收集的资料越来越多（你的购物记录、位置……），每一次修改也都被记录下来（**audit trail**）。资料越多，被偷的价值就越高。
2. **用户贪方便**：有人把自己的**登入帐号密码借给同事**，结果没有权限的人也能进系统。
3. **系统越来越复杂**：连在一起的电脑、手机、软件越来越多，**可以攻进来的入口**也越来越多。
4. **科技变得太快**：新技术一直出来，公司来不及检查它安不安全。

📖 **讲师原句**
- "Technology allows **faster, cheaper data storage**."
- "**More data collected**, easier to track users."
- "**Audit trails** = record **every modification**."
- Problems: "Users **share login credentials** → **unauthorized access**" · "**Increasing complexity** → more vulnerabilities" · "**Rapid tech changes** → harder to keep secure"

🌰 **讲师的例子**
- 把**公司 Wi-Fi 密码**告诉朋友 → 可能造成公司资料外泄。
- **指纹登入**很方便，但如果指纹资料被偷了，你**不能像密码一样"重设"指纹**。

| English | 中文 | 大白话 |
|---|---|---|
| **Audit trail** | 审计轨迹 | 记录每一次谁改了什么 |
| **Login credentials** | 登入凭证 | 帐号和密码 |
| **Unauthorized access** | 未经授权的存取 | 没有权限的人进入系统 |
| **Vulnerability** | 漏洞 / 弱点 | 系统可以被攻击的地方 |

### ✅ 真题满分答案

**Jan 2025 Q1c** — *Describe TWO factors that contribute to the rise of computer-related vulnerabilities.* **(6 marks)**

> 1. **Increasing complexity (3):** modern systems connect many computers, networks, operating systems, applications and devices. The more things are connected, the more entry points there are for attackers, and every change or expansion can create new weaknesses. *Example:* a company connects dozens of IoT security cameras to its network, but they still use their default passwords, giving attackers an easy way in.
> 2. **Rapid technological change (3):** new technologies appear faster than companies can check and secure them, so it becomes harder to keep systems safe. *Example:* a company starts using a new cloud storage service before its IT team understands how to configure it, and customer files are accidentally left open to the public.
>
> *(Also acceptable: users sharing login credentials, which leads to unauthorised access.)*

---

## 3. Reliance on Commercial Software 依赖商业软件

### ❓ 软件公司已经出了修补，为什么大家还是被攻击？

**大白话**
软件都会有**弱点（漏洞）**。骇客利用这些弱点来攻击，这叫 **exploit**。软件公司发现弱点后，会推出**修补程式（patch）**，也就是你手机、电脑常常跳出来的"更新"。
但问题是：**很多人懒得更新**。修补已经出了，漏洞还开着，骇客就从这里进来。讲师说：**大部分网络攻击，都是因为人们没有及时安装 patch**。

📖 **讲师原句**
- "**Weaknesses in software = exploits.**"
- "Companies release **patches** to fix flaws, but if users **don't install** → **risk of breach**."
- "**Most cyberattacks happen because people don't install patches in time.**"
- 🌰 "**WannaCry ransomware** spread because people **didn't install Windows updates**."

| English | 中文 | 大白话 |
|---|---|---|
| **Exploit** | 漏洞利用 | 骇客利用软件弱点的攻击 |
| **Patch** | 修补程式 | 软件公司推出来修补弱点的更新 |
| **Ransomware** | 勒索软件 | 把你的档案锁起来，要你付钱才解锁 |

### ✅ 真题满分答案

**Oct 2024 Q1c** — *Define "exploits" and explain how a software developer responds to exploits.* **(4 marks)**

> - **Definition (2):** an exploit is an attack that takes advantage of a weakness (vulnerability) in software, for example a flaw caused by poor design or programming, to gain unauthorised access or cause damage.
> - **Response (2):** when the weakness is discovered, the software developer quickly builds a fix called a **patch** and releases it to users, usually through automatic updates, and advises them to install it immediately — because systems that are not patched remain at risk of a breach, as happened with the WannaCry ransomware.

---

## 4. Types of Security Attacks 攻击的类型 ⭐

### ❓ 4.1 小偷"偷看你的信"和"把你的信改掉"，有什么不一样？

**大白话**
一条判断规则：**攻击者有没有改动资料、或影响系统运作？**
- **没有**，只是偷看、偷听 → **Passive attack（被动攻击）**。讲师说它像**"安静的小偷"**（silent thieves）：东西还在，只是被偷看了，所以**很难发现**。
- **有**，会改资料、冒充别人、让系统当机 → **Active attack（主动攻击）**。讲师说它像**"打破窗户的窃贼"**（burglars smashing windows）：会造成**看得见的破坏**。

```
  PASSIVE（Darth 只是偷看，讯息没变）          ACTIVE（Darth 动手：冒充、改、重发、瘫痪）
                [Darth]                                   [Darth]
                   ↑ 偷看                                    ↓ 假冒 Bob 发信
  [Bob] ──────→ Internet ──────→ [Alice]        [Bob]     Internet ──────→ [Alice]
       Bob 和 Alice 都不知道被偷看                         Alice 以为信来自 Bob
```

📖 **讲师原句**
- **A. Passive Attacks** (**stealing info, no damage**): **Eavesdropping** (listening to calls/emails) · **Traffic monitoring** (observing communication patterns) · **Hard to detect**.
- **B. Active Attacks** (**tampering/disrupting**): 1. **Masquerade** 2. **Replay** 3. **Message alteration** 4. **Denial of Service (DoS/DDoS)**
- "Contrast: Passive attack = **silent spying**. Active attack = **visible damage**."
- 🌰 "Passive: Hacker **listens to your Wi-Fi traffic**. Active: Hacker **floods your Wi-Fi router so it crashes**."

| | **Passive Attack** 被动攻击 | **Active Attack** 主动攻击 |
|---|---|---|
| 📖 讲师 | **Stealing info, no damage** | **Tampering / disrupting** |
| 比喻 | Silent spying / silent thieves | Visible damage / burglars smashing windows |
| 容易发现吗 | **Hard to detect**（资料没变） | 比较容易发现（资料变了、服务停了） |
| 类型 | Eavesdropping、Traffic monitoring | Masquerade、Replay、Message alteration、DoS/DDoS |
| 🌰 讲师例子 | Hacker listens to your Wi-Fi traffic | Hacker floods your Wi-Fi router so it crashes |

**2 种 Passive attack**

| Type | 中文 | 📖 讲师 | 大白话 |
|---|---|---|---|
| **Eavesdropping** | 窃听 | **Listening to calls/emails** | 偷听电话、偷看 email 内容 |
| **Traffic monitoring** | 流量监测 | **Observing communication patterns** | 不看内容，只看"谁跟谁联络、多常、什么时候" |

**4 种 Active attack**

| Type | 中文 | 📖 讲师 | 大白话 | 🌰 例子 |
|---|---|---|---|---|
| **Masquerade** | 冒充 | **Pretending to be someone else** | 假装是别人 | 用偷来的帐号登入，假装是 HR 经理 |
| **Replay** | 重放 | **Resending captured messages** | 先录下来，之后再发一次 | 录下"转账 RM500"，再重发 10 次 |
| **Message alteration** | 修改讯息 | **Changing/delaying messages** | 把讯息改掉或拖延 | 把转账金额 RM500 改成 RM5,000 |
| **Denial of Service (DoS/DDoS)** | 阻断服务 | **Overwhelming a system with fake requests** | 用大量假请求把系统灌爆 | 网站被大量假流量淹没，真的顾客打不开 |

> 💬 **答题句 (EN)** — *A passive attack steals information without causing damage — for example eavesdropping on calls and emails, or traffic monitoring — and is hard to detect. An active attack tampers with or disrupts the system, for example masquerade (pretending to be someone else), replay (resending captured messages), message alteration (changing or delaying messages) and denial of service (overwhelming a system with fake requests).*

### ✅ 真题满分答案

**Oct 2024 Q1e** — *Identify the main objective of an attacker performing a passive attack.* **(1 mark)**

> To secretly steal (intercept) information — for example by eavesdropping on emails or monitoring traffic — without changing the data, so that the attack is not detected. (1)

**May 2025 Q1b** — *Discuss (i) passive attacks and (ii) active attacks, and provide an example for each.* **(3 + 3 marks)**

> **(i) Passive attack (3):** the attacker only steals information and does not cause damage or change anything, for example by eavesdropping on calls and emails or by monitoring traffic patterns. Because nothing is changed, it is hard to detect — like silent spying. *Example:* in a café, a hacker listens to the Wi-Fi traffic and captures a staff member's password as she logs into her company's old, unencrypted web page.
>
> **(ii) Active attack (3):** the attacker tampers with or disrupts the system — through masquerade, replay, message alteration or denial of service — causing visible damage. *Example:* the hacker uses the stolen password to log in as the staff member (masquerade) and changes the bank account number for a supplier payment, so the next RM45,000 payment goes to the hacker. Another example is flooding the company's Wi-Fi router with traffic so it crashes.

**May 2026 Q1c** — *Describe passive attacks and active attacks.* **(4 marks)**

> - **Passive attacks (2):** attacks that steal information without causing damage, such as eavesdropping on calls and emails or monitoring traffic patterns; they are hard to detect because nothing is changed. *Example:* a hacker listens to a company's Wi-Fi traffic to capture passwords.
> - **Active attacks (2):** attacks that tamper with or disrupt the system, such as masquerade, replay, message alteration and denial of service; they cause visible damage. *Example:* a hacker floods the company's Wi-Fi router with fake requests so that it crashes.

**Jan 2025 Q1d** — *Explain THREE examples of active security attacks.* **(9 marks)**

> 1. **Masquerade (3)** – the attacker pretends to be someone else to gain access or privileges they should not have. Messages or logins appear to come from a trusted user. *Example:* an attacker uses a stolen staff ID and password to log in as an HR manager and approve fake salary payments.
> 2. **Replay (3)** – the attacker captures a valid message and resends it later to cause an unauthorised effect. *Example:* the attacker records a customer's "transfer RM500" request and resends it several times, so the money is transferred again and again.
> 3. **Denial of Service (DoS/DDoS) (3)** – the attacker overwhelms a system with fake requests so that real users cannot use it; in a DDoS the requests come from many infected computers (a botnet). *Example:* a botnet floods a university's online course-registration server on registration day, so students cannot register.

---

## 5. Common Malware & Threats 常见恶意软件 ⭐

### ❓ 5.1 Virus 和 worm 都会传染，差在哪？

**大白话**
- **Virus（病毒）**：**附在档案上**，要**有人打开那个档案**，它才会发作和传播。就像感冒病毒要靠人传人。
- **Worm（蠕虫）**：**不用人帮忙**，**自己会传**，例如自动把自己寄给你通讯录里的每一个人。
- **Trojan horse（木马）**：**伪装成正常的软件**（例如"免费游戏"），骗你安装。它**不会自己复制**。名字来自希腊故事：士兵躲在木马里被当成礼物送进城。
- **Botnet（殭尸网络）**：很多台**被感染的电脑**组成一个网络，**被骇客遥控**。每一台叫 bot 或 zombie（殭尸）。
- **Rootkit**：一套工具，让骇客**躲在你的电脑里不被发现**，还能远程控制。
- **Spam（垃圾讯息）**：你没有要求、却一直收到的讯息。
- **Phishing（网络钓鱼）**：**骗你**说出密码、信用卡号，例如假的银行 email。

📖 **讲师原句**

| Threat | 中文 | 📖 讲师定义 | 会自己复制？ | 🌰 例子 |
|---|---|---|---|---|
| **Virus** | 病毒 | **Attaches to files, spreads when opened** | ✅（要人打开） | Email 附件里的 Word 档，一打开就删档案 |
| **Worm** | 蠕虫 | **Spreads automatically (no user needed)** | ✅（自己传） | 自动 email 给通讯录所有人 |
| **Trojan Horse** | 木马 | **Disguised as legit software, cannot self-replicate** | ❌ | 假的"免费 PDF 转换器"偷你的密码 |
| **Botnet** | 殭尸网络 | **Network of infected computers controlled by hacker** | – | 几千台被感染的电脑一起攻击一个网站 |
| **Rootkit** | 隐匿工具 | **Hides attacker's presence** | – | 骇客躲在 server 里，antivirus 看不到 |
| **Spam** | 垃圾讯息 | **Unwanted messages (email, SMS, social media)** | – | "你中奖了！点这里" |
| **Phishing** | 网络钓鱼 | **Tricking users to reveal sensitive info** (passwords, credit cards) | – | **A fake "bank email" asking you to click a link** |

🌰 **讲师的串连例子**：点了一个假的 **"free game"** 下载（**Trojan**）→ 电脑被偷偷装上 **botnet** client → 你的电脑**在你不知道的情况下参与 DDoS 攻击**。

### ❓ 5.2 DoS 和 DDoS 差在哪？

**大白话**
- **DoS**：一个来源不停发假请求，把网站灌爆。
- **DDoS（Distributed DoS，分散式阻断服务）**：用 **botnet 里成千上万台电脑同时**发假请求。因为每一台都是真的电脑，网站**分不出哪些是攻击、哪些是真的顾客**，所以更难挡。

```
  [infected PC]──┐
  [infected PC]──┤ 攻击流量                        ┌─[Server]
  [infected PC]──┼──────→ [ Internet ] ──────────→├─[Server]   !! 被灌爆
  [real user]  ──┤ 正常流量                        └─[Server]
  [real user]  ──┘
```
*看图重点：攻击流量和真顾客的流量混在一起，server 分不出来，真顾客也进不去。*

> 💬 **答题句 (EN)** — *A virus attaches to files and spreads when they are opened; a worm spreads automatically without any user action; a Trojan horse is disguised as legitimate software and cannot self-replicate; a botnet is a network of infected computers controlled by a hacker; a rootkit hides the attacker's presence; spam is unwanted messages; and phishing tricks users into revealing sensitive information such as passwords and credit card numbers.*

### ✅ 真题满分答案

**Oct 2024 Q1d** — *Discuss the following active attacks: (i) trojan horse (ii) DDoS attack (iii) rootkit.* **(2 + 2 + 2 marks)**

> - **(i) Trojan horse (2):** malware that is disguised as legitimate software to trick users into installing it; once installed, it damages or steals data, but unlike a virus or worm it cannot self-replicate. *Example:* a fake "free game" download that secretly installs a program to steal saved passwords.
> - **(ii) DDoS attack (2):** a distributed denial-of-service attack uses a botnet — many infected computers controlled by a hacker — to overwhelm a server with fake requests so that real users cannot use it. Because every bot is a real device, the attack traffic is hard to separate from normal traffic. *Example:* thousands of infected computers flood an online store's website during a big sale, so customers cannot check out.
> - **(iii) Rootkit (2):** a set of tools that hides the attacker's presence on a computer while allowing them to control it remotely — running programs, reading logs and changing settings — without being detected by normal antivirus. *Example:* an attacker installs a rootkit on a company server to secretly watch the administrators' activity for months.

**Oct 2025 Q1b** — *Discuss the following attacks, with an example each: (i) Virus (ii) Rootkit (iii) DDoS.* **(2 + 2 + 2 marks)**

> - **(i) Virus (2):** malicious code that attaches to a file or program and spreads when the user opens it; it can display messages, delete files or slow the computer. *Example:* a virus hidden in a Word attachment called "Invoice.docm" deletes files on the shared drive when an employee opens it.
> - **(ii) Rootkit (2):** a set of tools that hides the attacker's presence so they can control the computer secretly and remotely. *Example:* an attacker uses a rootkit to change a victim's firewall settings and read their files without being noticed.
> - **(iii) DDoS (2):** an attack in which a botnet of many infected computers overwhelms a server with fake requests, making the service unavailable to real users. *Example:* a botnet sends millions of requests to a bank's website, so customers cannot log in to pay their bills.

---

## 6. Spam 的 5 种类型 ⭐⭐（讲师只写一行，但考了两次 × 10 分）

### ❓ 你今天收到的垃圾讯息，是从哪几个地方来的？

**大白话**
**Spam = 你没有要求、却被大量发送给你的讯息**，通常是广告，有时藏着诈骗连结或病毒。按照"从哪里来"分成 5 种：

| Type | 中文 | 特点 / 怎么运作 | 🌰 例子 |
|---|---|---|---|
| **Email spam** | 电邮垃圾信 | 最常见的 spam：**大量**发送没人要的 email，塞满邮箱，常夹 phishing 连结或恶意附件 | "You've won an iPhone! Click here" |
| **Social media spam** | 社交媒体 spam | 用**假帐号（fake profiles）**在热门社交平台留言、发私讯、发连结 | Instagram 贴文下的"投资赚钱，私讯我"留言 |
| **Mobile phone spam** | 手机 spam | 用 **SMS 简讯**发送，有些用 app 的 **push notification（推播通知）** | "RM0 loan approved! Reply YES" |
| **Text message (instant messaging) spam** | 即时通讯 spam | 像 email spam，但**快得多**，透过 **WhatsApp、Telegram** 等通讯 app 群发 | 陌生人在 WhatsApp 群组发"兼职一天赚 RM300" |
| **SEO spam**（spam indexing） | 搜寻引擎 spam | 用 **SEO 技巧**（塞关键字、假连结）把骗人的网站**推到搜寻结果最前面** | 搜"cheap flights"，排第一的是诈骗网站 |

⚠️ **Mobile phone spam vs Text message spam 很像**，答案里写出差别：前者是 **SMS / push notifications**；后者是透过 **instant messaging apps（WhatsApp / Telegram）**。

> 💬 **答题句 (EN)** — *Spam is unwanted messages sent in bulk to many people, often containing advertisements, scams or malicious links. Five common types are email spam, social media spam (sent through fake profiles), mobile phone spam (SMS and push notifications), text message spam (sent through instant-messaging apps such as WhatsApp and Telegram), and SEO spam (pushing spam websites to the top of search results).*

### ✅ 真题满分答案

**May 2025 Q1c / Oct 2025 Q1c** — *List and describe / Explain FIVE common types of spam, including their characteristics and how they function.* **(10 marks)**

> 1. **Email spam (2)** – the most common form of spam: unwanted emails sent in bulk, usually advertisements. Spammers send the same message to millions of addresses at almost no cost; it fills the inbox and often carries phishing links or malicious attachments. *Example:* an email claiming "You've won an iPhone! Click here to claim."
> 2. **Social media spam (2)** – spam spread through social networking sites. Spammers create fake profiles and use them to post comments, send private messages or share links on popular platforms, taking advantage of users' trust in their social networks. *Example:* fake accounts posting "earn money fast, DM me" under popular Instagram posts.
> 3. **Mobile phone spam (2)** – spam sent to mobile phones as SMS messages; some spammers also use app push notifications to get attention. Users receive it directly on their phones whether they asked for it or not. *Example:* an SMS saying "RM0 loan approved, reply YES".
> 4. **Text message (instant messaging) spam (2)** – similar to email spam but much faster; spammers use instant-messaging apps such as WhatsApp and Telegram to send promotions or scams to individuals and groups. *Example:* a stranger posts a "part-time job, earn RM300 a day" message in a WhatsApp group.
> 5. **SEO spam (spam indexing) (2)** – spammers misuse search engine optimisation techniques, such as keyword stuffing and fake links, to push their websites to the top of search results, so users are tricked into visiting low-quality or malicious sites. *Example:* searching "cheap flights" shows a scam website as the first result.

---

## 7. Hackers vs Crackers 骇客 vs 破解者

### ❓ 同样会入侵系统，为什么有人被公司请去工作，有人被抓去坐牢？

**大白话**
差别在于**有没有得到许可**和**目的是什么**。
- **Ethical hacker（白帽 white hat）**：**得到公司许可**才去测试系统，找出漏洞，帮公司**变得更安全**。很多有 **CEH（Certified Ethical Hacker）** 证书。
- **Cracker（黑帽 black hat）**：**没有许可**，为了**自己的好处**（钱、报复）去破坏系统。

📖 **讲师原句**
- "**Ethical Hackers**: **Test systems legally**, help **improve security** (e.g., **CEH certified**)."
- "**Crackers**: **Malicious hackers** who break systems **for personal gain**."
- "Contrast: Ethical hackers = **white hats**. Crackers = **black hats**."
- 🌰 "**Facebook pays bug bounty hunters** (ethical hackers) who report security flaws."

| | **Ethical hacker**（white hat）| **Cracker**（black hat）|
|---|---|---|
| 有许可吗 | 有，合法测试 | 没有 |
| 目的 | 改善安全 | 个人利益 |
| 🌰 | Facebook 付钱给报告漏洞的 bug bounty hunter | 入侵网店偷信用卡资料去卖 |

---

## 8. Malicious Insiders 恶意内部人员

### ❓ 最危险的攻击者，会不会就坐在公司里面？

**大白话**
会。**Malicious insider（恶意内部人员）**是**现任或前任员工**，利用自己本来就有的权限做坏事，而且常常**和外面的人勾结**。因为他们本来就有密码、知道系统在哪里，所以很难防。

📖 **讲师原句**：*"**Employees or ex-staff abusing access.** Often **collude with outsiders** for fraud."*

🌰 **讲师的例子**：**银行员工泄露顾客的帐户资料**；大学的 **IT admin 出售学生资料**。

---

## 9. Cybercriminal Categories 三种网络犯罪者

### ❓ 同样的骇客技术，为什么动机不同就有不同的名字？

**大白话**
讲师说：**"Motives matter"**——同样的技术，看你**为了什么**而用：
- 为了**钱** → **Cybercriminal（网络罪犯）**
- 为了**政治或社会理念、抗议** → **Hacktivist（骇客行动主义者）**，hacker + activist
- 为了**制造大规模伤害和恐慌** → **Cyberterrorist（网络恐怖分子）**

📖 **讲师原句**
- "1. **Cybercriminals** – **financially motivated** (stealing credit card data, funds)."
- "2. **Hacktivists** – **politically/socially motivated**."
- "3. **Cyberterrorists** – aim to cause **large-scale harm** (e.g., hacking air traffic systems)."
- "**Motives matter** — the same hacking skill can be used for **money, protest, or destruction**."

| Category | 中文 | 动机 | 🌰 讲师例子 |
|---|---|---|---|
| **Cybercriminals** | 网络罪犯 | **Financially motivated** | **Stealing credit cards** |
| **Hacktivists** | 骇客行动主义者 | **Politically / socially motivated** | **Defacing a government website** to protest policy |
| **Cyberterrorists** | 网络恐怖分子 | **Large-scale harm** | **Attacking air traffic systems** to cause chaos |

> 💬 **答题句 (EN)** — *Cybercriminals are financially motivated, e.g. stealing credit card data; hacktivists are politically or socially motivated, e.g. defacing a government website to protest a policy; cyberterrorists aim to cause large-scale harm, e.g. attacking air traffic systems. Motives matter: the same hacking skill can be used for money, protest or destruction.*

---

## 🔁 自测：我问，你先想，答案在下面

1. 讲师对 cybercrime 的定义？电脑可以是哪三种角色？
2. 员工不小心点了 phishing link，是 malicious 还是 accidental harm？
3. 讲师说用户的期待带来哪 3 个问题？
4. Exploit 是什么？WannaCry 为什么传播得那么快？
5. 偷听 Wi-Fi 是 passive 还是 active？把 Wi-Fi router 灌爆呢？讲师用什么比喻？
6. 4 种 active attack？"录下讯息再重发"是哪一种？
7. Virus 和 worm 的差别？Trojan 能不能自我复制？Rootkit 做什么？
8. Spam 的 5 种类型？Mobile phone spam 和 text message spam 差在哪？
9. Facebook 付钱给 bug bounty hunter，他们是 white hat 还是 black hat？
10. 黑掉政府网站来抗议政策的人叫什么？攻击航空交通系统的呢？

.

.

.

.

**答案**
1. **Illegal activity using the Internet**；**weapon, target, or both**
2. **Accidental harm**
3. **Users share login credentials** → unauthorised access · **increasing complexity** → more vulnerabilities · **rapid tech changes** → harder to keep secure
4. **Weaknesses in software** that attackers take advantage of；很多人**没装 Windows update（patch）**
5. 偷听 = **Passive**（eavesdropping）；灌爆 = **Active**（DoS）；passive = **silent thieves / silent spying**，active = **burglars smashing windows / visible damage**
6. **Masquerade, Replay, Message alteration, DoS/DDoS**；**Replay**
7. Virus **attaches to files, spreads when opened**；worm **spreads automatically**；Trojan **cannot self-replicate**；Rootkit **hides the attacker's presence**
8. **Email · Social media · Mobile phone · Text message (IM) · SEO**；mobile phone = **SMS / push**，text message = **WhatsApp / Telegram**
9. **White hat**（ethical hackers）
10. **Hacktivist**；**Cyberterrorist**

---

## 🧾 一页背诵（考前 5 分钟看）

- **Cybercrime** = illegal activity using the Internet · computer = **weapon / target / both** · **malicious**（create virus, steal data）vs **accidental**（click phishing link）
- **User expectations**：faster, cheaper storage → more data · **audit trails** · **share login** → unauthorised access · **complexity** · **rapid change** · Wi-Fi password · fingerprint can't reset
- **Exploit** = weakness in software · **patch** fixes it · not installed → breach · most attacks = patches not installed · **WannaCry**
- **Passive** = stealing info, no damage, hard to detect：**eavesdropping, traffic monitoring**（silent thieves; listen to Wi-Fi）
- **Active** = tampering / disrupting：**masquerade, replay, message alteration, DoS/DDoS**（burglars smashing windows; flood Wi-Fi router）
- **Malware**：Virus（attaches, spreads when opened）· Worm（automatic）· Trojan（disguised, cannot self-replicate）· Botnet（infected PCs controlled by hacker）· Rootkit（hides attacker）· Spam（unwanted messages）· Phishing（trick to reveal info; fake bank email）
- **DDoS** = botnet floods server with fake requests · free game (Trojan) → botnet → DDoS
- **Spam ×5**：Email · Social media（fake profiles）· Mobile phone（SMS, push）· Text message（WhatsApp, Telegram）· SEO（top of search results）
- **Ethical hacker**（white hat, legal, CEH, Facebook bug bounty）vs **Cracker**（black hat, personal gain）
- **Malicious insider** = employees / ex-staff abusing access, collude with outsiders（bank staff leak data）
- **Cybercriminal**（money）· **Hacktivist**（protest; deface gov website）· **Cyberterrorist**（large-scale harm; air traffic）· motives matter

➡️ 五章都读完了！回到 `00_8H_Plan.md` 看最后的复习方法。
