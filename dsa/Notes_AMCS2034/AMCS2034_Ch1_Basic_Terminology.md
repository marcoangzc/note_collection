# AMCS2034 — Chapter 1: Basic Terminology — Elementary Data Organization

> Slide 只列定义，不解释为什么要这样分。考卷却很爱把它们放进**情境题**：ride-sharing app 要面对哪三个 challenge、哪两个 operation 最适合、大型系统的 algorithm 要有什么 characteristic。这份笔记用一个 ride-sharing app 贯穿全章，把每个定义变成"答题时能套到 case 上的句子"，并回答 slide 上所有 "What will be the output?" 题。

---

## 0. 一句话总览

**Data structure 决定资料"怎么摆"，algorithm 决定"怎么处理"；摆得好，搜寻就不必一个一个找。这章给你后面八章都会用到的词汇：资料的层级（data → field → record → file）、资料型态（primitive / non-primitive）、data structure 的分类和六个 operation，以及 algorithm 的特性和 pseudocode 写法。**

| Part | 内容 | Slide |
|---|---|---|
| 1 | 为什么要学 DSA：三个常见问题 | p4–6 |
| 2 | Basic terminology（data、data item、entity、field、record、file…） | p8–10 |
| 3 | Primitive vs non-primitive data types | p11–12 |
| 4 | Data structure 定义与分类（linear / non-linear） | p13–20 |
| 5 | Data structure operations（6 个） | p21–24 |
| 6 | Algorithm：定义、characteristics、good algorithm | p25–30 |
| 7 | Pseudocode 与它的 components | p31–42 |
| 8 | 怎么写 algorithm（step 格式） | p43–47 |

🎯 **这章回答的考题**
- **May 2025 Q1a** THREE data structure operations (9)
- **May 2025 Q1b** TWO categories of non-primitive data structures + example (6)
- **May 2025 Q1c** TWO components of pseudocode + example (6)
- **May 2025 Q1d** TWO characteristics of an algorithm + role in efficiency (4)
- **Oct 2024 Q1a(i)** ride-sharing app：THREE challenges (9)
- **Oct 2024 Q1a(ii)** ride-sharing app：TWO data structure operations (6)
- **Dec 2025 Q1a** CampusMart：THREE characteristics of an algorithm for a large-scale system (6)

**Prerequisites**：会一点 Java（variable、if、loop、method）。

---

## Scene：凌晨一点的 ride-sharing app

你在一家类似 Grab 的公司上班。系统里有 **100 万个 driver 的位置**。每一个乘客按下 "Book"，系统就要找出**最近的空车**。

最笨的做法：把 100 万个 driver 从头到尾看一遍。一个乘客还好，但下班尖峰时间**同时有几千个乘客**在按 Book，每个人都要扫一次 100 万笔资料——伺服器再快也会卡住。

问题不在 CPU 不够快，而在**资料摆放的方式**：如果 driver 按地区分好组，你只需要看乘客附近那一组。这就是这门课的起点。

---

## 1. 为什么要学 Data Structures & Algorithms（p4–6）

Slide 说：application 越来越复杂、资料越来越多，现在的 application 都面对**三个常见问题**：

| 问题 | 意思 | 在 ride-sharing app 里 |
|---|---|---|
| **Data Search** | 资料量变大，每次搜寻都要扫过更多 item，搜寻越来越慢（slide 例子：店里 100 万个 item） | 每次找附近司机都要扫全部司机位置 |
| **Processor Speed** | CPU 虽然很快，但资料到了**十亿笔**时，速度仍然不够 | 全国多个城市的 trip 纪录累积到数十亿笔 |
| **Multiple Requests** | 几千个 user **同时**搜寻，就算是快的 server 也会撑不住 | 尖峰时间几千个乘客同时 request |

**解法（p6）**：把资料组织成适当的 data structure，让搜寻时**不需要看全部 item**，几乎可以立刻找到要的资料。

> 💬 **答题句 (EN)** — *As applications become complex and data-rich, they face three common problems: data search becomes slower as data grows, processor speed becomes a limit when data reaches billions of records, and multiple simultaneous requests can overload even a fast server. Organising data in a suitable data structure means not every item has to be searched.*

### ✅ 满分答法 — Oct 2024 Q1a(i) (9 marks)
*Ride-sharing app that connects drivers and passengers in real time across multiple cities. Identify and explain THREE main challenges.*

每个 challenge 3 分：名称 + 解释 + 连到 case。

1. **Data search.** The app must search a very large and growing collection of driver locations, passenger requests and trip records. If the data is not organised, every search has to scan through all records, e.g. checking every driver in every city to find the nearest available driver, so the search becomes slower and slower as the number of drivers grows.
2. **Processor speed.** Although processors are very fast, their speed becomes a limitation when the data grows to millions or billions of records. As the company expands to more cities, the volume of GPS updates and trip histories grows so large that even a high-speed processor cannot match drivers to passengers in real time without an efficient data structure.
3. **Multiple requests.** Thousands of passengers and drivers use the app at the same time, especially during peak hours. Each request triggers searches on the server, so even a fast server can fail or respond slowly when handling so many simultaneous searches. Efficient data structures reduce the work per request so the app stays responsive.

---

## 2. Basic Terminology（p8–10）

这些词是一个**由小到大的层级**。用一个 student record 当例子最清楚：

```
File  (all students of TAR UMT)                     ← collection of records
 └── Record  (one student: Ali)                      ← collection of field values of ONE entity
      ├── Field: StudentID = 2501234                 ← elementary unit = one attribute
      ├── Field: Name = { First: "Ali", Last: "Tan" } ← Name is a GROUP item (has sub-items)
      └── Field: CGPA = 3.79                         ← CGPA is an ELEMENTARY item
```

*看这张图要注意：Group item 能再拆（Name → First/Last），Elementary item 不能再拆（CGPA）。*

| Term | Slide 定义 | 例子 |
|---|---|---|
| **Data** | values or set of values | 3.79、"Ali" |
| **Data Item** | a single unit of values | CGPA 这一个值 |
| **Group Items** | data items that are divided into sub-items | Name → First name, Last name |
| **Elementary Items** | data items that cannot be divided | StudentID、CGPA |
| **Attribute & Entity** | an entity contains attributes (properties) which may be assigned values | Entity = 学生 Ali；attributes = ID、Name、CGPA |
| **Entity Set** | entities with similar attributes | 全部学生 |
| **Field** | a single elementary unit of information representing an attribute of an entity | CGPA 栏位 |
| **Record** | collection of field values of a given entity | Ali 的整行资料 |
| **File** | collection of records of the entities in an entity set | 全校学生档案 |

> 💬 **答题句 (EN)** — *A field is a single elementary unit representing one attribute of an entity; a record is the collection of field values of one entity; a file is the collection of records of all entities in an entity set.*

（Tutorial 1 Q2 a–e 全部可以用上表回答。）

---

## 3. Primitive vs Non-primitive Data Types（p11–12）

| | Primitive | Non-primitive |
|---|---|---|
| Slide 定义 | **directly supported by the machine**；可以直接在上面做运算 | 不能直接对 data item 下指令；由 primitive **组合**而成 |
| 例子 | integer, float, double, character, boolean | array, structure, union, class |
| 大小 | 固定、由语言规定（Java `int` = 4 bytes） | 由程序员决定、可大可小 |
| 存什么 | 一个值 | 一群值 / 一群物件 |

⚠️ Slide p12 的说法（"do not allow any specific instructions to be performed on the data items directly"）很模糊。更好懂的说法：CPU 有指令可以直接把两个 `int` 相加，但没有一个指令能直接"把两个 array 相加"——你要写 loop 或 method 去处理。*Structures / Unions 是 C 语言的概念，Java 用 class 取代。*

> 💬 **答题句 (EN)** — *Primitive data types (int, float, double, char, boolean) are directly supported by the machine and hold a single value; non-primitive data types (arrays, structures, unions, classes) are built from primitive types, hold a group of values, and are manipulated through programmer-defined operations.*

（Tutorial 1 Q3 = 五个 primitive；Q5 = 四个 non-primitive；Q6 = 上表。）

---

## 4. Data Structure：定义与分类（p13–20）

**Data structure** = *an arrangement of data in a computer's memory or even disk storage*（p13）。例子：arrays, linked lists, queues, stacks, binary trees, hash tables。

**Algorithm** 则是**操作**这些 data structure 里资料的方法，例如 searching、sorting（p14）。两者合起来就是程序：

```
   Algorithm  +  Data Structure  =  Program
   (how to process)  (how data is arranged)
```

### 4.1 分类图（p17，考试可以直接画）

```
                     Data Structures
                    /               \
          Primitive DS          Non-primitive DS
                                /              \
                  Linear DS                    Non-linear DS
                  ├── Arrays                   ├── Trees
                  ├── Stacks                   └── Graphs
                  ├── Queues
                  └── Linked list
```

*看这张图要注意：linear / non-linear 是 **non-primitive** 下面的两个分类——这正是 May 2025 Q1b 问的。*

| | Linear | Non-linear |
|---|---|---|
| 排列 | 元素一个接一个，排成**一条线**（p19） | 元素以**阶层 / 网状**排列（p20） |
| 走访 | 一趟可以走完全部 | 不能一趟走完（要分支） |
| 例子 | stack、queue、list（array、linked list） | tree、graph |

⚠️ **两种"两类"不要混**：Ch1 说 data structure 分 **linear / non-linear**；Ch7 p4 又说分 **static / dynamic**。两个都对，分类标准不同（一个看**排列方式**，一个看**大小能否改变**）。看题目怎么问：
- "TWO categories of **non-primitive** data structures" → linear / non-linear（本章）
- "Data structure is a way of storing and organising data **efficiently** … Explain TWO types" → 这句话是 Ch7 p4 的原句 → static / dynamic（见 Ch7）

### ✅ 满分答法 — May 2025 Q1b (6 marks)
*Identify TWO categories of non-primitive data structures, explain each category, and provide ONE example for each.*

1. **Linear data structures** (3) – the data elements are arranged in a linear (sequential) order, where each element is attached to the one before and after it, so all elements can be traversed in a single run. *Example:* a **stack**, where elements are added and removed only from the top (also: array, queue, linked list).
2. **Non-linear data structures** (3) – the data elements are arranged in a hierarchical or network order, where one element can be connected to several other elements, so they cannot be traversed in a single run. *Example:* a **tree**, which represents a hierarchy such as folders and sub-folders (also: graph).

---

## 5. Data Structure Operations（p21–24）

Slide 列了六个 operation：

| Operation | 意思（slide） | Ride-sharing app 例子 |
|---|---|---|
| **Traversing** | accessing each data element **exactly once** so that certain items may be processed | 每晚把所有 trip 走一遍算司机收入 |
| **Searching** | finding the **location** of a data element (key) | 找离乘客最近的空车 |
| **Insertion** | adding a new data element | 新司机上线 → 加入可用司机名单 |
| **Deletion** | removing a data element | 司机接单后 → 从可用名单删除 |
| **Sorting** | arranging elements in a logical order (asc / desc) | 把附近司机按距离排序 |
| **Merging** | combining elements from two or more structures into one | 合并两个城市的司机名单 |

> 💬 **答题句 (EN)** — *Data structure operations include traversing (accessing each element exactly once), searching (finding the location of a key), insertion, deletion, sorting (arranging in logical order) and merging (combining two or more structures into one).*

### ✅ 满分答法 — May 2025 Q1a (9 marks)
*Provide THREE data structure operations that are crucial for solving complex computer science problems.*

每点 3 分：名称 + 定义 + 为什么重要 / 例子。

1. **Searching** – finding the location of a particular data element (the key) in the structure. It is crucial because most problems need to retrieve data quickly, e.g. finding a customer record by ID; a well-organised structure allows the item to be found without checking every element.
2. **Insertion** – adding a new data element into the structure at the correct position. It is crucial because real data keeps growing, e.g. adding a new order to an order list, and the structure must accept new data while keeping its organisation (such as sorted order).
3. **Sorting** – arranging the data elements in a logical order, ascending or descending. It is crucial because sorted data makes other operations faster, e.g. binary search only works on sorted data, and reports are easier to read.

*（也可以写 Traversing / Deletion / Merging，每个都要有"定义 + 为什么重要 + 例子"。）*

### ✅ 满分答法 — Oct 2024 Q1a(ii) (6 marks)
*Explain and relate TWO data structure operations that are suitable to the ride-sharing application.*

1. **Searching** (3) – finding the location of a data element in the structure. The app must constantly search the driver data to find the nearest available driver for each ride request, so an efficient search operation is essential for real-time matching.
2. **Insertion and deletion** (3) – adding and removing data elements. When a driver goes online, the driver is inserted into the list of available drivers; when the driver accepts a trip or goes offline, the driver is deleted from it. These updates happen thousands of times per minute, so they must be fast.

*（另一个好选择：**Sorting** — 把候选司机按距离或评分排序，给乘客最好的配对。）*

p24 另外列了"可以用 data structure 解的问题"：Fibonacci、Knapsack、Tower of Hanoi、Floyd-Warshall、Dijkstra、project scheduling。同一张表在 Ch2 Dynamic Programming 又出现一次。

---

## 6. Algorithm（p25–30）

### 6.1 定义

- *A finite set of instructions which accomplish a particular task*
- *A method or process to solve a problem*
- *Transforms input of a problem to output*

```
   ┌────────┐      ┌───────────────────────────┐      ┌────────┐
   │ Input  │ ───▶ │ Set of rules to obtain     │ ───▶ │ Output │
   └────────┘      │ the expected output from   │      └────────┘
                   │ the given input (Algorithm)│
                   └───────────────────────────┘
          Algorithm = Input + Process + Output
```

*看这张图要注意：algorithm 本身是中间的"规则"，不是 input 或 output。*

### 6.2 Characteristics of an Algorithm（p27）⭐

| Characteristic | 意思 |
|---|---|
| **Unambiguous** | 每一步和它的 input/output 都清楚，**只有一个意思** |
| **Input** | 有 **0 个或以上** well-defined input |
| **Output** | 有 **1 个或以上** well-defined output，而且符合预期 |
| **Finiteness** | 在**有限步**后一定结束 |
| **Feasibility** | 用现有资源就能执行 |
| **Independent** | 步骤独立于任何程序语言 |

> 💬 **答题句 (EN)** — *An algorithm must be unambiguous (each step has only one meaning), have zero or more well-defined inputs and one or more well-defined outputs, be finite (terminate after a finite number of steps), be feasible with available resources, and be independent of any programming language.*

**Good algorithm（p28）**：correct、finite（时间和大小）、terminate、unambiguous、**space and time efficient**。*A program is an instance of an algorithm written in a specific language.*

### ✅ 满分答法 — May 2025 Q1d (4 marks)
*Identify TWO characteristics of an algorithm. For each, determine its role in ensuring the algorithm's efficiency in real-world applications.*

1. **Finiteness** (2) – the algorithm must terminate after a finite number of steps. This ensures that a real-world system, such as an online payment service, always returns a result within a bounded time and never hangs in an endless loop that wastes processor time.
2. **Unambiguous** (2) – every step and its inputs/outputs must be clear and have only one meaning. This ensures the algorithm is implemented correctly and consistently by different programmers, so no time is wasted on wrong or repeated processing.

*（Feasibility 也很好用：只用现有资源就能执行 → 不会因为需要超出现有硬体的资源而无法在真实系统中运作。）*

### ✅ 满分答法 — Dec 2025 Q1a (6 marks)
*CampusMart (bookstore growing from 200 to thousands of book records). State and explain THREE characteristics of an algorithm for a large-scale system.*

1. **Finiteness** (2) – the search algorithm must terminate after a finite number of steps, so every book search on CampusMart returns a result (found / not found) in bounded time even when the database grows to thousands of records.
2. **Unambiguous** (2) – each step must be clear with only one meaning, so the search logic (e.g. compare title, then move to the next half) produces the same correct result every time and can be maintained as the system grows.
3. **Feasibility** (2) – the algorithm must run with the available resources; CampusMart's server has limited memory and processing power, so the chosen search must work efficiently on that hardware without needing extra resources as the number of records increases.

*（Input/Output 也可：input = 书名关键字，output = 符合的书目清单，必须明确定义。）*

### 6.3 一个简单的 algorithm（p29）：Find max of a, b, c

```
Input  = a, b, c
Output = max
Process:
   Let max = a
   If b > max then max = b
   If c > max then max = c
   Display max
```
*"Order is very important!"*——如果先 Display 再比较，结果就错。

### 6.4 Algorithm development basics（p30）— Tutorial 2 Q4

要先弄清楚三件事：**(1) 需要什么 output？(2) input 是什么？(3) 把 input 变成 output 需要哪些步骤？**（第三点最关键，需要 problem-solving skill；同一个问题有多种解法，要选最优的。）

---

## 7. 怎么表达 algorithm：Pseudocode（p31–42）

为什么不用 natural language？**太模糊（ambiguous）**。为什么不用程序语言？**algorithm 应该独立于程序语言**。所以要取中间：**pseudocode**——NL 和 PL 的混合，有系统地写，让非程序员也看得懂程序的大致运作（p31–33）。

Guidelines（p34）：用和现代高阶语言（C++、Java）一致的结构；适当注解；简单精确。

### 7.1 Components of pseudocode（p35–42）⭐

| Component | 写法 | 例子 |
|---|---|---|
| **Expressions** | `←` 是 assignment（= Java `=`）；`=` 是比较（= Java `==`） | `Sum ← 0` / `Sum ← Sum + 5` |
| **Decision structures** | `if condition then … [else …] end if`，用缩排分块 | `if marks > 50 then print "Passed" else print "Failed"` |
| **Loops – pre-condition** | `while condition do … end while`；`for … do … end for` | `while counter < 5 do …` |
| **Loops – post-condition** | `do … while condition`（**至少执行一次**） | `do print … while counter < 5` |
| **Method declarations** | `Return_type method_name(parameter_list) method_body` | `integer sum(integer num1, integer num2)` |
| **Method calls** | `object.method(args)` | `mycalculator.sum(num1, num2)` |
| **Method returns** | `return value` | `return result` |
| **Comments** | `/* … */`、`//`，也有人用 `{ }` | `// compute total` |
| **Arrays** | `A[i]` 是第 i 格；n 格 array 是 `A[0]` 到 `A[n−1]` | `A[0] ← 10` |

### 7.2 Slide 上的 "What will be the output?"（全部答案）

| Slide | 题目 | 答案 |
|---|---|---|
| p35 | `Sum ← 0; Sum ← Sum + 5` 最后 Sum？ | **5** |
| p36 | marks = 75 | `Congratulation, you are passed!`（75 > 50） |
| p37 | while counter < 5，counter 初始 **0** | 印 **5 次** "Welcome to CS204!"（counter = 0,1,2,3,4） |
| p37 | 同上，counter 初始 **7** | **什么都不印**——pre-condition loop 一开始条件就 false |
| p38 | `for counter ← 0; counter < 5; counter ← counter + 2` | 印 **3 次**（counter = 0, 2, 4） |
| p39 | do-while，counter 初始 **10** | 印 **1 次**——post-condition loop 的 body 至少执行一次，之后 10 < 5 为 false 才停 |

⚠️ p37 的 while 例子忘了先写 `counter ← 0`，所以 slide 才问"初始 0 或 7"——写 pseudocode 时，**用到的变量要先初始化**。

> 💬 **答题句 (EN)** — *In pseudocode, the left arrow (←) is the assignment operator and the equal sign (=) is the equality relation; decision structures use if–then–else with indentation to show which actions belong to each branch.*

### ✅ 满分答法 — May 2025 Q1c (6 marks)
*Examine TWO components of pseudocode and provide an example for each.*

1. **Expressions** (3) – pseudocode uses standard mathematical symbols: the left arrow (←) is the assignment operator (equivalent to = in Java) and the equal sign (=) is the equality relation in Boolean expressions (equivalent to == in Java).
   ```
   total ← 0
   total ← total + price
   ```
2. **Decision structures** (3) – if–then–else logic, where indentation shows which actions belong to the true-actions and false-actions.
   ```
   if marks > 50 then
       print "Congratulations, you passed!"
   else
       print "Sorry, you failed!"
   end if
   ```

*（也可写 Loops：`while counter < 5 do … end while`，pre-condition loop 先检查条件；do-while 是 post-condition，至少执行一次。）*

---

## 8. 怎么写一个 Algorithm（p43–47）

没有统一标准；视问题和资源而定，但**不会为某个程序语言而写**。Slide 用"两数相加"示范两种写法：

| 写法 1（详细） | 写法 2（精简，分析时常用） |
|---|---|
| Step 1 − START | Step 1 − START ADD |
| Step 2 − declare three integers a, b & c | Step 2 − get values of a & b |
| Step 3 − define values of a & b | Step 3 − c ← a + b |
| Step 4 − add values of a & b | Step 4 − display c |
| Step 5 − store output of step 4 to c | Step 5 − STOP |
| Step 6 − print c | |
| Step 7 − STOP | |

p47：设计和分析 algorithm 时**通常用第二种**，因为分析者可以忽略不必要的宣告。

### 8.1 Tutorial 的 algorithm 题（已用 Java 验证逻辑）

**Tutorial 1 Q11 — 显示前 N 个 even / odd number**
```
Step 1 − START
Step 2 − get N
Step 3 − for k ← 1; k ≤ N; k ← k + 1 do
             print 2 * k            // even: 2, 4, 6, ...
         end for
Step 4 − for k ← 1; k ≤ N; k ← k + 1 do
             print 2 * k − 1        // odd: 1, 3, 5, ...
         end for
Step 5 − STOP
```
N = 5 → even `2 4 6 8 10`，odd `1 3 5 7 9`（Java 跑过）。

**Tutorial 2 Q8 — 两个 index 之间最大值的 index**
```
integer indexOfMax(integer A[], integer low, integer high)
start
    maxIndex ← low
    for i ← low + 1; i ≤ high; i ← i + 1 do
        if A[i] > A[maxIndex] then
            maxIndex ← i
        end if
    end for
    return maxIndex
end
```
A = [7, 2, 9, 4, 9, 1, 5]，low = 1，high = 4 → **2**（A[2] = 9；A[4] 也是 9，但用 `>` 所以保留第一个）。

**Tutorial 2 Q9 — 最小与最大值**
```
min ← A[0]; max ← A[0]
for i ← 1; i < n; i ← i + 1 do
    if A[i] < min then min ← A[i] end if
    if A[i] > max then max ← A[i] end if
end for
return min, max
```

**Tutorial 2 Q10 — 平均值**
```
sum ← 0
for i ← 0; i < n; i ← i + 1 do
    sum ← sum + A[i]
end for
average ← sum / n
return average
```
同一个 A → min = 1、max = 9、average = 37 / 7 ≈ 5.29（Java 跑过）。

---

## Closing the loop

回到凌晨一点的 ride-sharing app：它面对的三个问题（data search、processor speed、multiple requests）不能靠买更快的 CPU 解决，要靠**把资料摆好（data structure）**再配上**清楚、有限、可行的步骤（algorithm）**。下一章会先看 Java 怎么把资料装进"容器"（collection），以及怎么用 iterator 一个一个拿出来。

---

## ⚠️ Where the slides mislead

| Slide | 说法 | 更准确的理解 |
|---|---|---|
| p12 | Non-primitive "do not allow any specific instructions to be performed on the data items directly" | 意思是：没有机器指令能直接处理整个 array / class；要靠程序员写的 operation。Structures/Unions 是 C 的概念 |
| p16 vs Ch7 p4 | 两处都说 "two types of data structure"，一个是 linear/non-linear，一个是 static/dynamic | 分类标准不同，都对；看题目用哪一页的句子 |
| p37 | while 例子没有初始化 counter | 写 pseudocode 时变量要先给初值 |
| p24 | Tower of Hanoi 列在"可用 DS/DP 解的问题" | Tower of Hanoi 通常是 **recursion** 的经典例子，不是典型的 DP 问题；考试照 slide 列出没问题 |

考试策略：题目若照抄 slide 的句子，就照 slide 的说法回答。

---

## Term table

| English | 中文 | 一句话说明 |
|---|---|---|
| Data structure | 资料结构 | 资料在记忆体 / 磁碟中的排列方式 |
| Algorithm | 演算法 | 有限的指令集合，把 input 变成 output |
| Data item | 资料项 | 单一单位的值 |
| Group item / Elementary item | 群组项 / 基本项 | 可再拆 / 不可再拆 |
| Entity / Attribute | 实体 / 属性 | 一个对象 / 它的特征 |
| Entity set | 实体集 | 属性相似的实体集合 |
| Field / Record / File | 栏位 / 纪录 / 档案 | 一个属性 / 一个实体的全部栏位 / 全部纪录 |
| Primitive data type | 基本资料型态 | 机器直接支援（int, char…） |
| Non-primitive data type | 非基本资料型态 | 由基本型态组成（array, class…） |
| Linear / Non-linear | 线性 / 非线性 | 一条线排列 / 阶层或网状排列 |
| Traversing | 走访 | 每个元素恰好处理一次 |
| Merging | 合并 | 两个以上结构合成一个 |
| Unambiguous | 无歧义 | 每一步只有一个意思 |
| Finiteness | 有限性 | 有限步后结束 |
| Feasibility | 可行性 | 现有资源就能执行 |
| Pseudocode | 虚拟码 | 自然语言 + 程序语言的混合写法 |
| Pre-condition / Post-condition loop | 前测 / 后测回圈 | 先检查再做 / 先做再检查（至少一次） |

---

## Cheat sheet

- **3 problems**：Data search · Processor speed · Multiple requests → 解法：data structure 让你不必搜全部。
- **层级**：Data → Data item（group / elementary）→ Field → Record → File；Entity has attributes；Entity set。
- **Primitive**：integer, float, double, character, boolean。**Non-primitive**：array, structure, union, class。
- **分类**：Primitive / Non-primitive → Linear（array, stack, queue, linked list）/ Non-linear（tree, graph）。
- **6 operations**：Traversing · Searching · Insertion · Deletion · Sorting · Merging。
- **Algorithm** = Input + Process + Output；finite set of instructions for a task。
- **6 characteristics**：Unambiguous · Input (≥0) · Output (≥1) · Finiteness · Feasibility · Independent。
- **Good algorithm**：correct, finite, terminates, unambiguous, space & time efficient。
- **Develop**：identify output, input, steps。
- **Pseudocode components**：expressions (←, =) · decision (if-then-else) · loops (while/for = pre, do-while = post) · method declaration / call / return · comments · arrays A[0..n−1]。

---

## Practice (answers included)

### A. MCQ
1. Which is **not** a primitive data type? (a) boolean (b) double (c) array (d) character
2. A data item that cannot be divided into sub-items is a/an: (a) group item (b) elementary item (c) record (d) file
3. In pseudocode, `x ← x + 1` means: (a) compare x with x + 1 (b) assign x + 1 to x (c) x equals x + 1 (d) a comment
4. Which loop always executes its body at least once? (a) while (b) for (c) do–while (d) none
5. An algorithm must have: (a) at least one input (b) zero or more outputs (c) one or more outputs (d) a Java implementation

**Answers:** 1 (c) · 2 (b) · 3 (b) · 4 (c) · 5 (c) — input can be zero, output must be at least one.

### B. Short answer
**B1. Differentiate a field and a record. (2 marks)**
A field is a single elementary unit of information representing one attribute of an entity (e.g. CGPA), whereas a record is the collection of all field values of one entity (e.g. one student's ID, name and CGPA).

**B2. Why is pseudocode preferred over natural language and programming language for expressing algorithms? (4 marks)**
Natural language is ambiguous, so the same sentence may be understood in different ways. A programming language ties the algorithm to one language's syntax, but an algorithm should be language-independent. Pseudocode balances both: it is clear and systematic like a programming language, yet language-independent and easy for non-programmers to understand.

**B3. State the THREE items to identify when developing an algorithm. (3 marks)**
The required output, the input, and the steps needed to transform the input into the output.

### C. Application
**C1. A food-delivery app stores restaurants and orders. Explain TWO data structure operations it needs. (6 marks)**
*Searching* – find restaurants near the user's location or an order by its ID so the user sees results quickly. *Sorting* – arrange restaurants by distance, rating or delivery time so the best choices appear first, which also allows faster searching on sorted data.

**C2. Trace the pseudocode. What is printed? (3 marks)**
```
counter ← 3
do
    print counter
    counter ← counter + 2
while counter < 8
```
Printed: **3, 5, 7**. After printing 7, counter becomes 9; 9 < 8 is false, so the loop stops.

**C3. Write pseudocode that counts how many values in array A (size n) are greater than 50. (4 marks)**
```
count ← 0
for i ← 0; i < n; i ← i + 1 do
    if A[i] > 50 then
        count ← count + 1
    end if
end for
return count
```

### D. Thinking
**D1. "Buying a faster server solves data search problems." Do you agree? (4 marks)**
Partly. A faster processor reduces time per operation, but if the data grows to billions of records, or thousands of users search at once, a linear scan still takes too long. Organising the data (e.g. indexing or sorting to allow binary search) reduces the number of operations, which scales far better than faster hardware.

**D2. Can an algorithm have zero inputs? Give an example. (2 marks)**
Yes. An algorithm that prints the first 10 even numbers needs no input; it still has one or more outputs (the printed numbers), which is required.

---

## Slide index

| Note section | Slides |
|---|---|
| 1 Why DSA | p4–6 |
| 2 Terminology | p8–10 |
| 3 Primitive / non-primitive | p11–12 |
| 4 Data structure + classification | p13–20 (图 p15, p17, p18) |
| 5 Operations | p21–24 |
| 6 Algorithm | p25–30 (图 p26) |
| 7 Pseudocode | p31–42 |
| 8 How to write an algorithm | p43–47 |

## Links to other chapters
- **Ch2**：Java collections、iterator、dynamic programming
- **Ch3–5**：怎么衡量 algorithm 的效率（running time、Big-O）
- **Ch7**：linear / non-linear 结构的细节（array、linked list、stack、queue、tree）以及 static / dynamic 分类
- **Ch9**：graph
