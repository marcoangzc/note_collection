# AMCS2123 — Chapter 5: Advanced Functions I (Pass by Value, Reference, Address)

> 每份考卷都有一题 **8 分的 function 宣告题**（"一个 bool function，receives num，两个参数分别用 pass by reference 和 pass by address"），加上 **9 分写 main 去呼叫它**。Slide 把三种传递方式分三段讲，每段一个 `fun(a)` 例子，但**从来没有把三种放在同一个 function 里**——而考题正好就是这样。Slide 也没讲清楚 "reference parameter vs return statement" 的差别（Oct 2025 Q3a）。这份笔记用一张"三种传递方式对照表"＋一个"写 function 的四步模板"，把两份卷的题和 Tutorial 5 全部写成可执行的程式（g++ 验证）。

---

## 0. 一句话总览

**Function 让程式模组化；资料可以用 pass by value（给副本，原值安全）、pass by reference（给别名 `&`，function 改就是改原本的）、pass by address（给地址 `*`，function 用 `*` 去改原本的）传进去；只能 return 一个值，所以要"回传多个结果"就用 reference 或 address 参数。**

| Part | 内容 | Slide |
|---|---|---|
| 1 | Why function：modular programming、benefits、structure chart | p2–4 |
| 2 | Void function vs value-returning function | p5 |
| 3 | Global variable 的问题；为什么用参数 | p6–8 |
| 4 | **Pass by value** | p9–11 |
| 5 | **Pass by reference** | p12–15 |
| 6 | **Pass by address** | p16–19 |
| 7 | 用 reference / address 交换两个变数 | p20–22 |
| 8 | Flowchart & pseudocode for functions | p23–24 |

🎯 **这章回答的考题**
- **Oct 2025 Q3a** Reference parameter vs return statement 回传结果的主要差别 (2)
- **Oct 2025 Q3b(i)** `bool analyseNum(int num, int &square, int *cube)` (8)
- **Oct 2025 Q3b(ii)** main：输入整数、呼叫、显示 square、cube、even (9)
- **Jan 2026 Q4a** `bool processNumber(int num, int &doubleVal, int *tripleVal)` (8)
- **Jan 2026 Q3g** TWO benefits of modular programming (2)
- **Jan 2026 Q4c** `checkResult(int mark, string *result)`（pass by address）→ 本章写法 + **Ch6**（传 struct 成员）

**Prerequisites**：Ch4 的 `&`（取地址 / reference）与 `*`（pointer / dereference）。

---

## Scene：电子钱包 top-up

你在写一个电子钱包 app。`main` 里有 `balance = 3.50`。你写了一个 function `topUp(balance, 50)`，结果回到 main，balance 还是 3.50——钱没进去！

为什么？因为 C++ 预设**把值复制一份**给 function（pass by value）。function 改的是副本。

要让 function 改到"本人"，有两个办法：给它**别名**（reference）或给它**地址**（address）。这就是本章。

---

## 1. Why function？（p2–5）

- **Modular programming**：把程式分成小的、独立的 **modules / functions**，每个有名字、可以单独呼叫，**每个 module 只做一件事**。
- **Benefits**：**more manageable code**、**code reusability**、**data protection**、**ease of debugging**。
- **Structure chart** 显示 modules 之间的**阶层关系**（谁呼叫谁）：

```
                    ┌──────────────┐
                    │ Generate_bill│   ← calling module
                    └──────┬───────┘
             ┌─────────────┴──────────────┐
      ┌──────┴────────┐            ┌──────┴──────┐
      │ Read_user_data│            │ Print_bill  │   ← called module / calling module
      └──────┬────────┘            └──────┬──────┘
       ┌─────┴──────┐           ┌─────────┴─────────┐
 ┌─────┴─────┐┌─────┴──────┐┌───┴──────────┐┌───────┴───────┐
 │Print_     ││Search_user_││Calculate_    ││Print_deadline │  ← called modules
 │header     ││name        ││phone_bill    ││               │
 └───────────┘└────────────┘└──────────────┘└───────────────┘
```
*看图重点：上层呼叫下层；中间层（Print_bill）既是被呼叫者也是呼叫者。*

**Function 的三部分（p4）**：
```cpp
void header();              // 1. function declaration (prototype) — 写在 main 前
int main() {
    header();               // 2. function call
}
void header() {             // 3. function definition
    cout << "function";
}
```

**两种 function（p5）**：

| | Void function | Value-returning function |
|---|---|---|
| 回传值 | **不回传** | 用 `return` 回传**一个**值 |
| 宣告 | `void f(…)` | `int f(…)`, `double f(…)`, `bool f(…)` |
| 呼叫 | 独立一句：`f(x);` | 放在表达式里：`y = f(x);`、`if (f(x))` |

### ✅ 满分答法 — Jan 2026 Q3g: TWO benefits of modular programming. (2 marks)
1. **Code reusability** – a function written once can be called many times, in different parts of the program or in other programs, with different inputs.
2. **Easier debugging and maintenance** – each module performs one task and can be tested and corrected independently, so errors are easier to locate and fix.
（其他可接受：more manageable/readable code；data protection；teamwork — different programmers write different modules.）

---

## 2. 为什么不用 global variable？（p6–8）

- **Global variable**：宣告在所有 function 之外，**所有 function 都能读写**。
- 问题：**data protection 被破坏**——任何人任何时候都能改 → 难除错，**不建议使用**（→ Ch11 的 common coupling）。
- 用**参数**传资料的好处：
  - **Data protection**：function 操作自己的副本，共享资料受控制；
  - **Modularity**：function 自给自足，好管理、测试、阅读、除错；
  - **Reusability**：同一个 function 可以用不同的输入重用；
  - **Efficiency**：避免重复 code。

---

## 3. 三种传递方式（p9–19）⭐

### 3.1 一张对照表（slide 没有，最重要）

| | **Pass by value** | **Pass by reference** | **Pass by address** |
|---|---|---|---|
| Function 收到什么 | 值的**副本** | 原变数的**别名** | 原变数的**地址**（pointer） |
| Parameter 写法 | `void fun(int x)` | `void fun(int &x)` | `void fun(int *x)` |
| 呼叫写法 | `fun(a);` | `fun(a);`（**不加 &**） | `fun(&a);`（**要加 &**） |
| Function 内使用 | `x = x + 3;` | `x = x + 3;`（**不用 \***） | `*x = *x + 3;`（**要用 \***） |
| 改到原变数吗 | ❌ | ✅ | ✅ |
| `a = 5; fun(a);` 后 `cout << a;` | **5** | **8** | **8** |
| 用途 | 输入资料、保护原值 | 输出/修改结果 | 输出/修改结果 |

```
 pass by value          pass by reference         pass by address
  main   fun             main   fun                main        fun
 ┌───┐  ┌───┐           ┌───┐                     ┌───┐ addr  ┌──────┐
 │a 5│  │x 5│ copy      │a 5│ ← x is another      │a 5│ <──── │x &a  │
 └───┘  └───┘           └───┘   name for a        └───┘       └──────┘
  x changes, a doesn't   x changes = a changes     *x changes = a changes
```

> 💬 **答题句 (EN)** — *In pass by value, a copy of the argument is passed, so changes inside the function do not affect the original variable. In pass by reference, the parameter (declared with &) is an alias of the original variable, so changes directly modify it. In pass by address, the address of the variable is passed to a pointer parameter (declared with *), and the function modifies the original through dereferencing.*

### 3.2 Pass by value（p9–11）
```cpp
void fun(int x) { x = x + 3; }
int main() { int a = 5; fun(a); cout << a; }   // 5 — a 不受影响
```
- 在被呼叫的 function 里建立**副本**（local variable）；原资料**安全**。
- **最推荐**的方式（为了 data protection）——只要 function 不需要改原值就用它。

**p11 练习 checkMark**
```cpp
bool checkMark(int mark) { return mark >= 1 && mark <= 100; }
int main() {
    int mark; cout << "Enter mark: "; cin >> mark;
    if (checkMark(mark)) cout << "Valid mark";
    else                 cout << "Invalid mark";
}
```

### 3.3 Pass by reference（p12–15）
```cpp
void fun(int &x) { x = x + 3; }    // & 在参数
int main() { int a = 5; fun(a); cout << a; }   // 8
```
- 把呼叫端的变数和参数**连起来**：对参数的任何操作就是对原变数的操作。
- 用 **reference variable (`&`)** 当参数，像建立别名；呼叫时**不加** `&`，function 内**不用** `*`。

**p14 applyDiscount**
```cpp
void applyDiscount(double &price, double discount) { price -= discount; }
```
**p15 updateSteps**（三个 reference 参数）
```cpp
void updateSteps(int &todaySteps, int &weeklySteps, int &extraSteps) {
    todaySteps += extraSteps;
    weeklySteps += extraSteps;
}
int main() {
    int todaySteps = 4700, weeklySteps = 21350, extra;
    cout << "Extra steps: "; cin >> extra;
    updateSteps(todaySteps, weeklySteps, extra);
    cout << "Today: " << todaySteps << ", Week: " << weeklySteps << endl;
}
```
（extraSteps 不需要改，照题目要求用 reference；平常会用 pass by value。）

### 3.4 Pass by address（p16–19）
```cpp
void fun(int *x) { *x = *x + 3; }   // * 在参数，function 内用 * 去改
int main() { int a = 5; fun(&a); cout << a; }   // 8 — 呼叫时传 &a
```
- 把变数的**记忆体地址（pointer）**传进去，function 透过地址**直接存取并修改**原资料。
- `&`（address operator）：**传**地址；`*`（indirection operator）：在地址**存/取**资料。

**p18 topUpWallet**
```cpp
void topUpWallet(double topUpAmount, double *balance) { *balance += topUpAmount; }
int main() {
    double balance = 3.50, amt;
    cout << "Top-up amount: RM"; cin >> amt;
    topUpWallet(amt, &balance);
    cout << "New balance: RM" << fixed << setprecision(2) << balance << endl;
}
```
**p19 withdrawCash**（三个 address 参数）
```cpp
void withdrawCash(double *balance, int *transCount, double *withdrawAmt) {
    if (*withdrawAmt <= *balance) {
        *balance -= *withdrawAmt;
        (*transCount)++;                 // 括号必要：先 dereference 再 ++（→ Ch4 §6）
    } else cout << "Insufficient balance\n";
}
```

### 3.5 Swap（p20–21）

| Pass by reference | Pass by address |
|---|---|
| `void exchange(int &num1, int &num2)` | `void exchange(int *num1, int *num2)` |
| `temp = num1; num1 = num2; num2 = temp;` | `temp = *num1; *num1 = *num2; *num2 = temp;` |
| 呼叫 `exchange(a, b);` | 呼叫 `exchange(&a, &b);` |

### 3.6 Reference parameter vs return statement

### ✅ 满分答法 — Oct 2025 Q3a (2 marks)
A function can **return only one value** through the `return` statement, and the value must be received/used by the caller (e.g. `x = f(y);`). A **reference parameter** gives the function direct access to the caller's variable, so the function can pass back **more than one** output value by modifying the original variables directly, without using `return`.

---

## 4. 写"混合传递"function 的四步模板

> **① 回传类型**：题目说 "boolean type function" → `bool`；"returns the result" → 看类型。
> **② 参数列**：按题目顺序；by value → `type name`；by reference → `type &name`；by address → `type *name`。
> **③ 函式体**：reference 参数直接用名字；address 参数一律 `*name`。
> **④ return**：条件式直接 `return num > 0;`（或 if/else 写清楚）。

### ✅ 满分答法 — Jan 2026 Q4a: processNumber (8 marks) — g++ 验证
```cpp
bool processNumber(int num, int &doubleVal, int *tripleVal)   // (1) bool  (3) three params
{
    doubleVal  = num * 2;          // (1) reference: use name directly
    *tripleVal = num * 3;          // (1) address: dereference with *
    if (num > 0)                   // (1) check positive
        return true;               // (1) return true / false
    else
        return false;
}
```
呼叫例：`int d, t; bool pos = processNumber(5, d, &t);` → d = 10, t = 15, pos = true。

### ✅ 满分答法 — Oct 2025 Q3b(i): analyseNum (8 marks)
```cpp
bool analyseNum(int num, int &square, int *cube)
{
    square = num * num;            // pass by reference
    *cube  = num * num * num;      // pass by address
    if (num % 2 == 0)              // even?
        return true;
    else
        return false;
}
```

### ✅ 满分答法 — Oct 2025 Q3b(ii): main (9 marks) — g++ 验证输出与题目样本一致
```cpp
#include <iostream>
using namespace std;

bool analyseNum(int num, int &square, int *cube);   // (1) prototype

int main()
{
    int num, square, cube;                          // (1) declare
    bool isEven;

    cout << "Enter an integer: ";                   // (1) prompt
    cin >> num;                                     // (1) input

    isEven = analyseNum(num, square, &cube);        // (2) call: & only for cube

    cout << "\nResult" << endl;                     // (3) display
    cout << "Square of the integer: " << square << endl;
    cout << "Cube of the integer: "   << cube   << endl;
    cout << "Even number: " << (isEven ? "true" : "false") << endl;
    return 0;
}
```
样本（输入 5）：`Square of the integer: 25` / `Cube of the integer: 125` / `Even number: false`。
（也可以 `cout << boolalpha << isEven;`——`boolalpha` 让 bool 印成 true/false 而不是 1/0。）

### ✅ Jan 2026 Q4c(i) checkResult — pass by address with string（6 marks）
```cpp
void checkResult(int mark, string *result)
{
    if (mark >= 50)
        *result = "PASS";
    else
        *result = "FAIL";
}
```
(ii) 呼叫（2 marks）：`checkResult(stud.pspMark, &result);` → result = "PASS"（86 ≥ 50）。详见 **Ch6**（传 struct 的成员）。

---

## 5. Flowchart & pseudocode for functions（p23–24）

每个 function 有**自己的**一张 flowchart / pseudocode；main 里的呼叫用**双边长方形**（predefined process）表示。

```
   main                        title()                  addition(a,b)
  ( START )                   ( title )                ( addition(a,b) )
      │                           │                          │
  ║ title() ║                /Display     /             [ sum = a + b ]
      │                     / "Welcome to /                  │
  /Prompt & read /          /  Addition" /             ( return sum )
  /   a, b      /               │
      │                     ( return )
  ║ sum =        ║
  ║ addition(a,b)║
      │
  / Display sum /
      │
   ( END )
```
```
START                        addition(a,b)            title()
  Call title()                  sum = a + b              Display "Welcome to Addition"
  Prompt to enter a & b      Return sum               Return
  Sum = Call addition(a, b)
  Display Sum
END
```
*看图重点：main 的 flowchart 以 START/END 开始结束；每个被呼叫的 function 以**自己的名字**开始、以 **return** 结束。*

---

## 6. Tutorial 5 答案（全部 g++ 验证）

**Q1 withdrawal**（i）void 版 vs（ii）return 版
```cpp
// (i) void: 在 function 里显示
void withdrawal(double balance, double amount) {
    if (amount <= balance) cout << "New balance: RM" << balance - amount << endl;
    else cout << "Insufficient balance!" << endl;
}
// (ii) value-returning: 把新余额回传给 main
double withdrawal(double balance, double amount) {
    if (amount <= balance) return balance - amount;
    cout << "Insufficient balance!" << endl;
    return balance;              // 失败时回传原余额
}
// main: double balance = 2000; balance = withdrawal(balance, amt);
```
重点：(i) 的新余额**没有回到 main**；(ii) 用 return 让 main 得到新余额。

**Q2 calculateBill**（题目没给 base fee，这里假设 RM5.00；采用累进计费）
```cpp
const double BASE_FEE = 5.00;
double calculateBill(int units) {
    double bill = BASE_FEE;
    if (units <= 100)      bill += units * 0.50;
    else if (units <= 300) bill += 100 * 0.50 + (units - 100) * 0.75;
    else                   bill += 100 * 0.50 + 200 * 0.75 + (units - 300) * 1.20;
    return bill;
}
// 80 units → 45.00；250 → 167.50；400 → 325.00
```

**Q4 convertTime**（reference 回传两个结果）
```cpp
void convertTime(double hours, double &minutes, double &seconds) {
    minutes = hours * 60;  seconds = hours * 3600;
}   // 1.5 h → 90 min, 5400 s
```
**Q5 updateItemPrice**：`void updateItemPrice(double &price) { price *= 0.90; price *= 1.06; }`（100 → 95.40）

**Q6 fixBalances**：`void fixBalances(double *a, double *b) { double t = *a; *a = *b; *b = t; }` 呼叫 `fixBalances(&acc1, &acc2);`

**Q7 rectangle**：`void rectangle(double l, double w, double *area, double *peri) { *area = l*w; *peri = 2*(l+w); }`（6.5 × 3 → 19.50, 19.00）

**Q8 processScore**（score by address，grade by reference）
```cpp
void processScore(int *score, char &grade) {
    *score += 5;                       // bonus
    if (*score > 100) *score = 100;    // (合理的上限，题目没说)
    if      (*score >= 85) grade = 'A';
    else if (*score >= 70) grade = 'B';
    else if (*score >= 60) grade = 'C';
    else if (*score >= 50) grade = 'D';
    else                   grade = 'F';
}   // 82 → 87, A
```
**Q9**：`void toFahrenheit(double &celsius, double *fahrenheit) { *fahrenheit = celsius * 9 / 5 + 32; }`（30 → 86）

**Q10 calculateRental**（三种都用 + return）
```cpp
const double DAILY_RATE = 80;
double calculateRental(int *days, int threshold, double &discount) {
    double base = *days * DAILY_RATE;
    discount = (*days > threshold) ? base * 0.10 : 0;
    return base - discount;
}
// main: int days; cin >> days; double disc;
//       double total = calculateRental(&days, 7, disc);   // 10 days → disc 80.00, total 720.00
```

---

## Closing the loop

电子钱包的 balance 为什么没变？因为是 pass by value。改成 `topUpWallet(amt, &balance)`（address）或 `void topUp(double &balance, …)`（reference）就能改到本人。需要"回传两三个结果"时，return 只能给一个，其余用 reference / address。

下一章把 array 和 structure 传进 function，并讲 variable 的**生命周期与可见范围**（storage classes）。

---

## ⚠️ Where the slides mislead

| Slide | Slide 写 | 更准确 |
|---|---|---|
| p4, p10… | `void main()` | 标准 C++：`int main()`（g++ 不接受 void main） |
| p15 | extraSteps 也用 reference | 按题目可以，但不需修改的参数平常用 pass by value |
| p9 | "preferred passing technique" | 对"只读输入"而言；大型 struct 为效率常用 `const &`（extra） |
| p21 | "Exchanging…Pass by Address" 图示 `num1 num2 (…are addresses)` | 正确；注意呼叫要 `exchange(&a, &b)` |

---

## Term table

| English | 中文 | 一句话说明 |
|---|---|---|
| Modular programming | 模组化程式设计 | 程式拆成独立 functions |
| Structure chart | 结构图 | 显示 module 阶层 |
| Function prototype / declaration | 函式原型 | main 前告诉 compiler |
| Function call / definition | 呼叫 / 定义 | |
| Void function | 无回传函式 | |
| Value-returning function | 有回传值函式 | 用 return |
| Actual / formal parameter | 实际 / 形式参数 | 呼叫时给的 / 定义里的 |
| Pass by value | 传值 | 副本 |
| Pass by reference | 传参考 | 别名 `&` |
| Pass by address | 传地址 | pointer `*` |
| Global variable | 全域变数 | 所有 function 可用，不推荐 |

---

## Cheat sheet

- Benefits of modular programming：manageable, reusable, data protection, easier debugging。
- Value：`f(int x)` / `f(a)` / 原值不变。
- Reference：`f(int &x)` / `f(a)` / 直接用 x。
- Address：`f(int *x)` / `f(&a)` / 用 `*x`。
- return 只能一个值；多个输出用 & 或 *。
- `(*p)++` 要括号。
- bool 印 true/false：`boolalpha` 或 `? "true" : "false"`。
- Swap：ref `temp=a;a=b;b=temp;`；addr `temp=*a;*a=*b;*b=temp;` 呼叫 `swap(&x,&y)`。

---

## Practice (answers included)

### A. MCQ
1. `void f(int *p){ *p = 0; } int x = 9; f(&x);` x is  (a) 9 (b) 0 (c) address (d) error
2. `void g(int &r){ r++; } int y = 1; g(y); g(y);` y is  (a) 1 (b) 2 (c) 3 (d) error
3. Which call is correct for `void h(double *d);` with `double v;`?  (a) `h(v)` (b) `h(*v)` (c) `h(&v)` (d) `h(&&v)`
4. Which parameter method protects the original data best?  (a) value (b) reference (c) address (d) global
5. A function must give back a total and an average. Best design?  (a) two return statements (b) return one, other by reference (c) global variables (d) impossible

**Answers:** 1-b, 2-c, 3-c, 4-a, 5-b

### B. Short answer
**B1.** Write `void stats(int a, int b, int &sum, double *avg)` and a call.
```cpp
void stats(int a, int b, int &sum, double *avg) { sum = a + b; *avg = sum / 2.0; }
int s; double m; stats(4, 7, s, &m);   // s = 11, m = 5.5
```
**B2.** State one advantage of pass by reference over pass by address. — Simpler syntax: no `&` at the call and no `*` inside the function, so fewer pointer mistakes.

### C. Application
**C1.** Write `bool checkBMI(double weight, double height, double &bmi, string *status)` — calculates BMI = weight / height², sets status "Normal" if 18.5–24.9 else "Not normal", returns true if BMI ≥ 25 (overweight).
```cpp
bool checkBMI(double weight, double height, double &bmi, string *status) {
    bmi = weight / (height * height);
    if (bmi >= 18.5 && bmi <= 24.9) *status = "Normal";
    else                            *status = "Not normal";
    return bmi >= 25;
}
```
**C2.** Write the main to call C1 and print results.
```cpp
double w, h, bmi; string status;
cin >> w >> h;
bool over = checkBMI(w, h, bmi, &status);
cout << "BMI: " << bmi << "\nStatus: " << status << "\nOverweight: " << boolalpha << over << endl;
```

### D. Thinking
**D1.** Why is using global variables instead of parameters considered poor design? — Any function can change them at any time, so data is unprotected, modules become tightly coupled (common coupling), and bugs are hard to trace.
**D2.** When should a function be void vs value-returning? — Value-returning when it produces one result the caller needs (e.g. calculateBill); void when it only performs an action (display) or returns several results through reference/address parameters.

---

## Slide index
| Note section | Slides |
|---|---|
| 1 Why function | p2–5 |
| 2 Global vs parameters | p6–8 |
| 3.2 Pass by value | p9–11 |
| 3.3 Pass by reference | p12–15 |
| 3.4 Pass by address | p16–19 |
| 3.5 Swap | p20–22 |
| 5 Flowchart | p23–24 |

## Links to other chapters
- `&`, `*`, reference variable → **Ch4**
- Passing arrays / structures, storage classes → **Ch6**
- Template functions → **Ch7**
- Coupling types (data vs common coupling) → **Ch11**
