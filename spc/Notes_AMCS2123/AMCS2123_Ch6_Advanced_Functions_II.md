# AMCS2123 — Chapter 6: Advanced Functions II (Passing Arrays & Structures, Storage Classes)

> 这一章在考卷上以两种样子出现：**写 function 处理 array / struct**（Oct 2025 Q4c–d 共 17 分、Jan 2026 Q4c 8 分）和**辨认 storage class**（Jan 2026 Q3d 2 分）。Slide 最大的问题是几个关键例子**本身不能 compile**：p14 `num[1][2]` 超出 `[3][2]` 的范围、p15 `int x[][]` 两个维度都空（第二维一定要写）、p20 `cout << cgpa` 用了不存在的变数、p35 static counter 的注释写 "1, 2" 但实际输出是 **0, 1**。另外"为什么传整个 array 会改到原本的资料"slide 只写了一句 "array name is actually a pointer"。这份笔记全部 g++ 跑过并改正。

---

## 0. 一句话总览

**传 array 元素 = pass by value；传整个 array = 传它的地址（function 改就改到原本）；传 struct 可以传成员、整个 struct（by value 副本），或 by reference / address（改到原本，pointer 用 `->`），struct 也可以被 return。Storage class（auto, static, register, extern）决定变数的 scope（在哪里看得到）、duration（活多久）和 location（放在哪里）。**

| Part | 内容 | Slide |
|---|---|---|
| 1 | 传 array：individual element（by value） | p4–7 |
| 2 | 传 whole array（by reference/address）；strings；2D array | p8–15 |
| 3 | 传 structure：成员 / 整个 by value / by reference & address；return structure | p16–25 |
| 4 | Storage classes：visibility (scope)、duration、location | p26–30 |
| 5 | auto, static, register, extern | p31–39 |
| 6 | Duration：static, local, dynamic；题目 | p40–43 |

🎯 **这章回答的考题**
- **Oct 2025 Q4c(i)** `checkCount(score[], selected)` 数有几个分数比 selected 低并显示 (9)；**(ii)** 呼叫：最后一位学生的分数 (2)
- **Oct 2025 Q4d(i)** `updateSalary` 加薪 5%，呼叫端要拿到新薪水 (4)；**(ii)** 呼叫 (2)
- **Jan 2026 Q4c(i)** `checkResult(mark, string *result)` (6)；**(ii)** 用 `stud` 的资料呼叫 (2)
- **Jan 2026 Q3d** 辨认 storage class：(i) function 结束也不销毁、保留值 → static (1)；(ii) 定义在所有 function 外 → extern/global (1)

**Prerequisites**：Ch1 array、Ch3 struct、Ch4 pointer（`->`）、Ch5 三种传递方式。

---

## Scene：班级成绩系统

老师有一个 `int score[20]`，想写一个 function 算"有几个人比某个分数低"；又有一个 `Employee` 记录，年底要加薪 5%，而且加完之后 main 要看到新的薪水。

- 传 array 进 function：function 里改了会不会影响 main？
- 传 struct 进 function：怎样才能让 main 拿到"加薪后"的值？
- 另一个问题：function 里的变数什么时候诞生、什么时候消失？为什么有的变数能"记得"上一次呼叫的值？

---

## 1. 传 array 的一个元素（p4–7）

**一个元素就是普通变数** → **pass by value**（副本）。
```cpp
void initialize(int x) { … }
initialize(list[5]);          // 把第 6 个元素的"值"传进去
```
**p6 例子**：逐个元素比较找最大
```cpp
int findLargest(int x, int largest) { return (x > largest) ? x : largest; }
for (largest = list[0], i = 1; i < SIZE; i++)
    largest = findLargest(list[i], largest);   // 68
```
（`SIZE` 是 15 但只初始化 11 个值；其余自动是 0，不影响最大值 68。）

**p7 练习：Celsius → Fahrenheit**
```cpp
float toFahrenheit(float c) { return c * 9.0 / 5.0 + 32; }
int main() {
    float celsius[5] = {0, 25, 30, 37, 100};
    for (int i = 0; i < 5; i++)
        cout << celsius[i] << "C = " << toFahrenheit(celsius[i]) << "F\n";
}
```

---

## 2. 传整个 array（p8–15）⭐

- **参数宣告用 `[]`，不用 `&`**；size 通常省略（写了 compiler 也忽略），所以**另外传 size**：
```cpp
void initialize(int list[], int size) { … }
initialize(list, SIZE);        // 呼叫：只写 array 名字，不加 []
```
- **Returning an array is NOT allowed**（function 不能 `return` 整个 array）。

**为什么 function 改了 array，main 的 array 也变？（p10–11）**
```cpp
void multiply2(int x[], int size) {
    for (int i = 0; i < size; i++) x[i] *= 2;
}
int base[5] = {3, 7, 1, 5, 8};
multiply2(base, 5);            // base 变成 6 14 2 10 16
```
```
 main:  base ──> [ 3 | 7 | 1 | 5 | 8 ]   ← 真正的资料只有这一份
                   ^
 multiply2:  x ────┘   x 收到的是"第一格的地址"，不是副本
```
**原因：array 名字其实就是第一个元素的地址（pointer）**。所以传 array = 传地址 → function 透过地址直接改原本的资料（效果等同 pass by reference/address）。
> 💬 **答题句 (EN)** — *When a whole array is passed to a function, only the array name (the address of its first element) is passed, not a copy; therefore any change made to the array elements inside the function changes the original array.*

### ✅ 满分答法 — Oct 2025 Q4c(i): checkCount (9 marks) — g++ 验证
```cpp
void checkCount(int score[], int selected)          // (2) array param + selected
{
    int count = 0;                                  // (1) initialise counter
    for (int i = 0; i < 20; i++)                    // (2) loop 20 students
        if (score[i] < selected)                    // (2) compare
            count++;
    cout << "The selected score " << selected       // (2) display message
         << " is higher than the scores of " << count
         << " students in the class." << endl;
}
```
### ✅ (ii) Function call (2 marks)
```cpp
checkCount(score, score[19]);       // last (20th) student → index 19
```

### ✅ Slide p12 练习：Civics 及格人数
```cpp
int countPass(int marks[], int size) {
    int count = 0;
    for (int i = 0; i < size; i++) if (marks[i] >= 50) count++;
    return count;
}
int main() {
    int mark[25] = { /* 25 marks */ };
    cout << "Passed Civics: " << countPass(mark, 25) << endl;
}
```

### 2.1 传字串（char array）（p13）
```cpp
void fun1(char x[]) { cout << x; }
void fun2(char y[]) { strcpy(y, "love"); cout << y; }
char str[5] = "hate";
fun1(str);  fun2(str);  cout << str;      // hate love love
```
fun2 改了 y 就是改了 str（原因同上）。

### 2.2 传 2D array（p14–15）
- **一个元素**（by value）：`display(num[1][1]);` → 44
- **整个 2D array**：**第一维可空，第二维（column 数）一定要写**：
```cpp
void display(int x[][2], int rows) {
    for (int r = 0; r < rows; r++)
        for (int c = 0; c < 2; c++)
            cout << x[r][c] << " ";
}
int num[3][2] = {{11,22},{33,44},{55,66}};
display(num, 3);                         // 11 22 33 44 55 66
```
⚠️ Slide p14 写 `display(num[1][2])`：`num` 只有 2 个 column（index 0–1），`[2]` **越界**。Slide p15 写 `void display(int x[][])`：**compile error**，因为 compiler 必须知道每行有几个元素才能计算位置。

### Tutorial 6 Q1–Q3（g++ 验证）
```cpp
// Q1
char toUppercase(char c) { return (c >= 'a' && c <= 'z') ? c - 32 : c; }   // 或 toupper(c)
char word[50]; cin >> word;
for (int i = 0; word[i] != '\0'; i++) word[i] = toUppercase(word[i]);

// Q2
void inputSales(double sales[], int size) {
    for (int i = 0; i < size; i++) { cout << "Month " << i+1 << ": "; cin >> sales[i]; }
}
double calculateAvgSales(double sales[], int size) {
    double total = 0;
    for (int i = 0; i < size; i++) total += sales[i];
    return total / size;
}
// main: double sales[12]; inputSales(sales, 12); cout << calculateAvgSales(sales, 12);

// Q3
bool checkEven(int n) { return n % 2 == 0; }
vector<int> numbers = {13, 18, 24, 35};
cout << "Numbers\tEven\n-------\t----\n";
for (int n : numbers) cout << n << "\t" << (checkEven(n) ? "Yes" : "No") << endl;
```

---

## 3. 传 structure（p16–25）⭐

| 方式 | Parameter | 呼叫 | 改到原本？ | 存取成员 |
|---|---|---|---|---|
| **个别成员** (by value) | `void update(float cgpa)` | `update(stud.cgpa);` | ❌ | `cgpa` |
| **整个 struct by value** | `void update(Student s)` | `update(stud);` | ❌（副本） | `s.cgpa` |
| **整个 struct by reference** | `void update(Student &s)` | `update(stud);` | ✅ | `s.cgpa` |
| **整个 struct by address** | `void update(Student *s)` | `update(&stud);` | ✅ | `s->cgpa` 或 `(*s).cgpa` |
| **成员 by address** | `void f(float *c)` | `f(&stud.cgpa);` | ✅ | `*c` |

- **Returning a structure is allowed**（与 array 不同）。
- **`->`（arrow operator）**：pointer 指向 struct 时存取成员；`s->cgpa` 等于 `(*s).cgpa`（括号必要，因为 `.` 比 `*` 先做）。

**p17 / p18 / p20 的输出**（`stud.cgpa` 起始 3.5，function 设为 4.0 并印出）：

| 方式 | ① function 内印 | ② 回到 main 印 `stud.cgpa` |
|---|---|---|
| 成员 by value `update(stud.cgpa)` | 4.0 | **3.5** |
| 整个 by value `update(stud)` | 4.0 | **3.5** |
| 整个 by address `update(&stud)` | 4.0 | **4.0** |

填空答案：p17 (A) = `float cgpa`；p18 (A) = `Student`；p20 (A) = `Student *`。
⚠️ p20 第一个版本写 `(*s).cgpa = 4.0; cout << cgpa;`——`cgpa` 不存在于该 function，**compile error**；要写 `cout << (*s).cgpa;` 或 `s->cgpa`。

**p21 填空（pass by reference）**：
```cpp
void askRecord(Student &s) {       // (A) Student
    cin >> s.name;                 // (B) s.name
    cin >> s.cgpa;                 // (C) s.cgpa
}
askRecord(stud);                   // (D) stud
cout << stud.name << stud.cgpa;    // slide 多了一个 ')'
```

### ✅ 满分答法 — Oct 2025 Q4d: updateSalary (4 + 2 marks) — g++ 验证
"The calling function must be able to access the updated salary" → 必须改到原本 → **by reference**（或 by address）。
```cpp
// (i) pass by reference
void updateSalary(Employee &emp)          // (2) Employee& param
{
    emp.salary = emp.salary * 1.05;       // (2) 5% increment
}
// (ii) call
updateSalary(emp);
```
等价的 by address 版本：
```cpp
void updateSalary(Employee *emp) { emp->salary *= 1.05; }
updateSalary(&emp);
```
（3000 → 3150。若写成 `void updateSalary(Employee emp)`（by value），main 看不到新薪水——扣分。）

### ✅ 满分答法 — Jan 2026 Q4c (6 + 2 marks)
```cpp
struct Student { string name, id; int pspMark; };
Student stud = {"Henry Ford", "25WMR12345", 86};
string result;

// (i) receives an int mark; returns the result through pass by address
void checkResult(int mark, string *result)      // (2) int + string* params
{
    if (mark >= 50)                             // (2) condition >= 50
        *result = "PASS";                       // (2) dereference to assign
    else
        *result = "FAIL";
}

// (ii) function call using stud (2 marks)
checkResult(stud.pspMark, &result);             // pass the member by value, result by address
cout << result;                                 // PASS
```

### 3.1 Return a structure（p24–25）
```cpp
Student askRecord() {
    Student stud;
    cin >> stud.name >> stud.cgpa;
    return stud;                     // structure can be returned
}
Student stud2 = askRecord();
```
**p25 练习：Time**
```cpp
struct Time { int hour, minute; };
Time getTime() {
    Time t;
    cout << "Hour and minute: "; cin >> t.hour >> t.minute;
    return t;
}
int main() { Time now = getTime(); cout << now.hour << ":" << now.minute; }
```

### Slide p22–23 练习：Car（g++ 验证）
```cpp
struct Car { string model; int km; double fuel; };     // slide 把 fuel 打成 "duel"
void serviceCar(Car &c) { c.km = 0; }                   // by reference
void refuel(Car *c, double litres) { c->fuel += litres; }  // by address
int main() {
    Car car = {"X70", 56000, 18.5};
    serviceCar(car);
    refuel(&car, 20);
    cout << car.model << ", " << car.km << " km, " << car.fuel << " L";   // X70, 0 km, 38.5 L
}
```

### Tutorial 6 Q4–Q5
```cpp
// Q4: pass only the temperature member (by value)
struct Patient { string name; int age; double temp; };
void checkFever(double temp) {
    if (temp > 37.5) cout << "Fever detected!";
    else             cout << "Temperature is normal";
}
Patient p; cin >> p.temp; checkFever(p.temp);

// Q5: struct by reference + value parameter
struct Package { double baseWeight, totalWeight; };
void computeTotalWeight(Package &p, double packagingWeight) {
    p.totalWeight = p.baseWeight + packagingWeight;
}
```

---

## 4. Storage classes（p26–30）

**Storage class** 定义变数（与 function）的：

| 特性 | 意思 |
|---|---|
| **Visibility (scope)** | 程式**哪些地方能存取**这个变数 |
| **Duration (lifespan)** | 变数的记忆体**存在多久** |
| **Location** | 执行时放在记忆体**哪里**（stack、static area、CPU register） |

语法：`storage_class data_type variable_name;` 例：`static int x;`

### 4.1 Scope（p28–30）

**变数的 scope：从宣告那一行开始，到它所在的 block 结束为止。**

```
 #include <stdio.h>                                    ┐
 void fun(int a, int b);                               │ Global area
 int main() {                                ┐         │
     int a = 0, b, c;                        │ main's  │
     if (a > 0)                              │ area    │
     {   float x, y;          ┐ nested block │         │
         x = a * 2;           ┘ area (x, y)  │         │
     }                                       ┘         │
 }                                                     │
 void fun(int a, int b) {     ┐ fun's area             │
     cout << a << b;          │ (these a, b)           │
 }                            ┘                        ┘
```

**p29 例子**

| 变数 | scope |
|---|---|
| `a`（line 1，global） | 1–14 |
| `b`（line 3，参数） | 3–7 |
| `a`（line 5，local） | 5–7（在这段**遮住**了 global a） |
| `d`（line 9，global） | 9–14 |
| `e`（line 12） | 12–14 |

**p30 练习答案**：x(line 1) 1–12；x(line 2 参数) 2–6；a(line 4) 4–6；a(line 7 global) 7–12；x(line 10) 10–12。

---

## 5. 四种 storage class（p31–39）⭐

| Storage class | 适用 | Scope | Duration | Location | 重点 |
|---|---|---|---|---|---|
| **auto** | local 变数与参数的**预设** | 所在 block | function 被呼叫时建立，**返回时销毁** | runtime **stack** | `int a;` = `auto int a;` |
| **static** | local（或 global）变数 | 所在 block（local static） | **整个程式执行期间**；只初始化**一次** | static memory | **保留上次呼叫的值** |
| **register** | local 变数 | 所在 block | 同 auto | 尝试放 **CPU register** | 加快速度，但 compiler 通常忽略 |
| **extern** | 定义在所有 function **外面**的变数（global） | 从宣告处到档案结束；其他档案用 `extern` 宣告即可用 | 整个程式 | static memory | `extern int b;` 是**宣告**，**不配置记忆体** |

> 💬 **答题句 (EN)** — *A static local variable is allocated and initialised only once and is not destroyed when its function ends, so it retains its value between function calls. An extern (global) variable is defined outside all functions and is available to every function from its point of declaration; a declaration with the keyword extern does not allocate memory.*

### ✅ 满分答法 — Jan 2026 Q3d (1 + 1 mark)
- (i) Not destroyed when the function terminates and retains values between calls → **static**
- (ii) Defined outside any function and available to any function from the point of declaration → **extern** (global variable)

### 5.1 static 的例子（p35）⚠️
```cpp
int counter() {
    static int count = 0;
    return count++;          // postfix：先回传，再 +1
}
cout << counter();           // 0
cout << counter();           // 1
```
⚠️ **Slide 的注释写 `// 1` 和 `// 2`，是错的**。`count++` 是**后置**递增：第一次回传 0（然后 count 变 1），第二次回传 1。g++ 输出 `01`。若要印 1、2 应写 `return ++count;`。重点不变：第二次呼叫时 count 没有被重新设成 0——这就是 static。

### 5.2 extern 的例子（p38）
```cpp
int b = 10;                         // global
int main() { cout << b; display(); }
void display() { int b = 5; cout << b; }          // local b 遮住 global → 输出 10 5

void display() { extern int b; cout << b; }       // 明确使用 global b → 输出 10 10
```

### ✅ Slide p39 练习（g++ 验证）
```cpp
void f1(int); void f2(int);
int A = 20;
int main() { int B; for (B = 0; B < 3; B++) { f1(B); f2(B); } }
void f1(int E) { int C = 2; C += E + A; cout << C << "  "; }
void f2(int F) { static int D = 2; D += F + A; cout << D << "   "; }
```
**(a) Storage class**：A = **extern**（global）；B = **auto**；C = **auto**；D = **static**；E = **auto**（参数）。
**(b) Trace**

| B | f1: C = 2 + B + 20 | f2: D = D + B + 20 |
|---|---|---|
| 0 | 22 | 2 + 0 + 20 = **22** |
| 1 | 23 | 22 + 1 + 20 = **43** |
| 2 | 24 | 43 + 2 + 20 = **65** |

Output：`22  22   23  43   24  65`（C 每次重新从 2 开始；D 累积）。

---

## 6. Duration（p40–43）

| Duration | 谁 | 存在期间 |
|---|---|---|
| **Static duration** | global 变数、static 变数 | 程式开始 → 程式结束 |
| **Local (automatic) duration** | auto 变数 | function 开始 → function 结束 |
| **Dynamic duration** | `new` 建立的变数（Ch4） | 执行时用特殊函式建立与销毁 |

p41：`num`（global）在 `display()` 前后都是 20；`a` 只在 `display()` 执行时存在。

**p42：哪个可能不配置记忆体给 p？** → **(e) `extern int p;`**（只是宣告，记忆体在别处定义）；**(d) `register int p;`** 也可能放在 CPU register 而不是记忆体。
**p43：`fun()` 结束时哪些被释放？** → **a、b、d**（auto / 预设 auto / register）；c（static）与 e（extern）不会。

### Tutorial 6 Q6–Q9（g++ 验证）

**Q6**（`int x = 3;` global；fun1 有 `static int w = 2`）
(a)

| 变数 | Visibility | Storage class |
|---|---|---|
| x at line 6 | Global：从 line 6 到程式结束，所有之后的 function 可用 | extern |
| z at line 22 | 只在 fun2（line 22–24） | auto |

(b) Output：fun1(1..4)：w = 2, 4, 12, 48 → 印 `2` `4` `12` `48`；fun2 回传 1 + 3 = 4 → 整行 `2412484`。

**Q7**

| 变数 | Line | Storage class | Scope |
|---|---|---|---|
| x | 3 | extern (global) | 3–26（在 f1 的 16–18 被 local x 遮住） |
| c | 7 | auto | 7–10 |
| y | 12 | static (global, file scope) | 12–26 |
| x | 16 | auto | 16–18 |
| d | 20 | extern (global) | 20–26（main 在 line 8 以 `extern char d;` 宣告后可用） |
| w | 22 | auto（参数） | 22–26 |
| v | 24 | static | 24–26 |

**Q8**

| 变数 | Storage class | Scope |
|---|---|---|
| K (line 2) | extern (global) | 2–20 |
| G (line 10) | auto | 10–14 |
| C (line 11) | static | 11–14 |
| B (line 12) | extern（宣告，指向 line 15 的 global B） | 12–14 |
| A (line 16) | auto（参数） | 16–20 |

部分被遮住的变数：**B，宣告在 line 15**（global），在 `funct2(int A, int B)`（line 16–20）里被参数 B 遮住。

**Q9**（`static int pr = 1`）

| count | pr = pr × count | c | d = (c > 10) ? b : a + c |
|---|---|---|---|
| 1 | 1 | 1 | 10 + 1 = **11** |
| 2 | 2 | 2 | **12** |
| 3 | 6 | 6 | **16** |
| 4 | 24 | 24 | b = **20** |

Output：`11121620`（没有空格，因为程式没印分隔符）。（注意原码 `int a = 10; b = 20;` 少了 `int`，b 未宣告；应为 `int a = 10, b = 20;`。）

---

## Closing the loop

班级成绩：`checkCount(score, score[19])`——传整个 array 只传地址，function 能读能改；Employee 加薪要让 main 看到 → by reference 或 by address。function 里的 `count` 每次重来（auto），除非宣告成 static。

下一章让 function 更"通用"：同一个 function 可以处理 int、double、string——**Template**。

---

## ⚠️ Where the slides mislead

| Slide | Slide 写 | 更准确 |
|---|---|---|
| p14 | `display(num[1][2])`，`num[3][2]` | column index 最大 1 → 越界；应如 `num[1][1]` |
| p15 | `void display(int x[][])` | compile error；第二维必须写：`int x[][2]` |
| p20 | `(*s).cgpa = 4.0; cout << cgpa;` | `cgpa` 未宣告；用 `(*s).cgpa` / `s->cgpa` |
| p21 | `cout << stud.name << stud.cgpa);` | 多一个 `)` |
| p22 | `duel (a double)` | typo：fuel |
| p35 | `counter()` 输出注释 1, 2 | 实际 **0, 1**（`count++` 后置） |
| p28 | `#include <stdio.h>` 配 `cout` | 要 `<iostream>` |
| T6 Q9 | `int a = 10; b = 20;` | b 没宣告；应 `int a = 10, b = 20;` |

---

## Term table

| English | 中文 | 一句话说明 |
|---|---|---|
| Individual element | 个别元素 | pass by value |
| Whole array | 整个阵列 | 传地址，改到原本 |
| Arrow operator `->` | 箭头运算子 | pointer 存取 struct 成员 |
| Return a structure | 回传结构 | 可以；array 不行 |
| Storage class | 储存类别 | scope + duration + location |
| Scope / visibility | 作用范围 | 从宣告到 block 结束 |
| Duration / lifespan | 生命周期 | static / local / dynamic |
| auto | 自动 | local 预设，stack |
| static | 静态 | 保留值，只初始化一次 |
| register | 暂存器 | 建议放 CPU register |
| extern | 外部 | global；宣告不配置记忆体 |
| Shadowing (hidden) | 遮蔽 | 内层同名变数盖住外层 |

---

## Cheat sheet

- 元素：`f(arr[i])` by value；整个：`f(arr, size)`，参数 `type arr[]`，**改到原本**，不能 return array。
- 2D 参数：`type x[][COLS]`。
- Struct：成员 `f(s.m)`；by value `f(s)`；by ref `f(Type &s)` 呼叫 `f(s)`；by address `f(Type *s)` 呼叫 `f(&s)`，内部 `s->m`。
- 呼叫端要看到修改 → reference 或 address。
- Return struct ✅；return array ❌。
- **auto** = local 预设、结束即毁；**static** = 保留值；**register** = CPU；**extern** = global、宣告不配置。
- Scope：宣告那行 → block 结束；内层同名会遮住外层。
- static 局部变数：初始化只做一次。

---

## Practice (answers included)

### A. MCQ
1. `void f(int a[]) { a[0] = 9; } int x[3] = {1,2,3}; f(x);` x[0] is  (a) 1 (b) 9 (c) error (d) 0
2. Which declaration is valid for passing `int m[4][5]`?  (a) `void g(int m[][])` (b) `void g(int m[][5])` (c) `void g(int m[4][])` (d) `void g(int **m[][])`
3. `void h(Emp e) { e.pay = 0; }` — after `h(emp);` emp.pay is  (a) 0 (b) unchanged (c) error (d) random
4. Which storage class keeps its value between function calls?  (a) auto (b) register (c) static (d) extern declaration
5. `s->name` is the same as  (a) `*s.name` (b) `(*s).name` (c) `&s.name` (d) `s.name*`

**Answers:** 1-b, 2-b, 3-b, 4-c, 5-b

### B. Trace
```cpp
int total = 0;
void add(int n) { static int calls = 0; calls++; total += n * calls; }
int main() { add(2); add(3); add(4); cout << total; }
```
**Answer:** calls = 1, 2, 3 → total = 2×1 + 3×2 + 4×3 = 2 + 6 + 12 = **20**.

### C. Application
**C1.** Write `double findAverage(double arr[], int size)` and call it for `double temps[7]`. → loop sum / size; `cout << findAverage(temps, 7);`
**C2.** Write `void raisePrice(Product *p, double percent)` that increases `p->price` by percent%, and call it for `Product item`.
```cpp
void raisePrice(Product *p, double percent) { p->price *= (1 + percent / 100); }
raisePrice(&item, 10);
```
**C3.** Write `Book createBook()` that reads title and price from the user and returns a `Book`.
```cpp
Book createBook() { Book b; getline(cin, b.title); cin >> b.price; return b; }
```

### D. Thinking
**D1.** Why can a function modify an array passed "by value"-looking syntax `f(arr)`? — Because the array name is the address of the first element; the function receives that address, not a copy.
**D2.** Why pass a large struct by reference even when the function should not modify it? — To avoid copying the whole struct (efficiency); add `const` (`const Student &s`) to protect it (extra).

---

## Slide index
| Note section | Slides |
|---|---|
| 1 Individual element | p4–7 |
| 2 Whole array, strings, 2D | p8–15 |
| 3 Structures | p16–25 |
| 4 Storage class & scope | p26–30 |
| 5 auto/static/register/extern | p31–39 |
| 6 Duration & questions | p40–43 |

## Links to other chapters
- Arrays, 2D arrays → **Ch1**；structures → **Ch3**
- `->`, pointers, `new` (dynamic duration) → **Ch4**
- Pass by value / reference / address → **Ch5**
- Template function over arrays/structs (Tutorial 7 Q2, Q6) → **Ch7**
- Global variables → common coupling → **Ch11**
