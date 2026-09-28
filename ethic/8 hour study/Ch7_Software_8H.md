# Ch7 软件开发 · Software Development

> ⏱ **建议 1 小时 30 分** · 📝 **每份 past year 的 Q3（25 分）都是这一章**（Ethan 的 ABC Bank 排队系统、banking app、payment app）
> 📖 范围 = 讲师 Revision Notes 的 Ch7（4 节）
> 用法：每一节先看 ❓，自己想 5 秒，再往下读。英文 **粗体** 是考试要写的字。

---

## 🗺️ 全章地图（Flow map：做软件的顺序）

```
 ① 选方法              ② 测试                    ③ 品质保证 QA           ④ 好软件的特征
 Methodology    ──→    Software Testing   ──→    PDCA cycle      ──→    Quality attributes
 · Waterfall           · Manual                  Plan → Do →             · Scalability
 · Agile               · Automation              Check → Act             · Usability
 · 越早修越便宜 100×    · Dynamic:                 （一直循环）             · Reusability
                         Black-box / White-box                           · Correctness
                                                                         · Maintainability
```
*看图重点：①怎么做 → ②找 bug → ③让流程越来越好 → ④最后的软件要长什么样。*

---

## 🎯 考试怎么问（五份 past year）

| 考点 | 考过哪几份 | 分数 | 重要度 |
|---|---|---|---|
| **Black-box & White-box + 情境** | Oct 2024、Jan 2025、May 2025、Oct 2025 | 8 | ⭐⭐⭐ |
| **Quality attributes（3–4 个）** | Oct 2024、May 2025、Oct 2025、May 2026 | 2–12 | ⭐⭐⭐ |
| **越早发现 defect 越便宜（agree? / effects）** | Oct 2024、Jan 2025、Oct 2025、May 2026 | 3–9 | ⭐⭐⭐ |
| **PDCA（要画图）** | Oct 2024、Jan 2025 | 10 | ⭐⭐ |
| 选 Waterfall / Agile + justify | May 2025、May 2026 | 4–5 | ⭐⭐ |
| Manual vs automation | May 2026 | 6 | ⭐ |
| Goals of testing | Jan 2025 | 4 | ⭐ |

---

## 1. Software Development Methodology 软件开发方法

### ❓ 盖房子要先画图再打地基；做软件有没有"固定的做法"？

有，叫 **methodology**（方法论）。

📖 **讲师原句**：*"A **structured way** to create software **efficiently and with quality**."*

### ❓ Waterfall 和 Agile 差在哪？

想象两种做蛋糕的方法：
- **Waterfall**：食谱全部定好，一步做完才做下一步，**不能回头**。
- **Agile**：先做一个小杯子蛋糕给客人试吃，照意见改，**一轮一轮**做。

```
      WATERFALL（瀑布，一层一层往下）             AGILE（一圈一圈重复）
 ┌────────────┐
 │Requirements│─┐                                 Plan
 └────────────┘ ↓                               ↗      ↘
      ┌────────┐                            Review      Design
      │ Design │─┐                             ↑           ↓
      └────────┘ ↓                           Test  ←  Develop
         ┌────────────────┐                  （每 2 周交一个小版本）
         │ Implementation │─┐
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
| 适合 | **Fixed requirements**（需求固定） | **Changing requirements**（需求会变） |
| 🌰 讲师例子 | **Government tax software** system | **Mobile app updates every 2 weeks** |

> 💬 **答题句 (EN)** — *Waterfall is a step-by-step methodology with no going back, suitable for projects with fixed requirements, such as a government tax system. Agile is flexible, iterative and feedback-driven, suitable for projects with changing requirements, such as a mobile app updated every two weeks.*

### 📝 真题：怎么选？看题目的"线索字"

| 题目说… | 选 |
|---|---|
| requirements are **well defined / fixed**，有 **deadline**，团队经验丰富 | **Waterfall** |
| requirements **may change**，要 **customer feedback**，要快速推出、常更新 | **Agile** |

- **May 2025 Q3a**（Ethan / ABC Bank，需求已定义清楚、一年内完成）(5) → **Waterfall**：① requirements are **well defined** by ABC Bank ② clear **phases & milestones** to meet the **one-year deadline** ③ each phase **documented** before the next ④ the team is experienced, so it does not need frequent iterations。
- **May 2026 Q3a**（banking app，要改善开发和测试流程）(4) → **Agile**：① small **iterations**, each is **tested** → bugs found early ② **customer feedback** on each feature (login, transfer) ③ **frequent releases** to check security & reliability。

⚠️ 选哪个都可以拿分，**分数在 justify**：每个理由都要连到题目里的线索。

### 1.1 越早修越便宜 ⭐

### ❓ 同一个 bug，设计时发现 vs 上线后才发现，为什么价钱差这么多？

📖 **讲师原句**：*"Fixing a bug **early (design stage)** is **100x cheaper** than fixing it **after release**."*

大白话：越晚发现，前面做好的东西**全部要重做**。

```
 设计时发现   → 改一份文件                                     成本 ×1
 写程式时发现 → 改文件 + 改设计 + 改 code                      成本 ↑
 测试时发现   → 以上全部 + 重新测试                            成本 ↑↑
 上线后发现   → 以上全部 + 每个用户都要 patch + 客服 + 赔偿     成本 ×100
               + 可能被告（product warranty lawsuit）+ 名声受损
```

| 理由（写 agree 时用） | 中文 |
|---|---|
| **Rework** of earlier deliverables | 前面做好的要重做 |
| Higher cost of **communicating and correcting** | 沟通和修正成本更高（patch、客服） |
| **Legal liability** – product warranty lawsuits | 用户受损 → 被告 |
| Loss of **reputation** and customers | 名声变差、客户流失 |

### 📝 真题：Oct 2024 / Oct 2025 Q3a — *"Identifying and eliminating a defect early… will cost up to 100 times less…" Do you agree? Justify.* (5 marks)
✅ **Yes, I agree** (1) + 上表 4 个理由各 1 分，每个加一句例子。
> 🌰 例子：a bank app's interest calculation is wrong. Found in design → change one formula in the document. Found after release → fix the code, retest, push an update to 200,000 users, recalculate thousands of accounts, and handle complaints.

- **Jan 2025 Q3d** — *Impact of discovering a flaw later.* (3) → rework + 100× cost + lawsuits / reputation。
- **May 2026 Q3b** — *THREE effects of failing to detect defects early in the banking app.* (9) → 每个 3 分：**higher cost** · **customers lose money / satisfaction**（转账失败、扣两次钱）· **reputation damage + legal liability**（漏洞 → 帐户被盗）。

---

## 2. Software Testing 软件测试

### ❓ 为什么要测试？

📖 **讲师原句**：*"Ensures the software **works as expected**."*

📝 **Jan 2025 Q3a** — *TWO goals of software testing.* (4) → ① verify the software **meets its required specifications** ② **find bugs before release**，交付更好的产品。

### 2.1 三种测试

| Type | 中文 | 📖 讲师定义 | 大白话 |
|---|---|---|---|
| **Manual testing** | 人工测试 | **Human testers simulate real usage** | 真人拿手机一个一个按 |
| **Automation testing** | 自动化测试 | **Tools/scripts** automatically test functionality | 写好 script，电脑自己跑几千次 |
| **Dynamic testing** | 动态测试 | **Runs the program** to check behavior | 把程式跑起来看结果（分 black-box / white-box） |

### 📝 真题：May 2026 Q3c — *How can the company use manual testing and automation testing to test the banking system?* (6 marks)
- **Manual (3)** – human testers simulate real usage from the customer's view: log in on different phones, transfer money, pay bills, and judge whether screens are **clear and easy** for elderly users — something a tool cannot judge.
- **Automation (3)** – scripts automatically run thousands of transfers, logins and balance calculations **after every code change**, giving **fast, repeatable** results before each release.

### 2.2 Black-box vs White-box ⭐（4 份卷都考）

### ❓ 测试的人需不需要看得懂程式码？

- **Black-box**（黑箱）：箱子是黑的，**看不到里面**，只看**输入 → 输出**对不对。
- **White-box**（白箱）：箱子是透明的，**看得到里面的 code**，每一条路都要走过。

```
  BLACK-BOX                                   WHITE-BOX
  input ──→ ┌──────────┐ ──→ output           input ──→ ┌────────────────┐ ──→ output
            │██████████│                                │ if (a>b) ─→ ●  │
            │██ ??? ███│                                │  else ────→ ●  │
            └──────────┘                                └────────────────┘
  不知道 code，只看结果对不对                   知道 code，每条 logic path 都测
```

| | **Black-box** 黑箱测试 | **White-box** 白箱测试 |
|---|---|---|
| 📖 讲师 | Tester **doesn't know the code**, only checks **inputs & outputs** | Tester **knows code structure** and checks **all logic paths** |
| 🌰 讲师例子 | Testing a **login form** with **valid/invalid inputs** | Checking that **every line** of a function **runs at least once** |
| 谁做 | 独立测试员、用户（不是写 code 的人） | 开发者 |
| 找到什么 | 功能错、画面错、输出错 | 逻辑错、条件写错、跑不到的 code |

> 💬 **答题句 (EN)** — *In black-box testing the tester does not know the code and only checks that inputs produce the expected outputs, e.g. testing a login form with valid and invalid inputs. In white-box testing the tester knows the code structure and checks all logic paths, e.g. making sure every line of a function runs at least once.*

### 📝 真题：May 2025 / Oct 2025 Q3b — *Explain black-box and white-box testing and provide a scenario for each.* (4 + 4 marks)
✅ 每个 = **定义 (2) + 自己的情境 (2)**。以 Ethan 的**排队系统**为例：
- **Black-box** *scenario:* an independent tester uses the app to take a queue number for "Cash Deposit", and checks that the next number appears, the waiting time is shown, and the branch screen updates when the counter calls it — **without looking at the code**.
- **White-box** *scenario:* a developer tests the function that assigns customers to counters, with test data for **every branch** — a counter is free, all counters are busy, a priority customer arrives, the queue is empty — so **every line runs at least once**.

- **Jan 2025 Q3b** — *Differences + a scenario where each should be applied.* (8) → 差别用上表 4 行（4 分）+ black-box 用在 **user acceptance testing**、white-box 用在 **unit testing** of the interest-calculation function（4 分）。
- **Oct 2024 Q3b** — *How can both be used to eliminate defects?* (8) → black-box 找出**功能 / 输出**错（输入 31/02/2000 竟然被接受）；white-box 找出**逻辑**错（收入刚好 RM3,000 时，code 写 `>` 而不是 `>=`）。

---

## 3. Quality Assurance (QA) & PDCA 品质保证

### ❓ 测完一次就够了吗？怎样让"做软件的流程"越来越好？

用一个**不停循环**的方法：**PDCA**。

📖 **讲师原句**：*"QA ensures software is **'fit for use'**."* · *"**PDCA Cycle (Deming Cycle)**"*

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
*画图要点：4 个框 + 顺时针箭头 + **Act 回到 Plan**（这就是"持续改进"）。*

| Stage | 中文 | 📖 讲师 | 🌰 banking app 例子 |
|---|---|---|---|
| **Plan** | 计划 | **Define objectives** | 目标：上线时 0 个 critical bug，crash rate < 0.1% |
| **Do** | 执行 | **Implement** | 照计划写 code、跑测试 |
| **Check** | 检查 | **Monitor results** | 看 bug 报告：转账模组 bug 最多 |
| **Act** | 行动 | **Improve process** | 转账模组加强 white-box testing，再进入下一轮 Plan |

🌰 讲师例子：*If **user complaints rise**, QA **reviews the process** and **updates testing**.*

> 💬 **答题句 (EN)** — *Quality assurance ensures software is fit for use. It applies the PDCA (Deming) cycle: Plan defines the objectives, Do implements the plan, Check monitors the results, and Act improves the process; the cycle repeats for continuous improvement.*

### 📝 真题
- **Oct 2024 Q3c** — *With the aid of a diagram, discuss how an organisation applies the PDCA cycle to verify software quality.* (10) → **图 2 分** + 每个 stage 2 分（讲师定义 + 软件例子）。⚠️ 没画图会失分。
- **Jan 2025 Q3c** — *Purpose of PDCA in QA + describe its stages.* (10) → Purpose（2）：a **repeating cycle** so the development process is **continually evaluated and improved**, reducing faults in the final product + 4 stages × 2。

---

## 4. Software Quality Attributes 好软件的 5 个特征

### ❓ 怎样的 app 才算"好 app"？

📖 **讲师的 5 个 attributes**（记法 **S-U-R-C-M**，"**S**uper **U**ser **R**eally **C**an **M**aintain"）：

| Attribute | 中文 | 📖 讲师定义 | 🌰 讲师例子（mobile payment app） |
|---|---|---|---|
| **Scalability** | 可扩展性 | **Runs on many devices** | Works on **Android & iOS** |
| **Usability** | 易用性 | **Easy for all users** | **Simple UI**, usable by all |
| **Reusability** | 可重用性 | Code components **reused in other projects** | 登入 / OTP 模组拿去另一个 app 用 |
| **Correctness** | 正确性 | **Meets specifications** | 转账金额、手续费照规格算对 |
| **Maintainability** | 可维护性 | **Easy to fix and update** | **Bugs fixed quickly**，容易加新功能 |

⚠️ 讲师的 Scalability（"runs on many devices"）其实比较像 portability。**照讲师写**，想加分可补一句 *"and it can also handle more users as the business grows."*

> 💬 **答题句 (EN)** — *Quality software shows scalability (runs on many devices), usability (easy for all users), reusability (code components can be reused in other projects), correctness (meets the specifications) and maintainability (easy to fix and update).*

### 📝 真题
- **May 2025 Q3c** — *FOUR characteristics Ethan's system should exhibit.* (12) → 每个 3 分 = **定义 + 为什么排队系统需要 + 例子**（例：Usability – customers of all ages can take a number in one tap）。
- **Oct 2025 Q3c** — *As the designer of a secure payment app, how would you apply FOUR characteristics?* (12) → 每个 3 分 = attribute + **你在设计上怎么做** + 满足用户什么需要。
- **May 2026 Q3d** — *THREE attributes that improve the banking app.* (6) → 每个 2 分：Correctness（钱算对）、Usability（少按错）、Maintainability（漏洞修得快）。
- **Oct 2024 Q3d** — *ONE characteristic of good software.* (2) → **Maintainability** – easy to fix and update（1）+ 例子（1）。

---

## 🔁 自测：我问，你先想，答案在下面

1. 讲师怎么形容 Waterfall 和 Agile？各一个例子。
2. 题目说"requirements are well defined, must finish in one year"，选哪个？
3. 早期修 bug 比上线后修便宜几倍？给两个理由。
4. Black-box 和 white-box 最大的差别？讲师的例子各是什么？
5. PDCA 四个字母各是什么？它又叫什么 cycle？
6. 讲师的 5 个 quality attributes？
7. "Easy to fix and update" 是哪一个？

.

.

.

.

**答案**
1. Waterfall = **step-by-step, no going back**（government tax software）；Agile = **flexible, iterative, feedback-driven**（mobile app updates every 2 weeks）
2. **Waterfall**
3. **100×**；**rework** of earlier work、**lawsuits / reputation**（或 patch 给所有用户的成本）
4. Black-box **不知道 code**，只看 input/output（login form valid/invalid）；White-box **知道 code**，测所有 logic paths（every line runs at least once）
5. **Plan, Do, Check, Act**；**Deming cycle**
6. **Scalability, Usability, Reusability, Correctness, Maintainability**
7. **Maintainability**

---

## 🧾 一页背诵（考前 5 分钟看）

- **Methodology** = structured way to create software efficiently & with quality
- **Waterfall** = step-by-step, no going back, **fixed requirements**（tax system）· **Agile** = flexible, iterative, feedback-driven, **changing requirements**（app every 2 weeks）
- **Bug early = 100× cheaper**：rework · patch & support cost · lawsuits · reputation
- **Testing** = works as expected：**Manual**（humans simulate real usage）· **Automation**（tools/scripts）· **Dynamic**（runs the program）
- **Black-box** = don't know code, inputs & outputs（login form）· **White-box** = know code, all logic paths（every line once）
- **QA** = fit for use · **PDCA / Deming**：Plan (objectives) → Do (implement) → Check (monitor) → Act (improve) → 循环 · **画图！**
- **Quality**：**S**calability（many devices）· **U**sability（all users）· **R**eusability（reuse code）· **C**orrectness（meets spec）· **M**aintainability（easy fix & update）

➡️ 下一章：**Ch8 Intellectual Property**（软件做好了，怎么保护它不被抄）
