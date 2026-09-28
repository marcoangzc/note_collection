# Ch2 电脑与网络犯罪 · Computer and Internet Crime

> ⏱ **建议 1 小时 20 分** · 📝 **Q1 后半（约 14 分）**：attacks（passive / active、virus、DDoS、rootkit）和 **FIVE types of spam（10 分，考了两次）**
> 📖 范围 = 讲师 Revision Notes 的 Ch2（8 节）+ **5 types of spam**（tips 只写一行，但 May 2025、Oct 2025 各考 10 分）
> 用法：每一节先看 ❓，自己想 5 秒，再往下读。英文 **粗体** 是考试要写的字。

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

---

## 🎯 考试怎么问（五份 past year，都在 Q1）

| 考点 | 考过哪几份 | 分数 | 重要度 |
|---|---|---|---|
| **FIVE types of spam** | May 2025、Oct 2025 | **10** | ⭐⭐⭐ |
| **Active attacks / malware**（trojan、DDoS、rootkit、virus） | Oct 2024、Jan 2025、Oct 2025 | 6–9 | ⭐⭐⭐ |
| **Passive vs active** | Oct 2024、May 2025、May 2026 | 1–6 | ⭐⭐⭐ |
| Define cybercrime · TWO types of cybercrime | Jan 2025、May 2026 | 2 + 6 | ⭐⭐ |
| TWO factors → more vulnerabilities | Jan 2025 | 6 | ⭐⭐ |
| Exploit + how developers respond | Oct 2024 | 4 | ⭐ |

---

## 1. What is Cybercrime? 什么是网络犯罪

### ❓ 用电脑犯罪时，电脑是"武器"还是"受害者"？

两个都可能。

📖 **讲师原句**
- "**Illegal activity using the Internet.**"
- "A computer can be: **Weapon** (used to launch attack), **Target** (hacked), or **both**."
- "Two types: **Malicious harm** (e.g., stealing data), **Accidental harm** (e.g., employee accidentally spreading virus)."

| | 中文 | 🌰 例子 |
|---|---|---|
| **Weapon** | 电脑当武器 | 用电脑发 phishing email 骗钱 |
| **Target** | 电脑是目标 | 骇客入侵银行 server |
| **Both** | 两者 | 被感染的电脑（target）组成 botnet 去攻击别的网站（weapon） |
| **Malicious harm** | 恶意伤害 | 故意**创造 virus**、偷资料 |
| **Accidental harm** | 意外伤害 | 员工**不小心点了 phishing link** → 公司网络被骇客打开 |

> 💬 **答题句 (EN)** — *Cybercrime is illegal activity using the Internet, in which a computer can be the weapon, the target, or both. It can cause malicious harm (e.g. deliberately stealing data) or accidental harm (e.g. an employee accidentally spreading a virus).*

### 📝 真题
- **Jan 2025 Q1b** — *Define cybercrime.* (2) → illegal activity using the Internet (1) + weapon / target / both (1)。
- **May 2026 Q1d** — *Explain TWO types of cybercrime.* (6) → **Malicious harm** 和 **Accidental harm**，每个 3 分 = 定义 + 特点 + 例子。

---

## 2. Computer User Expectations 用户的期待

### ❓ 为什么电脑越来越方便，被攻击的机会反而越来越多？

📖 **讲师原句**
- Technology allows **faster, cheaper data storage** → **more data collected**, easier to track users.
- **Audit trails** = record **every modification**.
- Problems:
  - Users **share login credentials** → **unauthorized access**
  - **Increasing complexity** → more vulnerabilities
  - **Rapid tech changes** → harder to keep secure

🌰 讲师例子：把**公司 Wi-Fi 密码**给朋友 → 资料外泄风险；**指纹登入**方便，但指纹被偷了，**不能像密码一样"重设"**。

### 📝 真题：Jan 2025 Q1c — *Describe TWO factors that contribute to the rise of computer-related vulnerabilities.* (6 marks)
1. **Increasing complexity** – systems connect many computers, networks, apps and devices, so there are **more entry points** for attackers. *e.g.* a company adds IoT cameras that still use default passwords.
2. **Rapid pace of technological change** – new technology appears faster than companies can check its security. *e.g.* a new cloud service is launched before the team knows how to configure it, leaving files public.

---

## 3. Reliance on Commercial Software 依赖商业软件

### ❓ 软件有漏洞，公司已经出了修补，为什么还是被攻击？

因为**用户没有装**修补。

📖 **讲师原句**
- "Weaknesses in software = **exploits**."
- "Companies release **patches** to fix flaws, but if users **don't install** → **risk of breach**."
- "Most cyberattacks happen because people **don't install patches in time**."

🌰 讲师例子：**WannaCry ransomware** 大规模传播，就是因为很多人**没装 Windows update**。

### 📝 真题：Oct 2024 Q1c — *Define "exploits" and explain how a software developer responds.* (4)
- **Exploit** – an attack that takes advantage of a **weakness (vulnerability)** in software (2).
- **Response** – the developer quickly builds a **patch** to fix the flaw and releases it, telling users to **install it immediately**, because unpatched systems can be breached (2).

---

## 4. Types of Security Attacks 攻击的类型 ⭐

### ❓ 小偷"偷看你的信" 和 "把你的信改掉"，有什么不一样？

一条判断规则：**有没有改动资料 / 影响系统？** 没有 → **Passive**；有 → **Active**。

📖 **讲师原句**：*Passive attack = **"silent spying"**, like **"silent thieves"**. Active attack = **"visible damage"**, like **"burglars smashing windows"**.*

| | **Passive Attack** 被动攻击 | **Active Attack** 主动攻击 |
|---|---|---|
| 📖 讲师 | **Stealing info, no damage** | **Tampering / disrupting** |
| 类型 | **Eavesdropping**（窃听电话、email）· **Traffic monitoring**（观察通讯模式） | **Masquerade** · **Replay** · **Message alteration** · **DoS / DDoS** |
| 容易发现吗 | **Hard to detect** | 容易发现（资料变了 / 服务停了） |
| 🌰 讲师例子 | 骇客**偷听你的 Wi-Fi traffic** | 骇客**用大量流量淹没你的 Wi-Fi router**，让它当机 |

```
  PASSIVE（Darth 只是偷看）                  ACTIVE（Darth 动手改 / 冒充 / 重发）
                [Darth]                                  [Darth]
                   ↑ 偷看                                   ↓ 假冒 Bob 发信
  [Bob] ──────→ Internet ──────→ [Alice]      [Bob]      Internet ──────→ [Alice]
       讯息完全没变                                     Alice 以为来自 Bob
```

**4 种 Active attack**

| Type | 中文 | 📖 讲师定义 | 🌰 例子 |
|---|---|---|---|
| **Masquerade** | 冒充 | **Pretending to be someone else** | 用偷来的帐号登入，假装是 HR 经理 |
| **Replay** | 重放 | **Resending captured messages** | 录下"转账 RM500"，再重发 10 次 |
| **Message alteration** | 修改讯息 | **Changing / delaying messages** | 把 RM500 改成 RM5,000 |
| **Denial of Service (DoS/DDoS)** | 阻断服务 | **Overwhelming a system with fake requests** | 大量假请求让网站打不开 |

> 💬 **答题句 (EN)** — *A passive attack steals information without causing damage, such as eavesdropping or traffic monitoring, and is hard to detect. An active attack tampers with or disrupts the system, such as masquerade (pretending to be someone else), replay (resending captured messages), message alteration (changing or delaying messages) and denial of service (overwhelming a system with fake requests).*

### 📝 真题
- **May 2025 Q1b** — *Explain passive and active attacks.* (3+3) → 每个 = 讲师定义 + 类型 + 例子。
- **May 2026 Q1c** — passive / active（4）· **Oct 2024 Q1e** — *Main purpose of a passive attack* (1) → **to intercept / steal information without being detected**。
- **Jan 2025 Q1d** — *THREE examples of active attacks.* (9) → Masquerade、Replay、DoS，每个 3 分 = 定义 + 怎么运作 + 例子。

---

## 5. Common Malware & Threats 常见恶意软件 ⭐

### ❓ Virus 和 worm 都会传染，差在哪？

**Virus 要人帮忙（打开档案），worm 自己会跑。**

| Threat | 中文 | 📖 讲师定义 | 会自己复制？ | 🌰 例子 |
|---|---|---|---|---|
| **Virus** | 病毒 | **Attaches to files, spreads when opened** | ✅（要人打开） | Word 附件里的 macro virus |
| **Worm** | 蠕虫 | **Spreads automatically (no user needed)** | ✅（自己传） | 自动 email 给通讯录所有人 |
| **Trojan Horse** | 木马 | **Disguised as legit software, cannot self-replicate** | ❌ | 假的"免费游戏"下载 |
| **Botnet** | 殭尸网络 | **Network of infected computers controlled by hacker** | – | 几千台被感染的电脑一起攻击 |
| **Rootkit** | 隐匿工具 | **Hides attacker's presence** | – | 骇客躲在 server 里，antivirus 看不到 |
| **Spam** | 垃圾讯息 | **Unwanted messages (email, SMS, social media)** | – | "你中奖了！" |
| **Phishing** | 网络钓鱼 | **Tricking users to reveal sensitive info** (passwords, credit cards) | – | 假的"银行 email"叫你点连结 |

🌰 讲师的串连例子：点了一个假的 **"free game"**（**Trojan**）→ 电脑被装上 **botnet** client → 你的电脑**偷偷参与 DDoS 攻击**。

💡 **DDoS**（Distributed DoS）= 用 **botnet** 的很多台电脑一起发流量，比单一来源的 DoS 更难挡，因为攻击流量和正常用户分不出来。

> 💬 **答题句 (EN)** — *A virus attaches to files and spreads when they are opened; a worm spreads automatically with no user action; a Trojan horse is disguised as legitimate software and cannot self-replicate; a botnet is a network of infected computers controlled by a hacker; a rootkit hides the attacker's presence; and phishing tricks users into revealing sensitive information.*

### 📝 真题
- **Oct 2024 Q1d** — *Discuss (i) trojan horse (ii) DDoS (iii) rootkit.* (2 each)
- **Oct 2025 Q1b** — *Discuss (i) Virus (ii) Rootkit (iii) DDoS, with an example each.* (2 each)
✅ 每个 2 分 = **讲师定义 (1) + 例子 (1)**：
- **Trojan horse** – disguised as legitimate software and cannot self-replicate; *e.g.* a fake "free PDF converter" that steals saved passwords.
- **DDoS** – a botnet of many infected computers floods a server with fake requests so real users cannot use it; *e.g.* an online shop's checkout crashes during an 11.11 sale.
- **Rootkit** – hides the attacker's presence so they can control the computer secretly; *e.g.* a hacker hides inside a company server and reads admin logs without being noticed.
- **Virus** – attaches to files and spreads when opened; *e.g.* a macro virus in an "Invoice.docm" email attachment deletes files.

---

## 6. Spam 的 5 种类型 ⭐⭐（tips 只写一行，但考了两次 × 10 分）

### ❓ 你今天收到的垃圾讯息，是从哪几个地方来的？

| Type | 中文 | 特点 / 怎么运作 | 🌰 例子 |
|---|---|---|---|
| **Email spam** | 电邮垃圾信 | 最标准的 spam，**大量** unsolicited email，常夹 phishing link / 恶意附件 | "You've won an iPhone! Click here" |
| **Social media spam** | 社交媒体 spam | 用**假帐号 (fake profiles)** 在热门平台留言、私讯 | Instagram 贴文下的"投资赚钱"留言 |
| **Mobile phone spam** | 手机 spam | **SMS** + app 的 **push alerts** | "RM0 loan approved, reply YES" |
| **Text message (IM) spam** | 即时通讯 spam | 像 email spam 但**快得多**，透过 **WhatsApp、Telegram** | 陌生人在 WhatsApp 群发兼职广告 |
| **SEO spam** (spam indexing) | 搜寻引擎 spam | 用 **SEO 技巧**把骗局网站推到**搜寻结果前面** | 搜"cheap flights"第一个是诈骗网站 |

⚠️ Mobile phone spam vs text message spam：前者 = **SMS / push alerts**；后者 = **IM apps（WhatsApp / Telegram）**。答案里写出这个区别。

> 💬 **答题句 (EN)** — *Spam is unsolicited messages sent in bulk. Five common types are email spam, social media spam (via fake profiles), mobile phone spam (SMS and push alerts), text message spam (via instant-messaging apps such as WhatsApp and Telegram) and SEO spam (pushing spam websites to the top of search results).*

### 📝 真题：May 2025 / Oct 2025 Q1c — *List and describe / Explain FIVE common types of spam.* (10 marks)
✅ 每个 2 分 = **特点 (1) + 怎么运作 / 例子 (1)**，照上表写成句子。

---

## 7. Hackers vs Crackers 骇客 vs 破解者

### ❓ 同样会入侵系统，为什么有人被公司请去工作，有人被抓去坐牢？

差别在**有没有得到许可、目的是什么**。

| | **Ethical Hackers**（white hats 白帽） | **Crackers**（black hats 黑帽） |
|---|---|---|
| 📖 讲师 | **Test systems legally**, help **improve security** (e.g. **CEH** certified) | **Malicious** hackers who break systems for **personal gain** |
| 🌰 | **Facebook** pays **bug bounty** hunters who report security flaws | 入侵网店偷信用卡资料来卖 |

---

## 8. Malicious Insiders 恶意内部人员

### ❓ 最危险的攻击者，可能就坐在公司里面？

是的。

📖 **讲师原句**："**Employees or ex-staff abusing access.** Often **collude with outsiders** for fraud."

🌰 讲师例子：**银行员工泄露客户帐户资料**；大学的 **IT admin 出售学生资料**。

---

## 9. Cybercriminal Categories 三种网络犯罪者

### ❓ 同样的骇客技术，为什么动机不同就有不同名字？

📖 **讲师原句**："**Motives matter** — the same hacking skill can be used for **money, protest, or destruction**."

| Category | 中文 | 动机 | 🌰 讲师例子 |
|---|---|---|---|
| **Cybercriminals** | 网络罪犯 | **Financially motivated** 为钱 | **Stealing credit cards** |
| **Hacktivists** | 骇客行动主义者 | **Politically / socially motivated** 为抗议 | **Defacing a government website** to protest policy |
| **Cyberterrorists** | 网络恐怖分子 | **Large-scale harm** 制造恐慌 | **Attacking air traffic systems** to cause chaos |

---

## 🔁 自测：我问，你先想，答案在下面

1. 讲师说电脑在网络犯罪里可以是哪三种角色？
2. 员工不小心点了 phishing link，是 malicious 还是 accidental harm？
3. WannaCry 为什么传播得那么快？
4. 偷听 Wi-Fi 是 passive 还是 active？把 Wi-Fi router 灌爆呢？
5. 录下讯息再重发，是哪一种 active attack？
6. Virus 和 worm 的差别？Trojan 能不能自我复制？
7. Spam 的 5 种类型？
8. Facebook 付钱给 bug bounty hunter，他们是 hacker 还是 cracker？
9. 黑掉政府网站来抗议政策的人叫什么？

.

.

.

.

**答案**
1. **Weapon, target, or both**
2. **Accidental harm**
3. 很多人**没装 Windows update（patch）**
4. 偷听 = **Passive**（eavesdropping）；灌爆 = **Active**（DoS）
5. **Replay**
6. Virus **要人打开档案**才传，worm **自己自动传**；Trojan **cannot self-replicate**
7. **Email · Social media · Mobile phone (SMS/push) · Text message (IM) · SEO spam**
8. **Ethical hackers**（white hats）
9. **Hacktivist**

---

## 🧾 一页背诵（考前 5 分钟看）

- **Cybercrime** = illegal activity using the Internet · computer = **weapon / target / both** · **malicious** (create virus) vs **accidental** (click phishing link)
- **User expectations**：more data, audit trails · **share login** · **complexity** · **rapid change** · Wi-Fi password · fingerprint can't reset
- **Exploit** = weakness in software · **patch** fixes it · not installed → breach · **WannaCry**
- **Passive** = steal info, no damage, hard to detect：**eavesdropping, traffic monitoring**（silent thieves）
- **Active** = tamper / disrupt：**masquerade, replay, message alteration, DoS/DDoS**（burglars smashing windows）
- **Malware**：Virus（attaches, spreads when opened）· Worm（automatic）· Trojan（disguised, no self-replicate）· Botnet（infected PCs controlled）· Rootkit（hides attacker）· Phishing（trick to reveal info）
- **Spam ×5**：Email · Social media（fake profiles）· Mobile phone（SMS, push）· Text message（WhatsApp, Telegram）· SEO（search results）
- **Ethical hacker**（white hat, CEH, bug bounty）vs **Cracker**（black hat, personal gain）
- **Malicious insider** = employees / ex-staff abusing access, collude with outsiders
- **Cybercriminal**（money）· **Hacktivist**（protest）· **Cyberterrorist**（large-scale harm, air traffic）

➡️ 读完五章了！回到 `00_8H_Plan.md` 做最后的模拟考。
