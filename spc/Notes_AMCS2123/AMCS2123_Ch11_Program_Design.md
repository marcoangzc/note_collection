# AMCS2123 — Chapter 11: Program Design (Coupling & Cohesion)

> 两份考卷都有 2–6 分的理论题：**Oct 2025 Q1e** functional vs coincidental cohesion（2 分）；**Jan 2026 Q3f** tight coupling 为什么不好、最好的 coupling、最好的 cohesion 及原因（4 分）+ **Q3g** modular programming 的两个好处（2 分）。Slide 的问题是：几乎所有重点（levels 排序、每一级的特征、"characteristics → cohesion type" 判断表）都是**图片**；而且 p21 把 "If modules can access the same variables" 列为 **strong cohesion** 的条件——这其实是 **tight coupling**（p9 自己也这样写），前后矛盾。这份笔记把 7 级 cohesion、4 级 coupling 各配一个例子与"一句话判断法"，并完整回答 Tutorial 11。

---

## 0. 一句话总览

**好的程式设计要把程式分成模组，并遵守两条规则：reduce coupling（模组之间越少依赖越好，最好只靠参数传资料 = data coupling）、increase cohesion（模组内部的每一句都为同一件事服务，最好只做一件事 = functional cohesion）。**

| Part | 内容 | Slide |
|---|---|---|
| 1 | 为什么需要好的设计；分档储存；重复步骤抽成模组 | p2–5 |
| 2 | 变数与 function 命名 | p6 |
| 3 | **Coupling**：tight vs loose、4 levels | p7–17, p33 |
| 4 | **Cohesion**：strong vs weak、7 levels | p18–32 |
| 5 | Good programming habits | p34 |

🎯 **这章回答的考题**
- **Oct 2025 Q1e** Functional cohesion vs coincidental cohesion (2)
- **Jan 2026 Q3f** (i) 一个 tight coupling 不好的理由 (1) (ii) 最好的 coupling level (1) (iii) 最好的 cohesion level + 原因 (2)
- **Jan 2026 Q3g** Modular programming 两个好处 (2)（→ 也见 Ch5）

**Prerequisites**：Ch5 的 modular programming、global variable 的问题、pass by value / reference。

---

## Scene：ATM 程式

银行要你写 ATM：提款、存款、查余额。你一看，三种交易都有"验证 PIN → 显示选单 → 查余额"：

```
 Withdrawal: [Verify pin]→[Display menu]→[Prompt amount]→[Check balance]
             →[Eject money]→[Print receipt]
 Deposit:    [Verify pin]→[Display menu]→[Receive money]→[Check balance]
             →[Print receipt]
 Balance:    [Verify pin]→[Display menu]→[Check balance]→[Display balance]
```
*看图重点：Verify pin、Display menu、Check balance、Print receipt 重复出现 → 应该各写成**一个模组**，三种交易都呼叫它。*

但怎么切？切太细（一个模组一行）或太粗（一个模组做十件事）都不好。模组之间怎么传资料？这章给你两把尺：**coupling** 和 **cohesion**。

---

## 1. 为什么需要好的设计（p2–5）

- 程式**又大又复杂**，而且通常是**团队合作**。
- 把长程式存在**不同档案**：**reusability**（其他程式可重用）、**maintainability**（容易管理）。
- 从其他档案**引入**元件（如 function）：常见说法 **include / import / copy**（C++ 用 `#include`）。

### ✅ 满分答法 — Jan 2026 Q3g: TWO benefits of modular programming (2 marks)
1. **Reusability** – a module (e.g. `verifyPin()`) is written once and reused wherever it is needed, in this program or in other programs, reducing duplicated code.
2. **Maintainability / easier debugging** – each module is small and performs one task, so it can be tested, understood and corrected independently without affecting other modules; team members can also develop different modules in parallel.

---

## 2. 命名（p6）

| 原则 | Good | Bad |
|---|---|---|
| Use meaningful names | `student` | `s` |
| Use pronounceable names | `score` | `xfqb` |
| Use suitable abbreviations | `stud` | `st` |
| Avoid digits in names | `menu` | `menu1` |
| Separate words clearly | `studName` | `dotwice` |
| Use verbs for flags（存状态的变数） | `approve` | `approval` |

（Slide 表格的 Good 栏是空的——内容来自 slide 的文字清单。）

---

## 3. Coupling（p7–17）⭐

**Coupling** = 两个模组之间**连结（关系）强度**的量度。

| | **Tight coupling** ❌ | **Loose coupling** ✅ |
|---|---|---|
| 意思 | 模组彼此**紧密相关** | 模组大致**互不相关** |
| 何时发生 | 一个模组改了变数值，**其他模组受影响**（如用 global variable）；两个模组**能存取同一批变数** | 一个模组的改变**不影响**其他模组；资料透过**传参数**分享 |
| 后果 | 依赖多 → **容易出错**：要追踪的资料路径多；坏资料容易从一个模组传到另一个；一个模组容易改掉另一个模组需要的资料 | **易写、易维护、易重用** |

> 规则：**REDUCE coupling**。

### ✅ 满分答法 — Jan 2026 Q3f(i): ONE reason why tight coupling is not good (1 mark)
Tight coupling makes modules highly dependent on each other, so a change in one module (e.g. to a shared global variable) can unexpectedly affect other modules, making the program error-prone and hard to maintain.

### 3.1 四个 levels（loosest/best → tightest/worst）

```
 (Best) Loosest ◄────────────────────────────────────────────────► Tightest (Worst)
   Data coupling     Control coupling     External & common       Pathological
                                          coupling                coupling
   share data by     main program         share global            modules change
   passing           controls module      variables or            each other's
   parameters        actions              structures              data
```

| Level | 判断法 | Slide 例子 |
|---|---|---|
| **1. Data coupling**（最好，又称 normal / simple data coupling） | 只靠**参数**传必要的资料 | `final = calculateFinalPrice(totalPrice, salesTax)` → `finalPrice = price + (tax * price)` |
| **2. Control coupling** | 传进去的参数是**控制旗标**，告诉对方"做哪件事" | `chooseAction(inputCode)` 里 `CASE OF inputCode: 1 读员工记录 / 2 印页首 / 3 开档 / 4 pageCount 归零 / 5 错误讯息` |
| **3. External & common coupling** | 模组共用 **global variable 或 global structure** | global `salesTax`：`calculateSalesTax` 写入、`calculateAmountDue` 读取；global struct `Employee emp` 被 `readEmpRecord()` 与 `validateEmpSalary()` 共用 |
| **4. Pathological coupling**（最差） | 一个模组**直接修改另一个模组的资料**（不属于它的） | `calculateSalesTax` 把 `amountDue = 0`（不是它的）；`calculateAmountDue` 把 `salesTax = 0`（不是它的） |

> 💬 **答题句 (EN)** — *Data coupling is the best (loosest) level of coupling because modules share only the data they need by passing parameters, so each module is independent and a change in one does not affect the others. Pathological coupling is the worst (tightest) because modules change each other's data.*

### ✅ 满分答法 — Jan 2026 Q3f(ii): Best level of coupling (1 mark)
**Data coupling** – modules communicate only by passing the required data as parameters.

---

## 4. Cohesion（p18–32）⭐

**Cohesion** = 一个模组**内部强度（独立性）**的量度：模组里的叙述**多大程度上都为同一个目的服务**。

| | **Strong / high cohesion** ✅ | **Weak / low cohesion** ❌ |
|---|---|---|
| 意思 | 所有叙述都是为了达成**该 function 的目的** | 叙述和 function 的目的**无关** |
| 好处 | 可靠；较强、与其他模组关系少；**易写、易读、易维护** | 像 p20 的漫画：一个人同时煮饭、扫地、喂小孩、晾衣服——什么都做、什么都做不好 |

> 规则：**INCREASE cohesion**。

### 4.1 七个 levels（strongest/best → weakest/worst）

```
 (Best) strongest ◄──────────────────────────────────────────────────────────► weakest (Worst)
 Functional  Sequential  Communicational │ Temporal  Procedural  Logical     Coincidental
 perform     fewer tasks; tasks quite    │ more tasks; tasks less or not   many tasks;
 only 1 task related; often in sequence; │ related; may not be in sequence;totally not
             data are shared             │ data are not shared             related
```

| # | Level | 一句话判断 | Slide 例子 |
|---|---|---|---|
| 1 | **Functional** | 模组**只做一件事** | `calculateSalesTax`：依价格区间算出 salesTax |
| 2 | **Sequential** | 多个任务**有顺序**，前一个的输出是后一个的输入，**同一份资料** | `processPurchases`：累加 totalPurchase → 算 salesTax → 算 amountDue |
| 3 | **Communicational** | 多个相关任务作用在**同一份资料**，**顺序不重要** | 检查 productType、productNo 是否合法、是否空白（都针对同一笔产品资料） |
| 4 | **Temporal** | 任务**只因为要在同一时间做**而放在一起 | `Initialization`：开档、提示日期、读记录、印页首、归零… |
| 5 | **Procedural** | 主要做**两件无关的事**，有**特定顺序**，作用在**不同资料** | `readStudentRecords_And_TotalStudentAges` |
| 6 | **Logical** | 由一个**决定（CASE / if）**选择执行几件**无关**任务之一 | `readAllFiles(fileCode)`：1 读交易档 / 2 读客户档 / 3 读产品档 |
| 7 | **Coincidental** | 叙述之间**没有任何有意义的关系** | `fileProcessing`：开更新档、读员工记录、印页首、开主档、pageCount=1、errorFlag=false、印错误讯息 |

### 4.2 判断表（p31，Tutorial 11 Q9 的答案）

```
 Q1: Does the module perform a single task?
     YES ─────────────────────────────────────────► (i)   Functional
     NO  → Q2: What relates the activities?
           ├─ DATA relates them
           │     ├─ sequence important ───────────► (ii)  Sequential
           │     └─ sequence not important ───────► (iii) Communicational
           ├─ LOGIC CONTROL relates them
           │     ├─ sequence important ───────────► (iv)  Procedural
           │     └─ sequence not important ───────► (v)   Logical
           └─ NEITHER data nor logic
                 ├─ related by time ──────────────► (vi)  Temporal
                 └─ not related by time ──────────► (vii) Coincidental
```
*看图重点：先问"只做一件事吗？"→ 再问"是什么把这些任务绑在一起：资料？控制逻辑？还是什么都没有？"→ 最后问"顺序/时间"。*

### ✅ 满分答法 — Oct 2025 Q1e: Functional vs coincidental cohesion (2 marks)
**Functional cohesion** is the strongest (best) level: all statements in the module work together to perform **only one single task**, e.g. `calculateSalesTax`. (1)
**Coincidental cohesion** is the weakest (worst) level: the module performs **many unrelated tasks** whose statements have **no meaningful relationship** with each other, e.g. a module that opens a file, prints a heading and sets an error flag. (1)

### ✅ 满分答法 — Jan 2026 Q3f(iii): Best level of cohesion and reason (2 marks)
**Functional cohesion** (1) — every statement in the module contributes to performing only one task, so the module is independent, reliable, and easy to understand, test, maintain and reuse. (1)

---

## 5. Good programming habits（p34）

- **Plan before coding**；
- 用 **flowchart 或 pseudocode** 设计程式，方便写 code；
- **Desk check**：在写 code **之前**，手动检查程式逻辑（通常在 pseudocode 完成后）；
- 设计 **high cohesion、loose coupling** 的程式，方便维护与理解。

---

## 6. Tutorial 11 答案

**Q1** Seven levels：functional, sequential, communicational, temporal, procedural, logical, coincidental。最理想：**functional**；最不理想：**coincidental**。

**Q2** 任 6 种 → 见 §4.1 表（每种一句判断 + 例子）。

**Q3 定义**

| Term | Definition |
|---|---|
| i. Pathological coupling | Two or more modules change one another's data; the tightest (worst) coupling |
| ii. Simple data coupling | Modules share data only by passing parameters; the loosest (best) coupling |
| iii. External coupling | Modules share global variables or global data structures |
| iv. "Communicational coupling" | ⚠️ 不是 coupling 的一级；slide 只有 **communicational cohesion**：a module performs several related tasks on the same data, not necessarily in sequence |
| v. Functional cohesion | All operations in a module contribute to performing only one task |
| vi. Coincidental cohesion | A module's statements have no meaningful relationship with one another |
| vii. Logical cohesion | A module performs one of several unrelated tasks selected by a decision (case / if structure) |
| viii. Temporal cohesion | A module performs several tasks that are related only because they must occur within the same time span (e.g. initialisation) |

**Q4** Coupling = the measure of the strength of the connection between two modules；三级任选：data / control / external & common / pathological（§3.1）。

**Q5** Tight coupling：modules closely related and dependent；loose coupling：modules generally unrelated, sharing data only through parameters。**Most intimate way to share data** = **pathological coupling**（模组直接改彼此的资料）。

**Q6** (i) 任两级 §4.1；(ii) highest = **functional**；(iii) lowest = **coincidental**。

**Q7** Coupling = strength of the connection **between** modules → reduce it so modules are independent；Cohesion = internal strength **within** a module (how its statements serve one purpose) → increase it so each module does one well-defined job。

**Q8**
| 描述 | 答案 |
|---|---|
| i. loosest, normal coupling, passing parameters | **Data coupling** |
| ii. a parameter controls the module's actions | **Control coupling** |
| iii. modules change one another's data, tightest | **Pathological coupling** |
| iv. all operations contribute to one task, highest | **Functional cohesion** |
| v. performs tasks depending on a decision (case / if) | **Logical cohesion** |

**Q9**：(i) Functional (ii) Sequential (iii) Communicational (iv) Procedural (v) Logical (vi) Temporal (vii) Coincidental（§4.2 判断表）。

---

## Closing the loop

ATM：把 `verifyPin()`、`displayMenu()`、`checkBalance(balance)`、`printReceipt(...)` 各写成**只做一件事**的模组（functional cohesion），彼此之间**只传参数**（data coupling），不共用 global 变数——这样换一台 ATM、加一种交易，都不必改其他模组。

这也把整门课串起来：Ch5–6 的参数传递（value/reference/address）就是 data coupling 的工具；Ch6 的 global/extern 变数就是 common coupling 的来源。

---

## ⚠️ Where the slides mislead

| Slide | Slide 写 | 更准确 |
|---|---|---|
| p21 | Strong cohesion when "If modules can access the same variables" | 这是 **tight coupling**（p9 自己的定义）；strong cohesion 是"所有操作都与该 function 有关" |
| p25 | Communicational cohesion 的 callout 写 "tasks not necessarily in sequence" 但 code 标题 "ValIF" | 截断；内容是三个针对同一产品资料的检查 |
| p16 | global struct 例子中 `emp.ID`，struct 栏位是 `id` | 大小写不一致（示意用） |
| p6 | Good 栏空白 | 答案在文字清单：student, score, stud, menu, studName, approve |
| T11 Q3(iv) | "Communicational coupling" | 没有这一级；应是 communicational **cohesion** |
| p7, p18 | "Orgnization" | typo：Organization |

---

## Term table

| English | 中文 | 一句话说明 |
|---|---|---|
| Module | 模组 | 独立的程式单元（function） |
| Coupling | 耦合度 | 模组**之间**的依赖强度 |
| Tight / loose coupling | 紧 / 松耦合 | 依赖多 / 少 |
| Data coupling | 资料耦合 | 只传参数（最好） |
| Control coupling | 控制耦合 | 传控制旗标 |
| External / common coupling | 外部 / 共用耦合 | 共用 global |
| Pathological coupling | 病态耦合 | 改别人资料（最差） |
| Cohesion | 内聚力 | 模组**内部**的一致性 |
| Functional / sequential / communicational / temporal / procedural / logical / coincidental cohesion | 功能 / 顺序 / 通讯 / 时间 / 程序 / 逻辑 / 偶然内聚 | 7 级，由好到坏 |
| Desk check | 桌上检查 | 写 code 前手动检查逻辑 |

---

## Cheat sheet

- **Reduce coupling, increase cohesion.**
- Coupling（好→坏）：**Data → Control → External & Common → Pathological**（4）。
- Cohesion（好→坏）：**Functional → Sequential → Communicational → Temporal → Procedural → Logical → Coincidental**（7）。记法：**F**ast **S**ailors **C**an **T**ravel **P**eacefully **L**ike **C**aptains。
- Tight coupling 坏：一改影响别人、错误易传播、资料路径多、难维护。
- Best coupling = data（只传参数）；best cohesion = functional（只做一件事）。
- 判断 cohesion：一件事？→ 资料绑定（顺序重要 = sequential / 否 = communicational）；逻辑绑定（顺序重要 = procedural / 否 = logical）；都没有（时间相关 = temporal / 否 = coincidental）。
- Modular programming 好处：reusability、maintainability、easier debugging、teamwork。
- Habits：plan、flowchart/pseudocode、desk check、high cohesion + loose coupling。

---

## Practice (answers included)

### A. MCQ
1. Two modules both read and write a global variable `total`. Coupling?  (a) data (b) control (c) common (d) pathological
2. `printReport(int mode)` prints a summary if mode = 1, a detail if mode = 2. The caller's relationship is  (a) data coupling (b) control coupling (c) common coupling (d) none
3. A module `startUp()` opens files, clears counters and prints the date heading. Cohesion?  (a) functional (b) temporal (c) logical (d) sequential
4. `computeGPA(marks)` only calculates the GPA. Cohesion?  (a) functional (b) procedural (c) coincidental (d) communicational
5. Which is the WORST cohesion?  (a) logical (b) temporal (c) coincidental (d) procedural

**Answers:** 1-c, 2-b, 3-b, 4-a, 5-c

### B. Short answer
**B1.** Rewrite this pathological design as data coupling: `calcTax()` sets global `amountDue = 0`. — `calcTax(price)` should *return* the tax; `calcAmountDue(total, tax)` should *return* the amount due; no module changes another's variables.
**B2.** Why is logical cohesion weaker than communicational cohesion? — In logical cohesion the tasks are unrelated and chosen by a control flag; in communicational cohesion the tasks are related because they all work on the same data.

### C. Application
*A module `processOrder()` does: read order → validate order → compute total → apply tax → print invoice, all on the same order record in this order.*
**C1.** Identify its cohesion. → **Sequential** (several tasks in sequence on the same data; output of one feeds the next).
**C2.** How would you raise it to functional cohesion? → Split into `readOrder()`, `validateOrder()`, `computeTotal()`, `applyTax()`, `printInvoice()`, each doing one task, called in order by `processOrder()` and passing data as parameters (data coupling).

### D. Thinking
**D1.** Can a program have high cohesion but tight coupling? — Yes: each module may do one task (functional) but still share global variables (common coupling). Both rules must be followed.
**D2.** Why is desk checking done before coding? — Logic errors found in pseudocode are cheaper and faster to fix than after the program is written and tested.

---

## Slide index
| Note section | Slides |
|---|---|
| 1 Need for design | p2–5 |
| 2 Naming | p6 |
| 3 Coupling | p7–17, p33 |
| 4 Cohesion | p18–32 |
| 5 Habits | p34 |

## Links to other chapters
- Modular programming benefits, global variables → **Ch5**
- Parameter passing (data coupling tools), extern/global (common coupling) → **Ch5, Ch6**
- Structure chart, flowchart, pseudocode → **Ch5**
