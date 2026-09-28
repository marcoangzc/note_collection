# AMIS1012 Ethics in Computing — 高分版：先读这一页

> 讲师说得很清楚：**只写定义、只抄 revision notes 的点，拿不到高分。** 题目说 "provide a scenario / an example / apply to the company" 时，改卷的人要看的是：**你自己想出来的情境，而且细节具体**。
> 这一版笔记只做一件事：把 Ch1、Ch2、Ch6、Ch7、Ch8（讲师 tips 圈的五章）每一个考点，都写成"**点 + 解释 + 自己的具体情境 + 后果/对策**"的高分答法，并教你**在考场上 30 秒编出一个情境**。

---

## 1. 为什么"普通答案"拿不到高分

看同一题的两个写法（Q: *Explain confidentiality with an example.* 3 marks）：

**📉 普通答案（约 1–1.5 分）**
> Confidentiality means only authorised people can access data. Example: password-protected medical records.

**📈 高分答案（3 分）**
> Confidentiality means information is accessible only to authorised people. *Scenario:* MedikaCare, a clinic chain with 12 branches in Penang, stores 80,000 patient records in a cloud database. A receptionist should see only a patient's name and appointment time, while only the doctor on duty can open the diagnosis. MedikaCare enforces this with role-based access control and two-factor login, and encrypts the records with AES-256. If a leaked receptionist password is used by an outsider, the attacker still cannot read any diagnosis, so the clinic avoids a breach of the Personal Data Protection Act 2010 and keeps patients' trust.

差别不在英文，而在**具体程度**：谁、在哪里、多少人/多少钱、用了什么技术、发生了什么事、结果如何、违反什么法律。

---

## 2. 高分公式：P-E-S-C

每一个"点"都写四步：

| 步 | 写什么 | 一句话范例 |
|---|---|---|
| **P** – Point | 名称 / 立场，用题目的关键字 | *Availability* ensures … |
| **E** – Explain | 一到两句解释（定义 + 为什么重要） | … authorised users can access systems whenever needed. |
| **S** – Scenario | **你自己的情境**，至少 3 个具体细节（见 §3） | During KedaiKu's 11.11 sale, 18,000 customers … |
| **C** – Consequence / Control | 结果、法律、或该怎么做，**连回题目的公司** | … so PQR should add load balancing and a backup site. |

**分数怎么对应**：
- 2 分一点 = P+E（1）+ S+C（1）
- 3 分一点 = P（1）+ E（1）+ S+C（1）
- 4 分一点 = P（1）+ E（1）+ S（1）+ C（1）

**案例题（TAR、PQR、Cybertron、Ethan、Nintendo、Domitri…）**：S 一定要用**题目的公司**，再加上你自己补的细节（人名、数字、时间、技术）。不要写 "a company"。

---

## 3. 30 秒编情境：5W + 1N 清单

写 S 的时候，至少放进下面 **3 项**：

| | 问自己 | 例子 |
|---|---|---|
| **Who** | 谁？（名字 + 职位 / 公司 + 做什么） | Aina, a junior IT executive at KedaiKu Sdn Bhd |
| **Where** | 在哪里？哪个系统？ | the cloud POS at the Mid Valley branch |
| **When** | 什么时候？多频繁？ | at 2 a.m. during the 11.11 sale / every Friday |
| **What** | 具体做了什么？用什么技术？ | clicked a fake "parcel delivery" SMS link / used SQL injection |
| **How** | 怎么发生 / 怎么防？ | the rootkit hid the process from Task Manager |
| **Number** | **数字**：RM、人数、%、小时 | 46,000 customer records, RM250,000 in fines, 6 hours downtime |

**再加一个"结果"**：损失多少、谁受影响、违反哪条法律、公司该怎么做。

### 可以一直重复使用的"角色表"
考试时不用每题重新想公司，下面这些**你自己的角色**可以重复用（记住 3–4 个就够）：

| 角色 | 设定 | 适合哪章 |
|---|---|---|
| **KedaiKu Sdn Bhd** | Petaling Jaya 的网购杂货平台，50,000 名注册顾客，用 FPX 和信用卡付款，3 个仓库 | Ch2 攻击、Ch6 风险 / CIA、Ch8 商标 |
| **Aina** | KedaiKu 的 junior IT executive | Ch1 道德决策、Ch2 phishing |
| **Farid** | KedaiKu 的 system administrator（有最高权限） | Ch1、Ch2 insider、Ch6 policy |
| **MedikaCare** | 槟城 12 间分行的诊所连锁，80,000 份病历 | Ch1 privacy、Ch6 confidentiality |
| **BayarLah** | 一个马来西亚 e-wallet app，30 万用户 | Ch6、Ch7 银行 / 付款 app |
| **Studio Rimba** | Cyberjaya 的 4 人 indie game studio，游戏叫 *Hikayat Hutan* | Ch8 IP |
| **Kopi Kaki** | 雪隆 8 间分行的咖啡连锁 | Ch6 小公司风险、Ch8 trademark |

### 常用的马来西亚法律（写进 C，立刻显得具体）

| 法律 | 什么时候写 |
|---|---|
| **Computer Crimes Act 1997** | 未经授权进入电脑、hacking、修改资料 |
| **Personal Data Protection Act (PDPA) 2010**（2024 年修订） | 个人资料外泄、未经同意分享顾客资料 |
| **Communications and Multimedia Act 1998** | 网络上发布恶意 / 虚假内容、spam |
| **Cyber Security Act 2024 (Act 854)** | 关键资讯基础设施（银行、医疗、电讯）的网络安全义务 |
| **Copyright Act 1987** | 盗版软件、抄袭程序码、音乐、角色设计 |
| **Patents Act 1983** | 发明、演算法的技术实作 |
| **Trademarks Act 2019** | 品牌名、logo、仿冒 |
| **Penal Code s.420（cheating）** | 诈骗（例如 phishing 骗钱） |

（*MyIPO* = Intellectual Property Corporation of Malaysia，注册 patent / trademark 的机构；*NSRC 997* = National Scam Response Centre 热线；*CyberSecurity Malaysia – Cyber999* = 通报网络事件。）

---

## 4. "模糊 → 具体"替换表

| 📉 模糊 | 📈 具体 |
|---|---|
| a company | KedaiKu Sdn Bhd, an online grocery platform with 50,000 customers |
| a hacker steals data | an attacker used a stolen admin password to download 46,000 customer records at 3 a.m. |
| the system goes down | the checkout page was unavailable for 6 hours during the 11.11 sale, losing about RM120,000 in orders |
| an employee misuses the computer | Farid used the company server to mine cryptocurrency every night, doubling the electricity bill |
| a virus infects the computer | a macro virus in an "Invoice_Sept.docm" attachment spread to 40 PCs on the shared drive |
| the company is fined | the company faces a fine of up to RM1 million under the PDPA 2010 (as amended in 2024) |
| test the software | test the login form with a valid password, a wrong password, a blank field and a 300-character input |
| protect the IP | register "Hikayat Hutan" as a trademark with MyIPO under Class 9 and Class 41 |

⚠️ 数字不需要是真的（这是**你的情境**），但要**合理**：一间咖啡店不会有 1,000 万顾客。

---

## 5. 真实案例（可选的"加分句"）

情境以**自己编的**为主（讲师要的就是这个）；如果记得真实案例，可以在最后加一句当佐证：

| 主题 | 真实案例（只写你确定的部分） |
|---|---|
| 没装 patch → exploit | **WannaCry (2017)**：利用 Windows SMB 漏洞，Microsoft 两个月前已发布 patch，没更新的电脑（包括英国 NHS 医院）大量中招 |
| Botnet / DDoS | **Mirai (2016)**：感染大量 IoT 摄影机和路由器，组成 botnet，DDoS 攻击 DNS 供应商 Dyn，使多个大型网站无法访问 |
| 资料外泄 | **Equifax (2017)**：未修补的 Apache Struts 漏洞，约 1.47 亿人的个人资料外泄 |
| 马来西亚 | **2017 年**：媒体报道约 **4,620 万**笔马来西亚手机用户资料外泄 |
| 晚发现 defect 的代价 | **CrowdStrike (2024)**：一个有问题的更新让全球约 **850 万**台 Windows 电脑当机，航空、银行受影响 |
| 软件错误 | **Knight Capital (2012)**：部署错误的交易程序，**45 分钟**亏损约 **4.4 亿美元** |
| Patent | **Apple v Samsung (2012)**：美国陪审团判 Samsung 赔偿约 **10.5 亿美元** |
| Trade secret | **Coca-Cola** 的配方一百多年来以 trade secret 保护，从未申请 patent |
| 游戏 IP | **Nintendo 在 2024 年**对 *Palworld* 的开发商 Pocketpair 提出 **patent** 诉讼 |

---

## 6. 考场规则

1. **看分数写长度**：1 分 ≈ 1 个有内容的句子。9 分 "THREE" = 三段，每段 P-E-S-C。
2. **每一题都要写**（讲师原话：*leave no question empty*）。不会的题：写定义 + 一个你自己的情境，通常还能拿一半。
3. **案例题每句都叫公司名字**：TAR Sdn. Bhd.、PQR、Cybertron、Ethan's team、Nintendo、Domitri、Cadiz、Disney、the indie game developers。
4. **"Provide a scenario"** = 写一个小故事（谁 + 做了什么 + 结果）；**"Provide an example"** = 至少一句具体例子（有名字、有数字）。
5. **"Do you agree?"** 先写立场（*Yes, I agree*），再给理由 + 情境。
6. **"With the aid of a diagram"**（PDCA、8 steps）一定要画，图本身有分。
7. **时间**：4 题 × 30 分钟。每题先花 1 分钟**决定你要用哪个角色**（KedaiKu / MedikaCare / BayarLah / Studio Rimba），整题都用同一个，写起来最快。

---

## 7. 各章高分版

| 文件 | 考卷位置 | 内容 |
|---|---|---|
| `Ch1_Ethics_HighScore.md` | Q1 前半 | Morals / ethics / law（一个情境串三个）、5 steps（完整走一次）、IT users 三个问题、IT professionals、certifications |
| `Ch2_Crime_HighScore.md` | Q1 后半 | Cybercrime 两类、vulnerabilities、exploit、passive / active、malware、**五种 spam**、hacker 类型 |
| `Ch6_Risk_HighScore.md` | Q2 | **8 steps（带数字的风险表）**、**CIA（每个一个情境）**、四根柱子、security policies、ISO 27001、NIST |
| `Ch7_Software_HighScore.md` | Q3 | Waterfall / Agile 选择、defect 成本（带数字）、**black-box / white-box 真的 test case**、PDCA 图、quality attributes |
| `Ch8_IP_HighScore.md` | Q4 | IPR 重要性、四种 IP 怎么选、trademark spectrum（自己的品牌名）、**IP issues 各一个情境**、cybersquatting、被侵权时的行动 |

每个文件的结构：**考点 → 高分答法（附你可以改写的情境）→ 每一题 past year 的高分版 → 自己练习的新情境题**。
