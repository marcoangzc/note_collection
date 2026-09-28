# AMCS2034 — Chapter 7: Types of Data Structures

> 这是 84 页、**分数第二高**的一章：近两份考卷的 **Q3 整题 21–25 分**都在这里——array vs linked list、static vs dynamic、stack vs queue（要选哪个做 task scheduler）、tree vs graph（social network）、**binary tree traversal 输出（7–9 分）**。Slide 的 traversal 只给了一棵完美的 7 节点树；考卷的树却**缺左或缺右子树**（例如 6 只有右孩子 17、53 只有右孩子 26），学生最常在这里失分。Slide 关于 static/dynamic 的 overflow 说法也和一般教科书相反。这份笔记给一个不会错的 traversal 追踪法，四棵树全部用程序跑过。

---

## 0. 一句话总览

**Data structure 可以按"大小能不能变"分成 static（array）和 dynamic（linked list），也可以按"怎么排列"分成 linear（array、linked list、stack、queue）和 non-linear（tree、graph）。每种结构都在"存取快"和"插删快"、"省记忆体"和"有弹性"之间取舍。**

| Part | 内容 | Slide |
|---|---|---|
| 1 | Static data structure | p3–7 |
| 2 | Dynamic data structure | p8–11 |
| 3 | Array vs linked list（含 cache locality） | p12–27 |
| 4 | Linked list 实作（node、insert、delete、doubly、header） | p28–46 |
| 5 | Linear DS：queue、stack（含 stock span） | p47–61 |
| 6 | Non-linear DS：tree、binary tree traversal | p62–76 |
| 7 | Graph 简介；linear vs non-linear | p77–82 |

🎯 **这章回答的考题**
- **Oct 2024 Q3d** Explain TWO types of data structure (4)
- **Oct 2025 Q3a(i)** Static vs dynamic data structures (4)
- **Dec 2025 Q3a** Arrays vs linked lists: memory, insertion/deletion, access (6)
- **Oct 2025 Q3c** TWO advantages of linked list over array (4)
- **May 2025 Q3d** THREE disadvantages of linked lists (6)
- **May 2025 Q3e** FOUR fundamental stack operations (8)
- **Oct 2025 Q3a(iii)** Queue vs Stack (4)
- **Dec 2025 Q3b** Stack or queue for a task scheduler? (6)
- **Dec 2025 Q3c** Tree or graph for a social network? (6)
- **Dec 2025 Q3d** Pre-order + post-order (3.5 + 3.5)
- **Oct 2025 Q3b** In-order + post-order (4.5 + 4.5)

**Prerequisites**：Ch1 linear / non-linear；Java object reference；recursion。

---

## Scene：图书馆系统的四个难题

TAR UMT 图书馆要重写系统，工程师遇到四个问题：

1. 书的数量今年 5,000 本、明年可能 20,000 本——**要先订好大小，还是让它自己长？**
2. 新书要按 ID 排序插在中间——**每次插入都要把后面几千笔往后挪吗？**
3. 印表机前排队的列印工作、"上一页"按钮——**先来的先处理，还是最后来的先处理？**
4. 分类目录（科学 → 电脑科学 → 资料结构）和"读者之间互相推荐"——**这种关系还能用一条线表示吗？**

这四个问题正好对应本章四大块。

---

## 1. Static Data Structure（p3–7）

**Data structure**（p4）：*a way of storing and organising data efficiently such that the required operations on them can be performed efficiently with respect to time as well as memory*；简单说，用来**降低程序的复杂度（主要是时间复杂度）**。分两种：**static** 和 **dynamic**。

**Static data structure**（p5–7）：
- 记忆体中**大小固定**的资料集合；**最大大小要事先知道**，之后不能重新配置。例子：**Array**。
- 适合存**固定数量**的资料（Tutorial 7 Q3）。
- 缺乏 dynamic 结构的弹性（需要时多用记忆体、不需要时释放）。
- **Static 着重 performance；dynamic 着重记忆体使用效率**（p6）。
- **优点（p7）——Tutorial 7 Q4 的动机**：记忆体配置固定，所以新增 / 删除时**不需要额外控制来防止 overflow / underflow**，比较容易写程序；代价是记忆体使用可能不够有效率。

## 2. Dynamic Data Structure（p8–11）

- **Dynamic data structure (DDS)**：记忆体中可以**变大或缩小**的资料集合，让程序员可以精确控制用多少记忆体（p9）。
- 透过从 **heap** 配置或释放未使用的记忆体来改变大小（p9）。
- 例子：**Linked List**（p10）。
- **何时用（p11）**：**资料数量无法事先预测时**。
- **缺点（p11）**：配置不固定，可能超过允许的最大记忆体而 **overflow**，或变空时 **underflow**；程序员要加控制，持续监控资料的大小和位置。

**p27 比较表（考试照这张写）**

| Dynamic data structures | Static data structures |
|---|---|
| Memory is allocated **dynamically**, i.e. as the program executes | Memory is allocated at **compile time**; **fixed size** |
| Possible to **overflow** if it exceeds its allowed limit, or **underflow** if it becomes empty | Allocation is fixed, so **no problem** with adding and removing items |
| **Most efficient use of memory** — uses only as much as it needs | Can be **very inefficient** — memory is set aside whether needed or not |
| **Harder to program** — must track size and item locations at all times | **Easier to program** — no need to check the size |

> 💬 **答题句 (EN)** — *A static data structure has a fixed size that must be known in advance, e.g. an array; it is easier to program but may waste memory. A dynamic data structure can grow or shrink at run time by allocating and de-allocating memory from the heap, e.g. a linked list; it uses memory efficiently but is harder to program because its size and item locations must be tracked.*

### ✅ 满分答法 — Oct 2025 Q3a(i) (4 marks) / Oct 2024 Q3d (4 marks)
*Distinguish static and dynamic data structures / Explain TWO types of data structure.*

1. **Static data structure** (2) – a collection of data in memory whose size is fixed; the maximum size must be known in advance because memory cannot be reallocated later. It is easy to program because no size checks are needed, but memory reserved for unused elements is wasted. *Example:* an array.
2. **Dynamic data structure** (2) – a collection of data whose size can grow or shrink during program execution, because memory is allocated and de-allocated from the heap as needed. It makes efficient use of memory and is suitable when the number of items cannot be predicted, but it is harder to program since its size and item locations must be monitored. *Example:* a linked list.

---

## 3. Array vs Linked List（p12–27）⭐

```
Array (contiguous, indexed)                 Linked list (nodes + references)
 index:  0    1    2    3    4                head
       ┌────┬────┬────┬────┬────┐              │
       │ 40 │ 55 │ 63 │ 17 │ 22 │              ▼
       └────┴────┴────┴────┴────┘            ┌───┬──┐   ┌───┬──┐   ┌───┬──┐
  arr[3] → jump straight to 17               │ A │ ─┼──▶│ B │ ─┼──▶│ C │ ─┼──▶ NULL
                                             └───┴──┘   └───┴──┘   └───┴──┘
                                              data next
```
*看这张图要注意：array 用 index 一步到位；linked list 要从 head 一格一格跟着 next 走。*

| Aspect | Array | Linked list |
|---|---|---|
| 元素 | 同型态资料，属于 **index** | 无序连结的 **nodes** |
| **Access** | 用 index **直接存取，快**（O(1)） | 从 head 走到第 k 个，**linear time**（O(n)） |
| **Insertion / deletion** | **很耗时**——要挪动后面的元素 | **快**——只改 reference |
| **Size** | **固定** | **动态**，可伸缩 |
| **Memory allocation** | compile time（slide 说法） | **执行时（runtime）** |
| **Storage** | **连续（consecutive）** | **随机**分布 |
| **Memory per element** | 较少（只存资料） | 较多（多存 next / previous reference） |
| **Memory utilisation** | 不够有效率（预留的空间可能没用到） | 有效率（用多少配多少） |

**插入例子（p19–20）**：`id[] = [1000, 1010, 1050, 2000, 2040, …]`
- 插入 1005 保持排序 → 1000 之后的**全部元素**都要往后挪一格。
- 删除 1010 → 1010 之后的**全部元素**都要往前挪。

**Linked list 的两个优点（p21）**：(1) **Dynamic size**；(2) **Ease of insertion / deletion**。

**Linked list 的缺点（p22）**：
1. **不能 random access**——必须从第一个 node 依序走，所以**不能对 linked list 做 binary search**；
2. 每个元素要额外空间存 **pointer**；
3. **Array 的 cache locality 较好**，效能差别可能很大。

**Cache locality（p23–26）**：array 是**连续的记忆体块**，第一次存取时一大块会被载入 cache，之后存取很快。Linked list 的 node 不一定连续：

```
  Array data (contiguous)           Linked list l_data (scattered)
  aaaa 0000   data[0]               aaaa 1000   l_data
  aaaa 0040   data[1]               aaaa 3040   l_data->next
  aaaa 0080   data[2]               aaaa 4050   l_data->next->next
```
读 data[0] 时 CPU 把附近（到 aaaa 2000 左右）一起放进 cache → data[1]、data[2] 已在 cache。读 l_data（aaaa 1000）时也会载入附近，但下一个 node 在 aaaa 3040，**不在 cache 里**，又要回主记忆体 → 更多 **cache miss**。

> 💬 **答题句 (EN)** — *An array stores elements in contiguous memory with a fixed size, so any element can be accessed directly by index in constant time, but insertion and deletion require shifting elements. A linked list stores nodes anywhere in memory connected by references, so it can grow dynamically and insert or delete by changing references, but it must be traversed sequentially from the head and needs extra memory for the pointers.*

### ✅ 满分答法 — Dec 2025 Q3a (6 marks)
*Explain the differences between arrays and linked lists in terms of memory allocation, ease of insertion/deletion, and access time.*

| Aspect | Array | Linked list |
|---|---|---|
| **Memory allocation** (2) | A fixed block of **contiguous** memory is allocated for a fixed maximum size (static), so unused slots are wasted and the size cannot grow. | Memory for each node is allocated **dynamically at runtime** from the heap, anywhere in memory; the list grows or shrinks as books are added or removed, but each node needs extra memory for its next reference. |
| **Ease of insertion / deletion** (2) | **Expensive**: to insert or delete a book record in the middle, all the following elements must be shifted. | **Easy**: only the references of the neighbouring nodes are changed; no elements are shifted. |
| **Access time** (2) | **Fast, constant time O(1)**: any record is accessed directly by its index; also better cache locality. | **Slow, linear time O(n)**: the list must be traversed from the head node to reach a record; no random access, so binary search is not possible. |

### ✅ 满分答法 — Oct 2025 Q3c (4 marks)
*Discuss any TWO advantages of using a linked list compared to an array.*

1. **Dynamic size** (2) – an array's size is fixed, so the upper limit must be known in advance and memory is usually allocated for that limit even if it is rarely reached; a linked list grows and shrinks at runtime, allocating memory only for the nodes actually used.
2. **Ease of insertion and deletion** (2) – inserting into a sorted array (e.g. ID 1005 into [1000, 1010, 1050, …]) requires shifting every later element to make room, and deletion requires shifting them back; in a linked list, a node is inserted or deleted by changing a few references, without moving other elements.

### ✅ 满分答法 — May 2025 Q3d (6 marks)
*Provide THREE disadvantages of linked lists.*

1. **No random access** (2) – elements must be accessed sequentially starting from the first node, so reaching the k-th element takes linear time and efficient algorithms such as binary search cannot be used.
2. **Extra memory for pointers** (2) – every node must store a reference (pointer) to the next node (and to the previous node in a doubly linked list) in addition to the data, which is memory overhead an array does not need.
3. **Poor cache locality** (2) – array elements are stored in contiguous memory, so after the first access nearby elements are already in the CPU cache; linked-list nodes are scattered in memory, causing more cache misses and slower traversal.

---

## 4. Linked List 实作（p28–46）

### 4.1 Object reference 当作 link（p29–32）

- **Object reference** = 存物件**位址**的变量，也可叫 **pointer**。
- 一个 class 如果含有指向**同类别**物件的 reference，就能把物件串起来：
```java
class Node {
   int info;
   Node next;        // ← 指向下一个 Node
}
```
- **Intermediate nodes（p31）**：被存的物件（例如 Student）**不应该**自己管"下一个 Student 是谁"。改用**独立的 node class**，有两部分：指向物件的 reference + 指向下一个 node 的 link。

```
 list
  │
  ▼
┌──────┐    ┌──────┐    ┌──────┐    ┌──────┐
│ info │    │ info │    │ info │    │ info │
│ next ┼───▶│ next ┼───▶│ next ┼───▶│ next ┼──╳ (null)
└──────┘    └──────┘    └──────┘    └──────┘
```

### 4.2 MagazineList 例子（p33–40）

`MagazineList` 管理 `Magazine` 物件，内部用 private inner class `MagazineNode`：

```java
public class MagazineList {
   private MagazineNode list;              // ← head reference

   public MagazineList() { list = null; }  // 空 list

   public void add (Magazine mag) {        // 加在尾端
      MagazineNode node = new MagazineNode (mag);
      MagazineNode current;
      if (list == null)
         list = node;                      // ← 空的：新 node 就是 head
      else {
         current = list;
         while (current.next != null)      // ← 走到最后一个 node
            current = current.next;
         current.next = node;              // ← 接在后面
      }
   }

   public String toString () {
      String result = "";
      MagazineNode current = list;
      while (current != null) {            // ← traversal
         result += current.magazine + "\n";
         current = current.next;
      }
      return result;
   }

   private class MagazineNode {
      public Magazine magazine;
      public MagazineNode next;
      public MagazineNode (Magazine mag) { magazine = mag; next = null; }
   }
}
```
MagazineRack 加入 Time、Woodworking Today、Communications of the ACM、House and Garden、GQ，实跑输出与 slide 一致：
```
Time
Woodworking Today
Communications of the ACM
House and Garden
GQ
```

### 4.3 Insert / Delete（p41–44）

**插入 newNode 到 current 之后（p43，Quick Check 答案）**
```java
newNode.next = current.next;   // ① 新 node 先指向 current 的下一个
current.next = newNode;        // ② current 再指向新 node
```
```
before:  current ──▶ [B] ──▶ [C]
step ①:  current ──▶ [B] ──▶ [C]
                      ▲
          newNode ────┘      (newNode.next = B)
step ②:  current ──▶ newNode ──▶ [B] ──▶ [C]
```
⚠️ **顺序不能反**：先做 ② 的话，`current.next` 已经变成 newNode，原本的 B 就找不到了（整段 list 断掉）。

**删除 current 后面那个 node（p44）**：改**前一个 node** 的 next：
```java
prev.next = prev.next.next;    // 跳过要删的 node
```
实跑：10 → 20 → 40，插入 30 到 20 之后 → 10 → 20 → 30 → 40；删除 20 → 10 → 30 → 40。

### 4.4 其他 dynamic 表示法（p45–46）

**Doubly linked list**——每个 node 有 **next 和 prev**：
```
        ┌──────┐     ┌──────┐     ┌──────┐     ┌──────┐
 list ─▶│ info │     │ info │     │ info │     │ info │
        │ next ┼────▶│ next ┼────▶│ next ┼────▶│ next ┼──╳
   ╳ ◀──┼ prev │◀────┼ prev │◀────┼ prev │◀────┼ prev │
        └──────┘     └──────┘     └──────┘     └──────┘
```
*看这张图要注意：每个 node 都能往前、往后走；代价是每个 node 多存一个 reference。*
**Header node**——另外一个 node 存 **count** 以及指向 **front** 和 **rear** 的 reference：
```
 list ──▶ ┌─────────┐
          │ count: 4│
          │ front  ─┼──▶ [info|next] ─▶ [info|next] ─▶ [info|next] ─▶ [info|next]─╳
          │ rear   ─┼─────────────────────────────────────────────────────▲
          └─────────┘
```

---

## 5. Linear Data Structures：Queue 与 Stack（p47–61）

**Linear DS（p48, p50）**：元素一个接一个线性排列；走访时一次只能直接到达一个元素；因为电脑记忆体本身也是线性的，所以**容易实作**。常用：array、linked list、stack、queue。

### 5.1 Queue（p51–55）

```
 Items go on at the REAR (enqueue)          Items come off at the FRONT (dequeue)
        ──────▶ ┌──┬──┬──┬──┬──┬──┐ ──────▶
                │  │  │  │  │  │  │
                └──┴──┴──┴──┴──┴──┘
                rear              front
```
- 只在 **rear 加入**、只从 **front 移除** → **FIFO**（First-In, First-Out）。比喻：银行柜台前排队。
- **三个 operation（Tutorial 7 Q14）**：**enqueue**（加到 rear）、**dequeue / serve**（从 front 移除）、**empty**（是否为空）。
- 用在 simulation、或任何东西"排队等待处理"的情况。
- **两种表示法（Tutorial 7 Q15）**：(1) **singly linked list**，reference **从 front 指向 rear** 最有效率；(2) **array**，用**余数运算 %** 在到达 array 尾端时"绕回"前面（circular array）。
- **Direct applications（p54）**：OS 按**到达顺序**排程同优先权的工作（例：**print queue**）；模拟真实排队（售票处）；multiprogramming；asynchronous data transfer（file IO、pipes、sockets）；call center 等待时间；超市要几个收银员。
- **Indirect**：其他 algorithm 的辅助资料结构（例如 BFS，见 Ch9）；其他资料结构的组件。

### 5.2 Stack（p56–61）

```
  The last item to go          must be the first item
  on the stack (push)          to come off (pop)
          │                          ▲
          ▼                          │
        ┌──────────────────────────────┐
        │  top  →  [ C ]               │
        │          [ B ]               │
        │          [ A ]   ← bottom    │
        └──────────────────────────────┘
```
- 也是 linear；只从**一端（top）**加入和移除 → **LIFO**（Last-In, First-Out）。比喻：一叠盘子、一叠书。
- **四个 operation（p58）**：

| Operation | 作用 |
|---|---|
| **push** | 把 item 加到 **top** |
| **pop** | 移除并回传 **top** 的 item |
| **peek / top** | **取得** top 的 item，但**不移除** |
| **empty** | stack 为空时回传 true |

实跑（Java `ArrayDeque`）：push A, B, C → `peek = C`，`pop = C`，剩 [B, A]，`empty = false`。

- **两种表示法（Tutorial 7 Q17）**：(1) **singly linked list**，**第一个 node 就是 top**；(2) **array**，**stack 的底部在 index 0**。
- **Direct applications（p59）**：balancing of symbols；infix-to-postfix conversion；evaluation of postfix expression；**implementing function calls (including recursion)**；finding spans（stock span）；**browser 的 Back 按钮**；**文字编辑器的 undo**；HTML/XML 的 tag matching。
- **Indirect**：其他 algorithm 的辅助结构（例如 tree traversal）；其他结构的组件。

> 💬 **答题句 (EN)** — *A stack is a linear LIFO structure where items are added (push) and removed (pop) only at the top, with peek returning the top item without removing it and empty checking whether the stack has no items. A queue is a linear FIFO structure where items are added at the rear (enqueue) and removed from the front (dequeue).*

### ✅ 满分答法 — May 2025 Q3e (8 marks)
*Examine the FOUR fundamental operations associated with the stack and explain their purpose and functionality.*

每个 2 分：做什么 + 目的 / 例子。
1. **push(item)** – adds a new item to the top of the stack; the new item becomes the top, above all earlier items. Purpose: store data that must be processed in reverse order, e.g. pushing each visited web page onto the browser history.
2. **pop()** – removes and returns the item at the top of the stack; the item below it becomes the new top. Purpose: retrieve the most recently added item first (LIFO), e.g. the Back button returns to the last visited page.
3. **peek() / top()** – returns the top item without removing it, so the stack is unchanged. Purpose: inspect the next item to be processed, e.g. check the last opened HTML tag when matching tags.
4. **empty() / isEmpty()** – returns true if the stack contains no items, otherwise false. Purpose: prevent errors such as popping from an empty stack (underflow), e.g. stop an undo operation when there is nothing left to undo.

### ✅ 满分答法 — Oct 2025 Q3a(iii) (4 marks)
*Distinguish between Queue and Stack.*

| | Queue | Stack |
|---|---|---|
| Order (1+1) | **FIFO** – the first item added is the first removed. | **LIFO** – the last item added is the first removed. |
| Ends / operations (1+1) | Items are added at the **rear** (enqueue) and removed from the **front** (dequeue) — two ends. | Items are added (push) and removed (pop) at the **same end, the top**. |
| Example | Print queue, bank-teller line. | Undo in a text editor, browser Back button. |

### ✅ 满分答法 — Dec 2025 Q3b (6 marks)
*A student wants to implement a task scheduler. Explain whether a stack or a queue is more suitable and explain the reason.*

A **queue** is more suitable (2). A task scheduler should process tasks in the order they arrive, which matches the queue's **FIFO** behaviour: new tasks are enqueued at the rear and the scheduler always dequeues the oldest task from the front (2). This is exactly how operating systems schedule jobs of equal priority, such as a print queue. A stack is LIFO, so the most recently added task would always run first; if new tasks keep arriving, older tasks would wait indefinitely (starvation), which is unfair and unpredictable for a scheduler (2).

### 5.3 The Stock Span Problem（p60）

有 n 天的股价，第 i 天的 **span Sᵢ** = 当天之前（含当天）**连续**几天的价格 ≤ 当天价格。
例：prices = {100, 80, 60, 70, 60, 75, 85} → spans = **{1, 1, 1, 2, 1, 4, 6}**（程序验证）。

```
Day:    0    1    2    3    4    5    6
Price: 100   80   60   70   60   75   85
Span:   1    1    1    2    1    4    6
          75 ≥ 60, 70, 60 (3 days before) + itself = 4
          85 ≥ 75, 60, 70, 60, 80 (5 days)  + itself = 6
```
*（extra）用 stack 解：stack 存"还没被超越的较高价格"的 index；新价格进来时，把比它低或相等的都 pop 掉，span = 今天 index − stack 顶端 index（stack 空则 = index + 1）。每个 index 最多 push、pop 各一次 → O(n)。*

---

## 6. Non-linear Data Structures：Tree（p62–76）

**Non-linear（p63）**：元素**不是**依序排列；一个元素可以连到**多个**其他元素来表示特殊关系；**不能一趟走完**。例：**tree、graph**（p65）。

### 6.1 Tree（p66–69）

- **Tree**：由 **root node** 和可能很多层的其他 node 组成的**阶层（hierarchy）**。
- 没有 children 的 node = **leaf node**；root 和 leaf 以外的 = **internal node**。
- **General tree**：每个 node 可以有很多 children。
- **Binary tree**：每个 node **最多两个** children；通常用 reference 当 dynamic link，每个 node 只要存 **left、right** 两个 link。

**Binary tree 的应用（p69）**：compiler 的 **expression tree**；资料压缩的 **Huffman coding tree**；**Binary Search Tree (BST)**——search/insert/delete 平均 **O(log n)**；**Priority Queue**——搜寻和删除最小（最大）值 worst case 对数时间。

### 6.2 Binary Tree Traversals（p70–76）⭐⭐

**Traversal** = 走访树的所有 node 的过程；每个 node **只被处理一次**，但可能**经过多次**。和 searching 不同：searching 找到就停，traversal 要处理**全部** node（p70–71）。

三个步骤（p72）：**D** = visit 目前的 node；**L** = 走到左子树；**R** = 走到右子树。顺序决定 traversal 类型（用 recursion 描述最自然）：

| Traversal | 顺序 | 定义 |
|---|---|---|
| **Preorder** | **D L R** | Visit root → preorder(left) → preorder(right) |
| **Inorder** | **L D R** | inorder(left) → visit root → inorder(right) |
| **Postorder** | **L R D** | postorder(left) → postorder(right) → visit root |

**记法**：**pre / in / post 说的是 root（D）在哪里**——前面、中间、后面。左永远在右前面。

**Slide 的例子（p74–76）**
```
            1
          /   \
         2     3
        / \   / \
       4   5 6   7
```
- Preorder：**1 2 4 5 3 6 7**
- Inorder：**4 2 5 1 6 3 7**
- Postorder：**4 5 2 6 7 3 1**

**不会错的追踪法：一次处理一棵子树，把它当成 [root, (左), (右)] 展开**

以 Dec 2025 的树为例：
```
            3
          /   \
         9     20
        /     /  \
       6     15    7
        \
         17
```
⚠️ 注意 **6 没有左孩子、只有右孩子 17**；9 **只有左孩子** 6。

**Preorder（D L R）逐步展开：**
```
pre(3)  = 3, pre(9), pre(20)
pre(9)  = 9, pre(6), (right of 9 is empty)
pre(6)  = 6, (left empty), pre(17)  = 6, 17
pre(20) = 20, pre(15), pre(7) = 20, 15, 7
⇒ 3, 9, 6, 17, 20, 15, 7
```

**Postorder（L R D）逐步展开：**
```
post(3)  = post(9), post(20), 3
post(9)  = post(6), 9          (right of 9 is empty)
post(6)  = (left empty), post(17), 6 = 17, 6
post(20) = 15, 7, 20
⇒ 17, 6, 9, 15, 7, 20, 3
```

### ✅ 满分答法 — Dec 2025 Q3d (3.5 + 3.5 marks)
*Using the binary tree in Figure 3-1, determine the traversal outputs.*

(i) **Pre-order** (root, left, right): **3, 9, 6, 17, 20, 15, 7**
(ii) **Post-order** (left, right, root): **17, 6, 9, 15, 7, 20, 3**

*（7 个 node，每个位置约 0.5 分——一个放错就会连带扣分，所以一定要用上面的展开法。In-order 备用：6, 17, 9, 3, 15, 20, 7。）*

### ✅ 满分答法 — Oct 2025 Q3b (4.5 + 4.5 marks)
*Given the binary tree in Figure 3-1, provide the output for in-order and post-order traversal.*

```
               12
             /    \
           17      53
          /  \       \
         3    8       26
             / \      /
           10   18   15
```
⚠️ **53 只有右孩子 26；26 只有左孩子 15。**

In-order 展开（L D R）：
```
in(12) = in(17), 12, in(53)
in(17) = in(3), 17, in(8) = 3, 17, (10, 8, 18)
in(53) = (left empty), 53, in(26) = 53, (15, 26)
⇒ 3, 17, 10, 8, 18, 12, 53, 15, 26
```
Post-order 展开（L R D）：
```
post(12) = post(17), post(53), 12
post(17) = 3, (10, 18, 8), 17
post(53) = (left empty), post(26), 53 = (15, 26), 53
⇒ 3, 10, 18, 8, 17, 15, 26, 53, 12
```

(i) **In-order**: **3, 17, 10, 8, 18, 12, 53, 15, 26**
(ii) **Post-order**: **3, 10, 18, 8, 17, 15, 26, 53, 12**

*（Pre-order 备用：12, 17, 3, 8, 10, 18, 53, 26, 15。以上四棵树的三种 traversal 都用程序跑过。）*

> 💬 **答题句 (EN)** — *Preorder visits the root, then traverses the left subtree in preorder, then the right subtree in preorder (DLR); inorder traverses the left subtree, visits the root, then the right subtree (LDR); postorder traverses the left subtree, then the right subtree, then visits the root (LRD).*

---

## 7. Graph 简介；Linear vs Non-linear（p77–82）

- **Graph**（p77）：另一种 non-linear 结构；**和 tree 不同，graph 没有 root**；任何 node 都可以用 edge 连到任何其他 node。比喻：连接城市的公路系统。
- **Directed graph (digraph)**（p79）：每条 edge 有方向，也叫 **arc**。比喻：航空班机（A → B 有班机不代表 B → A 有）。
- Graph 和 digraph 都可以用 **dynamic links** 或 **arrays** 表示；表示方式要方便想做的 operation（p81）。细节见 **Ch9**。

### ✅ 满分答法 — Dec 2025 Q3c (6 marks)
*A social networking app models friendships where each user can connect with multiple others. Tree or graph? Justify.*

A **graph** is the most suitable structure (2). Friendships are many-to-many relationships: any user can be connected to any number of other users, and there is no natural root or hierarchy, which is exactly what a graph models — vertices are users and edges are friendships (2). A tree cannot represent this because every node (except the root) has exactly one parent and a tree contains no cycles, whereas friendships naturally form cycles (A is a friend of B, B of C, and C of A) and a user may be linked to many others at the same level. The graph also supports operations the app needs, such as BFS to suggest "friends of friends" or to find the shortest connection between two users (2).

### Linear vs Non-linear（p82）

| Linear data structures | Non-linear data structures |
|---|---|
| Data elements organised in a **sequential** manner | Data elements **not** organised sequentially |
| Possible to traverse all elements **in a single run** | **Not** possible in a single run |
| **Easier** to implement | **Difficult** to implement |
| Examples: array, stack, queue, linked list | Examples: tree, graph |

---

## Closing the loop

回到图书馆的四个难题：书数量无法预测 → **dynamic**（linked list）；排序插入不想挪几千笔 → **linked list**，但要接受不能 random access；列印工作 → **queue**，"上一页" → **stack**；分类目录 → **tree**，读者互相推荐 → **graph**。下一章看这些结构背后的记忆体：变量到底住在 stack、heap 还是 static 区。

---

## ⚠️ Where the slides mislead

| Slide | 说法 | 更准确的理解 |
|---|---|---|
| p7, p11, p27 | Static：固定配置，**没有** overflow/underflow 问题；Dynamic：**可能** overflow/underflow | 一般教科书反而说 array 满了会 overflow。Slide 的意思是 dynamic 结构可能超出**允许的记忆体上限**。**考试照 slide 写** |
| p16, p27 | Array 的记忆体在 **compile time** 配置 | Java 的 array 其实在执行时用 `new` 于 heap 配置；重点是"一旦建立，大小固定"。照 slide 写可以 |
| p17 | "Memory requirement is less in array" 又说 "memory utilization is inefficient in the array" | 两句不矛盾：array **每个元素**占得少（没有 pointer），但**预留**的空间可能用不到 |
| p22 | "Arrays have better cache locality" 列为 linked list 的缺点 | 对，意思是 linked list 的 cache locality 较差 |
| p79 vs Ch9 p17 | Ch7 说航空班机是 **directed**；Ch9 p17 说 flight network 是 **undirected** | 看建模方式：若每条航线双向都飞可当 undirected。考试举例时说清楚方向即可 |
| p58 | "peek (or top)… empty" | Java 的 `Stack` class 用 `empty()`；`Deque` 用 `isEmpty()`；名称不同意义相同 |

---

## Term table

| English | 中文 | 一句话说明 |
|---|---|---|
| Static data structure | 静态资料结构 | 大小固定（array） |
| Dynamic data structure | 动态资料结构 | 可伸缩（linked list） |
| Heap | 堆积 | 动态配置记忆体的区域 |
| Overflow / Underflow | 溢位 / 下溢 | 超过上限 / 空了还取 |
| Contiguous | 连续的 | 记忆体相邻 |
| Random access | 随机存取 | 用 index 直接到任一元素 |
| Node | 节点 | 资料 + link |
| Head | 头 | 第一个 node 的 reference |
| Doubly linked list | 双向链结串列 | next + prev |
| Header node | 标头节点 | 存 count、front、rear |
| Cache locality | 快取区域性 | 相邻资料一起进 cache |
| Queue / FIFO | 伫列 / 先进先出 | enqueue at rear, dequeue at front |
| Stack / LIFO | 堆叠 / 后进先出 | push / pop / peek / empty at top |
| Enqueue / Dequeue | 入列 / 出列 | — |
| Push / Pop / Peek | 推入 / 弹出 / 查看 | — |
| Root / Leaf / Internal node | 根 / 叶 / 内部节点 | — |
| Binary tree | 二元树 | 每个 node 最多两个 children |
| Preorder / Inorder / Postorder | 前序 / 中序 / 后序 | DLR / LDR / LRD |
| Stock span | 股票跨度 | 连续几天价格 ≤ 当天 |

---

## Cheat sheet

- **Static**：fixed size，known in advance，array，easy to program，may waste memory，focus on performance。
- **Dynamic**：grow/shrink via heap，linked list，use when size unpredictable，efficient memory，harder（track size），may overflow/underflow（slide）。
- **Array**：index O(1) access · shifting on insert/delete · fixed · contiguous · better cache locality。
- **Linked list**：dynamic size · easy insert/delete · no random access（no binary search）· extra pointer memory · poor cache locality。
- **Insert after current**：`newNode.next = current.next; current.next = newNode;`（顺序不能反）。
- **Delete**：`prev.next = prev.next.next;`。
- **Queue**：FIFO；enqueue（rear）· dequeue/serve（front）· empty；linked list（front→rear）或 circular array（%）；print queue、scheduling。
- **Stack**：LIFO；push · pop · peek/top · empty；linked list（first node = top）或 array（bottom at 0）；undo、Back、function calls、postfix。
- **Tree**：root、leaf、internal；binary ≤ 2 children；BST O(log n) avg。
- **Traversal**：Pre DLR · In LDR · Post LRD；slide 树：1245367 / 4251637 / 4526731。
- **Dec 2025**：pre 3 9 6 17 20 15 7 · post 17 6 9 15 7 20 3。
- **Oct 2025**：in 3 17 10 8 18 12 53 15 26 · post 3 10 18 8 17 15 26 53 12。
- **Graph**：no root，any-to-any；social network → graph。

---

## Practice (answers included)

### A. MCQ
1. Which structure allows O(1) access by position? (a) linked list (b) array (c) queue (d) tree
2. Browser Back button behaviour is best modelled by: (a) queue (b) stack (c) set (d) graph
3. In the slide's example tree, the inorder output is: (a) 1 2 4 5 3 6 7 (b) 4 2 5 1 6 3 7 (c) 4 5 2 6 7 3 1 (d) 1 2 3 4 5 6 7
4. Which is **not** a disadvantage of a linked list? (a) no random access (b) extra pointer memory (c) dynamic size (d) poor cache locality
5. A queue implemented with an array uses which operator to wrap around? (a) / (b) % (c) * (d) ++

**Answers:** 1 (b) · 2 (b) · 3 (b) · 4 (c) · 5 (b)

### B. Short answer
**B1. State TWO ways to represent a stack for implementation. (2 marks)**
A singly linked list with the first node as the top of the stack; an array with the bottom of the stack at index 0.

**B2. Why can binary search not be used on a linked list efficiently? (2 marks)**
Binary search needs direct access to the middle element; a linked list has no random access, so reaching the middle requires traversing from the head, which takes linear time.

**B3. Explain why the order of the two statements when inserting a node matters. (2 marks)**
`newNode.next = current.next` must run first; if `current.next = newNode` runs first, the reference to the rest of the list is overwritten and lost, so the list is cut off after newNode.

### C. Application
**C1. Give the preorder, inorder and postorder of this tree. (6 marks)**
```
          A
        /   \
       B     C
        \   /
         D E
```
Preorder: A B D C E · Inorder: B D A E C · Postorder: D B E C A.

**C2. Show the stack after: push 5, push 8, pop, push 3, peek, push 9, pop. What does each pop/peek return? (4 marks)**
push 5 → [5]; push 8 → [5, 8]; pop → returns **8**, [5]; push 3 → [5, 3]; peek → returns **3**, unchanged; push 9 → [5, 3, 9]; pop → returns **9**, final stack [5, 3] (top = 3).

**C3. Show the queue after: enqueue 5, enqueue 8, dequeue, enqueue 3, enqueue 9, dequeue. (3 marks)**
[5] → [5, 8] → dequeue returns **5**, [8] → [8, 3] → [8, 3, 9] → dequeue returns **8**, final [3, 9] (front = 3).

### D. Thinking
**D1. A hospital emergency room must treat the most critical patient first, not the first to arrive. Is a queue suitable? (3 marks)**
A plain FIFO queue is not suitable because it serves patients in arrival order. A priority queue (listed in Ch2 and as a binary-tree application) is better, because it always removes the element with the highest priority.

**D2. When would an array still be preferred over a linked list for storing 10,000 sensor readings? (3 marks)**
When the number of readings is known and fixed and the program mostly reads them by index or scans them sequentially: the array gives O(1) access, no pointer overhead and better cache locality, and insertion in the middle is rarely needed.

---

## Slide index

| Note section | Slides |
|---|---|
| 1 Static | p3–7（图 p5） |
| 2 Dynamic | p8–11（图 p10） |
| 3 Array vs linked list | p12–27（图 p12, p24, 表 p27） |
| 4 Linked list implementation | p28–46（图 p29, p32, p41, p44, p45, p46） |
| 5 Queue / stack | p47–61（图 p49, p51, p57, p60） |
| 6 Tree / traversal | p62–76（图 p64, p67, p74–76） |
| 7 Graph intro, linear vs non-linear | p77–82（图 p78, p80, 表 p82） |

## Links to other chapters
- **Ch1**：linear / non-linear 分类
- **Ch6**：LinkedHashSet（hash + linked list）、TreeSet（tree）
- **Ch8**：heap 与 stack 记忆体（dynamic DS 用 heap；method call 用 stack）
- **Ch9**：graph 的完整内容；BFS 用 queue、DFS 用 stack / recursion
