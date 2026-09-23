# AMCS2123 — Chapter 8: User-defined Types (typedef & enum)

> 两份考卷都各考一题：**Oct 2025 Q4a** 把 6 间咖啡店写成 `enum Cafe` 并宣告 `favouriteCoffee = Cotti`（4 分），**Jan 2026 Q4b** 用 `typedef` 定义 `Day`, `Month`, `Year` 再写一个输入生日的程式（9 分）。Slide 的陷阱在于：咖啡店名字有**空格**（"Zus Coffee"），但 enum 的值必须是**合法识别字**——slide 只说 "literal values must be identifier"，没示范怎么处理。另外 slide p14 当"正确例子"的 `enum Program {RMM, RSD, RSF, RSD, …}` 里 **RSD 重复**，本身就不合法；p10 答案 `cons int SUN` 是 typo。这份笔记补上命名规则、enum 的合法/非法运算表，所有 code 以 g++ 验证。

---

## 0. 一句话总览

**Simple type 是不可再分的值；built-in type 有 bool/char/short/int/long/float/double/void；程式设计师可以自己定义类型：typedef 为既有类型取"别名"（不产生新类型），enum 列出一组有顺序的识别字当作新类型的全部可能值（内部是 0, 1, 2…）。**

| Part | 内容 | Slide |
|---|---|---|
| 1 | Simple type vs complex type；primitive (built-in) types 与 byte 数 | p2–5 |
| 2 | User-defined types 概念 | p6 |
| 3 | **typedef**：语法、例子、typedef struct | p7–12 |
| 4 | **enum**：定义、合法性、变数、运算、switch、function | p13–27 |

🎯 **这章回答的考题**
- **Oct 2025 Q4a** `enum Cafe` 表示 Zus Coffee, San Francisco Coffee, Gigi, Cotti, HWC, Starbuck，并宣告 `favouriteCoffee` 初始化为 Cotti (4)
- **Jan 2026 Q4b** `typedef` 定义 `Day`, `Month`, `Year`（int），宣告变数、输入生日、显示 `Date of birth: day/month/year` (9)

**Prerequisites**：Ch3 struct（`typedef struct`）、switch statement、Ch5 function 回传值。

---

## Scene：看不懂的数字

同事留下这段 code：
```cpp
int d = 3;
if (d == 5) price = 12;
```
3 是什么？5 是什么？原来 3 是"星期四"、5 是"Cotti 咖啡"。数字没有意义，别人（和三个月后的你）都看不懂。

两个办法：
- 给类型一个有意义的名字：`typedef int Day;` → `Day d;`
- 给值一个有意义的名字：`enum Cafe {ZUS, SANFRAN, GIGI, COTTI, …};` → `if (fav == COTTI)`

---

## 1. Simple type 与 built-in type（p2–5）

- **Simple type**：每个值是 **atomic（不可分割）** 的，如 `int x = 9;`；相对地，array、struct 是 **complex type**（由多个值组成）。
- 几乎所有语言都提供一组 **primitive (built-in) data types**：

| 类别 | Type | Bytes（slide / Windows） |
|---|---|---|
| Integral | bool | 1 |
| | char | 1 |
| | short | 2 |
| | int | 4 |
| | long | 4（64-bit Linux 为 8） |
| Floating | float | 4 |
| | double | 8 |
| void | void | — |

- **User-defined types**：让程式设计师**建立新的资料类型**，贴合程式的需要：
  - **typedef**：为**已存在的类型**取一个**新名字**；
  - **enum**：列出组成该类型的**字面值 (literal values)** 来**定义新类型**。

---

## 2. typedef（p7–12）

- **只是替既有类型取别名**，**不会**产生新类型。
- 语法：`typedef ExistingType NewName;`

```cpp
typedef int Status;          // Status 就是 int
const int GOOD = 1;
Status condition;
condition = GOOD;            // 等同 int condition; condition = 1;
```

**p9–10：星期几**
```cpp
typedef int Days;
const int MON = 1, TUE = 2, WED = 3, THU = 4, FRI = 5, SAT = 6, SUN = 7;
Days weekday[5] = {MON, TUE, WED, THU, FRI};
Days restday[2] = {SAT, SUN};
```
⚠️ Slide 答案写 `cons int SUN = 7;`——typo，应为 `const`。

**p11–12：typedef struct**（结构取别名，之后宣告不必再写 `struct`）
```cpp
typedef struct {
    string name;
    int age;
    string seatNumber;
} Passenger;                              // Passenger 是这个 struct 的名字

void printPassenger(vector<Passenger> passengers) {
    cout << "Passenger List:\n";
    for (Passenger p : passengers)
        cout << "Name: " << p.name << ", Age: " << p.age << ", Seat: " << p.seatNumber << endl;
}
int main() {
    vector<Passenger> flight101 = { {"Ah Chin", 34, "12A"}, {"Ah Lam", 45, "12B"}, {"Ah Wong", 29, "13A"} };
    printPassenger(flight101);
}
```
（需要 `#include <vector>`。）

> 💬 **答题句 (EN)** — *typedef creates an alias (an additional name) for an existing data type; it does not create a new type. It makes programs more readable and easier to maintain, e.g. typedef double Temperature;*

### ✅ 满分答法 — Jan 2026 Q4b (9 marks) — g++ 验证
```cpp
#include <iostream>
using namespace std;

typedef int Day;                    // (1)
typedef int Month;                  // (1)
typedef int Year;                   // (1)

int main()
{
    Day day;                        // (2) declare with the new type names
    Month month;
    Year year;

    cout << "Enter your birth day: ";      // (2) prompt and input
    cin >> day;
    cout << "Enter your birth month: ";
    cin >> month;
    cout << "Enter your birth year: ";
    cin >> year;

    cout << "Date of birth: " << day << "/" << month << "/" << year << endl;   // (2) display format
    return 0;
}
```
输入 5, 10, 2004 → `Date of birth: 5/10/2004`。
评分重点：三个 typedef 写对（`typedef int Day;` 不是 `typedef Day int;`）；变数用 `Day/Month/Year` 宣告而不是 `int`；输出格式 `day/month/year`。

---

## 3. enum（p13–27）⭐

**Enumeration type** = 使用者定义的类型，它的**值域**是一组**有顺序的字面值（enumerators）**，以**识别字**表示。
```cpp
enum Days {MON, TUE, WED, THU, FRI, SAT, SUN};
//   ↑新类型     ↑ enumerators：依序是 0, 1, 2, 3, 4, 5, 6
```

### 3.1 合法规则

| 规则 | 说明 |
|---|---|
| Enumerator 必须是**识别字** | 不能是数字、字元常数、字串；不能有空格；不能以数字开头 |
| **同一 scope 内唯一** | 同一个名字不能出现在两个 enum 里（也不能在同一个 enum 出现两次） |
| 有**顺序** | 依序对应 0, 1, 2, …（可自订，如 `PARK = 1`，extra） |

**p15 题目答案**
1. `enum Vowel {'A','E','I','O','U'};` → **Invalid**：'A' 是字元常数，不是识别字。
2. `enum Prizes {1st, 2nd, 3rd};` → **Invalid**：识别字不能以数字开头。
3. `enum Animals {…, RABBIT, …}; enum Pets {…, RABBIT, …};` → **Invalid**：RABBIT 在同一 scope 出现两次。

⚠️ Slide p14 把 `enum Program {RMM, RSD, RSF, RSD, REI, RIT};` 当例子——**RSD 重复**，这行本身就 compile error。

**Tutorial 8 Q5**：`Colors` 和 `Flowers` 都有 **VIOLET** → 同一 scope 重复，不合法。

### 3.2 名字有空格怎么办？（考题关键）
把空格改成底线或连写，通常全大写：
| 原名 | Enumerator |
|---|---|
| Zus Coffee | `ZUS_COFFEE` |
| San Francisco Coffee | `SAN_FRANCISCO_COFFEE` |
| Gigi | `GIGI` |
| Cotti | `COTTI` |
| HWC | `HWC` |
| Starbuck | `STARBUCK` |

### ✅ 满分答法 — Oct 2025 Q4a (4 marks) — g++ 验证
```cpp
enum Cafe {ZUS_COFFEE, SAN_FRANCISCO_COFFEE, GIGI, COTTI, HWC, STARBUCK};   // (2)
Cafe favouriteCoffee = COTTI;                                              // (2)
```
（`cout << favouriteCoffee;` 会印 **3**——Cotti 是第 4 个，index 3。）

### 3.3 宣告变数与运算（p16–20）

```cpp
enum Weekdays {MON, TUE, WED, THU, FRI};
Weekdays workday = MON;          // 宣告 + 初始化
workday = MON;                   // 赋值
if (workday == MON) cout << "Monday Blue";   // 比较
cout << workday;                 // 印出 index → 0
```

| 运算 | 合法？ | 说明 |
|---|---|---|
| `workday = MON;` | ✅ | 赋 enumerator |
| `if (workday == MON)`、`workday < WED` | ✅ | 比较（按顺序） |
| `cout << workday;` | ✅ | 印出**整数 index**，不是名字 |
| `workday = 1;` | ❌ | int 不能**隐式**转成 enum |
| `workday++;` | ❌ | 同上 |
| `workday = workday + 1;` | ❌ | `workday + 1` 是 int |
| `workday = Weekdays(1);` | ✅ | **明确转型** → TUE |
| `workday = Weekdays(workday + 1);` | ✅ | 往下一天 |

**p21 题目**：`workday = FRI;` → 不 < WED、不 == WED → `mood = EXCITED` → `cout << mood;` 印 **`Mood = 3`**（EXCITED 的 index；g++ 验证）。

### 3.4 enum + switch（p24–25）
Enumerators 是常数，可以当 `case` 标签：
```cpp
enum Emotion {SAD, HAPPY, ANGRY, EXCITED};
Emotion mood = ANGRY;
switch (mood) {
    case SAD:              cout << "Don't worry, be happy"; break;
    case HAPPY: case EXCITED: cout << "Keep being happy!"; break;
    case ANGRY:            cout << "Forgive, and you'll forget";
}
```

### 3.5 enum + function（p26–27）
```cpp
enum Weekday {MON, TUE, WED, THU, FRI};
Weekday stringToDay(string d) {
    if (d == "Monday") return MON;
    else if (d == "Tuesday") return TUE;
    // ...
}
Weekday workday = stringToDay("Monday");
```
**p27 填空**
```cpp
enum Mood {SAD, HAPPY};
Mood getMood(int);                 // ①  prototype
int main() {
    int x = 1;
    Mood ans = getMood(1);         // ②  Mood
    return 0;
}
Mood getMood(int m) {              // ③ Mood   ④ int
    Mood emotion;
    if (m == 0) emotion = SAD;
    else        emotion = HAPPY;
    return emotion;
}
```

### 3.6 Slide p22–25 快餐练习（g++ 验证）
```cpp
enum Fastfood  {FF, BG, PZ};
enum Friedfood {FFC, FFN, FFF};
enum Burger    {BGC, BGB, BGF};
enum Pizza     {PZC, PZV, PZT};

Fastfood  food  = FF;              // 初始化为每类第一项
Friedfood fried = FFC;
Burger    burg  = BGC;
Pizza     piz   = PZC;
cout << food << fried << burg << piz;     // 0000（印 index）

int index;
cin >> index;                      // 例如 1
food = Fastfood(index);            // 明确转型 → BG

double price;                      // p25：用 switch 决定 burger 价钱
switch (burg) {
    case BGC: price = 5.80; break;
    case BGB: price = 5.50; break;
    case BGF: price = 6.80; break;
}
```

> 💬 **答题句 (EN)** — *An enumeration type is a user-defined type whose values are an ordered list of named identifiers (enumerators). Each enumerator has an integer value starting from 0, the enumerators must be valid unique identifiers, and an integer can only be assigned to an enum variable with an explicit cast such as Weekdays(1).*

---

## 4. Tutorial 8 答案（g++ 验证）

**Q1 ProductNo**
```cpp
typedef int ProductNo;
ProductNo pNo = 7;
double uPrice;
switch (pNo) {
    case 7:  uPrice = 345.00; break;
    case 8:  uPrice = 853.00; break;
    case 9:  uPrice = 471.00; break;
    case 10: uPrice = 933.00; break;
}
```
**Q2 Temperature**：`typedef double Temperature; Temperature morning = 26.5, afternoon = 33.1; cout << (morning + afternoon) / 2;`（29.8）

**Q3 地址**
```cpp
typedef string FirstName, LastName, Street, City, State, PostalCode;
FirstName first; LastName last; Street street; City city; State state; PostalCode postcode;
getline(cin, first); getline(cin, last); getline(cin, street);
getline(cin, city); getline(cin, state); getline(cin, postcode);
cout << last << ", " << first << endl
     << street << endl << postcode << " " << city << ", " << state << endl;
```
**Q4 patientRecord**
```cpp
typedef struct { string id, name; int age; string bloodGroup; double bill; } patientRecord;
void applyDiscount(patientRecord p) {
    if (p.age >= 60) p.bill *= 0.80;
    cout << "Final bill: RM" << fixed << setprecision(2) << p.bill << endl;
}
int main() {
    patientRecord patient = {"P001", "Tan Ah Kow", 65, "O+", 500.00};
    cout << patient.id << " " << patient.name << " " << patient.age << " " << patient.bloodGroup << endl;
    applyDiscount(patient);                   // RM400.00
}
```
**Q6 planets**：`enum Planet {MERCURY, VENUS, EARTH, MOON, MARS, JUPITER, SATURN, URANUS, NEPTUNE, PLUTO};`

**Q7 coffee menu**
```cpp
enum Coffee {ESPRESSO = 1, LATTE, CAPPUCCINO};      // 1, 2, 3 对应菜单编号
int choice, qty; double price = 0;
cout << "=== Coffee Shop Menu ===\n1. Espresso (RM 8)\n2. Latte (RM 10)\n3. Cappuccino (RM 12)\n";
cout << "Choose your coffee (1-3): "; cin >> choice;
cout << "Enter quantity: "; cin >> qty;
switch (Coffee(choice)) {
    case ESPRESSO:   price = 8;  break;
    case LATTE:      price = 10; break;
    case CAPPUCCINO: price = 12; break;
}
cout << "====> Total price = RM " << price * qty << endl;     // 2, 2 → RM 20
```
**Q8 traffic light**
```cpp
typedef int Light;
enum TrafficLight {RED = 1, YELLOW, GREEN};
Light choice; cin >> choice;
switch (TrafficLight(choice)) {
    case RED:    cout << "STOP"; break;
    case YELLOW: cout << "SLOW DOWN"; break;
    case GREEN:  cout << "GO"; break;
    default:     cout << "Invalid";
}
```
**Q9 cinema**
```cpp
enum AgeGroup {CHILD, TEEN, ADULT, SENIOR};
AgeGroup classifyGroup(int age) {
    if (age < 12) return CHILD;
    else if (age <= 17) return TEEN;
    else if (age <= 59) return ADULT;
    else return SENIOR;
}
double getPrice(AgeGroup g) {
    switch (g) {
        case CHILD: return 8;   case TEEN:   return 12;
        case ADULT: return 20;  case SENIOR: return 10;
    }
    return 0;
}
// main: int age; cin >> age; cout << "Price: RM" << getPrice(classifyGroup(age));   (65 → RM10)
```
**Q10 gear**
```cpp
enum Gear {PARK = 1, REVERSE, NEUTRAL, DRIVE};
string findGearAction(Gear g) {
    switch (g) {
        case PARK:    return "Car stopped";
        case REVERSE: return "Car moving backward";
        case NEUTRAL: return "Car idle";
        case DRIVE:   return "Car moving forward";
    }
    return "Invalid gear";
}
// main: int mode; cin >> mode; cout << findGearAction(Gear(mode));   (4 → Car moving forward)
```
（`PARK = 1` 是 extra：指定起始值，让菜单编号与 enum 值一致，免去 −1 的换算。若只用 slide 的写法，就用 `Gear(mode - 1)`。）

---

## Closing the loop

同事的 `if (d == 5)` 改写成 `if (favouriteCoffee == COTTI)`，一眼就懂。typedef 让**类型**有意义，enum 让**值**有意义；两者都不增加功能，却大幅提高可读性与可维护性。

下一章换一种思路：让 function **呼叫自己**来解决问题——**Recursion**。

---

## ⚠️ Where the slides mislead

| Slide | Slide 写 | 更准确 |
|---|---|---|
| p10 | `cons int SUN = 7;` | typo：`const` |
| p14 | `enum Program {RMM, RSD, RSF, RSD, REI, RIT};` 当例子 | RSD 重复 → 不合法 |
| p5 | long = 4 bytes | 依平台；64-bit Linux 为 8 |
| p12 | 使用 `vector<Passenger>` | 需要 `#include <vector>` |
| p15 | 只问 valid 与否 | 三个都 invalid（原因见 §3.1） |
| 档名 | "User-definedTypesNotyetready" | 讲师标示未完成，但已被考 |

---

## Term table

| English | 中文 | 一句话说明 |
|---|---|---|
| Simple type | 简单类型 | 值不可再分 |
| Primitive / built-in type | 基本 / 内建类型 | int, double… |
| User-defined type | 使用者定义类型 | typedef、enum、struct |
| typedef | 类型别名 | 不产生新类型 |
| Enumeration type (enum) | 列举类型 | 一组有序识别字 |
| Enumerator | 列举值 | enum 中的每个名字 |
| Explicit cast | 明确转型 | `Weekdays(1)` |
| Implicit type coercion | 隐式型别转换 | int → enum 不允许 |

---

## Cheat sheet

- `typedef ExistingType NewName;`（顺序：旧 → 新）；`typedef struct {…} Name;`
- `enum Name {A, B, C};` → A=0, B=1, C=2；变数 `Name v = B;`
- Enumerator：合法识别字（无空格、不以数字开头、不是 'A' 或数字）、同 scope 唯一。
- 空格名 → `ZUS_COFFEE`。
- `cout << enumVar` 印整数。
- int → enum 要 `Name(i)`；`v++`、`v = 1`、`v = v + 1` 都错。
- enum 可用在 switch case、function 回传/参数。

---

## Practice (answers included)

### A. MCQ
1. `enum Size {S, M, L, XL}; Size s = L; cout << s;` prints  (a) L (b) 2 (c) 3 (d) error
2. Which is valid?  (a) `enum Grade {'A','B'};` (b) `enum Level {1, 2, 3};` (c) `enum Level {LOW, MID, HIGH};` (d) `enum Level {LOW MID};`
3. `typedef double Money;` means  (a) Money is a new type different from double (b) Money is another name for double (c) Money is a variable (d) error
4. Given `enum Day {MON, TUE}; Day d = MON;` which is valid?  (a) `d = 1;` (b) `d++;` (c) `d = Day(1);` (d) `d = d + 1;`
5. `enum A {X, Y}; enum B {Y, Z};` is  (a) valid (b) invalid — Y repeated (c) valid with warning (d) invalid — enums need values

**Answers:** 1-b, 2-c, 3-b, 4-c, 5-b

### B. Short answer
**B1.** Define `enum Payment` for Cash, Credit Card, E-Wallet and DuitNow; declare `method` initialised to E-Wallet. → `enum Payment {CASH, CREDIT_CARD, E_WALLET, DUITNOW}; Payment method = E_WALLET;`
**B2.** Use typedef to define `Kilogram` and `Metre` as double, then compute BMI = kg / (m × m).
```cpp
typedef double Kilogram; typedef double Metre;
Kilogram w; Metre h; cin >> w >> h; cout << w / (h * h);
```

### C. Application
**C1.** Write a function `nextDay` that receives a `Weekday` (MON..SUN) and returns the next day (SUN → MON).
```cpp
enum Weekday {MON, TUE, WED, THU, FRI, SAT, SUN};
Weekday nextDay(Weekday d) { return (d == SUN) ? MON : Weekday(d + 1); }
```
**C2.** Write a switch that prints the name of a `Cafe` value (for display, since `cout` prints numbers).
```cpp
switch (favouriteCoffee) {
    case ZUS_COFFEE: cout << "Zus Coffee"; break;
    case COTTI:      cout << "Cotti"; break;
    // ...
}
```

### D. Thinking
**D1.** Why use enum instead of `const int` constants? — enum groups the related values into one type, so the compiler can prevent assigning an unrelated int (type safety) and the code documents the allowed values.
**D2.** typedef does not create a new type — so what is its benefit? — Readability and maintainability: changing `typedef float Money;` to `double` updates every Money variable in one place.

---

## Slide index
| Note section | Slides |
|---|---|
| 1 Simple & primitive types | p2–5 |
| 2 typedef | p6–12 |
| 3 enum | p13–27 |
| 4 Tutorial 8 | Tutorial 8 |

## Links to other chapters
- struct → **Ch3**；typedef struct in global (coupling example) → **Ch11**
- Functions returning values → **Ch5**
- `vector<Passenger>` → **Ch1**
