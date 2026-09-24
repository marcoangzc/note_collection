# AMCS2104 — 考前总结（Lecture 关键字 + 中文意思）

> **来源**：只根据 lecture slides（Chapter 1A–5C）和 Tutorial 1–12 的题目整理。英文关键字都是 **slide 原文**，考试照写。
> 每节有 3 样东西：
> - **关键字**：slide 的英文，考试要写出来
> - **意思**：用中文解释
> - **比喻 / 口诀**：帮你记住
>
> 📝 = Tutorial 问过的题型。⚠️ = slide 写错或容易写错的地方。
> 顺序：**Ch3 → Ch4 → Ch5 → Ch1 → Ch2**（照你的复习顺序）。

---

## 0. 答题方法

- **"Briefly describe / explain"**：slide 关键字 + 一句中文意思翻成英文 + 一个例子（最好用题目的情境）
- **"Differentiate / Compare"**：用表格，每一点**两边都写**
- **"Justify"**：先写结论（Yes / No / 哪一个），再写理由
- **算题 / 画树**：一步一步写，每一步写出 frontier、值、为什么这样选

---

# Chapter 3 — Local Search and Optimization Problems

## 3.1 Optimization problem 与 Local search

| 关键字（slide） | 意思 |
|---|---|
| **Classical search** | Ch2 的方法："Explore search spaces **systematically**"、"Keep track of expanded and unexpanded nodes"；solution = "**A fixed sequence of actions**" |
| Issue | "The **path to the goal is irrelevant** in many problems"（很多问题只要答案，不在乎怎么走到） |
| **Optimization problem** | "the problem of **finding the best solution from all feasible solutions**" |
| 怎么缩小范围 | "narrow the set of **candidate solutions** down to a subset of the **feasible solutions**, according to an **objective function**" |
| **Candidate solutions** | "a set of possible solutions of a given problem"（所有可能的答案） |
| 例子 | **8-Queen Problem、Traveling-Salesman Problem、Course Scheduling Problem、Cutting Stock Problem** |
| Optimization problem 的 solution | "**The goal state itself**" |

**Local search**（比喻：**蒙着眼睛爬山**，只摸得到脚边）：
- "searching from a start state to **neighboring states**"
- "The decision is made solely based on the information about the **neighbourhood of the current state**"
- "The paths followed by the search or reached states are **not retained**"（不记路）
- "The search is **not systematic**"
- **Strengths**："Have **very low memory consumption**"；"Often can find reasonable solutions in **large or infinite state spaces**"

📝 Tutorial 7：UCSP 是不是 optimization problem？（是：要从所有可行的课表里找最好的，而且路径不重要）；用 classical 还是 local search？（local：只要最后的课表，而且 state space 很大）

## 3.2 State Space Landscape ⭐

**定义**："A state-space landscape is a **graphical representation of different states** of a problem reachable by a search algorithm"

| 关键字 | slide 定义 | 意思 |
|---|---|---|
| **Current state** | "The state an agent is currently present" | 你现在站的点 |
| **Global maximum** | "The best possible state with the **highest value of objective function**" | 全图最高的山顶 |
| **Local maximum** | "A state which is **better than its neighbor states, but not the best one**" | 小山顶 |
| **Flat local maximum** | "**All neighboring states** of the current state have the **same value**" | 平的山顶 |
| **Shoulder** | "A **plateau** whose edge is **stretching upwards**" | 半山腰的平台，走过去还能往上 |

- Y 轴 = **elevation**："defined by the value of an **objective function**"；X 轴 = **state space**
- Elevation = **Objective function** → 找 **global maximum**，叫 **hill climbing**
- Elevation = **Heuristic cost function** → 找 **global minimum**，叫 **gradient descent**（图上下颠倒，maximum 全改成 minimum）

```
Objective function
   ^            global maximum
   |                /\
   |               /  \        local maximum
   |              /    \          /\       flat local maximum
   |   shoulder  /      \        /  \      ______
   |     ______ /        \  *   /    \    /      \
   |    /                 \/ current  \  /        \
   |   /                     state     \/
   +-------------------------------------------------> State space
```

## 3.3 Hill Climbing Search ⭐

| 关键字 | 意思 |
|---|---|
| "Keeps track of **one current state**" | 只记住自己站的位置 |
| "Moves to the neighboring state with the **highest value**" | 往最高的邻居走一步 |
| "Terminates when it reaches a **'peak'** where no neighbor has a higher value" | 四周都比你低就停 |
| "does **not look ahead** beyond the immediate neighbors" | 不往前多看 |
| **steepest-ascent version** | 基本版；node 有两部分："**state and value**" |

**Pros**（slide 原文）：
- "**A greedy approach**"，也叫 "**greedy local search**"："Only consider good neighboring state **without thinking ahead**"
- "**A quick approach** with acceptable performance"：rapid progress、"An efficient solution to improve a bad state"
- "Requires **low memory consumption**"、"Requires **low computational power**"

**Limitation**："often gets **stuck at a sub-optimal solution**"（口诀：**小山顶、窄山脊、平地**）

| 关键字 | slide 定义 | 比喻 |
|---|---|---|
| **Local maxima** | "A peak that is higher than each of its neighboring states but **lower than the global maximum**"；"it will be stuck with nowhere else to go" | 爬到小山顶以为到了 |
| **Ridges** | "A **sequence of local maxima** that is very difficult for greedy algorithms to navigate"；"all the available actions **point downhill**" | 斜斜的窄山脊 |
| **Plateaus** | "A **flat area**"：flat local maximum 或 shoulder；"Search can get **lost wandering** on the plateau" | 一片平地，不知往哪走 |

**8-queens 数据**：8⁸ ≈ 17 million states；**Stuck 86%**（平均 3 steps）/ **Success 14%**（平均 4 steps）

**Solutions**（📝 Tutorial 7 Q1a(iii)：写 2 个）：
1. **Sideways move**："in the hope that the plateau is really a **shoulder**"；限制 "**100 consecutive sideways moves**" → **Success 94%**（21 steps）/ Stuck 6%（64 steps）
2. **Backtracking**："Maintain a list of **promising visited states**"；走到不好的 state 就 "backtrack to the previous configuration and explore a new path"

**3 个 variants**

| 关键字 | slide 定义 | 意思 |
|---|---|---|
| **Stochastic hill climbing** | "Chooses **at random** from among the **uphill moves**"；概率 "vary with the **steepness**" | 上坡的路里随机挑，越陡越容易被挑 |
| **First-choice hill climbing** | "Chooses the **first better state** from randomly generated neighbors"；"good strategy when a state has **many successors**" | 随机看邻居，第一个比较好的就走 |
| **Random-restart hill climbing** | "series of hill-climbing search from **randomly generated initial states** until a goal is found"；"global maxima will **eventually be found**" | 失败就换个随机起点重来 |

**Insight**：hill climbing "never making **downhill** moves" → **efficient 但 incomplete**；purely **random walk** → "**complete but extremely inefficient**"

## 3.4 8-Queens（算题）

| 关键字 | 意思 |
|---|---|
| **complete-state formulation** | "Each state has 8 queens on the board, **one per column**" |
| Successors | 把一个 queen 移到**同一 column** 的其他格：**8 × 7 = 56** |
| **Heuristic cost h** | "the number of **pairs of queens that are attacking each other**, either directly or indirectly" |
| Global minimum | **h = 0** = solution |

📝 Tutorial 7 Q2（4-Queens）：states = **4⁴ = 256**；successors = **4 × 3 = 12**；算 h = 逐对检查**同一行**或**同一对角线**（|行差| = |列差|）
📝 8-puzzle 的 h = 每块 tile 离目标的**横 + 直距离总和**（Manhattan distance）

## 3.5 Simulated Annealing

| 关键字 | 意思 |
|---|---|
| **Annealing** | 金属 "heated to a high temperature"，再 "**slowly cooling it down**" 改善性质 |
| 目的 | "going **uphill and downhill**" → "yields both **efficiency and completeness**" |
| Y 轴 | 用 **cost function**，找 **global minimum**（gradient descent） |
| 怎么选 | "Instead of picking the best move … picks a **random move**" |
| 与 stochastic hill climbing 比 | stochastic HC："A move is made **if the new state is an improvement**"；SA："sometimes **worse states are also accepted**" |

**比喻（slide 原本的乒乓球）**：球滚进 local minimum 就停住 → **摇一摇表面**把它弹出来。"Start by **shaking hard** (high temperature T)"，"then **gradually reduce** the intensity (lower T)"。摇得够用力跳出 local minima，但不能太用力把它弹出 global minimum。

## 3.6 Local Beam Search

| 关键字 | 意思 |
|---|---|
| "keeps track of **k states** (or **beam width**)" | 同时派 k 个登山者 |
| "begins with k **randomly generated** states" | |
| "all the successors of **all k states** are generated" | 把 k 个人的所有邻居放一起 |
| "selects the **k best successors** from the complete list" | 只留最好的 k 个 |

📝 **Random-restart HC vs Local beam search**（Tutorial 7 Q2b(v)）：

| Random-restart hill climbing | Local beam search |
|---|---|
| "Each search process runs **independently**" | "Useful information is **passed among the parallel search threads**" |
| 没希望的也会跑完 | "Unfruitful searches are **abandoned immediately**"；"moves its resources to where the **most progress** is being made" |

缺点："tends to get stuck at local maxima or plateau" → 解决：**stochastic beam search**

## 3.7 Evolutionary Algorithms ⭐

**定义**："a variant of **stochastic beam search** in which successor states are generated by **combining two parent states**"；模仿 "**natural selection**"
**比喻**：让答案"生小孩"，好的答案比较容易被挑去生

**5 phases**（口诀：**生、打分、挑、交换、突变**）

| Phase | slide 关键字 | 8-queens 例子 |
|---|---|---|
| **Initialization** | "a set of **k randomly generated states**, called the **population**"；每个 state = 8 digit string（位置 = column，数字 = row） | 24748552、32752411… |
| **Fitness Function** | "rated by an **objective function, or a fitness function**"；分数 "**normalized to probabilities**"，好的比较容易被选 | **non-attacking pairs**：24, 23, 20, 11；solution = **28** |
| **Selection** | "Two pairs are **selected at random** … in accordance with the probabilities"；每对随机选一个 **crossover point** | |
| **Crossover** | "offspring … created by **crossing over the parent strings** at the crossover point" | 327 + 48552 = 32748552 |
| **Mutation** | "Each location is subject to random mutation with a **small independent probability**" | 随机选一个 queen 移到同 column 的随机格 |

**3 种 crossover**

| 关键字 | 做法 |
|---|---|
| **Single Point Crossover** | 选一个点，之后的部分对调 |
| **Two-Point Crossover** | "specific case of a **N-point Crossover**"；选两个点，中间对调 |
| **Uniform Crossover** | "Each bit is selected **randomly** from one of the corresponding bits of the parent"（**tossing a coin**） |

| 关键字 | 意思 |
|---|---|
| **Schema** | 有用的区块，例 **247\*\*\*\*\***（\* = 未指定）；符合的字串（24713578）叫 **instances** |
| **Convergence** | 后期个体都很像，crossover 步子变小；"every individual in the population is **identical**" → evolution 停止 |
| **Mixing number (ρ)** | 几个 parents 组成一个 offspring；**ρ = 1 → stochastic beam search**；最常见 **ρ = 2** |
| **Mutation rate** | 每个 bit 被翻转的概率 |
| 新 population | offspring + 几个 **top-scoring parents**（保证 fitness "**never decrease**"） |
| Representation | **genetic algorithms** = string；**evolution strategies** = real numbers；**genetic programming** = computer program |

**Termination**（3 个）：
1. "**no improvement** in the population"
2. "a **predefined number** of the reproduction cycle has already been fulfilled"
3. "the fitness function has reached a **predefined value**"

**Insights**：EA 结合了 3 种优点

| 优点 | 来自 | 对应 phase |
|---|---|---|
| "An **uphill tendency**" | hill climbing | **Selection** |
| "**Random exploration**" | stochastic search / simulated annealing | **Mutation** |
| "**Exchange of information** among parallel search threads" | local beam search | **Crossover** |

📝 Tutorial 7 Q3：算 fitness（例 f(X) = (A+B) − (C+D) + (E+F) − (G+H)）、做 3 种 crossover、比较 offspring 和 parents 的 fitness、**没有 mutation 能不能找到 optimal？**（不一定：如果某个 digit 在整个 population 里都没出现过，crossover 永远产生不出来，只有 mutation 能带进新的值）

---

# Chapter 4 — Adversarial Search and Games

## 4.1 Games

| 关键字 | 意思 |
|---|---|
| **Adversarial search problems** | "In a **competitive environment**, agents' goals are **in conflict**" → 就是 **games**（我赢 = 你输） |
| **Move** = action；**Position** = state | |

**5 个 characteristics**（📝 Tutorial 8）

| 关键字 | 意思 |
|---|---|
| **Turn-taking** | 轮流走 |
| **Two-player** | 两个人 |
| **Perfect information** | "a synonym for **fully observable**"，全部看得到 |
| **Deterministic** | 没有运气成分 |
| **Zero-sum** | "What is **good for one player is just as bad for the other**" |

例子：**Tic-Tac-Toe、Chess、Go**

## 4.2 Problem Formulation in Game ⭐

- Players 叫 **MAX** 和 **MIN**；"**MAX moves first**, followed by MIN"
- 为什么叫 MAX / MIN？leaf 上的数字是 "utility value … **from the point of view of MAX**"：高分对 MAX 好、对 MIN 不好 → MAX 要**最大化**，MIN 要**最小化**

**6 个 elements**

| 关键字 | slide 定义 | Tic-Tac-Toe |
|---|---|---|
| **S₀** | "The **initial state**, which specifies how the game is set up at the start" | 9 格全空 |
| **ACTIONS(s)** | "The set of **legal moves** in state s" | MAX 放 X、MIN 放 O 在空格 |
| **RESULT(s, a)** | "The **transition model** … the state resulting from taking action a in state s" | 放入后那格被占 |
| **TO-MOVE(s)** | "The player **whose turn** it is to move" | MAX、MIN 轮流，MAX 先 |
| **IS-TERMINAL(s)** | "A **terminal test** which returns true when the **game is over**" | 连成三个或 9 格填满 |
| **UTILITY(s, p)** | "A utility function (also known as **objective function** or **payoff function**) defines the **final numeric value** to player p" | 1 = MAX 赢、0 = 平手、−1 = MAX 输（chess：1、0、½） |

| 关键字 | 意思 |
|---|---|
| **Game tree** | "a type of search tree that represents **all possible game states** as a result of each move made by the players alternatively" |
| **Complete game tree** | 从开局一路展开到 **terminal states** |
| **Leaf nodes / terminal nodes** | 游戏结束的 nodes |
| **Ply** | "**one move by one player**"；two-ply = MAX 一步 + MIN 一步 |
| **Conditional plan** | MAX 的策略要对 MIN 每一种回应都有对策；输赢两种结果 → **AND-OR search**；多种分数 → **minimax** |

## 4.3 Minimax ⭐

| 关键字 | 意思 |
|---|---|
| **Minimax value** | "the utility (for MAX) of being in that state, **assuming that both players play optimally**"；terminal state 的 minimax value = 它的 utility |
| **Minimax decision** | MAX 选 "leads to the state with the **highest minimax value**" 的那一步 |
| **Minimax algorithm** | "simple **recursive** computation that proceeds all the way down to the leaves"；值 "**backed up** through the tree as the recursion unwinds" |

**做法**：从 leaves 往上，**MAX 层取最大、MIN 层取最小**

```
                 A (MAX) = 3
        ┌─────────────┼─────────────┐
    B (MIN)=3     C (MIN)=2     D (MIN)=2
     /  |  \       /  |  \       /  |  \
    3  12   8     2   4   6    14   5   2
```
MAX 的 best move = 往 B，value **3**

**Analysis**：
- Completeness："A complete **depth-first** search exploration"
- Time：**O(bᵐ)**（exponential，不实际）；chess：b = 35、m = 80 → 35⁸⁰ ≈ **10¹²³**
- Space：**O(bm)**（一次生成所有 actions）或 **O(m)**（一次一个）

📝 **MIN 不走最优会怎样？** MAX 的结果**至少一样好，甚至更好**（minimax value 是 MAX 能保证的最低分）

**Multiplayer**：每个 node 变成 **vector** <v_A, v_B, v_C>；"backed-up value … is the utility vector of the successor state with the **highest value for the player choosing** at n"
例：轮到 C，选 <1, 2, **6**> 还是 <4, 2, **3**>？→ C 选 **<1, 2, 6>**（v_C 比较大）

## 4.4 Alpha-Beta Pruning ⭐

**比喻**：你已经找到一家 RM10 的餐厅。去看第二家，第一道菜就 RM15 → **后面不用看了**，这家一定比较贵。

| 关键字 | 意思 |
|---|---|
| 作用 | "**Returns the same move as minimax** would" 但 "**prunes away branches** that cannot possibly influence the minimax decision" |
| 范围 | "can be applied to trees of **any depth**"；可以剪掉 "**entire subtrees** rather than just leaves" |
| **General principle** | 如果玩家在同一层（m）或更上层（m'）已经有 **better choice**，"Player will **never move to n**" → n 可以剪 |
| **α** | "the best **highest-value** choice found so far along the path for **MAX**"（MAX 的下限，初始 −∞） |
| **β** | "the best **lowest-value** choice found so far along the path for **MIN**"（MIN 的上限，初始 +∞） |
| 条件 | node 只有在 "**α ≤ N ≤ β**" 才有用；"when **α > β** … its remaining successors will be **pruned**" |

**做题规则**：
- **MIN node** 拿到 child 值 v：若 **v ≤ α** → 剪掉剩下的；否则 β = min(β, v)
- **MAX node** 拿到 v：若 **v ≥ β** → 剪掉剩下的；否则 α = max(α, v)
- α、β **由上往下传**；值**由下往上传**

上面那棵树（左到右）：B = 3 → root α = 3 → C 第一个 leaf = 2 ≤ 3 → **剪掉 C 的 4、6** → D：14、5、2 → D = 2 → root = **3**

⚠️ slide 写 "gigantic number can be **reduced by almost half**"，但最佳排序其实是 O(b^(m/2))（指数减半），不是 nodes 减半。

## 4.5 Chapter 4B：Alpha-beta 的限制与改进

**Move ordering**："effectiveness … is **highly dependent on the order** in which the states are examined"

| 顺序 | Nodes examined |
|---|---|
| Depth-first move（最差） | **O(bᵐ)** |
| **The best move**（也叫 **killer move**） | **O(b^(m/2))** → effective branching factor 变 **√b**（chess 35 → 约 6） |
| **Random move** | **O(b^(3m/4))** |

- **Dynamic move-ordering**：先试以前找到的最好走法；实作 = **iterative deepening search**（先搜 1 ply 记下 best path，再深一层用它排序）
- 📝 Tutorial 9：同一棵树**左到右**和**右到左**各做一次，比较：**root 的 minimax value 一样**，但剪掉的 nodes 不同

| 关键字 | 意思 |
|---|---|
| **Transpositions** | "Different permutations of a move sequence ends up in the **same position**"（[w1, b1, w2, b2] 和 [w2, b2, w1, b1] 都到 s） |
| **Transposition table** | "a **cache of previously seen positions**"；chess 里 "**double the reachable search depth**"；缺点：占内存 |

**Type A vs Type B**（Claude Shannon，1950）

| Type A Strategy | Type B Strategy |
|---|---|
| "**wide but shallow**" | "**deep but narrow**" |
| "Considers **all possible moves** to a certain depth" | "**Ignores moves that look bad**" |
| 用 **heuristic evaluation function** 估计 | "Follows promising lines **as far as possible**" |

**Heuristic Alpha-Beta Tree Search**（改两个地方）：
1. **UTILITY → EVAL(s, p)**：heuristic evaluation function，"estimates the position's utility"
2. **IS-TERMINAL → IS-CUTOFF(s, d)**：**cutoff test**

**好的 evaluation function**：
- **Time**："must not take too long"
- **Performance**："**strongly correlated** with the actual chances of winning"

| Approach | 做法 | 问题 |
|---|---|---|
| **#1 Categories** | 按 features 分类，算 **expected value**：(0.82 × 1) + (0.02 × 0) + (0.16 × ½) = **0.90** | "requires **too many categories**"、"**too much experience**" |
| **#2 Weighted linear function** | EVAL = w₁f₁ + w₂f₂ + … + wₙfₙ；pawn **1**、knight/bishop **3**、rook **5**、queen **9** | 假设每个 feature "**independent**"（其实一对 bishops > 两倍单个）→ 用 **non-linear**；weights 来自人类经验或 **machine learning** |

**Cut-off point**（📝 Tutorial 9 Q2c：哪个比较好？）：
- **Fixed depth limit**：depth > d 就 cutoff
- **Iterative deepening**：时间到就回传 "**deepest completed search**" 的走法 → **较好**（永远有答案、用满时间）

**Cut-off search 的 3 个问题**（📝 Tutorial 9 Q2c(ii)）

| 问题 | 意思 | 解决 |
|---|---|---|
| **Approximation errors** | EVAL 只是估计，可能不准 | 更好的 features / weights |
| **Non-quiescent positions** | 局面**马上会剧烈变化**（例：Black 多一个 knight 两个 pawns，但 White 下一步吃掉 Black 的 queen） | **Quiescence search**：继续搜到 **quiescent** 才用 EVAL |
| **Horizon effect** | 对手有 "**serious and unavoidable damage**"，但程序用 "**delaying tactics**" 把它推到搜索深度外 | **Singular extensions**："clearly better" 的走法到了深度上限还继续考虑 |

**Forward pruning**（Type B）："cuts away moves that **appear to be poor**"；省时间但 "**at the risk of making an error**"
- **Beam search**：每层只看 "n best moves" → "too risky"
- **PROBCUT**（probabilistic cut，**Michael Buro 1995**）：用 "**statistics gained from prior experience**" 降低剪掉最佳走法的机会；Othello 胜 **64%**（即使对方有两倍时间）；Othello 和 Shogi 都有效

📝 **Alpha-beta pruning vs forward pruning**：alpha-beta 只剪**一定不影响结果**的分支（结果跟 minimax 一样）；forward pruning 剪**看起来差**的走法（更快，但可能剪掉最好的）

**Search vs Lookup**：
- **Opening**（选择少）→ **table lookup**（"Humans expertise + the winning statistics from a database"）
- 约 **10–15 moves** 后（选择多）→ **search**
- **Ending**（选择少）→ table lookup

**Improved minimax search**：
- + cutoff test + quiescence search → **5 ply** → **Average level**（普通人类 6–8 ply）
- + alpha-beta + large transposition table → **14 ply** → **Expert level**
- + tuned evaluation function + endgame DB → **≥ 30 ply** → **Grandmaster level**

📝 **Tutorial 9 Q2：6 种技术**

| Label | 技术 | 一句话 |
|---|---|---|
| A | **Minimax** | 假设双方最优，算出最佳走法 |
| B | **Alpha-beta pruning** | 剪掉不影响结果的分支 |
| C | **Cut-off search** | 搜到某深度就停 |
| D | **Heuristic function** | 在 cutoff 的地方估计分数 |
| E | **Transposition table** | 记住看过的局面，不重复算 |
| F | **Lookup table** | 开局、残局直接查表 |

组合：A+B 结果不变但更快；+C+D 能在时间内决定，但有 3 个 cut-off 问题；+E 同样时间搜更深；+F 开局残局几乎不用搜，时间留给中局

---

# Chapter 5 — Constraint Satisfaction Problems (CSP)

## 5.1 CSP Definition ⭐

| | Search problems（Ch2–4） | CSP |
|---|---|---|
| State | **Atomic representation** | **Factored representation**："Each state has a **set of variables** holding different values" |
| 解决条件 | "a **goal state** is met (via a **goal test**)" | "**each variable has a value that satisfies all the constraints**" |
| Heuristic | **domain-specific**（straight-line distance、Manhattan distance） | "**general-purpose heuristics**" |

CSP 的好处："**eliminate large portions of the search space all at once** by identifying variable/value combinations that violate the constraints"

**3 个 components：X, D, C**

| 关键字 | slide 定义 |
|---|---|
| **X** | "a set of **variables**, {X₁, …, Xₙ}" |
| **D** | "a set of **domains**, one for each variable"；domain = "a set of **allowable values**"；可以大小不同 |
| **C** | "a set of **constraints** … that specify **allowable combinations of values**" |

每个 constraint = **<scope, rel>**：**scope** = 参与的 variables；**rel** = 它们能取的值
两种写法（📝 Tutorial 10 Q1）：
- **Abstract relation**：〈(X₁, X₂), X₁ > X₂〉
- **Explicit list（enumeration）**：〈(X₁, X₂), {(3,1), (3,2), (2,1)}〉

**Assignments**（⭐ 常考定义）

| 关键字 | slide 定义 |
|---|---|
| **Consistent / legal assignment** | "does **not violate any constraints**" |
| **Complete assignment** | "**every** variable is assigned" |
| **Partial assignment** | "assigns values to only **some** of the variables" |
| **Partial solution** | "A partial assignment that is **consistent**" |
| **Solution** | "a **consistent and complete** assignment" |

## 5.2 CSP Problem Formulation ⭐（老师的重点）

**格式**：写 **Variables、Domains、Constraints** 三样

**① Map colouring（Australia）**
- X = {WA, NT, Q, NSW, V, SA, T}
- Dᵢ = {red, green, blue}
- C = {SA≠WA, SA≠NT, SA≠Q, SA≠NSW, SA≠V, WA≠NT, NT≠Q, Q≠NSW, NSW≠V}

**② Timetable（📝 Tutorial 10 Q2、Tutorial 11、12）**
- Variables：每一门课一个，C1…C5（值 = 由谁教）
- Domains：能教那门课的讲师（先做 unary constraint）
- Constraints：**时间重叠的课不能同一位讲师**，例 C1 ≠ C2
- 画 **constraint graph**：有 constraint 的两门课连一条线

例（Tutorial 10 Table 1 + 2）：C1 {A, C}、C2 {B, C}、C3 {A, B}、C4 {A, B, C}、C5 {B}；C = {C1≠C2, C2≠C3, C3≠C4, C3≠C5, C4≠C5}

**③ Sudoku（9×9）**
- Variables：**81** 个，A1 … A9（第一行）… I1 … I9（最后一行）
- Domains：空格 {1, …, 9}；已填的 = 只有那个值
- Constraints：**27 个 Alldiff**（9 rows + 9 columns + 9 boxes）
- 行、列、box 都叫 **unit**
- （同样道理：4×4 → 16 variables、12 Alldiff；6×6 → 36 variables、18 Alldiff）

**④ Queen Problem（📝 Tutorial 10 Q3：4-Queens）**
- Variables：Q₁…Q₄（Qᵢ = 第 i 列的 queen 在哪一行）
- Domains：{1, 2, 3, 4}
- Constraints：每对 i < j：**Qᵢ ≠ Qⱼ**（不同行）且 **|Qᵢ − Qⱼ| ≠ j − i**（不同对角线）

**⑤ Job Shop Scheduling**
- Variables：每个 task 的**开始时间**（AxleF、WheelRF、NutsRF、CapRF、Inspect…）
- **Precedence constraint**：T₁ + d₁ ≤ T₂（例 AxleF + 10 ≤ WheelRF）
- **Disjunctive constraint**：(AxleF + 10 ≤ AxleB) **or** (AxleB + 10 ≤ AxleF)
- 30 分钟完成 → Dᵢ = {1, 2, …, 27}

**Constraint graph**："nodes … correspond to **variables**"；"An **edge** connects any two variables that participate in a constraint"

**为什么要 formulate 成 CSP？** "**easy to formulate**"、"CSP solvers are **fast and efficient**"
例：选了 SA = blue → 5 个邻居不能是 blue → 3⁵ = **243** 变 2⁵ = **32**（**87% reduction**）

## 5.3 Variations

**Domains**：discrete finite（map coloring、scheduling、8-queens）；discrete **infinite**（integers、strings → 要用 "abstract constraint language"）；**continuous**（精准时间的实验）

| Constraint | 意思 | 例子 |
|---|---|---|
| **Unary** | 限制**一个** variable | 〈(SA), SA ≠ green〉 |
| **Binary** | 关联**两个** variables | SA ≠ NSW |
| **Ternary / higher-order** | 三个或以上 | Between(X, Y, Z) |
| **Global** | "**arbitrary number** of variables"（不一定是全部） | **Alldiff**：全部都要不同 |

- **Binary CSP**：只有 unary 和 binary constraints
- **Binarization of constraints**：任何 finite-domain constraint 都能转成 binary；做法 **dual graph transformation**（每个 constraint 变成一个 variable；共用 variables 的两个 constraints 之间加一个 binary constraint）
- **为什么还是用 Alldiff？** "**easier and less error-prone** to write"；可以设计 "**special-purpose inference algorithms**"

| 关键字 | 意思 | 例子 |
|---|---|---|
| **Absolute constraint** | 违反 → "**rules out** a potential solution" | 教授不能同时教两门课 |
| **Preference constraint** | 违反 → 还是 solution，但 "**non-optimal**" | Prof. R 喜欢早上 |
| **COP**（constraint optimization problem） | 把 preference 写成 **cost**，用 optimization search 解 | 下午 2 分、早上 1 分 |

## 5.4 Constraint Propagation

**意思**：用 constraints **减少** variable 能取的值 → 连带减少别的 variable → 下次要选的更少。可以在 search **之前**做（pre-processing，有时直接解完），也可以和 search **交替**做。

**Local consistency** 的 3 种：

| 关键字 | slide 定义 | 例子 |
|---|---|---|
| **Node consistency** | "all the values in the variable's domain satisfy the variable's **unary constraints**" | SA 讨厌 green → {red, blue} |
| **Arc consistency** | Xᵢ 对 Xⱼ arc-consistent："for **every** value in Dᵢ there is **some** value (the **support**) in Dⱼ" | Y = X²，domain 0–10 → **D_X = {0,1,2,3}**、**D_Y = {0,1,4,9}** |
| **Path consistency** | "looking at **triples** of variables"；{Xᵢ, Xⱼ} 的每个 consistent assignment，Xₘ 都要有值同时满足两边 | 只有 2 色的 WA、SA、NT |

## 5.5 AC-3 ⭐

**做法**：
1. Queue 一开始放**所有 arcs**；"Each binary constraint becomes **two arcs, one in each direction**"
2. Pop 一个 (Xᵢ, Xⱼ)，让 **Xᵢ** 对 Xⱼ arc-consistent（删掉 Xᵢ 里找不到 support 的值）
3. Dᵢ **没变** → 看下一个 arc
4. Dᵢ **变小** → 把所有 **(Xₖ, Xᵢ)**（Xₖ 是 Xᵢ 的邻居）加回 queue
5. Dᵢ **变空** → "whole CSP has **no consistent solution**" → 立刻 return failure

结果："still **equivalent** to the original CSP"（solutions 一样），但 domains 更小 → search 更快
Complexity：**O(cd³)**（c = arcs，d = domain 大小）

**Arc consistency 的限制**：有时找到 solution（每个 domain 剩 1）；有时发现无解（domain 变 0）；**有时推不出来** → 例：只有 {red, blue} 的 WA、SA、NT 都 arc-consistent，但**无解** → 要用 **path consistency**
📝 Tutorial 11："AC-3 做完 arc-consistent" 和 "这个 CSP 没有解" **可以同时成立**（3 门课互相重叠、只有 2 位讲师）→ 用 **Alldiff** 就能发现：3 个 variables > 2 个值

**Sudoku 用 AC-3 的答题模板**（📝 Tutorial 11 Q2：I4、A4、E4）：
> **Variable X** — initial domain {1..9}
> - Row constraint: remove {…} → {…}
> - Column constraint: remove {…} → {…}
> - Box constraint: remove {…} → {…}
> - Therefore X = v

Slide 例子：**E6** → box 删 {1, 2, 7, 8}、column 删 {2, 3, 5, 6, 8, 9} → **{4}**；**I6** → **{7}**；**A6** → column 已有 8 个值 → **{1}**

**Global / Resource / Bound constraints**

| 关键字 | 怎么发现 inconsistency |
|---|---|
| **Alldiff** | m 个 variables、只剩 n 个不同的值，**m > n** → 无法满足 |
| **Resource (Atmost)** | Atmost(10, T₁…T₄)：domain {3,4,5,6} → 最小值总和 12 > 10 → 不行；domain {2..6} → 删掉 5、6 |
| **Bound constraint** | 用上下界：F1 [0, 165]、F2 [0, 385]、F1 + F2 = 420 → **F1 [35, 165]、F2 [255, 385]** |

## 5.6 Backtracking Search

**两种 search**：**Backtracking search** → "work on **partial assignments**"；**Local search** → "work on **complete assignments**"

**为什么不用普通 DLS？** n 个 variables、domain d → "Total number of tree nodes: **n! × dⁿ**"
**Commutative**：赋值的顺序不影响结果（{WA=Red, NT=Green} = {NT=Green, WA=Red}）→ "consider only a **single variable at each node**" → leaves 降到 **dⁿ**

| 关键字 | 意思 |
|---|---|
| **Backtracking search** | "a variant of **depth-first search** that chooses values for **one variable at a time** and **backtracks when a variable has no legal values left**" |
| 特点 | "keeps only a **single representation of a state**"；"doesn't rely on **domain-specific heuristic**" |
| 与 DFS 的关系 | "a combination of **depth-first search and pruning**" |

**4 个改进的地方**（📝 Tutorial 12 Q1a(iv)）：
1. **SELECT-UNASSIGNED-VARIABLE()**：下一个选哪个 variable？
2. **ORDER-DOMAIN-VALUES()**：值用什么顺序试？
3. **INFERENCE()**：每一步做什么推论？
4. **BACKTRACK()**：能不能退不止一步？

**Variable ordering**

| 关键字 | 意思 |
|---|---|
| Static / random ordering | 不 optimal |
| **MRV (minimum-remaining-values)** | 选**合法值最少**的 variable；也叫 "**most constrained variable**" 或 "**fail-first**"；比 random 好 "by a factor of **1,000** or more"；第一步没用（一开始大家一样多） |
| **Degree heuristic** | 选 "involved in the **largest number of constraints on other unassigned variables**"；Australia：**SA = 5**（最高）、**T = 0** |
| 关系 | **MRV = "A powerful guide"**；**Degree = "A tie-breaker"** |

**Value ordering**

| 关键字 | 意思 |
|---|---|
| **Least-constraining-value (LCV)** | 选 "rules out the **fewest choices** for the neighboring variables"；"leave the **maximum flexibility**" |
| 例子 | {WA=Red, NT=Green}：Q = Blue → SA = {}（坏）；Q = Red → SA = {Blue} → **选 Red** |

⭐ **为什么 variable 要 fail-first、value 要 fail-last？**
- Variable：**每个 variable 最后都要赋值**，先处理最容易失败的，失败马上发现，少走冤枉路
- Value：只要找**一个** solution，先试最可能成功的值；先试会失败的值没意义

## 5.7 Interleaving search and inference（5C）

| 关键字 | 意思 |
|---|---|
| **Forward checking** | 每次 X 被赋值，"For each **unassigned variable Y** connected to X … delete from Y's domain any value that is inconsistent" |
| 例子 | WA=Red → NT、SA 变 {G, B}；Q=Green → NT、SA 只剩 {B}；V=Blue → **SA 变空** → 立刻 backtrack |
| **Forward checking + MRV** | WA=Red 后 NT、SA 被删最多 → 下一步先处理它们 |
| 限制 | "only makes the **current variable** arc-consistent"；WA=Red、Q=Green 后 NT、SA 都 = {Blue}，但它们相邻不能同色 → **没发现** |
| **MAC (Maintaining Arc Consistency)** | 赋值 Xᵢ 后，queue 从 **(Xⱼ, Xᵢ)** 开始（Xⱼ = 未赋值的邻居），"**recursively propagating** constraints" → 能发现 forward checking 漏掉的 |

📝 Tutorial 12 Q1a(vi)：范围 **backtracking**（只检查已赋值的）< **forward checking**（当前 variable 的邻居）< **FC + MAC**（一路传下去）

**Intelligent Backtracking**

| 关键字 | 意思 |
|---|---|
| **Chronological backtracking** | 失败就回到 "**the most recent decision point**" |
| 问题 | 顺序 {Q, NSW, V, T, SA…}，SA 没值 → 回到 T，但 **T 跟 SA 根本不相邻**，没用 |
| **Conflict set** | "a set of assignments that are **in conflict with some values** for the current variable"；SA 的 = {Q=Red, NSW=Green, V=Blue} |
| **Backjumping** | 回到 conflict set 里 "**the most recent assignment**" → 跳过 T，改 **V** |
| 注意 | "a simple backjumping is **redundant in a forward-checking search**"（forward checking 早就挡掉了） |
| **Conflict-directed backjumping** | 找**真正的原因**：跳回去的 variable "**absorbs**" 失败 variable 的 conflict set；例 {WA=Red, NSW=Red} 是真正问题 → 跳回 **NSW** |
| **Constraint learning** | 找出造成问题的 "**minimum set of variables**" + 它们的值 = **no-good**，记录下来不再犯 |

📝 Tutorial 12 Q2：给定顺序 WA, V, NT, Q, NSW, T, SA，写出每个 variable 的 **conflict set**，再说 backjumping 会跳到哪里

## 5.8 Local Search for CSPs

| 关键字 | 意思 |
|---|---|
| **Complete-state formulation** | "Each state assigns a value to **every variable**"；每次改**一个** variable |
| Backtracking 用的 | **Incremental formulation**（partial） |
| **Min-conflicts heuristic** | 选 "the value that results in the **minimum number of conflicts**" |
| 效果 | **million-queens** 平均 **50 steps** 解完 |
| 问题 | landscape 有很多 **plateau** |
| 解决 | **Plateau search (sideways moves)**、**simulated annealing** |

---

# Chapter 1 — Introduction & Intelligent Agents

## 1.1 四种 AI approaches（1A）⭐

**2 个 dimensions**："**Human performance vs. rationality**"；"Thought processes and reasoning (**thinking**) vs. behavior (**action**)"

| | Human Performance | Rationality |
|---|---|---|
| **Thinking** | **Thinking Humanly** = **Cognitive Modelling** approach | **Thinking Rationally** = **"Laws of Thought"** approach |
| **Acting** | **Acting Humanly** = **Turing Test** approach | **Acting Rationally** = **Rational Agent** approach |

**Turing Test**：
- "Proposed by **Alan Turing in 1950**"；"Also called the **imitation game**"
- "Test a computer's ability to exhibit **intelligent behavior equivalent to human**"
- "a **computer**, a **human respondent** and a **human interrogator**"；"Direct physical interaction … is **avoided**"
- 通过：interrogator "after posing some **written questions**, **cannot tell** whether the written responses come from a person or a computer"

**Total Turing Test**：**Stevan Harnad, 1991**；加 2 个 requirement："**Perceptual ability**"、"**Object manipulation ability**"

**6 capabilities**（口诀：**说、记、想、学、看、动**）

| 关键字 | 用途（slide） |
|---|---|
| **Natural language processing** | "To communicate in human languages" |
| **Knowledge representation** | "To store what it knows or hears" |
| **Automated reasoning** | "To do reasoning and to draw new conclusions" |
| **Machine learning** | "To adapt to new circumstances and handle uncertainties" |
| **Computer vision** | "To derive information from its percepts" |
| **Robotics** | "To manipulate objects and move about" |

**Cognitive Modelling**：3 ways
- **Introspection**："Try to catch our own thoughts as they go by"
- **Psychological experiments**："Observe a person in action"
- **Brain imaging**："Observe the brain in action"
- **Cognitive science**："An **interdisciplinary** field"，结合 AI 的 computer models 和心理学的实验，建立 "**precise and testable theories** of the human mind"

**"Laws of Thought"**：
- "Rules that guide and underlie everyone's thinking"；"Attempts to codify the **'right thinking'**, or **irrefutable reasoning** processes"；"Initiated the field called **logic**"
- **Syllogism**："deductive reasoning consisting of **two premises, and a conclusion**"
- 例：Sam is Michelle's parent. Sam is a woman. → Sam is Michelle's mother.
- 📝 Challenge：现实的知识很难用严格的逻辑写出来

**Rational Agent**："A **rational agent** acts so as to achieve the **best outcome** or, when there is uncertainty, the **best expected outcome**"

**为什么用 Rational Agent approach？**
- vs Thinking Rationally："correct inference is **just one of several possible mechanisms** for achieving rationality"（有时没有能证明对的事可做，但还是要行动；reflex action 不用推理）
- vs Humanly："challenging to **model them mathematically**"；rationality "can be **well defined mathematically**, making it (provably) **achievable**"

## 1.2 Agents（1B）

| 关键字 | 意思 |
|---|---|
| **Agent** | "anything that can be viewed as **perceiving its environment through sensors** and **acting upon that environment through actuators**" |
| **Perception** | "A **passive** interaction … gains information **without changing it**" |
| **Action** | "An **active** interaction where the environment **is changed**" |
| **Percept** | "the agent's perceptual inputs at any **given instant**" |
| **Percept sequence** | "the **complete history** of everything the agent has ever perceived" |
| **Agent function** | "maps any given **percept sequence to an action**"；"an **abstract mathematical description**" |
| **Agent program** | "a **concrete implementation**, running within some physical system" |

Agent 例子：Human（eyes, ears / hands, legs）；Robotic（cameras, infrared range finders / motors）；Software（keyboard, mouse / display screen, speaker）

## 1.3 Rationality ⭐

| 关键字 | 意思 |
|---|---|
| **Consequentialism** | "**outcomes are all that matter**"：结果好就是对的 |
| **Performance measure** | 机器没有自己的想法，所以要 performance measure："evaluates any given **sequence of environment states**" |
| **Rule of thumb** | "Design performance measures according to **what one actually wants to be achieved** in the environment, not according to how one thinks the agent should behave" |

**Rationality 取决于 4 件事**（口诀 **P-K-A-P**）：
1. "The **performance measure** that defines the criterion of success"
2. "The agent's **prior knowledge** of the environment"
3. "The **actions** that the agent can perform"
4. "The agent's **percept sequence** to date"

**Rational agent 定义**："For each possible percept sequence, a rational agent should select an action that is **expected to maximize its performance measure**, based on the evidence provided by the **percept sequence** and its **built-in knowledge**"

📝 Vacuum world 例子（Tutorial 1、slide Example #1–#5）：每个例子只改 4 件事里的**一件**，要说出改了哪一件、还 rational 吗

| | Rational agent | Omniscient agent |
|---|---|---|
| 结果 | "Doesn't know the **actual outcome**" | "Knows the actual outcome" |
| 情况 | "Acts under **uncertainty**" | "Acts under **certainty**" |
| 目标 | "**best expected outcome**" | "**best actual outcome**" |

**3 capabilities**：
- **Information gathering**："Perceive the environment **before making any action**"
- **Learning**："compensate for agents' **incomplete or incorrect prior knowledge**"
- **Autonomy**：只靠设计者的 prior knowledge → "**fragile and lack autonomy**"；rational agent 应能 "act based on its **own experience**"

## 1.4 Task Environment（1C）⭐

**PEAS**："the **first step** in designing an agent, must always be to **specify the task environment**"
**P**erformance measure · **E**nvironment · **A**ctuators · **S**ensors

📝 Tutorial 2 写过：Facial recognition、Hearts card game、Chinese chess、Vacuum cleaner。每项写 3–5 个具体东西。

**Slide 的 PEAS 例子表**（1C p4–6，图片页）

| Agent Type | Performance Measure | Environment | Actuators | Sensors |
|---|---|---|---|---|
| **Taxi driver** | Safe, fast, legal, comfortable trip, maximize profits, minimize impact on other road users | Roads, other traffic, police, pedestrians, customers, weather | Steering, accelerator, brake, signal, horn, display, speech | Cameras, radar, speedometer, GPS, engine sensors, accelerometer, microphones, touchscreen |
| **Medical diagnosis system** | Healthy patient, reduced costs | Patient, hospital, staff | Display of questions, tests, diagnoses, treatments | Touchscreen/voice entry of symptoms and findings |
| **Satellite image analysis system** | Correct categorization of objects, terrain | Orbiting satellite, downlink, weather | Display of scene categorization | High-resolution digital camera |
| **Part-picking robot** | Percentage of parts in correct bins | Conveyor belt with parts; bins | Jointed arm and hand | Camera, tactile and joint angle sensors |
| **Refinery controller** | Purity, yield, safety | Refinery, raw materials, operators | Valves, pumps, heaters, stirrers, displays | Temperature, pressure, flow, chemical sensors |
| **Interactive English tutor** | Student's score on test | Set of students, testing agency | Display of exercises, feedback, speech | Keyboard entry, voice |
| **Vacuum cleaner**（slide 留 "?" 给你填） | Cleanliness, energy used, time | Squares / rooms, dirt | Wheels (Left, Right), suction (Suck) | Location sensor, dirt sensor |

**Properties of the environment**

| 关键字 | slide 定义 | 例子 |
|---|---|---|
| **Fully vs Partially observable** | sensors 给 "**complete state** of the environment at each point in time" | Fully：chess、image recognition；Partially：card game、taxi |
| **Single-agent vs Multiagent** | 一个 vs 多个 agent；multiagent 分 **Competitive / Cooperative / Partially both** | Single：crossword、spider card；Multi：chess（competitive）、robot rescue（cooperative）、taxi / soccer（both） |
| **Deterministic vs Non-deterministic** | "next state … **completely determined by the current state and the action**" | Det：chess、tic-tac-toe；Stochastic：soccer、taxi |
| **Episodic vs Sequential** | episodic = "**atomic episodes**"，下一个 "does not depend on the actions taken in previous episodes"；sequential = "current decision could **affect all future decisions**" | Episodic：mail sorting、defect detection；Sequential：chess、taxi |
| **Static vs Dynamic** | static = 思考时环境不变；dynamic = "changes while an agent is **deliberating**"；**semi-dynamic** = 环境不变但 "performance score does" | Static：crossword、chess without clock；Dynamic：taxi、soccer；Semi：**chess with a clock** |
| **Discrete vs Continuous** | "**finite** number of distinct states … percepts and actions" | Discrete：tic-tac-toe、chess；Continuous：basketball、taxi |
| **Known vs Unknown** | 不是环境本身，是 agent 知不知道 "**'laws of physics'** of the environment" | Known：棋的规则 |

**Slide 的 Task Environments 例子表**（1C p29，图片页）

| Task Environment | Observable | Agents | Deterministic | Episodic | Static | Discrete |
|---|---|---|---|---|---|---|
| Crossword puzzle | Fully | Single | Deterministic | Sequential | Static | Discrete |
| Chess with a clock | Fully | Multi | Deterministic | Sequential | Semi | Discrete |
| Poker | Partially | Multi | Stochastic | Sequential | Static | Discrete |
| Backgammon | Fully | Multi | Stochastic | Sequential | Static | Discrete |
| Taxi driving | Partially | Multi | Stochastic | Sequential | Dynamic | Continuous |
| Medical diagnosis | Partially | Single | Stochastic | Sequential | Dynamic | Continuous |
| Image analysis | Fully | Single | Deterministic | Episodic | Semi | Continuous |
| Part-picking robot | Partially | Single | Stochastic | Episodic | Dynamic | Continuous |
| Refinery controller | Partially | Single | Stochastic | Sequential | Dynamic | Continuous |
| English tutor | Partially | Multi | Stochastic | Sequential | Dynamic | Discrete |

容易错：**Backgammon** 看得到全部但掷骰子 → Stochastic；**Image analysis** 是 **Semi**；**Part-picking robot** 是 **Episodic**。

**判断 entity 是不是 agent**：B 的行为是不是在 "maximizing a performance measure whose value also **depends on agent A's behavior**"

## 1.5 Agent Programs（1C）⭐

**Agent = Architecture + Program**

**Table-driven agent**：表格要 Σ|P|ᵗ 个 entries → 问题：表太大存不下、做表太久、**不能学习**

**4 种 agent**（一个比一个聪明）

| 关键字 | 怎么做决定（slide） | 缺点 |
|---|---|---|
| **Simple reflex agent** | "selects actions on the basis of the **current percept**, ignoring the rest of the percept history"；用 **condition–action rule**（if–then） | "Only works well if the environment is **fully observable**"；会 **infinite loop**（randomized 版比较好） |
| **Model-based reflex agent** | "Maintains **internal states**"，用 **sensor model** + **transition model** 记住看不到的部分 | internal state 只是 "**best guesses**"；没有 goal，不知道该选哪个 action |
| **Goal-based agent** | 记住 "**goal information** that describes situations that are desirable"，选能达成 goal 的 action | goal 只有 "**binary distinction**"（达成 / 没达成） |
| **Utility-based agent** | **utility function** 打分 "how **happy** they would make the agent"；选 "**best expected utility**" | 很难做（要建模、算法要有效率） |

- **Utility function** = "**Internal** performance measure"；**Performance measure** = "**External** performance measure"
- Utility-based 比 goal-based 好：**more than one goal**、**conflicting goals**、**uncertain environment** 都能做决定

**Learning agent 的 4 components**

| 关键字 | slide 定义 | 自动驾驶例子 |
|---|---|---|
| **Performance element** | "selecting **external actions**" | 决定踩油门、刹车、转弯 |
| **Learning element** | "making **improvements**" | 按 critic 的回馈修改驾驶方法 |
| **Critic** | "**evaluating** the agent's behavior with respect to a **fixed performance standard**" | 评估有没有保持车道、有没有撞 |
| **Problem generator** | "suggesting actions that will lead to **new experiences**" | 建议试新路线 |

📝 Learning vs non-learning agent（3 点）：会不会从经验学、能不能适应变化、performance 会不会进步

**Representation**（越来越复杂）

| 关键字 | 意思 | 用在 |
|---|---|---|
| **Atomic** | "each state … is **indivisible** without any internal structure" | Search（Ch2–4） |
| **Factored** | "splits up each state into a **fixed set of variables or attributes**" | CSP（Ch5） |
| **Structured** | state 包含 "**objects** … attributes … and **relationships** to other objects" | 卡车倒车被牛挡住 |

---

# Chapter 2 — Solving Problems by Searching

## 2.1 Problem-solving agent（2A）

- "A type of **goal-based agent**"；"Find a **sequence of actions** that achieves its goals"；用 **atomic representation**

**4 phases**：**Goal Formulation → Problem Formulation → Solution Searching → Solution Execution**
⚠️ Tutorial 有同学把第 3 个写成 "Solution Formulation"，slide 是 **Solution Searching**

| Phase | 意思 |
|---|---|
| **Goal formulation** | 第一步；goals "Limit the **objectives**" 和 "Limit the **actions** it needs to consider" |
| **Problem formulation** | "creating an **abstract model** of the relevant part of the world" |
| **Search** | "simulates sequences of actions in its model" 直到找到 **solution** 或确定没有 |
| **Execute** | "executes all actions in the solution, **one at a time**" |

**6 components** ⭐

| 关键字 | slide 定义 | 例子（Arad → Bucharest） |
|---|---|---|
| **States** | "set of possible states … referred to as **state space**" | 所有城市 |
| **Initial state** | "A state that the agent **starts in**" | Arad |
| **Goal state** | "A state that represents or **satisfies the goal**" | Bucharest |
| **Action** | "possible actions available … in a particular state"；**ACTIONS(s)** | ACTIONS(Arad) = {ToSibiu, ToTimisoara, ToZerind} |
| **Transition model** | "A description of **what each action does**"；**RESULT(s, a)** | RESULT(Arad, ToZerind) = Zerind |
| **Cost function** | "The cost of applying an action in state to reach another state"；**ACTION-COST(s, a, s')** | 距离（km）或时间 |

| 关键字 | 意思 |
|---|---|
| **Path** | 一串 actions |
| **Solution** | "a path from the initial state to a goal state" |
| **Optimal solution** | "the **lowest path cost** among all solutions" |
| Action costs are **additive** | path cost = 每个 action cost 加起来 |
| **State space graph** | vertices = states，directed edges = actions |

**Abstraction**："The process of **removing details** from a representation"
- **State abstraction**（在 Arad → "Arad"）、**Action abstraction**（开车去 Sibiu → "ToSibiu"）
- **Valid**："any abstract solution can be **elaborated into a solution** in the more detailed world"
- **Useful**："carrying out each of the actions … is **easier** than the original problem"

**Standardized vs Real-world problems**：
- Standardized："illustrate or exercise" 方法、"concise and exact description"、研究者的 **benchmark**（vacuum world、8-puzzle、8-queens）
- Real-world："solutions **people actually use**"；formulation "**idiosyncratic**"（VLSI layout、TSP、robot navigation、airline travel）
- 数字：vacuum 2 格 = 2 × 2² = **8** states（n 格 = n × 2ⁿ）；8-puzzle = 9!/2 = **181,440**；8-queens：放任何位置 ≈ **1.8 × 10¹⁴**，每 column 一个 = 8⁸ ≈ **1.7 × 10⁷**
- 8-queens 两种 formulation：**Incremental**（从空盘一个一个放）vs **Complete-state**（8 个都在，每次移一个）

## 2.2 Search Tree

| 关键字 | 意思 |
|---|---|
| **Search tree** | Node = state，Edge = action，**root = initial state** |
| **Frontier (open list)** | "a set of **unexpanded nodes**" |
| **Node expansion** | "application of available action(s) onto a node" |
| **Node generation** | 产生 child nodes（successors）并 "**add them to the frontier**" |
| **Reached** | "Any state that has had a **node generated**"（不管有没有 expand） |
| **Explored set (closed list)** | "Nodes that have already been **expanded**" |

📝 **Search tree vs State space**：state space 每个 state "**uniquely represented**"；search tree "**Multiple nodes** correspond to the same state can exist"

**Node 的 4 components**：**n.STATE**、**n.PARENT**、**n.ACTION**、**n.PATH-COST**（沿 PARENT 往回走就能找回整条路）

**3 种 queue**（frontier 用）

| Queue | 先拿出谁 | 用在 |
|---|---|---|
| **FIFO** | 最早放进去的 | **Breadth-first search** |
| **LIFO**（stack） | 最新放进去的 | **Depth-first search** |
| **Priority queue** | evaluation function **f 最小**的 | **Best-first search**（UCS、Greedy、A*） |

操作：**IS-EMPTY(frontier)**、**POP(frontier)**、**ADD(node, frontier)**

| 关键字 | 意思 |
|---|---|
| **Loopy path (cycle)** | "Any path that contains a **repeated state**"；会 "stuck in an infinite loop" |
| **Redundant path** | 用比较差的路到同一个 state（Arad-Zerind-Oradea-Sibiu vs Arad-Sibiu）；loopy path 是 redundant path 的 "**special case**" |
| **Graph search** | 用 **reached** 检查 redundant paths |
| **Tree-like search** | "does not check for redundant paths"（不记 reached） |

## 2.3 COST（2B）

| 关键字 | 问什么 |
|---|---|
| **Completeness** | 有 solution 一定找到？没有时正确报 failure？ |
| **Optimality** | 找到的是 **lowest path cost** 吗？ |
| **Time complexity** | 要多久？ |
| **Space complexity** | 要多少内存？ |

- **b** = "branching factor or **maximum number of successors**"；**d** = "depth of the **shallowest goal** node"；**m** = "**maximum length** (depth) of any path"
- **Big-O**：衡量 "how quickly the code **grows relative to the input**"；用 "**worst case**"
- O(1) constant < O(log n) < O(n) linear < O(n log n) < O(n²) quadratic < O(2ⁿ) exponential < O(n!) factorial
- ⚠️ slide 把 O(n log n) 写成 "Logarithmic time"，其实 O(n log n) 叫 **linearithmic**，logarithmic 是 O(log n)
- BFS 表的结论："**Memory** requirements are a bigger problem … than is the execution time"

## 2.4 Uninformed Search

**Uninformed (blind) search**："no additional information about states"；"Only can differentiate between **goal state and non-goal state**"

| Algorithm | 做法 | Complete? | Optimal? | Time | Space |
|---|---|---|---|---|---|
| **Breadth-first** | 一层一层 expand；**FIFO**；**early goal test** | Yes（b finite） | Yes（costs 都一样） | O(bᵈ) | O(bᵈ) |
| **Uniform-cost**（= **Dijkstra's**） | expand **lowest path cost g(n)**；priority queue；**late goal test** | Yes（costs ≥ ε） | **Yes** | O(b^(1+C*/ε)) | O(b^(1+C*/ε)) |
| **Depth-first** | expand **deepest** node；**LIFO**；通常 tree-like | 有限才 complete；infinite 不行 | **No** | O(bᵐ) | **O(bm)** |
| **Backtracking** | DFS 变体，一次只生成一个 successor | | | | **O(m)** |
| **Depth-limited** | 限深度 **l**；找不到回传 **cutoff** | l < d 不 complete | l > d 不 optimal | O(bˡ) | O(bl) |
| **Iterative deepening** | l 从 0 慢慢加到 d | Yes（有限） | Yes（costs 一样） | O(bᵈ) | **O(bd)** |
| **Bidirectional** | 从 initial 往前 + 从 goal 往回，**两边 frontier 相遇** | Yes | Yes（costs 一样） | O(b^(d/2)) | O(b^(d/2)) |

| 关键字 | 意思 |
|---|---|
| **Early goal test** | node **一生成**就检查是不是 goal（BFS） |
| **Late goal test** | node **被 pop 出来**才检查（UCS、A*） |
| **Diameter** | 任两个城市最多几步（Romania = 9），可以当 DLS 的好 l |
| IDS 的好处 | 结合 DFS（省内存）和 BFS（complete、optimal）；"**preferred** uninformed search method when the search space is **large** and the **depth of the solution is not known**" |
| IDS 浪费吗？ | b = 10、d = 5：IDS = **123,450**、BFS = **111,111** → 多一点点而已 |

## 2.5 Informed Search ⭐

**Informed (heuristic) search**："additional information about states"；"can identify whether one non-goal state is **'more promising'**"；通常更有效率

| 关键字 | 意思 |
|---|---|
| **Best-first search** | 用 **evaluation function f(n)** 选 node；"**lowest f-value** … expanded first" |
| **Heuristic function h(n)** | "**estimated cost** of the cheapest path from the state at node n to a goal state"；例 **straight-line distance (h_SLD)**；不能从 problem description 算出来 |
| **Greedy best-first search** | **f(n) = h(n)**；expand "closest to the goal"；**Not cost-optimal** |
| **A\* search** | **f(n) = g(n) + h(n)**；g = 已走的 cost，h = 估计还要多少；f = "estimated cost of the **cheapest solution through n**" |

| | Uniform-cost | Greedy | A* |
|---|---|---|---|
| Priority queue 按 | **g** | **h** | **g + h** |

**A\* performance**：Complete（b finite、cost ≥ ε）；**Cost-optimal if the heuristic is admissible / consistent**；Time O(bᵈ)；Space exponential

**A\* 的 pruning**：Romania 例子里 Timisoara（f = 447）和 Zerind（f = 449）**从来没被 expand**；"effectiveness of pruning is dependent on the **quality of heuristic**"

| 关键字 | 意思 |
|---|---|
| **Admissible** | "**never overestimates** the cost to reach a goal"；h_SLD 是 admissible，因为 "the shortest path between any two points is a **straight line**" |
| **Consistent**（**monotonicity**） | **h(n) ≤ c(n, a, n') + h(n')**；"slightly **stronger** condition than admissibility"；是 **triangle inequality** |
| Consistent 例子 | h(Arad) = 366 ≤ 140 + h(Sibiu) 253 = 393 ✓ |

**Romania A\* 答案**：Arad → Sibiu → Rimnicu Vilcea → Pitesti → Bucharest = **418**（Greedy 走 Fagaras 得 **450**，不是 optimal）

📝 **画 search tree 的格式**（Tutorial 5、6）：
- 先写假设：**graph / tree search**、**early / late goal test**、**alphabetical order**
- A* 每个 node 写 **g + h = f**，标 expansion 顺序 [1][2][3]…
- 写出每一步的 **frontier** 和 **reached / explored**


---

# Tutorial 图题的答案（我用程序验算过）

> Tutorial 7–13 的答案格在 handbook 里是**空的**，下面是我照 lecture 的方法算出来的答案。Tutorial 1–6 有同学的答案，我对照过，错的地方有标出。

## T7 Q2a：4-Queens（图：queen 在第 1、2、3、2 行）

- States = **4⁴ = 256**；successors = **4 × 3 = 12**
- Initial state (1, 2, 3, 2) 的 h = **5**（c1–c2 对角、c1–c3 对角、c2–c3 对角、c2–c4 同一行、c3–c4 对角）
- Steepest ascent：h **5 → 2 → 1 → 0**，solution = **(3, 1, 4, 2)**，3 步到 global minimum，没有遇到 local minimum 或 plateau

## T7 Q2b：8-puzzle（Manhattan distance）

```
Initial        Goal
 _ 1 3         1 2 3
 4 2 5         4 5 6
 7 8 6         7 8 _
```
- h(initial) = tile 1 差 1 + tile 2 差 1 + tile 5 差 1 + tile 6 差 1 = **4**；h(goal) = **0**
- 第一步只有 2 个动作：blank **Right** → h = **3**；blank **Down** → h = **5** → 选 Right
- 之后：blank Down（h = 2）→ blank Right（h = 1）→ blank Down（h = 0，goal）
- 路径 h：**4 → 3 → 2 → 1 → 0**

## T4 / T5 / T6：地图 A → J

边：A–B 6、A–F 3、B–C 1、B–D 2、C–D 1、C–E 5、D–E 1、E–I 5、E–J 5、F–G 2、F–H 7、G–I 3、H–I 3、I–J 1
h：A 10、B 8、C 5、D 7、E 3、F 6、G 5、H 3、I 1、J 0

| Strategy（题目的设定） | Expansion 顺序 | Solution | Cost | Optimal? |
|---|---|---|---|---|
| **BFS**（graph、early goal test、alphabetical） | A, B, F, C, D, G, H, E（生成 J 就停） | A-B-C-E-J | **17** | ✗ |
| **DFS**（tree、检查 loop、alphabetical） | A, B, C, D, E, I, G, F, H, H, F, G, J | A-B-C-D-E-I-J | **15** | ✗ |
| **UCS**（graph、late goal test） | A0, F3, G5, B6, C7, D8, I8, J9 | A-F-G-I-J | **9** | ✓ |
| **Greedy**（tree、late goal test、检查 loop） | A10, F6, H3, I1, J0 | A-F-H-I-J | **14** | ✗ |
| **A\***（graph、late goal test） | A 0+10, F 3+6, G 5+5, I 8+1, J 9+0 | A-F-G-I-J | **9** | ✓ |

- A\* 和 UCS 答案一样（optimal），但 A\* 只 expand **5** 个 node，UCS 要 **8** 个 → heuristic 的价值
- UCS 里 D 和 I 都是 8，同分时按 alphabetical 先 D

## T8 / T9：Game tree

```
MAX                          A = 6
MIN              B = 6                      C = 3
MAX        D = 6       E = 16         F = 3        G = 8
MIN      H=6  I=3   J=16 K=3 L=4    M=3  N=2     O=1  P=8
leaf     6 8 3 11 9 17 16 7 3 9 4   5 3  2 4    10 1  8 11
```
- 层次：A **MAX**、B/C **MIN**、D–G **MAX**、H–P **MIN**、leaves = terminal
- Minimax：A = **6**，MAX 的 best move = **往 B**

**Alpha-beta，左到右**：剪掉
- I 的 **T(11)、U(9)**：I 看到 3 ≤ α = 6（D 已有 6）
- E 的 **K、L**：E 看到 J = 16 ≥ β = 6（B 已有 6）
- M 的 **AC(3)**：M 看到 5 ≤ α = 6（A 已有 6）
- N 的 **AE(4)**：N 看到 2 ≤ α = 6
- C 的 **G 整棵**：C ≤ 5 ≤ α = 6

**Alpha-beta，右到左**：只剪掉
- O 的 **AF(10)**：O 先看到 1 ≤ α = 8（G 已有 P = 8）
- K 的 **X(7)**：K 先看到 3 ≤ α = 4（E 已有 L = 4）

**结论**：两个方向 root 都是 **6**（结果一样），但左到右剪了很多，右到左只剪 2 个 → "effectiveness … **highly dependent on the order**"

## T11 Q2：Sudoku（AIMA 那一题）

| Variable | Row 删 | Column 删 | Box 删 | 结果 |
|---|---|---|---|---|
| **I4** | {1, 3, 5} | {1, 2, 3, 6, 7, 8} | {1, 2, 3, 6, 9} | **{4}** → I4 = 4 |
| **A4** | {2, 3, 6} | {1, 2, 3, 6, 7, 8} | {2, 3, 5, 6, 8} | {4, 9} → I4 = 4（同 column）删 4 → **A4 = 9** |
| **E4** | {7, 8} | {1, 2, 3, 6, 7, 8} | {1, 2, 7, 8} | {4, 5, 9} → A4 = 9 删 9、E6 = 4（同 row）删 4 → **E4 = 5** |

（Slide 的例子：E6 = 4、I6 = 7、A6 = 1，也是同样做法）

## T12 Q2：Backjumping（Australia 地图）

顺序 WA, V, NT, Q, NSW, T, SA；{WA=Red, V=Blue, NT=Blue, Q=Red, NSW=Green, T=Red}

| Variable | Conflict set（之前已赋值、而且相邻的） |
|---|---|
| WA | { } |
| V | { } |
| NT | {WA} |
| Q | {NT} |
| NSW | {Q, V} |
| T | { } |
| SA | **{WA, V, NT, Q, NSW}** |

- SA 没有合法值（Red、Blue、Green 都被邻居用了）
- **Chronological backtracking** 回到最近的 **T** → 没用，T 跟 SA **不相邻**，改 T 也救不了 SA
- **Backjumping** 回到 SA 的 conflict set 里最近的 **NSW** → 改 NSW 才可能让 SA 有颜色

## T10 Q2 / T11 Q1：Timetable

- **T10 Table 2**：C1 {A, C}、C2 {B, C}、C3 {A, B}、C4 {A, B, C}、C5 {B}；C = {C1≠C2, C2≠C3, C3≠C4, C3≠C5, C4≠C5}（2 和 4 在 11am 只是碰到，不重叠）
  - AC-3 后：C1 {A, C}、C2 {B, C}、**C3 {A}**、**C4 {C}**、**C5 {B}**
  - 一个 solution：C1 = Alex、C2 = Billy、C3 = Alex、C4 = Carolyn、C5 = Billy
- **T10 Table 3**：C1 {C}、C2 {B, C}、C3 {A, B}、C4 {B, C}、C5 {A, B} → AC-3 直接解完：**C1 = C、C2 = B、C3 = A、C4 = C、C5 = B**
- **T11 Q1**：3 门课**两两重叠**，只有 Alex、Billy 两人 → AC-3 做完每个 domain 还是 {A, B}（**arc-consistent**），但**没有 solution** → 结论：arc consistency 不一定能发现无解 → 用 **Alldiff(C1, C2, C3)**：3 个 variables > 2 个值 → 马上发现 inconsistent

## ⚠️ Tutorial 1–6 同学答案里要注意的地方

- T3 Q2：4 phases 第 3 个写 "Solution Formulation" → slide 是 **Solution Searching**
- T1 Q2a(v)：rational agent 的 3 capabilities 同学写了 expected utility / planning / learning → slide 是 **Information gathering、Learning、Autonomy**
- T2 Q1 表格叫 "12 types"，slide 是 **6 对 properties**（再加 Known vs Unknown）

---

## 最后：每章默写（考前 10 分钟）

1. **Ch3**：Landscape 5 元素 + 2 轴；卡住 3 原因（local maxima、ridges、plateaus）；3 variants；EA 5 phases + 3 termination
2. **Ch4**：5 characteristics；6 elements（S₀、ACTIONS、RESULT、TO-MOVE、IS-TERMINAL、UTILITY）；minimax value；α / β 定义；cut-off 3 问题
3. **Ch5**：X, D, C；5 种 assignment；formulation（timetable、sudoku、queens）；node / arc / path consistency；MRV、degree、LCV；forward checking vs MAC
4. **Ch1**：4 approaches；Turing Test 3 方 + 6 capabilities；rationality 4 件事（P-K-A-P）；PEAS；7 properties；4 agents + learning 4 components
5. **Ch2**：4 phases；6 components；FIFO / LIFO / priority；COST；A* = g + h；admissible vs consistent

---

> ❓ **待确认**：你说 lecture 1–6，但资料夹里只有 Chapter 1A–5C 的 slides。Tutorial 13 是 propositional logic（truth table、CNF），应该属于 Chapter 6。如果 Chapter 6 也会考，请把 slides 上传，我再补进来。
