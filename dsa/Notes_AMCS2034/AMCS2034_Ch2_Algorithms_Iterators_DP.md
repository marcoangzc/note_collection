# AMCS2034 — Chapter 2: Algorithms — Iterators, Strings, Dynamic Programming, Pattern Matching

> 这章把五个不太相关的主题放在一起。考卷只考两块：**Iterator**（hasNext/next 的差别、自己写 iterator class 的两个 method）和 **Dynamic Programming vs Divide and Conquer**。可是 slide p24 的 NameRepository 程序**被截断了**——最关键的 `hasNext()` / `next()` 根本没出现在 slide 上，Container interface 也没给。这份笔记补上完整、已用 javac 编译执行过的程序，并把 DP 用 Fibonacci 的呼叫次数讲清楚。

---

## 0. 一句话总览

**Collection 是装物件的容器；iterator 让你不用知道容器内部怎么存，也能一个一个拿出元素（hasNext 问"还有吗"，next 拿"下一个"）。Dynamic programming 则是"记住已经算过的子问题答案，不要重算"。**

| Part | 内容 | Slide |
|---|---|---|
| 1 | Collections 与 Java Collections Framework | p4–8 |
| 2 | Iterator 与 Iterable | p9–11 |
| 3 | Design patterns（GoF、两种用途、三种类型） | p12–18 |
| 4 | Iterator Pattern 实作（4 steps） | p19–26 |
| 5 | Java String 操作 | p27–35 |
| 6 | Dynamic Programming | p36–52 |
| 7 | Pattern Matching（java.util.regex） | p53–58 |

🎯 **这章回答的考题**
- **May 2025 Q2a** Differentiate `hasNext()` and `next()` (4)
- **Oct 2024 Q1b(i)** How iterators can be implemented in the inventory system (2)
- **Oct 2024 Q1b(ii)** Write TWO methods for an iterator class over a list of products (4)
- **Oct 2024 Q1b(iii)** Differentiate dynamic programming and divide and conquer (4)

**Prerequisites**：Java interface、inner class、`for` loop、recursion（SPC Ch9）。

---

## Scene：Tess Sdn Bhd 的仓库

Tess Sdn Bhd 做了一个家用 inventory system：产品放在不同 **section**，每个 section 有很多 **shelf**。今天 product 存在 `ArrayList` 里；下个月老板可能想改成 linked list，或按 section 分组的 map。

问题来了：系统里有几十个地方要"把所有产品列出来"。如果每个地方都写 `for (int i = 0; i < list.size(); i++) list.get(i)`，一旦换了储存方式，**几十个地方都要改**。

你需要一个**统一的"走访方式"**：不管里面怎么存，外面只问两件事——"还有下一个吗？""给我下一个"。这就是 iterator。

---

## 1. Collections（p4–8）

- Data structure = **a collection of data organised in some fashion**；它不只存资料，也提供存取和操作资料的 operation（p4）。
- 在 OOP 里，data structure 也叫 **container / container object**：一个存放其他物件（elements）的物件。**定义一个 data structure 就是定义一个 class**；建立 data structure 就是建立它的 instance（p5）。
- **Java Collections Framework** 支援两种 container（p6）：

| Container | 存什么 | 例子 |
|---|---|---|
| **Collection** | 一群 elements | Set, List, Queue |
| **Map** | **key/value pairs**；用 key 快速搜寻 | HashMap |

- 各种 collection 存什么（p7）——**Tutorial 3 Q2**：

| Collection | 存什么 |
|---|---|
| **Sets** | 一群**不重复**的元素 |
| **Lists** | **有顺序**的元素集合 |
| **Stacks** | 以 **LIFO**（last-in, first-out）处理的物件 |
| **Queues** | 以 **FIFO**（first-in, first-out）处理的物件 |
| **PriorityQueues** | 依**优先顺序**处理的物件 |

**p8 的架构图**（interfaces → abstract classes → concrete classes），重点部分：

```
Interfaces              Abstract classes          Concrete classes
Collection ◁── Set ◁── SortedSet ◁── NavigableSet ◁- - - - TreeSet
           │        ◁- - - AbstractSet ◁──────────── HashSet ◁── LinkedHashSet
           ├── List ◁- - - AbstractList ◁──────────── ArrayList, Vector ◁── Stack
           │                 ◁── AbstractSequentialList ◁── LinkedList
           └── Queue ◁── Deque ◁- - - - - - - - - - - LinkedList
                     ◁- - - AbstractQueue ◁────────── PriorityQueue
```
*看这张图要注意：HashSet、LinkedHashSet、TreeSet 都在 Set 这条线上（Ch6）；LinkedList 同时实作 List 和 Deque/Queue。*

---

## 2. Iterator 与 Iterable（p9–11）

- 每个 collection 都是 **Iterable**；可以取得它的 **Iterator** 物件来走访所有元素（p9）。
- Iterator 是一个经典 design pattern：**走访一个 data structure，而不需要暴露资料在里面是怎么存的**。
- **Collection** interface extends **Iterable** interface；Iterable 定义了 `iterator()` method，回传一个 Iterator（p10）。
- Iterator 提供**统一的方式**走访各种 collection 的元素（p10），三个 method（p11）：

| Method | 回传 | 做什么 |
|---|---|---|
| `hasNext()` | `boolean` | 检查 iterator 里**还有没有**元素；**不移动**位置 |
| `next()` | 下一个元素 | **回传**下一个元素，并把位置**往前移一格** |
| `remove()` | void | 删除**最后一次 `next()` 回传**的那个元素 |

```java
Iterator<String> it = list.iterator();   // Iterable.iterator()
while (it.hasNext()) {                    // 问：还有吗？
    String s = it.next();                 // 拿：下一个，并前进
    if (s.isEmpty()) it.remove();         // 删：刚刚拿到的那个
}
```

> 💬 **答题句 (EN)** — *The Iterator interface provides a uniform way to traverse the elements of any collection without exposing how the data is stored: hasNext() checks whether more elements remain, next() returns the next element and advances the iterator, and remove() removes the last element returned by next().*

### ✅ 满分答法 — May 2025 Q2a (4 marks)
*Differentiate between the operations supported by hasNext() and next() in the Iterator.*

| | `hasNext()` | `next()` |
|---|---|---|
| Purpose (1+1) | Checks whether there are more elements left to traverse in the collection. | Returns the next element in the collection. |
| Return / effect (1+1) | Returns a boolean (true/false) and does **not** move the iterator's position. | Returns the element itself and **advances** the iterator to the following element; calling it when no element remains throws `NoSuchElementException`. |

They are used together: `while (it.hasNext()) { x = it.next(); }` — hasNext() guards the loop and next() retrieves each element.

⚠️ **Tutorial 3 Q4** 问"THREE methods belonging to the **Iterable** interface"——严格来说 `hasNext()`、`next()`、`remove()` 属于 **Iterator** interface；Iterable 只有 `iterator()`（以及 Java 8 的 `forEach()`、`spliterator()`）。考试若这样问，写 hasNext / next / remove 三个，并在开头一句说明 "The iterator() method of the Iterable interface returns an Iterator, which provides…"。

---

## 3. Design Patterns（p12–18）

**Design pattern**：有经验的 OO 开发者的 **best practices**；针对软件开发常见问题的**通用解法**，是许多开发者**长时间 trial and error** 得来的（p12）——**Tutorial 3 Q5**。

**Gang of Four (GoF)**（p13–14）：1994 年 Erich Gamma、Richard Helm、Ralph Johnson、John Vlissides 出版 *Design Patterns – Elements of Reusable Object-Oriented Software*。两个 OO 设计原则（**Tutorial 3 Q6**）：
1. **Program to an interface, not an implementation**
2. **Favor object composition over inheritance**

**两种用途**（p15–16）：
1. **Common platform for developers**——标准术语。例如说"这里用 singleton"，大家就知道全程只用一个物件。
2. **Best practices**——长期演化出的最佳解法；新手学了可以更快学会软件设计。

**三种类型**（p17–18）：

| Type | 关心什么 | 例子（extra） |
|---|---|---|
| **Creational** | 建立物件，同时**隐藏建立逻辑**，不直接用 `new` | Singleton, Factory |
| **Structural** | class 和 object 的**组合**（用 inheritance 组合 interface） | Adapter, Composite |
| **Behavioral** | 物件之间的**沟通** | **Iterator**, Observer |

**Iterator pattern 属于 behavioral**（p19）：用来**依序存取 collection 物件的元素，不需要知道它的底层表示方式**。

---

## 4. Iterator Pattern 实作（p19–26）⭐

### 4.1 Class diagram（p21）

```
   <<interface>>                         <<interface>>
     Container                              Iterator
 +getIterator(): Iterator          +hasNext(): boolean
          ▲                        +next(): Object
          │ implements                      ▲
          │                                 │ implements
 IteratorPatternDemo ──uses──▶ NameRepository ──has──▶ NameIterator
 +main(): void                 -name: String[]          +hasNext(): boolean
                               +getIterator(): Iterator +next(): Object
```
*看这张图要注意：Demo 只认识 Container 和 Iterator 两个 interface；NameIterator 是 NameRepository 的 **private inner class**，外面看不到 names[] 是 array。*

### 4.2 完整程序（slide p24 被截断，下面是补齐版；javac 25 编译执行过）

**Step 1 — Create interfaces**
```java
// Iterator.java
public interface Iterator {
   public boolean hasNext();
   public Object next();
}

// Container.java   ← slide 没给这个档
public interface Container {
   public Iterator getIterator();
}
```

**Step 2 — Concrete class implementing Container, with inner class NameIterator**
```java
// NameRepository.java
public class NameRepository implements Container {
   public String names[] = {"Robert" , "John" ,"Julie" , "Lora"};

   @Override
   public Iterator getIterator() {
      return new NameIterator();
   }

   private class NameIterator implements Iterator {
      int index;                                   // ← 目前位置，初始 0

      @Override
      public boolean hasNext() {                   // ← slide 截掉的部分
         if (index < names.length) {
            return true;
         }
         return false;
      }

      @Override
      public Object next() {
         if (this.hasNext()) {
            return names[index++];                 // ← 先回传，再 index + 1
         }
         return null;
      }
   }
}
```

**Step 3 — Use the repository to get the iterator and print names**
```java
// IteratorPatternDemo.java
public class IteratorPatternDemo {
   public static void main(String[] args) {
      NameRepository namesRepository = new NameRepository();
      for (Iterator iter = namesRepository.getIterator(); iter.hasNext();) {
         String name = (String) iter.next();
         System.out.println("Name : " + name);
      }
   }
}
```

**Step 4 — Verify the output**（实际执行结果，与 slide p26 一致）
```
Name : Robert
Name : John
Name : Julie
Name : Lora
```

**Trace**（index 怎么走）：

| 呼叫 | index 前 | hasNext() | next() 回传 | index 后 |
|---|---|---|---|---|
| 1 | 0 | true | "Robert" | 1 |
| 2 | 1 | true | "John" | 2 |
| 3 | 2 | true | "Julie" | 3 |
| 4 | 3 | true | "Lora" | 4 |
| 5 | 4 | **false** → loop 结束 | — | 4 |

**Tutorial 3 Q7 — THREE steps**：(1) 建立 `Iterator` interface（hasNext、next）和 `Container` interface（getIterator）；(2) 建立实作 Container 的 concrete class，里面用 inner class 实作 Iterator；(3) 用这个 class 取得 iterator，以 `hasNext()`/`next()` 走访并印出元素（第 4 步是验证输出）。

### ✅ 满分答法 — Oct 2024 Q1b(i) (2 marks)
*Examine how the iterators can be implemented in the inventory management system.*

The inventory class (e.g. `Inventory`) implements a `Container`/`Iterable` interface whose `getIterator()`/`iterator()` method returns an iterator object. The iterator is implemented as an inner class that keeps the current position and provides `hasNext()` and `next()`, so the rest of the system can traverse all products in every section and shelf sequentially without knowing whether they are stored in an array, a list or a map.

### ✅ 满分答法 — Oct 2024 Q1b(ii) (4 marks)
*Assume the inventory is stored in a list of product objects. Write TWO methods for an iterator class that iterates over a collection of products.*

```java
import java.util.Iterator;
import java.util.List;
import java.util.NoSuchElementException;

public class ProductIterator implements Iterator<Product> {
    private List<Product> products;   // the inventory list
    private int index = 0;            // position of the next product

    public ProductIterator(List<Product> products) {
        this.products = products;
    }

    // Method 1: is there another product to visit?
    @Override
    public boolean hasNext() {
        return index < products.size();
    }

    // Method 2: return the current product and move forward
    @Override
    public Product next() {
        if (!hasNext()) {
            throw new NoSuchElementException("No more products");
        }
        return products.get(index++);
    }
}
```
使用方式与输出（已执行）：
```java
ProductIterator it = new ProductIterator(inventory);
while (it.hasNext()) {
    System.out.println(it.next());
}
```
```
Rice 5kg (Kitchen, shelf 1)
Detergent (Laundry, shelf 2)
Light bulb (Storeroom, shelf 3)
```
*给分点：`hasNext()` 正确回传 boolean（2）；`next()` 回传元素并前进 index（2）。`throw` 那行是加分的防呆，不写也可以用 `return null;`（slide 的写法）。*

---

## 5. Java Strings（p27–35）

| 主题 | 重点 |
|---|---|
| 定义（Tut 3 Q8） | String 是**字元的序列**；在 Java 里 string 是**物件**，由 `String` class 建立和操作 |
| 建立 | `String greeting = "Hello world!";`（literal，compiler 自动建立 String 物件）或 `new String(...)`（String class 有 11 个 constructor，例如用 char array） |
| **Immutable**（Tut 3 Q9） | String 物件**一旦建立就不能改变**；看起来"修改"其实是**产生新的 String** |
| 要大量修改 | 用 **`StringBuffer`** 或 **`StringBuilder`** |
| `length()` | accessor method，回传字元数 |
| `concat()` | `string1.concat(string2)` 回传**新**字串；更常用 `+` |
| `String.format()` | 和 `printf()` 同格式，但**回传 String** 可重复使用，不是直接印出 |

已执行验证：
```java
String s = "Hello";
String t = s.concat(" world");       // s 仍是 "Hello"（immutable）
System.out.println(s + " | " + t + " | length=" + t.length());
StringBuilder sb = new StringBuilder("Hello");
sb.append(" world");                 // 同一个物件被修改
```
```
Hello | Hello world | length=11
```

> 💬 **答题句 (EN)** — *The String class is immutable: once a String object is created its value cannot be changed, and operations such as concat() return a new String. If a string must be modified many times at runtime, StringBuilder or StringBuffer should be used.*

---

## 6. Dynamic Programming（p36–52）⭐

### 6.1 问题：递回在重复做同一件事

Slide 的 Fibonacci 定义（p46）：`Fib(0) = 1, Fib(1) = 1, Fib(n) = Fib(n−1) + Fib(n−2)` → 1, 1, 2, 3, 5, 8, 13, 21…

纯递回版本（p47）：
```java
int fib(int n) {
    if (n < 2)
        return 1;
    return fib(n-1) + fib(n-2);
}
```

p50 的递回树（fib(6)）：
```
                         fib(6)
                /                     \
           fib(5)                     fib(4)
          /      \                   /      \
      fib(4)     fib(3)          fib(3)    fib(2)
      /    \     /    \          /    \
  fib(3) fib(2) fib(2) fib(1) fib(2) fib(1)
  /    \
fib(2) fib(1)
```
*看这张图要注意：fib(3) 算了 3 次、fib(4) 算了 2 次、fib(2) 算了 5 次——全部是重复工作。*

实际执行 slide 的 code（base case `n < 2`，所以 fib(2) 还会再拆成 fib(1)+fib(0)）：

| | 结果 |
|---|---|
| fib(6) | **13**，共 **25 次**呼叫 |
| 各值被算的次数 | fib(0)×5, fib(1)×8, fib(2)×5, fib(3)×3, fib(4)×2, fib(5)×1, fib(6)×1 |
| fib(30) | **2,692,537 次**呼叫——呼叫次数呈指数成长 |

### 6.2 解法：记住算过的答案

DP 版本（p48，**memoization / bottom-up**）：
```java
void fib() {
    fibresult[0] = 1;
    fibresult[1] = 1;
    for (int i = 2; i < n; i++)
        fibresult[i] = fibresult[i-1] + fibresult[i-2];   // ← 只查表，不重算
}
```
n = 7 执行结果：`fibresult = 1 1 2 3 5 8 13`——每个值只算一次，共 n 步，O(n)。

p45 的比喻："1+1+1+1+1+1+1+1 = 8"，再加一个 "1+" 你马上知道是 9，因为你**记得**前面是 8。*Dynamic programming is just a fancy way to say remembering stuff to save time later.*

### 6.3 定义与特性（p37–42）

- 和 divide and conquer 一样把问题拆成越来越小的 sub-problems；**但 sub-problems 不是独立解**，而是**记住**较小 sub-problem 的结果，用在相似或**重叠（overlapping）**的 sub-problems（p37）。
- 用于可拆成**相似** sub-problems、结果可**重用**的问题；**多用于 optimization**；解新的 sub-problem 前，先查已解过的结果；把 sub-problem 的解**组合**成最佳解（p38）。
- 条件（p39）：(1) 问题可拆成**较小的 overlapping sub-problems**；(2) 用较小 sub-problems 的**最佳解**可以得到**整体最佳解**（optimal substructure）；(3) 使用 **memoization**。
- 对比（p40）：
  - **Greedy**：只做**局部**最佳化；**DP**：追求**整体**最佳化。
  - **Divide and conquer**：把各 sub-problem 的解**合起来**得到整体解；**DP**：用较小 sub-problem 的输出去**最佳化**较大的 sub-problem。
- 可用 **top-down**（递回 + memo）或 **bottom-up**（从小算到大填表）两种方式；**查表比重算便宜**（p42）。
- 例子（p41）：Fibonacci、Knapsack、Tower of Hanoi、Floyd-Warshall all-pair shortest path、Dijkstra shortest path、project scheduling。
- 两类 DP 问题（p51）：**Optimization problems**（找使函数最大/最小的可行解）；**Combinatorial problems**（算有几种做法或某事件的机率）。

**DP 的四步 schema（p52）——Tutorial 3 Q12**：
1. 证明问题可以拆成**最佳的 sub-problems**。
2. 用较小 sub-problems 的最佳解，**递回地定义**解的值。
3. 以 **bottom-up** 方式计算最佳解的值。
4. 从计算出的资讯**建构**最佳解。

**Benefit（Tutorial 3 Q11）**：避免重复计算——每个 sub-problem 只算一次，把指数时间（Fibonacci 递回）降到线性时间，代价是用一点额外记忆体存结果（space/time trade-off，见 Ch4）。

> 💬 **答题句 (EN)** — *Dynamic programming breaks a problem into smaller overlapping sub-problems, solves each sub-problem only once and stores (memoizes) its result so that it can be reused, and combines these results to obtain the overall optimal solution.*

### 6.4 Divide and Conquer（slide 没独立讲，这里补上 — *extra*）

把问题拆成**互相独立**的 sub-problems → 分别（通常递回）解 → 把结果**合并（combine）**。例子：**merge sort**（拆两半各自排序再合并）、**binary search**（每次只留一半）。因为 sub-problems 不重叠，不需要存中间结果。

### ✅ 满分答法 — Oct 2024 Q1b(iii) (4 marks)
*Differentiate dynamic programming and divide and conquer.*

| Aspect | Divide and conquer | Dynamic programming |
|---|---|---|
| Sub-problems (1) | Divides the problem into **independent**, non-overlapping sub-problems. | Divides the problem into **overlapping** sub-problems that share smaller sub-problems. |
| Reuse of results (1) | Each sub-problem is solved separately; results are **not stored**, so repeated sub-problems would be recomputed. | The result of each sub-problem is **stored (memoization)** and reused, so each is solved only once. |
| How the answer is built (1) | Solutions of sub-problems are **combined** to form the overall solution. | Outputs of smaller sub-problems are used to **optimise** larger sub-problems, bottom-up or top-down. |
| Typical use / example (1) | Sorting and searching, e.g. **merge sort** of products by price or binary search of a product ID. | Optimisation problems, e.g. choosing the best combination of items to restock within a budget (**knapsack**) or Fibonacci. |

---

## 7. Pattern Matching（p53–58）

`java.util.regex` package 用 **regular expression** 做 pattern matching：一串特殊字元，用来**比对 / 寻找**其他字串（搜寻、编辑、处理文字）。三个 class（**Tutorial 3 Q13**）：

| Class | 作用 |
|---|---|
| **Pattern** | regular expression 的**编译后表示**；没有 public constructor，要用 static `Pattern.compile(regex)` 取得 |
| **Matcher** | 解读 pattern 并对 input string 执行比对的**引擎**；没有 public constructor，用 `pattern.matcher(input)` 取得 |
| **PatternSyntaxException** | **unchecked exception**，表示 regex 的**语法错误** |

p58 程序执行结果：
```java
Pattern pat = Pattern.compile("JavaFX");
Matcher mat = pat.matcher("JavaFX");      // → "1 : Matches"
mat = pat.matcher("JavaSwing");           // → "2 : No Match"
```
```
1 : Matches
2 : No Match
```
`matches()` 要**整个字串**都符合 pattern 才是 true。`Pattern.compile("Java(")` 会丢出 `PatternSyntaxException: Unclosed group`（已测试）。

---

## Closing the loop

回到 Tess Sdn Bhd：只要 inventory class 提供一个 iterator，几十个"列出产品"的地方都只写 `while (it.hasNext()) it.next()`；哪天从 ArrayList 换成别的结构，只要改 iterator 那一个 class。而"重复做同一件事"这个主题会在下一章变成量化的问题：一个 algorithm 到底做了多少工作？

---

## ⚠️ Where the slides mislead

| Slide | 说法 | 更准确的理解 |
|---|---|---|
| p24 | NameRepository 程序在 `int index;` 就结束 | 缺 `hasNext()`、`next()` 和 Container interface——本笔记 §4.2 已补齐并执行 |
| p48 | `void fib()` 里用到的 `n` 和 `fibresult` 没有宣告 | 它们是 class 的 field；loop 填的是 fibresult[0..n−1] |
| p46–47 vs p50 | code 的 base case 是 `n < 2`（fib(0)=fib(1)=1），但 p50 的树把 fib(2) 画成叶子 | 树是示意图；照 code 跑 fib(6)=13、25 次呼叫。考试画树时照 slide 画法即可 |
| Tut 3 Q4 | "THREE methods belonging to the **Iterable** interface" | hasNext/next/remove 是 **Iterator** 的 method；Iterable 只提供 iterator() |
| p41 | Tower of Hanoi 列为 DP 问题 | Tower of Hanoi 是 recursion 经典题，没有重叠子问题；照 slide 列出不会扣分 |

---

## Term table

| English | 中文 | 一句话说明 |
|---|---|---|
| Collection / Container | 集合 / 容器 | 存放其他物件的物件 |
| Map | 映射 | 存 key/value pairs |
| Iterable | 可迭代的 | 提供 iterator() 的 interface |
| Iterator | 迭代器 | hasNext / next / remove，统一走访 |
| Design pattern | 设计模式 | 常见问题的最佳实践解法 |
| Gang of Four (GoF) | 四人帮 | 1994 年 Design Patterns 一书的四位作者 |
| Creational / Structural / Behavioral | 建立型 / 结构型 / 行为型 | 三类 design pattern |
| Immutable | 不可变 | 建立后值不能改 |
| StringBuilder / StringBuffer | — | 可修改的字串类别 |
| Dynamic programming | 动态规划 | 记住子问题的解，避免重算 |
| Memoization | 记忆化 | 存下已算过的结果 |
| Overlapping sub-problems | 重叠子问题 | 同一个子问题出现多次 |
| Optimal substructure | 最佳子结构 | 大问题的最佳解由子问题最佳解组成 |
| Top-down / Bottom-up | 由上而下 / 由下而上 | 递回+记忆 / 从小填表 |
| Divide and conquer | 分治法 | 拆成独立子问题再合并 |
| Greedy | 贪婪法 | 每步取局部最佳 |
| Regular expression | 正规表示式 | 描述字串 pattern 的语法 |

---

## Cheat sheet

- **JCF 两种 container**：Collection（elements）· Map（key/value）。
- **Set** 不重复 · **List** 有序 · **Stack** LIFO · **Queue** FIFO · **PriorityQueue** 按优先。
- **Collection extends Iterable**；`iterator()` 回传 Iterator。
- **hasNext()** → boolean，不移动 · **next()** → 元素并前进 · **remove()** → 删最后回传的那个。
- **Design pattern**：best practices，trial and error 得来；GoF 1994；两原则：program to an interface · favor composition over inheritance；两用途：common platform · best practices；三类：creational · structural · behavioral（iterator = behavioral）。
- **Iterator pattern 3 steps**：interfaces（Iterator, Container）→ concrete class + inner iterator → use it（→ verify output）。
- **String**：物件、immutable；大量修改用 StringBuilder/StringBuffer；length() · concat() · + · String.format()。
- **DP**：overlapping sub-problems + memoization + optimal substructure；top-down / bottom-up；vs greedy（local）、vs D&C（independent, combine）。
- **DP 4 steps**：optimal sub-problems → recursive definition → compute bottom-up → construct solution。
- **regex 3 classes**：Pattern（compile）· Matcher（matcher, matches）· PatternSyntaxException（unchecked）。

---

## Practice (answers included)

### A. MCQ
1. Which method of Iterator does **not** advance the position? (a) next() (b) hasNext() (c) remove() (d) iterator()
2. The Iterator pattern belongs to which category? (a) Creational (b) Structural (c) Behavioral (d) Functional
3. Which class should be used for a string that changes many times? (a) String (b) StringBuilder (c) Pattern (d) Matcher
4. Dynamic programming differs from divide and conquer because its sub-problems are: (a) independent (b) overlapping and stored (c) always two halves (d) solved greedily
5. How do you obtain a Pattern object? (a) `new Pattern()` (b) `Pattern.compile(regex)` (c) `matcher.pattern()` only (d) `String.format()`

**Answers:** 1 (b) · 2 (c) · 3 (b) · 4 (b) · 5 (b)

### B. Short answer
**B1. Explain what is meant by "String is immutable" and its consequence. (3 marks)**
Once a String object is created its content cannot be changed. Any "modification" such as concat() or + creates a new String object, so repeated modification in a loop creates many temporary objects and is inefficient; StringBuilder/StringBuffer should be used instead.

**B2. State the TWO principles of object-oriented design on which GoF design patterns are based. (2 marks)**
Program to an interface, not an implementation; favor object composition over inheritance.

**B3. Describe ONE benefit of dynamic programming. (2 marks)**
It avoids recomputing the same sub-problem by storing its result, e.g. computing Fibonacci with a table needs only n steps instead of an exponential number of recursive calls.

### C. Application
**C1. A library system stores books in an array `Book[] books`. Write an inner iterator class with hasNext() and next(). (4 marks)**
```java
private class BookIterator implements Iterator<Book> {
    private int index = 0;
    public boolean hasNext() { return index < books.length; }
    public Book next() {
        if (!hasNext()) throw new NoSuchElementException();
        return books[index++];
    }
}
```

**C2. Using the slide's recursive fib (base case n < 2), how many times is fib(1) called when computing fib(5)? (2 marks)**
fib(5) calls: fib(4)+fib(3); counting leaves gives fib(1) called **5 times** (and fib(0) 3 times; fib(5) = 8). Working: calls(fib(1)) for n = 2,3,4,5 = 1, 2, 3, 5 (it follows the Fibonacci pattern).

**C3. The NameRepository iterator is used with `names = {"A", "B"}`. After both names are printed, what does hasNext() return, and why? (2 marks)**
It returns **false**. After two next() calls, index = 2, which equals names.length, so the condition `index < names.length` is false and the for loop ends.

### D. Thinking
**D1. Why is the iterator implemented as a private inner class instead of letting the client use the array directly? (3 marks)**
The inner class can access the private array but the client cannot, so the internal representation stays hidden (encapsulation). The client depends only on the Iterator interface, so the storage can change from an array to a list without changing client code — this is "program to an interface, not an implementation".

**D2. A student says "DP is always better than plain recursion." Evaluate. (3 marks)**
Not always. DP helps only when sub-problems overlap; it trades memory for time. For problems with independent sub-problems (merge sort) storing results gives no benefit, and for small inputs the extra table may not be worth it.

---

## Slide index

| Note section | Slides |
|---|---|
| 1 Collections | p4–8（图 p8） |
| 2 Iterator | p9–11 |
| 3 Design patterns | p12–18 |
| 4 Iterator pattern | p19–26（图 p21；code p22–26） |
| 5 Strings | p27–35 |
| 6 Dynamic programming | p36–52（图 p43, p50） |
| 7 Pattern matching | p53–58 |

## Links to other chapters
- **Ch1**：algorithm 的定义与 characteristics
- **Ch3–5**：怎么量化 recursion vs DP 的效率（O(2ⁿ) vs O(n)）
- **Ch4**：DP 用记忆体换时间 = space/time trade-off
- **Ch6**：Set 也是 Iterable，可用 for-each / iterator 走访
