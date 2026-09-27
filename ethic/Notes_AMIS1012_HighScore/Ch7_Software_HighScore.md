# Ch7 Software Development — 高分版

> **每份考卷的 Q3（25 分）**：选 Waterfall / Agile 并 justify、"defect 越早修越便宜"同不同意、**black-box / white-box 各给一个 scenario（8 分，四份卷都考）**、PDCA（10 分）、quality characteristics（6–12 分）。
> 高分关键：black-box / white-box 的 scenario 要写出**真的 test case**（输入什么、预期结果是什么、发现了什么 bug）；defect 成本要有**数字**；quality attribute 要写**这个系统的具体设计决定**。

---

## 0. 讲师 tips 的考点 → 本页位置

| Tips 考点 | 本页 |
|---|---|
| Waterfall vs Agile | §1 ⭐ |
| Cost of fixing errors（早修便宜 100 倍） | §2 ⭐ |
| Manual、automation、dynamic testing（black-box、white-box） | §3 ⭐⭐ |
| QA definition（fit for use）+ **PDCA** | §4 ⭐ |
| Quality attributes：scalability、usability、reusability、correctness、maintainability | §5 ⭐ |

---

## 1. Waterfall vs Agile ⭐

| | **Waterfall** | **Agile** |
|---|---|---|
| 形式 | 线性、一步一步，**不回头** | 弹性、iterative，短周期（例如 2 星期一个 sprint） |
| 适合 | 需求**固定、清楚**，期限固定（tips 例：政府报税系统） | 需求**会变**，要常常拿客户回馈（tips 例：每 2 星期更新的手机 app） |
| 测试 | 在后段集中测试 | 每个 sprint 都测试 |

### ✅ 高分答法 — May 2025 Q3a: *Identify the suitable system development model for Ethan and his team (ABC Bank real-time Queue Management System, requirements well defined, one-year deadline). Justify.* (5 marks)

**Waterfall** is the suitable model (1).
- **Requirements are well defined and stable** (1): ABC Bank has already specified the features — ticket issuing at the kiosk, counter-calling screens in each branch, and a mobile app that shows the live queue number — so the team can finish the full requirements and design before coding, with little risk of later change.
- **Fixed one-year deadline** (1): Waterfall's sequential phases can be scheduled as milestones, e.g. requirements and design by month 3, coding by month 8, testing by month 10, deployment to pilot branches by month 12, making progress easy for the bank to monitor.
- **Team expertise and documentation** (1): Ethan's team is experienced in development and testing, so a full test phase can be planned; Waterfall produces complete documentation, which a bank needs for audit and future maintenance.
- **Conclusion** (1): because change is unlikely and the scope is clear, Waterfall gives ABC Bank a predictable schedule and cost; Agile's frequent iteration is not necessary here.

### ✅ 高分答法 — May 2026 Q3a: *Recommend a suitable software development methodology for the online banking application (must be secure, reliable and error-free before release). Justify.* (4 marks)

**Agile** is recommended (1). Security and correctness are tested in every 2-week sprint instead of only at the end — e.g. the login and fingerprint module is built, security-tested and reviewed by the bank's compliance team in sprint 1, before fund transfer is added in sprint 2 — so defects are found early when they are cheap to fix (1). Banking regulations and customer needs change often (e.g. new DuitNow QR rules), and Agile can adapt to them through continuous customer feedback (1). Frequent working releases to a pilot group of 500 staff let the company confirm reliability before the public launch (1).

*（也可选 Waterfall，但必须 justify：需求由法规固定、需要完整文件与审核。两个都能拿分，重点是理由连到"secure, reliable, error-free"。）*

---

## 2. Cost of Fixing Defects ⭐

**核心点**：在设计阶段修 bug 比上线后修**便宜到 100 倍**。越晚发现：要改的东西越多（需求 → 设计 → 程序 → 测试 → 已安装在用户那里的版本），还要处理客户、赔偿、名誉、诉讼。

**📈 用数字说明（你自己的情境）**

| 发现阶段 | 例子：BayarLah 利息计算错误 | 修复成本 |
|---|---|---|
| Requirements / design review | 分析师在 review 时发现公式写错，改一行文件 | 约 RM100（1 小时） |
| Coding / unit test | Developer 的 unit test 失败，改程序重测 | 约 RM1,000 |
| System testing | 测试员发现，要改程序、重新整合、重跑所有测试 | 约 RM3,000 |
| **After release** | 30 万用户的利息算错，要紧急更新、人工核对、退款、客服、公关、罚款 | **RM100,000 以上（≈ 1,000 倍）** |

### ✅ 高分答法 — Oct 2024 Q3a / Oct 2025 Q3a: *"Identifying and eliminating a defect early … will cost (up to 100 times) less than removing a defect in software already shipped to consumers." Do you agree? Justify.* (5 marks)

**Yes, I agree** (1).
- **Less rework early** (1): a defect found in requirements or design only needs a document to be corrected; once coded, integrated and shipped, the same defect requires changes to the code, retesting of every related module, a new release and re-installation. *E.g.* a wrong interest formula in BayarLah's design costs about RM100 to correct in review, but after release it needs an emergency update to 300,000 phones.
- **Customer and financial damage** (1): shipped defects affect real users — the wrong formula would mis-credit interest to 300,000 accounts, requiring manual checking, refunds and extra customer-service staff, easily costing over RM100,000.
- **Reputation and legal risk** (1): customers lose trust and may leave; a defect that exposes data or money can lead to regulatory fines and lawsuits. *E.g.* in 2024 a faulty CrowdStrike update crashed about 8.5 million Windows computers worldwide, disrupting airlines and banks.
- **Conclusion** (1): therefore investing in reviews, unit testing and early testing is far cheaper than fixing defects after shipment.

### ✅ 高分答法 — Jan 2025 Q3d: *Identify the impact of discovering a flaw later in the production process.* (3 marks)
1. **Much higher cost** – more work must be redone: e.g. a flaw in KedaiKu's checkout found after launch requires changing the code, retesting the payment flow and redeploying, costing about 100 times more than fixing it in design (1).
2. **Delays** – the release date slips, e.g. the 11.11 sale feature is postponed by two weeks, losing expected sales (1).
3. **Customer and reputation damage** – users are affected before the fix: 1,200 customers are double-charged, complain on social media and some move to competitors; the company may also face legal claims (1).

### ✅ 高分答法 — May 2026 Q3b: *Analyse THREE effects of failing to detect software defects early in the banking application.* (9 marks)
3 分一个（效果 + 具体情境 + 后果）：
1. **Higher cost and delay to fix** – a defect missed in design must later be fixed across code, tests and deployed apps. *E.g.* a rounding error in the transfer-fee calculation, found only after launch, forces an emergency release, re-testing of every payment module and manual correction of 40,000 transactions, costing far more than a design-stage fix and delaying the next planned features by a month.
2. **Financial harm and loss of customer trust** – customers' money is directly affected. *E.g.* a bug that double-debits bill payments takes RM2.3 million from 18,000 customers over a weekend; they cannot pay rent or buy groceries, flood the hotline and close their accounts.
3. **Security breaches, legal action and regulatory penalties** – undetected security defects can be exploited. *E.g.* a session-token flaw lets attackers take over 900 accounts; the bank must compensate victims, report to the regulator, may be fined and sued, and its name appears in the news, damaging its reputation for years.

---

## 3. Software Testing ⭐⭐

| | **Manual** | **Automation** |
|---|---|---|
| 做法 | 人像真实用户一样操作软件，照 spec 检查每个功能 | 用工具 / test scripts 自动执行测试、自动产生结果 |
| 适合 | 使用体验、探索性测试、只测一次的功能 | 重复的 regression test、大量资料、压力测试 |

**Dynamic testing** = **执行程序**来检查行为：
- **Black-box**：测试员**不看程序码**，只看 **input → output** 是否符合 spec。
- **White-box**：测试员**知道程序码结构**，检查**每一条逻辑路径 / 每一行**都被执行过且正确。

### ✅ 高分答法 — May 2025 Q3b: *Explain black-box and white-box testing and provide a scenario on how Ethan and his team can carry out each (Queue Management System).* (4 + 4 marks)

**(i) Black-box testing (4)** – testing the system's functions against the requirements without looking at the internal code; the tester only checks whether each input gives the expected output (2). *Scenario:* a tester from Ethan's team acts as a bank customer and runs these cases on the kiosk and app (2):

| Test case | Input | Expected output |
|---|---|---|
| Normal ticket | Choose "Cash Deposit" at 10:05 a.m. | Ticket **D-015** printed; app shows "7 people ahead" |
| Counter call | Teller at Counter 3 presses "Next" | Screen shows "D-015 → Counter 3" and the app sends a push notification |
| Boundary | Choose a service at 4:59 p.m. (branch closes 5:00 p.m.) | Ticket issued with a warning |
| Invalid | Choose a service at 5:01 p.m. | "Branch closed" message, no ticket |

When the 5:01 p.m. case still printed a ticket, the tester reported the defect without needing to read any code.

**(ii) White-box testing (4)** – testing the internal logic and code structure, making sure every statement, branch and path is executed and correct (2). *Scenario:* the developer who wrote the `assignCounter()` function designs tests so that every branch runs (2): (1) a normal customer with Counter 3 free; (2) a senior citizen, which should take the priority branch; (3) all counters busy, which should add the ticket to the waiting list. Running branch (3) reveals that the waiting-list loop never ends when the list is empty, so the developer fixes the loop condition before release. A code-coverage tool confirms 100% of branches were executed.

### ✅ 高分答法 — Oct 2025 Q3b: *(Same question for "a software development team".)* (4 + 4 marks)
用同样结构，自己设定系统。例：**BayarLah e-wallet transfer function**
- **Black-box (4):** without seeing the code, the tester enters a transfer of RM50 to a valid phone number (expected: success and new balance shown), RM0 (expected: "Invalid amount"), RM5,001 above the RM5,000 daily limit (expected: "Limit exceeded"), and a wrong 6-digit PIN three times (expected: account locked for 30 minutes). The RM5,001 case went through, revealing a missing limit check.
- **White-box (4):** the developer reviews `transfer()` and writes tests to cover each branch — balance sufficient, balance insufficient, limit reached, PIN wrong — and checks that the database rollback runs if the network fails halfway. The test shows money was deducted from the sender but not credited when the connection dropped, so the developer wraps both steps in one database transaction.

### ✅ 高分答法 — Jan 2025 Q3b: *Examine the differences between black-box and white-box testing and provide a scenario for each where it should be applied.* (8 marks)

| | Black-box | White-box |
|---|---|---|
| Knowledge of code (1) | Tester does **not** need to know the code | Tester **must** know the code structure |
| Focus (1) | Functions and outputs against requirements | Internal logic, paths, branches and statements |
| Who (1) | Independent testers, users, QA team | Developers |
| When found (1) | Missing / wrong features, interface errors | Logic errors, unreachable code, wrong conditions |

- **Black-box scenario (2):** before KedaiKu launches its new voucher feature, the QA team tests it like shoppers: code "RAYA10" on a RM100 cart should give RM10 off; an expired code should be rejected; a RM30 cart below the RM50 minimum should be rejected. Applied here because the goal is to confirm the feature behaves as customers and the marketing spec expect.
- **White-box scenario (2):** MedikaCare's developer tests the dosage-calculation function that uses a patient's weight and age; she writes tests to execute every `if` branch (child, adult, elderly, weight missing) because a single wrong branch could give a dangerous dose — so every path in the code must be proven correct.

### ✅ 高分答法 — Oct 2024 Q3b: *Examine how black-box and white-box testing can be used to eliminate defects in software, with relevant examples.* (8 marks)
- **Black-box testing (4):** it eliminates defects by checking every function against the specification using valid, invalid and boundary inputs, so missing or wrong behaviour is found before users see it. *Example:* testers of an online movie-ticket site try booking 0 seats, 1 seat and 11 seats (limit 10); booking 11 seats succeeds, exposing a missing validation that is fixed before launch.
- **White-box testing (4):** it eliminates defects by testing the internal code so that every statement and branch is executed at least once, exposing logic errors that normal use might not trigger. *Example:* a developer testing the discount function finds that the "member AND above RM200" branch uses `OR`, giving every customer the member discount; branch coverage reveals it and the condition is corrected.

### ✅ 高分答法 — Jan 2025 Q3a: *Determine TWO goals of software testing to ensure high-quality software.* (4 marks)
1. **Verify that the software meets its required specifications** (2) – e.g. testing confirms that ABC Bank's queue app really shows each customer's live position and sends a notification when they are 3 numbers away, as the specification requires.
2. **Find defects (bugs) before release** (2) – e.g. testing KedaiKu's checkout finds that a 0% voucher crashes the payment page, so it is fixed before 50,000 customers use it, delivering a more reliable product.

### ✅ 高分答法 — May 2026 Q3c: *Explain how the company can use manual testing and automation testing to test the banking system.* (6 marks)
- **Manual testing (3)** – human testers use the banking app as real customers would, following the specification and exploring the interface. *E.g.* 10 testers, including two senior citizens, open an account, add a payee and transfer RM100 on different phones, checking whether the steps are clear, the fingerprint prompt appears and the error messages are understandable — things a script cannot judge.
- **Automation testing (3)** – test scripts run by tools execute large numbers of tests automatically and repeatedly. *E.g.* after every code change, an automated suite runs 1,200 regression tests overnight (login, transfer, bill payment, balance checks) and a load test simulates 50,000 users logging in at 9 a.m. on salary day; a report shows any failed test by morning, so a new change cannot silently break existing features.

---

## 4. Quality Assurance & PDCA ⭐

**核心点**：QA 确保软件 **"fit for use"**。**PDCA（Deming cycle）**：Plan → Do → Check → Act，不断重复，持续改进。

```
                ┌────────┐
         ┌─────>│  PLAN  │──────┐
         │      └────────┘      │
         │                      ▼
    ┌────────┐             ┌────────┐
    │  ACT   │             │   DO   │
    └────────┘             └────────┘
         ▲                      │
         │      ┌────────┐      │
         └──────│ CHECK  │<─────┘
                └────────┘
       Quality assurance process (repeats)
```

### ✅ 高分答法 — Oct 2024 Q3c: *With the aid of a diagram, discuss how an organisation applies the PDCA cycle to verify the quality of software goods or services. Explain each process.* (10 marks)

**Diagram (2):** draw the cycle above.
用**一个完整的改进情境**走四步（每步 2 分）——*KedaiKu 的 app 在 11.11 之后 crash 投诉暴增*：
- **Plan (2):** the QA team sets a clear objective — reduce app crashes from 2.1% of sessions to below 0.5% before the next sale in December — and plans the process: add automated crash reporting, require code review for every change, and write test cases for low-memory phones.
- **Do (2):** developers implement the plan over two sprints: they add the crash-reporting tool, fix the top 10 crash causes, and testers run the new test cases on 15 phone models, including older Android phones with 3 GB of RAM.
- **Check (2):** the team measures results against the objective: after release, crash reports fall to 0.8% — better, but still above the 0.5% target; analysis shows most remaining crashes come from the image-upload screen.
- **Act (2):** the team standardises what worked (mandatory code review and the phone test list become permanent rules) and acts on the gap by rewriting the image-upload module; the cycle then returns to **Plan** with a new target, so quality keeps improving.

### ✅ 高分答法 — Jan 2025 Q3c: *Explain the purpose of the PDCA cycle in quality assurance and describe its stages.* (10 marks)
**Purpose (2):** PDCA gives the organisation a repeatable loop for continually evaluating and improving its processes, so software is verified to be developed and delivered correctly and defects in the final product are reduced — quality becomes an ongoing activity rather than a one-time check.
**Stages (2 each):** use the same KedaiKu crash-reduction scenario above — Plan (target 0.5% crashes, new review and test process), Do (implement over two sprints, test 15 phone models), Check (measured 0.8%, cause found in image upload), Act (standardise code review, rewrite module, start the next cycle).

---

## 5. Quality Attributes ⭐

| Attribute（tips） | 意思 | 设计决定的例子 |
|---|---|---|
| **Scalability** | 能在多种装置 / 平台上运作（slide 定义）；也能应付用户增加 | Android + iOS + web；auto-scaling server 应付 10 倍流量 |
| **Usability** | 所有用户都容易使用 | 3 步完成付款、大字体、BM/English/中文 |
| **Reusability** | 元件可重复用在其他系统 | 登入 + OTP 模组也用在同公司的保险 app |
| **Correctness** | 符合 specification | 手续费、汇率、利息计算完全正确 |
| **Maintainability** | 容易修 bug、加功能 | 模组化、有注解、有自动测试 |

### ✅ 高分答法 — Oct 2025 Q3c: *You are a software designer planning a new mobile app for secure online payments. Explain how you would apply FOUR characteristics of a high-quality software product in your design.* (12 marks)

3 分一个（特性 + **我会做的具体设计** + 为什么满足用户）：
1. **Usability** – I would design the payment flow so any user, including elderly users, can pay in three steps: scan QR → confirm amount → fingerprint. Buttons will be large, the app will offer BM, English and Chinese, and each error message will say what to do (e.g. "Balance too low — top up RM20?"). This reduces failed payments and support calls.
2. **Correctness** – every calculation will match the specification: amounts are stored in cents (integers) to avoid rounding errors, transfer limits follow the RM5,000 daily rule, and each payment is recorded exactly once using a unique transaction ID. I would verify this with 300 automated test cases before each release, so users are never under- or over-charged.
3. **Scalability** – the app will run on Android and iOS phones and tablets, and the backend will auto-scale from 5 to 50 servers when traffic rises, e.g. at 12 a.m. during a shopping festival when payments jump tenfold, so users do not see "server busy".
4. **Maintainability** – I would split the system into separate modules (login, wallet, payment, rewards) with clear documentation and automated tests, so a new feature like DuitNow QR can be added, or a security bug patched, in days without breaking other modules.

### ✅ 高分答法 — May 2025 Q3c: *Describe FOUR characteristics of a high-quality software product that Ethan's team's (Queue Management) system should exhibit.* (12 marks)
3 分一个，全部连到 ABC Bank 的系统：
1. **Correctness** – the system must implement ABC Bank's specification exactly: ticket numbers are issued in order per service (D-001, D-002…), the counter screen and the app always show the same number, and priority customers are served according to the bank's rules. A wrong number would send customers to the wrong counter.
2. **Usability** – customers of all ages must use the kiosk and app easily: large touch buttons, service names in BM and English, and a clear "7 people ahead — about 15 minutes" display, so elderly customers do not need staff help.
3. **Scalability** – the system should work on kiosks, counter screens, Android and iOS, and handle all 150 branches on salary day, when queues triple, without slowing down.
4. **Maintainability** – the code should be modular (ticketing, counter display, notification) and documented, so when ABC Bank adds a new service such as "Foreign Exchange" next year, Ethan's team can add it in a few days without affecting the rest of the system.

### ✅ 高分答法 — May 2026 Q3d: *Explain how THREE software quality attributes help improve the quality of the banking application.* (6 marks)
2 分一个：
1. **Correctness** – transfer amounts, fees and interest are calculated exactly as specified (e.g. a RM1,000 transfer with a RM0.50 fee deducts exactly RM1,000.50), so customers' balances are always right.
2. **Usability** – a clear three-step transfer with fingerprint confirmation lets all customers, including the elderly, bank without errors or branch visits.
3. **Maintainability** – modular code with automated tests lets the bank patch a security flaw within hours and add new features such as DuitNow QR without breaking existing functions.

### ✅ 高分答法 — Oct 2024 Q3d: *Describe a characteristic of good software.* (2 marks)
**Usability** – all types of users can use the software's features conveniently (1). *E.g.* KedaiKu's app lets a first-time 65-year-old user place a grocery order in under two minutes, with large buttons and a "Reorder last week's items" button (1).

---

## 6. 自己练习的新情境题（附高分答案）

**P1. A university wants a new course-registration system. Registration rules change every semester. Recommend Waterfall or Agile. (4 marks)**
> Agile. Rules such as credit limits and prerequisite checks change every semester, and Agile's 2-week sprints let the team add or adjust them quickly with feedback from the registry office. A working version can be piloted with 200 students before full launch, finding problems early instead of after 20,000 students register.

**P2. Give one black-box test and one white-box test for a function `calculateGrade(mark)` that returns A for 80–100, B for 65–79, C for 50–64, F for 0–49. (4 marks)**
> *Black-box:* without the code, test boundary inputs — 80 → A, 79 → B, 50 → C, 49 → F, 101 → error — and compare outputs with the grading rules. *White-box:* using the code, write tests so every `if` branch executes at least once (one mark in each range plus an invalid mark), and check that the invalid-input branch is reachable.

---

## 7. Cheat sheet

- **Waterfall**：fixed clear requirements、sequential、fixed deadline（Ethan）；**Agile**：changing requirements、sprints、feedback。
- **Defect cost**：early fix up to 100× cheaper；late = rework + customer harm + reputation + lawsuits（用 RM100 → RM100,000 的例子）。
- **Testing goals**：meet specifications · find bugs before release。
- **Manual**（人、体验）vs **Automation**（scripts、regression、load）。
- **Black-box**：不看 code，input → output；**White-box**：看 code，每条 branch / path。**一定写 test case 表**。
- **PDCA**：Plan（目标 + 流程）→ Do（执行）→ Check（量测比较）→ Act（标准化 + 改进）→ 回到 Plan；**一定画图**。
- **Quality**：scalability · usability · reusability · correctness · maintainability——每个写"我会怎么设计"。
