# AMIS1012 — Chapter 7: Software Development

> 只有 27 页，却是**每一份 past year 的 Q3（25 分）**。Slide 只给定义，没有：① 怎样根据情境**选** Agile 或 Waterfall 并 justify（May 2025、May 2026 都考）；② 为什么"越晚发现 defect 越贵"的**推理链**（四份卷都考，3–9 分）；③ black-box / white-box 的**具体测试场景**（四份卷都考，8 分）；④ PDCA 的**图 + 在软件 QA 中的应用**（10 分）。另外 slide 把 "Scalable" 定义为"能在不同 OS、电脑、软件上运作"，这其实是 **portability**。这份笔记用一个银行 app 贯穿全章，补上每一题的满分答法。

---

## 0. 一句话总览

**选对 software development methodology（Agile 或 Waterfall），并在开发早期就找出 defect（越晚越贵，最多贵 100 倍，还可能被告）；用 manual / automation / dynamic testing（black-box 看输入输出、white-box 看内部逻辑）来验证软件符合规格；再用 quality assurance 的 PDCA cycle 持续改进流程，做出 scalable、usable、reusable、correct、maintainable 的高品质软件。**

| Part | 内容 | Slide |
|---|---|---|
| 1 | Software development methodology：Agile vs Waterfall；defect 越早修越便宜；liability | p3–9 |
| 2 | Software testing：manual、automation、dynamic（black-box、white-box） | p10–15 |
| 3 | Quality assurance & standards：quality、assurance、**PDCA (Deming) cycle** | p16–20 |
| 4 | Software quality & assurance techniques：**5 quality characteristics**、quality system | p21–25 |

🎯 **这章回答的考题（Q3 每年必考）**

| 题 | Oct 2024 | Jan 2025 | May 2025 (Ethan / ABC Bank) | Oct 2025 | May 2026 (banking app) |
|---|---|---|---|---|---|
| Choose methodology + justify | — | — | 3a (5) | — | 3a (4) |
| **Defect early vs late** | 3a agree? (5) | 3d impact (3) | — | 3a agree? (5) | 3b THREE effects (9) |
| Testing goals | — | 3a TWO (4) | — | — | — |
| Manual vs automation | — | — | — | — | 3c (6) |
| **Black-box & white-box** | 3b (8) | 3b differences + scenario (8) | 3b (4+4) | 3b (4+4) | — |
| **PDCA** | 3c with diagram (10) | 3c purpose + stages (10) | — | — | — |
| **Quality characteristics** | 3d ONE (2) | — | 3c FOUR (12) | 3c FOUR for payment app (12) | 3d THREE (6) |

（Tutorial 7 的 10 题也全在这章。）

**Prerequisites**：Ch6 的 SDLC policy（每个阶段都要考虑安全）、CIA（banking app 要 secure）。

---

## Scene：Ethan 的 Queue Management System

Ethan 是软件团队主管。**ABC Bank** 要他们做一个**实时排队管理系统 (Queue Management System)**，包括手机 app。团队擅长开发和测试；**需求已经定义清楚**；必须在**一年内**完成。

他要回答四个问题：
1. 用什么开发方法？
2. 如果 bug 到上线后才发现，会怎样？
3. 怎么测试？
4. 怎样确保"品质"——什么叫高品质软件？

---

## 1. Software Development Methodology（p3–9）

- **Software development methodology** = 一套**经过验证 (tried-and-true)** 的工作程序，帮助系统分析师、设计师、项目经理等以**有管理、有秩序**的方式做出高品质软件。
- 它描述开发阶段的**任务**，以及个人与团队完成任务的**责任**；也有执行各功能的具体技术（如用 **flowchart** 记录程式逻辑）。

**两种方法（p4–6 的图）**：

```
          AGILE                                    WATERFALL
                                          ┌──────────┐
       Deploy ──> Test                    │ Analysis │─┐
         ^          │                     └──────────┘ v
         │          v                          ┌────────┐
       Review <── Develop                      │ Design │─┐
         (repeating cycle)                     └────────┘ v
                                               ┌────────────────┐
  Plan ─> Design ─────────────> Launch         │ Implementation │─┐
  (each short iteration goes round the         └────────────────┘ v
   cycle, then the next increment is                  ┌─────────┐
   planned)                                           │ Testing │─┐
                                                      └─────────┘ v
                                                        ┌─────────────┐
                                                        │ Maintenance │
                                                        └─────────────┘
```
*看图重点：Agile 是**圆圈**（不断重复、每圈交付一部分）；Waterfall 是**阶梯**（一层做完才下一层，很难往回走）。*

| | **Agile** | **Waterfall** |
|---|---|---|
| 形式 | **Flexible, iterative** | **Traditional, linear** |
| 流程 | 把项目拆成**小而可管理的部分**，一轮一轮做 | **严格顺序**：requirements → design → implementation → testing → deployment → maintenance |
| 变更 | 强调 **continuous delivery、customer feedback、adapting to change**；可根据客户回馈**调整方向** | 每阶段**完成后才进入下一阶段**，**很少回头** |
| 交付 | **快速有效地交付可用的产品增量** | 最后一次交付完整产品 |
| 适合 | 需求**不清楚或会改变**、需要客户频繁回馈 | 需求**明确 (well-defined)**、变更**不太可能或代价高** |

> 💬 **答题句 (EN)** — *Waterfall is a traditional, linear methodology in which each phase (requirements, design, implementation, testing, deployment, maintenance) must be completed before the next begins; it suits projects with well-defined, stable requirements. Agile is a flexible, iterative methodology that breaks the project into small increments delivered frequently with continuous customer feedback; it suits projects whose requirements are likely to change.*

### ✅ 满分答法 — May 2025 Q3a: Identify the suitable system development model for Ethan and justify. (5 marks)
**Waterfall** (1) is the most suitable model.
- The **requirements are well defined** by ABC Bank, and Waterfall suits projects with clear, stable requirements where changes are unlikely. (1)
- It follows a **strict sequence** of phases — requirements, design, implementation, testing, deployment and maintenance — so the team can plan the whole year with clear milestones and deliverables to meet the **one-year deadline**. (1)
- Each phase is completed and documented before the next, which gives the bank thorough documentation and sign-off for a real-time queue system. (1)
- The team is **experienced** in development and testing, so it can execute each phase correctly without needing frequent iterations; a dedicated testing phase fits their testing expertise. (1)

### ✅ 满分答法 — May 2026 Q3a: Recommend a suitable software development methodology for the online banking application. Justify. (4 marks)
**Agile** (1) is recommended.
- The company wants to **improve its development and testing process**; Agile breaks the project into small increments, and **each iteration is developed, tested and reviewed**, so defects are found early rather than at the end. (1)
- **Continuous customer feedback** lets the bank's stakeholders review each increment (e.g. login, transfer, bill payment) and adjust requirements, reducing the risk of delivering the wrong features. (1)
- Frequent, working increments allow security and reliability to be verified repeatedly before the final release, supporting the requirement that the system be secure, reliable and error-free. (1)
*(Alternative accepted with justification: Waterfall, if you argue that banking requirements and regulations are fixed and each phase needs formal documentation and approval.)*

### 1.1 越早修越便宜（p7–9）⭐

- **Slide 原句**：在开发**早期（requirements definition）**找出并消除 defect，成本**最多比**修复已经**交付给消费者**的软件中的 defect **少 100 倍**。
- **为什么？**（p8）如果 flaw 在后期才发现，**之前做好的所有 deliverables 都要重做 (rework)** → 沟通与修正错误的成本更高。
- 所以工程师努力**及早**找出错误，不只为了省钱，也为了**提高软件品质**。
- **法律风险**（p9）：若软件有潜在缺陷导致消费者受损，可能被提出 **product warranty lawsuit**；没有仔细、可靠地设计应用程式会导致**重大的法律责任 (liability exposure)**。

**推理链**（写答案用）：

```
 defect in REQUIREMENTS ──> fix one sentence in the spec                 cost ×1
 found in DESIGN        ──> fix spec + design                            cost ↑
 found in CODING        ──> fix spec + design + code                     cost ↑↑
 found in TESTING       ──> fix spec + design + code + retest            cost ↑↑↑
 found AFTER RELEASE    ──> all of the above + patch every user's copy,  up to ×100
                            customer support, data repair, lawsuits,
                            lost reputation
```
*看图重点：每晚一个阶段，就要多重做前面所有已完成的东西——这就是 rework。*

### ✅ 满分答法 — Oct 2024 Q3a / Oct 2025 Q3a: "Identifying and eliminating a defect early … will cost (up to 100 times) less than removing a defect in software already shipped to consumers." Do you agree? Justify. (5 marks)
**Yes, I agree.** (1)
1. **Rework of earlier deliverables** – a defect found late means that all work built on the faulty requirement or design (design documents, code, test cases) must be redone; a defect caught at the requirements stage is fixed by changing a document. (1)
2. **Higher cost of communication and correction** – after release, the fix must be developed, re-tested and distributed as a patch to every customer, with support staff handling complaints. (1)
3. **Damage to customers and legal liability** – shipped defects may harm users (e.g. financial loss), exposing the company to product warranty lawsuits and compensation claims. (1)
4. **Loss of reputation and quality** – customers lose trust and may switch to competitors; finding defects early therefore saves money and increases software quality. (1)

### ✅ 满分答法 — Jan 2025 Q3d: Identify the impact of discovering a flaw later in the production process. (3 marks)
Any rework on the deliverables created earlier in the process is required (1); therefore the cost of communicating and correcting the mistake is much higher — up to 100 times more than fixing it at the requirements stage (1); and if the flaw reaches customers and causes damage, the company faces product warranty lawsuits and liability, as well as reputational loss (1).

### ✅ 满分答法 — May 2026 Q3b / Tutorial 7 Q2: Analyse THREE effects of failing to detect software defects early in the banking application. (9 marks)
3 分一个（effect + 原因 + 银行 app 例子）：
1. **Higher long-term cost** – a defect discovered after release requires rework of requirements, design, code and tests, plus emergency patches and customer support, costing up to 100 times more than fixing it early. *e.g.* a calculation error in interest found after launch requires changes across several modules and recalculation of thousands of accounts.
2. **Lower customer satisfaction and financial harm to users** – defects such as failed transfers, wrong balances or app crashes directly affect customers' money and access to services, causing frustration and loss of confidence. *e.g.* customers are charged twice for a bill payment and must wait days for refunds.
3. **Damage to company reputation and legal liability** – a security flaw could expose customers' data or allow fraud, attracting media attention, regulatory penalties and product warranty lawsuits. *e.g.* a login vulnerability leads to account takeovers, and the bank and developer are sued and fined.

---

## 2. Software Testing（p10–15）

- **Software testing** = 评估软件**功能**的方法，目的是判断软件**是否符合规格要求 (required specifications)** 并**找出 bug**，以交付更好的产品。

### ✅ 满分答法 — Jan 2025 Q3a / Tutorial 7 Q3: Determine TWO goals of software testing to ensure high-quality software. (4 marks)
1. **To verify that the software meets its required specifications** – testing checks that every function listed in the requirements works as expected, so the delivered product does what the customer asked for. (2)
2. **To identify bugs (defects) before release** – testing finds errors so they can be fixed early, reducing cost and delivering a better, more reliable product to users. (2)

### 2.1 Manual vs Automation testing

| | **Manual testing** | **Automation testing** |
|---|---|---|
| 定义 | **人手**评估软件，了解它、找出哪里正常哪里不正常 | 用**自动化工具**找 bug |
| 做法 | 验证**规格文件**列出的所有功能；常以**终端用户的角度**使用软件 | 测试人员执行 **test scripts**，**自动**产生测试结果 |
| 优点 | 能发现使用体验、介面问题；适合探索性、一次性测试 | 快、可重复、适合大量回归测试 (regression)、可随时跑 |
| 缺点 | 慢、易出错、重复时费人力 | 需先写 script、工具成本；不擅长判断"好不好用" |

### ✅ 满分答法 — May 2026 Q3c / Tutorial 7 Q4: Explain how the company can use manual testing and automation testing to test the banking system. (6 marks)
- **Manual testing (3):** testers manually evaluate the application from the end customer's perspective, verifying every function in the specification. *For the bank:* testers log in on different phones, perform transfers, pay bills and check statements to judge whether the screens are clear, error messages are understandable, and the flow is easy for elderly customers — issues an automated tool cannot judge.
- **Automation testing (3):** an automated tool runs test scripts and produces results automatically. *For the bank:* scripts repeatedly test thousands of transfer amounts, login attempts and balance calculations after every code change (regression testing), and simulate many simultaneous users, giving fast and consistent results before each release.

### 2.2 Dynamic testing：Black-box vs White-box（p13–15）⭐

- **Dynamic testing** = 对**完成的软件单元**，**直接输入测试资料**，把**结果与预期结果比对**。分为 black-box 和 white-box。
- *Extra*：相对的 **static testing** 是**不执行**程式，只检查 code 与文件（code review、walkthrough）。

```
   BLACK-BOX                                WHITE-BOX
   Input ──> ┌──────────┐ ──> Output        Input ──> ┌─────────────────┐ ──> Output
             │██████████│                             │ if ─> ● ─> ●    │
             │██ ??? ███│                             │  └──> ● ─> ●    │
             │██████████│                             │ (logic visible) │
             └──────────┘                             └─────────────────┘
   inside unknown: check only                 inside known: test every logic path;
   that outputs match the spec                every statement executed at least once
```

| | **Black-box testing** | **White-box testing** |
|---|---|---|
| 视角 | 软件单元是有**预期输入输出行为**、但**内部运作未知**的系统 | 软件是有**计划的输入输出行为**且**内部运作已知**的系统 |
| 通过条件 | 对测试集中**任何输入**都表现出**预期行为** | 理解程式逻辑，测试**所有可行的逻辑路径**；**每一行语句至少执行一次** |
| 需要看 code 吗 | **不需要**（不知道 code 的组成或设计） | **需要**彻底理解 code |
| 谁做 | 通常**不是写 code 的人**（独立测试员、用户） | 通常是**开发者**或懂 code 的测试员 |
| 测试资料设计 | 根据**规格/需求** | 根据**程式结构**，需**仔细组合** |
| 找到什么 | 缺少功能、介面错误、输出错误 | 逻辑错误、无法到达的 code、错误的分支/回圈 |

> 💬 **答题句 (EN)** — *Black-box testing treats the software unit as a system with expected inputs and outputs but unknown internal workings; the tester, usually not the code's author, checks that it produces the expected behaviour for all test inputs. White-box testing treats the unit as having known internal workings; the tester understands the program logic and designs test data so that every feasible logic path and every statement is executed at least once.*

### ✅ 满分答法 — May 2025 Q3b: Explain black-box and white-box testing and provide a scenario on how Ethan's team can carry out each (Queue Management System). (4 + 4 marks)
- **(i) Black-box testing** – the tester treats the system as a "black box" with known inputs and expected outputs but does not look at the code; the unit passes if it gives the expected behaviour for all test data, and it is usually done by someone other than the code's author. (2)
  *Scenario:* an independent tester uses the mobile app to take a queue number for "Cash Deposit" at a branch. They check that the app shows the next number in sequence, the estimated waiting time, and that the branch display screen updates in real time when the counter calls the number — without knowing how the code works. (2)
- **(ii) White-box testing** – the tester knows the internal logic and designs test data to execute every feasible logic path and every statement at least once. (2)
  *Scenario:* a developer tests the function that assigns tickets to counters. They create test cases for every branch: when a counter is free, when all counters are busy, when a VIP/priority customer arrives, and when the queue is empty — ensuring each `if/else` path in the allocation code is executed and gives the correct result. (2)

### ✅ 满分答法 — Oct 2025 Q3b: Same question for "a software development team" (4 + 4 marks)
Same definitions; any consistent system as the scenario, e.g. an online shopping system: black-box — a tester enters valid and invalid card numbers at checkout and checks that valid ones are accepted and invalid ones rejected with an error message; white-box — a developer tests the discount-calculation function so that every path (no discount, member discount, voucher, both, expired voucher) is executed at least once.

### ✅ 满分答法 — Jan 2025 Q3b: Examine the differences between black-box and white-box testing and provide a scenario where each should be applied. (8 marks)
Differences (4) — use the table above, four rows: knowledge of internal code; who performs it; basis of test data (specification vs program logic); goal (expected behaviour vs every path/statement).
Scenarios (2 + 2):
- **Black-box should be applied** in **user acceptance/system testing** of an online banking app, where testers who did not write the code check that transfers, bill payments and balance displays behave according to the requirements.
- **White-box should be applied** in **unit testing** of a critical module such as the interest-calculation or password-validation function, where developers must be sure that every branch and statement works correctly.

### ✅ 满分答法 — Oct 2024 Q3b: Examine how black-box and white-box testing can be used to eliminate defects, with examples. (8 marks)
- **Black-box (4):** by feeding in test data based on the specification and comparing actual outputs with expected outputs, testers detect missing or incorrect functions and interface errors. Because the tester is not the author, they do not share the developer's assumptions, so they catch defects the developer overlooked. *Example:* entering a birth date of 31/02/2000 in a registration form reveals that the system accepts an invalid date.
- **White-box (4):** by understanding the code and executing every logic path and statement at least once, testers find logic errors, wrong conditions, unreachable code and faulty loops that may not show up in normal use. *Example:* testing a loan-eligibility function with income exactly at the RM3,000 boundary reveals that the code uses `>` instead of `>=`, wrongly rejecting eligible applicants.

### Tutorial 7 Q5–7
- **Q5 difference** → 上表。
- **Q6 black-box scenario** → 例：测试员在电商 app 结帐页输入有效/无效的信用卡号，检查是否正确接受或拒绝。
- **Q7 white-box scenario** → 例：开发者测试 "计算运费" 函式，设计资料使每个分支（本地/海外、超重/不超重）都执行至少一次。

---

## 3. Quality Assurance & Standards（p16–20）

- **Quality assurance (QA)** = 在**生产期间**使用的方法，确保产品**一致的服务**。
- **Quality** 很难定义，可概括为 **"fit for use or purpose"**：满足消费者在**功能、风格、效率、耐用性、价格**方面的期望。
- **Assurance** = 对产品或服务的**正面声明**，**激发信心**：保证它能表现良好、**按标准或规格**运作。
- **Software testing QA** = 确保企业提供给客户的软件产品或服务的品质的方法。
- QA 让软件开发流程**更可靠、更有竞争力**，同时遵守软件产品的品质标准。

### ✅ 满分答法 — Tutorial 7 Q8: Define quality in software and explain its significance.
Quality in software means the product is **"fit for use or purpose"** — it satisfies customers' expectations in functionality, efficiency, durability, style and price, and fulfils the requirements in the Software Requirements Specification (SRS). It is significant because high-quality software reduces defects and failures, lowers maintenance and support costs, protects users from harm (and the company from liability), and builds customer satisfaction and the company's reputation.

### 3.1 PDCA (Deming) Cycle（p18–19）⭐

- 在 QA 中，**PDCA cycle**（又叫 **Deming cycle**）是一个固定循环：**Plan, Do, Check, Act**。
- 这些步骤**定期重复**，确保组织的运作**持续被评估与改进**。
- 组织用 QA 来**验证产品被正确开发与交付**，减少最终产品的问题与缺陷（p20）。

**Slide 的图（p19，照版面）**：

```
                    ┌────────┐
            ┌──────>│  PLAN  │───────┐
            │       └────────┘       │
            │                        v
       ┌────────┐               ┌────────┐
       │  ACT   │               │   DO   │
       └────────┘               └────────┘
            ^                        │
            │       ┌────────┐       │
            └───────│ CHECK  │<──────┘
                    └────────┘
               Quality assurance process
         (clockwise, repeated continuously)
```
*看图重点：Act 之后回到 Plan——这是**持续改进**的循环，不是做一次就结束。*

| Stage | Slide 的说法 | 软件 QA 的应用（银行 app 例子） |
|---|---|---|
| **Plan** | 建立**流程相关的目标**，以及交付高品质结果所需的**流程** | 设定品质目标（例：上线时 critical defects = 0，app crash rate < 0.1%）；规划 coding standards、测试策略、code review 流程 |
| **Do** | **实施计划**；开发与测试流程，"do" 系统变更 | 按计划开发功能、执行 code review、跑 black-box/white-box 与自动化测试 |
| **Check** | **监控与检查**流程是否达到目标 | 收集并分析缺陷数据、测试覆盖率、用户回馈，与目标比较（例：发现 transfer 模组缺陷最多） |
| **Act** | QA tester 采取**必要措施改进流程** | 把有效做法标准化；针对问题改流程（例：transfer 模组加强 unit testing、加 static code analysis），然后进入下一轮 Plan |

> 💬 **答题句 (EN)** — *The PDCA (Deming) cycle is a continuous quality-assurance cycle: Plan sets process objectives and the processes needed to deliver a high-quality result; Do implements the plan by developing and testing; Check monitors the processes and results against the objectives; Act takes actions to improve the processes. The cycle repeats so that operations are continually evaluated and improved.*

### ✅ 满分答法 — Oct 2024 Q3c: With the aid of a diagram, discuss how an organisation applies the PDCA cycle to verify the quality of software goods or services. Explain each process. (10 marks)
**Diagram (2):** draw the four boxes Plan → Do → Check → Act in a clockwise loop with arrows back to Plan, labelled "Quality assurance process".
**Explanation (2 each):**
- **Plan** – the organisation sets quality objectives for the software (e.g. performance targets, maximum defect rates) and designs the processes needed to achieve them, such as coding standards, test plans and review procedures.
- **Do** – the plan is implemented: developers build the software following the defined processes and testers carry out the planned tests and system changes.
- **Check** – the results are monitored and measured against the objectives: test results, defect reports, customer feedback and process metrics are analysed to see whether the quality goals were met.
- **Act** – based on the findings, QA takes corrective and improvement actions — fixing defects, updating standards and improving the process — and successful practices are standardised. The cycle then repeats, providing continual assessment and improvement.

### ✅ 满分答法 — Jan 2025 Q3c: Explain the purpose of the PDCA cycle in quality assurance and describe its stages. (10 marks)
**Purpose (2):** PDCA is a fixed, repeating cycle used in QA to make the software development process more dependable and competitive while meeting quality standards; by repeating it regularly, the organisation continually evaluates and improves its operations and verifies that the product is developed and delivered properly, reducing problems and faults in the final product.
**Stages (2 each):** Plan, Do, Check, Act — as above.

### Tutorial 7 Q9: How does PDCA provide continual assessment and improvement?
Because the cycle never ends: every **Check** stage assesses the results of the previous **Do** against the **Plan**'s objectives, and every **Act** stage feeds improvements into the next **Plan**. Each loop starts from a better process, so quality improves continuously rather than being checked only once at the end.

---

## 4. Software Quality & Assurance Techniques（p21–25）

- **高品质产品** = **满足消费者期望**；软件是否适用，通常以是否满足 **Software Requirements Specification (SRS)** 的要求来描述。

**五个品质特性（p22–23）**：

| Characteristic | Slide 定义 | 银行 app 例子 |
|---|---|---|
| **Scalable** | 能在**多种 OS、不同电脑或与其他软件设备**上运作 | App 在 Android 与 iOS、手机与平板上都能用；与银行核心系统整合 |
| **Usability** | **所有类型的用户**都能方便地使用系统功能 | 大字体、清楚的转账步骤，长者也会用 |
| **Reusability** | 系统的各个元件能**容易地被复制来做新应用** | 登入/OTP 模组可重用于银行的投资 app |
| **Correctness** | **SRS** 中的各项规格被**正确地实现** | 利息、转账金额、手续费计算都完全符合规格 |
| **Maintainability** | 故障**容易修复**、新功能**容易加入**、功能**容易修改** | 模组化设计，新增 DuitNow QR 功能不影响其他部分 |

⚠️ Slide 对 "Scalable" 的定义（在不同 OS/电脑/软件上运作）其实是 **portability（可移植性）**；一般所说的 **scalability** 是**用户量或资料量增加时仍能良好运作**。考试写 "Scalable" 时，**先照 slide 定义写**，可再补一句"and can handle growth in users/data"，两者都拿分。

> 💬 **答题句 (EN)** — *High-quality software fulfils customer expectations and the requirements in the SRS, and exhibits characteristics such as scalability (operates on various operating systems, computers and devices), usability (all types of users can conveniently use its features), reusability (components can be reused to build new applications), correctness (the SRS specifications are implemented properly) and maintainability (defects can be fixed and functions added or changed easily).*

### ✅ 满分答法 — May 2025 Q3c: Describe FOUR characteristics of a high-quality software product that Ethan's team's system should exhibit. (12 marks)
3 分一个（定义 + 为什么对 Queue Management System 重要 + 例子）：
1. **Scalability** – the software can operate on a range of operating systems, computers and devices. The queue system runs on customers' Android and iOS phones, branch kiosks and counter terminals, and should keep performing as more branches and users are added.
2. **Usability** – all types of users can conveniently use the system's features. Bank customers of all ages must easily take a number, see their position and receive a notification; staff should call the next customer with one click.
3. **Correctness** – the specifications in the SRS are implemented properly. Queue numbers must be issued in the right order, waiting-time estimates calculated as specified, and priority customers handled according to the bank's rules.
4. **Maintainability** – defects can be fixed easily and new functions added or changed. ABC Bank may later add appointment booking or new service types; a modular, well-documented design allows this without breaking existing features.
（Reusability 也可：ticketing and notification modules reused for other bank systems.）

### ✅ 满分答法 — Oct 2025 Q3c: As a software designer planning a mobile app for secure online payments, explain how you would apply FOUR characteristics of high-quality software in your design to meet user needs. (12 marks)
3 分一个（characteristic + 设计上怎么做 + 满足什么用户需求）：
1. **Correctness** – I would derive detailed test cases from the SRS for every payment function (amount entry, currency conversion, fees, receipts) and use both black-box and white-box testing, so that every payment is processed exactly as specified — users need accurate payments.
2. **Usability** – I would design a simple, consistent interface with clear confirmation screens, readable fonts, accessibility options and meaningful error messages, so that users of all ages can pay quickly without mistakes.
3. **Scalability** – I would build the app to run on Android and iOS and different screen sizes, and use cloud-based back-end services that scale with transaction volume, so the app works on users' devices and stays fast during peak periods.
4. **Maintainability** – I would use a modular architecture, coding standards and documentation, so security patches and new payment methods (e.g. new e-wallets) can be added quickly without breaking existing features — users need a secure, up-to-date app.

### ✅ 满分答法 — May 2026 Q3d: Explain how THREE software quality attributes help improve the quality of the banking application. (6 marks)
2 分一个：
1. **Correctness** – ensures that all SRS requirements, such as balance calculation and fund transfers, are implemented properly, so customers' money is always handled accurately.
2. **Usability** – makes the app easy for all types of customers to use, reducing user errors such as transferring to the wrong account and increasing satisfaction.
3. **Maintainability** – allows defects and security vulnerabilities to be fixed quickly and new banking features to be added easily, keeping the app secure and reliable over time.

### ✅ 满分答法 — Oct 2024 Q3d: Describe a characteristic of good software. (2 marks)
**Maintainability** (1) – defects can be easily fixed when they arise, and new functions can be easily added to or changed in the product (1).

### Tutorial 7 Q10: FIVE characteristics → scalable, usability, reusability, correctness, maintainability（上表）。

### 4.1 Quality system（p24–25）

- **Quality control scheme** 是企业确保产品品质最高的常见方式。
- **组成**：个人责任 + 管理架构：
  - **整个组织**都对 quality scheme 负责；可有多个 quality 部门做不同活动；
  - quality scheme 必须得到**高层管理的批准/支持**——否则员工会消极看待。
- **Quality management system 的活动**：**project auditing**、**quality system review**、**制定原则、程序与协议**、为高层编写**总结品质系统成效的报告**。

### Tutorial 7 Q1: What is a software development methodology? Describe TWO with diagrams.
Definition → §1；diagrams → Agile 圆圈与 Waterfall 阶梯（§1 的图）+ 表格。

---

## Closing the loop

Ethan 选了 **Waterfall**（需求清楚、一年期限）；他知道 defect **越早发现越便宜**（最多 100 倍，还避免诉讼）；他让独立测试员做 **black-box**、让开发者做 **white-box**，并用 **automation** 跑回归测试；他用 **PDCA** 持续改进流程，目标是一个 scalable、usable、correct、maintainable 的系统。

软件做好了，**谁拥有它**？它的 code、名字、logo、演算法怎样防止被抄？下一章 **Intellectual Property**。

---

## ⚠️ Where the slides mislead

| Slide | Slide 说 | 更准确的理解 |
|---|---|---|
| p22 | **Scalable** = able to operate on a range of operating systems, computers, devices | 这是 **portability**；scalability 通常指随用户/资料增长仍能运作。考试照 slide 写，可补一句 |
| p22–23 | 标题 "quality **methods**" | 实际列的是 quality **characteristics / attributes** |
| p15 | White-box: "considers the software **machine**…" "White-box **research**" | 用词奇怪，意思是 "software unit" 与 "white-box testing" |
| p19 | Plan / Do / Check / Act 描述很短 | 本笔记补上软件 QA 的具体应用 |
| p7 | "up to 100 times" | 这是常被引用的经验数字（不同研究结果不一），重点是"越晚越贵"的趋势 |

---

## Term table

| English | 中文 | 一句话说明 |
|---|---|---|
| Software development methodology | 软件开发方法论 | 有秩序地开发高品质软件的程序 |
| Agile | 敏捷开发 | 迭代、弹性、持续回馈 |
| Waterfall | 瀑布模型 | 线性、顺序、需求明确 |
| Iteration / increment | 迭代 / 增量 | 一轮开发 / 一部分可用产品 |
| Defect / flaw / bug | 缺陷 | 软件中的错误 |
| Rework | 返工 | 重做之前的成果 |
| Product warranty lawsuit | 产品保证诉讼 | 因缺陷造成损害而被告 |
| Liability exposure | 法律责任风险 | 可能要赔偿 |
| Software testing | 软件测试 | 检查是否符合规格并找 bug |
| Manual / automation testing | 手动 / 自动化测试 | 人手 vs 工具与 script |
| Test script | 测试脚本 | 自动化测试的程式 |
| Regression testing | 回归测试 | 修改后重测旧功能 |
| Dynamic / static testing | 动态 / 静态测试 | 执行程式 vs 不执行（review） |
| Black-box / white-box testing | 黑箱 / 白箱测试 | 看输入输出 / 看内部逻辑 |
| Test suite | 测试集 | 一组测试资料 |
| Logic path | 逻辑路径 | 程式可走的分支 |
| Quality assurance (QA) | 品质保证 | 生产期间确保品质的方法 |
| Fit for use or purpose | 合乎用途 | 品质的定义 |
| PDCA / Deming cycle | 计划-执行-检查-行动循环 | 持续改进 |
| SRS | 软件需求规格书 | 软件要求文件 |
| Scalability / usability / reusability / correctness / maintainability | 可扩展性 / 易用性 / 可重用性 / 正确性 / 可维护性 | 品质特性 |
| Portability | 可移植性 | 在不同平台运作 |
| Quality control scheme | 品质控制制度 | 全组织、高层支持 |

---

## Cheat sheet

- **Methodology** = tried-and-true procedure；**Agile**（iterative, feedback, change）vs **Waterfall**（linear, well-defined requirements）。
- 选择：需求**清楚 + 固定期限** → Waterfall（Ethan）；需求会变 / 要早测试 / 要客户回馈 → Agile。
- **Defect early**：最多便宜 **100 倍**；因为晚了要 **rework** 之前所有 deliverables，沟通修正成本高；还有 **product warranty lawsuit / liability**；reputation。
- **Testing 目的**：符合规格 + 找 bug。
- **Manual**（人、从用户角度、验证规格）vs **Automation**（工具、test scripts、自动结果）。
- **Dynamic testing** = 输入测试资料比对预期结果 → **black-box**（内部未知、非作者、预期行为）vs **white-box**（内部已知、所有逻辑路径、每行至少执行一次）。
- **QA** = 生产期间确保一致服务；quality = **fit for use or purpose**。
- **PDCA (Deming)**：Plan（目标与流程）→ Do（实施、开发测试）→ Check（监控是否达标）→ Act（改进）→ 重复。
- **5 quality characteristics**：scalable, usability, reusability, correctness, maintainability（满足 **SRS**）。
- **Quality system**：全组织负责 + 高层批准；活动 = project auditing, system review, 制定程序, 给高层报告。

---

## Practice (answers included)

### A. MCQ

1. A project has fixed, well-documented requirements and a strict deadline. The most suitable methodology is:
   (a) Agile (b) Waterfall (c) Prototyping only (d) No methodology
2. Which testing requires every program statement to be executed at least once?
   (a) Black-box (b) White-box (c) Manual (d) Acceptance
3. Running test scripts with a tool to produce results automatically is:
   (a) Manual testing (b) Static testing (c) Automation testing (d) Black-box testing
4. In PDCA, measuring defect rates against objectives happens in:
   (a) Plan (b) Do (c) Check (d) Act
5. "The SRS specifications have been applied properly" describes:
   (a) Usability (b) Correctness (c) Reusability (d) Maintainability

**Answers:** 1-b, 2-b, 3-c, 4-c, 5-b

### B. Short answer

**B1. Give TWO differences between Agile and Waterfall. (4 marks)**
Agile is iterative and delivers small increments frequently, whereas Waterfall is linear and delivers the full product at the end. Agile welcomes requirement changes through continuous customer feedback, whereas Waterfall requires each phase to be completed before the next, with little room for revisiting earlier phases.

**B2. Why is black-box testing often performed by someone other than the code's author? (2 marks)**
Because the tester does not need to know the code, and an independent tester is not influenced by the developer's assumptions, so they are more likely to find defects the author overlooked.

**B3. State the four activities of a quality management system. (4 marks)**
Project auditing; quality system review; development of principles, procedures and protocols; production of reports for senior management summarising the quality system's success.

### C. Application

*Case: FoodNow is building a food-delivery app. The client keeps changing requirements after seeing demos.*

**C1.** Recommend a methodology and justify. (4 marks)
Agile — requirements are changing, and Agile's short iterations with continuous customer feedback let the team adjust direction after each demo and deliver working increments (ordering, payment, tracking) quickly; Waterfall would be costly because changes after a phase is complete require rework.

**C2.** Give one black-box and one white-box test for the "apply promo code" feature. (4 marks)
Black-box — a tester enters a valid, an expired and a misspelled promo code and checks that only the valid code reduces the total and the others show the correct error message. White-box — a developer designs test data so that each branch of the promo function (valid/expired, minimum spend met/not met, first-order-only true/false) executes at least once.

**C3.** A bug in delivery-fee calculation is found after launch. Explain two consequences. (4 marks)
Higher cost — the fix requires rework of design, code and tests, an emergency release to all users and refunds to over-charged customers. Customer dissatisfaction and reputational damage — users lose trust, leave bad reviews and may switch to competitors; if customers are harmed financially, the company may face complaints or legal claims.

### D. Thinking

**D1.** "Automation testing can replace manual testing completely." Discuss. (4 marks)
No. Automation is fast, repeatable and ideal for regression and load testing, but it only checks what the scripts are written to check. Manual testing is still needed to explore the software from the end user's perspective and judge usability, layout and unexpected behaviour. The best strategy combines both.

**D2.** Why must a quality system have top management approval? (3 marks)
Because quality is the responsibility of the whole organisation; without visible top-level support, employees view the quality system negatively, resources are not provided and procedures are not followed.

---

## Slide index

| Note section | Slides |
|---|---|
| 1 Methodology, Agile vs Waterfall | p3–6 |
| 1.1 Defect cost & liability | p7–9 |
| 2 Testing, manual, automation | p10–12 |
| 2.2 Dynamic, black-box, white-box | p13–15 |
| 3 QA & PDCA | p16–20 |
| 4 Quality characteristics & quality system | p21–25 |

## Links to other chapters
- SDLC policy & change management → **Ch6**
- Reliability pillar & CIA for banking apps → **Ch6**
- Product liability & professional responsibility → **Ch1**
- Protecting the finished software (copyright, patents, trade secrets) → **Ch8**
