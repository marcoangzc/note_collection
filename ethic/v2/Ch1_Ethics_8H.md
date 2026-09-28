# Ch1 道德导论 · Introduction to Ethics

> ⏱ **建议 1 小时 15 分** · 📝 **Q1 前半（约 11 分）**：**Morals / Ethics / Laws 五份都考**
> 📖 范围 = 讲师 Revision Notes 的 Ch1 全部 6 节 + **5 steps of ethical decision-making**（讲师没写，但 Oct 2024 考了 5 分）
> **怎么读**：每一节先看 ❓，自己想 5 秒 → 读"大白话" → 看例子 → 背 📖 讲师原句和 💬 答题句 → 最后看 ✅ 真题满分答案。

---

## 🗺️ 全章地图（Tree map）

```
                               Ch1 Introduction to Ethics
                                          │
     ┌───────────────┬──────────────┬─────┴────────┬────────────────┬─────────────────┐
     ▼               ▼              ▼              ▼                ▼                 ▼
 ① What is       ② Morals vs    ③ Why ethics   ④ Ethics for     ⑤ 3 issues for    ⑥ Certifications
   Ethics          Ethics vs      in IT          IT Professionals  IT Users
 · values of       Laws ⭐       · privacy       · 4 characteristics · Software piracy  · Vendor
   society       · based on     · data security · code of ethics    · Inappropriate      (Cisco, AWS)
 · invisible     · nature       · global        · IEEE, ACM           use of IT        · Industry
   guidebook     · example        concerns                          · Inappropriate      association
 · computer                                                           sharing of info    ((ISC)², ISACA)
   ethics                    （+ 5 steps of ethical decision-making，考过）
```

## 🎯 考试怎么问（五份 past year 的 Q1 前半）

| 考点 | Oct 2024 | Jan 2025 | May 2025 | Oct 2025 | May 2026 |
|---|---|---|---|---|---|
| **Morals / Ethics / Law** | 1a (9) | 1a morals vs ethics (8) | 1a (9) | 1a 用情境 (9) | 1b (6) |
| 5 steps of decision-making | 1b (5) | | | | |
| **3 ethical issues of IT users** | | | | | 1a (9) |

👉 **Morals / Ethics / Law 每年都考，6–9 分。** 这一章最重要的就是第 2 节。

---

## 1. What is Ethics? 什么是道德伦理

### ❓ 法律没有规定的事，就可以随便做吗？

**大白话**
法律不可能把每件事都写进去。例如：法律没有规定你不能在朋友背后说坏话，但大家都知道这样不好。社会上有一套**大家都认同的"什么可以做、什么不可以做"的标准**，这就是 **ethics（伦理）**。讲师把它比喻成一本**"看不见的规则书"（invisible guidebook）**——法律管不到的地方，就靠它来判断。

**Computer ethics（电脑伦理）**就是把这套标准用在电脑上：怎样用电脑才算道德上可以接受？例如：可以偷看同事的 email 吗？可以下载盗版软件吗？

有些事**到处都被认为是错的**（说谎、偷东西），但有些事**不同地方的看法不一样**。

### 🔑 关键词

| English | 中文 | 一句话 |
|---|---|---|
| **Ethics** | 伦理 / 道德规范 | 社会认为可以接受的行为标准 |
| **Computer ethics** | 电脑伦理 | 怎样用电脑才算道德上可以接受 |
| **Software piracy** | 软件盗版 | 没有 license 就复制、安装软件 |
| **Culture differences** | 文化差异 | 同一件事，不同国家有不同看法 |

📖 **讲师原句（Revision Notes）**
- "**Ethics = a set of values that define acceptable behaviour in society.**"
- "Ethics helps society decide what is **right or wrong** behaviour."
- "Ethics acts like an **'invisible guidebook'** for behavior **when laws don't cover everything**."
- "**Computer ethics = guidelines for morally acceptable use of computers.**"
- "**Universally wrong**: lying, stealing. What's ethical in one place may be **unethical elsewhere**."
- "**Culture differences**: e.g., software piracy might be tolerated in some countries but illegal in others."

🌰 **讲师的例子**
- **盗版 Microsoft Office**：在马来西亚，没 license 复制 Office 是 **unethical and illegal**；但在某些国家，人们还觉得"很正常"。→ 这就是**文化差异**。
- **用 ChatGPT 写功课**：如果讲师没有禁止，算不算 unethical？有人说**算**（不诚实 dishonesty），有人说**不算**（聪明地使用工具 smart use of tools）。→ 说明 ethics **不一定黑白分明**，要自己判断。

> 💬 **答题句 (EN)** — *Ethics is a set of values that define acceptable behaviour in society, helping people decide what is right or wrong. It acts as an "invisible guidebook" when laws do not cover everything. Computer ethics are guidelines for the morally acceptable use of computers.*

---

## 2. Morals, Ethics, and Laws ⭐⭐⭐（每年必考）

### ❓ 2.1 "我觉得不对"、"公司规定不能"、"法律禁止"，这三个一样吗？

**大白话**
不一样。用一个例子想：你在公司看到同事的电脑没锁，萤幕上是他的私人讯息。
- 你**自己觉得**偷看别人的讯息是不对的 → 这是 **Morals（个人道德）**：来自你自己的宗教、家庭教育、良心。**每个人不一样**，违反了顶多内疚，不会被罚。
- 公司的**员工守则**规定不可以看别人的资料 → 这是 **Ethics（伦理规范）**：一个**群体 / 专业 / 公司**对成员的要求。违反了会被公司警告、开除，或被专业团体处分。
- **法律**规定未经授权进入别人的电脑是犯罪 → 这是 **Laws（法律）**：**政府**订的规则，**警察和法院**会执行，违反了会被罚款、坐牢。

📖 **讲师的比较表**（考试照这个结构写：Based on / Nature / Example）

| Aspect | **Morals** 个人道德 | **Ethics** 伦理规范 | **Laws** 法律 |
|---|---|---|---|
| **Based on** 根据什么 | **Religion or personal belief** | **Society/profession's code of conduct** | **Government rules** |
| **Nature** 性质 | **Individual & internal** | **Collective & professional** | **Enforceable by police/courts** |
| **Example** 讲师例子 | Believing **abortion is wrong** | **IT professional code against piracy** | **Abortion legalized** in some countries |
| 违反的后果（补充） | 内疚、良心不安 | 被公司 / 专业团体处分、开除 | 罚款、坐牢 |
| 大白话 | "我觉得" | "我们这一行 / 公司规定" | "国家规定" |

### ❓ 2.2 讲师为什么用"律师"做例子？

📖 **讲师的对照例子（Contrast）**：*"A **lawyer** may **ethically defend a guilty client** (professional duty), though it may seem **immoral** to some."*

**大白话**：律师的**专业伦理**要求他尽全力替客户辩护，就算客户真的有罪。对律师这一行来说，这是**对的（ethical）**；但有些人**个人觉得**帮坏人脱罪是**不对的（immoral）**。
→ 意思是：**符合专业伦理（ethics），不一定符合每个人的个人道德（morals）**。三者**有重叠，但不相等**。

### ❓ 2.3 IT 的例子怎么举？（考试一定要有 IT 例子）

| | IT 例子 | 为什么属于这一类 |
|---|---|---|
| **Morals** | 帮客户修电脑的技术员，**自己觉得**偷看客户的私人照片不对，就算没人知道也不看 | 来自他**个人**的良心，没有人规定 |
| **Ethics** | 公司的 **code of conduct** 规定：IT staff 只能看工作需要的资料，不能用公司电脑做私人生意 | 这是**公司 / 专业**对成员的要求，违反会被处分 |
| **Laws** | **Computer Crimes Act 1997**：未经授权进入电脑是犯罪；**PDPA 2010**：不能未经同意把个人资料给别人；**Copyright Act 1987**：盗版违法 | 这是**政府**的法律，违反会被罚款、坐牢 |

**加分句（三者不完全一样）**：
- **Legal but unethical（合法但不道德）**：公司在合约里写明会监控员工的 email，法律允许；但如果连员工的私人讯息都偷看，很多人认为不道德。
- **Ethical motive but illegal（动机好但违法）**：一个 hacker 为了证明银行系统有漏洞，**没有得到授权**就去测试，动机是好的，但违反 Computer Crimes Act 1997。

> 💬 **答题句 (EN)** — *Morals are based on religion or personal belief and are individual and internal; ethics are based on a society's or profession's code of conduct and are collective and professional; laws are government rules that are enforceable by the police and courts. The three overlap but are not the same — for example, a lawyer may ethically defend a guilty client, though it may seem immoral to some.*

### ✅ 真题满分答案

**Oct 2024 Q1a / May 2025 Q1a** — *Discuss the differences among morals, ethics and law. Provide an example (related to IT) for each term.* **(9 marks)**

> - **Morals (3):** Morals are based on religion or personal belief about what is right and wrong. They are individual and internal, so they differ from person to person, and breaking them leads to guilt rather than punishment. *IT example:* a technician repairing a customer's laptop does not open the customer's private photos, because he personally believes it is wrong, even though no one would find out.
> - **Ethics (3):** Ethics are based on the code of conduct of a society or profession. They are collective and professional — shared by all members of the group — and breaking them can lead to disciplinary action by the employer or professional body. *IT example:* a company's IT code of conduct forbids staff from using company computers for personal business or accessing data they do not need for their work; the IT professional code of ethics also forbids software piracy.
> - **Law (3):** Laws are rules made by the government and are enforceable by the police and courts, with penalties such as fines or imprisonment. *IT example:* under Malaysia's Computer Crimes Act 1997, accessing a computer system without authorisation is a criminal offence.
>
> The three overlap but are not the same: for example, monitoring employees' emails may be legal but still considered unethical, and a lawyer may ethically defend a guilty client although it may seem immoral to some.

**Oct 2025 Q1a** — *Provide a scenario related to IT to explain the differences among morals, ethics, and law.* **(9 marks)**

（题目要 **scenario**，就用**一个故事**串三个概念。）

> *Scenario:* Daniel is a database administrator at an online shop. His manager asks him to secretly copy the customer database and give it to a partner marketing company.
>
> - **Morals (3):** Daniel personally feels that giving away customers' information without their knowledge is dishonest and betrays their trust. This judgement comes from his own personal beliefs and upbringing — it is individual and internal, and if he went ahead he would feel guilty.
> - **Ethics (3):** the shop's code of conduct and the IT professional's code of ethics require Daniel to protect confidential data and use it only for authorised purposes. This is a collective, professional standard; breaking it could lead to his dismissal or to action by his professional body.
> - **Law (3):** Malaysia's Personal Data Protection Act 2010 does not allow personal data to be disclosed to a third party without the customers' consent. This is a government rule enforceable by the authorities and courts, so Daniel and the company could face fines or imprisonment.
>
> In this scenario all three tell Daniel not to share the data, but they differ in where they come from (personal belief, professional code, government) and how they are enforced (guilt, disciplinary action, legal penalties).

**Jan 2025 Q1a** — *Distinguish between morals and ethics in the context of IT, with an example of each.* **(8 marks)**

> | | Morals | Ethics |
> |---|---|---|
> | Based on | Religion or personal belief | A society's or profession's code of conduct |
> | Nature | Individual and internal — differs from person to person | Collective and professional — the same for all members of the group |
> | Written? | Usually not written | Usually written (code of ethics, company IT policy) |
> | If broken | Personal guilt | Disciplinary action, dismissal, loss of certification |
>
> (4 marks — one for each row)
>
> - **Morals example (2):** a programmer refuses to develop an online gambling app because his personal and religious beliefs tell him gambling is wrong, even though his company and the law allow it.
> - **Ethics example (2):** a certified IT security professional must follow the professional code of ethics, so he must not reveal a client's security weaknesses to anyone else, even if he is offered money.

**May 2026 Q1b** — *Describe morals, ethics, and laws in computing.* **(6 marks)**

> - **Morals (2):** personal beliefs about right and wrong, based on religion or personal values; they are individual and internal. *Example:* a user personally decides not to download pirated games because she believes it is stealing.
> - **Ethics (2):** the code of conduct expected by a society or profession; they are collective and professional. *Example:* the IT professional code of ethics forbids software piracy and requires staff to keep customer data confidential.
> - **Laws (2):** government rules that are enforceable by the police and courts. *Example:* the Computer Crimes Act 1997 makes unauthorised access to a computer system a crime, and the Copyright Act 1987 makes software piracy illegal.

---

## 3. 5 Steps of Ethical Decision-Making 道德决策五步（讲师没写，但考过）

### ❓ 老板叫你把客户资料交给别人，你会怎么"一步一步"决定要不要做？

**大白话**
遇到道德难题，不要只凭感觉。用 5 个步骤：先**把问题写清楚** → **想几个方案** → **比较、选最好的** → **去做** → **检查结果**。如果结果不好，就回到第一步重新想。

```
 1. Develop problem statement      把问题写清楚
        ↓
 2. Identify alternatives          想出几个可能的方案
        ↓
 3. Evaluate & choose alternative  比较每个方案，选最好的
        ↓
 4. Implement decision             去执行
        ↓
 5. Evaluate results               检查结果 ──→ 不成功？回到 1
```

| Step | 中文 | 大白话 | 🌰 例子（老板要你交出客户资料） |
|---|---|---|---|
| 1 **Develop problem statement** | 写出问题 | 用一两句话写清楚：发生什么事、谁受影响 | "老板要我把客户资料给营销公司，但客户没有同意" |
| 2 **Identify alternatives** | 列出方案 | 想几个可以怎么做 | ① 照做 ② 直接拒绝 ③ 先问法律部门，取得客户同意才给 |
| 3 **Evaluate & choose alternative** | 比较与选择 | 看每个方案的风险、成本、合不合法、对谁有影响 | ① 违反 PDPA 2010 → 不行；选 ③ |
| 4 **Implement decision** | 执行 | 把选好的方案做出来 | 写 email 给老板和法律部门，建立取得客户同意的流程 |
| 5 **Evaluate results** | 检讨结果 | 看有没有解决问题；没有就回到第 1 步 | 一个月后检查：有没有客户投诉？做法有没有效？ |

> 💬 **答题句 (EN)** — *The five steps of ethical decision-making are: develop a problem statement, identify alternatives, evaluate and choose an alternative, implement the decision, and evaluate the results. If the result is not successful, the process starts again from the first step.*

### ✅ 真题满分答案

**Oct 2024 Q1b** — *Describe the FIVE major steps involved in ethical decision-making.* **(5 marks)**

> 1. **Develop a problem statement** – write a clear and short description of the problem, including the facts and who is affected. (1)
> 2. **Identify alternatives** – think of several possible solutions, involving the people affected where possible. (1)
> 3. **Evaluate and choose an alternative** – compare the options by their risks, costs, legality and effect on others, and choose the best one. (1)
> 4. **Implement the decision** – carry out the chosen solution in a timely and effective way. (1)
> 5. **Evaluate the results** – check whether the problem was solved and how it affected the people involved; if not successful, go back to step 1. (1)

---

## 4. Why Ethics in IT? 为什么 IT 特别需要伦理

### ❓ 为什么 IT 行业的人特别常碰到道德问题？

**大白话**
今天几乎所有东西都靠 IT：政府办事、公司运作、你的私人生活（银行、社交媒体、健康记录）。所以 IT 人员做的每个决定，都可能影响很多人。而且很多时候，**公司想要的**和**个人的权利**会冲突。

例如：公司想**监控员工的电脑**，防止员工偷资料或偷懒（公司的利益）；但员工也有**隐私权**（个人的权利）。IT 人员要在两者之间取得平衡——这就是 ethical decision。

📖 **讲师原句**
- "IT affects **governance, work, personal life** → ethical decisions matter."
- "In IT, ethical decisions often involve **balancing company interests with individual rights**."
- "Issues include: **Privacy** (employee monitoring, email usage) · **Data security** (protection against manipulation/leaks) · **Global concerns** (cross-border data, cybercrime)."

| Issue | 中文 | 大白话 | 讲师例子 |
|---|---|---|---|
| **Privacy** | 隐私 | 公司能看员工的东西到什么程度？ | **Employee monitoring**, email usage |
| **Data security** | 资料安全 | 资料有没有被改、被外泄？ | Protection against **manipulation / leaks** |
| **Global concerns** | 全球问题 | 资料跨国传送、网络犯罪跨国发生，要守哪一国的规矩？ | **Cross-border data, cybercrime** |

🌰 **讲师的例子**
- 公司**追踪员工的浏览记录**来防止滥用，但这会引起**隐私**的道德问题。
- 公司安装软件**记录员工的每一个键盘输入（keystrokes）**。

> 💬 **答题句 (EN)** — *Ethics matters in IT because IT affects governance, work and personal life, and ethical decisions in IT often involve balancing company interests with individual rights. Key issues include privacy (e.g. a company tracking employees' browsing history or keystrokes), data security (protecting data against manipulation or leaks) and global concerns (cross-border data and cybercrime).*

---

## 5. Ethics for IT Professionals IT 专业人员的伦理

### ❓ 医生、律师要守专业守则，IT 人员也要吗？

**大白话**
要。医生把病人的资料交给医院的 IT 人员管理，银行把几百万人的钱交给系统管理员。**别人把最重要的东西交给你，你就要对得起这份信任**。如果 IT 人员把病历泄露出去，大家就不敢再信任这家医院。

📖 **讲师原句**
- *Who is an IT professional?* "**Anyone working in IT** (e.g., developers, engineers, analysts)."
- *Characteristics of professionals:* "**Advanced knowledge & creativity** · **Independent judgment** · **Commitment to lifelong learning** · **Contribution to society**."
- *Professional Code of Ethics:* "Sets out **goals + rules** for members. Guides behaviour **beyond just following laws**."

**Professional 的 4 个特征**

| English | 中文 | 大白话 |
|---|---|---|
| **Advanced knowledge & creativity** | 高深的知识与创造力 | 懂一般人不懂的专业知识，能想出解决方法 |
| **Independent judgment** | 独立判断 | 能自己做专业决定，不是只听命令 |
| **Commitment to lifelong learning** | 终身学习 | 科技一直变，要一直学新东西 |
| **Contribution to society** | 对社会有贡献 | 工作是为了让社会更好 |

**Professional code of ethics（专业守则）**：一个专业团体写给会员的**目标和规则**，要求的标准**比法律更高**（法律只是最低要求）。

🌰 **讲师的例子**
- **IEEE** 和 **ACM** 都有专业守则，引导 IT 人员负责任地做事。
- **医生信任 IT 人员**会把病历保密；如果 IT staff 泄露病人资料，**public trust is lost**（公众的信任就没了）。

> 💬 **答题句 (EN)** — *An IT professional is anyone working in IT, such as developers, engineers and analysts. Professionals have advanced knowledge and creativity, independent judgment, a commitment to lifelong learning and a contribution to society. A professional code of ethics, such as those of IEEE and ACM, sets out goals and rules for members and guides behaviour beyond just following laws.*

---

## 6. Common Ethical Issues for IT Users ⭐（May 2026 考 9 分）

### ❓ 普通员工用公司电脑，最常犯哪三种道德问题？

**大白话**
1. **Software piracy（软件盗版）**：为了省钱，一个软件 license 装在很多台电脑，或下载破解版。
2. **Inappropriate use of IT（不当使用 IT）**：上班时间用公司电脑和网络做私事——看影片、玩游戏、分享不雅或冒犯人的内容，**浪费公司资源**。
3. **Inappropriate sharing of information（不当分享资讯）**：把公司的机密或顾客的资料**泄露**给不该知道的人，就算是**不小心**也算。

📖 **讲师原句**
1. "**Software Piracy** – **illegal copying of software**."
2. "**Inappropriate Use of IT** – **browsing, gaming, sharing offensive content at work, wasting company resources**."
3. "**Inappropriate Sharing of Information** – **leaking confidential business or customer data**."

🌰 **讲师的例子**
- 员工**不小心**把顾客的**信用卡号**分享出去 → **breach of privacy**（侵犯隐私）。
- 一个电商网站的**信用卡资料库外泄** → 顾客**可能再也不在那里网购**。

| Issue | 中文 | 为什么是问题（后果） | 🌰 例子 |
|---|---|---|---|
| **Software piracy** | 软件盗版 | 开发者失去收入；公司可能被告；盗版软件常带病毒 | IT 人员把一个 Office license 装在 30 台电脑 |
| **Inappropriate use of IT** | 不当使用 IT | 浪费网络和时间、降低生产力；冒犯人的内容会造成不友善的工作环境 | 上班时间用公司网络看电影 |
| **Inappropriate sharing of information** | 不当分享资讯 | 侵犯顾客隐私、违反 PDPA 2010；机密落入对手手中 | 员工不小心把顾客信用卡资料 email 给错的人 |

> 💬 **答题句 (EN)** — *Three common ethical issues for IT users are software piracy (illegal copying of software), inappropriate use of IT (browsing, gaming or sharing offensive content at work, wasting company resources), and inappropriate sharing of information (leaking confidential business or customer data).*

### ✅ 真题满分答案

**May 2026 Q1a** — *Explain THREE common ethical issues faced by IT users.* **(9 marks)**

> 1. **Software piracy (3)** – software piracy is the illegal copying of software, such as installing one licence on many computers or downloading cracked programs. It takes income away from software developers, exposes the company to lawsuits under the Copyright Act 1987, and pirated software often contains malware. *Example:* to save money, an IT officer installs one licensed copy of Microsoft Office on 30 office computers.
> 2. **Inappropriate use of IT (3)** – this means using company IT for browsing, gaming or sharing offensive content at work, which wastes company resources. It lowers productivity, uses up network bandwidth, and offensive content can create a hostile workplace. *Example:* an employee streams movies on the office network during working hours, slowing the network for colleagues.
> 3. **Inappropriate sharing of information (3)** – this means leaking confidential business or customer data, even by accident. It breaches customers' privacy and the PDPA 2010, and can make customers lose trust. *Example:* an employee accidentally emails a file of customers' credit card numbers to the wrong person; if an e-commerce site's card database is leaked, customers may never shop there again.

---

## 7. Certifications in IT IT 认证

### ❓ 你说你会管理网络，雇主凭什么相信？

**大白话**
看你的**证书（certification）**。证书是**技能的证明**，常常能帮你**找到工作、拿更高的薪水**。证书分两种：
- **Vendor certification（厂商认证）**：由**卖产品的公司**颁发，证明你会用**它家的产品**。例如 Cisco 证明你会用 Cisco 的网络设备，AWS 证明你会用 Amazon 的云端服务。
- **Industry association certification（行业协会认证）**：由**专业协会**颁发，内容**比较广**，不限某家公司的产品，而且要**考试和持续学习**才能保持。例如 (ISC)² 的资安证书、ISACA 的 IT 审计证书。

📖 **讲师原句**
- "**Proof of skills**, often **boosts career & salary**."
- "**Vendor Certifications**: **Specific to a company's products**. Examples: **Cisco, Microsoft, AWS**."
- "**Industry Association Certifications**: **Broader**, require **exams & ongoing learning**. Examples: **(ISC)², ISACA**."

| | **Vendor certification** 厂商认证 | **Industry association certification** 行业协会认证 |
|---|---|---|
| 范围 | **Specific to a company's products** | **Broader** |
| 要求 | 通过该公司的考试 | **Exams & ongoing learning**（要持续进修） |
| 例子 | **Cisco, Microsoft, AWS** | **(ISC)², ISACA** |

🌰 **讲师的例子**
- **AWS Cloud Certification** = 证明你能管理云端服务。
- 有 **Cisco 认证**的网络工程师可能**更快被录用**，因为证书让雇主放心你有基本能力。

> 💬 **答题句 (EN)** — *An IT certification is proof of skills that often boosts a person's career and salary. Vendor certifications are specific to a company's products, such as Cisco, Microsoft and AWS, while industry association certifications, such as those from (ISC)² and ISACA, are broader and require exams and ongoing learning.*

---

## 🔁 自测：我问，你先想，答案在下面

1. 讲师对 ethics 的定义？讲师把它比喻成什么？
2. Computer ethics 的定义？
3. Morals、Ethics、Laws 各自 **based on** 什么？**nature** 是什么？
4. 讲师用哪个职业举例"符合 ethics 但有人觉得不符合 morals"？
5. 写 "morals / ethics / law + IT example" 时，每个 term 要写哪 3 样？
6. 道德决策 5 步按顺序说出来。
7. IT 里的 ethical decision 通常在平衡哪两样东西？讲师举的 3 个 issues？
8. Professional 的 4 个特征？
9. IT users 的三个 ethical issues？
10. AWS 证书是 vendor 还是 industry association certification？ISACA 呢？

.

.

.

.

**答案**
1. **A set of values that define acceptable behaviour in society**；**"invisible guidebook"**（法律管不到的时候用）
2. **Guidelines for morally acceptable use of computers**
3. Morals = **religion or personal belief**，**individual & internal** · Ethics = **society/profession's code of conduct**，**collective & professional** · Laws = **government rules**，**enforceable by police/courts**
4. **Lawyer** defending a guilty client
5. **Based on（定义）+ Nature（特点）+ IT 例子**，最后加一句三者重叠但不相等
6. **Develop problem statement → Identify alternatives → Evaluate & choose → Implement decision → Evaluate results**
7. **Company interests** vs **individual rights**；**privacy, data security, global concerns**
8. **Advanced knowledge & creativity · Independent judgment · Lifelong learning · Contribution to society**
9. **Software piracy · Inappropriate use of IT · Inappropriate sharing of information**
10. AWS = **Vendor**；ISACA = **Industry association**

---

## 🧾 一页背诵（考前 5 分钟看）

- **Ethics** = set of values defining acceptable behaviour in society · **invisible guidebook** when laws don't cover everything · **computer ethics** = morally acceptable use of computers · universally wrong: lying, stealing · 文化不同（piracy）· ChatGPT 功课（dishonesty vs smart tool）
- **Morals** = religion / personal belief · individual & internal · abortion is wrong
- **Ethics** = society / profession's code of conduct · collective & professional · IT code against piracy
- **Laws** = government rules · enforceable by police / courts · abortion legalised · Computer Crimes Act 1997 · PDPA 2010 · Copyright Act 1987
- **Lawyer** defends a guilty client = ethical but may seem immoral → 三者重叠但不相等
- **5 steps**：Problem statement → Alternatives → Evaluate & choose → Implement → Evaluate results（→ 回 1）
- **Why ethics in IT**：governance, work, personal life · balance **company interests vs individual rights** · privacy（browsing, keystrokes）· data security · global concerns
- **IT professional**：knowledge & creativity · independent judgment · lifelong learning · contribution to society · **code of ethics**（IEEE, ACM）goes beyond laws · leak patient records → public trust lost
- **3 issues**：**Software piracy · Inappropriate use of IT · Inappropriate sharing of information**
- **Certifications** = proof of skills, boosts career：**Vendor**（Cisco, Microsoft, AWS）vs **Industry association**（(ISC)², ISACA; broader, exams & ongoing learning）

➡️ 下一章：**Ch2 Computer and Internet Crime**（Q1 后半）
