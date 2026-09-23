# AMCS2123 — Chapter 7: Template

> 只有 12 页，但两份考卷都考：**Oct 2025 Q4b**（加法 template，4 分）和 **Jan 2026 Q2b**（显示温度讯息的 template + main，10 分）。Slide 有两个例子本身是错的：p9 对 `void` function 写 `cout << printPair(…)`（**compile error**）；p10 说输出 `equal? false`，但没有 `boolalpha` 时 `cout` 印的是 **0**。Slide 也没说"什么时候用一个 T、什么时候用 T1/T2"——而 Jan 2026 的温度题正好需要**两个不同类型**（string 的星期几 + double 的温度）。这份笔记补上判断规则，所有 code 以 g++ 验证。

---

## 0. 一句话总览

**Template 是 function 的"设计图"：用 `template <typename T>` 把类型变成参数，compiler 按你呼叫时给的实际类型自动产生对应版本；参数类型都一样用一个 T，不同就用 T1、T2。**

| Part | 内容 | Slide |
|---|---|---|
| 1 | Template 是什么 | p3 |
| 2 | 为什么用（reusability、type safety、less duplication、maintenance） | p4 |
| 3 | Syntax 1：单一 type parameter + 例子 | p5–7 |
| 4 | Syntax 2：多个 type parameters + 例子 | p8–10 |
| 5 | Exercises | p11–12 |

🎯 **这章回答的考题**
- **Oct 2025 Q4b** 计算机的"加两个值"function 写成 template，可加不同类型 (4)
- **Jan 2026 Q2b(i)** Template 显示 "Monday average temperature is 30.5 Celsius" (5)；**(ii)** main：输入星期几与温度并用 template 显示 (5)

**Prerequisites**：Ch5 function 的写法（prototype、参数、return）。

---

## Scene：同一个 function 写三次

你写了 `int add(int a, int b)`。接着要加 double → 再写 `double add(double, double)`。再来 float……三个 function 内容一模一样（`return a + b;`），只有**类型**不同。改一个 bug 要改三次。

能不能写**一次**，让类型"留空"，用的时候再填？→ **Template**。

---

## 1–2. Template 是什么？为什么用？（p3–4）

- **Template** 是 C++ 支援**泛型程式设计 (generic programming)** 的功能。
- 是一个**蓝图/公式**，用来产生**通用的 function 或 class**，以达到**重用性与弹性**——不必为每种资料类型各写一个。
- 当**同一个运算**要作用在**不同资料类型**上时最有用。

| Benefit | 说明 |
|---|---|
| **Code reusability** | 只写一个 function，多种类型都能用 |
| **Type safety** | Compile 时检查类型正确，减少 runtime error |
| **Reduced code duplication** | 不必为每种类型重写相似 function |
| **Ease of maintenance** | 只有一个版本要更新 |

> 💬 **答题句 (EN)** — *A function template is a blueprint for a generic function in which the data type is a parameter (e.g. T). The compiler generates a version of the function for each data type it is called with, so one function can be reused for different types, reducing code duplication and making maintenance easier while keeping type safety at compile time.*

---

## 3. Syntax 1：一个 type parameter（p5–7）

```cpp
template <typename T>
T functionName(T parameter1, T parameter2)
{
    // function body
}
```
- `T` 是资料类型的**占位符 (placeholder)**；名字可自取，惯例用 T（Type）。
- `template <typename T>` 告诉 compiler：接下来的 function 是 template，T 是 generic type。
- （extra：`typename` 也可以写成 `class`，意思相同。）

**例 1（p6）**
```cpp
template <typename T>
T add(T a, T b) { return a + b; }

cout << add(3, 4);       // T = int    → 7
cout << add(2.5, 6.8);   // T = double → 9.3
```
**例 2（p7）**
```cpp
template <typename T>
T maximum(T a, T b) { return (a > b) ? a : b; }

cout << maximum(3, 7);       // 7
cout << maximum(3.5, 2.1);   // 3.5
cout << maximum('a', 'z');   // z   (char 按 ASCII 比较)
```
⚠️ 一个 T 时，**两个参数必须同类型**：`add(3, 4.5)` 会 compile error（T 不能同时是 int 和 double）。要嘛写 `add<double>(3, 4.5)`，要嘛用两个 type parameter。

### ✅ 满分答法 — Oct 2025 Q4b: Calculator add template (4 marks)
```cpp
template <typename T>              // (1) template header
T add(T value1, T value2)          // (1) return type T, two T parameters
{
    return value1 + value2;        // (1) add and return
}
// usage (1): add(10, 20) → 30 ; add(2.5, 3.75) → 6.25
```

---

## 4. Syntax 2：多个 type parameters（p8–10）

```cpp
template <typename T1, typename T2>
returnType functionName(T1 parameter1, T2 parameter2)
{
    // function body
}
```
- 当 function 要处理**不同类型的参数**时使用。

**例 1（p9）**
```cpp
template <typename T1, typename T2>
void printPair(T1 a, T2 b) {
    cout << "First: "  << a << endl;
    cout << "Second: " << b << endl;
}
printPair(8, 42.5);          // First: 8 / Second: 42.5
printPair("Hello", 2025);    // First: Hello / Second: 2025
```
⚠️ Slide 写 `cout << printPair(8, 42.5) << endl;`——`printPair` 是 **void**，没有值可以印 → **compile error**。正确是直接呼叫 `printPair(8, 42.5);`。

**例 2（p10）**
```cpp
template <typename T1, typename T2>
bool compareValues(T1 a, T2 b) { return a == b; }

int a = 69, b = 96;
cout << "Are both values " << a << " & " << b << " equal? " << compareValues(a, b);
```
⚠️ Slide 说输出 `equal? false`，但 `cout` 印 bool 预设是 **0 / 1**，实际输出 `equal? 0`（g++ 验证）。要印 `false` 须加 `cout << boolalpha`。

### 4.1 什么时候用 T，什么时候用 T1/T2？

| 情况 | 用 |
|---|---|
| 所有参数**同类型**（加法、最大值、交换两个同类变数） | `template <typename T>` |
| 参数**不同类型**（名字 + 数字、int + float） | `template <typename T1, typename T2>` |
| 不需要回传 | 回传类型写 `void` |

### ✅ 满分答法 — Jan 2026 Q2b(i): Template to display "Monday average temperature is 30.5 Celsius" (5 marks)
星期几是**文字**、温度是**数字** → 两种类型 → T1、T2。
```cpp
template <typename T1, typename T2>                       // (1)
void displayTemperature(T1 day, T2 temperature)           // (2) two generic params, void
{
    cout << day << " average temperature is "             // (2) message format
         << temperature << " Celsius" << endl;
}
```
### ✅ 满分答法 — Jan 2026 Q2b(ii): main (5 marks) — g++ 验证
```cpp
int main()
{
    string day;                                    // (1) declare
    double temperature;

    cout << "Enter the day of the week: ";         // (1) prompt + input
    cin >> day;
    cout << "Enter the average temperature: ";     // (1)
    cin >> temperature;

    displayTemperature(day, temperature);          // (2) call the template
    return 0;
}
```
输入 `Monday` 与 `30.5` → `Monday average temperature is 30.5 Celsius`。
（template 必须写在 main **之前**，或在 main 前再写一次 `template <typename T1, typename T2> void displayTemperature(T1, T2);` 当 prototype。）

---

## 5. Exercises（p11–12）与 Tutorial 7（全部 g++ 验证）

**Slide p11 squareValue**
```cpp
template <typename T>
T squareValue(T v) { return v * v; }
int main() { int n; cin >> n; cout << "Square: " << squareValue(n); }
```
**Slide p12 swapValues（两种类型，by reference）**
```cpp
template <typename T1, typename T2>
void swapValues(T1 &a, T2 &b) { T1 temp = a; a = b; b = temp; }

int x = 5; float y = 2.7;
swapValues(x, y);     // x = 2, y = 5
```
⚠️ 不同类型交换会**失去资料**：2.7 放进 int x 被截成 **2**（g++ 验证）。这是题目的设计问题，值得在答案里点出。

**Tutorial 7 Q1 calculateInterest**
```cpp
template <typename T>
T calculateInterest(T balance, T rate) { return balance + balance * rate / 100; }
// main: double bal, rate; cin >> bal >> rate;
//       cout << "Before: " << bal; bal = calculateInterest(bal, rate); cout << " After: " << bal;
// 1000, 3.5 → 1035
```
**Q2 findAverage（template + array）**
```cpp
template <typename T>
double findAverage(T marks[], int size) {
    double sum = 0;
    for (int i = 0; i < size; i++) sum += marks[i];
    return sum / size;
}
int marks[5] = {80, 75, 90, 66, 70};
cout << findAverage(marks, 5);          // 76.2
```
**Q3 announcement**
```cpp
template <typename T>
void announce(T a, T b) {
    cout << "The book prize winners for A and B categories are " << a << " and " << b << "." << endl;
}
string nameA, nameB; getline(cin, nameA); getline(cin, nameB);
announce(nameA, nameB);
```
**Q4 cheaper Item（template + struct）**
```cpp
struct Item { string code; double price; };
template <typename T>
T cheaper(T a, T b) { return (a.price < b.price) ? a : b; }
Item i1 = {"A01", 4.50}, i2 = {"B02", 3.90};
cout << cheaper(i1, i2).code;           // B02
```
**Q5 minimum of 3（不同类型也可）**
```cpp
template <typename T1, typename T2, typename T3>
double minOf3(T1 a, T2 b, T3 c) {
    double m = a;
    if (b < m) m = b;
    if (c < m) m = c;
    return m;
}
cout << minOf3(4, 2, 9);             // 2
cout << minOf3(1.5f, 0.5f, 2.5f);    // 0.5
```
（若三个同类型，用一个 T：`template <typename T> T minOf3(T a, T b, T c)`。）

**Q6 increaseSalary（template + array of struct）**
```cpp
struct Employee { int id; string name; double salary; };
template <typename T>
void increaseSalary(T emp[], int size, double percent) {
    for (int i = 0; i < size; i++) emp[i].salary *= (1 + percent / 100);
}
Employee emp[3] = {{1,"Ali",3000}, {2,"Mei",4000}, {3,"Raj",5000}};
increaseSalary(emp, 3, 10);              // 3300, 4400, 5500
```

---

## Closing the loop

三个一样的 `add` 变成一个 `template <typename T> T add(T a, T b)`；要混合类型时用 T1、T2。Template 让 function 通用，但资料本身还可以更有意义——下一章用 **typedef** 和 **enum** 自己定义"类型"。

---

## ⚠️ Where the slides mislead

| Slide | Slide 写 | 更准确 |
|---|---|---|
| p9 | `cout << printPair(8, 42.5) << endl;` | void function 不能放进 cout → compile error；直接 `printPair(8, 42.5);` |
| p9 | `’’First: ’’` 弯引号 | 真 code 用 `"First: "` |
| p10 | 输出 `equal? false` | 预设印 `0`；要 `boolalpha` 才是 false |
| p12 | swap 不同类型的两个变数 | 会截断资料（float → int） |
| 档名 | "TemplatesNotyetready" | 讲师档名标示未完成，但内容已被考 |

---

## Term table

| English | 中文 | 一句话说明 |
|---|---|---|
| Template | 模板 | 类型参数化的 function 蓝图 |
| Generic programming | 泛型程式设计 | 写一次适用多类型 |
| Type parameter (T) | 类型参数 | 类型的占位符 |
| `typename` / `class` | — | 宣告 type parameter 的关键字 |
| Type safety | 类型安全 | compile 时检查 |
| Template instantiation | 模板实例化 | compiler 按实际类型产生版本（extra） |
| `boolalpha` | — | 让 bool 印成 true/false |

---

## Cheat sheet

- `template <typename T> T f(T a, T b) { … }`
- `template <typename T1, typename T2> void f(T1 a, T2 b) { … }`
- 同类型 → 一个 T；不同类型 → T1, T2（, T3）。
- 一个 T 时 `f(3, 4.5)` 错（类型冲突）。
- void template 直接呼叫，不能 `cout << f()`。
- Template 写在 main 前（或加 prototype）。
- 好处：reusability、type safety、less duplication、ease of maintenance。

---

## Practice (answers included)

### A. MCQ
1. `template <typename T> T mul(T a, T b){return a*b;}` — `mul(3, 2.0)` gives  (a) 6 (b) 6.0 (c) compile error (d) 5
2. Which header declares two type parameters?  (a) `template <T1, T2>` (b) `template <typename T1, typename T2>` (c) `template typename <T1,T2>` (d) `typename <T1, T2>`
3. `template <typename T> void show(T x){cout<<x;}` — `cout << show(5);` is  (a) 5 (b) error (c) 0 (d) blank
4. The main benefit of templates is  (a) faster run time (b) code reusability for different types (c) smaller variables (d) automatic memory management
5. `cout << (2 == 3);` prints  (a) false (b) 0 (c) 1 (d) error

**Answers:** 1-c, 2-b, 3-b, 4-b, 5-b

### B. Short answer
**B1.** Write a template `isEqual` that returns true if two values of the same type are equal. → `template <typename T> bool isEqual(T a, T b) { return a == b; }`
**B2.** Write a template `displayPrice` that prints `"Item: <name>, Price: RM<price>"` for any name and price types.
```cpp
template <typename T1, typename T2>
void displayPrice(T1 name, T2 price) { cout << "Item: " << name << ", Price: RM" << price << endl; }
```

### C. Application
**C1.** Write a template `total` that returns the sum of an array of any numeric type, and use it for `int qty[4]` and `double price[3]`.
```cpp
template <typename T>
T total(T arr[], int n) { T s = 0; for (int i = 0; i < n; i++) s += arr[i]; return s; }
cout << total(qty, 4) << " " << total(price, 3);
```
**C2.** Write a template `larger` that works for two `Student` structs by comparing `cgpa`, and returns the student with the higher CGPA. → same pattern as Tutorial 7 Q4 using `.cgpa`.

### D. Thinking
**D1.** Why is a template better than writing overloaded functions `add(int,int)`, `add(double,double)`? — One definition to write and maintain; any new type works automatically without extra code.
**D2.** When is a template NOT appropriate? — When the logic differs by type (e.g. comparing strings case-insensitively vs numbers), or when the operation is not valid for some types (e.g. `*` on strings).

---

## Slide index
| Note section | Slides |
|---|---|
| 1–2 What & why | p3–4 |
| 3 Syntax 1 | p5–7 |
| 4 Syntax 2 | p8–10 |
| 5 Exercises | p11–12, Tutorial 7 |

## Links to other chapters
- Function basics / parameters → **Ch5**
- Passing arrays / structs to templates → **Ch6**
- typedef (another way to name types) → **Ch8**
