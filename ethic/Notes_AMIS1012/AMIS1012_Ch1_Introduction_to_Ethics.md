# AMIS1012 — Chapter 1: Introduction to Ethics

> Slide 把 morals / ethics / law 各用一句话定义，然后放一张 Venn 图就结束了。但这一题是**每一份 past year 的 Q1a（8–9 分）**，考的是"区别 + IT 例子 + 三者的关系"，slide 没有给你一个能直接写的比较框架，也没有 IT 例子。这份笔记补上：一个比较表、Venn 图七个区域的意思、一个贯穿全章的 IT 场景，以及每种问法的满分答法。另外 slide 把 morals 说成"religious views"，太窄，这里也修正。

---

## 0. 一句话总览

**Ethics 是社会/专业对"什么行为可以接受"的共同标准；它和个人的 morals、国家的 law 有重叠但不相等。IT 让"做坏事"变得更容易、更隐形，所以 IT 人需要一套决策步骤（5 steps）、一套 professional code of ethics，并且要处理 IT users 常见的三种道德问题。**

| Part | 内容 | Slide |
|---|---|---|
| 1 | Definition of ethics / virtue & vice | p3–6 |
| 2 | Ethical decision making（5 steps 流程图） | p7–9 |
| 3 | **Morals vs Ethics vs Law**（Venn 图） | p10–12 |
| 4 | Why ethics matters in IT（public concern scenarios） | p13–19 |
| 5 | Ethics for IT professionals（特质、professional 定义、code of ethics、7 trends） | p20–29 |
| 6 | **Common ethical issues for IT users** + IT use policy | p30–34 |
| 7 | Certifications（vendor vs industry association） | p35–40 |

🎯 **这章回答的考题**
- **Morals / ethics / law 的区别 + 例子** — Oct 2024 Q1a (9)、Jan 2025 Q1a morals vs ethics (8)、May 2025 Q1a (9)、Oct 2025 Q1a 用 IT scenario (9)、May 2026 Q1b (6) → **五份全考**
- **Five steps of ethical decision-making** — Oct 2024 Q1b (5)
- **THREE common ethical issues faced by IT users** — May 2026 Q1a (9)

**Prerequisites**：没有。这是第一章。

---

## Scene：凌晨两点的 USB

Aina 是一家电商公司的 junior IT executive。凌晨两点，她在做 server migration，发现同事 Ken 的电脑装了一套**盗版**的设计软件；同时她自己有 admin 权限，可以看到所有员工的**薪水表**；老板还叫她"顺便"把旧客户资料 copy 一份给合作的营销公司。

没有人在看。她该怎么做？

- 心里觉得"偷看别人薪水不好"——这是 **morals**。
- 公司的 IT policy 和专业守则说"只能看你工作需要的资料"——这是 **ethics**。
- PDPA 规定未经同意不能把客户个人资料交给第三方——这是 **law**。

这一章就是把这三把尺子分清楚，再给 Aina 一个做决定的流程。

---

## 1. 什么是 Ethics？（p3–6）

- **Ethics** = a set of values that define what is and is not acceptable behaviour in a certain society（某个社会认为可以/不可以的行为标准）。
- **Computer ethics** = guidelines for the **morally acceptable use of computers** in society。
- 很多标准几乎是全世界通用的（说谎、偷窃是错的），但**看法也会因地而异**：slide 的例子是 **software piracy**——有些国家的人觉得无所谓，有些国家认为是错的。
- p4 的图是 Revenera 的 **Top 20 Software License Misuse and Piracy Hotspots**（China, Russia, United States, India, Brazil …）。重点不是背排名，而是：**连美国这种法律完善的国家也在前三名**——说明"有法律"不等于"大家都守 ethics"。
- **Virtue**（美德）= 鼓励人做对的事的习惯，如 fairness、honesty；**vice**（恶习，slide 写 "sin"）= 鼓励做错事的习惯，如 envy、rage。
- 哲学上的定义：ethics 是 philosophy 的一个分支，研究行为的**对错（rightness/wrongness）**以及动机和目的的**好坏（goodness/badness）**。

> 💬 **答题句 (EN)** — *Ethics is a set of values and standards that define what is acceptable and unacceptable behaviour in a society or profession; computer ethics applies these standards to the use of computers and information technology.*

---

## 2. Ethical Decision Making：五个步骤（p7–9）

Aina 不能只凭感觉。她需要一个流程。p7 的流程图是这样的（照 slide 的版面画）：

```
 ┌──────────────────────────────┐ <────────────────┐
 │ 1. Develop Problem Statement │                  │
 └──────────────┬───────────────┘                  │
                v                                  │
   ┌───────────────────────────┐                   │
   │ 2. Identify Alternatives  │                   │
   └────────────┬──────────────┘                   │
                v                                  │
   ┌────────────────────────────────┐              │
   │ 3. Evaluate & Choose Alternative│             │
   └────────────┬───────────────────┘              │
                v                                  │
     ┌──────────────────────┐                      │
     │ 4. Implement Decision│                      │
     └──────────┬───────────┘                      │
                v                                  │
     ┌──────────────────────┐                      │
     │ 5. Evaluate Results  │                      │
     └──────────┬───────────┘                      │
                v                                  │
           < SUCCESS? > ──────── NO ───────────────┘
                │ YES
                v
              [ OK ]
```
*看图重点：失败（NO）就回到第 1 步重新定义问题——这是一个循环，不是一条直线。*

| Step | 做什么 | Aina 的例子 |
|---|---|---|
| 1. **Develop problem statement** | 用一两句清楚写出**到底要解决什么问题**（事实、谁受影响） | "老板要求我把客户资料交给第三方营销公司，但客户没有同意。" |
| 2. **Identify alternatives** | 找 stakeholders 一起想**多个**方案 | ① 照做 ② 拒绝 ③ 先问 legal/DPO，取得客户同意后才分享 ④ 只给匿名化资料 |
| 3. **Evaluate & choose alternative** | 用多个指标比较：**效果、风险、成本、实施时间**（也考虑法律、道德、对 stakeholders 的影响） | ① 违法、风险最高；③ 合法、成本稍高 → 选 ③ |
| 4. **Implement decision** | **及时、可靠、有效**地执行 | 写 email 给老板和 DPO，建立 consent 流程 |
| 5. **Evaluate results** | 追踪结果：目标达到了吗？对公司和各 stakeholders 有什么影响？ | 一个月后检查：有没有投诉？营销效果如何？不成功 → 回到 step 1 |

⚠️ Slide p8 表格把 step 2 写成 "Identity Alternatives"，是 typo，正确是 **Identify** Alternatives。

**为什么道德决策很难？（p9）** 大多数 ethical choices 涉及：
a. several options（多个选项） b. long-term consequences（长期后果） c. unpredictable outcomes（结果难预测） d. 混合了 fiscal、legal、social 的利益和成本 e. personal ramifications（对自己有影响）。
所以 organisational ethics 不是非黑即白，要把 **economic / self-interest** 和 **legal / ethical / social** 的利弊放在一起权衡。

> 💬 **答题句 (EN)** — *Ethical decision-making follows five steps: develop a problem statement, identify alternatives, evaluate and choose an alternative, implement the decision, and evaluate the results; if the result is unsuccessful, the process restarts from the problem statement.*

### ✅ 满分答法 — Oct 2024 Q1b: Describe the FIVE major steps involved in ethical decision-making. (5 marks)

1 分一步，每步一句"做什么"：

1. **Develop a problem statement** – write a clear, concise description of the issue to be solved, including the facts and the people affected.
2. **Identify alternatives** – involve stakeholders to generate several possible solutions to the problem.
3. **Evaluate and choose an alternative** – compare the alternatives using criteria such as effectiveness, risk, cost and implementation time, and choose the best one.
4. **Implement the decision** – carry out the chosen solution in a timely, reliable and effective manner.
5. **Evaluate the results** – monitor the outcome to check whether the desired result was achieved and how it affects the organisation and stakeholders; if not successful, return to step 1.

---

## 3. Morals vs Ethics vs Law（p10–12）⭐ 每年必考

### 3.1 Slide 的定义

- **Morals** – one's (slide 说 religious) views on what is right and wrong。
- **Ethics** – the rules or codes of conduct that a **society (nation, institution, or profession)** expects of an individual。
- **Law** – a set of rules that determines what we are **permitted / not permitted** to do（由政府制定、强制执行）。

⚠️ Slide p10 说 morals 是 "religious views"。这太窄：morals 是**个人**的是非观，来源可以是宗教，也可以是家庭、文化、个人经验。考试写 "personal beliefs of right and wrong, which may come from religion, culture or upbringing" 最安全，既包含 slide 的说法又更准确。

### 3.2 最重要的比较表（slide 没有，考试最好用）

| 比较点 | Morals | Ethics | Law |
|---|---|---|---|
| 是谁的标准？ | **个人** (individual) | **群体/专业/社会** (society, profession, organisation) | **国家/政府** (state) |
| 来源 | 宗教、家庭、文化、良心 | Code of conduct, professional code of ethics, company policy | 立法机关 (parliament) |
| 是否写下来 | 通常没有 | 通常有 (codes, policies) | 一定有 (statutes, acts) |
| 违反的后果 | 内疚、罪恶感 | 被专业团体/公司处分、名声受损、被开除 | 罚款、坐牢、民事赔偿 |
| 是否强制 | 否 | 在该群体内半强制 | 是，由国家强制执行 |
| 会不会因人/地而异 | 因人而异 | 因专业/组织而异 | 因国家而异 |
| IT 例子 | 觉得偷看同事 email 是不对的，即使没有人规定 | ACM/公司 code 规定不能用公司电脑做私人生意；IT staff 只能存取工作需要的资料 | **Computer Crimes Act 1997**：未经授权进入电脑系统是犯罪；**PDPA 2010**：不可未经同意披露个人资料；**Copyright Act 1987**：盗版违法 |

### 3.3 Slide 的两个例子（p11）

- **合法但有人觉得不道德**：abortion（堕胎）在某些国家合法，但一些人从 morals 上反对。→ *legal ≠ moral*。
- **符合 ethics 但违反个人 morals**：律师的 professional ethics 要求他尽全力为当事人辩护，**即使他知道当事人犯了可怕的罪**。→ *ethical (for the profession) ≠ moral (for the person)*。

IT 版本（考试写这种会加分）：
- **Legal but unethical**：公司在员工合约里写明"会监控所有键盘输入"，法律允许；但若连员工的私人讯息都看，很多人认为不 ethical。
- **Ethical but illegal**：Ethical hacker 为了证明漏洞，在**没有授权**的情况下测试一家银行的系统，动机是好的，但违反 Computer Crimes Act。
- **Moral but against company ethics**：员工觉得帮朋友下载公司内部软件"只是帮忙"，但违反公司 IT policy。

### 3.4 Venn 图（p12，Figure 1）

```
                 .-----------.
               /    ETHICS     \
              |                 |
              |   EM   .--. EL  |
         .----+-------/ EML\----+----.
       /       \     |------|  /       \
      | MORALITY '---| ML  |--'   LAW   |
      |              '----'              |
       \                                /
        '------------------------------'
```
*看图重点：三个圆互相重叠——大部分"对"的事同时是 moral、ethical、legal（中间 EML），但每个圆都有只属于自己的部分。*

| 区域 | 意思 | IT 例子 |
|---|---|---|
| **EML** | 三者都符合 | 不散布电脑病毒：个人觉得不对、专业守则禁止、法律禁止 |
| **EM**（ethics + morality，不是法律） | 道德上和专业上都对，但法律没规定 | 主动告诉客户软件有已知的小 bug |
| **EL**（ethics + law，个人不一定认同） | 专业和法律要求，个人可能不认同 | 按 PDPA 删除一位"讨厌的"前客户的资料 |
| **ML**（morality + law，专业守则没提到） | 个人和法律都认为对 | 不偷别人的笔记本电脑 |
| 只 Morality | 纯个人信念 | 自己决定不玩暴力游戏 |
| 只 Ethics | 专业/公司规则 | 公司规定不能在公司网络上玩游戏 |
| 只 Law | 法律要求但与个人道德无关 | 公司要按时提交 data breach 报告格式 |

> 💬 **答题句 (EN)** — *Morals are an individual's personal beliefs about right and wrong; ethics are the codes of conduct expected by a society or profession; laws are rules enforced by the government with legal penalties. An act can be legal but unethical, or ethical but illegal, so the three overlap but are not the same.*

### ✅ 满分答法 — Oct 2024 Q1a / May 2025 Q1a: Discuss the differences among morals, ethics and law. Provide an example (related to IT) for each term. (9 marks)

9 分 = 3 个 term × (定义 1 + 特点/区别 1 + IT 例子 1)。

- **Morals** are an individual's own beliefs about what is right and wrong, shaped by religion, culture and upbringing. They are personal, unwritten and differ from person to person; breaking them leads to guilt rather than punishment. *Example:* an IT technician who repairs a customer's laptop does not open the customer's private photos because he personally believes it is wrong, even though nobody would find out.
- **Ethics** are the rules or codes of conduct that a society, organisation or profession expects its members to follow. They are usually written (e.g. a professional code of ethics or company IT policy) and are enforced by the group through disciplinary action. *Example:* a company's code of conduct states that employees must not use company computers or software for personal business, and staff who do so may be disciplined.
- **Law** is a set of rules made and enforced by the government that states what people are permitted or not permitted to do; breaking it leads to fines, imprisonment or civil claims. *Example:* under Malaysia's Computer Crimes Act 1997, accessing a computer system without authorisation is a criminal offence.
- **Difference / relationship:** the three overlap but are not identical — software piracy may be accepted by some individuals' morals yet is illegal under the Copyright Act 1987; monitoring employees' emails may be legal yet considered unethical if done without notice.

### ✅ 满分答法 — Oct 2025 Q1a: Provide a scenario related to IT to explain the differences among morals, ethics, and law. (9 marks)

用**一个**场景串三个概念（和上面不同，题目要 scenario）：

*Scenario:* Daniel is a database administrator at an online retailer. His manager asks him to secretly copy customer records and give them to a partner marketing company.
- **Morals:** Daniel personally feels it is wrong to betray customers' trust, because his own values tell him that taking someone's information without permission is dishonest. This is his individual moral judgement.
- **Ethics:** The company's code of conduct and the professional code of ethics for IT professionals require him to protect confidential data and to access it only for authorised work purposes. Sharing it would breach professional ethics and could lead to disciplinary action by his employer or professional body.
- **Law:** Malaysia's Personal Data Protection Act 2010 prohibits disclosing personal data to third parties without the data subject's consent. If Daniel shares the data, he and the company may face fines or imprisonment.
- **Difference:** morals guide Daniel as an individual, ethics are the standards expected by his profession and company, and law is enforced by the state with penalties. Here all three point the same way, but they may conflict — e.g. if the manager had obtained legal consent through fine print, the sharing might be legal but still unethical.

### ✅ 满分答法 — Jan 2025 Q1a: Distinguish between morals and ethics in the context of IT, with an example of each. (8 marks)

| | Morals | Ethics |
|---|---|---|
| Source | Individual conscience, religion, culture, upbringing | Society, organisation or profession (codes of conduct) |
| Scope | Personal — differs from person to person | Shared by a group — same for all members |
| Form | Unwritten | Usually written (code of ethics, IT policy) |
| Enforcement | Self-enforced (guilt) | Enforced by the group (warning, dismissal, loss of certification) |
| IT example | A programmer refuses to write a game that he feels promotes gambling, even though his company and the law allow it | A certified security professional must follow the (ISC)² code of ethics and must not disclose a client's vulnerabilities to others |

(8 分：4 个比较点各 1 分 + 两个例子各 2 分。)

### ✅ 满分答法 — May 2026 Q1b: Describe morals, ethics, and laws in computing. (6 marks)

每个 2 分（定义 + computing 例子），用 3.3 或上面的例子，每个两句即可。

---

## 4. 为什么 IT 特别需要 Ethics？（p13–19）

- Information systems 和社会、政治事务交织，所以"资料怎样被处理"变得更重要。
- 电子设备进入了政府、工作、个人生活的每个角落——**连没有用电脑的人也受影响**（例如他们的资料被别人存进系统里）。
- 电子知识系统的问题：**records 的管理与存取、privacy 与 data manipulation、国际影响**，特别出现在 networks、databases、GIS。
- **Privacy policy 的难处（p15–16）**：公司给员工 email 和电脑，员工会用来发私人讯息；新技术可以追踪 email、message board、浏览记录。公司想确保员工专心工作、发现 ethical lapses，**但单凭"员工访问过某网站"无法判断动机**（可能是查资料，也可能是偷懒）。
- **Internet 普及 + 可收集大量个人资料 + 高度依赖电脑系统 → 被不道德使用的机会增加（p17）。**

**引起公众关注的六种情况（p18–19）**：

| Scenario | 问题在哪 |
|---|---|
| Tracking employee email and Internet usage | 管理层要生产力，员工要自主与隐私，要取得平衡 |
| Downloading free music and movies | 违反 copyright，版权持有人损失大（→ Ch8） |
| Spamming | 用低成本的 unsolicited email 大量发送（→ Ch2） |
| Hackers and identity fraud | 入侵金融/零售机构，盗取客户资料冒用身份、刷卡（→ Ch2/Ch3） |
| Plagiarism | 学生从网上抄袭作业（→ Ch8） |
| Cookies and spyware | 网站在用户硬盘放 cookies/spyware 追踪行为（→ Ch5） |

> 这张表几乎就是整门课的目录：每一行都会在后面某一章展开。

---

## 5. Ethics for IT Professionals（p20–29）

### 5.1 好的 IT 从业员的特质（p21–22）

| Characteristic | 说明 |
|---|---|
| **Patience** | 要不断分析、试错；不要急着下结论 |
| **Able to communicate effectively** | 对不同客户要清楚、简洁 |
| **Able to adapt quickly** | 技术一直变，要快速学新技术 |
| **Multitasking** | 同时处理多项任务 |
| **Problem-solving ability** | 遇到陌生问题，用自己的方法 + 经验 + 学识解决 |
| **Excited about the job** | 对工作有热情，愿意学习并应用 |

### 5.2 谁算 "professional"？（p23–25）

US Code of Federal Regulations 的四个条件：
1. 工作需要**高级专业知识**（通常经长时间的专门训练/研究获得）；
2. 工作是**原创、有想象力**的，结果取决于员工自己的发明、想象或能力；
3. **持续运用判断力 (discretion and judgment)**；
4. 工作主要是**智力性、多样化**的，产出无法在特定时间内标准化。

另外 professionals 被期待**回馈社会、终身学习 (lifelong learning)**，并帮助其他专业人士成长。

### 5.3 Professional code of ethics（p26–27）

- 定义：列出某职业群体实践的**核心理想和标准**。
- 通常分两部分：**① 组织的目标 (goals)；② 会员要遵守的准则和标准 (guidelines & standards)**。
- **"Laws should not serve as a comprehensive roadmap to ethical conduct"**——法律只是底线，不是全部。

遵守 code of ethics 的四个好处（p27）：

| Benefit | 说明 |
|---|---|
| **Ethical decision-making** | 大家根据一套共同的原则做决定 |
| **High ethical principles and practice** | 提醒专业人士在日常压力下不要违反责任；规定什么可以、什么不可以 |
| **Public interest and confidence** | 公众相信专业人士会说真话、不给 self-serving 的建议、会警告风险 → 增加信任与尊重 |
| **Self-evaluation benchmark** | 自我检视的标准；同行也可据此赞扬或批评 |

### 5.4 重塑专业服务业的七个趋势（p28–29）

| Trend | 说明 |
|---|---|
| **Client sophistication** | 客户更清楚自己要什么，更会外包、更会压价 |
| **Governance** | 丑闻后法律更严（如 **Sarbanes-Oxley Act 2002**），客户与供应商之间信任减少、监督增加 |
| **Interconnectedness** | 用电子会议、email、无线设备随时随地沟通 |
| **Discretion** | 客户要**实时**监控、管理进行中的工作，不再等最终成品才给意见 |
| **Modularization** | 客户把业务流程拆成基本步骤，决定哪些自己做、哪些外包 |
| **Globalization** | 可以在全世界选供应商，竞争激烈 |
| **Commoditization** | 低端服务（如临时补人手）被视为商品，只比价格；高端服务（如 IT business plan）则寻求 partnership |

⚠️ "Discretion" 这一行的描述其实是 **transparency（透明度）**——客户要看到实时进度。考试照 slide 写 "Discretion" 就好，但你理解成"客户要求透明、实时参与"。

---

## 6. Common Ethical Issues for IT Users（p30–34）⭐ May 2026 考过 9 分

回到 Aina 的场景，三个问题刚好都出现了：

### 6.1 Software piracy（盗版）
- 在企业里，盗版常可追溯到 **IT 人员**——他们可能鼓励或直接参与（例如为了省钱，把一个 license 装在 50 台电脑）。
- 公司的 IT 存取与管理制度应该让员工能**举报和质疑**盗版行为。
- *Aina 场景*：Ken 电脑里的盗版设计软件。

### 6.2 Inappropriate use of computing resources（不当使用电脑资源）
- 员工用公司电脑上与工作无关的网站、聊天室、玩游戏 → **消耗资源、降低生产力**。
- 分享不当内容、笑话、发 hate email → 可能让公司被指控**纵容种族歧视或性骚扰的工作环境**。

### 6.3 Inappropriate sharing of information（不当分享资讯）
- 公司资料分两类：
  - **Private data**：员工的薪水、出勤、健康记录、绩效评估；客户的信用卡号、电话、住址。
  - **Confidential information**：销售和营销日程、生产流程、配方、战术与战略计划、R&D。
- IT user 若（即使是**无意地**）把这些资讯给了未授权的人 → 侵犯隐私，或让商业机密落入竞争对手手中。
- *Aina 场景*：看到全体员工薪水、被要求把客户资料给营销公司。

### 6.4 公司怎么应对：IT use policy 的三个做法（p34）

| Guideline | 说明 |
|---|---|
| **Establishing guidelines for use of company software** | 规定电脑与软件的使用；与供应商签约让员工在家也用正版；协助员工取得合法 copy |
| **Defining and limiting the appropriate use of IT resources** | 书面规定、传达、执行；允许有限度的私人使用，但禁止上不良网站、用公司 email 发骚扰讯息 |
| **Structuring information systems to protect data and information** | 限制资料存取只给**需要的人**（如 sales manager 只能看自己负责的 sales 资料，不能看 R&D 结果）→ 就是 **least privilege / need-to-know** |

> 💬 **答题句 (EN)** — *Common ethical issues for IT users include software piracy, inappropriate use of computing resources, and inappropriate sharing of private or confidential information; organisations address them through an IT usage policy that sets software guidelines, limits the use of IT resources, and restricts data access to those who need it.*

### ✅ 满分答法 — May 2026 Q1a: Explain THREE (3) common ethical issues faced by IT users. (9 marks)

3 × 3 分（定义 + 为什么是问题/后果 + 例子）：

1. **Software piracy** – the unauthorised copying, installation or distribution of software. In organisations, IT staff sometimes encourage or take part in it to save costs. It deprives software developers of income and exposes the company to legal action and malware. *Example:* an IT officer installs one licensed copy of Microsoft Office on 30 office PCs.
2. **Inappropriate use of computing resources** – using company computers and networks for activities unrelated to work, such as online gaming, chatting or browsing entertainment sites. It wastes bandwidth and reduces productivity, and sharing offensive jokes or hate emails can lead to claims of a hostile or harassing workplace. *Example:* an employee streams movies on the office network during working hours.
3. **Inappropriate sharing of information** – disclosing private data (e.g. employee salaries, customer credit card numbers) or confidential business information (e.g. R&D results, marketing plans) to unauthorised people, even accidentally. This violates individuals' privacy and may let competitors obtain trade secrets. *Example:* an employee forwards a spreadsheet of customer addresses to a friend who runs a delivery business.

---

## 7. Certifications（p35–40）

- **Certification** = 由认证机构颁发，证明某人具备某套技能/知识。
- 雇主的看法两极：有人视为**掌握基本技能的指标**；有人**怀疑**，因为证书不能代替经验，也不能保证工作表现。
- 好处：正式提升技能、得到认可、有时**加薪和改善职业前景**；通常需要通过笔试。

| | Vendor certification | Industry association certification |
|---|---|---|
| 谁发 | 厂商 | 行业协会 |
| 内容 | 配置、实施、管理、排除故障**该厂商的产品** | 更广的视野；越来越多学术、行业、行为能力（如 project management、network security） |
| 例子 | **Cisco**, **Microsoft**, **AWS** certifications | **(ISC)²**（如 CISSP）, **ISACA**（如 CISA, CISM） |
| 要求 | 通过考试 | 需有资格与经验、通过考试、**遵守 code of ethics**、付年费、continuing education credits，有时要定期更新考试 |

⚠️ 注意 industry association certification 要求**遵守 code of ethics**——这是本章和"ethics"的连结点，可以用在 code of ethics 相关的答案里。

---

## Closing the loop

回到 Aina：她用 5 steps 分析——问题是"未经同意分享客户资料"；选项中"先取得同意"最合适；她也知道盗版和偷看薪水属于 IT users 常见的 ethical issues，公司应该有 IT use policy 来管。这些行为有时只是 **unethical**，有时已经 **illegal**。

下一章看的是完全越过线的人：**cybercriminals**——他们用什么攻击、为什么攻击会越来越多。

---

## ⚠️ Where the slides mislead

| Slide | Slide 说 | 更准确的理解 |
|---|---|---|
| p8 | "Identity Alternatives" | Typo：**Identify** Alternatives |
| p10 | Morals refer to one's **religious** views | Morals 是个人的是非观，宗教只是来源之一（还有文化、家庭、良心） |
| p10 | Law is "a set of laws that determines…" | 循环定义；应是 "a set of rules made and enforced by the government" |
| p5 | "a sin is a habit…" | 课本通常用 **vice**（p5 后面也用 "vices"）；两个词考试都可 |
| p29 | Trend "Discretion" 的描述 | 描述的其实是 **transparency**（客户要实时看到进度）；考试照 slide 名称写即可 |

考试策略：题目引用 slide 原句时，照 slide 的说法作答，再补一句更完整的解释。

---

## Term table

| English | 中文 | 一句话说明 |
|---|---|---|
| Ethics | 伦理 | 社会/专业对可接受行为的标准 |
| Computer ethics | 电脑伦理 | 使用电脑时的道德准则 |
| Morals | 道德 | 个人的是非观 |
| Law | 法律 | 政府制定并强制执行的规则 |
| Virtue / Vice | 美德 / 恶习 | 让人做对 / 做错的习惯 |
| Problem statement | 问题陈述 | 清楚描述要解决的问题 |
| Stakeholder | 利益相关者 | 会受决定影响的人 |
| Professional code of ethics | 专业道德守则 | 职业群体的理想与行为标准 |
| Software piracy | 软件盗版 | 未经授权复制/使用软件 |
| Private data | 私人资料 | 员工/客户的个人资料 |
| Confidential information | 机密资讯 | 公司商业机密 |
| IT use policy | IT 使用政策 | 公司规定如何使用 IT 资源 |
| Vendor certification | 厂商认证 | Cisco、Microsoft、AWS 等 |
| Industry association certification | 行业协会认证 | (ISC)²、ISACA 等 |
| Sarbanes-Oxley Act (SOX) | 萨班斯法案 | 2002 美国法律，加强企业治理与财报透明 |

---

## Cheat sheet

- **Ethics** = society/profession 的 code；**Morals** = 个人；**Law** = 政府 + 惩罚。三者重叠（Venn：EM, EL, ML, EML）。
- 例子：legal but unethical（合法监控私人讯息）；ethical but illegal（未授权的 ethical hacking）。
- **5 steps**：Problem statement → Identify alternatives → Evaluate & choose → Implement → Evaluate results →（失败就回到 1）。
- Ethical choices 难：several options, long-term consequences, unpredictable, mixed costs/benefits, personal ramifications。
- **Professional**（4）：advanced expertise；original/creative；discretion & judgment；intellectual & non-standardised。
- **Code of ethics** 两部分：goals + guidelines；好处（4）：ethical decision-making, high principles, public confidence, self-evaluation benchmark。
- **7 trends**：client sophistication, governance, interconnectedness, discretion, modularization, globalization, commoditization。
- **IT users 三个 issue**：software piracy, inappropriate use of resources, inappropriate sharing of information。
- **IT use policy**（3）：software guidelines, define/limit use, structure IS to protect data。
- **Certifications**：vendor（Cisco/Microsoft/AWS）vs industry association（(ISC)², ISACA）。

---

## Practice (answers included)

### A. MCQ

1. A lawyer defends a client she knows is guilty because her profession requires it. This best illustrates:
   (a) law overriding ethics (b) professional ethics conflicting with personal morals (c) morals overriding law (d) an illegal act
2. Which step of ethical decision-making involves consulting stakeholders to find multiple solutions?
   (a) Develop problem statement (b) Identify alternatives (c) Implement decision (d) Evaluate results
3. A sales manager is blocked from viewing R&D results on the company network. Which IT use policy practice is this?
   (a) Establishing guidelines for company software (b) Defining appropriate use of IT resources (c) Structuring information systems to protect data (d) Certification
4. Which is an industry association certification?
   (a) AWS Solutions Architect (b) Cisco CCNA (c) ISACA CISA (d) Microsoft Azure Fundamentals
5. Employee salary records are classified as:
   (a) confidential business information (b) private data (c) public data (d) trade secrets

**Answers:** 1-b, 2-b, 3-c, 4-c, 5-b（slide p32：wage information 属于 worker's private data）

### B. Short answer

**B1. State FOUR benefits of adhering to a professional code of ethics. (4 marks)**
Ethical decision-making based on shared principles; high ethical principles and practice (reminds professionals of their responsibilities); public interest and confidence (people trust the profession); a self-evaluation benchmark for professionals and their peers.

**B2. Why are most ethical decisions in organisations "not black-and-white"? (4 marks)**
Because they usually involve several options, long-term consequences, unpredictable outcomes, and a mix of financial, legal and social costs and benefits, as well as personal consequences for the decision-maker. Managers must weigh economic and self-interest against legal, ethical and social factors.

**B3. List the four criteria of a person "working in a professional capacity" (US Code of Federal Regulations). (4 marks)**
Work requiring advanced knowledge gained through prolonged specialised study; original and creative work depending on the person's invention or talent; consistent exercise of discretion and judgment; predominantly intellectual and varied work whose output cannot be standardised over time.

### C. Application

**C1.** *An IT executive discovers that his company installs one licence of a design program on 40 computers to save cost.* Identify the ethical issue and apply the five steps of ethical decision-making. (8 marks)

Issue: **software piracy**.
1. Problem statement: "The company is using unlicensed copies of software, which breaches the licence agreement and copyright law."
2. Alternatives: ignore it; report it to the IT manager; propose buying the required licences; switch to a free/open-source alternative.
3. Evaluate: ignoring it risks legal action and malware; buying licences costs money but removes legal risk; open-source reduces cost but needs training. Choose: report and propose licensing or open-source alternative.
4. Implement: write a formal report to the IT manager with costs and options; uninstall illegal copies after approval.
5. Evaluate: audit software after one month; if copies remain, escalate again (back to step 1).

**C2.** *Give one IT example each of (i) legal but unethical, (ii) ethical but illegal.* (4 marks)
(i) A company legally tracks all employee web browsing (disclosed in the contract) but managers read employees' personal chats for gossip — legal but unethical. (ii) A security researcher tests a bank's website without permission to warn the bank of a flaw — good intention (ethical to some) but illegal under the Computer Crimes Act 1997.

**C3.** *A staff member emails a customer list to her personal Gmail "to work from home". Which ethical issue is this and which IT use policy practice would prevent it?* (4 marks)
Inappropriate sharing of information (private customer data leaves the company's control). Prevent by structuring information systems to protect data — restrict export/download rights to those who need them, and by defining appropriate use (forbid sending company data to personal accounts).

### D. Thinking

**D1.** "If something is legal, it is ethical." Do you agree? (4 marks)
Disagree. Law is only the minimum standard enforced by the state and often lags behind technology. Many acts are legal but unethical — e.g. collecting large amounts of user data with consent hidden in long terms and conditions. Professionals should follow a code of ethics that goes beyond the law.

**D2.** Should employers value certifications over experience? (4 marks)
Both matter. Certifications show verified mastery of a set of skills and commitment to learning (and industry certifications require following a code of ethics), but they do not guarantee on-the-job performance. Employers should use certifications as one indicator, together with experience and interviews.

---

## Slide index

| Note section | Slides |
|---|---|
| 1 Definition of ethics | p3–6 |
| 2 Ethical decision making | p7–9 |
| 3 Morals / ethics / law | p10–12 |
| 4 Ethics in IT | p13–19 |
| 5 Ethics for IT professionals | p20–29 |
| 6 Ethical issues for IT users | p30–34 |
| 7 Certifications | p35–40 |

## Links to other chapters
- Piracy, plagiarism → **Ch8** Intellectual Property
- Spam, hackers, identity fraud → **Ch2** Computer & Internet Crime
- Email/Internet tracking, cookies → **Ch3** (workplace monitoring) & **Ch5** (cookies)
- Code of conduct in organisations → **Ch4** ICT code of conduct
