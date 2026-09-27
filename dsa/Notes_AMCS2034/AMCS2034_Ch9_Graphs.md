# AMCS2034 — Chapter 9: Graphs

> **全科分数最高的一章：每份考卷 Q4 都是 graph，20–25 分。** 必考两件事：**根据一张图写出 adjacency matrix + adjacency list（10–18 分）**，以及 **DFS / BFS**（说明、应用、或看编号判断是哪一种）。Slide 只示范了**无向图**和一个没有自环的简单有向图，但考卷的图是**有向图**，Dec 2025 还有**自环（self-loop）**和**双向边**——学生最常在这里写错。更糟的是，slide p46 的 12 城市 adjacency matrix **本身有两格错**（不对称，和 slide 自己的 edge array 矛盾）。这份笔记给一个"逐条 edge 填表"的固定做法，四份考卷的矩阵和列表都用程序生成核对过。

---

## 0. 一句话总览

**Graph G = (V, E) 用 vertices 表示事物、edges 表示关系；可以有方向、有权重、有环。电脑里用 adjacency matrix（n × n 的 0/1 表）或 adjacency list（每个 vertex 一串邻居）表示。走访方式有两种：DFS 一路走到底再回头（stack / recursion），BFS 一层一层往外扩（queue）。**

| Part | 内容 | Slide |
|---|---|---|
| 1 | Graph theory 与应用 | p6–10 |
| 2 | 定义 G = (V, E)；directed vs undirected | p11–17 |
| 3 | 基本术语（adjacent、self-loop、path、cycle、DAG、bipartite…） | p18–31 |
| 4 | 表示 vertices 与 edges（array、edge array、Edge objects） | p32–41 |
| 5 | Adjacency matrix | p44–47 |
| 6 | Adjacency list；matrix vs list | p48–57 |
| 7 | 考卷的矩阵 / 列表题 | — |
| 8 | Depth-First Search | p58–67 |
| 9 | Breadth-First Search；DFS vs BFS | p68–73 |

🎯 **这章回答的考题**
- **May 2025 Q4a**, **Dec 2025 Q4b**, **Oct 2025 Q4c** THREE applications of graphs (6 / 6 / 3)
- **May 2025 Q4b** Directed vs undirected (4)
- **Oct 2025 Q4b** THREE types of graphs with diagrams (6)
- **Oct 2024 Q4a(i)(ii)(iii)** adjacency list (9) + matrix (9) + Java array of vertices (3)
- **Dec 2025 Q4a(i)(ii)** matrix (7) + list (7)
- **Oct 2025 Q4a(i)(ii)** matrix (5) + list (5)
- **May 2025 Q4c** & **Oct 2025 Q4d** TWO traversal methods (6 / 6)
- **May 2025 Q4d** TWO applications of each traversal (4)
- **Oct 2024 Q4b** Classify figures (a), (b) as DFS or BFS (4)

**Prerequisites**：Ch7 stack、queue、tree；2D array；`ArrayList`。

---

## Scene：从 TAR UMT 出发

Slide p10 是一张 Google Maps 截图：从 Tunku Abdul Rahman University College 到另一个校区，Maps 给了三条路线（28 分钟、39 分钟、40 分钟）。

Maps 背后的资料是什么？每个路口是一个**点**，每段路是一条**线**；单行道有**方向**；每条线有**距离或时间**。要找最快路线，就是在这张"点和线"的图上搜寻。

这种"点和线"的结构，从 1736 年开始就有人研究了。

---

## 1. Graph Theory 与应用（p6–10）

**Graph theory** 由 **Leonhard Euler 在 1736 年**为了解决 **Seven Bridges of Königsberg** 问题而创立：Königsberg 城被 Pregel 河分开，河中有两个岛，城市与岛之间有七座桥。问题：能否散步一趟，**每座桥恰好走一次**，并回到起点？Euler 证明**不可能**（p6）。

p6 的图 (b) graph model：
- **Vertices**（4 块陆地）：A = 北岸、B = 南岸、C = Island 1、D = Island 2
- **Edges**（7 座桥）：A–C ×2、B–C ×2、A–D、B–D、C–D

*看这张图要注意：陆地变成 vertex、桥变成 edge——这就是"建模成 graph"。A–C 有两座桥，所以是 **parallel edges**（见 §3）。*

Graph 提供**最大的资料结构弹性**，能模拟真实系统和抽象问题（p7）。

**Graph applications（p8–9）**：
1. **Modeling connectivity** in computer and communications networks
2. **Representing a map** as locations with distances → compute **shortest routes**
3. **Modeling flow capacities** in transportation networks
4. **Finding a path** from a starting condition to a goal condition, e.g. **AI problem solving (robotics)**
5. **Modeling computer algorithms**, showing transitions from one program state to another
6. **Finding an acceptable order for finishing subtasks** in a complex activity, e.g. constructing large buildings
7. **Modeling relationships** such as family trees, business or military organisations, scientific taxonomies

### ✅ 满分答法 — May 2025 Q4a / Dec 2025 Q4b (6 marks)  ·  Oct 2025 Q4c (3 marks)
*Provide THREE applications of graphs in the real world.*

每个 2 分：应用 + vertex/edge 代表什么。（3 分版本：每个一句即可。）

1. **Navigation and route planning (maps)** – locations are vertices and roads are weighted edges with distances or travel times; the graph is searched to compute the shortest or fastest route, as Google Maps does between two campuses.
2. **Computer and communication networks** – computers and routers are vertices and cables or wireless links are edges; the graph models connectivity, e.g. to check whether every computer can reach the server or to route data packets.
3. **Social networks / relationship modelling** – users are vertices and friendships or "follows" are edges; the graph is used to suggest friends of friends and to model relationships such as family trees or organisation charts.

*（其他：project scheduling / 施工顺序（subtask ordering）；交通网的流量；AI / robotics 的路径规划；程序状态转换。）*

---

## 2. 定义与 Directed vs Undirected（p11–17）

**Graph（p11）**：*a mathematical structure that represents relationships among entities in the real world*，由
- **一个非空的 vertices 集合**（也叫 nodes 或 points），和
- **一个连接 vertices 的 edges 集合**

组成。记作 **G = (V, E)**，V 是 vertices 的集合，E 是 edges 的集合。

p12 例子：美国城市航班
```
V = {"Seattle", "San Francisco", "Los Angeles", "Denver", "Kansas City", "Chicago",
     "Boston", "New York", "Atlanta", "Miami", "Dallas", "Houston"};
E = {{"Seattle", "San Francisco"}, {"Seattle", "Chicago"}, {"Seattle", "Denver"},
     {"San Francisco", "Denver"}, ... };
```

| | **Directed edge** | **Undirected edge** |
|---|---|---|
| 定义 | **Ordered pair (u, v)**：u 是 **origin**，v 是 **destination** | **Unordered pair (u, v)**：(u, v) = (v, u) |
| 图示 | `(u) ───▶ (v)` | `(u) ────── (v)` |
| 例子（slide） | **One-way road traffic** | **Railway lines** |

| | **Directed graph (digraph)** | **Undirected graph** |
|---|---|---|
| 定义 | **所有** edges 都有方向 | **所有** edges 都没有方向 |
| 例子（slide） | Route network（p16） | Flight network（p17） |

p16 的 directed graph：A → B、B → C、C → A、C → D、A → D。
p13 的例子：(a) Peter → Mark、Jane → Mark、Mark → Wendy、Cindy → Wendy 的 directed graph；(b) A–E 五点的 complete graph；(c) 它的一个 subgraph。

> 💬 **答题句 (EN)** — *In a directed graph every edge has a direction and is an ordered pair (u, v) from origin u to destination v, so it can be traversed only from u to v (e.g. one-way roads); in an undirected graph every edge is an unordered pair and can be traversed in both directions (e.g. railway lines), so its adjacency matrix is symmetric.*

### ✅ 满分答法 — May 2025 Q4b (4 marks)
*Compare and contrast directed and undirected graphs, focusing on the directionality of edges.*

*Similarity* (1): both consist of a set of vertices V and a set of edges E connecting pairs of vertices, G = (V, E), and both can be represented by an adjacency matrix or adjacency list.
*Differences* (3):
- In a **directed graph** each edge is an **ordered pair (u, v)** with an origin u and a destination v, so it can only be followed from u to v — e.g. a one-way road; an edge u → v does not imply v → u (1).
- In an **undirected graph** each edge is an **unordered pair**, so it connects both vertices in both directions — e.g. a two-way railway line (1).
- Consequently, the adjacency matrix of an undirected graph is **symmetric** (matrix[i][j] = matrix[j][i]) and each edge appears in the lists of both vertices, whereas a directed graph's matrix is generally **not symmetric** (1).

---

## 3. 基本术语（p18–31）— Tutorial 9 Q4–Q9

p18、p23–25 的范例 undirected graph（下面多个定义都用它）：
```
  G ─── C ─────── D ─────── F
        │         │  ╲      │
        │         │    ╲    │
        B ─────── A      ╲─ E
```
（edges：G–C、C–D、C–B、B–A、A–D、D–F、D–E、F–E）

| Term | 定义（slide） | 例子 |
|---|---|---|
| **Adjacent** | 一条 edge 连接两个 vertices 时，这两个 vertices **互相 adjacent** | C 和 D adjacent |
| **Incident** | 那条 edge **incident on** 两个 vertices | edge C–D incident on C 和 D |
| **Tree** | **没有 cycle** 的 graph；**acyclic connected graph** | 把上图的 A–D 和 F–E 拿掉 |
| **Self-loop** | 连接 vertex **到自己**的 edge | A → A |
| **Parallel edges** | 两条 edge 连接**同一对** vertices | A 和 B 之间两条线 |
| **Degree** | 一个 vertex 上 incident 的 edges 数 | D 的 degree = 4（C, A, F, E） |
| **Subgraph** | graph 的 edges（与相关 vertices）的**子集**所形成的 graph | |
| **Path** | 一串**相邻的** vertices | G → C → D → E（p23 虚线） |
| **Simple path** | **没有重复 vertex** 的 path | |
| **Cycle** | 第一个和最后一个 vertex **相同**的 path | C → D → A → B → C（p24 虚线） |
| **Simple cycle** | 除了首尾，**没有重复** vertex 或 edge 的 cycle | |
| **Connected (vertices)** | 有一条 path 包含两者 | |
| **Connected graph** | **每个** vertex 到**每个其他** vertex 都有 path | 上图是 connected |
| **Connected components** | 不 connected 的 graph 由几个 connected components 组成 | p25：{G, C, B} 和 {D, F, E, A} 两块 |
| **DAG** | **没有 cycle 的 directed graph** | p26：A→B、A→D、C→A、C→B、C→D |
| **Forest** | **不相交（disjoint）的 trees** 的集合 | |
| **Spanning tree** | connected graph 的 subgraph，**包含所有 vertices**且**是一棵 tree** | |
| **Spanning forest** | 各 connected component 的 spanning trees 的**联集** | |
| **Bipartite graph** | vertices 可分成**两组**，**所有 edges 都连接不同组**的 vertices | |
| **Weighted graph** | 每条 edge 有一个整数 **weight**（距离或成本） | p29：A–C 7、A–D 5、C–B 8… |
| **Complete graph** | **所有可能的 edges 都存在** | p30：A、B、C、D 两两相连 |
| **Sparse / Dense** | edges 相对少（一般 **|E| < |V| log |V|**）/ 只缺少少数可能的 edges | |
| **Network** | **directed weighted** graph | |

**Edge 数量上限（p31）**：无向图 |E| 介于 **0 到 |V|(|V| − 1)/2**（每个 node 都能连到其他每个 node）。例：5 个 vertex 的 complete graph 有 5 × 4 / 2 = 10 条 edge。

*extra*：有向图的 vertex 有 **in-degree**（指进来的 edge 数）和 **out-degree**（指出去的 edge 数）。

### ✅ 满分答法 — Oct 2025 Q4b (6 marks)
*With the help of diagrams, identify and illustrate any THREE distinct types of graphs.*

每个 2 分：名称 + 一句定义 + 图。

1. **Directed graph** – every edge has a direction (ordered pair), e.g. one-way roads.
   ```
   (A) ───▶ (B)
    │  ▲      │
    ▼    ╲    ▼
   (D) ◀─── (C)
   ```
2. **Weighted graph** – each edge carries a weight representing distance or cost.
   ```
   (A) ──7── (C) ──8── (B)
    │         │
    5         7
    │         │
   (D) ──15─ (E)
   ```
3. **Complete graph** – every pair of distinct vertices is connected by an edge (4 vertices → 4 × 3 / 2 = 6 edges).
   ```
   (A) ──── (B)
    │ ╲    ╱ │
    │   ╳    │
    │ ╱    ╲ │
   (D) ──── (C)
   ```
*（其他可选：undirected graph、DAG、bipartite graph、tree（acyclic connected graph）、connected / disconnected graph。）*

---

## 4. 表示 Vertices 与 Edges（p32–41）

**Vertices 用 array 或 list 存（p33–34）**：用 0, 1, …, n − 1 当编号：
```java
String[] vertices = {"Seattle", "San Francisco", "Los Angeles", "Denver",
    "Kansas City", "Chicago", "Boston", "New York", "Atlanta", "Miami",
    "Dallas", "Houston"};
// vertices[0] = "Seattle", vertices[1] = "San Francisco", ...
```

**Edge array（p35–37）**：用 2D array，每一列 `{u, v}` 是一条 edge：
```java
String[] names = {"Peter", "Jane", "Mark", "Cindy", "Wendy"};
int[][] edges = {{0, 2}, {1, 2}, {2, 4}, {3, 4}};   // Peter→Mark, Jane→Mark, Mark→Wendy, Cindy→Wendy
```
（无向图的 edge array 两个方向都要写，例如 `{0, 1}` 和 `{1, 0}`。）

**Edge objects（p38–41）**：定义 `Edge` class，存进 `ArrayList`：
```java
public class Edge {
    int u;
    int v;
    public Edge(int u, int v) { this.u = u; this.v = v; }
    public boolean equals(Object o) {
        return u == ((Edge)o).u && v == ((Edge)o).v;
    }
}

java.util.ArrayList<Edge> list = new java.util.ArrayList<>();
list.add(new Edge(0, 1));
list.add(new Edge(0, 3));
list.add(new Edge(0, 5));
```
**事先不知道有哪些 edges 时**用 ArrayList 很方便。Edge array 和 Edge objects 适合**输入**，但**不适合内部处理**；处理 graph 时用 **adjacency matrix / adjacency list**（p41）。

---

## 5. Adjacency Matrix（p44–47）⭐⭐

- n 个 vertices → 一个 **n × n** 的 2D array。
- **`adjacencyMatrix[i][j] = 1` 表示有一条 edge 从 vertex i 到 vertex j**；否则为 0。
- **无向图的矩阵是对称的**：`adjacencyMatrix[i][j] == adjacencyMatrix[j][i]`。

**Directed 例子（p47）**——Peter/Jane/Mark/Cindy/Wendy：
```java
int[][] a = {{0, 0, 1, 0, 0},   // Peter → Mark
             {0, 0, 1, 0, 0},   // Jane  → Mark
             {0, 0, 0, 0, 1},   // Mark  → Wendy
             {0, 0, 0, 0, 1},   // Cindy → Wendy
             {0, 0, 0, 0, 0}};  // Wendy (no outgoing edge)
```
（用程序从 edge array 生成，结果与 slide 完全相同。）

**读法**：**row = from（起点）**，**column = to（终点）**。一列里的 1 = 这个 vertex 指出去的 edges（out-degree）；一行（column）里的 1 = 指进来的 edges（in-degree）。

⚠️ **Slide p46 的 12 城市矩阵有两处错**（程序检查：矩阵不对称，且和 p36 的 edge array 不符）：
- **Los Angeles 那列**写成 `{0,1,0,1,1,1,0,0,0,0,0,0}`（邻居 1, 3, 4, **5**），但 edge array 是 {2,1},{2,3},{2,4},**{2,10}**——第 6 格（Chicago）应为 0，第 11 格（Dallas）应为 1。
- **Atlanta 那列**第 4 格（Denver）写了 1，但 Denver 那列 Atlanta 是 0，edge array 也没有 {8,3}——应为 0。
- p50 的 adjacency list 是对的。**教训**：画完无向图的矩阵，**检查对称**就能抓到这种错。

---

## 6. Adjacency List（p48–57）⭐⭐

- **Adjacency vertex list**：vertex i 的 list 存所有**和 i 相邻的 vertices**（i → j 的 j）。
- **Adjacency edge list**：vertex i 的 list 存所有**和 i 相邻的 edges**（Edge 物件）。
- 做法：定义一个**有 n 个元素的 array of lists**，每个元素是一个 list（p48）。

```java
java.util.List<Integer>[] neighbors = new java.util.List[12];   // vertex lists (p49)
java.util.List<Edge>[]    neighbors = new java.util.List[12];   // edge lists   (p51)
```

**p50 — 12 城市的 adjacency vertex lists**
```
Seattle        neighbors[0]  → 1 → 3 → 5
San Francisco  neighbors[1]  → 0 → 2 → 3
Los Angeles    neighbors[2]  → 1 → 3 → 4 → 10
Denver         neighbors[3]  → 0 → 1 → 2 → 4 → 5
Kansas City    neighbors[4]  → 2 → 3 → 5 → 7 → 8 → 10
Chicago        neighbors[5]  → 0 → 3 → 4 → 6 → 7
Boston         neighbors[6]  → 5 → 7
New York       neighbors[7]  → 4 → 5 → 6 → 8
Atlanta        neighbors[8]  → 4 → 7 → 9 → 10 → 11
Miami          neighbors[9]  → 8 → 11
Dallas         neighbors[10] → 2 → 4 → 8 → 11
Houston        neighbors[11] → 8 → 9 → 10
```
p52 的 adjacency edge lists 同样结构，只是每格存 `Edge(0, 1)`、`Edge(0, 3)`…

**用 ArrayList 建 adjacency edge list（p57）**
```java
List<ArrayList<Edge>> neighbors = new ArrayList<>();
neighbors.add(new ArrayList<Edge>());
neighbors.get(0).add(new Edge(0, 1));
neighbors.get(0).add(new Edge(0, 3));
neighbors.get(0).add(new Edge(0, 5));
neighbors.add(new ArrayList<Edge>());
neighbors.get(1).add(new Edge(1, 0));
...
neighbors.get(11).add(new Edge(11, 10));
```

**Matrix 还是 List？（p53–56）**

| | Adjacency matrix | Adjacency list |
|---|---|---|
| 适合 | **Dense** graph（很多 edges） | **Sparse** graph（很少 edges）——matrix 会浪费很多空间 |
| 空间 | n × n，不管 edges 多少 | 只存存在的 edges |
| 检查 i、j 是否相连 | **O(1)**（直接看 `m[i][j]`） | 要扫 i 的 list |
| 列出所有 edges | 要扫整个 n × n | 线性时间（slide 写 O(n)，n = edge 数） |

- **Adjacency vertex list** 表示**无权重** graph 较简单；**adjacency edge list** 较有弹性，容易在 edge 上加限制（例如 weight）（p55）。
- 可用 array、ArrayList 或 LinkedList 存 adjacency lists；**list 比 array 容易扩充**（加新 vertex）；只需要搜寻相邻 vertex 时，**ArrayList 比 LinkedList 好**（p56）。

> 💬 **答题句 (EN)** — *An adjacency matrix is an n × n array where matrix[i][j] = 1 if there is an edge from vertex i to vertex j and 0 otherwise; it is preferred for dense graphs and checks an edge in O(1). An adjacency list stores, for each vertex, a list of its adjacent vertices (or edges); it is preferred for sparse graphs because it stores only existing edges.*

---

## 7. 考卷的矩阵 / 列表题 ⭐⭐⭐

### 7.1 固定做法（不会漏）

1. **把 vertices 排好顺序**（照题目的编号；没编号就按字母）。矩阵的 row 和 column 都用这个顺序。
2. **逐个 vertex、逐条"从它出发"的箭头**：在 row = 起点、column = 终点那格填 1。
   - **有向图只填箭头方向**；**两个箭头（A ⇄ B）就两格都填**。
   - **自环** A → A：填**对角线** m[A][A] = 1，list 里 A 也写自己。
   - **无向图**：每条线两格都填（矩阵对称）。
3. **其他格全部填 0**（不要留空白）。
4. **自我检查**：矩阵里 1 的总数 = edge 数（有向图）；list 里的元素总数 = 1 的总数；每一 row 的 1 = list 该列的元素。

### ✅ 满分答法 — Oct 2024 Q4a (9 + 9 + 3 marks)

Figure 1 的配置（从上到下）：Z(8) 在最上面；Y(7) 在 Z 下方；W(5) 在 Y 下方，S(3) 在 W 右边；R(2)、P(0)、T(4) 同一排；X(6)、Q(1) 在最下面。数字是 vertex 编号。

逐条读箭头（箭头尖端 = 终点）：

| 位置 | Edge |
|---|---|
| P 往左到 R | P → R |
| P 往上到 W | P → W |
| Q 往左到 X | Q → X |
| R 往下到 X | R → X |
| S 往下到 T | S → T |
| T 斜向左上到 W | T → W |
| W 往右到 S | W → S |
| W 往上到 Y | W → Y |
| Y 斜向左下到 R | Y → R |
| Y 往上到 Z | Y → Z |

共 **10 条** edges。

**(i) Adjacency list (9)**
```
P(0) → R → W
Q(1) → X
R(2) → X
S(3) → T
T(4) → W
W(5) → S → Y
X(6) → (none)
Y(7) → R → Z
Z(8) → (none)
```

**(ii) Adjacency matrix (9)**（row = from, column = to）
```
       P  Q  R  S  T  W  X  Y  Z
   P [ 0  0  1  0  0  1  0  0  0 ]
   Q [ 0  0  0  0  0  0  1  0  0 ]
   R [ 0  0  0  0  0  0  1  0  0 ]
   S [ 0  0  0  0  1  0  0  0  0 ]
   T [ 0  0  0  0  0  1  0  0  0 ]
   W [ 0  0  0  1  0  0  0  1  0 ]
   X [ 0  0  0  0  0  0  0  0  0 ]
   Y [ 0  0  1  0  0  0  0  0  1 ]
   Z [ 0  0  0  0  0  0  0  0  0 ]
```
检查：1 的总数 = 10 = edge 数 ✓（矩阵不对称，因为是有向图）。

**(iii) Java array to store the vertices (3)**
```java
String[] vertices = {"P", "Q", "R", "S", "T", "W", "X", "Y", "Z"};
// vertices[0] = "P", vertices[1] = "Q", ..., vertices[8] = "Z"  (matches the labels 0–8 in Figure 1)
```
*（也可写成 Java 完整程序：在 main 里宣告并用 for loop 印出。）*

### ✅ 满分答法 — Dec 2025 Q4a (7 + 7 marks)

Figure 4-1 的配置：b 在上方中间；a 在左上（有一个**自环**）；c 在右边；e 在最左；d 在正中间；f 在左下；g 在右下（底下有一个**自环**）。b 和 c 之间有**两条相反方向的箭头**；d 和 f 之间也有**两条相反方向的箭头**。

Edges（15 条）——**逐一从图上读出**：
- a → a（**self-loop**）、a → d
- b → a、b → c
- c → b、c → d、c → g（**b ⇄ c 是两条**）
- d → b、d → f、d → g
- e → d
- f → a、f → d、f → g（**d ⇄ f 是两条**）
- g → g（**self-loop**）

**(i) Adjacency matrix (7)**
```
       a  b  c  d  e  f  g
   a [ 1  0  0  1  0  0  0 ]
   b [ 1  0  1  0  0  0  0 ]
   c [ 0  1  0  1  0  0  1 ]
   d [ 0  1  0  0  0  1  1 ]
   e [ 0  0  0  1  0  0  0 ]
   f [ 1  0  0  1  0  0  1 ]
   g [ 0  0  0  0  0  0  1 ]
```
检查：1 的总数 = 15 ✓；对角线 a、g 为 1（self-loops）。

**(ii) Adjacency list (7)**
```
a → a → d
b → a → c
c → b → d → g
d → b → f → g
e → d
f → a → d → g
g → g
```
⚠️ 常见错误：漏掉 a→a 和 g→g；把 b–c、d–f 当成一条边只写一个方向；把 e→d 写成 d→e。

### ✅ 满分答法 — Oct 2025 Q4a (5 + 5 marks)

Figure 4-1 的配置：A 左上、C 右上、B 左中、D 右中、E 下方中间；右边一条弯曲的长线从 E 绕上去，**箭头指向 C**。

| 位置 | Edge |
|---|---|
| A 往右到 C | A → C |
| A 往下到 B | A → B |
| B 斜向右上到 C | B → C |
| B 往右到 D | B → D |
| B 斜向右下到 E | B → E |
| C 往下到 D | C → D |
| D 斜向左下到 E | D → E |
| E 沿右边弯线回到 C | E → C |

共 **8 条** edges。

**(i) Adjacency matrix (5)**
```
       A  B  C  D  E
   A [ 0  1  1  0  0 ]
   B [ 0  0  1  1  1 ]
   C [ 0  0  0  1  0 ]
   D [ 0  0  0  0  1 ]
   E [ 0  0  1  0  0 ]
```
**(ii) Adjacency list (5)**
```
A → B → C
B → C → D → E
C → D
D → E
E → C
```
检查：8 个 1 = 8 条 edges ✓。注意 **E → C**（右边那条弯的线箭头指向 C）。

（以上三题的矩阵与列表都由程序从 edge 清单生成，和手写结果一致。）

---

## 8. Depth-First Search（p58–67）⭐

**Graph traversal** = 把 graph 的每个 vertex **恰好造访一次**的过程。两种常用方法：**depth-first** 和 **breadth-first**（p59）。两者都会产生一棵 **spanning tree**，可用 `Tree` class 建模（p60：root、parent[]、searchOrder）。

**DFS 的概念（p61–62）**：
- Tree 从 root 开始；graph 可以**从任何 vertex 开始**。
- 从一个 vertex 出发，**尽可能往深处走**，走不下去才 **backtrack**（回头）。
- 和 tree 不同，**graph 可能有 cycle** → 可能无限递回；所以要**记录已造访的 vertices**（`isVisited[]`）。
- 造访 v 之后，去造访 v 的一个**未造访**邻居；如果 v 没有未造访的邻居，就退回到**到达 v 之前**的那个 vertex。

**DFS algorithm（p63）**
```
Input: G = (V, E) and a starting vertex v
Output: a DFS tree rooted at v
1 Tree dfs(vertex v) {
2   visit v;
3   for each neighbor w of v
4     if (w has not been visited) {
5       set v as the parent for w in the tree;
6       dfs(w);                      ← 递回 = 隐含地用了 stack
7   }
8 }
```

**p64–65 的例子**（undirected，edges：0–1、0–2、0–3、1–2、1–4、2–3）
```
   0 ─────────── 1
   │ ╲         ╱ │
   │   ╲     ╱   │
   │     2       │
   │   ╱         │
   3             4
```
| 步骤 | 目前 | 动作 | 已造访 |
|---|---|---|---|
| (a)→(b) | 0 | 造访 0，走向邻居 1 | 0, 1 |
| (c) | 1 | 1 的邻居 0（已访）、2、4 → 选 2 | 0, 1, 2 |
| (d) | 2 | 2 的邻居 0、1（已访）、3 → 走 3 | 0, 1, 2, 3 |
| | 3 | 3 的邻居 0、2 都已访 → **backtrack** 到 2，再到 1 | |
| (e) | 1 | 1 还有 4 没访 → 走 4 | 0, 1, 2, 3, 4 |
| | 4 → 1 → 0 | 全部邻居已访 → backtrack 结束 | |

**DFS order：0, 1, 2, 3, 4**；DFS tree edges：0→1、1→2、2→3、1→4（程序验证）。
**Time complexity：O(|E| + |V|)**——每个 edge 和每个 vertex 只处理一次。

**Applications of DFS（p66–67）**：
- **Detecting whether a graph is connected**：从任一 vertex 搜寻，若搜到的 vertex 数 = graph 的 vertex 数 → connected
- Detecting whether there is a **path** between two vertices；**finding** a path
- Finding all **connected components**（最大的 connected subgraph）
- Detecting whether there is a **cycle**；finding a cycle
- Finding a **Hamiltonian path / cycle**（造访每个 vertex 恰好一次的 path；cycle 还要回到起点）

---

## 9. Breadth-First Search（p68–73）⭐

**BFS 的概念（p68）**：**一层一层（level by level）**造访。第一层是起点；下一层是和上一层**相邻**的 vertices。Tree 的 BFS：先 root，再所有 children，再所有 grandchildren……

**BFS algorithm（p69）**
```
Input: G = (V, E) and a starting vertex v
Output: a BFS tree rooted at v
 1 Tree bfs(vertex v) {
 2    create an empty queue for storing vertices to be visited;
 3    add v into the queue;
 4    mark v visited;
 5
 6    while (the queue is not empty) {
 7        dequeue a vertex, say u, from the queue;
 8        add u into a list of traversed vertices;
 9        for each neighbor w of u
10            if w has not been visited {
11                add w into the queue;
12                set u as the parent for w in the tree;
13                mark w visited;
14            }
15    }
16 }
```

**p70 的例子**（注意这张图**多了一条 3–4**：edges 0–1、0–2、0–3、1–2、1–4、2–3、3–4）

| Dequeue | 它的邻居 | 新加入 queue | Queue 之后 |
|---|---|---|---|
| — | — | 0 | [0] |
| 0 | 1, 2, 3 | 1, 2, 3 | [1, 2, 3] |
| 1 | 0, 2, 4 | 4 | [2, 3, 4] |
| 2 | 0, 1, 3 | — | [3, 4] |
| 3 | 0, 2, 4 | — | [4] |
| 4 | 1, 3 | — | [] |

**BFS order：0, 1, 2, 3, 4**；BFS tree edges：0→1、0→2、0→3、1→4（程序验证）。**Time complexity：O(|E| + |V|)**。

**Applications of BFS（p71–72）**：
- Detecting whether a graph is **connected**
- Detecting whether there is a **path** between two vertices
- **Finding a shortest path between two vertices**——BFS tree 里 root 到任一 node 的 path 就是**最短 path**（以 edge 数计）
- Finding all **connected components**
- Detecting / finding a **cycle**
- **Testing whether a graph is bipartite**

### DFS vs BFS（p73）

| | **DFS** | **BFS** |
|---|---|---|
| Order of visits | 沿着一条分支**尽可能深入**，走不下去才 **backtrack** | 先造访**所有邻居**，再往下一层——**level by level** 由起点往外 |
| 用的结构 | **Stack**（或 recursion） | **Queue** |
| Spanning tree 形状 | 又深又窄 | 又宽又浅 |
| 特别适合 | cycle detection、Hamiltonian path、找所有 components | **最短 path（未加权）**、bipartite 测试 |
| Time | O(|V| + |E|) | O(|V| + |E|) |

> 💬 **答题句 (EN)** — *Depth-first search starts from a vertex and visits as far as possible along each branch before backtracking, using recursion or a stack and an isVisited array; breadth-first search visits the starting vertex, then all its neighbours, then their neighbours, level by level, using a queue. Both run in O(|V| + |E|) time.*

### ✅ 满分答法 — May 2025 Q4c / Oct 2025 Q4d (6 marks)
*Identify / list and explain the TWO popular methods used to traverse a graph.*

1. **Depth-first search (DFS)** (3) – starts from a chosen vertex, visits it, then recursively visits an unvisited neighbour, going as deep as possible along one branch; when a vertex has no unvisited neighbours, the search backtracks to the vertex from which it came. An isVisited array prevents revisiting vertices, since a graph may contain cycles. It is implemented with recursion or a stack and takes O(|V| + |E|) time. *E.g. starting from 0: 0, 1, 2, 3, then backtrack to visit 4.*
2. **Breadth-first search (BFS)** (3) – starts from a chosen vertex, visits all of its neighbours first, then the neighbours of those neighbours, level by level. It uses a queue: the start vertex is enqueued and marked visited; repeatedly a vertex is dequeued and each unvisited neighbour is marked and enqueued. It also takes O(|V| + |E|) time. *E.g. starting from 0: 0, then 1, 2, 3, then 4.*

### ✅ 满分答法 — May 2025 Q4d (4 marks)
*State TWO common applications for each graph traversal method.*

- **DFS** (2): (1) detecting whether a graph is connected, or finding all connected components; (2) detecting or finding a cycle in a graph (also: finding a path, Hamiltonian path).
- **BFS** (2): (1) finding the shortest path (fewest edges) between two vertices, e.g. the fewest connections between two users in a social network; (2) testing whether a graph is bipartite (also: detecting connectivity, finding connected components).

### ✅ 满分答法 — Oct 2024 Q4b (4 marks)
*Classify and explain graphs (a) and (b) in Figure 2 as Depth-first search or Breadth-first search.*

两棵树结构相同：v 的 children 是 u、w、x；u 的 children 是 q、t；q 的 children 是 r、s。括号数字是造访顺序。

```
 (a)                          (b)
        (1)v                         (1)v
      /   |   \                    /   |   \
   (2)u (7)w (8)x              (2)u  (3)w  (4)x
   /   \                       /   \
 (3)q  (6)t                  (5)q  (6)t
 /   \                       /   \
(4)r (5)s                  (7)r  (8)s
```

- **(a) is Depth-first search** (2): the visiting order v, u, q, r, s, t, w, x goes as deep as possible along the first branch (v → u → q → r) before backtracking to visit s, then t, and only then the other children of v (w, x).
- **(b) is Breadth-first search** (2): the visiting order v, u, w, x, q, t, r, s visits the graph level by level — first the start vertex v, then all its neighbours u, w, x, then the next level q, t, and finally r, s.

（两个顺序都用程序在这棵树上跑 DFS / BFS 验证过。）

---

## Closing the loop

回到 TAR UMT 的 Google Maps：路口和道路是一个 **weighted directed graph**；Maps 存它用 **adjacency list**（道路网很 sparse）；要找"转最少次的路线"用 **BFS**，要确认两个地点之间"到底连不连得通"用 DFS 或 BFS 都可以。整门课在这里收尾：从 Ch1 的"资料怎么摆"，到 Ch3–5 的"怎么衡量快慢"，到 Ch6–9 的具体结构——graph 是其中最有弹性的一种。

---

## ⚠️ Where the slides mislead

| Slide | 说法 | 更准确的理解 |
|---|---|---|
| **p46** | 12 城市 adjacency matrix | **两处错**：Los Angeles 那列应是邻居 1, 3, 4, **10**（不是 5）；Atlanta 那列不应有 Denver。矩阵不对称，和 p36 的 edge array、p50 的 list 矛盾 |
| p16–17 vs Ch7 p79 | Directed 例子 "route network"；undirected 例子 "flight network"；Ch7 却说 airline flights 是 directed | 看怎么建模：单向航线用 directed；若航线都双向可当 undirected。考试举 slide 的 one-way road（directed）/ railway line（undirected）最安全 |
| p19 | "A graph with no cycles is called a tree" | 精确地说是 **connected** 且无 cycle（下一行有写）；无 cycle 但不连通的是 **forest** |
| p54 | "O(n) to print all edges using adjacency lists, where n is the number of edges" | 严格是 O(|V| + |E|)，因为每个 vertex 的 list 都要看一次 |
| p64 vs p70 | DFS 和 BFS 的例图看起来一样 | **不一样**：BFS 的图多了 3–4 这条 edge |
| p62 | "We assume that the graph is connected" | 不连通的 graph 要从每个未造访的 vertex 再做一次 DFS 才能走完 |

---

## Term table

| English | 中文 | 一句话说明 |
|---|---|---|
| Graph G = (V, E) | 图 | vertices + edges |
| Vertex / Node | 顶点 / 节点 | 事物 |
| Edge / Arc | 边 / 弧 | 关系；arc = 有向边 |
| Directed / Undirected | 有向 / 无向 | ordered / unordered pair |
| Adjacent / Incident | 相邻 / 关联 | 两点被边连 / 边接在点上 |
| Self-loop | 自环 | 点连到自己 |
| Parallel edges | 平行边 | 同一对点两条边 |
| Degree (in / out) | 度（入 / 出） | 接在点上的边数 |
| Path / Simple path | 路径 / 简单路径 | 相邻点序列 / 不重复点 |
| Cycle / Simple cycle | 环 / 简单环 | 首尾相同 |
| Connected / Component | 连通 / 连通分量 | 任两点有 path |
| Tree / Forest | 树 / 森林 | 无环连通 / 多棵不相交的树 |
| Spanning tree / forest | 生成树 / 生成森林 | 含所有顶点的树 |
| DAG | 有向无环图 | directed acyclic graph |
| Bipartite graph | 二部图 | 边只连不同组 |
| Weighted graph / Network | 加权图 / 网络 | 边有权重 / 有向加权图 |
| Complete graph | 完全图 | 所有边都存在 |
| Sparse / Dense | 稀疏 / 稠密 | 边少 / 边多 |
| Adjacency matrix | 邻接矩阵 | n × n 的 0/1 表 |
| Adjacency list | 邻接串列 | 每个点一串邻居 |
| Edge array / Edge objects | 边阵列 / 边物件 | {u, v} 列表 / Edge class |
| DFS / BFS | 深度 / 广度优先搜寻 | stack / queue |
| Backtrack | 回溯 | 走不下去就退回 |
| Hamiltonian path | 汉米尔顿路径 | 每个点恰好一次 |

---

## Cheat sheet

- **Euler 1736，Königsberg 七桥**——证明不可能。
- **Applications**：networks · maps/shortest route · flow capacity · AI path finding · program states · task ordering · relationships。
- **G = (V, E)**；directed edge = ordered (u, v)，origin → destination（one-way road）；undirected = unordered（railway）。
- **术语**：adjacent/incident · tree = acyclic connected · self-loop · parallel · degree · path/simple · cycle/simple · connected/components · DAG · forest · spanning tree/forest · bipartite · weighted · complete · sparse (|E| < |V| log |V|) / dense · network = directed weighted · |E| ≤ |V|(|V|−1)/2。
- **Vertices**：`String[] vertices = {...}`。**Edges**：edge array `int[][]`、Edge objects in ArrayList。
- **Matrix**：`m[i][j] = 1` if i → j；undirected → symmetric；dense；O(1) edge check。
- **List**：array of lists；vertex list / edge list；sparse。
- **填表**：row = from，column = to；两个箭头填两格；self-loop 填对角线；其他填 0；数 1 的个数 = edges。
- **Oct 2024**：P→R,W · Q→X · R→X · S→T · T→W · W→S,Y · Y→R,Z（10）。
- **Dec 2025**：a→a,d · b→a,c · c→b,d,g · d→b,f,g · e→d · f→a,d,g · g→g（15）。
- **Oct 2025**：A→B,C · B→C,D,E · C→D · D→E · E→C（8）。
- **DFS**：deep then backtrack，recursion/stack，isVisited；connected、path、components、cycle、Hamiltonian。
- **BFS**：level by level，queue；**shortest path**、bipartite、connected、components、cycle。
- **Both O(|V| + |E|)**。Oct 2024 Fig 2：(a) DFS（v u q r s t w x）· (b) BFS（v u w x q t r s）。

---

## Practice (answers included)

### A. MCQ
1. The adjacency matrix of an undirected graph is always: (a) diagonal (b) symmetric (c) all ones (d) upper triangular
2. Which traversal finds the path with the fewest edges from the start vertex? (a) DFS (b) BFS (c) inorder (d) postorder
3. A complete undirected graph with 6 vertices has how many edges? (a) 12 (b) 15 (c) 30 (d) 36
4. In a directed graph, the edge (u, v) means: (a) v → u (b) u → v (c) both ways (d) u = v
5. Which structure does BFS use? (a) stack (b) queue (c) set (d) tree only

**Answers:** 1 (b) · 2 (b) · 3 (b) — 6 × 5 / 2 = 15 · 4 (b) · 5 (b)

### B. Short answer
**B1. Differentiate a simple path and a simple cycle. (2 marks)**
A simple path is a sequence of adjacent vertices with no repeated vertices; a simple cycle is a path whose first and last vertices are the same, with no other repeated vertices or edges.

**B2. When is an adjacency list preferred over an adjacency matrix? (2 marks)**
When the graph is sparse (few edges): the matrix would waste n × n space mostly filled with zeros, while the list stores only the edges that exist.

**B3. Why does DFS on a graph need an isVisited array while DFS on a tree does not? (2 marks)**
A graph may contain cycles, so without marking visited vertices the search could return to a vertex already visited and recurse forever; a tree has no cycles.

### C. Application
**C1. For the Oct 2025 graph (A→B, A→C, B→C, B→D, B→E, C→D, D→E, E→C), give the DFS and BFS orders starting from A (visit neighbours alphabetically). (4 marks)**
DFS: A → B → C → D → E (A, B, C, D, E). BFS: A; then A's neighbours B, C; then from B: D, E → **A, B, C, D, E**. (Same order here, but the trees differ: DFS tree A–B–C–D–E is a chain; BFS tree has A→B, A→C, B→D, B→E.)

**C2. For the Oct 2024 graph, give the DFS and BFS orders starting from P (neighbours in the order of the adjacency list). (4 marks)**
DFS: **P, R, X, W, S, T, Y, Z** (P → R → X, backtrack to P → W → S → T, backtrack to W → Y → Z; R already visited). BFS: **P, R, W, X, S, Y, T, Z**. Q is never reached from P because no edge points into Q.

**C3. Draw the adjacency matrix of the undirected graph with edges A–B, A–C, B–C, C–D. (4 marks)**
```
      A  B  C  D
  A [ 0  1  1  0 ]
  B [ 1  0  1  0 ]
  C [ 1  1  0  1 ]
  D [ 0  0  1  0 ]
```
Symmetric; 8 ones = 2 × 4 edges.

### D. Thinking
**D1. A delivery company wants the route with the fewest stops between two warehouses. Which traversal should it use, and why? (3 marks)**
BFS, because it visits vertices level by level from the start, so the first time it reaches the destination it has used the fewest edges (stops); the path from the root to that vertex in the BFS tree is a shortest path. (If the roads have different distances, a weighted shortest-path algorithm such as Dijkstra is needed.)

**D2. Can a graph's DFS from one vertex fail to visit every vertex? Give an example. (2 marks)**
Yes, if the graph is not connected (or, for a directed graph, some vertices cannot be reached). In the Oct 2024 graph, DFS from P never reaches Q because no edge leads into Q.

---

## Slide index

| Note section | Slides |
|---|---|
| 1 Graph theory & applications | p6–10（图 p6, p10） |
| 2 Definition, directed / undirected | p11–17（图 p12–17） |
| 3 Terminology | p18–31（图 p18–30） |
| 4 Vertices & edges | p32–41（图 p33–40） |
| 5 Adjacency matrix | p44–47（图 p46–47） |
| 6 Adjacency list | p48–57（图 p49–52, p57） |
| 7 Exam matrix / list | past papers |
| 8 DFS | p58–67（图 p60, p64–65） |
| 9 BFS, DFS vs BFS | p68–73（图 p70） |

## Links to other chapters
- **Ch3–5**：O(|V| + |E|)；n² shortest path 例子
- **Ch7**：stack（DFS）、queue（BFS）、tree 与 traversal；tree vs graph（social network 题）
- **Ch2**：dynamic programming 的 shortest path 例子（Dijkstra、Floyd-Warshall）
