# AMCS2123 — Chapter 4: Pointers

> 两份考卷都有一题 **pointer 输出追踪（5–6 分）**，而且一定混用 `*ptr` 和变数名、会在中途**改 pointer 指向**。Slide 讲了定义和四种 `*p++` 组合，但**没有教追踪的方法**——学生最常错在"pointer 换了指向之后，`*ptr` 指的是谁"。Slide 的 exercise（p25–27）也没有答案。这份笔记给一个**记忆体表格追踪法**，把 slide 与 tutorial 的每一题都用 g++ 跑过，并指出 slide 与 tutorial 中的两个陷阱（`void main`、同一式子里 `++v` 与 `*ptr1` 的 undefined behaviour）。

---

## 0. 一句话总览

**每个变数都有地址（`&x`）；pointer 是"存地址的变数"（`int *p = &x;`）；`*p` 是"去那个地址拿/放值"（dereference）；pointer 可以加减（按类型大小移动）；reference（`int &r = x;`）是变数的别名。**

| Part | 内容 | Slide |
|---|---|---|
| 1 | 地址与 `&`；pointer 定义与宣告 | p3–6 |
| 2 | 给 pointer 赋值；`*` dereference | p7–10 |
| 3 | NULL pointer；pointer 例子 | p11–13 |
| 4 | Dynamic variable：`new` | p14–15 |
| 5 | Pointer arithmetic；`*p++` 四种组合 | p16–22 |
| 6 | Reference variable | p23–24 |
| 7 | Exercises | p25–27 |

🎯 **这章回答的考题**
- **Jan 2026 Q3b** 追踪 `pPtr`, `qPtr` 的输出 (5)
- **Oct 2025 Q3c** 追踪 `m`, `n`，最后印 `&m`（给定地址）(6)
- 间接：Ch5–6 的 **pass by address**（`int *cube`、`string *result`）全靠这章

**Prerequisites**：变数、`cout`；Ch1 的 array（array name 其实是 pointer，→ Ch6）。

---

## Scene：寄物柜

商场有一排寄物柜，每格有**编号**（地址）和**里面的东西**（值）。
- 你把背包放进 A17 → 变数 `bag` 的值是背包，`&bag` 是 A17。
- 你把"A17"写在一张**纸条**上交给朋友 → 纸条就是 **pointer**：它本身不装背包，只装"去哪里找"。
- 朋友拿着纸条去 A17 拿背包 → 这就是 `*纸条`（dereference）。
- 纸条改写成 B03 → 从此"去纸条那里"就拿到 B03 的东西，A17 的背包没动过。

最后这一句就是所有 pointer trace 题的陷阱。

---

## 1. 地址与 pointer（p3–6）

- 每个变数都有记忆体位置，位置有**地址**，用 **`&`（address operator）** 取得：
```cpp
int var1; char var2;
cout << "Address of var1 : " << &var1;
cout << "Address of var2 : " << (void *)&var2;   // char 的地址要转 void*，否则 cout 会当成字串印
```
- **Pointer** = a variable whose value is the **address of another variable**。

```
   P (pointer)                A (int variable)
  addr BC231                  addr 10FFA
 ┌──────────┐                ┌──────────┐
 │  10FFA   │ ─────────────> │   300    │
 └──────────┘                └──────────┘
  stores an address only       stores the value
```
*看图重点：pointer 自己也有地址（BC231），它的**内容**是别人的地址（10FFA）。*

**宣告**：`type *pointerName;`
```cpp
int *ip;      // pointer to an int
double *dp;   // pointer to a double
char *ch;     // pointer to a char
```
- Pointer **只能指向同类型**的变数（`int*` 不能指 `double`）。
- `*` 可以放在类型和名字之间的任何位置：`int *p;` `int* p;` `int * p;` `int*p;` 都一样。
- ⚠️ **陷阱**：`int *p, q;` → 只有 **p** 是 pointer，**q 是普通 int**。要两个都是 pointer：`int *p, *q;`

---

## 2. 赋值与 dereference（p7–10）

- 宣告只配置 pointer 自己的空间，**不指向任何东西**。用 `&` 让它指向：
```cpp
int *ptr, x = 10;
ptr = &x;           // ptr 存 x 的地址 → "ptr points to x"
```
- 一行写法：`double a = 10; double *number = &a;` 等于 `double *number; number = &a;`
- ⚠️ **不能**写 `*number = &a;`——宣告之后 `*number` 的意思变成"number 指向的那个 double"，把地址塞进 double 是错的。

**`*` 的三种身份**（Tutorial 4 Q4）：

| 位置 | 名称 | 意思 | 例子 |
|---|---|---|---|
| 宣告中 | pointer declarator | "这是一个 pointer" | `int *p;` |
| 表达式中（一元） | **dereference / indirection operator** | "去 p 指的地方" 读或写 | `cout << *p; *p = 20;` |
| 两个运算元之间（二元） | multiplication | 乘法 | `a * b` |

```cpp
int num = 10, *ptr = &num;
cout << *ptr;    // 10    —— 读
*ptr = 20;       // num 变成 20 —— 写（透过 pointer）
```

**Slide p10**：`int a = 7, *p = &a;`
| 印什么 | 结果 |
|---|---|
| `a` / `*p` | 7 / 7 |
| `&a` / `p` | 同一个地址（如 0020FF00） |
| `&p` | pointer 自己的地址（另一个，如 0020FEF4） |
| `*p = 10;` 后 `a` | 10 |

> 💬 **答题句 (EN)** — *A pointer is a variable that stores the memory address of another variable of the same type. The address operator & gives the address of a variable, and the dereference operator * accesses the value stored at the address held by the pointer.*

---

## 3. NULL pointer 与例子（p11–13）

C++ 不会自动初始化变数；没初始化的 pointer 指向"随机地址"，非常危险。用 **null pointer**：`p = NULL;` 或 `p = 0;`（现代 C++ 用 `nullptr`）。

**p12**：
```cpp
int value1 = 5, value2 = 15;
int *mypointer = NULL;
mypointer = &value1;  *mypointer = 10;   // value1 = 10
mypointer = &value2;  *mypointer = 20;   // value2 = 20
cout << value1 << "/" << value2;         // 10/20
```
**p13**（注意"给值" vs "给地址"）：
```cpp
int value1 = 5, value2 = 15, *p1, *p2;
p1 = &value1; p2 = &value2;
*p1 = 10;        // value1 = 10
*p2 = *p1;       // 赋"值"：value2 = 10
p1 = p2;         // 赋"地址"：p1 现在也指 value2
*p1 = 20;        // value2 = 20
cout << value1 << "/" << value2;         // 10/20
```
| 写法 | 意思 |
|---|---|
| `*p2 = *p1;` | 把 p1 指的**值**复制到 p2 指的地方 |
| `p1 = p2;` | 让 p1 **指向** p2 所指的同一个地方 |

---

## 4. 记忆体表格追踪法（考试必用）

> **画一张表：每个变数一栏、每个 pointer 一栏（写它"指向谁"）。逐行执行：**
> - `p = &x;` → 在 p 那栏写 "→x"
> - `*p = …` / `… *p …` → 先查 p 指向谁，再去**那个变数**读/写
> - 输出时把值抄下来。

### ✅ 满分答法 — Jan 2026 Q3b (5 marks)
```cpp
int *pPtr, *qPtr;  int p, q, s;
pPtr = &p;  qPtr = &q;
*pPtr = 12;  q = 4;
cout << "p = " << p << "\nq = " << *qPtr << endl;
s = *pPtr * q;
cout << "s = " << s << endl;
(*pPtr) = *pPtr / 2;
q = *qPtr + 10;
cout << "p = " << p << "\nq = " << q << endl;
```
| 步骤 | pPtr | qPtr | p | q | s |
|---|---|---|---|---|---|
| `pPtr=&p; qPtr=&q;` | →p | →q | ? | ? | ? |
| `*pPtr = 12;` | →p | →q | **12** | ? | ? |
| `q = 4;` | | | 12 | **4** | |
| print | | | | | |
| `s = *pPtr * q;` = 12 × 4 | | | 12 | 4 | **48** |
| `(*pPtr) = *pPtr / 2;` = 12/2 | | | **6** | 4 | 48 |
| `q = *qPtr + 10;` = 4+10 | | | 6 | **14** | 48 |

**Output**（g++ 验证）：
```
p = 12
q = 4
s = 48
p = 6
q = 14
```
（每行 1 分。注意 `*pPtr * q` 中第二个 `*` 是乘法。）

### ✅ 满分答法 — Oct 2025 Q3c (6 marks)
地址：p = OE89XC0，q = OE89XC4，r = OE89XC8。
```cpp
int p = 20, q, r;  int *m, *n;
m = &p;  n = &q;
*n = 88;
r = *m + q;
cout << "p = " << *m << endl;
cout << "q = " << q << endl;
cout << "r = " << r << endl;
m = &r;
(*n)++;
p = *m - *n;
cout << "p = " << p << endl;
cout << "q = " << q << endl;
cout << "&m = " << m << endl;
```
| 步骤 | m | n | p | q | r |
|---|---|---|---|---|---|
| 宣告 | ? | ? | 20 | ? | ? |
| `m=&p; n=&q;` | →p | →q | 20 | ? | ? |
| `*n = 88;` | →p | →q | 20 | **88** | ? |
| `r = *m + q;` = 20+88 | | | 20 | 88 | **108** |
| print p, q, r | | | | | |
| `m = &r;` ⚠️ 换指向 | **→r** | →q | 20 | 88 | 108 |
| `(*n)++;` → q+1 | | | 20 | **89** | 108 |
| `p = *m - *n;` = r − q = 108−89 | | | **19** | 89 | 108 |

**Output**（g++ 验证数值）：
```
p = 20
q = 88
r = 108
p = 19
q = 89
&m = OE89XC8
```
最后一行：虽然写 `"&m = "`，但印的是 **`m` 的值**，也就是 m 所指的地址 = **r 的地址 OE89XC8**（不是 m 自己的地址）。这是本题陷阱。（题目给的地址含字母 O、X，不是合法 16 进位，照抄即可。）

---

## 5. Dynamic variable：`new`（p14–15）

`new` 在执行时建立一个**没有名字**的变数，只能透过 pointer 使用：
```cpp
int *p1, *p2;
p1 = new int;   *p1 = 42;
p2 = p1;                            // p2 指向同一个无名变数
cout << *p1 << " " << *p2;          // 42 42
*p2 = 53;
cout << *p1 << " " << *p2;          // 53 53   (同一个)
p1 = new int;   *p1 = 88;           // p1 改指新的无名变数
cout << *p1 << " " << *p2;          // 88 53
```
（extra：用完应 `delete p1;` 释放，否则 memory leak。）

### Tutorial 4 Q5（已执行）
```cpp
int *p1, *p2;
p1 = new int;  p2 = new int;
*p1 = 10;  *p2 = 20;
cout << *p1 << " " << *p2 << endl;   // 10 20
p1 = p2;
cout << *p1 << " " << *p2 << endl;   // 20 20
*p1 = 30;
cout << *p1 << " " << *p2 << endl;   // 30 30
```
改成 `*p2 = 30;` 输出**一样**（30 30），因为 p1 和 p2 此时指向同一个变数。（第一个 `new int` 从此没有 pointer 指着 → memory leak。）

---

## 6. Pointer arithmetic（p16–22）

- 可以对 pointer 做 `++`, `--`, `=`, `+`, `-`。
- **pointer + 1 = 移到下一个同类型元素**，地址增加 **sizeof(类型)** 个 byte：

```
 char  *mychar  = 1000;  ++mychar  → 1001   (+1 byte)
 short *myshort = 2000;  ++myshort → 2002   (+2 bytes)
 long  *mylong  = 3000;  ++mylong  → 3004   (+4 bytes, 在 Windows；64-bit Linux 是 8)
 int   *p       = X;     p + 1     → X + 4
```
- p20：`pnPtr`, `pnPtr+1`, `+2`, `+3` 印出的地址各差 4。
- p21：`x = &c; p = x + 2; q = x - 2; a = p - q;` → **a = 4**（两个 pointer 相减 = 中间相隔几个**元素**，不是几个 byte）。

### `*p++` 四种组合（p22）

| 写法 | 等于 | 做什么 | `int arr[]={10,20,30}; p=arr;` 结果 |
|---|---|---|---|
| `*p++` | `*(p++)` | 先取 `*p`，**再**把 p 往后移 | 得 10，p 指到 20 |
| `*++p` | `*(++p)` | **先**把 p 往后移，再取值 | 得 20 |
| `++*p` | `++(*p)` | 把 p 指的**值**加 1（先加后用） | 得 11，arr[0] = 11 |
| `(*p)++` | | 把 p 指的**值**加 1（先用后加） | 得 10，arr[0] 变 11 |

（g++ 验证：`10 20 | 20 | 11 11 | 11 12`——最后一栏是在前一栏已改为 11 的基础上。）记法：**没有括号时 `++` 靠 p 近就移 pointer；括号包住 `*p` 就改值。**

---

## 7. Reference variable（p23–24）

- **Reference** = 一个已存在变数的**别名 (alias)**；宣告时用 `&`：`type &newName = existingName;`
- 读作 "newName is a reference to / an alias of existingName"。**两个名字同一个格子**。

```cpp
int number = 88;
int &refNumber = number;
cout << number << " " << refNumber;   // 88 88
refNumber = 99;
cout << refNumber << " " << number;   // 99 99
number = 55;
cout << number << " " << refNumber;   // 55 55
```

| | Pointer | Reference |
|---|---|---|
| 存什么 | 地址（自己是独立变数） | 不存东西，就是别名 |
| 宣告 | `int *p = &x;` | `int &r = x;` |
| 使用 | 要 `*p` 才拿到值 | 直接用 `r` |
| 可改指向 | 可以（`p = &y;`） | **不行**，永远绑定同一个 |
| 可为 NULL | 可以 | 不行，宣告时必须初始化 |

→ Ch5 的 **pass by reference**（`int &x`）与 **pass by address**（`int *x`）就是这两种。

---

## 8. Slide 与 Tutorial 练习答案（全部 g++ 验证）

**Slide p25**
```cpp
double *ptrNum;          // a)
double num;              // b)
ptrNum = &num;
*ptrNum = 123;           // c) num = 123 through the pointer
```

**Slide p26**（a 在 0x32AB，b 在 0xE1F2）
```cpp
int *ptr1, *ptr2, *ptr3, a = 10, b = 20;
ptr2 = ptr3 = &a;  ptr1 = &b;
cout << *ptr2 + b;       // 10 + 20 = 30
cout << *&*ptr2;         // *& 互相抵消 → *ptr2 = 10
ptr3 = &b;
a += *ptr3 + *ptr1;      // a = 10 + (20 + 20) = 50
ptr1 = ptr2;             // ptr1 → a
cout << a;               // 50
cout << *ptr3 + *ptr1;   // b + a = 20 + 50 = 70
cout << ptr1;            // 0x32AB (address of a)
cout << ptr3;            // 0xE1F2 (address of b)
```
Output：`30` `10` `50` `70` `0x32AB` `0xE1F2`

**Slide p27**
```cpp
aPtr=&a; bPtr=&b; *aPtr=12; b=13;       // a=12, b=13
c = a + *bPtr;                          // c = 25
aPtr = &c;  a = *aPtr - 2;              // a = 25 - 2 = 23
(*bPtr)++;                              // b = 14
```
Output：`a = 12` `b = 13` `c = 25` `a = 23` `b = 14`

**Tutorial 4**

| Q | 答案 |
|---|---|
| 1 | Pointer = variable that stores the address of another variable；宣告 `type *name;` |
| 2a | `int* int_ptr1, int_ptr2;` 看起来两个都是 pointer，其实**只有 int_ptr1 是**，int_ptr2 是普通 int |
| 2b | `int *p;` `int * p;` `int*p;`（和 `int* p;` 共四种） |
| 3a | `int *pX = x;` 把 int 值给 pointer（要 `&x`）；且 `px` 与 `pX` 大小写不同（未宣告）；印 pointer 要 `*pX` 才是 30 |
| 3b | `int *pX = &x;` 但 x 是 double → 类型不符 |
| 5 | `10 20` / `20 20` / `30 30`（见 §5） |
| 6 | (a) 3 (b) 1 (c) 8 |
| 7 | i. `*p * a` = 5×5 = **25** ii. `(*p) * *r` = 5×5 = **25**（r 也指 a） iii. `*p + *q` = 5+7 = **12** iv. `*&*q` = **7** |
| 9 | `2` `4` `48`（2×4×6）`35`（`--*p`=1，`++*q`=5，`++*r`=7 → 1×5×7） |
| 10 | `v = 5` `n = 6` `r = 11` `v = 6` `n = 12`（注意 `*nptr` 大小写 typo，应为 `*nPtr`） |
| 11 | `12ff7c` `100` `12ff78` `200` `12ff7c` `100` |
| 12 | `p1 = 15` `p2 = 47` `ptr1 = 8` `ptr2 = 47` `v = 8`（⚠️ 见下） |
| 14 | `x = 24` `y = 49` `z = -25` `x = -35` `y = 50` |

**Q8 记忆体图**（`result` 在 65fdf4，pointer `a` 在 994f33，a → result）
```cpp
int result = 10;
int *a = &result;
```
| 语句 | 输出 |
|---|---|
| `cout << a;` | 65fdf4（a 存的地址） |
| `cout << *a;` | 10 |
| `cout << &a;` | 994f33（a 自己的地址） |
| `cout << result;` | 10 |
| `cout << *result;` | **compile error**（result 不是 pointer） |
| `cout << &result;` | 65fdf4 |
| `cout << *&a;` | 65fdf4（`*&` 抵消 = a） |
| `cout << &*a;` | 65fdf4（`&*` 抵消 = a） |

**Q12 ⚠️**：`p2 = 3 * (++v + *ptr1);`——同一个式子里既改 `v`（`++v`）又透过 `*ptr1` 读 `v`，C++ 标准规定这是 **undefined behaviour**（没有规定哪个先做）。g++ 算成 3×(8+8)=48，再 `p2--` → 47；讲师预期的答案应该也是 47，但知道这种写法在真实程式里不可靠。

**Q13 修正版**（不改数值）：
```cpp
double *length = new double;       // 原码：没给 length 配置就用 *length
double *width  = new double;       // 原码：width 从未配置
cout << fixed << showpoint << setprecision(2);
*length = 6.5;                     // 原码写 length = 6.5（把 double 给 pointer）
*width  = 3.0;                     // 原码写 &width = 3.0（不能给地址赋值）
cout << "Area: " << (*length) * (*width) << ", ";
cout << "Perimeter: " << 2 * (*length + *width) << endl;
```
Output：`Area: 19.50, Perimeter: 19.00`

---

## Closing the loop

寄物柜的纸条：`&` 是查柜号，pointer 是写着柜号的纸条，`*` 是照纸条去开柜。追踪题只要画表、每次 `p = &x` 就更新"指向谁"，就不会错。

下一章把这个用在 function 上：function 拿到的是**副本**（pass by value）、**别名**（pass by reference）还是**纸条**（pass by address）？

---

## ⚠️ Where the slides mislead

| Slide | Slide 写 | 更准确 |
|---|---|---|
| p17–19 | long 占 4 bytes，`++mylong` +4 | Windows 是 4；64-bit Linux/macOS 是 8。原则是"加 sizeof(类型)" |
| p11 | `p = NULL;` | 可用；现代 C++ 建议 `nullptr` |
| p13 | `int value1 = 5, value2 = 15， *p1` | 全形逗号（PowerPoint 打字），真 code 要半形 `,` |
| T4 Q12 | `3 * (++v + *ptr1)` | 同一式子修改并读取 v → undefined behaviour；预期答案 47 |
| T4 Q9/Q12 | `void main()` | 标准 C++ 用 `int main()` |

---

## Term table

| English | 中文 | 一句话说明 |
|---|---|---|
| Address | 地址 | 变数在记忆体的位置 |
| Address operator `&` | 取址运算子 | `&x` = x 的地址 |
| Pointer | 指标 | 存地址的变数 |
| Dereference / indirection operator `*` | 解参考 / 间接运算子 | 去地址拿/放值 |
| Null pointer | 空指标 | 不指向任何东西（0 / NULL / nullptr） |
| Dynamic variable | 动态变数 | `new` 建立的无名变数 |
| Memory leak | 记忆体泄漏 | 没 delete 又失去 pointer |
| Pointer arithmetic | 指标运算 | +1 = 下一个元素 |
| Reference variable | 参考变数 | 别名，`int &r = x;` |

---

## Cheat sheet

- `int *p;` 宣告；`p = &x;` 指向；`*p` 取/改值；`&p` 是 pointer 自己的地址。
- `int *p, q;` → 只有 p 是 pointer。
- `*p2 = *p1;` 复制值；`p1 = p2;` 复制地址（之后同指一处）。
- `*&x` = x；`&*p` = p。
- **Trace 表**：变数一栏、pointer 写"→谁"；`p = &y` 时立刻更新。
- `cout << p;` 印 p **所存的地址**（即它指向的变数地址）。
- p+1 移 sizeof(type) bytes；p−q = 相隔几个元素。
- `*p++` 移指标取旧值；`*++p` 移后取值；`++*p` / `(*p)++` 改值（前/后）。
- Reference：`int &r = x;` 别名、不能改绑、必须初始化。

---

## Practice (answers included)

### A. MCQ
1. `int a = 3, b = 9; int *p = &a; p = &b; *p = 1;` Now a, b =  (a) 1, 9 (b) 3, 1 (c) 1, 1 (d) 3, 9
2. `int *x, y;` declares  (a) two pointers (b) pointer x and int y (c) two ints (d) error
3. `int arr[4]={5,6,7,8}; int *p = arr; p += 2; cout << *p;`  (a) 5 (b) 6 (c) 7 (d) address
4. Which cannot be changed to refer to another variable after initialisation?  (a) pointer (b) reference (c) both (d) neither
5. If an `int` pointer holds address 1000, `p + 3` holds (int = 4 bytes)  (a) 1003 (b) 1012 (c) 1004 (d) 3000

**Answers:** 1-b, 2-b, 3-c, 4-b, 5-b

### B. Trace
```cpp
int x = 5, y = 8, *p = &x, *q = &y;
*p = *q + 2;          // ?
q = p;                // ?
*q = *q * 2;          // ?
cout << x << " " << y << " " << *p << " " << *q;
```
**Answer:** `*p = 8 + 2` → x = 10; `q = p` → q → x; `*q = 10 × 2` → x = 20. Output: `20 8 20 20`.

### C. Application
**C1.** Write statements to swap the values of `a` and `b` using only pointers `pa` and `pb`.
```cpp
int *pa = &a, *pb = &b, temp;
temp = *pa;  *pa = *pb;  *pb = temp;
```
**C2.** Create a dynamic double, store 3.5 in it, print it and release it.
```cpp
double *d = new double;  *d = 3.5;  cout << *d;  delete d;
```

### D. Thinking
**D1.** Why is `cout << &m` different from `cout << m` for a pointer m? — `m` prints the address stored in m (the variable it points to); `&m` prints the address where m itself is stored.
**D2.** Give one danger of an uninitialised pointer. — It holds a random address; dereferencing it may overwrite unrelated memory or crash the program. Initialise to `nullptr` until it points to a valid variable.

---

## Slide index
| Note section | Slides |
|---|---|
| 1 Address & pointer | p3–6 |
| 2 Assign & dereference | p7–10 |
| 3 NULL & examples | p11–13 |
| 4 Trace method | (p25–27 + past papers) |
| 5 new | p14–15 |
| 6 Arithmetic | p16–22 |
| 7 Reference | p23–24 |
| 8 Exercises | p25–27, Tutorial 4 |

## Links to other chapters
- Vector iterator `*it` → **Ch1**
- Pass by reference (`&`) / pass by address (`*`) → **Ch5**
- Array name as pointer; pointer to struct `s->cgpa` → **Ch6**
- Pointer variables' storage class & duration (dynamic) → **Ch6**
