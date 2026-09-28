# Ch7 软件开发 · Software Development

> ⏱ **建议 1 小时 45 分** · 📝 **每份 past year 的 Q3（25 分）都是这一章**（Ethan 的 ABC Bank 排队系统、banking app、payment app）
> 📖 范围 = 讲师 Revision Notes 的 Ch7 全部 4 节
> **怎么读**：每一节先看 ❓，自己想 5 秒 → 读"大白话" → 看例子 → 背 📖 讲师原句和 💬 答题句 → 最后看 ✅ 真题满分答案。

---

## 🗺️ 全章地图（Flow map：做一个软件的顺序）

```
 ① 选开发方法           ② 测试找 bug              ③ 品质保证 QA           ④ 好软件的特征
 Methodology     ──→    Software Testing   ──→    PDCA cycle      ──→    Quality attributes
 · Waterfall            · Manual                  Plan → Do →             · Scalability
 · Agile                · Automation              Check → Act             · Usability
 · 越早修越便宜 100×     · Dynamic:                 （一直循环）             · Reusability
                          Black-box / White-box                           · Correctness
                                                                          · Maintainability
```
*看图重点：① 决定"怎么做" → ② 找出错误 → ③ 让整个流程越来越好 → ④ 最后的软件要长成什么样。*

## 🎯 考试怎么问（五份 past year 的 Q3）

| 考点 | Oct 2024 | Jan 2025 | May 2025 (Ethan) | Oct 2025 | May 2026 (bank app) |
|---|---|---|---|---|---|
| 选 Waterfall / Agile + justify | | | 3a (5) | | 3a (4) |
| **越早发现 defect 越便宜** | 3a agree? (5) | 3d (3) | | 3a agree? (5) | 3b (9) |
| Goals of testing | | 3a (4) | | | |
| Manual vs automation | | | | | 3c (6) |
| **Black-box & White-box** | 3b (8) | 3b (8) | 3b (8) | 3b (8) | |
| **PDCA** | 3c 要画图 (10) | 3c (10) | | | |
| **Quality attributes** | 3d (2) | | 3c (12) | 3c (12) | 3d (6) |

👉 **Black/white-box 考了四次，每次 8 分；quality attributes 考了四次。** 这两块最先搞懂。

---

## 1. Software Development Methodology 软件开发方法

### ❓ 1.1 盖房子要先画图、再打地基、再起墙。做软件有没有"固定的做法"？

**大白话**
有。一群人一起做软件，如果没有固定的做法，你写你的、我写我的，最后一定乱掉。**Software development methodology（软件开发方法）就是一套大家都遵守的做事步骤**，让团队能**有效率地**做出**有品质**的软件。

📖 **讲师原句**：*"A **structured way** to create software **efficiently and with quality**."*

### ❓ 1.2 Waterfall 和 Agile 差在哪里？

**大白话**：想象两种做婚礼蛋糕的方法。
- **Waterfall（瀑布式）**：先和客人把蛋糕的样子**全部确定好**，然后烤蛋糕 → 抹奶油 → 装饰 → 交货。**一步做完才做下一步，不能回头**，就像瀑布的水只会往下流。适合客人**早就知道要什么、不会改**的情况。
- **Agile（敏捷式）**：先做一个小杯子蛋糕给客人试吃，客人说"甜一点"，就改；再做一个，再试……**一轮一轮做，一直听客人的意见来调整**。适合客人**还不确定、会一直改**的情况。

```
      WATERFALL（一层一层往下，不回头）            AGILE（一圈一圈重复）
 ┌────────────┐
 │Requirements│─┐                                    Plan
 └────────────┘ ↓                                  ↗      ↘
      ┌────────┐                              Review       Design
      │ Design │─┐                               ↑            ↓
      └────────┘ ↓                             Test   ←   Develop
         ┌────────────────┐                   （每一圈交出一个可以用的小版本，
         │ Implementation │─┐                   例如每 2 个星期更新一次 app）
         └────────────────┘ ↓
             ┌─────────┐
             │ Testing │─┐
             └─────────┘ ↓
                ┌─────────────┐
                │ Maintenance │
                └─────────────┘
```

| | **Waterfall** 瀑布式 | **Agile** 敏捷式 |
|---|---|---|
| 📖 讲师 | **Step-by-step, no going back** | **Flexible, iterative, feedback-driven** |
| 大白话 | 一步做完才做下一步，不回头 | 一小段一小段做，每段都听意见再改 |
| 📖 适合 | Projects with **fixed requirements** | **Changing requirements** |
| 🌰 讲师例子 | **Government tax software** system | **Mobile app updates every 2 weeks** |
| 好处 | 计划清楚、文件完整、容易控制进度和预算 | 能接受改变、早点给客户看到成果、问题早发现 |
| 坏处 | 后期才测试，需求一改就很难改 | 需要客户常常参与，最后的范围比较难预测 |

> 💬 **答题句 (EN)** — *Waterfall is a step-by-step methodology with no going back, suitable for projects with fixed requirements, such as a government tax software system. Agile is a flexible, iterative and feedback-driven methodology, suitable for projects with changing requirements, such as a mobile app that is updated every two weeks.*

### ❓ 1.3 考试叫你"选一个"，怎么选？

看题目的**线索字**：

| 题目说… | 选 |
|---|---|
| requirements are **well defined / fixed / clear**，有**固定 deadline**，团队**经验丰富** | **Waterfall** |
| requirements **may change**，需要 **customer feedback**，要**快速推出**、常常更新，想**改善测试流程** | **Agile** |

⚠️ 选哪一个都可以拿分，**分数在 justify**：每个理由都要连到题目里的线索。

### ✅ 真题满分答案

**May 2025 Q3a** — *Ethan leads a software team developing a real-time Queue Management System (including a mobile app) for ABC Bank. The team is skilled in development and testing, the requirements have been clearly defined, and the system must be completed within one year. Identify the most suitable system development model and justify your answer.* **(5 marks)**

> The most suitable model is **Waterfall** (1), because:
> - ABC Bank's requirements have already been **clearly defined**, and Waterfall is designed for projects with fixed requirements that are unlikely to change. (1)
> - Waterfall is **step-by-step**: requirements, design, implementation, testing and maintenance are done in order, so Ethan can plan clear milestones for the whole year and meet the **one-year deadline**. (1)
> - Each phase is completed and documented before the next begins, which gives the bank clear documents to review and approve for an important real-time banking system. (1)
> - Ethan's team is **skilled in development and testing**, so it can complete each phase correctly without needing many rounds of customer feedback. (1)

**May 2026 Q3a** — *A company is developing an online banking application and wants to improve its development and testing process so that the system is secure, reliable and error-free. Recommend a suitable software development methodology and justify your answer.* **(4 marks)**

> I recommend **Agile** (1), because:
> - Agile is **iterative**: the banking app is built in small parts (e.g. login, fund transfer, bill payment), and each part is developed and **tested in every iteration**, so errors are found early instead of only at the end. (1)
> - Agile is **feedback-driven**: bank staff and customers can review each version and give feedback, so problems with security or usability are corrected before the final release. (1)
> - Agile is **flexible**: banking rules and customer needs change often, and Agile can adapt to changing requirements through frequent updates, keeping the app reliable and up to date. (1)

---

### ❓ 1.4 同一个 bug，设计时发现 vs 上线后才发现，为什么价钱差那么多？⭐

**大白话**
想象你盖房子，**画图时**发现厕所画错位置，擦掉重画就好。但如果**房子盖好、住进去了**才发现，就要打掉墙、重新拉水管、重新铺地砖，还要赔住户的损失。

软件也一样：**越晚发现错误，前面做好的东西就要重做越多**，所以成本越高。讲师说：**早期修 bug 比上线后修便宜 100 倍**。

```
 设计时发现   → 改一份设计文件                                   成本 ×1
 写程式时发现 → 改设计 + 改程式码                                 成本 ↑
 测试时发现   → 改设计 + 改程式码 + 重新测试                      成本 ↑↑
 上线后发现   → 以上全部 + 发 patch 给每一个用户 + 客服处理投诉      成本 ×100
               + 用户受损要赔偿、可能被告 + 公司名声受损
```

📖 **讲师原句**：*"Fixing a bug **early (design stage)** is **100x cheaper** than fixing it **after release**."*

**为什么越晚越贵？（写答案用的 4 个理由）**

| 理由 | 中文 | 大白话 |
|---|---|---|
| **Rework** of earlier work | 前面的工作要重做 | 设计、程式码、测试全部要改 |
| Higher cost of **fixing and distributing** | 修正和发布的成本更高 | 要做 patch、重新测试、送到每个用户手上、客服接电话 |
| **Legal liability** | 法律责任 | 用户因为 bug 损失钱 → 公司被告、要赔偿 |
| Loss of **reputation** and customers | 名声受损、客户流失 | 用户不再信任，改用对手的产品 |

> 💬 **答题句 (EN)** — *Fixing a bug early, at the design stage, is up to 100 times cheaper than fixing it after release, because a late bug requires rework of the earlier design, code and tests, a patch must be built and sent to every user, and the company may face lawsuits and damage to its reputation.*

### ✅ 真题满分答案

**Oct 2024 Q3a / Oct 2025 Q3a** — *"Identifying and eliminating a defect early in the software development process will cost up to 100 times less than removing a defect in software already shipped to consumers." Do you agree? Justify your answer.* **(5 marks)**

> **Yes, I agree.** (1)
> 1. **Less rework:** a defect found at the design stage can be fixed by changing a design document, but a defect found after release means the design, code and tests that were built on it must all be redone. (1)
> 2. **Lower cost of fixing and distributing:** after release, the company must develop a patch, retest the whole system, send the update to every user and pay support staff to handle complaints, which costs far more than an early fix. (1)
> 3. **Avoiding legal liability:** a defect in shipped software can cause users to lose money or data, and the company may be sued and have to pay compensation. For example, a wrong interest calculation in a banking app could affect thousands of accounts. (1)
> 4. **Protecting reputation:** customers who experience crashes or errors lose trust and may switch to competitors, so finding defects early saves money and also improves software quality. (1)

**Jan 2025 Q3d** — *Identify the impact of discovering a flaw later in the production process.* **(3 marks)**

> 1. The work done earlier — design documents, code and test cases — must be reworked, so time and effort are wasted. (1)
> 2. The cost of correcting the flaw is much higher, up to 100 times more than fixing it at the design stage, because a patch must be built, tested and sent to every user. (1)
> 3. If the flaw reaches customers and causes them harm, the company may face lawsuits and compensation claims, and its reputation is damaged. (1)

**May 2026 Q3b** — *Analyse THREE effects of failing to detect software defects early in the development of the online banking application.* **(9 marks)**

> 1. **Much higher cost (3):** a defect found after release requires rework of the design, code and tests, plus an emergency patch and extra customer support; this can cost up to 100 times more than fixing it early. *Example:* an error in the interest calculation found after launch requires changes in several modules and recalculation of thousands of customer accounts.
> 2. **Customers lose money and satisfaction (3):** defects such as failed transfers, wrong balances or app crashes directly affect customers' money and their access to banking services, causing frustration and complaints. *Example:* a bug charges customers twice for a bill payment, and they must wait days for a refund.
> 3. **Damage to reputation and legal liability (3):** a security defect could let attackers take over accounts or steal customer data. The bank would face lawsuits, penalties under the PDPA 2010 and negative news, and customers may move to another bank. *Example:* a weak login check allows account takeovers, and the bank has to compensate the victims.

---

## 2. Software Testing 软件测试

### ❓ 2.1 程式写完了，为什么不直接给客户用，还要测试？

**大白话**
就像考试前要先做模拟考，看看哪里还不会。软件写完也要**先检查它是不是照要求运作**，把错误（bug）找出来，**再**交给客户。等客户发现 bug，就太晚、太贵了（回想 1.4 的 100 倍）。

📖 **讲师原句**：*Software testing "ensures the software **works as expected**."*

### ✅ 真题满分答案

**Jan 2025 Q3a** — *Determine TWO goals of software testing to ensure high-quality software.* **(4 marks)**

> 1. **To make sure the software works as expected (2):** testing checks that every function in the requirements behaves correctly, so the product delivered to the customer does what it was asked to do. *Example:* testing that a fund transfer moves exactly the amount entered.
> 2. **To find and fix bugs before release (2):** testing discovers defects while they are still cheap to fix, so users receive a more reliable product with fewer crashes and errors. *Example:* finding that the app crashes when the password field is left empty.

### ❓ 2.2 测试有哪几种方法？

| Type | 中文 | 📖 讲师定义 | 大白话 | 适合做什么 |
|---|---|---|---|---|
| **Manual testing** | 人工测试 | **Human testers simulate real usage** | 真人拿着手机，像用户一样一个一个按 | 看画面清不清楚、好不好用 |
| **Automation testing** | 自动化测试 | **Tools/scripts automatically test functionality** | 写好一段测试程式（script），电脑自己跑几千次 | 重复的、大量的测试，每次改 code 后都跑一次 |
| **Dynamic testing** | 动态测试 | **Runs the program to check behavior** | 把程式真的跑起来，看结果对不对 | 分成 **black-box** 和 **white-box**（下一节） |

### ✅ 真题满分答案

**May 2026 Q3c** — *Explain how the company can use manual testing and automation testing to test the online banking system.* **(6 marks)**

> - **Manual testing (3):** human testers simulate real usage of the banking app from the customer's point of view. They log in on different phones, transfer money, pay bills and check statements, and judge whether the screens are clear, the error messages are easy to understand, and elderly customers can complete a transfer without confusion. These are things that an automated tool cannot judge.
> - **Automation testing (3):** the company uses testing tools and scripts that automatically run thousands of test cases — for example, logins with correct and wrong passwords, transfers of different amounts and balance calculations — every time the code is changed. This gives fast, consistent and repeatable results before each release and can also simulate many users at the same time.

### ❓ 2.3 测试的人需不需要看得懂程式码？（Black-box vs White-box）⭐

**大白话**
- **Black-box testing（黑箱测试）**：把软件当成一个**黑色的箱子**，**看不到里面**。测试员只做一件事：**放东西进去（input），看出来的结果（output）对不对**。就像你用计算机按 2 + 2，看它是不是显示 4，你不需要知道里面的电路。
- **White-box testing（白箱测试）**：把软件当成一个**透明的箱子**，**看得到里面的程式码**。测试员要懂 code，把程式里**每一条路（每个 if/else）都走一遍**，确定**每一行 code 至少跑过一次**。

```
  BLACK-BOX（看不到里面）                       WHITE-BOX（看得到里面）
  input ──→ ┌──────────┐ ──→ output           input ──→ ┌─────────────────┐ ──→ output
            │██████████│                                │ if (age >= 18)  │
            │██ ??? ███│                                │    → allow  ●   │
            │██████████│                                │ else            │
            └──────────┘                                │    → reject ●   │
  只问：输出对不对？                                     └─────────────────┘
                                                      两条路都要测到
```

| | **Black-box** 黑箱测试 | **White-box** 白箱测试 |
|---|---|---|
| 📖 讲师 | Tester **doesn't know the code**, only checks **inputs & outputs** | Tester **knows code structure** and checks **all logic paths** |
| 🌰 讲师例子 | Testing a **login form** with **valid/invalid inputs** | Checking that **every line of a function runs at least once** |
| 要懂 code 吗 | **不用** | **要** |
| 谁来做 | 独立测试员、用户（通常不是写 code 的人） | 开发者（programmer） |
| 根据什么设计测试 | 根据**需求 / 规格**（软件应该做什么） | 根据**程式的结构**（code 里有哪些路） |
| 找到什么错 | 功能缺少、画面错、输出不对 | 逻辑错、条件写错、跑不到的 code |

> 💬 **答题句 (EN)** — *In black-box testing, the tester does not know the code and only checks whether the inputs produce the expected outputs, for example testing a login form with valid and invalid inputs. In white-box testing, the tester knows the code structure and checks all logic paths, for example making sure every line of a function runs at least once.*

### ✅ 真题满分答案

**May 2025 Q3b** — *Explain black-box testing and white-box testing, and provide a scenario on how Ethan's team can carry out each testing method for the Queue Management System.* **(4 + 4 marks)**

> **(i) Black-box testing (4):** In black-box testing, the tester does not know the code and only checks whether the inputs produce the expected outputs; it is usually done by someone who did not write the code. (2)
> *Scenario:* an independent tester uses the mobile app to take a queue number for "Cash Deposit" at an ABC Bank branch. The tester checks that the app shows the next number in order and the estimated waiting time, and that the branch display screen updates immediately when the counter calls the number. The tester also enters invalid input, such as choosing a branch that is closed, and checks that a clear error message appears — all without looking at the code. (2)
>
> **(ii) White-box testing (4):** In white-box testing, the tester knows the code structure and checks all logic paths, so that every line of code runs at least once. (2)
> *Scenario:* a developer in Ethan's team tests the function that assigns customers to counters. They prepare test data for every path in the code — when a counter is free, when all counters are busy, when a priority customer (e.g. a senior citizen) arrives, and when the queue is empty — and confirm that each `if/else` branch runs and gives the correct result. (2)

**Oct 2025 Q3b** — *Explain black-box testing and white-box testing, and provide a scenario on how a software development team can carry out each method.* **(4 + 4 marks)**

> **(i) Black-box testing (4):** the tester does not know the code and only checks inputs and outputs against the requirements. (2) *Scenario:* for an online shopping system, a tester enters a valid card number, an expired card and a card number with missing digits at the checkout page, and checks that the valid card is accepted while the others are rejected with a clear error message. (2)
>
> **(ii) White-box testing (4):** the tester knows the code structure and checks all logic paths so every line runs at least once. (2) *Scenario:* a developer tests the discount-calculation function with data for every path — no discount, member discount, voucher only, member plus voucher, and an expired voucher — and checks that each branch of the code runs and calculates the correct price. (2)

**Jan 2025 Q3b** — *Examine the differences between black-box and white-box testing, and provide a scenario where each should be applied.* **(8 marks)**

> **Differences (4):**
>
> | | Black-box testing | White-box testing |
> |---|---|---|
> | Knowledge of code | Tester does not know the code | Tester knows the code structure |
> | What is checked | Inputs and outputs only | All logic paths; every line runs at least once |
> | Who performs it | Usually independent testers or users | Usually developers |
> | Test data based on | Requirements / specification | The program's internal logic |
>
> **Scenarios (4):**
> - **Black-box** should be applied when checking that the whole system meets user needs, e.g. before launching an online banking app, testers who did not write the code check that fund transfers, bill payments and balance displays work as described in the requirements. (2)
> - **White-box** should be applied when a critical piece of code must be fully correct, e.g. developers test the password-validation function so that every condition (too short, no number, correct password, account locked) is executed and handled properly. (2)

**Oct 2024 Q3b** — *Examine how black-box testing and white-box testing can be used to eliminate defects in software, with relevant examples.* **(8 marks)**

> - **Black-box testing (4):** testers enter data based on the requirements and compare the actual output with the expected output, without looking at the code. This finds missing or wrong functions and screen errors. Because the tester did not write the code, they do not share the developer's assumptions and can catch mistakes the developer missed. *Example:* entering the date 31/02/2000 in a registration form shows that the system wrongly accepts an impossible date, so the defect is reported and fixed.
> - **White-box testing (4):** testers who know the code design test data so that every logic path runs and every line is executed at least once. This finds logic errors, wrong conditions and code that never runs. *Example:* testing a loan-approval function with an income of exactly RM3,000 reveals that the code uses `>` instead of `>=`, wrongly rejecting customers who should qualify; the developer corrects the condition before release.

---

## 3. Quality Assurance (QA) & PDCA 品质保证

### ❓ 测完一次就够了吗？怎样让"做软件的方法"越来越好？

**大白话**
你每次考试后，会检讨"哪一章错最多"，下次就多读那一章——这就是在**改进你的读书方法**。公司做软件也一样，要有一个**不停循环的改进方法**，叫 **PDCA cycle**（又叫 **Deming cycle**，戴明循环）：

**先计划 → 去做 → 检查结果 → 改进 → 再计划……** 一直转下去，每一圈都比上一圈好。

**Quality Assurance（QA，品质保证）**就是确保软件 **"fit for use"（适合使用、能满足用户需要）** 的工作，PDCA 就是 QA 用的方法。

```
                 ┌──────────┐
          ┌─────→│   PLAN   │──────┐
          │      │ 定目标    │      │
          │      └──────────┘      ↓
     ┌──────────┐            ┌──────────┐
     │   ACT    │            │    DO    │
     │ 改进流程  │            │  去执行   │
     └──────────┘            └──────────┘
          ↑      ┌──────────┐      │
          └──────│  CHECK   │←─────┘
                 │ 检查结果  │
                 └──────────┘
          Quality Assurance process（一直循环）
```
*画图要点：4 个框、顺时针箭头、**Act 之后回到 Plan**（这就是"持续改进"）。考试说 "with the aid of a diagram" 一定要画，图本身有分。*

📖 **讲师原句**
- "QA Definition: Ensures software is **'fit for use'**."
- "**PDCA Cycle (Deming Cycle)**: 1. **Plan** → Define objectives. 2. **Do** → Implement. 3. **Check** → Monitor results. 4. **Act** → Improve process."
- 🌰 "If **user complaints rise**, QA **reviews the process** and **updates testing**."

| Stage | 中文 | 📖 讲师 | 大白话 | 🌰 banking app 例子 |
|---|---|---|---|---|
| **Plan** | 计划 | **Define objectives** | 定目标、想好怎么做 | 目标：上线时没有严重 bug，app 当机率少于 0.1%；定好测试计划 |
| **Do** | 执行 | **Implement** | 照计划去做 | 照计划写程式、做 black-box 和 white-box 测试 |
| **Check** | 检查 | **Monitor results** | 看结果有没有达到目标 | 发现转账功能的 bug 最多，用户投诉增加 |
| **Act** | 行动 | **Improve process** | 针对问题改进做法 | 转账功能加强测试、加 code review，然后开始下一轮 Plan |

> 💬 **答题句 (EN)** — *Quality assurance ensures that software is fit for use. It uses the PDCA (Deming) cycle: Plan defines the objectives, Do implements the plan, Check monitors the results, and Act improves the process. The cycle repeats continuously; for example, if user complaints rise, QA reviews the process and updates testing.*

### ✅ 真题满分答案

**Oct 2024 Q3c** — *With the aid of a diagram, discuss how an organisation applies the PDCA cycle to verify the quality of software goods or services. Explain each process.* **(10 marks)**

> **Diagram (2):** *(draw the four boxes Plan → Do → Check → Act in a clockwise circle, with an arrow from Act back to Plan, labelled "Quality Assurance process")*
>
> - **Plan (2):** the organisation defines its quality objectives for the software, such as the maximum number of defects allowed at release and response-time targets, and plans how to achieve them, including coding standards, test plans and review procedures.
> - **Do (2):** the plan is implemented — developers build the software following the agreed standards, and testers carry out the planned manual, automated, black-box and white-box tests.
> - **Check (2):** the organisation monitors the results against the objectives by analysing test reports, defect counts and customer feedback, to see where the quality goals were not met — for example, most defects come from the payment module.
> - **Act (2):** the organisation improves the process based on what it found — for example, adding extra testing and code reviews for the payment module and updating its standards. The cycle then starts again with a new Plan, so quality keeps improving.

**Jan 2025 Q3c** — *Explain the purpose of the PDCA cycle in quality assurance and describe its stages.* **(10 marks)**

> **Purpose (2):** Quality assurance ensures that software is fit for use. The PDCA (Deming) cycle is a repeating four-step method that QA uses to continually check and improve the development process, so that fewer defects reach the final product and quality gets better with each cycle. For example, if user complaints rise, QA reviews the process and updates the testing.
>
> **Stages (8):**
> - **Plan (2)** – define the quality objectives and decide how to reach them, e.g. set a target of zero critical bugs at release and prepare a test plan.
> - **Do (2)** – implement the plan by developing and testing the software as planned.
> - **Check (2)** – monitor the results by comparing test results, defect reports and user feedback with the objectives.
> - **Act (2)** – improve the process by fixing the causes of the problems found, e.g. adding more testing where most bugs occur; then the cycle repeats.

---

## 4. Software Quality Attributes 好软件的 5 个特征

### ❓ 怎样的 app 才算是"好 app"？

**大白话**
想想你最喜欢用的 app：它在 Android 和 iPhone 上都能用、你阿嬷也会用、算钱从来不会错、有 bug 很快就修好……这些就是**好软件的特征（quality attributes）**。讲师给了 5 个。

记法：**S-U-R-C-M** → "**S**uper **U**ser **R**eally **C**an **M**aintain"

| Attribute | 中文 | 📖 讲师定义 | 大白话 | 🌰 mobile payment app 例子 |
|---|---|---|---|---|
| **Scalability** | 可扩展性 | **Runs on many devices** | 在很多种装置上都能跑 | Android 和 iOS、手机和平板都能用 |
| **Usability** | 易用性 | **Easy for all users** | 谁都会用 | 简单的画面、大字体，老人家也会付钱 |
| **Reusability** | 可重用性 | **Code components reused in other projects** | 写好的零件可以拿去别的项目用 | 登入和 OTP 功能，拿去公司的另一个 app 用 |
| **Correctness** | 正确性 | **Meets specifications** | 照规格做对 | 转账金额、手续费都照规格算对 |
| **Maintainability** | 可维护性 | **Easy to fix and update** | 容易修、容易加新功能 | 发现漏洞一天内修好，新加 DuitNow QR 不影响其他功能 |

🌰 **讲师的例子**：*A **mobile payment app** should be **usable by all (simple UI)**, **scalable (works on Android & iOS)**, and **maintainable (bugs fixed quickly)**.*

⚠️ 讲师的 Scalability 写 "runs on many devices"（其实比较接近 portability）。**照讲师写**；想加分可以补一句 *"and it can also handle more users as the business grows."*

> 💬 **答题句 (EN)** — *High-quality software shows five key attributes: scalability (it runs on many devices), usability (it is easy for all users), reusability (its code components can be reused in other projects), correctness (it meets the specifications) and maintainability (it is easy to fix and update).*

### ✅ 真题满分答案

**May 2025 Q3c** — *Describe FOUR characteristics of a high-quality software product that Ethan's team's Queue Management System should exhibit.* **(12 marks)**

> 1. **Scalability (3)** – the software runs on many devices. The Queue Management System must work on customers' Android and iOS phones, on the kiosks at ABC Bank branches and on the counter staff's computers, and should keep working smoothly as more branches start using it.
> 2. **Usability (3)** – the software is easy for all users. Customers of all ages should be able to take a queue number with one tap and clearly see their position and waiting time, while counter staff can call the next customer with one click.
> 3. **Correctness (3)** – the software meets its specifications. Queue numbers must be issued in the correct order, waiting times must be calculated as specified, and priority customers must be served according to ABC Bank's rules.
> 4. **Maintainability (3)** – the software is easy to fix and update. ABC Bank may later want to add appointment booking or new service types; a well-organised design allows Ethan's team to add these and fix bugs quickly without breaking existing features.

**Oct 2025 Q3c** — *As a software designer planning a mobile app for secure online payments, explain how you would apply FOUR characteristics of high-quality software in your design to meet user needs.* **(12 marks)**

> 1. **Correctness (3)** – I would write detailed test cases from the specification for every payment function (entering amounts, fees, receipts) and use both black-box and white-box testing, so that every payment is processed exactly as specified. Users need their money to be handled accurately.
> 2. **Usability (3)** – I would design a simple, consistent interface with clear confirmation screens, readable fonts and easy-to-understand error messages, so that users of all ages can pay quickly without making mistakes.
> 3. **Scalability (3)** – I would build the app to run on both Android and iOS phones and different screen sizes, and use cloud servers that can handle more transactions during busy periods, so the app works on every user's device and stays fast.
> 4. **Maintainability (3)** – I would organise the code into separate modules with clear documentation, so that security patches and new payment methods (e.g. a new e-wallet) can be added quickly without breaking other features. Users need an app that stays secure and up to date.

**May 2026 Q3d** — *Explain how THREE software quality attributes help improve the quality of the online banking application.* **(6 marks)**

> 1. **Correctness (2)** – it makes sure all specified functions, such as balance calculations and fund transfers, work exactly as required, so customers' money is always handled accurately.
> 2. **Usability (2)** – it makes the app easy for all types of customers to use, which reduces mistakes such as transferring money to the wrong account and increases customer satisfaction.
> 3. **Maintainability (2)** – it allows bugs and security weaknesses to be fixed quickly and new banking features to be added easily, keeping the app secure and reliable over time.

**Oct 2024 Q3d** — *Describe a characteristic of good software.* **(2 marks)**

> **Maintainability** – good software is easy to fix and update (1). For example, when a bug is found in a banking app, developers can locate and fix it quickly, and new features can be added without affecting the rest of the system (1).

---

## 🔁 自测：我问，你先想，答案在下面

1. 讲师对 software development methodology 的定义？
2. Waterfall 和 Agile 各用哪 3 个字形容？各举讲师的一个例子。
3. 题目说"requirements are clearly defined, must finish within one year"，选哪个？
4. 早期修 bug 比上线后修便宜几倍？给 3 个理由。
5. Manual、automation、dynamic testing 各是什么？
6. Black-box 和 white-box 最大的差别？讲师的例子各是什么？
7. PDCA 四个字母各是什么？讲师怎么形容每一步？它又叫什么 cycle？
8. QA 的定义（讲师原句）？
9. 讲师的 5 个 quality attributes？
10. "Easy to fix and update" 是哪一个？"Runs on many devices" 呢？

.

.

.

.

**答案**
1. **A structured way to create software efficiently and with quality**
2. Waterfall = **step-by-step, no going back**（government tax software）；Agile = **flexible, iterative, feedback-driven**（mobile app updates every 2 weeks）
3. **Waterfall**
4. **100×**；**rework** of earlier work · higher cost of **patching and distributing** · **lawsuits** · **reputation** loss
5. Manual = **human testers simulate real usage** · Automation = **tools/scripts automatically test** · Dynamic = **runs the program to check behaviour**
6. Black-box **doesn't know the code**, checks **inputs & outputs**（login form valid/invalid）；White-box **knows the code**, checks **all logic paths**（every line runs at least once）
7. **Plan**（define objectives）· **Do**（implement）· **Check**（monitor results）· **Act**（improve process）；**Deming cycle**
8. **Ensures software is "fit for use"**
9. **Scalability, Usability, Reusability, Correctness, Maintainability**
10. **Maintainability**；**Scalability**

---

## 🧾 一页背诵（考前 5 分钟看）

- **Methodology** = structured way to create software efficiently & with quality
- **Waterfall** = step-by-step, no going back, **fixed requirements**（government tax system）· **Agile** = flexible, iterative, feedback-driven, **changing requirements**（app updates every 2 weeks）
- 选法：需求清楚 + deadline → Waterfall；需求会变 + 要回馈 → Agile；**justify 连到题目线索**
- **Bug early = 100× cheaper**：rework · patch & support cost · lawsuits · reputation
- **Testing** = works as expected · 目标：meet requirements + find bugs before release
- **Manual**（humans simulate real usage）· **Automation**（tools/scripts）· **Dynamic**（runs the program）
- **Black-box** = don't know code, inputs & outputs（login form）· **White-box** = know code, all logic paths（every line once）
- **QA** = fit for use · **PDCA / Deming**：Plan (define objectives) → Do (implement) → Check (monitor results) → Act (improve process) → 循环 · **要画图**
- **Quality attributes**：**S**calability（many devices）· **U**sability（all users）· **R**eusability（reuse code）· **C**orrectness（meets spec）· **M**aintainability（easy fix & update）

➡️ 下一章：**Ch8 Intellectual Property**（软件做好了，怎么保护它不被别人抄）
