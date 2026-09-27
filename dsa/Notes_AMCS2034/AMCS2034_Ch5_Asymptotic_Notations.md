# AMCS2034 — Chapter 5: Asymptotic Notations

> 考卷有一题**几乎每次都出、而且 9 分**：给一张 growth-rate function 表（例如 5n² + 20n + 100、40n log n + 600、150n + 90），要你 "Determine the Big-O notation … **Show your working**"，然后推荐一个。Slide 只在 p13 示范了两行"ignoring smaller terms / ignoring the coefficient"，没说 working 要写到什么程度，也没说 Ω、Θ 的正式定义怎么用。这份笔记给一个**三步固定写法**（每步 1 分），附上 c 和 N 的正式证明（已用程序检查到 n = 10⁶），并算出两份考卷的所有答案。

---

## 0. 一句话总览

**Asymptotic notation 只看 n 很大时函数"长得多快"：丢掉低次项、丢掉常数，剩下的主导项就是 order of growth。O 是上界（不会更慢）、Ω 是下界（不会更快）、Θ 是上下都夹住（tight bound）。**

| Part | 内容 | Slide |
|---|---|---|
| 1 | 什么是 asymptotic analysis | p3–7 |
| 2 | O、Ω、Θ 的定义 | p8–15 |
| 3 | 怎么算 Big-O（考试写法） | p11–13 |
| 4 | Assess the efficiency of an algorithm | p16–17 |
| 5 | 七个最常见的函数 | p18–25 |
| 6 | Growth-rate 表与"Picturing efficiency" | p26–31 |

🎯 **这章回答的考题**
- **May 2025 Q2d** Provide THREE notations used to classify the growth of an algorithm (9)
- **Dec 2025 Q1d** Big-O for P = 5n² + 20n + 100, Q = 40n log n + 600, R = 150n + 90, show working (9)
- **Oct 2025 Q1b(i)** Big-O for A = 4n² + n + 50, B = 80n + 250, C = 10 log n + 340, show working (9)
- **Oct 2025 Q1b(ii)** Recommend the most suitable algorithm and justify (3)

**Prerequisites**：Ch3 rate of growth、worst/best case；Ch4 counting operations（A = 2n + 1、B = n² + n + 1）。

---

## Scene：三个候选人，一个位置

EasyShop 的工程团队把三个搜寻 algorithm 都数好了 operation：

| Algorithm | f(n) |
|---|---|
| A | 4n² + n + 50 |
| B | 80n + 250 |
| C | 10 log n + 340 |

n = 1 时 A 只要 55 步，C 要 340 步——A 看起来最好。可是 EasyShop 要从 100 个产品扩张到全球。**哪一个才是"长大以后"最好的？** 我们需要一个只描述"长大以后"的语言。

---

## 1. 什么是 Asymptotic Analysis（p3–7）

- 对一个函数 f(n)，我们找**另一个函数 g(n)**，让它在 **n 很大时近似 f(n)**。数学上这种曲线叫 **asymptotic curve**，所以 algorithm analysis 又叫 **asymptotic analysis**（p3）。
- 我们已经有 best / average / worst case 的 expression，接着要为它们找出 **upper bound 和 lower bound**，需要一套语法来表示（p5）。
- **Asymptotic notation**（Big O、Big Omega、Big Theta）是描述函数和 algorithm **growth rate** 的数学记号，常用于分析 **time complexity 和 space complexity**（p6）。
- **Algorithm 的效率由它的 order of growth 决定；order of growth 又由 basic operation count 决定**。比较和排名 order of growth 用 O、Ω、Θ 三个记号（p7）——**Tutorial 5 Q2**。

p4 的图（f(n) = O(g(n))）画的是两条曲线：f(n) 一开始上下起伏，在某个点 **n₀** 之前曾经高过 c·g(n)；过了 n₀ 之后，c·g(n) 一路在 f(n) 上方。

```
  n <  n₀ :  f(n) may be above or below c·g(n)   (ignored)
  n >= n₀ :  f(n) <= c·g(n)  always              (this is what O(g(n)) promises)
```
*看这张图要注意：Big-O 只管 n₀ 之后（n 很大）的情况，小 n 时谁高谁低不重要。*

> 💬 **答题句 (EN)** — *Asymptotic notation describes the growth rate of an algorithm's running time (or space) as the input size becomes large, by finding a simpler function g(n) that approximates f(n) for large n; the three notations are Big O, Big Omega and Big Theta.*

---

## 2. O、Ω、Θ（p8–15）⭐

### 2.1 一句话定义（p8）

| Notation | 名称 | 意义 |
|---|---|---|
| **O(g(n))** | Big O | **Upper bound**——程序的**最多**时间（maximum time complexity） |
| **Ω(g(n))** | Big Omega | **Lower bound**——程序**至少**要花的时间（minimum time） |
| **Θ(g(n))** | Big Theta | **Tight bound**——同时在 upper 和 lower bound 之内 |

### 2.2 正式定义

**Big O（p9）**：algorithm 的时间需求 f(n) 是 **order at most g(n)**，写作 f(n) = O(g(n))，如果存在正实数 **c** 和正整数 **N**，使得
```
f(n) ≤ c · g(n)    for all n ≥ N
```

**Big Omega（p14，正式式子为 *extra*）**：代表 lower bound；当 n 趋近无限大时，algorithm 解问题所需的**最少**时间。正式：存在 c > 0 和 N，使
```
f(n) ≥ c · g(n)    for all n ≥ N
```

**Big Theta（p15）**：前两者的重叠。t(n) ∈ Θ(g(n))，如果存在正常数 c₁、c₂ 和非负整数 N，使
```
c₂ · g(n) ≤ t(n) ≤ c₁ · g(n)    for all n ≥ N
```

**p10 的图**（Big-O 的定义图）：两条曲线在 n = N（约 8）相交，之后 c·g(n) 一直在 f(n) 上方。

```
      c·g(n) ─── above f(n) for every n ≥ N
      f(n)   ─── may be above c·g(n) before N
   ───────────────┼───────────────────────▶ n
                  N
```

**比喻**：你搭 Grab 去机场，app 说"**最多** 40 分钟"（O）、"**至少** 25 分钟"（Ω）；如果它能说"**大约 30 分钟**，上下不会差太多"，那就是 Θ。

> 💬 **答题句 (EN)** — *Big O gives an upper bound: f(n) = O(g(n)) if there exist positive constants c and N such that f(n) ≤ c·g(n) for all n ≥ N, so the algorithm never takes longer than a constant multiple of g(n). Big Omega gives a lower bound (f(n) ≥ c·g(n)), the minimum time the algorithm takes. Big Theta gives a tight bound: c₂·g(n) ≤ f(n) ≤ c₁·g(n) for all n ≥ N.*

### ✅ 满分答法 — May 2025 Q2d (9 marks)
*The efficiency of an algorithm is gauged by its order of growth. Provide THREE notations used to classify the growth of an algorithm.*

每个 3 分：名称 + 意义 + 正式定义或例子。

1. **Big O notation, O(g(n)) – upper bound.** It represents the maximum time (worst-case growth) an algorithm can take. f(n) = O(g(n)) if there exist positive constants c and N such that f(n) ≤ c·g(n) for all n ≥ N. *Example:* 2n² + 4n + 1 = O(n²), since 2n² + 4n + 1 ≤ 7n² for all n ≥ 1.
2. **Big Omega notation, Ω(g(n)) – lower bound.** It represents the minimum time an algorithm will take as the input size grows. f(n) = Ω(g(n)) if f(n) ≥ c·g(n) for all n ≥ N. *Example:* linear search needs at least one comparison, so its best case is Ω(1); any algorithm that must read all n inputs is Ω(n).
3. **Big Theta notation, Θ(g(n)) – tight bound.** It is used when the growth is bounded both above and below by the same function: c₂·g(n) ≤ f(n) ≤ c₁·g(n) for all n ≥ N. *Example:* 2n + 1 = Θ(n), because n ≤ 2n + 1 ≤ 3n for all n ≥ 1.

---

## 3. 怎么算 Big-O（p11–13）⭐⭐

### 3.1 Slide 的写法（p13）
```
Algorithm B = n² + n + 1
            = n², ignoring smaller terms
            ⇒ O(n²)

Algorithm A = 2n + 1
            = 2n, ignoring smaller terms
            = n, ignoring the coefficient 2
            ⇒ O(n)
```

| Algorithm | Growth-rate function | Big-O（p12） |
|---|---|---|
| A | 2n + 1 | O(n) |
| B | n² + n + 1 | O(n²) |

p11 另外两个例子：**2n² + 4n + 1 → O(n²)**；**7 log n + 5 → O(log n)**。

### 3.2 考试固定三步（每个函数 3 分）

1. **找出 dominant term**（成长最快的项）——依 1 < log n < n < n log n < n² < n³ < 2ⁿ 排序；
2. **忽略 lower-order terms 和常数项**；
3. **忽略 dominant term 的 coefficient** → 写出 O(·)。

*加分（可选）：写一行 c 和 N 的证明，例如 5n² + 20n + 100 ≤ 125n² for all n ≥ 1。*

### ✅ 满分答法 — Dec 2025 Q1d (9 marks)
*Determine the Big-O notation for algorithms P, Q and R. Show your working clearly.*

**P: f(n) = 5n² + 20n + 100**
```
Dominant term: 5n² (n² grows faster than n and the constant)
f(n) = 5n², ignoring smaller terms 20n and 100
     = n², ignoring the coefficient 5
⇒ O(n²)
Check: 5n² + 20n + 100 ≤ 5n² + 20n² + 100n² = 125n² for all n ≥ 1  (c = 125, N = 1)
```

**Q: f(n) = 40n log n + 600**
```
Dominant term: 40n log n (n log n grows faster than a constant)
f(n) = 40n log n, ignoring the smaller term 600
     = n log n, ignoring the coefficient 40
⇒ O(n log n)
Check: for n ≥ 2, n log n ≥ 1, so 600 ≤ 600 n log n;
       40n log n + 600 ≤ 640 n log n for all n ≥ 2  (c = 640, N = 2)
```

**R: f(n) = 150n + 90**
```
Dominant term: 150n
f(n) = 150n, ignoring the smaller term 90
     = n, ignoring the coefficient 150
⇒ O(n)
Check: 150n + 90 ≤ 150n + 90n = 240n for all n ≥ 1  (c = 240, N = 1)
```

**结论（若被问到哪个最好）**：R = O(n) < Q = O(n log n) < P = O(n²)，大 n 时 **R 最有效率**。
（数值检查：n = 1000 时 P ≈ 5,020,100、Q ≈ 399,231、R = 150,090。）

### ✅ 满分答法 — Oct 2025 Q1b(i) (9 marks)
*Present the growth rate functions of Algorithms A, B and C using Big-O notation. Show your working steps.*

**A: f(n) = 4n² + n + 50**
```
Dominant term: 4n²
f(n) = 4n², ignoring smaller terms n and 50
     = n², ignoring the coefficient 4
⇒ O(n²)          (4n² + n + 50 ≤ 55n² for all n ≥ 1)
```

**B: f(n) = 80n + 250**
```
Dominant term: 80n
f(n) = 80n, ignoring the smaller term 250
     = n, ignoring the coefficient 80
⇒ O(n)           (80n + 250 ≤ 330n for all n ≥ 1)
```

**C: f(n) = 10 log n + 340**
```
Dominant term: 10 log n (log n grows, 340 is constant)
f(n) = 10 log n, ignoring the smaller term 340
     = log n, ignoring the coefficient 10
⇒ O(log n)       (10 log n + 340 ≤ 350 log n for all n ≥ 2)
```

### ✅ 满分答法 — Oct 2025 Q1b(ii) (3 marks)
*Recommend the most suitable algorithm (A, B or C) for EasyShop. Justify.*

**Algorithm C** is recommended (1). Its running time grows logarithmically, O(log n), which is the slowest growth rate of the three, compared with O(n) for B and O(n²) for A (1). EasyShop plans global expansion with a much larger inventory, so scalability matters more than performance on 100 products: at n = 100, A needs 40,150 operations, B 8,250 and C only about 406; at n = 1,000,000, A needs about 4 × 10¹² operations, B about 80 million, while C needs only about 540, so C keeps searches fast as the product catalogue grows (1).

*（注意：n ≤ 8 时 A 反而最快——这正是"小 input 时估计效率没有意义"的例子，但题目强调 scalability，所以选 C。）*

---

## 4. Assess the Efficiency of a Given Algorithm（p16–17）

评估效率 = 分析 time complexity 和 space complexity，了解 input 变大时的表现；实务上结合**理论分析、实际 benchmark、对问题需求与限制的理解**。步骤（p17）：

1. Analyze the algorithm
2. Determine the time complexity
3. Determine the space complexity
4. Consider best-case and worst-case scenarios
5. Compare with other algorithms
6. Benchmark and test
7. Optimize if necessary
8. Understand trade-offs

---

## 5. 七个最常见的函数（p18–25）— Tutorial 5 Q4

| Function | 写法 | 何时出现（slide） | 例子 |
|---|---|---|---|
| **Constant** | f(n) = c | 基本操作：两数相加、赋值、比较；n 多大都一样 | 用 index 取 array 元素 |
| **Logarithm** | f(n) = log_b n（b > 1） | x = log_b n ⇔ bˣ = n；**电脑科学预设 base 2**（二进位） | Binary search |
| **Linear** | f(n) = n | 对 n 个元素**各做一次**基本操作 | 把 x 和 n 个元素逐一比较 = n 次 |
| **n-log-n** | f(n) = n log n | 比 linear 快一点、比 quadratic 慢很多 | Merge sort |
| **Quadratic** | f(n) = n² | **巢状 loop**：外层 n 次 × 内层 n 次 = n² | 比较所有元素对 |
| **Cubic** | f(n) = n³ | 较少出现 | 矩阵相乘 |
| **Exponential** | f(n) = bⁿ（常见 b = 2） | loop 从 1 个 operation 开始，每轮**加倍** → 第 n 轮 2ⁿ | Towers of Hanoi |

> 💬 **答题句 (EN)** — *The quadratic function n² appears when an algorithm has nested loops, where the inner loop performs a linear number of operations and the outer loop is performed a linear number of times, giving n × n = n² operations.*

---

## 6. Growth-rate 表与 Picturing Efficiency（p26–31）

**Fig 2.4 — 各函数在不同 n 的值（log base 2）**

| n | log(log n) | log n | log² n | n | n log n | n² | n³ | 2ⁿ | n! |
|---|---|---|---|---|---|---|---|---|---|
| 10 | 2 | 3 | 11 | 10 | 33 | 10² | 10³ | 10³ | 10⁵ |
| 10² | 3 | 7 | 44 | 100 | 664 | 10⁴ | 10⁶ | 10³⁰ | 10⁹⁴ |
| 10³ | 3 | 10 | 99 | 1000 | 9966 | 10⁶ | 10⁹ | 10³⁰¹ | 10¹⁴³⁵ |
| 10⁴ | 4 | 13 | 177 | 10,000 | 132,877 | 10⁸ | 10¹² | 10³⁰¹⁰ | 10¹⁹³³⁵ |
| 10⁵ | 4 | 17 | 276 | 100,000 | 1,660,964 | 10¹⁰ | 10¹⁵ | 10³⁰¹⁰³ | 10²⁴³³³⁸ |
| 10⁶ | 4 | 20 | 397 | 1,000,000 | 19,931,569 | 10¹² | 10¹⁸ | 10³⁰¹⁰³⁰ | 10²⁹³³³⁶⁹ |

**Fig 5.1**：`for i = 1 to n: sum = sum + i` → n 个工人各做一次 → **O(n)**。

**问题大小变两倍时（p28）** ⭐

| Size n | Size 2n | Effect on time |
|---|---|---|
| 1 | 1 | None |
| log n | 1 + log n | Negligible |
| n | 2n | Doubles |
| n log n | 2n log n + 2n | Doubles and then adds 2n |
| n² | (2n)² = 4n² | Quadruples |
| n³ | (2n)³ = 8n³ | Multiplies by 8 |
| 2ⁿ | 2²ⁿ = (2ⁿ)² | Squares |

**处理 100 万个 item、每秒 100 万个 operation（p29）**

| g(n) | g(10⁶) / 10⁶ |
|---|---|
| log n | 0.0000199 seconds |
| n | 1 second |
| n log n | 19.9 seconds |
| n² | 11.6 days |
| n³ | 31,709.8 years |
| 2ⁿ | 10³⁰¹⁰¹⁶ years |

（验算：n² = 10¹² 次 ÷ 10⁶ = 10⁶ 秒 ≈ 11.57 天；n³ = 10¹² 秒 ≈ 31,709.8 年。）

**Comments on efficiency（p30–31）**：问题规模**小**时，用 O(n²)、O(n³) 甚至 O(2ⁿ) 也可以。p31 的图显示 O(1) 是水平线，O(log n)、O(√n)、O(n) 依序变陡，O(n²)、O(n³)、O(nⁿ) 急速上升。

---

## Closing the loop

回到 EasyShop 的三个候选：n = 1 时 A 最快只是假象；用 asymptotic notation 一写——A = O(n²)、B = O(n)、C = O(log n)——就知道"长大以后"C 最好。下一章开始讲具体的 data structure：Set，以及为什么 HashSet 查一个元素几乎不用花时间。

---

## ⚠️ Where the slides mislead

| Slide | 说法 | 更准确的理解 |
|---|---|---|
| p14 | "Big Omega … represents the lower bound **or best-case scenario**" | Ω 是 lower **bound**，不等于 best **case**；worst case 也可以有 Ω。考试照 slide 写"lower bound / minimum time"即可 |
| p8 | "Big O … is the maximum time complexity of a program" | O 是 upper bound；常用来描述 worst case，但两个概念不完全相同 |
| p6 | "Asymptotic notation, also known as Big O notation…" | Big O 只是 asymptotic notation 的**其中一种** |
| p26 | 表头 "2n" | 应为 **2ⁿ**（数值 10³⁰ 等是 2ⁿ） |
| p10 | 标题 "Formalities (Optional)" | 考试若问 "formal definition"，要写 f(n) ≤ c·g(n) for all n ≥ N |

---

## Term table

| English | 中文 | 一句话说明 |
|---|---|---|
| Asymptotic analysis | 渐近分析 | 只看 n 很大时的行为 |
| Asymptotic curve | 渐近曲线 | 在大 n 时近似 f(n) 的 g(n) |
| Order of growth | 成长阶 | 主导项决定的成长速度 |
| Big O | 大 O | Upper bound，最多 |
| Big Omega (Ω) | 大 Omega | Lower bound，至少 |
| Big Theta (Θ) | 大 Theta | Tight bound，上下夹住 |
| Dominant term | 主导项 | 成长最快的那一项 |
| Lower-order terms | 低次项 | 可忽略的项 |
| Coefficient | 系数 | 主导项前面的常数 |
| Logarithmic / Linear / Quadratic / Cubic / Exponential | 对数 / 线性 / 平方 / 立方 / 指数 | 七个常见函数 |

---

## Cheat sheet

- **O** upper bound（max）· **Ω** lower bound（min）· **Θ** tight（both）。
- **O 正式**：f(n) ≤ c·g(n) for all n ≥ N。**Θ**：c₂g(n) ≤ f(n) ≤ c₁g(n)。
- **三步**：dominant term → ignore smaller terms → ignore coefficient ⇒ O(·)。
- **顺序**：1 < log n < n < n log n < n² < n³ < 2ⁿ < n!。
- **Dec 2025**：P = O(n²) · Q = O(n log n) · R = O(n)。
- **Oct 2025**：A = O(n²) · B = O(n) · C = O(log n) → 推荐 C。
- **Slide 例**：2n+1 → O(n) · n²+n+1 → O(n²) · 2n²+4n+1 → O(n²) · 7 log n + 5 → O(log n)。
- **Doubling n**：log n 几乎不变 · n 两倍 · n² 四倍 · n³ 八倍 · 2ⁿ 平方。
- **10⁶ items @ 10⁶ ops/s**：n 1 s · n log n 19.9 s · n² 11.6 days · n³ 31,709.8 years。

---

## Practice (answers included)

### A. MCQ
1. f(n) = 3n³ + 100n² + 7 is: (a) O(n²) (b) O(n³) (c) O(3n³) only (d) O(100n²)
2. Which notation gives a tight bound? (a) O (b) Ω (c) Θ (d) none
3. If the input size doubles, an O(n²) algorithm's time: (a) doubles (b) quadruples (c) multiplies by 8 (d) squares
4. Which grows slowest? (a) n (b) n log n (c) log n (d) √n
5. In computer science the default base of log is: (a) 10 (b) e (c) 2 (d) n

**Answers:** 1 (b) — O(3n³) is also true but we drop the coefficient · 2 (c) · 3 (b) · 4 (c) · 5 (c)

### B. Short answer
**B1. Determine the Big-O of f(n) = 6n log n + 3n + 12. Show working. (3 marks)**
Dominant term 6n log n (n log n grows faster than n and the constant); ignoring smaller terms 3n and 12 gives 6n log n; ignoring the coefficient 6 gives n log n ⇒ **O(n log n)**.

**B2. Explain why an algorithm's order of growth determines its efficiency. (3 marks)**
The order of growth describes how the number of basic operations increases as the input grows. For large inputs the dominant term controls the running time, so an algorithm with a lower order of growth (e.g. O(log n)) will eventually be faster than one with a higher order (e.g. O(n²)) regardless of constants or hardware.

**B3. Give the formal definition of Big O. (2 marks)**
f(n) = O(g(n)) if there exist a positive real constant c and a positive integer N such that f(n) ≤ c·g(n) for all n ≥ N.

### C. Application
**C1. Table: X = 3n³ + 2, Y = 500n + 1000, Z = 2ⁿ + n². Give Big-O with working and rank them. (9 marks)**
- X: dominant 3n³; ignore 2 → 3n³; ignore 3 → **O(n³)**.
- Y: dominant 500n; ignore 1000 → 500n; ignore 500 → **O(n)**.
- Z: dominant 2ⁿ (exponential beats n²); ignore n² → **O(2ⁿ)**.
- Ranking from most to least efficient for large n: Y (O(n)) → X (O(n³)) → Z (O(2ⁿ)).

**C2. Prove that 3n + 8 = O(n). (2 marks)**
For n ≥ 1, 8 ≤ 8n, so 3n + 8 ≤ 3n + 8n = 11n. Take c = 11, N = 1 ⇒ 3n + 8 = O(n).

**C3. An O(n log n) algorithm takes 2 seconds for n = 1,000. Roughly how long for n = 2,000? (2 marks)**
Time ∝ n log n. T(2000)/T(1000) = (2000 × log₂2000) / (1000 × log₂1000) = (2000 × 10.97) / (1000 × 9.97) ≈ 2.2, so T(2000) ≈ 2 × 2.2 ≈ **4.4 seconds** — slightly more than double ("doubles and then adds 2n").

### D. Thinking
**D1. A startup with only 50 records picks an O(n²) algorithm because it is simpler. Is this acceptable? (3 marks)**
Yes for now: with small n the difference is negligible (slide p30: O(n²) is fine for small problems) and simplicity reduces developer effort. But if the records are expected to grow to millions, the algorithm should be replaced, because n² grows quadratically (1,000,000² = 10¹² operations ≈ 11.6 days at 10⁶ ops/s).

**D2. Is it correct to say "Big O is the worst case and Big Omega is the best case"? (3 marks)**
Not exactly. Big O and Big Omega are bounds on a function; worst case and best case are particular inputs. We usually use Big O to state the worst-case running time and Ω for a lower bound, but each case (best, worst, average) can itself be described with O, Ω or Θ.

---

## Slide index

| Note section | Slides |
|---|---|
| 1 Asymptotic analysis | p3–7（图 p4） |
| 2 O, Ω, Θ | p8–15（图 p10） |
| 3 Computing Big-O | p11–13 |
| 4 Assess efficiency | p16–17 |
| 5 Seven functions | p18–25 |
| 6 Tables & pictures | p26–31（图 p26–29, p31） |

## Links to other chapters
- **Ch3**：best / worst / average case；binary search O(log n)
- **Ch4**：Algorithm A/B/C 的 operation count 来源
- **Ch6**：HashSet contains/add 平均 O(1)，TreeSet O(log n)
- **Ch9**：DFS / BFS 的 O(|V| + |E|)
