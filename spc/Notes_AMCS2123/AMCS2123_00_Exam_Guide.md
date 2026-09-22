# AMCS2123 Systems and Programming Concepts — 考前总攻略

> 先读这一页。它告诉你：考卷长什么样、每一小题出自哪一章、哪几章最值钱、slide 哪里写错了、一天怎么复习。
> 各章笔记：讲解用中文，**术语与 code 保持英文**，理论重点有 **💬 答题句 (EN)**，每一道 past year 小题都有 **✅ 满分答法**。所有 code 与输出追踪都在 **g++ 13（-std=c++20）** 实际执行过。

---

## 1. 考卷格式

- **2 小时，4 题，全部要答，每题 25 分**（共 100 分）。
- 读过的两份考卷：**Oct 2025**（`QP - AMCS2123.pdf`，扫描档）与 **Jan 2026**（`AMCS2123.pdf`）。
- **几乎全是写 code**：写宣告、写 function、写 main、追踪输出；理论题（coupling/cohesion、storage class、base case、vector vs array）每题只有 1–2 分。
- 常见 command verbs：*Write C++ statements, Declare, Define, Develop, Construct a function call, Show/Examine the output, Determine the bit pattern, Explain, Identify, Give TWO*。
- 👉 **题目说 "pass by reference" / "pass by address" / "the calling function must be able to access the updated value" 时，一定要用 `&` / `*`**——这是最常见的失分点。

---

## 2. 两份考卷出题地图（每一小题 → 满分答法在哪）

| 题目 | Oct 2025 | 分 | Jan 2026 | 分 | 满分答法 |
|---|---|---|---|---|---|
| Normal array vs vector / 何时用 vector + 宣告 | Q1a | 6 | Q1a | 6 | **Ch1 §4.1** |
| Vector：push_back / erase 第二个 | Q1b | 4 | (Q1a) | | Ch1 §4.3 |
| **2D array**：宣告、改一格、加总一行/一列 | Q1c | 9 | Q1b(i–iii) | 11 | **Ch1 §3** |
| Nested for loop 的情境 | — | | Q1b(iv) | 3 | Ch1 §3 |
| Array in structure vs array of structure | — | | Q1c | 5 | Ch3 §5.2 |
| **Struct**：定义 / 宣告 / 改格 / nested / array of struct | Q2b | 21 | Q2a | 15 | **Ch3 §6** |
| `stod` / `regex_match` 结果 | Q2a | 4 | — | | Ch2 §1, §5 |
| substr 重组产品码 / `regex_search` 用途 | — | | Q3c | 5 | Ch2 §0', §5 |
| **Pointer 输出追踪** | Q3c | 6 | Q3b | 5 | **Ch4 §4** |
| Reference parameter vs return | Q3a | 2 | — | | Ch5 §3.6 |
| **bool function：by reference + by address** | Q3b(i) | 8 | Q4a | 8 | **Ch5 §4** |
| main 呼叫上述 function 并显示 | Q3b(ii) | 9 | — | | Ch5 §4 |
| Modular programming 两个好处 | — | | Q3g | 2 | Ch5 §1 / Ch11 §1 |
| 传 array 进 function（checkCount）+ 呼叫 | Q4c | 11 | — | | Ch6 §2 |
| 传 struct / 成员（updateSalary、checkResult）+ 呼叫 | Q4d | 6 | Q4c | 8 | **Ch6 §3** |
| Storage class 辨认 | — | | Q3d | 2 | Ch6 §5 |
| **Template** | Q4b | 4 | Q2b | 10 | **Ch7** |
| enum Cafe | Q4a | 4 | — | | Ch8 §3 |
| typedef Day/Month/Year + 生日程式 | — | | Q4b | 9 | Ch8 §2 |
| Recursion：base case / activation frames | Q1f | 2 | Q3a | 5 | Ch9 §2, §4 |
| Bitwise | Q1d | 2 | Q3e | 2 | Ch10 |
| Coupling / cohesion | Q1e | 2 | Q3f | 4 | Ch11 |

✅ **两份考卷的每一小题都有对应的满分答法。**

---

## 3. 每章值多少分

| 章 | Oct 2025 | Jan 2026 | 平均 | 优先级 |
|---|---|---|---|---|
| **Ch3** Structure | 21 | 20 | **20.5** | ★★★★★ |
| **Ch1** Array / 2D / vector | 19 | 20 | **19.5** | ★★★★★ |
| **Ch5** Functions I（value/ref/address） | 19 | 8 (+2) | **~14** | ★★★★★ |
| **Ch6** Functions II（array/struct 参数、storage class） | 17 | 10 | **13.5** | ★★★★ |
| **Ch7** Template | 4 | 10 | 7 | ★★★ |
| **Ch8** typedef / enum | 4 | 9 | 6.5 | ★★★ |
| **Ch4** Pointers（追踪） | 6 | 5 | 5.5 | ★★★ |
| Ch2 String / regex | 4 | 5 | 4.5 | ★★ |
| Ch11 Coupling / cohesion | 2 | 4 (+2) | ~4 | ★★ |
| Ch9 Recursion | 2 | 5 | 3.5 | ★★ |
| Ch10 Bitwise | 2 | 2 | 2 | ★（白拿分，10 分钟学会） |

**Ch1 + Ch3 + Ch5 + Ch6 ≈ 七成分数**，而且它们是连在一起的：array → struct（里面有 array）→ function 参数（传 array、struct、by ref/address）。

### "一定要会写"的十个答案
1. 2D array：宣告（表格左 = row、上 = column）、改一格、加总一列/一行（Ch1 §3）
2. Vector：宣告、`push_back`、`erase(begin()+1)`；array vs vector 两个差别（Ch1 §4）
3. Struct 存取路径 `arr[i].member.sub[j]`；nested struct；array in vs of struct（Ch3 §5–6）
4. Pointer 追踪表（每次 `p = &x` 更新指向）（Ch4 §4）
5. `bool f(int num, int &a, int *b)` + main 呼叫 `f(n, a, &b)`（Ch5 §4）
6. 传整个 array：`void f(int arr[], int size)`，呼叫 `f(arr, n)`（Ch6 §2）
7. 传 struct by reference / address，`->`；传成员 + `string *result`（Ch6 §3）
8. Template：一个 T vs T1/T2；void template 直接呼叫（Ch7）
9. enum（名字有空格 → `ZUS_COFFEE`）；typedef `typedef int Day;`（Ch8）
10. Activation frames 画法（Ch9 §4）；bitwise 逐位计算（Ch10）；best coupling = data、best cohesion = functional（Ch11）

---

## 4. 一天复习计划（约 10 小时）

| 时间 | 做什么 |
|---|---|
| 08:00–08:30 | 读这份 Exam Guide |
| 08:30–10:00 | **Ch1**：2D array 四种处理、两份考卷 Q1 自己手写一次；vector 函数表 |
| 10:00–11:30 | **Ch3**：Jan 2026 Q2a、Oct 2025 Q2b 盖住答案自己写；存取路径规则 |
| 11:30–12:30 | **Ch4**：两题 pointer 追踪自己画表；Tutorial 4 Q14 |
| 12:30–13:30 | 午餐 |
| 13:30–15:00 | **Ch5 + Ch6**：三种传递方式对照表；processNumber / analyseNum + main；checkCount；updateSalary；checkResult；storage class 表 |
| 15:00–15:45 | **Ch7 + Ch8**：template 两种写法；enum / typedef 考题 |
| 15:45–16:30 | **Ch2 + Ch9 + Ch10 + Ch11**：substr、regex match vs search；sum(3) frames；bitwise 四题；coupling/cohesion 排序 |
| 16:30–18:30 | **模拟考**：Jan 2026 整份，计时 2 小时手写，对照满分答法 |
| 20:00–21:00 | 订正错误；最后读每章 Cheat sheet |

---

## 5. 写 code 题的答题技巧

1. **Index 换算**：第 n 个 = n−1；Monday = 0 → Thursday = 3、Friday = 4、Saturday = 5；最后一个学生（20 人）= 19。
2. **宣告前先判断类型**：金额、薪水、温度 → `double`；人数、分数 → `int`；名字 → `string`。
3. **Pass by reference**：参数 `type &x`，呼叫不加 `&`，内部不加 `*`。**Pass by address**：参数 `type *x`，呼叫加 `&`，内部用 `*x`（struct 用 `->`）。
4. **传整个 array** 只写名字 `f(score, 20)`；参数 `int score[]`；2D 参数要写 column 数。
5. **Accumulator**（总和）一定先设 0；平均要用 `double` 或 `/ 10.0`。
6. **显示题**：照题目给的句子一字不差地拼出来（空格、标点）。
7. **追踪题**：画表、逐行更新、最后才写输出；`cout << ptr` 印的是地址，`*ptr` 才是值。
8. **Template**：参数类型相同用 `T`；不同用 `T1, T2`；template 写在 main 前。
9. 用 `int main()`（讲师的 `void main()` 在 Visual Studio 可用，但不是标准）。
10. 记得必要的 `#include`：`<vector>`、`<string>`、`<iomanip>`（setprecision）、`<regex>`、`<algorithm>`（transform）、`<sstream>`、`<format>`。

---

## 6. Slide 的主要错误（考试时注意）

| 章 | Slide | 问题 | 正确 |
|---|---|---|---|
| Ch1 | p7 | `{95,08,…}` | `08` 是非法八进制 → 写 8 |
| Ch1 | p13 | `int sum` 加 double | 用 `double sum` |
| Ch2 | p9 | transform 写进 4 格字串 | 越界；目的地要够大 |
| Ch2 | p22 | `"{:.2f }"`、注释 `5 * 3.14` | 多空格 compile error；印 `5 x 3.14` |
| Ch3 | p16/18 | `char deprtID;` vs `strcpy(… .deptID, "IT999")` | 名字不符、单 char → 改 `char deptID[10]` |
| Ch3 | p20 | struct 定义没 `;` | `};` |
| Ch6 | p14 | `num[1][2]`（num 是 [3][2]） | 越界 |
| Ch6 | p15 | `int x[][]` | 第二维必须写 |
| Ch6 | p20 | `cout << cgpa;` | `s->cgpa` |
| Ch6 | p35 | static counter 注释 1, 2 | 实际 **0, 1** |
| Ch7 | p9 | `cout << printPair(…)`（void） | compile error |
| Ch7 | p10 | 输出 `false` | 实际印 `0`（没 boolalpha） |
| Ch8 | p10 | `cons int SUN` | `const` |
| Ch8 | p14 | `enum Program {…RSD, RSF, RSD…}` | RSD 重复 → 不合法 |
| Ch9 | p15 | Factorial frames 用 `+`、停在 1 | 用 `*`，到 Factorial(0) |
| Ch10 | p7 | 右移 "bits inserted on right" | 补在**左边** |
| Ch11 | p21 | Strong cohesion 条件 "modules can access the same variables" | 那是 tight coupling |

---

## 7. 资料与限制

- **读过**：Chap01–Chap11 全部 slides、Tutorial 1–11、两份 past year（Oct 2025 扫描档已逐页看图；Jan 2026 为文字 PDF，表格页也看过图）。
- **所有含图的 slide 页**（array/2D 示意、vector begin/end、regex 表、struct 记忆体图、pointer 示意与 new 例子、structure chart、flowchart、storage class 图、scope 图、activation frames、Fibonacci 树、shift 示意、真值表、XOR 加密、ATM 流程、coupling/cohesion 阶梯与例子、cohesion 判断表）都已看过，并在笔记中重画或以表格呈现。
- **Code 验证**：所有 slide 例子、past year 答案、tutorial 答案都用 g++ 13.3（`-std=c++20`）编译执行；输出与笔记一致。追踪题的数值全部由程式确认。
- **有问题的档案**：
  - `Tutorial 3 - Advanced Structure (Student).pdf.crdownload` 是**未下载完成**的档案，文字有缺漏（例如 Q1(i) 的初始值）。Ch3 §7 按可读部分作答，缺的数值以合理假设标明。建议重新下载完整的 Tutorial 3。
  - Chap07、Chap08 档名标示 "Notyetready"，但内容完整且已出现在两份考卷，所以照常整理。
  - Chap02 p26 练习页是空白的。
- **标注为 *extra* 的内容**（不在 slide）：`nullptr`、`delete`、`boolalpha`、`<cstdint>` 固定大小整数、enum 自订起始值（`PARK = 1`）、regex 的 `[ ]` `{m,n}` `\\.`、masking 操作表、`const &` 传 struct——都已在笔记中注明。
- 没有官方 answer scheme；满分答法依题目分数分配自行撰写，并以编译执行确认正确。

---

## 各章笔记

| 文件 | 内容 |
|---|---|
| `AMCS2123_Ch1_Advanced_Array_Manipulation.md` | 1D、**2D array**（row/column 判断、四种处理）、**vector** |
| `AMCS2123_Ch2_Advanced_String_Functions.md` | substr、stoi/stod、find_first_of、stringstream、transform、**regex match vs search**、format |
| `AMCS2123_Ch3_Advanced_Structure.md` | struct、array in / of struct、**nested struct**、存取路径 |
| `AMCS2123_Ch4_Pointers.md` | `&` `*`、**追踪表**、new、pointer arithmetic、reference |
| `AMCS2123_Ch5_Advanced_Functions_I.md` | modular programming、**pass by value / reference / address**、混合传递模板 |
| `AMCS2123_Ch6_Advanced_Functions_II.md` | 传 array / 2D / struct、`->`、return struct、**storage classes**、scope、duration |
| `AMCS2123_Ch7_Templates.md` | `template <typename T>`、T1/T2 |
| `AMCS2123_Ch8_User_Defined_Types.md` | typedef、**enum**（合法规则、运算、switch、function） |
| `AMCS2123_Ch9_Recursion.md` | base case、**activation frames**、sum / factorial / Fibonacci |
| `AMCS2123_Ch10_Bitwise_Operators.md` | `~ & | ^ << >>`、反推、masking、XOR 加密 |
| `AMCS2123_Ch11_Program_Design.md` | 命名、**coupling 4 级**、**cohesion 7 级**、判断表 |
