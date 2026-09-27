# AMCS2034 — Chapter 6: Sets

> **这章是全科分数最高的一章之一**：近两份考卷 Q2 **整题 25 分都是 Set**，其中一个 **Java 程序题就 16–17 分**（建两个 HashSet → 找共同 / 独有的元素 → 用 loop 印出并印总数）。Slide 的程序输出顺序是旧版 Java 跑的，**和现在的 JDK 不一样**；p21 的 LinkedHashSet 程序**根本编译不过**（`Object` 没有 `toLowerCase()`）；p15 把 retainAll 的输出标成"removing common elements"，而且结果是空集合，容易误会 intersection 是空的。这份笔记把 slide 和考卷的所有程序都用 javac 25 实际跑过，给出能直接背的程序模板。

---

## 0. 一句话总览

**Set 是"不重复元素"的集合。Java 有三个 concrete class：HashSet（最快、没有顺序）、LinkedHashSet（保留插入顺序）、TreeSet（自动排序）。Set 的三个多值操作是 union（addAll）、intersection（retainAll）、difference（removeAll）。**

| Part | 内容 | Slide |
|---|---|---|
| 1 | Set 是什么；三个 concrete class | p3–6 |
| 2 | HashSet：capacity、load factor、hashCode | p7–15 |
| 3 | Union / Intersection / Difference | p16–18 |
| 4 | LinkedHashSet | p19–22 |
| 5 | TreeSet（SortedSet、NavigableSet） | p23–34 |
| 6 | 三者比较（考卷最爱） | p22, p33 + extra |
| 7 | 考卷程序题模板 | — |

🎯 **这章回答的考题**
- **May 2025 Q3a(i)(ii)(iii)** HashSet 程序：有没有重复加入？输出会不会重复？HashSet 怎么保证唯一？(2+3+2)
- **May 2025 Q3c** HashSet vs LinkedHashSet ordering (4)
- **Oct 2024 Q3a** THREE concrete classes to create a set (3)
- **Oct 2024 Q3b** HashSet of countries program (15)
- **Oct 2024 Q3c** HashSet vs TreeSet ordering (3)
- **Dec 2025 Q2a** TWO advantages of Set over Array (4)
- **Dec 2025 Q2b** HashSet vs LinkedHashSet：ordering + efficiency (4)
- **Dec 2025 Q2c** Program：items unique to CounterA (17)
- **Oct 2025 Q2a** THREE main operations of Set with multiple values (3)
- **Oct 2025 Q2b** HashSet vs LinkedHashSet vs TreeSet：ordering + performance (6)
- **Oct 2025 Q2c** Program：common items of two HashSets (16)

**Prerequisites**：Java generics（`Set<String>`）、for-each loop、Ch2 Iterable/Iterator。

---

## Scene：BrightNote 文具店的两个柜台

BrightNote 有两个柜台，各自记录卖什么：

- Counter A："notebook"、"pencil"、"highlighter"
- Counter B："pencil"、"eraser"、"marker"

店长想知道：**两边都有卖的**是什么（可以一起做促销）、**只有 A 有卖的**是什么（要帮 B 补货）。

用 array 做的话：每次加一个品项都要先检查有没有重复；找共同品项要写两层 loop 逐一比对。有没有一种结构，**本身就不允许重复**，而且"共同""独有"这种运算一行就能做完？

---

## 1. Set 是什么（p3–6）

- 在电脑科学里，**set** 是一种 abstract data type，存**唯一（unique）的值**，**没有特定顺序**；是数学上"有限集合"的电脑实作（p3）——**Tutorial 6 Q1**。
- 常用来**测试某个元素是否属于**一组值；并用 **union、intersection、difference** 一次处理多个值（p4）——**Tutorial 6 Q2**。
- **A set is an efficient data structure for storing and processing non-duplicate elements.** 三个 concrete class：**HashSet、LinkedHashSet、TreeSet**（p5）。
- `Set` interface **extends `Collection`**；它**没有新增任何 method 或常数**，只是保证 Set 的 instance **不包含重复元素**（p6）。

> 💬 **答题句 (EN)** — *A set is an abstract data type that stores unique values without duplicates; in Java the Set interface extends Collection and adds no new methods, but guarantees that an instance contains no duplicate elements. It can be created using the concrete classes HashSet, LinkedHashSet or TreeSet.*

### ✅ 满分答法 — Oct 2024 Q3a (3 marks)
*Provide THREE concrete classes to create a set.*
1. **HashSet** – stores elements with no particular order (1)
2. **LinkedHashSet** – keeps elements in insertion order (1)
3. **TreeSet** – keeps elements in sorted order (1)

### ✅ 满分答法 — Oct 2025 Q2a (3 marks)
*State THREE main operations of the Set data structure used with multiple values.*
1. **Union** – combine all elements of two sets without duplicates, `set1.addAll(set2)` (1)
2. **Intersection** – keep only the elements common to both sets, `set1.retainAll(set2)` (1)
3. **Difference** – remove from one set all elements that are in the other, `set1.removeAll(set2)` (1)

### ✅ 满分答法 — Dec 2025 Q2a (4 marks)
*Describe TWO advantages of using a Set instead of an Array to store the items at the counters.*

1. **No duplicates automatically** (2) – a Set rejects an element that is already present (add() returns false), so an item such as "pencil" can never be recorded twice at a counter; with an array the program must search the whole array before every insertion to avoid duplicates.
2. **Built-in set operations and fast membership test** (2) – a Set provides union, intersection and difference (addAll, retainAll, removeAll) and a contains() check that takes about constant time in a HashSet, so finding items available at both counters or unique to one counter takes one method call, whereas an array needs nested loops comparing every pair of items. *(Also: a Set grows dynamically, while an array has a fixed size.)*

---

## 2. HashSet（p7–15）⭐

### 2.1 建立、capacity、load factor（p8–9）— Tutorial 6 Q3, Q4

- **HashSet** 是实作 Set 的 concrete class；可以用 **no-arg constructor** 建空的，或**从现有 collection** 建立。
- 预设 **initial capacity = 16**，**load factor = 0.75**。知道大小的话可以在 constructor 指定。load factor 介于 0.0 和 1.0。
- **Load factor** 衡量 set 在容量增加前**可以多满**：当元素数量**超过 capacity × load factor** 时，capacity **自动加倍**。
  - 例：capacity 16、load factor 0.75 → 16 × 0.75 = **12**；size 达到 12 时 capacity 变 **32**。
- **Load factor 较高** → 省空间，但**搜寻时间变长**；预设 **0.75 是时间与空间的良好折衷**（又是 space/time trade-off，见 Ch4）。

> 💬 **答题句 (EN)** — *The load factor measures how full a HashSet is allowed to become before its capacity is increased: when the number of elements exceeds capacity × load factor, the capacity is doubled. With the default capacity 16 and load factor 0.75, the capacity doubles to 32 when the size reaches 12. A higher load factor saves space but increases search time.*

### 2.2 hashCode 与唯一性（p10–11）

- 加进 hash set 的物件要实作 **hashCode()**，而且要**把 hash code 分散得好**。
- **两个相等（equals）的物件，hash code 必须相同**。两个不相等的物件**可以**有相同 hash code（collision），但要尽量避免。
- Java API 的例子：`Integer` 的 hashCode 回传它的 int 值；`Character` 回传字元的 Unicode；`String` 回传 s₀·31⁽ⁿ⁻¹⁾ + s₁·31⁽ⁿ⁻²⁾ + … + sₙ₋₁。

**HashSet 怎么保证唯一（考过 2 分）**：
```
add(x):
  1. 计算 x.hashCode() → 决定放在哪个 bucket（桶）
  2. 该 bucket 里有没有 y 使 y.equals(x)？
       有 → 不加入，add() 回传 false
       没有 → 加入，add() 回传 true
```
（*extra*：HashSet 内部其实是一个 HashMap，元素当作 key。）
实测：`"Kota Kinabalu".hashCode()` 两次都是 −1458778744；第二次 `add("Kota Kinabalu")` 回传 **false**。

### 2.3 Slide 的 TestHashSet（p12）

```java
Set<String> set = new HashSet<>();
set.add("London");
set.add("Paris");
set.add("New York");
set.add("San Francisco");
set.add("Beijing");
set.add("New York");                 // ← 重复，不会存第二次
System.out.println(set);
for (String s: set) {
   System.out.print(s.toUpperCase() + " ");
}
```

| | 输出 |
|---|---|
| Slide（旧 JDK） | `[San Francisco, New York, Paris, Beijing, London]` |
| **javac/java 25 实跑** | `[San Francisco, Beijing, New York, London, Paris]` |

⚠️ **两个顺序不一样很正常**：HashSet **不保证任何顺序**，顺序取决于 hash 值和 JDK 内部实作。考试若问输出，写"elements are displayed in no particular (unpredictable) order"，**不要**说"按插入顺序"。重点是：**New York 只出现一次**。

### ✅ 满分答法 — May 2025 Q3a (2 + 3 + 2 marks)
```java
Set<String> cities = new HashSet<>();
cities.add("Kota Kinabalu");
cities.add("Kuching");
cities.add("Ipoh");
cities.add("Kota Kinabalu");
for (String city : cities) {
    System.out.println(city);
}
```
实跑输出（javac 25）：
```
Ipoh
Kota Kinabalu
Kuching
```

**(i) Determine if any cities are added to the set more than once. (2)**
Yes. "Kota Kinabalu" is added twice, at line 6 and again at line 9 (1). However, the second add() call is rejected because the element already exists, so the set stores it only once and contains three elements (1).

**(ii) Predict whether any city names will appear multiple times in the output. Explain. (3)**
No, each city name appears only once (1). A HashSet does not allow duplicate elements, so the second "Kota Kinabalu" is not stored and add() returns false (1). The loop therefore prints the three unique cities — Kota Kinabalu, Kuching and Ipoh — in no particular order, since a HashSet does not maintain insertion order (1).

**(iii) Briefly explain the mechanism a HashSet uses to ensure each element is unique. (2)**
When an element is added, HashSet calls its hashCode() method to find the bucket where it should be stored (1), then uses equals() to compare it with the elements already in that bucket; if an equal element is found the new element is not added and add() returns false (1).

### 2.4 Collection 的方法（p13–15）

| Method | 作用 |
|---|---|
| `add(e)` | 加入；已存在则回传 false |
| `remove(e)` | 删除 |
| `size()` | 元素数量 |
| `contains(e)` | 是否存在 |
| `addAll(c)` | **Union** |
| `retainAll(c)` | **Intersection** |
| `removeAll(c)` | **Difference** |

实跑 TestMethodsInCollection（javac 25，顺序与 slide 不同，但内容相同）：
```
set1 is [San Francisco, Beijing, New York, London, Paris]
5 elements in set1

set1 is [San Francisco, Beijing, New York, Paris]
4 elements in set1

set2 is [Shanghai, London, Paris]
3 elements in set2

Is Taipei in set2? false

After adding set2 to set1, set1 is [San Francisco, Beijing, New York, Shanghai, London, Paris]
After removing set2 from set1, set1 is [San Francisco, Beijing, New York]
After removing common elements in set2 from set1, set1 is []
```
⚠️ 最后一行为什么是 `[]`？因为**前一行已经 removeAll 把 set2 的元素全拿掉了**，这时 set1 和 set2 没有交集，retainAll 才会得到空集合。**不是 retainAll 删掉共同元素**——retainAll 是**保留**共同元素。slide 的输出文字 "removing common elements" 是误导。

---

## 3. Union / Intersection / Difference（p16–18）⭐

用 BrightNote：A = {notebook, pencil, highlighter}，B = {pencil, eraser, marker}

```
        Counter A                 Counter B
   ┌─────────────────┬──────┬────────────────┐
   │ notebook        │      │ eraser         │
   │ highlighter     │pencil│ marker         │
   └─────────────────┴──────┴────────────────┘
     A − B (difference)  A ∩ B    B − A
```
*看这张图要注意：中间那格是 intersection；左边那块是 A − B（只在 A）。*

| Operation | Java | 意思 | 结果（实跑） |
|---|---|---|---|
| **Union** A ∪ B | `a.addAll(b)` | 把一个 set 加进另一个；**重复的会自动忽略** | {eraser, highlighter, marker, pencil, notebook}，size 5 |
| **Intersection** A ∩ B | `a.retainAll(b)` | 只**保留**两边都有的 | {pencil} |
| **Difference** A − B | `a.removeAll(b)` | 从 a 删掉所有在 b 里的 | {highlighter, notebook} |

⚠️ 这三个 method 会**直接修改呼叫它的那个 set**。要保留原本的 set，先**复制一份**：
```java
Set<String> result = new HashSet<>(counterA);   // 复制
result.retainAll(counterB);                      // counterA 不变
```

> 💬 **答题句 (EN)** — *Union combines two sets without duplicates (addAll); intersection retains only the elements common to both sets (retainAll); difference removes from one set all elements contained in the other (removeAll).*

---

## 4. LinkedHashSet（p19–22）

- **LinkedHashSet extends HashSet**，加上 **linked-list** 实作，支援元素的**顺序**（p20）。
- HashSet 的元素没有顺序；**LinkedHashSet 可以按插入顺序取出**。有四个 constructor，与 HashSet 类似。
- 不需要维持插入顺序时，用 **HashSet，它比 LinkedHashSet 更有效率**（p22）。要其他顺序（递增 / 递减），用 **TreeSet**。

Slide p21 程序**有 bug**：
```java
for (Object element: set)
    System.out.print(element.toLowerCase() + " ");   // ✗ 编译错误
```
javac：`cannot find symbol: method toLowerCase() … variable element of type Object`。
修正：把 `Object` 改成 `String`。修正后实跑：
```
[London, Paris, New York, San Francisco, Beijing]
london paris new york san francisco beijing
```
——**完全按照插入顺序**，New York 只出现一次（和 slide p22 一致）。

---

## 5. TreeSet（p23–34）

### 5.1 SortedSet 与 NavigableSet（p24–25）

- **SortedSet**（Set 的 subinterface）保证元素**已排序**；提供 `first()`、`last()`、`headSet(toElement)`（< toElement 的部分）、`tailSet(fromElement)`（≥ fromElement 的部分）。
- **NavigableSet** extends SortedSet，提供 `lower(e)`（< e 的最大者）、`floor(e)`（≤ e 的最大者）、`ceiling(e)`（≥ e 的最小者）、`higher(e)`（> e 的最小者），没有就回传 null；`pollFirst()`、`pollLast()` 删除并回传第一个 / 最后一个。
- **TreeSet implements SortedSet**（其实是 NavigableSet）；元素之间必须能比较：用 **Comparable**（compareTo）或 **Comparator**（compare）（p26, p34）。

### 5.2 TestTreeSet（p27–31）——实跑结果与 slide 完全一致

```java
Set<String> set = new HashSet<>();
// add London, Paris, New York, San Francisco, Beijing, New York
TreeSet<String> treeSet = new TreeSet<>(set);    // 从 HashSet 建立 → 自动排序
```
```
Sorted tree set: [Beijing, London, New York, Paris, San Francisco]
first(): Beijing
last(): San Francisco
headSet("New York"): [Beijing, London]
tailSet("New York"): [New York, Paris, San Francisco]
lower("P"): New York
higher("P"): Paris
floor("P"): New York
ceiling("P"): Paris
pollFirst(): Beijing
pollLast(): San Francisco
New tree set: [London, New York, Paris]
```
*注意 headSet **不含** "New York"，tailSet **包含** "New York"。"P" 不在 set 里，所以 lower = floor = New York，higher = ceiling = Paris。*

- 每个 JCF concrete class 至少有两个 constructor：no-arg，以及**从一个 collection 建立**（`new TreeSet<>(set)`）（p32）。
- **HashSet vs TreeSet（p33）**：更新时不需要维持排序，就用 hash set——**插入和删除比较快**；需要排序时，再从 hash set 建一个 tree set。

---

## 6. 三者比较 ⭐⭐

| | **HashSet** | **LinkedHashSet** | **TreeSet** |
|---|---|---|---|
| Ordering | **No particular order**（不可预测） | **Insertion order** | **Sorted order**（natural order 或 Comparator） |
| 内部结构（*extra*） | Hash table | Hash table + **doubly linked list** | **Red-black tree**（平衡二元搜寻树） |
| add / remove / contains | **O(1)** average — **最快** | O(1) average，**稍慢**（要维护 linked list） | **O(log n)** — 最慢（要保持排序） |
| 记忆体 | 最少 | 较多（每个元素多存前后 link） | 较多（树的 node） |
| 允许 null（*extra*） | 可以一个 | 可以一个 | 不行（要比较） |
| 适合 | 只关心"有没有"、要最快 | 要不重复**又**要保留加入顺序 | 要按字母 / 数值排序、要 first/last/range |

> 💬 **答题句 (EN)** — *HashSet stores elements in no particular order and gives the fastest add, remove and contains operations (constant time on average); LinkedHashSet maintains insertion order using an additional linked list, so it is slightly slower and uses more memory; TreeSet keeps elements in sorted order, which costs O(log n) time per operation.*

### ✅ 满分答法 — May 2025 Q3c (4 marks)
*Compare and contrast HashSet and LinkedHashSet in terms of their ordering elements.*

*Similarity* (1): both implement the Set interface (LinkedHashSet extends HashSet), so neither stores duplicate elements, and both use hashing to store elements.
*Difference* (3): a HashSet does not maintain any order — elements are returned in an unpredictable order determined by their hash codes, which may differ from the order they were added (1). A LinkedHashSet maintains the **insertion order** by linking the elements in a doubly linked list, so iterating returns elements in exactly the order they were inserted (1). Therefore, if the order of insertion must be preserved (e.g. displaying items in the order customers added them), LinkedHashSet is used; otherwise HashSet is preferred because it is more efficient (1).

### ✅ 满分答法 — Oct 2024 Q3c (3 marks)
*Differentiate between HashSet and TreeSet based on their ordering characteristics.*

A HashSet stores elements in **no particular order**; the iteration order depends on the hash codes and is unpredictable (1). A TreeSet stores elements in **sorted (ascending) order**, using the elements' compareTo() method (Comparable) or a Comparator (1). Because TreeSet must keep the elements sorted, its insertion and removal are slower (O(log n)) than HashSet's (O(1) on average), so HashSet is used when order is not needed and TreeSet when sorted output or first()/last() is required (1).

### ✅ 满分答法 — Dec 2025 Q2b (4 marks)
*In the BrightNote system, compare HashSet and LinkedHashSet based on item ordering and how efficiently they perform when checking or managing items.*

- **Ordering (2)**: a HashSet stores the counter items in no particular order, so printing counterA may show "highlighter, pencil, notebook" instead of the order they were added. A LinkedHashSet keeps the insertion order, so the items are displayed as "notebook, pencil, highlighter", exactly as the staff added them.
- **Efficiency (2)**: both check and manage items (add, remove, contains) in constant time on average because both use hashing. However, HashSet is slightly faster and uses less memory, because LinkedHashSet must also maintain a linked list of the elements to remember their order. BrightNote should use HashSet if it only needs to check availability, and LinkedHashSet if the display order matters.

### ✅ 满分答法 — Oct 2025 Q2b (6 marks)
*Compare the differences between HashSet, LinkedHashSet and TreeSet in terms of ordering and performance.*

| | Ordering (1 each) | Performance (1 each) |
|---|---|---|
| **HashSet** | No guaranteed order; elements appear in an unpredictable order based on hash codes. | Fastest: add, remove and contains take constant time O(1) on average; lowest memory use. |
| **LinkedHashSet** | Maintains **insertion order** using a linked list through its elements. | Also O(1) on average, but slightly slower and uses more memory than HashSet because it maintains the linked list. |
| **TreeSet** | Maintains **sorted order** (ascending natural order or by a Comparator). | Slowest of the three: add, remove and contains take O(log n) time because the tree must stay sorted; provides first(), last(), headSet() etc. |

---

## 7. 考卷程序题模板 ⭐⭐⭐

所有 Set 程序题都是同一个骨架。背熟这个：

```java
import java.util.HashSet;          // ① import
import java.util.Set;

public class ClassName {           // ② class
    public static void main(String[] args) {      // ③ main
        Set<String> set1 = new HashSet<>();        // ④ create set(s)
        set1.add("...");                           // ⑤ add items
        // ⑥ set operation on a COPY (retainAll / removeAll / addAll)
        // ⑦ for-each loop to display + size() for the total
    }
}
```
*（以下分数分配是依题目要求估计的，不是官方 marking scheme。）*

### ✅ 满分答法 — Oct 2024 Q3b (15 marks)
*Create a HashSet to store a list of countries; add "New York", "Paris" and "Beijing"; display all items using a loop.*

```java
import java.util.HashSet;
import java.util.Set;

public class CountrySet {
    public static void main(String[] args) {
        // 1. Create a HashSet to store a list of countries
        Set<String> countries = new HashSet<>();

        // 2. Add the three items
        countries.add("New York");
        countries.add("Paris");
        countries.add("Beijing");

        // 3. Display all the items using a loop
        System.out.println("Countries stored in the HashSet:");
        for (String country : countries) {
            System.out.println(country);
        }
    }
}
```
实跑输出：
```
Countries stored in the HashSet:
Beijing
New York
Paris
```
*估计给分：import（2）· class + main（3）· 建立 HashSet（3）· 三个 add（3）· loop 显示（4）。题目说 "countries" 但给的是城市——照题目写，不用改。*

### ✅ 满分答法 — Dec 2025 Q2c (17 marks)
*Create two HashSet objects counterA and counterB; add the items; find the items unique to CounterA (in A but not in B); display them using a loop and print the total number of unique items.*

```java
import java.util.HashSet;
import java.util.Set;

public class BrightNote {
    public static void main(String[] args) {
        // Create two HashSet objects
        Set<String> counterA = new HashSet<>();
        Set<String> counterB = new HashSet<>();

        // Add the items to each counter
        counterA.add("notebook");
        counterA.add("pencil");
        counterA.add("highlighter");

        counterB.add("pencil");
        counterB.add("eraser");
        counterB.add("marker");

        // Items unique to CounterA = A - B (difference)
        Set<String> uniqueToA = new HashSet<>(counterA); // copy, so counterA is not changed
        uniqueToA.removeAll(counterB);

        // Display the results using a loop
        System.out.println("Items unique to CounterA:");
        for (String item : uniqueToA) {
            System.out.println("- " + item);
        }
        System.out.println("Total number of unique items in CounterA: " + uniqueToA.size());
    }
}
```
实跑输出：
```
Items unique to CounterA:
- highlighter
- notebook
Total number of unique items in CounterA: 2
```
*估计给分：import（1）· class/main（2）· 两个 HashSet（2）· 六个 add（3）· difference 逻辑（4）· loop 显示（3）· 总数（2）。*

**不记得 removeAll 也能拿分——用 loop + contains（已实跑，结果相同）：**
```java
int count = 0;
System.out.println("Items unique to CounterA:");
for (String item : counterA) {
    if (!counterB.contains(item)) {     // in A but not in B
        System.out.println(item);
        count++;
    }
}
System.out.println("Total: " + count);
```

### ✅ 满分答法 — Oct 2025 Q2c (16 marks)
*Create two HashSets; first: "pencil", "eraser", "ruler"; second: "marker pen", "pen", "eraser"; determine the common elements; display each common item using a loop and the total number of common elements.*

```java
import java.util.HashSet;
import java.util.Set;

public class CommonItems {
    public static void main(String[] args) {
        // Create two HashSet objects
        Set<String> set1 = new HashSet<>();
        Set<String> set2 = new HashSet<>();

        // Add three items to each HashSet
        set1.add("pencil");
        set1.add("eraser");
        set1.add("ruler");

        set2.add("marker pen");
        set2.add("pen");
        set2.add("eraser");

        // Common elements = intersection
        Set<String> common = new HashSet<>(set1); // copy so set1 stays unchanged
        common.retainAll(set2);

        // Display each common item using a loop, then the total
        System.out.println("Common items:");
        for (String item : common) {
            System.out.println(item);
        }
        System.out.println("Total number of common elements: " + common.size());
    }
}
```
实跑输出：
```
Common items:
eraser
Total number of common elements: 1
```
*注意 "pen" 和 "marker pen"、"pencil" 都是**不同的字串**——只有 "eraser" 完全相同。*

**常见扣分点**
- 忘了 `import java.util.HashSet;` / `java.util.Set`（或 `import java.util.*;`）
- 写 `HashSet set = new HashSet();`（raw type）虽然能跑，但建议写 `Set<String>`
- 直接对 `set1.retainAll(set2)`，之后又要用原本的 set1 → 资料已被改掉
- 用 `for (Object item : set)` 再呼叫 String 方法 → 编译错误（slide p21 的 bug）
- 忘了印**总数**（`size()` 或计数器）

---

## Closing the loop

回到 BrightNote：两个 HashSet、一行 `retainAll` 得到"两边都有"（pencil），一行 `removeAll` 得到"只有 A 有"（notebook、highlighter）；而且永远不会有重复。要照加入顺序显示就换 LinkedHashSet，要按字母排就换 TreeSet。下一章把视野拉大：Set 之外还有哪些 data structure，它们是 static 还是 dynamic、linear 还是 non-linear。

---

## ⚠️ Where the slides mislead

| Slide | 说法 | 更准确的理解 |
|---|---|---|
| p12, p15 | HashSet 输出 `[San Francisco, New York, Paris, Beijing, London]` | 旧 JDK 的顺序；javac 25 得到 `[San Francisco, Beijing, New York, London, Paris]`。HashSet **不保证顺序** |
| p15, p17 | "After removing common elements in set2 from set1, set1 is []" | retainAll 是**保留**共同元素；结果为空是因为前一行已 removeAll |
| p21 | `for (Object element: set) element.toLowerCase()` | **编译错误**；要写 `for (String element: set)` |
| p24/p26 | "TreeSet implements the SortedSet interface" | 正确，更精确地说实作 **NavigableSet**（extends SortedSet） |
| p27 | 注解写 "Create a hash set"（在 TestTreeSet / TestLinkedHashSet） | 注解是复制的；LinkedHashSet 那个其实建立的是 LinkedHashSet |

考试策略：若题目要你写 slide 程序的输出，HashSet 部分写"in no particular order"并列出元素即可。

---

## Term table

| English | 中文 | 一句话说明 |
|---|---|---|
| Set | 集合 | 不重复元素的集合 |
| HashSet | 杂凑集合 | 无顺序，最快 |
| LinkedHashSet | 链结杂凑集合 | 保留插入顺序 |
| TreeSet | 树集合 | 自动排序 |
| Capacity | 容量 | bucket 数量，预设 16 |
| Load factor | 负载因子 | 多满就扩容，预设 0.75 |
| hashCode | 杂凑码 | 决定放哪个 bucket |
| Collision | 碰撞 | 不同物件相同 hash code |
| Union / Intersection / Difference | 联集 / 交集 / 差集 | addAll / retainAll / removeAll |
| Insertion order | 插入顺序 | 加入的先后 |
| Natural order | 自然顺序 | compareTo 定义的顺序 |
| Comparable / Comparator | — | 元素自己比较 / 外部比较器 |
| SortedSet / NavigableSet | — | 有排序 / 有导航方法的 set 介面 |

---

## Cheat sheet

- **Set**：unique values，no particular order；Set extends Collection，无新 method。
- **3 concrete classes**：HashSet · LinkedHashSet · TreeSet。
- **3 operations**：union `addAll` · intersection `retainAll` · difference `removeAll`（会改原 set → 先复制）。
- **HashSet**：capacity 16，load factor 0.75 → size 12 时加倍到 32；higher LF = less space, slower search。
- **唯一性**：hashCode → bucket → equals 比对 → 已存在则 add() 回传 false。
- **Equal objects must have equal hash codes**。
- **Ordering**：Hash 无序 · Linked 插入顺序 · Tree 排序。
- **Performance**：Hash O(1) 最快 · Linked O(1) 稍慢多记忆体 · Tree O(log n)。
- **TreeSet methods**：first, last, headSet（<）, tailSet（≥）, lower（<）, floor（≤）, ceiling（≥）, higher（>）, pollFirst, pollLast。
- **程序骨架**：import → class/main → new HashSet → add → copy + operation → for-each 印 → size() 总数。

---

## Practice (answers included)

### A. MCQ
1. After `s.add("A"); s.add("B"); s.add("A");` on a HashSet, `s.size()` is: (a) 1 (b) 2 (c) 3 (d) error
2. Which set keeps elements in the order they were inserted? (a) HashSet (b) TreeSet (c) LinkedHashSet (d) SortedSet
3. With capacity 32 and load factor 0.75, the capacity doubles when size reaches: (a) 16 (b) 24 (c) 32 (d) 48
4. Which method performs intersection? (a) addAll (b) removeAll (c) retainAll (d) containsAll
5. For TreeSet {10, 20, 30, 40}, `floor(25)` returns: (a) 20 (b) 30 (c) 25 (d) null

**Answers:** 1 (b) · 2 (c) · 3 (b) — 32 × 0.75 = 24 · 4 (c) · 5 (a)

### B. Short answer
**B1. Explain the load factor of a HashSet. (3 marks)**
The load factor (0.0–1.0, default 0.75) measures how full the set may become before its capacity is increased. When the number of elements exceeds capacity × load factor, the capacity doubles (e.g. 16 × 0.75 = 12, so it grows to 32 at 12 elements). A higher load factor saves space but increases search time.

**B2. Why must two equal objects have the same hash code? (2 marks)**
HashSet uses the hash code to decide which bucket to look in. If two equal objects had different hash codes, they would be placed in different buckets, equals() would never compare them, and the set would store a duplicate.

**B3. When should TreeSet be preferred over HashSet? (2 marks)**
When the elements must be kept in sorted order or when methods such as first(), last(), headSet() or ceiling() are needed; otherwise HashSet is faster.

### C. Application
**C1. Write a program that stores the student IDs 1001, 1002, 1003, 1002 in a set, prints them in ascending order and prints how many unique IDs there are. (6 marks)**
```java
import java.util.Set;
import java.util.TreeSet;

public class StudentIDs {
    public static void main(String[] args) {
        Set<Integer> ids = new TreeSet<>();   // TreeSet → sorted
        ids.add(1001);
        ids.add(1002);
        ids.add(1003);
        ids.add(1002);                        // duplicate ignored
        for (int id : ids) {
            System.out.println(id);
        }
        System.out.println("Unique IDs: " + ids.size());
    }
}
```
Output: 1001, 1002, 1003 on separate lines, then `Unique IDs: 3`.

**C2. Given A = {1, 2, 3, 4} and B = {3, 4, 5}, write the result of A ∪ B, A ∩ B, A − B and B − A. (4 marks)**
A ∪ B = {1, 2, 3, 4, 5}; A ∩ B = {3, 4}; A − B = {1, 2}; B − A = {5}.

**C3. Trace: what is printed? (3 marks)**
```java
Set<String> s = new LinkedHashSet<>();
s.add("kiwi"); s.add("apple"); s.add("kiwi"); s.add("mango");
s.remove("apple");
System.out.println(s + " " + s.size());
```
`[kiwi, mango] 2` — insertion order kept, duplicate "kiwi" ignored, "apple" removed.

### D. Thinking
**D1. A shop uses an ArrayList to record which customers visited today, and the list keeps getting duplicate names. Suggest a better structure and justify. (3 marks)**
Use a HashSet (or LinkedHashSet to keep visit order). A set automatically rejects duplicates, and contains() checks whether a customer has visited in about constant time, whereas an ArrayList needs a linear search before each insertion.

**D2. Why does the slide's HashSet output differ from what you get today, and why is that not a bug? (2 marks)**
HashSet's iteration order depends on hash codes and the internal table implementation, which can change between Java versions; the Set contract never promises any order, so both outputs are correct as long as each element appears exactly once.

---

## Slide index

| Note section | Slides |
|---|---|
| 1 What is a set | p3–6 |
| 2 HashSet | p7–15 |
| 3 Union / intersection / difference | p16–18 |
| 4 LinkedHashSet | p19–22 |
| 5 TreeSet | p23–34 |
| 6 Comparison | p22, p33 + extra |
| 7 Exam programs | past papers |

## Links to other chapters
- **Ch2**：Set 是 Collection / Iterable，可用 for-each 或 iterator 走访
- **Ch4**：load factor = space/time trade-off
- **Ch5**：O(1) vs O(log n)
- **Ch7**：linked list、tree 的概念（LinkedHashSet、TreeSet 的底层）
