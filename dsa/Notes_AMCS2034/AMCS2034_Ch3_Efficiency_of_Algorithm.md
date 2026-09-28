# AMCS2034 — Chapter 3: Efficiency of an Algorithm

> Slide 讲了"为什么不用秒数比较 algorithm"和 best / worst / average case，但**从来没真正讲 binary search**，考卷却问"Distinguish linear search and binary search"；p27 标题写 "Linear Search Algorithms"，内容却是**找最大值**。考卷（Oct 2024 Q2）整题 25 分都是这章的 e-commerce 情境题。这份笔记用一个 e-commerce 搜寻功能贯穿全章，补上 binary search，并给每一小题满分答法。

---

## 0. 一句话总览

**比较 algorithm 不看"跑几秒"，而看"input 变大时，工作量长得多快"（rate of growth）；同一个 algorithm 对不同 input 也有快有慢，所以要分 best、worst、average case 来分析，而 worst case 最有用。**

| Part | 内容 | Slide |
|---|---|---|
| 1 | Algorithm analysis 的目的 | p3–4 |
| 2 | Running time analysis 与 input size | p5–6 |
| 3 | 怎么比较 algorithm（为什么不用执行时间） | p7–10 |
| 4 | Rate of growth | p10–16 |
| 5 | Types of analysis：best / worst / average | p17–25 |
| 6 | Linear search、找最大值、constant time | p26–31 |
| 7 | Linear search vs binary search（slide 没讲，补上） | p8 提到 |

🎯 **这章回答的考题**
- **May 2025 Q2c** Apply THREE types of analysis (9)
- **Oct 2024 Q2a(i)** TWO main concerns in the analysis of the algorithm (6)
- **Oct 2024 Q2a(ii)** & **Dec 2025 Q1c** Relate running time analysis with algorithm selection (3 / 2)
- **Oct 2024 Q2a(iii)** TWO problems with running time analysis (4)
- **Oct 2024 Q2a(iv)** Performance under THREE scenarios (9)
- **Oct 2024 Q2b** Distinguish linear search and binary search (3)

**Prerequisites**：Ch1 algorithm 定义；会看 for loop。

---

## Scene：两个工程师吵架

一个 e-commerce 平台要做产品搜寻。Amir 写了版本 A，Mei 写了版本 B。

Amir："我的在我电脑上跑 0.2 秒，你的要 0.5 秒，我赢。"
Mei："你电脑是新的 M4，我的是五年前的笔电，而且我测的时候在开 Zoom。还有，你搜的那个产品刚好排在第一个。"

两个人都对，也都说不清楚。他们需要一种**跟电脑、跟当下负载、跟刚好搜哪个产品都无关**的比较方式。这章就是在建立这个方式。

---

## 1. Algorithm Analysis 的目的（p3–4）

就像从城市 A 到城市 B 可以坐飞机、巴士、火车、骑脚踏车，同一个问题也有很多 algorithm（排序就有 insertion sort、selection sort、quick sort…）。**Algorithm analysis 帮我们判断哪一个在时间和空间上最有效率。**

**Goal of algorithm analysis**：比较 algorithm（或解法），依据：
1. **Running time**（时间）
2. **Memory requirement**（空间）
3. **Developer effort**（开发成本）

> 💬 **答题句 (EN)** — *Algorithm analysis compares different algorithms for the same problem in terms of running time, memory requirement and developer effort, to determine which one is most efficient.*

### ✅ 满分答法 — Oct 2024 Q2a(i) (6 marks)
*E-commerce platform handling large volumes of product data for search and filtering. Examine and explain TWO main concerns in the analysis of the algorithm.*

1. **Running time (time complexity)** (3) – how the processing time of the search algorithm grows as the number of products (input size n) increases. For the e-commerce platform, users expect search and filter results almost instantly, so an algorithm whose running time grows slowly (e.g. O(log n)) is needed as the catalogue grows to millions of products.
2. **Memory requirement (space complexity)** (3) – the amount of main memory the algorithm and its data structure need while running. Filtering by price, category and availability may use extra structures such as indexes or sorted copies; these speed up searching but consume memory, so the platform must make sure the memory used stays within the server's capacity when many users search at the same time.

*（第三个 concern 是 developer effort：演算法越复杂，开发和维护成本越高。题目只要两个，选上面两个最稳。）*

---

## 2. Running Time Analysis（p5–6）

**Running time analysis** = 判断**处理时间怎么随着问题大小（input size）增加而增加**。

**Input size** = input 里元素的数量。常见的 input 类型（p6）：

| Input 类型 | 例子 |
|---|---|
| Size of array | 搜寻 n 个产品 |
| Polynomial degree | 计算 n 次多项式 |
| Number of elements in the matrix | n × n 矩阵相乘 |
| Number of bits in the binary representation | 大整数运算 |
| Vertices and edges in a graph | 找最短路径（Ch9） |

---

## 3. 怎么比较 Algorithm（p7–10）

| 方法 | 为什么不好 |
|---|---|
| **Execution times** | 跟**特定电脑**有关 |
| **Number of statements executed** | 跟**程序语言**和**程序员的写法**有关 |

实际跑程序量时间，有**两个问题**（p8）：
1. **System load**——电脑上同时有很多工作在跑，执行时间会随系统负载变动。
2. **Depends on the specific input**——例如要找的元素刚好是 list 第一个，**linear search 会比 binary search 还快**，但这不代表 linear search 比较好。

**Ideal solution（p9）**：把 running time 写成 **input size n 的函数 f(n)**，比较这些函数。这种比较**独立于机器时间、程序写法等**。

> 💬 **答题句 (EN)** — *Execution time is not a good measure because it depends on the particular computer and system load, and the number of statements depends on the programming language and programmer's style; instead, running time is expressed as a function f(n) of the input size, which is independent of machine and programming style.*

### ✅ 满分答法 — Oct 2024 Q2a(iii) (4 marks)
*Point out TWO problems with the running time analysis for the algorithms of the e-commerce platform.*

1. **Dependence on system load and hardware** (2) – when the search algorithm is timed by running it, many tasks run concurrently on the server, so the measured time depends on the current load and on the machine used; two algorithms timed on different servers (or at peak vs off-peak hours) cannot be compared fairly.
2. **Dependence on the specific input** (2) – the measured time depends on which product is searched; if the product happens to be first in the list, linear search finds it faster than binary search, so a few test runs can give a misleading picture of which algorithm is better for all searches.

*（Ch4 p18 还有：只能测有限的 test input、必须先完整实作才能测。）*

---

## 4. Rate of Growth（p10–16）

**Rate of growth** = *the rate at which the running time increases as a function of input*（p11）。理论方法**不依赖电脑和特定 input**，只看 input 变大时执行时间**长得多快**，用 growth rate 比较两个 algorithm（p10）。

### 4.1 买车和脚踏车（p12–13）

你去店里买一辆车和一辆脚踏车，朋友问你买什么，你说"买车"——因为脚踏车的价钱跟车比起来可以忽略。

```
Total Cost = cost_of_car + cost_of_bicycle
Total Cost ≈ cost_of_car            (approximation)
```

同样地，对大的 n，**低次项可以忽略**：

```
n⁴ + 2n² + 100n + 500  ≈  n⁴
```

*n⁴ 的成长率最高，其他项在 n 很大时微不足道。*（n = 100 时：n⁴ = 100,000,000，其余三项合计只有 30,500。）

### 4.2 Linear search 的成长率（p14–15）

Linear search 依序比较 key 和每个元素：
- key **不在** array 里：需要 **n** 次比较；
- key **在** array 里：**平均 n/2** 次比较。

Array 变两倍，比较次数也变两倍 → **linear rate** → order of magnitude 是 n → **O(n)**，读作 "order of n"。O(n) 的 algorithm 叫 **linear algorithm**。

### 4.3 Commonly used rates of growth（p16）⭐

| Time complexity | Name | Example（slide） |
|---|---|---|
| 1 | Constant | Adding an element to the front of a linked list |
| log n | Logarithmic | Finding an element in a **sorted** array |
| n | Linear | Finding an element in an **unsorted** array |
| n log n | Linear logarithmic | Sorting n items by divide-and-conquer — **Mergesort** |
| n² | Quadratic | Shortest path between two nodes in a graph |
| n³ | Cubic | Matrix multiplication |
| 2ⁿ | Exponential | The Towers of Hanoi problem |

p16 右边的图是**由快到慢的排列**（上面成长最快）：

```
2^(2^n) > n! > 4ⁿ > 2ⁿ > n² > n log n ≈ log(n!) > n > 2^(log n) > log²n > √(log n) > log log n > 1
```

> 💬 **答题句 (EN)** — *Rate of growth is the rate at which running time increases as a function of input size; for large n the lower-order terms are insignificant, so n⁴ + 2n² + 100n + 500 is approximated by n⁴.*

---

## 5. Types of Analysis（p17–25）⭐

同一个 algorithm、同样大小的 input，执行时间仍然可能不同，取决于**是哪一种 input**。所以要用多个 expression 表示：

| Type | 定义 | 意义 |
|---|---|---|
| **Worst case** | 让 algorithm **跑最久**的 input | **Upper bound**——保证不会比这更慢；最常用 |
| **Best case** | 让 algorithm **跑最快**的 input | **Lower bound**——不具代表性 |
| **Average case** | 对**随机** input 的预测：用很多来自某分布的 input 跑，总时间除以次数 | 最理想，但**难做**（要知道 input 的机率分布） |

```
Lower Bound  <=  Average Time  <=  Upper Bound
 (best case)                        (worst case)
```

p23 的例子——同一个 algorithm 可用不同 expression 表示：
```
f(n) = n² + 500,         for worst case
f(n) = n + 100n + 500,   for best case
```

**为什么常用 worst case（p24–25）**：你能保证 algorithm **绝不会比 worst case 慢**；worst-case 分析**比较容易做**，只需要找出最坏的 input。Average case 虽然理想，但很难决定各种 input 的机率和分布。

> 💬 **答题句 (EN)** — *Worst-case analysis considers the input that makes the algorithm run slowest and gives an upper bound; best-case analysis considers the input that makes it run fastest and gives a lower bound; average-case analysis predicts the running time over random inputs, so lower bound ≤ average time ≤ upper bound.*

### ✅ 满分答法 — May 2025 Q2c (9 marks)
*Apply THREE types of analysis used in algorithms and demonstrate how each is used in algorithm performance evaluation.*

示范 algorithm：用 linear search 在 n 个元素的 array 中找 key。每点 3 分 = 定义 + 用在 linear search + 在评估中的作用。

1. **Worst-case analysis** – considers the input for which the algorithm takes the longest time. For linear search, the worst case is when the key is the last element or not in the array, requiring **n comparisons**, so f(n) = n → O(n). It is used to guarantee an upper bound: the algorithm will never be slower than this, which is why it is the most commonly used analysis.
2. **Best-case analysis** – considers the input for which the algorithm takes the least time. For linear search, the best case is when the key is the first element, requiring only **1 comparison** → constant time. It gives a lower bound but is not representative of normal performance, so it is rarely used alone.
3. **Average-case analysis** – predicts the running time for random inputs by averaging the time over many inputs from a distribution. For linear search, when the key is in the array it needs about **n/2 comparisons** on average, which is still O(n). It gives the most realistic prediction but is harder to perform because the probability distribution of inputs must be known.

### ✅ 满分答法 — Oct 2024 Q2a(iv) (9 marks)
*Examine the algorithm's performance under THREE different scenarios with different types of input.*

以平台用 linear search 在 n 个产品里找某个产品为例（也可写 binary search，见 §7）：

1. **Best-case scenario** (3) – the product the user searches for is the **first** product in the list. The algorithm finds it after **one comparison**, so the time is constant regardless of how many products there are. This is the fastest possible performance but it rarely happens.
2. **Worst-case scenario** (3) – the product is the **last** one in the list or is **not available** at all. The algorithm must compare with **all n products**, so the time grows linearly, O(n); with millions of products this search becomes slow. This gives the upper bound the developers must plan for.
3. **Average-case scenario** (3) – the searched products are randomly distributed in the list. On average the algorithm checks about **half the products (n/2 comparisons)**, which is still O(n). This predicts typical performance, showing the platform should use a faster method (e.g. binary search on sorted data or indexing) as the catalogue grows.

---

## 6. Linear Search、找最大值、Constant Time（p26–31）

### 6.1 Linear search（p26）
- 依序逐一搜寻所有 item。
- **Worst case n 次比较；average case n/2 次**（如果几乎都在找存在的东西）。两者用 Big-O 都是 **O(n)**。
- **乘法常数可以省略**：algorithm analysis 关心 growth rate，常数不影响 growth rate。**O(n) = O(n/2) = O(100n)**。

p28 的表（input 变两倍时，三个函数都变两倍）：

| n \ f(n) | n | n/2 | 100n |
|---|---|---|---|
| 100 | 100 | 50 | 10000 |
| 200 | 200 | 100 | 20000 |
| **f(200) / f(100)** | **2** | **2** | **2** |

*看这张表要注意：常数不同，但成长的**比例**完全一样——这就是为什么常数可以丢掉。*

### 6.2 找最大值（p27, p29–30）

⚠️ p27 标题写 "Linear Search Algorithms"，但步骤其实是**找 array 最大值**：
```
Step 1: Input – an array of numbers, arr
Step 2: Initialise max to store the maximum found so far (max ← arr[0])
Step 3: Iterate through the array
Step 4: Check for maximum (if arr[i] > max then max ← arr[i])
Step 5: Repeat until the end
```
n 个元素需要 **n − 1 次比较**（n = 2 → 1 次；n = 3 → 2 次）。n 很大时 −1 不重要，Big-O **忽略非主导部分** → **O(n)**。Algorithm analysis 是为**大的 input** 而做；input 小时估计效率没有意义（p30）。

### 6.3 Constant time（p31）

如果时间**跟 input size 无关**，就是 **constant time，O(1)**。例：用 index 取 array 的某个元素——array 再大，`arr[i]` 的时间都一样。

---

## 7. Linear Search vs Binary Search（*extra*：slide 只在 p8 提到）

**Binary search**：只能用在**已排序**的 array。每次看中间那个：
- 等于 key → 找到；
- key 比较小 → 只留左半；
- key 比较大 → 只留右半。

每一步排除一半，所以最多约 **log₂ n** 步。

**例子**：sorted array `[3, 8, 15, 22, 31, 47, 56, 70]`，找 47：

| Step | low | high | mid | arr[mid] | 动作 |
|---|---|---|---|---|---|
| 1 | 0 | 7 | 3 | 22 | 47 > 22 → low = 4 |
| 2 | 4 | 7 | 5 | 47 | **找到** |

Linear search 要比较 6 次（3, 8, 15, 22, 31, 47）；binary search 只要 2 次。n = 1,000,000 时，linear worst case 100 万次，binary 约 20 次（2²⁰ ≈ 1,048,576）。

| | Linear search | Binary search |
|---|---|---|
| 资料要求 | 不需要排序 | **必须已排序** |
| 做法 | 从头依序逐一比较 | 每次和中间比较，丢掉一半 |
| Best case | O(1)（第一个就是） | O(1)（中间就是） |
| Worst case | **O(n)** | **O(log n)** |
| 适合 | 小量或未排序资料 | 大量、已排序、常被搜寻的资料 |

### ✅ 满分答法 — Oct 2024 Q2b (3 marks)
*Distinguish linear search and binary search.*

Linear search checks each element one by one from the beginning until the target is found or the list ends; it works on unsorted data and takes O(n) time in the worst case (1). Binary search works only on **sorted** data: it compares the target with the middle element and discards half of the remaining elements each step (1), so its worst-case time is O(log n), which is much faster for large datasets (1).

---

## 8. 把分析连到"选哪个 algorithm"

### ✅ 满分答法 — Oct 2024 Q2a(ii) (3 marks) / Dec 2025 Q1c (2 marks)
*Relate the running time analysis with the algorithm selection.*

Running time analysis expresses each algorithm's running time as a function of input size, f(n), independent of hardware and programming style (1). By comparing the growth rates of these functions (e.g. O(n) for linear search vs O(log n) for binary search), developers can predict how each algorithm will perform as the data grows (1). They then select the algorithm whose running time grows most slowly for the expected input size — for a large, growing product catalogue, binary search or another O(log n) method is selected over linear search (1).

*（2 分版本：写前两句的合并 + 最后一句结论即可。）*

---

## Closing the loop

回到 Amir 和 Mei：他们该做的不是比秒数，而是写出两个版本的 f(n)。如果 A 是 n 次比较、B 是 log n 次，那不管在谁的电脑上，产品一多 B 就一定赢——而且还要说清楚是 best、worst 还是 average case。下一章会教怎么**数**出 f(n)（counting primitive operations），以及空间怎么算。

---

## ⚠️ Where the slides mislead

| Slide | 说法 | 更准确的理解 |
|---|---|---|
| p27 | 标题 "Linear Search Algorithms" | 内容是**找最大值**的步骤（n − 1 次比较），不是搜寻 |
| p18 | "asymptotic analysis/notation (Chapter 5)" | 正确，Big-O 等在 Ch5 |
| p16 | n² 的例子 "shortest path between two nodes in a graph" | 视演算法而定（例如 Dijkstra 简单版 O(V²)）；考试照 slide 写即可 |
| p8 | 提到 binary search 但没解释 | 见本笔记 §7 |
| p23 | best case 写成 "n + 100n + 500" | 就是 101n + 500，仍是 O(n)；slide 只是示意 |

---

## Term table

| English | 中文 | 一句话说明 |
|---|---|---|
| Algorithm analysis | 演算法分析 | 比较演算法的时间、空间、开发成本 |
| Running time analysis | 执行时间分析 | 时间随 input size 如何增加 |
| Input size | 输入大小 | input 中元素的数量 n |
| Rate of growth | 成长率 | 执行时间随 n 增加的速度 |
| Order of magnitude | 数量级 | 主导的成长项 |
| Worst / Best / Average case | 最坏 / 最佳 / 平均情况 | 最慢 / 最快 / 随机 input |
| Upper bound / Lower bound | 上界 / 下界 | 不会更慢 / 不会更快 |
| Linear search | 线性搜寻 | 逐一比较，O(n) |
| Binary search | 二分搜寻 | 已排序，每次砍半，O(log n) |
| Constant time | 常数时间 | 与 n 无关，O(1) |
| System load | 系统负载 | 同时执行的工作量 |

---

## Cheat sheet

- **Goal**：compare by running time · memory requirement · developer effort。
- **Running time analysis**：time 随 input size 增加的情形。Input 类型：array size、polynomial degree、matrix elements、bits、vertices & edges。
- **不用执行时间**（电脑、system load、specific input）**也不用 statement 数**（语言、风格）→ 用 **f(n)**。
- **Rate of growth**：丢掉低次项（车 + 脚踏车 ≈ 车）；n⁴ + 2n² + 100n + 500 ≈ n⁴。
- **表**：1 constant · log n sorted search · n unsorted search · n log n mergesort · n² shortest path · n³ matrix mult · 2ⁿ Hanoi。
- **Worst**（upper bound，最常用、较容易）≥ **Average**（理想但难）≥ **Best**（lower bound，不具代表性）。
- **Linear search**：worst n，average n/2 → O(n)；O(n) = O(n/2) = O(100n)。
- **Find max**：n − 1 comparisons → O(n)。**Array index** → O(1)。
- **Binary search**：sorted、砍半、O(log n)。

---

## Practice (answers included)

### A. MCQ
1. Which is the best reason not to compare algorithms by execution time? (a) it is hard to measure (b) it depends on the computer and system load (c) it is always zero (d) it ignores memory
2. Which analysis gives a guarantee that the algorithm will never be slower? (a) best case (b) average case (c) worst case (d) empirical
3. Finding the maximum in an array of 50 elements needs how many comparisons? (a) 50 (b) 49 (c) 25 (d) 1
4. Binary search requires the data to be: (a) in a linked list (b) sorted (c) unique (d) numeric only
5. O(100n) is the same as: (a) O(n²) (b) O(100) (c) O(n) (d) O(log n)

**Answers:** 1 (b) · 2 (c) · 3 (b) · 4 (b) · 5 (c)

### B. Short answer
**B1. Why is average-case analysis difficult? (2 marks)**
It requires knowing the probability distribution of all possible inputs of the same size, which is hard to determine for most problems.

**B2. State THREE common types of input size. (3 marks)**
Size of an array; number of elements in a matrix; number of vertices and edges in a graph (also polynomial degree, number of bits).

**B3. Explain why the constant 1/2 can be omitted in n/2. (2 marks)**
Algorithm analysis focuses on growth rate; multiplying by a constant does not change how fast the function grows — doubling n doubles n/2 exactly as it doubles n — so O(n/2) = O(n).

### C. Application
**C1. A sorted array has 1,024 student IDs. What is the maximum number of comparisons for linear search and for binary search? (2 marks)**
Linear search: 1,024. Binary search: about log₂1024 = 10 halvings, so at most 11 comparisons (10 halvings plus the final check).

**C2. For the running times f₁(n) = 3n + 20 and f₂(n) = n²/10, which is faster for n = 10 and for n = 1000? (4 marks)**
n = 10: f₁ = 50, f₂ = 10 → f₂ is faster. n = 1000: f₁ = 3,020, f₂ = 100,000 → f₁ is much faster. For large inputs the linear algorithm wins, even though it is slower on small input.

**C3. A hotel booking app searches for a room number in an unsorted list of 200 rooms. Describe the best, worst and average case. (6 marks)**
Best: the room is first in the list → 1 comparison, constant time. Worst: the room is last or not in the list → 200 comparisons, O(n). Average: about 100 comparisons (n/2), still O(n).

### D. Thinking
**D1. Mei's search is faster on her test data but has worse worst-case growth. Which should the team choose for a system that must respond within 1 second at peak time? (3 marks)**
Choose the algorithm with the better worst-case growth, because the system must guarantee a response time under the heaviest load; worst-case analysis gives the upper bound the team can rely on, whereas good results on a few test inputs may not hold for all inputs.

**D2. Is binary search always better than linear search? (3 marks)**
No. Binary search needs sorted data; if the data changes often, keeping it sorted costs extra time. For very small lists or data searched only once, linear search is simpler and fast enough.

---

## Slide index

| Note section | Slides |
|---|---|
| 1 Goal | p3–4 |
| 2 Running time analysis | p5–6 |
| 3 Comparing algorithms | p7–10 |
| 4 Rate of growth | p10–16（图 p12, p13, p16） |
| 5 Types of analysis | p17–25（图 p22, p23） |
| 6 Linear search / max / constant | p26–31（图 p28） |
| 7 Binary search | *extra*（p8 提到） |

## Links to other chapters
- **Ch4**：怎么数 primitive operations 得到 f(n)；time 与 space complexity；experimental analysis 的缺点
- **Ch5**：用 O、Ω、Θ 正式表示 upper / lower / tight bound
- **Ch6**：HashSet 查询为什么接近 O(1)
