# AMCS2104 — Past Year 考点分析（14 份，2022–2026）

> 新课号 **AMCS2104**（2025–2026，5 份）的题型跟你的考试一样，**最重要**。
> 旧课号 **AACS3273**（2022–2024，9 份）的题型可以当练习，但 Q4 常考 Chapter 6（逻辑），现在已经不考。

---

## 1. 新课号 5 份：每题考什么

| 考卷 | Q1 | Q2 | Q3 | Q4 |
|---|---|---|---|---|
| **Jun 2025** | 4 approaches 表格 (6)、Turing Test (5)、PEAS (12)、omniscience (2) | Landscape：objective (7) + cost (7)、5 个 labels (10)、改进算法 (1) | Problem formulation (6)、**A\*** 树 (9)、CSP 定义 + 排课 formulation (10) | **EA**：warehouse robots，5 phases (15) + 2 stopping criteria (8) |
| **Jul 2025** | Turing Test (5)、Total Turing Test 2 要求 + 6 capabilities (8)、**6 环境属性**：Google Photos (12) | Classical vs local search (8)、hill climbing + 3 优点 (6)、8-queens h + 两张 landscape (11) | Minimax / alpha-beta 定义 (4)、**game tree** minimax (7) + **左到右 + 右到左 pruning** (14) | **CSP**：CSP 定义、Alldiff、global constraints (6)、4×4 Sudoku formulation (9) + **AC-3** 5 格 (10) |
| **Oct 2025** | **4 approaches** (8)、Turing Test 别名 (1)、**6 capabilities** (12)、**rationality 4 criteria** (4) | Greedy (2)、**landscape objective + cost** (14)、**卡住 3 原因** (9) | Problem formulation (6)、**BFS + DFS + A\*** 树 (16) + solutions (3) | **6×6 Sudoku** formulation (11) + AC-3 3 格 (9)、**EA 5 concepts** (5) |
| **Jan 2026** | **4 approaches** (8)、Meta AI 能不能过 Turing / Total Turing Test (11)、agent 定义 (2)、**rationality 4 criteria** (4) | **EA**：交通灯，5 phases (15) + 2 stopping criteria (8) | Multiagent 类型 (2)、**minimax** (6)、greedy + 2 优点 + landscape + 2 原因 (17) | Problem formulation (6)、**BFS + DFS + A\*** 树 (16) + solutions (3) |
| **May 2026** | Optimisation 例子、local vs classical、hill climbing 优缺点、local search 例子 (12)、**landscape 5 元素 + 画图** (13) | **4 approaches** (4)、perception vs action (4)、**PEAS** SmartCar (6)、**A\*** 树 + path + cost (11) | Games 5 特征 (5)、**minimax** (6)、**左到右 + 右到左 pruning** (12) + 结论 (2) | CSP **4 种 assignments** + solution 定义 (6)、4×4 Sudoku formulation (9) + **AC-3** 5 格 (10) |

**新课号的 Q 号码会换**：同一个题型可能出现在 Q1–Q4 任何一题，所以要按**题型**准备，不要按题号。

---

## 2. 按题型统计（新课号 5 份，每份 100 分）

| 题型 | 出现次数 | 5 份合计分数 | 对应章节 |
|---|---|---|---|
| **Search tree（BFS / DFS / A\*）+ problem formulation** | **5 / 5** | ≈ 94 | Ch2 |
| **Hill climbing + state-space landscape** | **5 / 5** | ≈ 97 | Ch3 |
| **Ch1 背诵**（4 approaches、Turing Test、PEAS、环境属性、rationality） | **5 / 5** | ≈ 118 | Ch1 |
| **CSP + Sudoku + AC-3** | **4 / 5** | ≈ 71 | Ch5 |
| **Game tree（minimax + alpha-beta）** | **3 / 5** | ≈ 59 | Ch4 |
| **Evolutionary algorithm** | **3 / 5** | ≈ 51 | Ch3 |
| Chapter 6 逻辑 | **0 / 5** | 0 | — |

### 每次都考的（一定要会）

1. **State-space landscape 画图**：5 份都考。通常 objective 版和 cost 版各 7 分，要标 **2 条轴 + 5 个元素**。
2. **Search tree**：5 份都考，大多是 **BFS + DFS + A\* 三棵一起画**（约 16 分），前面一定有 **problem formulation 6 个 components**（6 分）。
3. **Ch1 背诵**：
   - **4 approaches**：5 份里 4 份考，约 8 分
   - **Turing Test**：5 份里 4 份考，包括别名 imitation game、3 方怎么运作、Total Turing Test 多 2 个要求
   - **6 capabilities**：约 12 分
   - **Rationality 4 criteria**：约 4 分
   - **PEAS**、**6 个环境属性**

### 常考的

4. **CSP + Sudoku**：4 份考。格式固定：
   - 先写 **formulation**（variables、domains、Alldiff constraints），约 9–11 分
   - 再用 **AC-3** 推几格，每格约 2 分，写 row / column / box 各删掉什么
5. **Game tree**：3 份考。先算 **minimax value**，再做 alpha-beta **左到右 + 右到左**两个方向，最后写结论（order 会影响 pruning，但 root 值一样）。
6. **EA**：3 份考。题目是**情境题**（仓库机器人、交通灯），答 **5 phases** 要套进情境（15 分），再写 **2 个 stopping criteria**（8 分）。

---

## 3. 旧课号 9 份：可以拿来练的题

| 题型 | 旧卷出现 | 例子 |
|---|---|---|
| Game tree minimax / alpha-beta | 9 / 9 | 每份都有，可以多练 |
| Search（BFS / DFS / A\* / GBF vs SAHC） | 6 / 9 | May 2022、Oct 2022、July 2023、May 2023（TSP）、Oct 2024 |
| CSP（排课、地图、Sudoku、abstract relation） | 6 / 9 | May 2023、July 2023、May 2024、July 2024 |
| PEAS / 环境属性 / agent 类型 / learning agent | 8 / 9 | Minesweeper、self-driving taxi、LPR、facial recognition |
| Hill climbing / landscape / 4-queens h | 4 / 9 | June 2022、May 2023、May 2024 |
| **Chapter 6 逻辑（跳过）** | 7 / 9 | 翻译 proposition、truth table、tautology、entailment |

旧卷的 **agent 题**比较多，新卷可能也会考：
- 画 **agent 方框图**
- **simple reflex agent** 怎么运作
- **table-driven vs simple reflex**、**goal-based vs utility-based** 哪个比较好
- **learning agent 4 components**
- **rational vs omniscient**

---

## 4. 考前的优先顺序

| 优先 | 练什么 | 为什么 |
|---|---|---|
| 1 | **Landscape 画图**（objective + cost） | 每份都有，14 分左右，背熟就能拿满分 |
| 2 | **Ch1 背诵**：4 approaches、Turing Test、6 capabilities、4 criteria、PEAS、6 properties | 每份都有，只要背 |
| 3 | **BFS + DFS + A\* 三棵树** + 6 components | 每份都有，分数最多，要练手 |
| 4 | **Sudoku formulation + AC-3** | 4 / 5 份，格式固定 |
| 5 | **Minimax + alpha-beta 双方向** | 3 / 5 份，要练手 |
| 6 | **EA 5 phases + stopping criteria（套情境）** | 3 / 5 份，只要背 + 套情境 |
| 7 | Hill climbing 细节：greedy、优缺点、卡住原因、variants、local vs classical | 常在 landscape 题附近出现 |
| — | Chapter 6 逻辑 | 新课号 0 次，跳过 |

---

## 5. 建议做题顺序

1. **May 2026**（最新）
2. **Jan 2026**
3. **Oct 2025**
4. **Jul 2025**
5. **Jun 2025**
6. 还有时间：旧卷的 game tree、CSP 和 agent 题
