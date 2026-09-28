# AMCS2034 — Chapter 8: Static and Dynamic Memory Allocation

> 这章每份考卷只占 4–5 分，但题型固定：**Garbage collection 的 TWO advantages + ONE type（5 分，May 2025、Dec 2025 两次一字不差）**，以及 **static vs dynamic memory allocation（4 分）**。问题是：**slide 几乎没讲 garbage collection 的"类型"**，只在 Java heap 提到 young space / old space。另外 slide 的 Stack vs Heap 表把 C 语言的 heap（要自己 free）和 Java 的 heap（GC 自动回收）混在一起，还说 heap "no size limit"。这份笔记把三种配置方式（static、stack、heap）画成一张记忆体地图，补上 GC 的类型，并给两题的满分答法。

---

## 0. 一句话总览

**变量住在哪里决定它活多久：static 配置在程序开始前就固定、一直存在；stack 配置随 method 呼叫建立、返回就消失；heap（dynamic）配置在执行时要多少给多少，Java 由 garbage collector 自动回收不再用的物件。**

| Part | 内容 | Slide |
|---|---|---|
| 1 | 记忆体是什么；memory management | p4–8 |
| 2 | Stack、global、heap 三种配置位置 | p9–11, p14 |
| 3 | 为什么变量大小要在 compile time 知道 | p12–15 |
| 4 | Memory binding 与 binding time | p16 |
| 5 | Static memory allocation | p17–20 |
| 6 | Stack memory allocation 与 Java stack | p21–24 |
| 7 | Dynamic memory allocation 与 Java heap | p25–33 |
| 8 | Garbage collection（slide 缺的部分补上） | p29–33 + extra |
| 9 | 两张比较表 | p34–35 |

🎯 **这章回答的考题**
- **May 2025 Q4e** & **Dec 2025 Q4c** Classify TWO advantages of garbage collection and identify ONE type (5 + 5)
- **Oct 2025 Q3a(ii)** Distinguish static and dynamic memory allocation (4)

**Prerequisites**：Ch7 static / dynamic data structure、stack（LIFO）；Java 的 `new`；SPC 的 pointer 概念。

---

## Scene：会互相踩到的变量

你写了一个 function，有三个变量：A（1 byte）、B（大小会变）、C（1 byte），依序放在记忆体：

```
address:  1   2   3   4   5   6
          A   B   C
```
后来 B 长到 4 bytes，只好把 C 往后挪：
```
address:  1   2   3   4   5   6
          A   B   B   B   B   C
```
但编译好的程序**不记得变量名字，只记得位址**——它仍以为 C 在 address 3。下一次写 C，其实是写进 **B 的第二个 byte**，B 被破坏了（p12–15）。

结论：**放在这种"一个接一个"区域的变量，大小必须固定、而且在 compile time 就知道**。那大小会变的资料要放哪里？这就是 stack 和 heap 分工的原因。

---

## 1. 记忆体与 Memory Management（p4–8）

- **记忆体（RAM）**像一大张方格纸，每一格是一个 **byte**（可存 0–255），每格有一个**位址（address）**（p4）。
- 宣告变量 `var` 时，电脑挑第一个空格，记下"var 指的是这一格的内容"；电脑记住第一个未使用 byte 的位址，用 **stack pointer** 指向已使用记忆体的尽头（p5）。
- **Memory management** = 管理记忆体的过程，也就是把变量**配置（allocate）与释放（deallocate）**到记忆体；是 **OS 的关键模组**；记忆体可配置给 OS 和使用者程序；可以在 **compile time** 或 **runtime** 配置（p7）。

```
                Memory Allocation
               /                 \
           Static               Dynamic
```

**Conceptual view of memory（p8）**
```
┌────────────────────────────────────────────────┐
│ Program Memory:  [ main ]  [ function ]        │
├────────────────────────────────────────────────┤
│ Data Memory:    [ global ]  [ heap ]  [ stack ]│
└────────────────────────────────────────────────┘
```

**Allocation in process memory（p14）**——考试若要画记忆体配置，用这张：
```
 high address 0x7FFF…
 ┌───────────────────────┐
 │ Stack          ↓      │  static size, dynamic allocation  — local variables
 ├───────────────────────┤
 │        (free)         │
 ├───────────────────────┤
 │ Shared Libraries      │
 ├───────────────────────┤
 │        (free)         │  programmer controlled
 │ Heap           ↑      │  dynamic size, dynamic allocation — variable-sized objects
 ├───────────────────────┤
 │ Read/Write Data       │  static size, static allocation   — global variables
 ├───────────────────────┤     (and static local variables)
 │ Read-only Code & Data │
 └───────────────────────┘
 low address 0x0000…
```
*看这张图要注意：三种配置对应三个区——global/static 在底下的 data 区、heap 往上长、stack 往下长。*

---

## 2. 三种配置位置（p9–11）

| 位置 | 特性（slide） |
|---|---|
| **Stack** | 配置容易（stack pointer 减少）、释放容易（增加）；执行时**自动**配置；可以把值**传给被呼叫的 procedure**，但**不能往上传给呼叫者** |
| **Global variables** | **Statically allocated**；要在 **compile time** 决定需要多少空间；**任何 procedure 之间都能传值** |
| **Heap** | 执行时 **dynamically allocated**；**与 procedure 呼叫无关**；必须小心管理：**自动（garbage collection）**或**手动（malloc/free、new/delete）** |

---

## 3. Memory Binding（p16）

- **Memory binding**：资料项目的"记忆体位址属性"与一块记忆体区域的位址之间的**关联**。
- **Memory allocation** 是执行 memory binding 的**程序**；**binding time** 是做 binding 的时间点。
- Memory allocation 大致分为 **static** 和 **dynamic**。

---

## 4. Static Memory Allocation（p17–20）

- **Memory binding 在程序编译时（compilation）完成**；**执行期间不做任何配置和释放** → 变量是**永久配置**的（p18）——**Tutorial 8 Q1**。
- 对应 **file-scope 变量**和 **local static 变量**。以下两种变量是 statically allocated（p19）——**Tutorial 8 Q2**：
  1. **所有 global variables**（不管有没有宣告 static）；
  2. **明确宣告为 static 的 local variables**。
- **性质（p20）**——**Tutorial 8 Q3**：
  1. 在 **main 开始执行之前**就配置并初始化；
  2. Static local 变量**不会在每次呼叫 function 时重新初始化**（会记住上次的值）；
  3. **位址和大小在 compile time 就固定**。

Java 对应（*extra*）：`static` field 属于 class，在 class 载入时配置一次，整个程序期间都存在：
```java
public class Visitor {
    static int count = 0;          // static: one copy, lives for the whole program
    Visitor() { count++; }         // every new Visitor updates the same count
}
```

---

## 5. Stack Memory Allocation（p21–24）

- 第二种配置：**stack memory allocation**，对应 **non-static 的 local variables** 和 **call-by-value 的参数变量**（p21）——**Tutorial 8 Q4**。
- 这些配置的**大小在 compile time 就固定**，但**位址会随 function 被呼叫的时机而不同**。放在叫做 **stack** 的系统记忆体区。
- 呼叫 function 时，**呼叫者的状态必须保存**，被呼叫的 function 返回后才能继续执行。

**Java Stack（p22）**——**Tutorial 8 Q5**：电脑记忆体中存放**所有 function 建立的暂时变量**的地方；用来**执行一个 thread**，可能有一些短命的值和**指向其他物件的 reference**；使用 **LIFO**。

**Java stack 的使用（p23–24）**——**Tutorial 8 Q6 三步**：
1. 呼叫一个 method 时，在 stack 上为它建立一个**新的 block（frame）**；
2. 这个 block 存放所有 **local values**，以及指向该 method 用到的**其他物件的 reference**；
3. method 结束时，block 被**清除**，空间给下一个 method 使用。

其他特性：这里的东西**只有该 function 能存取**、不会活得比它久；最新保留的 block 最先被释放，所以很好追踪；变量直接存在记忆体、**存取快**；stack 通常**比 heap 小很多**。

```java
public static void main(String[] args) {
    int x = 5;                    // x lives in main's stack frame
    Book b = new Book("DSA");     // reference b in the stack frame; the Book object in the heap
    printTwice(x);                // new frame pushed for printTwice(n = 5)
}                                 // frames popped in LIFO order when methods return
```
```
 Stack (LIFO)                          Heap
 ┌───────────────────────┐
 │ printTwice: n = 5     │ ← top (popped first)
 ├───────────────────────┤         ┌──────────────────┐
 │ main: x = 5, b ───────┼────────▶│ Book{"DSA"}      │
 │       args            │         └──────────────────┘
 └───────────────────────┘
```

---

## 6. Dynamic Memory Allocation（p25–33）

- **Binding 在程序执行期间进行**；memory binding 在执行时建立和摧毁（p26）——**Tutorial 8 Q7**。
- 叫 dynamic 是因为**位置和大小在程序生命期间都会变**；用 **malloc()**（C）或 **new**（Java）配置；大小、位址、内容在执行时变动，所以**除错困难**；放在叫做 **heap** 的区域（p28）。

**p27 的图——三个 program unit A、B、C**
```
   (1) Static           (2) Dynamic:       (3) A calls B      (4) B returns,
                            only A active                         A calls C
   Code(A)              Code(A)            Code(A)            Code(A)
   Data(A)              Code(B)            Code(B)            Code(B)
   Code(B)              Code(C)            Code(C)            Code(C)
   Data(B)              Data(A)            Data(A)            Data(A)
   Code(C)              [ free ]           Data(B)            Data(C)
   Data(C)                                 [ free ]           [ free ]
```
*看这张图要注意：static 一开始就把 A、B、C 的 data **全部**配置好（永久占用）；dynamic 只配置**正在执行**的 unit 的 data——B 返回后 Data(B) 的空间被 C 重用。*

### 6.1 Explicit vs Implicit allocator（p29）——Tutorial 8 Q8

```
   Application
       │  requests / (frees) blocks
   Dynamic Memory Allocator   ← usually a system or language library
       │
   Heap Memory
```

| | **Explicit allocator** | **Implicit allocator** |
|---|---|---|
| 谁释放 | **Application 自己配置、自己释放** | Application 配置，但**不释放**；由系统自动回收 |
| 例子 | C 的 `malloc` / `free`；C++ 的 `new` / `delete` / `delete[]` | **Garbage collection**：Java、ML、Lisp |
| 共同点 | 都把记忆体抽象为一组 **blocks**，提供 free blocks 给 application | |
| 风险 | 忘了 free → memory leak；重复 free / 用已释放的 → crash | 程序员较轻松；GC 执行时有额外负担 |

### 6.2 Java Heap（p30–33）——Tutorial 8 Q9

- Java 物件放在 **heap**；程序执行时建立，大小可**增减**。
- heap **满了**就会启动 **garbage collection**：删除不再使用的物件，腾出空间给新物件。
- 存取比 stack **慢一点**；像一个**全域记忆体池**；资料需要**活得比 method 久**时就放 heap；**所有 function 都能存取**。
- heap 保留 block **没有特定顺序**，随时配置、随时释放；追踪哪里空着比较复杂。
- 分成两个 **generation**：
  - **Young space（nursery）**：新物件配置在这里；满了就做 garbage collection；短命的暂时物件通常用这区。
  - **Old space**：活得久的物件。
  - 分代让 garbage collection **比没分区的 heap 更快**。

---

## 7. Garbage Collection ⭐（slide 只有 p29–33 零星提到，以下整理并补充）

**Garbage collection (GC)** = **implicit** memory management：JVM 自动找出**不再被任何 reference 指到**的物件（unreachable objects），回收它们在 heap 的记忆体。

```java
Book b = new Book("DSA");   // object reachable through b
b = new Book("Algo");       // the "DSA" object is now unreachable → eligible for GC
```

**Advantages（优点）**
1. **自动管理记忆体**：程序员不必手动 free（implicit allocator），**开发更快、程序更简单**。
2. **防止 memory leak**：没人用的物件会被回收，heap 不会被"忘了释放"的物件塞满。
3. **避免 dangling pointer / double free 之类的错误**：程序员不会释放仍在使用的记忆体，也不会重复释放 → **程序更可靠、更安全**。
4. **记忆体可重用**：回收后的空间给新物件（配合 young/old 分代，回收更有效率）。

**Types（类型）**
- 依 slide 的 heap 分代（**generational GC**）：
  - **Minor GC**——回收 **young generation（nursery）**，频繁、快速；
  - **Major / Full GC**——回收 **old generation**（或整个 heap），较少、较慢。
- JVM 的 collector（*extra*）：**Serial GC**（单执行绪，适合小程序）、**Parallel GC**（多执行绪，重视吞吐量）、**G1 (Garbage-First) GC**（Java 9 起的预设，把 heap 分区、控制停顿时间）、**ZGC**（极低停顿、适合大 heap）。
- 概念上的演算法（*extra*）：**Mark-and-Sweep**（标记可到达的物件，清除其他）、Reference counting。

考试只要一个 type：写 **Generational GC（young/old，minor/major）** 最贴近 slide，或写 **G1 Garbage Collector** / **Mark-and-Sweep** 并各加一句说明。

### ✅ 满分答法 — May 2025 Q4e / Dec 2025 Q4c (5 marks)
*Garbage collection is an important aspect of Java memory management. Classify TWO advantages of garbage collection and identify ONE type of garbage collection.*

**Advantages (2 × 2 marks)**
1. **Automatic memory management** – Java uses an implicit allocator: the program allocates objects with `new` but never frees them, because the garbage collector automatically reclaims objects that are no longer referenced. This makes programs simpler and development faster, since programmers do not write `free`/`delete` code.
2. **Prevents memory leaks and memory errors** – unused objects are removed so the heap does not fill up with objects the programmer forgot to free, and because programmers never free memory manually, errors such as dangling pointers or freeing the same memory twice cannot occur, making the program more reliable.

**Type (1 mark)**
**Generational garbage collection** – the Java heap is divided into a young space (nursery), where new short-lived objects are allocated and collected frequently by a quick *minor GC*, and an old space for long-lived objects, collected less often by a *major GC*; this makes collection faster than on an undivided heap. *(Alternatives: Serial, Parallel, G1 (Garbage-First), ZGC, or mark-and-sweep.)*

---

## 8. 两张比较表（p34–35）

### 8.1 Stack vs Heap（p34）——Tutorial 8 Q10

| Stack | Heap |
|---|---|
| 大小随 methods/functions 建立和删除 local variables 而变 | 记忆体**不会自动管理**，也不像 stack 被 CPU 紧密管理；不用时要**自己释放** *（C/C++ 如此；Java 由 GC 回收）* |
| 配置后**自动释放**，不需要你管理 | 容易 **memory leak**：记忆体配给不用的物件，其他 process 用不到 |
| 有**大小限制**，依 OS 而不同 | "**没有大小限制**" *（slide 说法；实际受 JVM 设定 / 实体记忆体限制）* |

补充（p24, p31）：stack **存取快**、只给该 function 用、LIFO；heap 存取**较慢**、所有 function 都能用、物件可活得比 method 久。

### 8.2 Static vs Dynamic Memory Allocation（p35）——Tutorial 8 Q11

| Static memory allocation | Dynamic memory allocation |
|---|---|
| Variables get allocated **permanently** | Variables get allocated **only if the program unit gets active** |
| Allocation done **before** program execution | Allocation done **during** program execution |
| Uses the data structure called **stack** for implementing static allocation *(slide)* | Uses the data structure called **heap** |
| **Less efficient** | **More efficient** |
| **No memory reusability** | **Memory reusability**; memory can be **freed** when not required |

> 💬 **答题句 (EN)** — *In static memory allocation, memory is bound at compile time, before execution, and variables are allocated permanently, so the memory cannot be reused. In dynamic memory allocation, memory is bound during execution from the heap only when a program unit becomes active, so memory can be freed and reused when no longer needed.*

### ✅ 满分答法 — Oct 2025 Q3a(ii) (4 marks)
*Distinguish between static and dynamic memory allocation.*

| Aspect | Static memory allocation | Dynamic memory allocation |
|---|---|---|
| When (1) | Memory binding is performed at **compile time**, before the program runs. | Memory binding is performed at **run time**, during program execution (e.g. using `new` in Java or `malloc()` in C). |
| Lifetime (1) | Variables are allocated **permanently** for the whole execution (global and static variables). | Memory is allocated **only when a program unit becomes active** and can be released when no longer needed. |
| Where (1) | Fixed-size area decided at compile time (stack / static data area in the slide). | The **heap**. |
| Efficiency / reuse (1) | **No memory reusability**, so memory may be wasted; sizes and addresses are fixed. | **Memory can be reused** (freed manually or by garbage collection), making more efficient use of memory. |

---

## Closing the loop

回到"会互相踩到的变量"：大小固定、编译时就知道的变量放 static 区或 stack frame（位址算得出来，不会踩到别人）；大小会变、要活得比 method 久的资料放 heap，程序只拿一个固定大小的 reference 指过去。Java 再加一层 garbage collector，让你只管 `new` 不管 free。下一章进入最后、也是每份考卷 20–25 分的 graph。

---

## ⚠️ Where the slides mislead

| Slide | 说法 | 更准确的理解 |
|---|---|---|
| p31 | "Unlike in a Java stack where memory allocation is done when your program is compiled" | Stack frame 是在**执行时**、method 被呼叫时才建立；compile time 只决定 frame 的**大小** |
| p34 | Heap "not managed automatically… free allocated memory yourself" | 这是 **C/C++** 的情况；**Java heap 由 GC 自动管理**。考试照表写，但若题目问 Java，写 GC |
| p34 | "There is no size limit in the heap" | Heap 受 JVM 设定（-Xmx）与实体记忆体限制，满了会 `OutOfMemoryError` |
| p35 | Static allocation "uses the data structure called **stack**" | Global/static 变量其实在 **static data 区**（p14 图的 Read/Write Data），stack 是给 local 变量的；照 slide 表写不会扣分 |
| p35 | Static "less efficient"、dynamic "more efficient" | 指**记忆体使用**效率；论**速度**，stack/static 存取反而较快（p24, p31） |
| p27 | 图下方编号 "1 2 3 4" 对不齐 | 四栏依序是 (1) static、(2) 只有 A、(3) A calls B、(4) B 返回后 A calls C |

---

## Term table

| English | 中文 | 一句话说明 |
|---|---|---|
| Memory management | 记忆体管理 | 配置与释放变量的记忆体 |
| Memory binding | 记忆体绑定 | 资料项目与记忆体位址的关联 |
| Binding time | 绑定时间 | 何时做 binding（compile / run time） |
| Static memory allocation | 静态记忆体配置 | 编译时配置，永久存在 |
| Stack memory allocation | 堆叠记忆体配置 | local 变量，随呼叫建立 |
| Dynamic memory allocation | 动态记忆体配置 | 执行时从 heap 配置 |
| Stack frame / block | 堆叠框 | 一次 method 呼叫的 local 资料 |
| Heap | 堆积 | 动态配置的记忆体池 |
| Stack pointer | 堆叠指标 | 指向已用记忆体尽头 |
| Explicit / Implicit allocator | 显式 / 隐式配置器 | 自己 free / GC 回收 |
| Garbage collection | 垃圾回收 | 自动回收不再被参考的物件 |
| Young space (nursery) / Old space | 新生代 / 老年代 | 新物件 / 长寿物件 |
| Minor / Major GC | — | 回收 young / old generation |
| Memory leak | 记忆体泄漏 | 不用的记忆体没被释放 |
| Dangling pointer | 悬置指标 | 指向已释放记忆体的 pointer |

---

## Cheat sheet

- **Memory** = 方格纸，每格 1 byte + address；stack pointer 指已用尽头。
- **Memory management**：allocate + deallocate；OS 模组；compile time 或 runtime。
- **Stack**：自动、快、值只能往下传；**Global**：static、compile time 决定、任何 procedure 可用；**Heap**：runtime、独立于呼叫、GC 或 malloc/free。
- **变量大小要固定**：否则 C 被挪位，程序仍写旧位址 → 覆写 B。
- **Binding**：address 关联；allocation = 做 binding 的程序；binding time = 时间点。
- **Static**：compile time；global + static local；main 前配置；static local 不重新初始化；address & size 固定。
- **Stack alloc**：non-static local + call-by-value 参数；size compile time 固定，address 随呼叫变。
- **Java stack 3 steps**：method 呼叫 → 新 block → 存 local + references → method 结束清除。
- **Dynamic**：runtime；malloc / new；heap；位置和大小会变 → 难除错。
- **Explicit**（malloc/free, new/delete）vs **Implicit**（GC：Java, ML, Lisp）。
- **Java heap**：objects；满了 → GC；young（nursery）/ old space。
- **GC advantages**：automatic、no leaks、no dangling pointers/double free、reuse。**Type**：generational（minor/major）· Serial · Parallel · G1 · ZGC · mark-and-sweep。
- **Static vs dynamic**：permanent vs when active · before vs during execution · stack vs heap（slide）· no reuse vs reuse。

---

## Practice (answers included)

### A. MCQ
1. Which variables are statically allocated? (a) method parameters (b) global variables (c) objects created with new (d) local non-static variables
2. In Java, an object created with `new` is stored in: (a) the stack (b) the heap (c) the code segment (d) a register
3. Garbage collection is an example of: (a) explicit allocator (b) implicit allocator (c) static allocation (d) stack allocation
4. When a Java method returns, its stack block is: (a) moved to the heap (b) garbage collected later (c) erased immediately (d) kept until the program ends
5. New short-lived Java objects are normally allocated in: (a) old space (b) young space (nursery) (c) static area (d) code area

**Answers:** 1 (b) · 2 (b) · 3 (b) · 4 (c) · 5 (b)

### B. Short answer
**B1. Describe THREE properties of statically allocated variables. (3 marks)**
They are allocated and initialised before main starts running; static local variables are not reinitialised on every call to the function that declares them; their address and size are fixed at compile time.

**B2. Compare explicit and implicit memory allocators. (4 marks)**
Both manage the heap as a set of blocks and provide free blocks to the application. With an explicit allocator the application both allocates and frees memory (malloc/free in C, new/delete in C++); with an implicit allocator the application only allocates, and unused memory is reclaimed automatically by garbage collection (Java, ML, Lisp).

**B3. Why must variables in the stack/static area have a size known at compile time? (2 marks)**
The compiled code refers to variables by address. If a variable could grow, the variables after it would have to move, but the code would still use their old addresses and overwrite the enlarged variable.

### C. Application
**C1. For the code below, state where each item is stored (stack / heap / static). (4 marks)**
```java
public class Shop {
    static int totalOrders = 0;
    public static void main(String[] args) {
        int qty = 3;
        Order o = new Order(qty);
    }
}
```
`totalOrders` → static area (class-level, allocated once); `qty` → stack (local primitive in main's frame); `o` (the reference) → stack; the `Order` object → heap.

**C2. Explain what happens to memory in the p27 example when B returns to A and A calls C. (3 marks)**
Under dynamic allocation, Data(B) is released when B returns, and the same space is reused for Data(C) when C becomes active; under static allocation, Data(A), Data(B) and Data(C) are all allocated permanently from the start, so no space is reused.

### D. Thinking
**D1. "Java programs cannot have memory leaks because of garbage collection." Discuss. (3 marks)**
GC reclaims only unreachable objects. If a program keeps references to objects it no longer needs (e.g. keeps adding to a static list and never removes them), those objects stay reachable and are never collected, so memory can still leak logically even in Java.

**D2. Why is stack access faster than heap access? (2 marks)**
Stack allocation and deallocation just move the stack pointer in LIFO order, and the latest block is always on top, whereas the heap allocates blocks in no particular order and must track free space (and run GC), which is more complex and slower.

---

## Slide index

| Note section | Slides |
|---|---|
| 1 Memory & management | p4–8（图 p6, p8） |
| 2 Stack / global / heap | p9–11, p14（图 p14） |
| 3 Variable size problem | p12–15 |
| 4 Binding | p16 |
| 5 Static allocation | p17–20 |
| 6 Stack allocation, Java stack | p21–24 |
| 7 Dynamic allocation, Java heap | p25–33（图 p27, p29） |
| 8 Garbage collection | p29–33 + extra |
| 9 Comparison tables | p34–35 |

## Links to other chapters
- **Ch4**：space complexity
- **Ch7**：static vs dynamic **data structure**（不要和本章的 static vs dynamic **memory allocation** 混淆）；stack LIFO；linked list 的 node 在 heap
- **SPC Ch4–6**：C++ 的 pointer、new/delete、activation frame
