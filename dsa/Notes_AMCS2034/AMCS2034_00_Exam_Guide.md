# AMCS2034 Introduction to Data Structures and Algorithms — 考前总攻略

> 先读这一页。它告诉你：考卷长什么样、每一小题出自哪一章、哪几章最值钱、slide 哪里讲错，以及 **SPC 考完（1/10）到 DSA 考试（5/10）这三天怎么复习**。
> 各章笔记：讲解用中文，**术语保持英文**，重要概念后面有 **💬 答题句 (EN)**，每一道 past year 小题都有 **✅ 满分答法**；所有 Java 程序都用 javac 25 实际编译执行过。

---

## 1. 考卷格式

- **2 小时，4 题，全部要答，每题 25 分**（共 100 分）。每题约 30 分钟。
- 读过的四份考卷：

| 档案 | 考试 | 对象 |
|---|---|---|
| `AMCS2034 (2).pdf` | **Oct 2024** | Diploma in Software Engineering |
| `AMCS2034 (1).pdf` | **May 2025** | Diploma in Software Engineering |
| `QP - AMCS2034.pdf` | **Oct 2025**（扫描档） | Diploma in Computer Science |
| `AMCS2034.pdf` | **Dec 2025**（扫描档） | Diploma in Computer Science + Software Engineering |

- **最近两份（Oct 2025、Dec 2025）结构几乎一样**，也是你最可能遇到的格式：

| 题号 | 近两份固定考 | 章 |
|---|---|---|
| **Q1** | 情境题（EasyShop / CampusMart）：**TWO types of complexity (8)** + **Big-O 表格计算 show working (9)** + trade-off / characteristics / running time | Ch1, Ch3, **Ch4, Ch5** |
| **Q2** | **Set**：operations / advantages + **HashSet vs LinkedHashSet (vs TreeSet)** + **Java HashSet 程序 (16–17)** | **Ch6** |
| **Q3** | **Data structures**：array vs linked list、static vs dynamic、stack vs queue、tree vs graph + **binary tree traversal (7–9)** | **Ch7**, Ch8 |
| **Q4** | **Graph**：**adjacency matrix + list (10–14)** + applications / types / traversals + garbage collection | **Ch9**, Ch8 |

- 较旧的两份（Oct 2024、May 2025）题目分布较散：Q1 有 Ch1 定义和 Ch2 iterator/DP，Q2 有 Ch3 分析，Q3 是 Set + linked list/stack，Q4 是 graph + GC。
- 常见 command verbs：*Identify, Explain, Describe, Differentiate / Distinguish, Compare and contrast, Examine, Apply, Relate, Determine, Show your working, Develop, Write a (complete) Java program, With the help of diagrams, Classify, Recommend … justify*。
- 👉 **情境题（ride-sharing、Tess Sdn Bhd、e-commerce、CampusMart、EasyShop、BrightNote）：每一点都要连回 case**，只写定义拿不到 "relate / apply" 的分。

---

## 2. 四份考卷出题地图（每一小题 → 满分答法在哪）

### Oct 2024
| 小题 | 分 | 内容 | 满分答法 |
|---|---|---|---|
| Q1a(i) | 9 | Ride-sharing：THREE challenges | **Ch1 §1** |
| Q1a(ii) | 6 | TWO data structure operations | Ch1 §5 |
| Q1b(i) | 2 | Iterators in inventory system | Ch2 §4 |
| Q1b(ii) | 4 | **Write TWO iterator methods** | **Ch2 §4** |
| Q1b(iii) | 4 | DP vs divide & conquer | Ch2 §6 |
| Q2a(i) | 6 | TWO concerns in algorithm analysis | Ch3 §1 |
| Q2a(ii) | 3 | Running time analysis ↔ algorithm selection | Ch3 §8 |
| Q2a(iii) | 4 | TWO problems with running time analysis | Ch3 §3 |
| Q2a(iv) | 9 | **THREE scenarios (best/worst/average)** | **Ch3 §5** |
| Q2b | 3 | Linear vs binary search | Ch3 §7 |
| Q3a | 3 | THREE concrete Set classes | Ch6 §1 |
| Q3b | 15 | **HashSet countries program** | **Ch6 §7** |
| Q3c | 3 | HashSet vs TreeSet ordering | Ch6 §6 |
| Q3d | 4 | TWO types of data structure (static/dynamic) | Ch7 §2 |
| Q4a(i)(ii) | 9+9 | **Adjacency list + matrix** | **Ch9 §7** |
| Q4a(iii) | 3 | Java array of vertices | Ch9 §7 |
| Q4b | 4 | Classify DFS / BFS | Ch9 §9 |

### May 2025
| 小题 | 分 | 内容 | 满分答法 |
|---|---|---|---|
| Q1a | 9 | THREE data structure operations | Ch1 §5 |
| Q1b | 6 | TWO categories of non-primitive DS | Ch1 §4 |
| Q1c | 6 | TWO components of pseudocode | Ch1 §7 |
| Q1d | 4 | TWO characteristics of algorithm | Ch1 §6 |
| Q2a | 4 | hasNext() vs next() | Ch2 §2 |
| Q2b | 3 | THREE primitive operations | Ch4 §3 |
| Q2c | 9 | **Apply THREE types of analysis** | **Ch3 §5** |
| Q2d | 9 | **THREE asymptotic notations** | **Ch5 §2** |
| Q3a(i)–(iii) | 2+3+2 | HashSet code: duplicates, output, uniqueness | Ch6 §2 |
| Q3c | 4 | HashSet vs LinkedHashSet ordering | Ch6 §6 |
| Q3d | 6 | THREE disadvantages of linked list | Ch7 §3 |
| Q3e | 8 | **FOUR stack operations** | **Ch7 §5** |
| Q4a | 6 | THREE graph applications | Ch9 §1 |
| Q4b | 4 | Directed vs undirected | Ch9 §2 |
| Q4c | 6 | TWO traversal methods | Ch9 §9 |
| Q4d | 4 | TWO applications of each traversal | Ch9 §9 |
| Q4e | 5 | **GC: TWO advantages + ONE type** | **Ch8 §7** |

*（May 2025 没有 Q3b，原卷就是 a → c。）*

### Oct 2025
| 小题 | 分 | 内容 | 满分答法 |
|---|---|---|---|
| Q1a | 8 | **TWO types of complexity** | **Ch4 §1** |
| Q1b(i) | 9 | **Big-O for A, B, C with working** | **Ch5 §3** |
| Q1b(ii) | 3 | Recommend algorithm | Ch5 §3 |
| Q1c(i)(ii) | 2+3 | Space/time trade-off: define + example | Ch4 §6 |
| Q2a | 3 | THREE Set operations | Ch6 §1 |
| Q2b | 6 | **HashSet vs LinkedHashSet vs TreeSet** | **Ch6 §6** |
| Q2c | 16 | **Common items program** | **Ch6 §7** |
| Q3a(i) | 4 | Static vs dynamic data structure | Ch7 §2 |
| Q3a(ii) | 4 | Static vs dynamic memory allocation | Ch8 §8 |
| Q3a(iii) | 4 | Queue vs stack | Ch7 §5 |
| Q3b | 4.5+4.5 | **In-order + post-order** | **Ch7 §6** |
| Q3c | 4 | TWO advantages of linked list | Ch7 §3 |
| Q4a | 5+5 | **Adjacency matrix + list** | **Ch9 §7** |
| Q4b | 6 | THREE types of graphs with diagrams | Ch9 §3 |
| Q4c | 3 | THREE graph applications | Ch9 §1 |
| Q4d | 6 | TWO traversal algorithms | Ch9 §9 |

### Dec 2025
| 小题 | 分 | 内容 | 满分答法 |
|---|---|---|---|
| Q1a | 6 | THREE algorithm characteristics (CampusMart) | Ch1 §6 |
| Q1b | 8 | **TWO types of complexity** | **Ch4 §1** |
| Q1c | 2 | Running time analysis ↔ selection | Ch3 §8 |
| Q1d | 9 | **Big-O for P, Q, R with working** | **Ch5 §3** |
| Q2a | 4 | TWO advantages of Set over array | Ch6 §1 |
| Q2b | 4 | HashSet vs LinkedHashSet (ordering + efficiency) | Ch6 §6 |
| Q2c | 17 | **Unique-to-CounterA program** | **Ch6 §7** |
| Q3a | 6 | Array vs linked list (memory, insert/delete, access) | Ch7 §3 |
| Q3b | 6 | Stack or queue for task scheduler | Ch7 §5 |
| Q3c | 6 | Tree or graph for social network | Ch7 §7 |
| Q3d | 3.5+3.5 | **Pre-order + post-order** | **Ch7 §6** |
| Q4a | 7+7 | **Adjacency matrix + list (self-loops!)** | **Ch9 §7** |
| Q4b | 6 | THREE graph applications | Ch9 §1 |
| Q4c | 5 | **GC: TWO advantages + ONE type** | **Ch8 §7** |

✅ **四份考卷的每一个小题都有对应的满分答法。**

---

## 3. 每章值多少分

| 章 | Oct 24 | May 25 | Oct 25 | Dec 25 | **平均** | 优先级 |
|---|---|---|---|---|---|---|
| **Ch9** Graphs | 25 | 20 | 25 | 20 | **22.5** | ★★★★★ |
| **Ch6** Sets | 21 | 11 | 25 | 25 | **20.5** | ★★★★★ |
| **Ch7** Types of DS | 4 | 14 | 21 | 25 | **16** | ★★★★★ |
| Ch1 Basic terminology | 15 | 25 | 0 | 6 | 11.5 | ★★★ |
| Ch3 Efficiency | 25 | 9 | 0 | 2 | 9 | ★★★ |
| **Ch5** Asymptotic | 0 | 9 | 12 | 9 | 7.5 | ★★★★（近三份都有 9 分） |
| **Ch4** Time & space | 0 | 3 | 13 | 8 | 6 | ★★★★（近两份都有 8 分） |
| Ch2 Iterators / DP | 10 | 4 | 0 | 0 | 3.5 | ★★ |
| Ch8 Memory | 0 | 5 | 4 | 5 | 3.5 | ★★★（GC 5 分题很固定） |

**Ch6 + Ch7 + Ch9 ≈ 六成分数。** 近两份考卷里 Ch4 + Ch5 的 Q1 也很固定（约 17–25 分）。

**"一定要会写"的十个答案**（覆盖近两份考卷大约八成分数）：
1. **Two types of complexity**（time vs space，套 case）— Ch4 §1
2. **Big-O 三步 working**（dominant term → ignore smaller terms → ignore coefficient）+ 推荐 — Ch5 §3
3. **Space/time trade-off** 定义 + lookup table 例子 — Ch4 §6
4. **Set 三个 operation** + Set vs array 优点 — Ch6 §1
5. **HashSet / LinkedHashSet / TreeSet**：ordering + performance 表 — Ch6 §6
6. **HashSet 程序模板**（两个 set、copy、retainAll / removeAll、loop 印、size 总数）— Ch6 §7
7. **Array vs linked list** 表 + static vs dynamic 表 — Ch7 §2–3
8. **Stack vs queue**（四个 stack operation；scheduler 用 queue）— Ch7 §5
9. **Binary tree traversal**（pre / in / post 展开法）— Ch7 §6
10. **Adjacency matrix + list 填表法** + DFS/BFS 说明与应用 + GC 两优点一类型 — Ch9 §7–9、Ch8 §7

---

## 4. 复习计划：2/10 – 4/10（SPC 1/10 考完后）

| 日期 | 时间 | 做什么 |
|---|---|---|
| **1/10 晚上** | 30 分钟 | 读这份 Exam Guide；把第 3 节的十个答案标出来。**不要开始新内容**，SPC 刚考完先休息 |
| **2/10** | 09:00–11:30 | **Ch9 Graphs**：§2 directed/undirected、§3 术语表、§5–7 **自己把三份考卷的 matrix + list 手写一遍**，再对答案 |
| | 11:30–12:30 | **Ch9** §8–9 DFS/BFS：手追踪 slide 的例子；背 applications；Oct 2024 Q4b 分类题 |
| | 14:00–16:30 | **Ch6 Sets**：§1–3 operations、§6 比较表（背）、§7 **闭卷手写两个程序**（unique / common），再对照 |
| | 16:30–18:00 | **Ch7** §1–3 static/dynamic、array vs linked list（两张表） |
| | 20:00–21:00 | **Ch7** §6 traversal：把四棵树的 pre/in/post 都自己做一次 |
| **3/10** | 09:00–10:30 | **Ch7** §4–5 linked list insert/delete、stack/queue（四个 operation、scheduler 题） |
| | 10:30–12:00 | **Ch4** §1 two complexities（背 8 分写法）、§3 primitive operations、§6 trade-off |
| | 13:00–14:30 | **Ch5** §2 O/Ω/Θ、§3 **Big-O working**：Dec 2025 P/Q/R、Oct 2025 A/B/C 手写 |
| | 14:30–16:00 | **Ch3** §5 best/worst/average（9 分写法）、§7 linear vs binary search |
| | 16:00–17:00 | **Ch8** §7 GC（两优点一类型）、§8 static vs dynamic allocation 表 |
| | 20:00–22:00 | **模拟考 1**：Dec 2025 卷，计时 2 小时，闭卷手写，对照满分答法 |
| **4/10** | 09:00–10:30 | **Ch1** §1、§4–7（三个 problem、operations、characteristics、pseudocode components） |
| | 10:30–11:30 | **Ch2** §2 hasNext/next、§4 iterator 程序、§6 DP vs D&C |
| | 13:00–15:00 | **模拟考 2**：Oct 2025 卷，计时 2 小时 |
| | 15:00–17:00 | 订正两份模拟考的错；重做错的题 |
| | 20:00–21:00 | 读每章的 **Cheat sheet**；最后再手写一次 HashSet 程序和一个 adjacency matrix；早点睡 |
| **5/10 早上** | 30 分钟 | 只看 Cheat sheet 和第 5 节答题技巧 |

*如果时间不够，砍 Ch1 和 Ch2 的细节，**不要**砍 Ch6、Ch7、Ch9、Ch5 §3。*

---

## 5. 答题技巧

1. **看分数决定写多少**：1 分 ≈ 1 个有内容的重点。"Explain THREE … (9 marks)" = 三点 × 3 分（名称 + 解释 + 例子 / 连回 case）。
2. **Big-O "show your working"**：每个函数写三行——*dominant term*、*ignoring smaller terms*、*ignoring the coefficient ⇒ O(·)*。有时间再加一行 c、N 的检查。
3. **Java 程序题（15–17 分）**：
   - 一定写 `import java.util.HashSet; import java.util.Set;`（或 `import java.util.*;`）、`public class`、`main`。
   - 用 `Set<String> x = new HashSet<>();`；每个 item 一行 `add`。
   - 做 retainAll / removeAll 前**先复制**：`new HashSet<>(set1)`。
   - **for-each loop 印每一项**，最后 **`size()` 印总数**。
   - 不记得 method？用 `for` + `contains()` 自己判断也能拿大部分分。
4. **Adjacency matrix / list**：
   - 先在考卷的图上**把每条 edge 编号**，列出 edge 清单再填表。
   - **row = from，column = to**；有向图只填箭头方向；**两个箭头 = 两格**；**self-loop = 对角线 1**；其他格一律写 0。
   - 最后数 1 的个数 = edge 数。
5. **Tree traversal**：每个位置约 0.5 分，一个放错会连带扣分。用 "pre(x) = x, pre(左), pre(右)" 的展开法，**特别注意只有一个孩子的 node**。
6. **Compare / Differentiate / Distinguish**：用**表格**，每一列一个 aspect，两边都要写；"compare and contrast" 先写一句相同点。
7. **"Justify / Recommend"**：先给答案（"A queue is more suitable" / "Algorithm C"），再给 2–3 个理由，每个理由连回 case。
8. **"With the help of diagrams"**：一定要画（graph types、DFS/BFS 树），图本身有分。
9. **情境题**：不要写 "a company"，写 "CampusMart's book records"、"EasyShop's products"、"BrightNote's counters"。

---

## 6. Slide 的主要错误（考试时注意）

| 章 | Slide | 问题 | 怎么写 |
|---|---|---|---|
| Ch2 | p24 | NameRepository 程序被截断，缺 hasNext / next / Container | 用 Ch2 §4.2 的完整版 |
| Ch2 | Tut 3 Q4 | "THREE methods of the **Iterable** interface" | 实际是 **Iterator** 的 hasNext / next / remove；写的时候加一句说明 |
| Ch3 | p27 | 标题 "Linear Search" 但内容是找最大值 | 知道它是 find-max，n − 1 comparisons |
| Ch5 | p14 | "Ω = lower bound **or best case**" | Ω 是 bound，不是 case；照 slide 写 "lower bound / minimum time" |
| Ch6 | p12, p15 | HashSet 输出顺序 | 新版 Java 顺序不同；写 "no particular order" |
| Ch6 | p15 | retainAll 的输出标成 "removing common elements"，结果 [] | retainAll **保留**共同元素；[] 是因为前一行已 removeAll |
| Ch6 | p21 | `for (Object element: set) element.toLowerCase()` | **编译错误**，要用 `String element` |
| Ch7 | p7, p11, p27 | Static 没有 overflow 问题、dynamic 会 overflow/underflow | 与一般教科书相反；**考试照 slide 写** |
| Ch8 | p34 | Heap "要自己释放"、"no size limit" | C/C++ 如此；Java 由 GC 管；heap 有上限 |
| Ch8 | p35 | Static allocation "uses stack" | 照 slide 写不扣分；实际在 static data 区 |
| Ch9 | **p46** | **12 城市 adjacency matrix 两格错**（Los Angeles、Atlanta 两列） | 无向图矩阵**一定对称**；用 p50 的 list 为准 |
| Ch9 | p17 vs Ch7 p79 | Flight network 一处说 undirected、一处说 directed | 举例时用 one-way road（directed）/ railway（undirected） |

---

## 7. 资料与限制

- **读过**：Lecture 1–9（全部页数）、Tutorial 1–9、四份 past year。两份扫描考卷逐页看过；所有考卷的图（HashSet 程序、三张 graph、两棵 binary tree、DFS/BFS 分类图）都放大核对过。
- **讲义里的图**：用逐页缩图的方式看过所有有图片或文字很少的页面（分类树、iterator UML、Fibonacci 递回树、growth-rate 表、Big-O 定义图、linked list 插入删除图、doubly / header node、queue / stack、stock span、traversal 树、记忆体配置图、A/B/C 程序单元图、所有 graph 术语图、adjacency list 图、DFS/BFS 步骤图），并在笔记中重画或描述。纯装饰图（卡通、背景）没有收录。
- **程序验证**：slide 与考卷的所有 Java 程序（Set 系列、iterator pattern、MagazineList、regex、String、binary sort、linked list 插删）都用 **javac/java 25** 编译执行；Fibonacci 呼叫次数、operation count、所有 tree traversal、DFS/BFS 顺序、stock span、Big-O 的 c/N 常数、四张 graph 的矩阵与列表都用程序算过。
- **缺少 / 无法确认的**：
  - **没有官方 answer scheme**：满分答法是依 slide 内容与分数分配自行撰写；程序题的分数分配是估计。
  - **Garbage collection 的"类型"slide 没讲**：笔记里 generational（minor/major）、Serial、Parallel、G1、ZGC、mark-and-sweep 都标为 *extra*。
  - **Binary search** slide 没有正式讲，笔记 Ch3 §7 为 *extra*。
  - 文件夹只有 **Tutorial 9A**（graph 概念）；若还有 9B（matrix / DFS / BFS 练习），不在资料里——Ch9 的 practice 已补上同类题。
- 标注为 *extra* 的内容都不在 slide 上，考试写了不会错，但以 slide 的说法为主。

---

## 各章笔记

| 文件 | 内容 |
|---|---|
| `AMCS2034_Ch1_Basic_Terminology.md` | 三个 problem、terminology、primitive/non-primitive、linear/non-linear、**6 operations**、**algorithm characteristics**、**pseudocode components** |
| `AMCS2034_Ch2_Algorithms_Iterators_DP.md` | Collections、**hasNext vs next**、design patterns、**iterator pattern 完整程序**、String、**DP vs divide & conquer**、regex |
| `AMCS2034_Ch3_Efficiency_of_Algorithm.md` | Goal of analysis、running time、rate of growth、**best/worst/average**、linear vs **binary search** |
| `AMCS2034_Ch4_Time_and_Space_Complexity.md` | **Time vs space complexity**、empirical analysis、**primitive operations**、Algorithm A/B/C、**space/time trade-off** |
| `AMCS2034_Ch5_Asymptotic_Notations.md` | **O / Ω / Θ**、**Big-O working（考卷两题）**、七个常见函数、growth 表 |
| `AMCS2034_Ch6_Sets.md` | **Union/intersection/difference**、load factor、hashCode、**HashSet/LinkedHashSet/TreeSet**、**三个考卷程序** |
| `AMCS2034_Ch7_Types_of_Data_Structures.md` | **Static/dynamic**、**array vs linked list**、linked list 插删、**stack/queue**、**tree traversal（考卷两棵树）**、tree vs graph |
| `AMCS2034_Ch8_Memory_Allocation.md` | Static / stack / heap、binding、Java stack & heap、**garbage collection**、**static vs dynamic allocation** |
| `AMCS2034_Ch9_Graphs.md` | Applications、**directed vs undirected**、术语、**adjacency matrix & list（考卷三张图）**、**DFS / BFS** |
