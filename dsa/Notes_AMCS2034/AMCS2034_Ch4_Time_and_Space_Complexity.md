# AMCS2034 — Chapter 4: Time and Space Complexity

> 近两份考卷（Dec 2025、Oct 2025）的 **Q1b / Q1a 都是"TWO main types of complexity"（8 分）**，Oct 2025 还加了 **space/time trade-off 的定义 + 例子**。Slide 分散在 50 页里讲 empirical analysis、counting operations、space complexity、lookup table、binary sort，但没有把"time vs space"整理成一张可以直接写的比较。这份笔记把 slide 的 Algorithm A/B/C 逐行数一次（已用程序验证），补上 time vs space 的对照，并给 8 分题的满分写法。

---

## 0. 一句话总览

**Time complexity 看"要做多少步"，space complexity 看"要占多少记忆体"；两者常常此消彼长（space/time trade-off）。实际去跑程序量时间（empirical）有很多干扰，所以改用"数 primitive operations"得到 f(n)。**

| Part | 内容 | Slide |
|---|---|---|
| 1 | Time complexity 与 space complexity 的定义 | p3–5, p13 |
| 2 | Empirical analysis 与它的问题 | p6–11, p14–19 |
| 3 | Counting primitive operations | p20–26 |
| 4 | 例子：计算 1 + 2 + … + n 的三个 algorithm | p27–34 |
| 5 | Space complexity 与 overhead | p35–39 |
| 6 | Space/time trade-off：lookup table、binary sort、disk-based | p40–50 |

🎯 **这章回答的考题**
- **Dec 2025 Q1b** & **Oct 2025 Q1a** Identify and explain the TWO main types of complexity (8 + 8)
- **May 2025 Q2b** THREE examples of counting primitive operations (3)
- **Oct 2025 Q1c(i)** Define the space/time trade-off principle (2)
- **Oct 2025 Q1c(ii)** Example of a space/time trade-off (3)

**Prerequisites**：Ch3 running time、rate of growth；等差数列公式。

---

## Scene：EasyShop 要扩张

EasyShop 目前只有 100 个产品，工程师随便写个搜寻都很快。老板说明年要做到全球、几十万个产品。

工程团队开会：
- "新的搜寻会不会变慢？"——这是**时间**的问题。
- "为了加快搜寻，要不要先建一个 index？那要多吃多少记忆体？"——这是**空间**的问题。
- "我们能不能先把两个版本都写好跑跑看？"——可以，但结果可信吗？

这章回答这三个问题。

---

## 1. 两种 Complexity（p3–5, p13）⭐

同一个问题可能有好几种解法，我们要比较它们的效能并选最好的。**Time complexity** 和 **space complexity** 是分析效率的两个不同面向。它们其实也受硬体、OS、处理器影响，但**分析时不考虑这些**，只看执行时间和所需储存空间（p3）。

| | **Time complexity** | **Space complexity** |
|---|---|---|
| 量什么 | 执行所需的**时间**（p13: the time the code takes to execute） | 执行所需的**记忆体**（p13: the memory required to execute the code） |
| 怎么估计 | **数 algorithm 执行的基本步骤（elementary steps）**，用 Big-O 表示 | 程序执行时在 **main memory** 占多少空间；由指令数与资料所需空间决定 |
| 通常用 | **Worst-case** time complexity（任何 input size 的最长时间） | 资料结构本身 + 额外空间（例如 linked list 每个 node 的 pointer = overhead） |
| 例子 | Linear search O(n)、binary search O(log n) | 用一个大小 n 的额外 array → O(n)；只用几个变量 → O(1) |
| 重要性 | 通常**比较重要**（p21） | 记忆体有限时是重要限制（p36） |

两者之间**通常有 trade-off**（p13）——见 §6。

> 💬 **答题句 (EN)** — *Time complexity measures how the execution time of an algorithm grows with input size, estimated by counting the elementary steps it performs and expressed in Big-O, usually for the worst case. Space complexity measures the amount of main memory an algorithm needs to execute as the input size grows, including the data structure and any extra storage.*

### ✅ 满分答法 — Dec 2025 Q1b (8 marks)
*CampusMart online bookstore growing from 200 to thousands of books. Identify and explain the TWO main types of complexity the team should consider when selecting algorithms.*

每一种 4 分：定义（1）+ 怎么量（1）+ 连到 case（1）+ 为什么重要（1）。

1. **Time complexity** – a measure of the execution time of an algorithm as a function of the input size n, expressed using Big-O notation. It is estimated by counting the number of elementary (primitive) steps the algorithm performs, usually for the worst case because that is the maximum time for any input of that size. For CampusMart, a linear search over the book records takes O(n) time, so as the collection grows from 200 to thousands of books each search becomes proportionally slower, while binary search on sorted titles takes only O(log n). The team should therefore choose the search algorithm whose time grows most slowly, so customers still find books quickly as the database grows.
2. **Space complexity** – a measure of the amount of main memory an algorithm needs while it executes, as a function of the input size. It includes the memory for the data itself and any extra structures or overhead (e.g. pointers, indexes, temporary arrays). For CampusMart, building an index or keeping a sorted copy of the book list speeds up searching but uses additional memory proportional to the number of books. The team must ensure the memory needed stays within the server's capacity as the records increase, balancing it against the time saved (space/time trade-off).

### ✅ 满分答法 — Oct 2025 Q1a (8 marks)
*EasyShop (100 products → global expansion). Identify and explain the TWO main types of complexity to analyse when selecting a search algorithm.*

同上结构，把 case 换成 EasyShop：
1. **Time complexity** – measures how the running time of the search algorithm increases as the number of products n increases; estimated by counting primitive operations and expressed in Big-O, usually for the worst case. With 100 products even a linear search O(n) is fast, but after global expansion to hundreds of thousands of products the difference between O(n), O(log n) and O(n²) becomes huge, so EasyShop must pick an algorithm with a slow growth rate to keep searches responsive (4).
2. **Space complexity** – measures how much main memory the search algorithm and its data structure require as n grows, including overhead such as indexes or hash tables. A faster search may need extra memory (e.g. a hash-based index of product names), so EasyShop must check that memory usage remains affordable when the inventory becomes much larger (4).

---

## 2. Empirical Analysis（p6–11, p14–19）

### 2.1 做法（p7, p14–17）— Tutorial 4 Q2
1. 把两个竞争的 algorithm **实作出来**并执行；
2. 收集各自完成所需的 **running time**；
3. 对两者做**多次**实验（多种 input 大小）；
4. 计算并比较**平均** running time。

Java 计时（p15）：
```java
long startTime = System.currentTimeMillis();
/* (run the algorithm) */
long endTime = System.currentTimeMillis();
long elapsed = endTime - startTime;
```
极快的操作可用 `System.nanoTime()`（奈秒）。结果可以画成 (n, t) 的散布图，再用统计找出最适合的函数（p16–17）。

### 2.2 为什么 comparative timing 很难（p8–10）— Tutorial 4 Q3, Q4

实验误差来自无法控制的因素：
1. **System load**
2. **Programming language used**
3. **Compiler efficiency**

另外还有 **programmer bias**（p9）：程序员的实作方式会反映在时间上；其中一个程序可能被做了比较多 **code tuning**。

**Code tuning（p10）**：当两个程序的执行时间只差一个**常数倍**、不管 input 多大都一样（也就是 **growth rate 相同**）时，code tuning 的差异就可能决定谁比较快。

**Simulation（p11）**——Tutorial 4 Q5：另一种分析方法，用程序**模拟**问题并执行得到结果；和"比较两个竞争程序"不同，simulation 的目的是分析**困难问题**的 algorithm。

### 2.3 Experimental analysis 的挑战（p18）
1. 两个 algorithm 的实验时间**很难直接比较**，除非在**相同硬体和软件环境**下做；
2. 只能测**有限的 test input**，没测到的 input（可能很重要）就被漏掉；
3. Algorithm 必须**完整实作**才能测。

**所以我们想要的分析方法（p19）**：与硬体/软件环境无关、只看 algorithm 的高阶描述（不用实作）、考虑**所有可能的 input**。

---

## 3. Counting Primitive Operations（p20–26）⭐

不做实验，改**数 primitive operations**（p22）。Primitive operations 例子：

| Primitive operation | 例子 |
|---|---|
| Assigning a value to a variable | `sum = 0` |
| Following an object reference | `node.next` |
| Performing an arithmetic operation | `sum + i` |
| Comparing two numbers | `a > b` |
| Accessing a single element of an array by index | `A[i]` |
| Calling a method | `max(a, b)` |
| Returning from a method | `return x` |

不去算每个 operation 实际要几奈秒，只数**执行了几个** operation，记为 t；t 和实际执行时间**成正比**（p23）。再把 t 写成 input size 的函数 **f(n)**（p24）。

Average case 很难（要定义 input 的机率分布，p25）；**worst case 容易得多**，只要找出最坏的 input，而且通常能引导出更好的 algorithm（p26）。

> 💬 **答题句 (EN)** — *Instead of timing programs, we count the primitive operations an algorithm performs — such as assigning a value to a variable, performing an arithmetic operation, comparing two numbers, accessing an array element by index, calling and returning from a method — and express the count as a function f(n) of the input size; this count is proportional to the actual running time.*

### ✅ 满分答法 — May 2025 Q2b (3 marks)
*Indicate THREE examples of counting primitive operations used to assess running time without experimental analysis.*

1. Assigning a value to a variable, e.g. `sum = 0` (1)
2. Performing an arithmetic operation, e.g. `sum + i` (1)
3. Comparing two numbers, e.g. `if (a > b)` (1)

*（其他：following an object reference、accessing an array element by index、calling a method、returning from a method。Tutorial 4 Q6 要四个，就多写一个。）*

---

## 4. 例子：计算 1 + 2 + … + n 的三个 Algorithm（p27–34）

有用公式（p27）：
```
1 + 2 + … + n       = n(n + 1) / 2
1 + 2 + … + (n − 1) = n(n − 1) / 2
```

**Fig 4.1 — 三个 algorithm**
```
Algorithm A            Algorithm B                  Algorithm C
sum = 0                sum = 0                      sum = n * (n + 1) / 2
for i = 1 to n         for i = 1 to n
   sum = sum + i       {  for j = 1 to i
                             sum = sum + 1
                       }
```

**逐行数（只数 assignment 和 arithmetic；for loop 本身的 operation 忽略）**

Algorithm A（p30）：
```
Total = assignments + additions
      = (1 for "sum = 0" + n in loop body) + (n additions)
      = (1 + n) + n
      = 2n + 1
```

Algorithm B（p31）：内层 loop 的 body 执行几次？

| i | j 的范围 | body 执行次数 |
|---|---|---|
| 1 | 1..1 | 1 |
| 2 | 1..2 | 2 |
| 3 | 1..3 | 3 |
| … | … | … |
| n | 1..n | n |
| **合计** | | **1 + 2 + … + n = n(n+1)/2** |

```
Total = (1 + n(n+1)/2) assignments + n(n+1)/2 additions
      = 1 + n(n+1)
      = n² + n + 1
```

Algorithm C（p32）：1 assignment + 1 multiplication + 1 addition + 1 division = **4**。

**Fig 4.2 — 汇总**

| | Algorithm A | Algorithm B | Algorithm C |
|---|---|---|---|
| Assignments | n + 1 | 1 + n(n+1)/2 | 1 |
| Additions | n | n(n+1)/2 | 1 |
| Multiplications | | | 1 |
| Divisions | | | 1 |
| **Total** | **2n + 1** | **n² + n + 1** | **4** |

用程序实际数过（n = 1, 2, 3, 5, 10）：A = 3, 5, 7, 11, 21；B = 3, 7, 13, 31, 111；C 永远 4——和公式完全吻合。

**Fig 4.3 的图**（operations vs n，n = 1 到 5）画的就是下面这些点：

| n | 1 | 2 | 3 | 4 | 5 |
|---|---|---|---|---|---|
| A = 2n + 1（直线） | 3 | 5 | 7 | 9 | 11 |
| B = n² + n + 1（向上弯的曲线） | 3 | 7 | 13 | 21 | 31 |
| C = 4（水平线） | 4 | 4 | 4 | 4 | 4 |

*看这张图要注意：n = 1 时 A = B = 3，甚至比 C 少；从 n = 2 起 C 最少，而 B 越来越快地往上冲——n 小时差别不大，n 变大后差距越来越大，这就是为什么要看 growth rate。*

结论（p34）：实际时间算不出来，改用**问题大小的函数**表示时间需求；问题变大某倍数，函数值也变大相应倍数；用 **Big-O** 表示 → A 是 O(n)，B 是 O(n²)，C 是 O(1)（见 Ch5）。

---

## 5. Space Complexity（p35–39）

- 空间是另一个程序员关心的资源；即使记忆体越来越大，**可用的磁碟或主记忆体仍然可能是重要限制**（p36）。
- 分析方法和时间类似；**但时间需求通常针对操作资料结构的 algorithm，空间需求通常针对资料结构本身**；asymptotic analysis 完全适用（p37）。
- **Overhead**（p38）：为了有效存取，除了资料本身还要存额外资讯，例如 **linked list 每个 node 要存指向下一个的 pointer**。这些额外资讯就是 overhead。
- 理想上 overhead 越少越好，同时又要最大化存取效率——在这两个对立目标间取得平衡，正是研究 data structure 有趣的地方（p39）。

---

## 6. Space/Time Trade-off（p40–50）⭐

### 6.1 原则（p40–41）— Tutorial 4 Q7

**Space/time trade-off principle**：*one can often achieve a reduction in time if one is willing to sacrifice space, or vice versa.*

两个方向：
- **省空间、花时间**：把资讯 **packing / encoding**（压缩），用的时候要 **unpacking / decoding**，要多花时间 → 空间少、跑较慢。
- **花空间、省时间**：**pre-store results（预存结果）**或重新组织资讯，让执行更快，代价是更多储存空间。

通常两者的变化都是**常数倍**（p41）。

> 💬 **答题句 (EN)** — *The space/time trade-off principle states that the running time of an algorithm can often be reduced by using more memory, or its memory usage reduced at the cost of more running time — for example, pre-storing results in a lookup table makes computation faster but uses extra space.*

### 6.2 例子 1：Lookup table（p42–44）— Tutorial 4 Q8

Lookup table **预先存好**一个函数的值，不用每次都重算。
- **12! 是 32-bit int 能存的最大 factorial**（12! = 479,001,600 ≤ 2,147,483,647；13! = 6,227,020,800 已经超过——程序验证过）。
- 如果程序常常算 factorial，预先把 0! 到 12! 这 13 个值存在表里，要用时**直接查**，比每次重算快得多；n > 12 反正也放不进 int。
- 也可以存昂贵函数的**近似值**，例如 sine/cosine 按整数角度存一张表。
- 注意：**建表本身要花时间**；程序要**常用**这张表，建表才值得。

### 6.3 例子 2：Binary sort（p45–48）

特殊情况：n 个整数正好是 0 到 n − 1 的一个排列（permutation）。

**版本 1——两个 array（快，但空间 2n）**
```java
for (i = 0; i < n; i++)
    B[A[i]] = A[i];          // 每个值直接放到它自己的位置
```

**版本 2——in place（省一半空间，但约慢两倍）**
```java
for (i = 0; i < n; i++)
    while (A[i] != i)        // 把 A[i] 和 A[A[i]] 交换，直到 A[i] 就是 i
        DSutil.swap(A, i, A[i]);
```

执行 A = [3, 0, 4, 1, 2]：版本 1 得到 B = [0, 1, 2, 3, 4]；版本 2 经过 3 次 swap 后 A = [0, 1, 2, 3, 4]（程序验证）。p48：第二个版本**通常约慢一倍，但只需要一半的空间**。

### 6.4 Disk-based space/time trade-off（p49–50）— Tutorial 4 Q9

处理**存在磁碟**上的资料时，原则**几乎相反**：**磁碟储存需求越小，程序跑得越快**。因为从磁碟读资料的时间远大于计算时间，解压缩多花的一点计算，比少读磁碟省下的时间少得多。（不是所有情况都成立，但设计处理磁碟资料的程序时要记得。）

### ✅ 满分答法 — Oct 2025 Q1c(i) (2 marks)
*Define the principle of space/time trade-off in the context of algorithm development.*

The space/time trade-off principle states that an algorithm's running time can often be reduced by using more memory space (e.g. storing extra or pre-computed data) (1), or conversely its memory requirement can be reduced at the cost of a longer running time (e.g. packing/encoding data that must be decoded when used) (1).

### ✅ 满分答法 — Oct 2025 Q1c(ii) (3 marks)
*Provide an example of a space/time trade-off in algorithm design.*

A **lookup table** for factorials (1): instead of computing n! with a loop every time it is needed, the program pre-computes the 13 values 0! to 12! (the largest factorials that fit in a 32-bit int) once and stores them in an array (1). Each later request is answered by a single array access in O(1) time instead of O(n) multiplications, so time is saved at the cost of the extra memory used by the table; this is worthwhile only if factorials are needed often enough to pay for building the table (1).

*（另一个好例子：Binary sort——用第二个 array B 让排序只需一次 pass，比 in-place 版本快约一倍，但空间变成两倍。或 Ch2 的 DP：用 fibresult[] 存答案，把指数时间降为线性。）*

---

## Closing the loop

回到 EasyShop 的三个问题：新搜寻会不会变慢？——数 primitive operations 得到 f(n)，看 time complexity。要不要建 index？——那是 space complexity，用空间换时间的 trade-off。先写好两个版本跑跑看？——可以参考，但 system load、语言、compiler、programmer bias、有限的 test input 都会干扰，所以真正的比较要靠下一章的 **asymptotic notation**。

---

## ⚠️ Where the slides mislead

| Slide | 说法 | 更准确的理解 |
|---|---|---|
| p5 | "the number of instruction getting executed or the number of instruction residing in the algorithm will decide the space complexity" | 空间主要由**资料**（input、额外 array、recursion stack）决定，不只是指令数；照 slide 写可以，最好补一句"and the data it stores" |
| p31 | Algorithm B 第一行又写 "n assignments in for loop body" | 是抄 A 的句子；B 的 body 实际执行 n(n+1)/2 次，下面的式子才对 |
| p33 | 图说写 "Fig. 2.3 … in Fig. 9.1" | 是教科书编号没改；指的就是本章 Fig 4.1 的三个 algorithm |
| p13 vs p4 | "time complexity is usually more important" | 一般如此；但记忆体受限的装置（手机、嵌入式）空间可能更关键 |

---

## Term table

| English | 中文 | 一句话说明 |
|---|---|---|
| Time complexity | 时间复杂度 | 执行时间随 n 的成长 |
| Space complexity | 空间复杂度 | 所需记忆体随 n 的成长 |
| Empirical analysis | 实证分析 | 实作后实际计时 |
| System load | 系统负载 | 同时执行的其他工作 |
| Programmer bias | 程序员偏差 | 实作方式影响计时结果 |
| Code tuning | 程序调校 | 针对常数倍的优化 |
| Simulation | 模拟 | 用程序模拟困难问题来分析 |
| Primitive operation | 基本操作 | 赋值、算术、比较、索引、呼叫、回传 |
| Overhead | 额外负担 | 资料以外为了存取而存的资讯 |
| Space/time trade-off | 空间/时间取舍 | 用空间换时间，或反之 |
| Lookup table | 查找表 | 预存函数值，查表代替计算 |
| Packing / Encoding | 压缩 / 编码 | 省空间但使用时要解码 |
| In-place | 原地 | 不用额外 array |

---

## Cheat sheet

- **Time complexity**：执行时间，数 elementary steps，Big-O，通常 worst case。
- **Space complexity**：main memory 用量；资料 + overhead。
- **Empirical steps**：implement both → collect running time → repeat many times → compare averages。
- **Timing 难**：system load · language · compiler efficiency · programmer bias · code tuning。
- **Experimental 挑战**：同环境才能比 · 有限 test input · 必须完整实作。
- **Primitive operations**：assign · follow reference · arithmetic · compare · array index · call · return。
- **1+…+n = n(n+1)/2**。A = 2n+1 → O(n)；B = n²+n+1 → O(n²)；C = 4 → O(1)。
- **Overhead**：e.g. linked list pointer。
- **Trade-off**：less time ↔ more space。**Lookup table**（12! 最大 int）· **Binary sort**（2 arrays 快一倍 / in-place 省一半）· **Disk-based**：越小越快（读磁碟最慢）。

---

## Practice (answers included)

### A. MCQ
1. Which is **not** a primitive operation? (a) comparing two numbers (b) calling a method (c) sorting an array (d) accessing A[i]
2. Algorithm B above performs how many operations when n = 4? (a) 9 (b) 17 (c) 21 (d) 16
3. The disk-based space/time trade-off says: (a) more disk space → faster (b) smaller disk storage → faster (c) disk speed equals memory speed (d) never compress
4. Which factor is **not** a source of experimental error in timing programs? (a) system load (b) compiler efficiency (c) growth rate (d) programming language
5. A lookup table is an example of: (a) saving space by spending time (b) saving time by spending space (c) empirical analysis (d) simulation

**Answers:** 1 (c) — sorting is made of many primitive operations · 2 (c) — 16 + 4 + 1 = 21 · 3 (b) · 4 (c) · 5 (b)

### B. Short answer
**B1. Describe the empirical approach to comparing two algorithms. (4 marks)**
Implement both algorithms, run them and record the running time of each, repeat the experiment many times on inputs of different sizes, then calculate and compare the average running times.

**B2. Explain overhead in space complexity with an example. (2 marks)**
Overhead is information stored in addition to the actual data so the data can be accessed efficiently, e.g. each node of a linked list stores a reference to the next node.

**B3. How does code tuning affect two programs with equal growth rates? (2 marks)**
When their running times differ only by a constant factor regardless of input size, the program that received more code tuning can run faster, so the timing difference reflects tuning, not a better algorithm.

### C. Application
**C1. Count the primitive operations (assignments + additions only, ignore loop control) and give the Big-O. (4 marks)**
```
total = 0
for i = 1 to n
    for j = 1 to n
        total = total + 1
```
Body runs n × n = n² times: n² assignments + n² additions, plus 1 initial assignment → **2n² + 1 → O(n²)**.

**C2. A game needs sin(x) for whole-number angles 0–359 thousands of times per second. Suggest a space/time trade-off. (3 marks)**
Pre-compute sin(0°) … sin(359°) once into a 360-element lookup table; each frame then reads table[x] in O(1) instead of calling the expensive sine function. The table costs 360 extra values of memory but saves a large amount of computation because it is used very often.

**C3. For Algorithm A and Algorithm C in Fig 4.1, at what n does C start to use fewer operations than A? (2 marks)**
A = 2n + 1, C = 4. For n = 1: A = 3 < 4. For n = 2: A = 5 > 4. So C uses fewer operations from **n = 2** onward.

### D. Thinking
**D1. "Our algorithm runs in 0.3 s on our laptop, so it is efficient." Criticise this claim. (4 marks)**
The measurement depends on the laptop's hardware and current system load, the programming language and compiler, and the particular test inputs; it says nothing about how time grows for larger inputs or other inputs. A proper claim needs the time complexity (e.g. O(n log n)) derived from counting operations, especially for the worst case.

**D2. When would you choose the in-place binary sort over the two-array version? (2 marks)**
When memory is limited (e.g. very large n or a memory-constrained device), because the in-place version needs only half the space even though it takes about twice as long.

---

## Slide index

| Note section | Slides |
|---|---|
| 1 Two complexities | p3–5, p13, p21 |
| 2 Empirical analysis | p6–11, p14–19（图 p15） |
| 3 Counting primitive operations | p22–26 |
| 4 Three algorithms for 1+…+n | p27–34（图 p28, p29, p33） |
| 5 Space complexity | p35–39 |
| 6 Space/time trade-off | p40–50 |

## Links to other chapters
- **Ch2**：DP 的 fibresult[] = 用空间换时间
- **Ch3**：running time analysis、best/worst/average
- **Ch5**：把 2n+1、n²+n+1、4 写成 O(n)、O(n²)、O(1)
- **Ch6**：HashSet 的 load factor 也是 space/time trade-off
