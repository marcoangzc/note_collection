# AMCS2123 — Chapter 1: Advanced Array Manipulation

> Slide 把 1D array、2D array、vector 各讲了一遍，但有三个地方会让你在考试丢分：① 2D array 的**行 (row) 和列 (column) 要代表什么**，slide 没教你怎么从题目的表格判断（两份考卷都考，9–14 分）；② "normal array vs vector" 的区别 slide 只写了一句"size 不固定"（两份卷都考 6 分）；③ `erase(begin()+1)` 为什么是"第二个元素"，begin() 与 front() 的差别只有一句话。这份笔记用一个"车行季度销售表"贯穿全章，所有 code 都用 g++ 实际跑过。

---

## 0. 一句话总览

**Array = 同一类型、固定数量的一排格子，用 index（从 0 开始）存取；2D array = 表格，`arr[row][col]`，用 nested for loop 处理；vector = 会自己变大变小的 array，用 push_back / pop_back / insert / erase 等函数操作。**

| Part | 内容 | Slide |
|---|---|---|
| 1 | 1D array：declaration、initialization、assignment、index | p2–9 |
| 2 | 1D array 的常见操作（sum、average、largest、double、square） | p10–17 |
| 3 | **2D array**：declaration、initialization、单行 / 单列 / 全表处理 | p18–26 |
| 4 | **Vector**：declaration、基本函数、size vs capacity、begin/front、iterator | p27–34 |

🎯 **这章回答的考题**
- **Oct 2025 Q1a** 两个 normal array vs vector 的区别 + 各自宣告 `scores` (6)
- **Oct 2025 Q1b** vector：(i) 加 23 (2) (ii) 删第二个元素 (2)
- **Oct 2025 Q1c** 2D `sales[7][10]`：(i) 宣告 (2) (ii) 计算并显示星期五总销售 (7)
- **Jan 2026 Q1a** 什么时候用 vector + 宣告 `numbers` + 加入 50 (2+4)
- **Jan 2026 Q1b** 6 款车 × 4 季度 2D array：(i) 宣告 (2) (ii) 改 Ativa 第 3 季 (2) (iii) 第 4 季总销售 (7) (iv) nested for loop 的情境与理由 (3)

**Prerequisites**：`for` loop、`cin`/`cout`、基本 data types（int, double, string）。

---

## Scene：车行老板的 Excel 表

Perodua / Proton 车行老板给你一张表：6 款车（MyVi, Saga, Alza, Pesona, Ativa, X50），4 个季度，每格是销售额。

如果用普通变量，你要 24 个名字：`myviQ1, myviQ2, …`。要算"第 4 季总销售"就要写 6 个变量相加——太累，而且车款一多就爆炸。

**第一个问题：怎样用一个名字存很多个同类型的值？** → array。
**第二个问题：资料本身是一张"表"怎么办？** → 2D array。
**第三个问题：车款数量会变（明年加新车）怎么办？** → vector。

---

## 1. One-dimensional array（p2–9）

**Array** = a collection of a **fixed number** of elements of the **same data type**；1D array 的元素排成**一列 (list form)**；每个元素用 **index / subscript** 存取，**永远从 0 开始**。

```
 score   ┌────┬────┬────┬────┬────┬────┬────┬────┐
         │ 95 │ .. │ .. │ .. │ 70 │ .. │ .. │ .. │
         └────┴────┴────┴────┴────┴────┴────┴────┘
 index     0    1    2    3    4    5    6    7
          ↑1st location       ↑5th location
```
*看图重点：第 1 个位置是 index 0，第 5 个是 index 4——"第 n 个" = index n-1。考题说"第二个元素"就是 `[1]`。*

**Declaration**：`<data type> <array name>[<array size>];` → `int score[8];`（8 格，内容未知 `?`）

**Assignment**
```cpp
score[0] = 95;             // 1st location
score[4] = 70;             // 5th location
score[3] = 88;             // 4th element
i = 2; score[i] = 45;      // index 可以是变量 → 3rd element
score[7] = score[7] + 5;   // 最后一个元素加 5
```

**Initialization（compile-time）**：用 `{ }`，逗号分隔
```cpp
int score[8] = {95, 8, 85, 67, 70, 60, 78, 90};
float costs[4] = {12.00, 5.00, 2.50, 7.25};
int ARR[] = {2, 3, 1};     // 全部给值时 [] 可留空 → 等于 int ARR[3]
```
⚠️ Slide p7 写 `{95,08,…}`——在 C++ 里以 0 开头的数字是**八进制**，`08` 不合法（8 不是八进制数字），会 compile error。写 `8` 就好。

**Run-time 输入**：用 loop 一格一格读（p10）
```cpp
int testscores[8], score;
for (int i = 0; i < 8; i++) {
    cin >> score;
    testscores[i] = score;   // 或直接 cin >> testscores[i];
}
```

**Index 可以是表达式（p17）**：`int x[5] = {1,2,3,8,7}; int i = 2;` → `x[i+1]` 是 `x[3]`（第 4 个 = 8），`x[2*i]` 是 `x[4]`（第 5 个 = 7），`x[0]` = 1。

---

## 2. 1D array 的常见操作（p10–17）

处理**整个** array 一定要用 `for` loop。以 `double sales[10];` 为例：

```cpp
for (index = 0; index < 10; index++) sales[index] = 0.0;    // 全部设 0
for (index = 0; index < 10; index++) cin >> sales[index];   // 读入
for (index = 0; index < 10; index++) cout << sales[index];  // 印出

double sum = 0;                                              // ← 求和与平均
for (index = 0; index < 10; index++) sum += sales[index];
average = sum / 10.0;

largest = sales[0];                                          // ← 找最大
for (index = 1; index < 10; index++)                         //   从 1 开始，因为 [0] 已是起点
    if (largest < sales[index]) largest = sales[index];
```
⚠️ Slide p13 写 `int sum = 0;`，但 `sales` 是 `double`——`sum += sales[index]` 会把小数截掉。要用 `double sum = 0;`。

**Double 每个元素**：`for (i=0;i<8;i++) testscore[i] *= 2;` → `{1..8}` 变 `{2,4,…,16}`。
**由一个 array 产生另一个**：`SQUARE[i] = ARR[i] * ARR[i];` → `{2,1,9,6,5,4,7,3}` 变 `{4,1,81,36,25,16,49,9}`。

### Tutorial 1 Q1（1D 综合，已执行）
```cpp
int num[7] = {2, 3, 5, 7, 11, 13, 17};
int total = 0, count = 0;
for (int i = 0; i < 7; i++) {
    num[i] *= 2;                          // 4 6 10 14 22 26 34
    if (num[i] % 3 == 1) {                // 余 1 的：4, 10, 22, 34
        cout << num[i] << " ";
        total += num[i];
        count++;
    }
}
cout << "\nTotal = " << total << ", Average = " << (double)total / count << endl;
```
Trace（每个都要算，不要凭感觉）：

| 原值 | ×2 | %3 | 要吗 |
|---|---|---|---|
| 2 | 4 | 1 | ✅ |
| 3 | 6 | 0 | |
| 5 | 10 | 1 | ✅ |
| 7 | 14 | 2 | |
| 11 | 22 | 1 | ✅ |
| 13 | 26 | 2 | |
| 17 | 34 | 1 | ✅ |

输出（g++ 验证）：`4 10 22 34` / `Total = 70, Average = 17.5`。注意平均要 `(double)total / count`，否则 70/4 = 17（整数除法）。

---

## 3. Two-dimensional array（p18–26）⭐

**2D array = 表格**：`array_name[row number][column number]`。

```
 ARR_2D[3][4]        col 0   col 1   col 2   col 3
               row 0 │  2   │  4   │  6   │  8   │
               row 1 │  1   │  3   │  5   │  7   │   ← ARR_2D[1][3] = 7
               row 2 │  1   │  4   │  9   │  16  │
```
*看图重点：第一个 [] 永远是 row（第几行），第二个是 column（第几列）。*

```cpp
int ARR_2D[3][4] = { {2,4,6,8},
                     {1,3,5,7},
                     {1,4,9,16} };   // 一行一个 { }
```

| | 1D | 2D |
|---|---|---|
| Declaration | `int Arr[5];` | `int Arr[2][3];` |
| Initialization | `int Arr[5] = {1,2,3,4,5};` | `int Arr[2][3] = {{2,2,2},{3,3,3}};` |
| Referencing | `cout << Arr[2];` | `cout << Arr[1][2];` |

### 3.1 怎么决定 row 和 column？（slide 没讲，考试最关键）

> **看题目的表格**：表格左边列出的东西（车款、星期几）= **row**；表格上面的标题（季度、产品）= **column**。题目若直接说 "row and column represent X and Y respectively"，就照题目。

| 题目 | Row | Column | 宣告 |
|---|---|---|---|
| Jan 2026 车款表 | 6 款车 | 4 个季度 | `double sales[6][4];` |
| Oct 2025 商店 | 7 天（Mon 起） | 10 个产品 | `double sales[7][10];` |
| Slide p25 酒店 | 25 层 | 15 间房 | `int hotel[25][15];` |
| Tutorial 1 Q5 水果 | 4 种水果 | 3 间店 | `double price[4][3];` |

### 3.2 四种处理方式（p21–24）

| 处理 | 固定哪个 | loop 哪个 | 例子 |
|---|---|---|---|
| **Single row** | row | col | 第 2 行总和：`row = 1; for (col=0;col<4;col++) sum += ARR_2D[row][col];` → 1+3+5+7 = **16** |
| **Single column** | col | row | 第 4 列总和：`col = 3; for (row=0;row<3;row++) sum += ARR_2D[row][col];` → 8+7+16 = **31** |
| **Row by column** | — | 外 row、内 col | 全表总和（逐行扫）= **66** |
| **Column by row** | — | 外 col、内 row | 全表总和（逐列扫）= **66**（结果一样，顺序不同） |

> 💬 **答题句 (EN)** — *To process a single row, fix the row index and loop through the columns; to process a single column, fix the column index and loop through the rows; to process the whole table, use a nested for loop — the outer loop for rows and the inner loop for columns.*

### ✅ 满分答法 — Jan 2026 Q1b（车款 × 季度）

```cpp
// (i) Declare (2 marks)  — rows = 6 cars (MyVi..X50), columns = 4 quarters
double sales[6][4];

// (ii) Ativa (5th car → row 4), 3rd quarter (column 2) (2 marks)
sales[4][2] = 295000;

// (iii) Total sales in 4th quarter (column 3) for all cars (7 marks)
double total = 0;                        // (1) initialise accumulator
for (int row = 0; row < 6; row++)        // (2) loop through all 6 cars
    total += sales[row][3];              // (2) fix column 3 = 4th quarter
cout << "Total sales in 4th quarter: RM"             // (2) display
     << fixed << setprecision(2) << total << endl;   // (#include <iomanip>)
```
**(iv) Nested for loop 的情境与理由 (3)**：
*Situation:* displaying (or entering, or totalling) the sales of **all 6 cars for all 4 quarters**. *Reason:* a 2D array has two indexes, so the outer loop moves through each row (car) and the inner loop moves through each column (quarter) of that row, visiting every element without writing 24 separate statements.
```cpp
for (int row = 0; row < 6; row++)
    for (int col = 0; col < 4; col++)
        cin >> sales[row][col];
```

### ✅ 满分答法 — Oct 2025 Q1c（7 天 × 10 产品）
```cpp
// (i) Declare (2): rows = 7 days (Mon=0 ... Sun=6), columns = products 1..10
double sales[7][10];

// (ii) Total sales on Friday (7): Friday = row 4
double totalFri = 0;
for (int col = 0; col < 10; col++)
    totalFri += sales[4][col];
cout << "Total sales on Friday: RM" << fixed << setprecision(2) << totalFri << endl;
```
（评分重点：Friday 是 **row 4** 不是 5；loop 10 个 column；有初始化 0；有显示。）

### Slide p25–26 酒店练习（已执行）
```cpp
int hotel[25][15];                                   // a) 25 levels × 15 rooms
for (int level = 10; level <= 11; level++)           // b) level 11 & 12 → index 10, 11
    for (int room = 0; room < 15; room++) {
        cout << "Guests in room " << level+1 << "-" << room+1 << ": ";
        cin >> hotel[level][room];
    }
hotel[17][4] = 4;                                    // c) room 18-5 → [17][4]
int total = 0;                                       // d) total guests on 2 levels
for (int level = 10; level <= 11; level++)
    for (int room = 0; room < 15; room++)
        total += hotel[level][room];
cout << "Total guests: " << total << endl;
```

### Tutorial 1 Q5（最便宜的水果）
```cpp
string fruit[4] = {"Orange","Apple","Pear","Peach"};
string store[3] = {"Store A","Store C","Store T"};
double price[4][3] = {{0.80,1.00,0.69},{1.20,1.50,1.80},{1.50,1.55,1.29},{2.00,1.99,1.80}};

void get_cheap_apple() {                 // (b) row 1 fixed
    int best = 0;
    for (int c = 1; c < 3; c++) if (price[1][c] < price[1][best]) best = c;
    cout << store[best] << " sells the cheapest apple\n";           // Store A (1.20)
}
void storeC_cheapest_fruit() {           // (c) column 1 fixed
    int best = 0;
    for (int r = 1; r < 4; r++) if (price[r][1] < price[best][1]) best = r;
    cout << "Cheapest fruit at Store C: " << fruit[best] << endl;   // Orange (1.00)
}
void get_cheapest_fruit() {              // (d) whole table
    int br = 0, bc = 0;
    for (int r = 0; r < 4; r++)
        for (int c = 0; c < 3; c++)
            if (price[r][c] < price[br][bc]) { br = r; bc = c; }
    cout << fruit[br] << " at " << store[bc] << endl;               // Orange at Store T (0.69)
}
```
（这里把 array 放 global 是为了配合 Tutorial 1 还没教 function 参数；学完 Ch6 应改成把 array 传进 function。）

---

## 4. Vector（p27–34）⭐

普通 array 的 size 在 compile time 就定死了。如果车款数量会变、使用者要输入多少笔资料不知道——就用 **vector**。

- **Vector** 是 **dynamic array**，以 **class** 实作，可以呼叫它的 methods/functions。
- **Size 不固定**：run time 可以**增加或删除**元素。
- 要 `#include <vector>`；语法：`vector<data type> vectorName;`

### 4.1 Normal array vs vector（考题最爱）

| | Normal array | Vector |
|---|---|---|
| Size | **Fixed**，宣告时决定，之后不能改 | **Dynamic**，run time 可增加/减少 |
| 本质 | 内建的资料结构 | **Class**（有 member functions） |
| 增删元素 | 不能真的增删，只能覆盖值 | `push_back`, `pop_back`, `insert`, `erase`, `clear` |
| 知道自己多大 | 不知道，要另外记 size | `size()` |
| 越界检查 | 没有 | `at(i)` 会检查（`[]` 不检查） |
| 宣告 | `int scores[10];` | `vector<int> scores;` |
| Header | 不需要 | `#include <vector>` |

> 💬 **答题句 (EN)** — *A normal array has a fixed size that must be decided at declaration and cannot change, whereas a vector is a dynamic array implemented as a class whose size can grow or shrink at run time using member functions such as push_back() and erase(). A vector is recommended when the number of elements is not known in advance or changes while the program runs.*

### ✅ 满分答法 — Oct 2025 Q1a (6 marks)
Two differences (2 × 2):
1. **Size:** a normal array has a fixed size decided at declaration; a vector is dynamic and can grow or shrink during run time.
2. **Functions:** a vector is implemented as a class, so it provides member functions (e.g. `push_back()`, `size()`, `erase()`) to add, remove and count elements; a normal array has no such functions.

Declarations (1 + 1):
```cpp
int scores[10];            // normal array of 10 integers
vector<int> scores;        // vector array (#include <vector>)
```

### ✅ 满分答法 — Jan 2026 Q1a (2 + 4 marks)
**When (2):** a vector is recommended when the number of elements is **not known in advance or will change during run time** (elements need to be added or removed), e.g. storing an unknown number of customer orders entered by the user.
```cpp
#include <vector>              // (1)
vector<int> numbers;           // (2) declare
numbers.push_back(50);         // (1) add 50 to the end
```

### 4.2 基本函数（p28–29）

| Function | 作用 | 例子 |
|---|---|---|
| `push_back(val)` | 加到**最后** | `vec.push_back(5);` |
| `pop_back()` | 删除**最后一个** | `vec.pop_back();` |
| `size()` | 目前元素数量 | `vec.size();` |
| `empty()` | 空的话 true | `vec.empty();` |
| `clear()` | 删除全部 | `vec.clear();` |
| `at(index)` | 取元素（**有越界检查**） | `vec.at(1);` |
| `[ ]` | 取元素（**不检查**） | `vec[0];` |
| `front()` / `back()` | 第一个 / 最后一个**的值** | `vec.front();` |
| `insert(pos, val)` | 在 pos 插入 | `vec.insert(vec.begin()+2, 10);` |
| `erase(pos)` | 删除 pos 的元素 | `vec.erase(vec.begin()+1);` |
| `resize(n)` | 改 size 为 n | `vec.resize(5);` |
| `swap(other)` | 与另一个 vector 互换内容 | `vec1.swap(vec2);` |

**size() vs capacity()（p30）**：size = 目前**存了**几个；capacity = 在需要重新配置记忆体之前**最多能放**几个（capacity ≥ size）。

### 4.3 begin() vs front()（p31–32）

```
   v.begin()                              v.end()
      ↓                                      ↓
    ┌────┬────┬────┬────┬────┬────┐ ┌────┐
    │    │    │    │    │    │    │ │    │  ← end() 指向"最后一个之后"的空位
    └────┴────┴────┴────┴────┴────┘ └────┘
    v.front()               v.back()
```
*看图重点：begin()/end() 是**位置**（iterator，像 pointer）；front()/back() 是**值**。end() 不是最后一个元素，而是最后一个的**下一格**。*

- `begin()` 回传指向第 1 个元素的 **iterator**，用于 loop、insert、erase。
- `front()` 回传第 1 个元素的**值**。
- `*it`：用 `*` **dereference** iterator，读或改它指向的值（→ Ch4 pointer）。

所以 "删除第二个元素" = `erase(begin() + 1)`：begin() 指第 1 个，+1 就是第 2 个。

### ✅ 满分答法 — Oct 2025 Q1b (2 + 2 marks)
```cpp
numbers.push_back(23);               // (i) add 23 to the end of numbers
numbers.erase(numbers.begin() + 1);  // (ii) remove the 2nd element (index 1)
```

### 4.4 Slide p33–34 完整例子（g++ 验证）
```cpp
vector<int> num = {1,2,3,4,5};
num.push_back(10); num.push_back(20);        // 1 2 3 4 5 10 20
num.pop_back();                              // 1 2 3 4 5 10
num.erase(num.begin()+2);                    // 删 3   → 1 2 4 5 10
num.insert(num.begin()+1, 30);               // 插 30  → 1 30 2 4 5 10
cout << num.front() + num.back();            // 1 + 10 = 11
vector<int>::iterator it = num.begin()+1;    // 指向 30
cout << *it;                                 // 30
it = it + 3;                                 // 指向 5
*it = 999;                                   // 1 30 2 4 999 10
```
执行结果与 slide 注释一致：`1 2 3 4 5 10 20` → `1 30 2 4 5 10` → `11` → `30` → `1 30 2 4 999 10`。
⚠️ Slide 用 `void main()`；标准 C++ 必须 `int main()`（Visual Studio 容许 `void main`，g++ 会报错）。考试照讲师习惯写哪个都行，但 `int main()` 永远正确。

### Tutorial 1 Q6–Q8（已执行）
```cpp
// Q6
vector<int> numbers;
numbers.push_back(10); numbers.push_back(20); numbers.push_back(30);
cout << numbers.size() << endl;                          // 3
cout << numbers.front() << " " << numbers.back() << endl; // 10 30
numbers.pop_back();
for (int n : numbers) cout << n << " ";                  // 10 20

// Q7
vector<string> fruits = {"banana","apple","orange","cherry"};
cout << fruits[1] << endl;               // apple
fruits[2] = "blueberry";                 // replace orange
fruits.push_back("papaya");
for (string f : fruits) cout << f << " ";

// Q8 appointments
vector<string> appointment = {"10:30","14:15","16:00","18:30"};
appointment[2] = "16:20";
for (int i = 0; i < appointment.size(); i++)             // insert 17:10 after 16:20
    if (appointment[i] == "16:20") { appointment.insert(appointment.begin()+i+1, "17:10"); break; }
for (int i = 0; i < appointment.size(); i++)             // cancel 10:30
    if (appointment[i] == "10:30") { appointment.erase(appointment.begin()+i); break; }
for (string t : appointment) cout << t << " ";           // 14:15 16:20 17:10 18:30
```
（`for (int n : numbers)` 是 range-based for loop：依序取出每个元素，读值很方便。）

---

## Closing the loop

车行老板的表：用 `double sales[6][4]` 存；改一格用 `sales[row][col] = …`；算一个季度就固定 column、loop row；全表用 nested loop。明年加新车？把车款名单放进 `vector<string>` 就能 `push_back`。

下一章处理另一种"一排东西"：**string**（其实就是一串 char），以及把数字和字串互相转换、用 regex 验证格式。

---

## ⚠️ Where the slides mislead

| Slide | Slide 写 | 更准确 |
|---|---|---|
| p7 | `int score[8] = {95,08,…}` | `08` 是不合法的八进制常数，compile error；写 `8` |
| p13 | `int sum = 0;` 加总 `double sales[]` | 用 `double sum`，否则小数被截掉 |
| p10 | `cin >> score; testscores[i] = score;` 没宣告 `score` | 要宣告 `int score;`（或直接 `cin >> testscores[i];`） |
| p33 | `void main()` | 标准 C++ 是 `int main()` |
| p33–34 | 字串用 “ ” 弯引号 | 真正写 code 要用直引号 `" "`（slide 是 PowerPoint 自动转换） |

---

## Term table

| English | 中文 | 一句话说明 |
|---|---|---|
| Array | 阵列 | 固定数量、同类型元素 |
| Index / subscript | 索引 / 下标 | 从 0 开始的位置编号 |
| Element | 元素 | array 的一格 |
| Compile-time initialization | 编译期初始化 | 宣告时用 `{ }` 给值 |
| Run-time initialization | 执行期初始化 | 用 loop + cin 读入 |
| Two-dimensional array | 二维阵列 | 表格，`[row][col]` |
| Row / column | 行 / 列 | 第一个 [] / 第二个 [] |
| Nested for loop | 巢状 for 回圈 | 外层 row、内层 col |
| Vector | 向量（动态阵列） | 会变大小的 array，是 class |
| Iterator | 迭代器 | 指向元素的"位置"，像 pointer |
| `begin()` / `end()` | — | 第一个 / 最后一个之后的位置 |
| `front()` / `back()` | — | 第一个 / 最后一个的值 |
| `size()` / `capacity()` | 大小 / 容量 | 目前数量 / 不需重新配置的最大数量 |
| Range-based for | 范围 for | `for (int x : v)` |

---

## Cheat sheet

- 第 n 个元素 = index **n-1**；Friday（Mon=0）= 4；Saturday = 5；Thursday = 3。
- `int a[5];` / `int a[] = {1,2,3};` / `a[i] = x;` / 处理全部用 `for (i=0;i<SIZE;i++)`。
- 找最大：`largest = a[0]; for (i=1; …) if (a[i] > largest) largest = a[i];`
- 2D：`type arr[ROWS][COLS];`；表格左边 = row，上面 = column。
- 单行：固定 row loop col；单列：固定 col loop row；全表：nested loop。
- Vector：`#include <vector>`；`vector<int> v;` `push_back` `pop_back` `insert(v.begin()+i, x)` `erase(v.begin()+i)` `size()` `front()` `back()` `at(i)` `clear()` `empty()`。
- Array vs vector：**fixed vs dynamic size**；**no functions vs class with member functions**。

---

## Practice (answers included)

### A. MCQ
1. `int x[5] = {4,8,1,6,3}; int i = 1;` What is `x[2*i+1]`?  (a) 8 (b) 1 (c) 6 (d) 3
2. Which statement removes the **third** element of vector `v`?  (a) `v.erase(3)` (b) `v.erase(v.begin()+3)` (c) `v.erase(v.begin()+2)` (d) `v.pop_back(3)`
3. `double t[4][6];` stores data for 4 branches × 6 months. How many elements?  (a) 10 (b) 20 (c) 24 (d) 35
4. Which returns the **value** of the last element of vector `v`?  (a) `v.end()` (b) `v.back()` (c) `v.size()` (d) `v.begin()`
5. Which is TRUE?  (a) A normal array can grow with `push_back()` (b) `v.at(i)` checks the index range (c) `v.end()` points to the last element (d) `vector` needs no header

**Answers:** 1-c (`x[3]` = 6), 2-c, 3-c, 4-b, 5-b

### B. Short answer
**B1.** Write statements to find the **average** of the 3rd column of `int m[5][4]`. 
```cpp
int sum = 0;
for (int r = 0; r < 5; r++) sum += m[r][2];
double avg = sum / 5.0;
```
**B2.** Explain the difference between `size()` and `capacity()` of a vector. — size() is the number of elements currently stored; capacity() is how many elements the vector can hold before it must allocate more memory (capacity ≥ size).
**B3.** Why does `erase(v.begin()+1)` remove the second element? — `begin()` returns an iterator to the first element (index 0); adding 1 moves it to index 1, which is the second element.

### C. Application
*A tuition centre records attendance: 5 classes (rows) × 12 weeks (columns), `int att[5][12];`*

**C1.** Set the attendance of class 3, week 10 to 18. → `att[2][9] = 18;`
**C2.** Display the total attendance of week 1 for all classes.
```cpp
int total = 0;
for (int c = 0; c < 5; c++) total += att[c][0];
cout << "Week 1 total: " << total << endl;
```
**C3.** Find which class has the highest total attendance over 12 weeks.
```cpp
int bestClass = 0, bestTotal = -1;
for (int c = 0; c < 5; c++) {
    int t = 0;
    for (int w = 0; w < 12; w++) t += att[c][w];
    if (t > bestTotal) { bestTotal = t; bestClass = c; }
}
cout << "Class " << bestClass + 1 << " has the highest attendance: " << bestTotal << endl;
```

### D. Thinking
**D1.** When would you still choose a normal array over a vector? — When the number of elements is fixed and known (e.g. 7 days, 12 months); an array is simpler and has no class overhead.
**D2.** Row-by-column and column-by-row processing give the same grand total. When does the order matter? — When the output order matters: e.g. displaying the table row by row (as it looks on paper) versus printing a report column by column (all cars for Q1, then Q2…).

---

## Slide index
| Note section | Slides |
|---|---|
| 1 1D array basics | p2–9 |
| 2 1D operations | p10–17 |
| 3 2D array & processing | p18–26 |
| 4 Vector | p27–34 |

## Links to other chapters
- Array **in** a structure / array **of** structures → **Ch3**
- Iterator 与 `*` dereference、array name 是 pointer → **Ch4**
- 把 array 传进 function（pass whole array）→ **Ch6**
- `vector<Passenger>` + typedef struct → **Ch8**
