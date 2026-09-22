# AMCS2123 — Chapter 2: Advanced String Functions

> Slide 列了 8 组函数，每组一两个例子，但：① regex 的 pattern 语法表是图片，文字版完全看不到；② `regex_match` 和 `regex_search` 的差别只各写一个例子，而 **Oct 2025 Q2a(ii)** 正是考这个差别（"abc123?" 为什么 No match）；③ 几个 slide 例子本身有 bug：`transform` 写进 4 个空格的字串会越界、`format("{:.2f }")` 多一个空格会 compile error、注释写 `5 * 3.14` 但 code 印的是 `x`。这份笔记所有例子都在 g++ 13（C++20）跑过。

---

## 0. 一句话总览

**数字 ↔ 字串用 to_string / stoi / stod / stringstream；在字串里找字元用 find_first_of 家族；整串改大小写用 transform；验证或抽取"格式"用 regex（match = 整串、search = 部分、replace = 取代）；排版输出用 C++20 的 format。**

| Part | 内容 | Slide |
|---|---|---|
| 0 | Recap：length, substr, find, insert, erase, replace… | p2 |
| 1–2 | `to_string`, `stoi`, `stod`, `stof` | p3 |
| 3–4 | `find_first_of`, `find_last_of`, `find_first_not_of`, `find_last_not_of` | p4 |
| 5 | `stringstream` | p5–6 |
| 6 | `transform` + `::toupper` / `::tolower` | p7–9 |
| 7 | **regex**：`regex_match`, `regex_search`, `regex_replace`, `smatch`，pattern 语法 | p10–16 |
| 8 | `format`（C++20） | p17–24 |

🎯 **这章回答的考题**
- **Oct 2025 Q2a(i)** 解释 `stod` + 例子 (2)
- **Oct 2025 Q2a(ii)** `regex_match("abc123?", "[a-z]+\\d+")` 的结果与原因 (2)
- **Jan 2026 Q3c(i)** 把 `ABC1234567` 变成 `ABC-1234(567)` (4)
- **Jan 2026 Q3c(ii)** `regex_search` 的用途 (1)

**Prerequisites**：Ch1 的 array/index 概念（string 的每个字元也用 index 从 0 开始）；基本 string 函数。

---

## Scene：乱七八糟的输入

你在帮一家电商写后台。资料都是字串：
- 产品编号 `ABC1234567`，老板要显示成 `ABC-1234(567)`；
- 价格从档案读进来是 `"128.50"`，要加税就必须先变成数字；
- 用户输入的 IP、电话、密码格式要检查对不对；
- 报表要对齐。

每一个问题对应本章一组函数。

---

## 0'. Recap：基本 string 函数（p2）

| Function | 作用 | 例子（`s = "Hello world"`） |
|---|---|---|
| `length()` / `size()` | 字元数 | 11 |
| `empty()` | 是否空字串 | false |
| `at(i)` / `s[i]` | 第 i 个字元 | `s.at(1)` = 'e' |
| `substr(pos, len)` | 从 pos 取 len 个 | `s.substr(6,5)` = "world" |
| `find(str)` | 第一次出现的 index；找不到回传 `string::npos` | `s.find("o")` = 4 |
| `insert(pos, str)` | 在 pos 插入 | `s.insert(5, ",")` → "Hello, world" |
| `erase(pos, len)` | 删除 | `s.erase(0,6)` → "world" |
| `replace(pos, len, str)` | 取代 | `s.replace(0,5,"Hi")` → "Hi world" |
| `append(str)` / `+` | 串接 | `s + "!"` |
| `==`, `<`, `>` / `compare` | 比较 | |
| `getline(cin, s)` | 读整行（含空格） | |

**`substr(pos, len)` 是本章最常用的**：第一个参数是**起点 index**，第二个是**长度**（不是终点）。

### ✅ 满分答法 — Jan 2026 Q3c(i): Convert `ABC1234567` to `ABC-1234(567)` and save in `code`. (4 marks)

先画出 index：
```
  A  B  C  1  2  3  4  5  6  7
  0  1  2  3  4  5  6  7  8  9
  └─ substr(0,3) ─┘ └ substr(3,4) ┘ └ substr(7,3) ┘
       "ABC"           "1234"          "567"
```
```cpp
string raw = "ABC1234567";
string code = raw.substr(0, 3) + "-" + raw.substr(3, 4) + "(" + raw.substr(7, 3) + ")";
// code = "ABC-1234(567)"
```
另一种写法（用 `insert`）：
```cpp
string code = "ABC1234567";
code.insert(3, "-");     // ABC-1234567   (index 3 前插入)
code.insert(8, "(");     // ABC-1234(567) 的前半（插入后字串变长，所以是 8 不是 7）
code += ")";             // ABC-1234(567)
```
两种都 g++ 验证输出 `ABC-1234(567)`。评分：用对 string function (2)、位置正确 (1)、存进 `code` (1)。

⚠️ 用 insert 时要注意：**每插入一次，后面的 index 就往后移**。所以第二次插入的位置是 7+1 = 8。

---

## 1. 数字 ↔ 字串（p3）

| Function | 方向 | 例子 |
|---|---|---|
| `to_string(n)` | 数字 → string | `to_string(123)` → `"123"` |
| `stoi(s)` | **s**tring **to** **i**nt | `stoi("123")` → 123 |
| `stod(s)` | string to **d**ouble | `stod("12.75")` → 12.75 |
| `stof(s)` | string to **f**loat | `stof("3.5")` → 3.5f |

**为什么需要？** `"128.50" + 1` 不是 129.5——字串不能做算术。要先 `stod`。

### ✅ 满分答法 — Oct 2025 Q2a(i): Explain `stod` and show an example. (2 marks)
`stod()` (string to double) converts a string containing a numeric value into a value of type `double`, so that it can be used in calculations. (1)
```cpp
string priceStr = "12.75";
double price = stod(priceStr);   // price = 12.75
cout << price + 1;               // 13.75
```
(1)

### Tutorial 2 Q1：电话号码（已执行）
```cpp
int64_t phone = 60123456789;            // 超过 int 范围，所以用 64-bit 整数
string s = to_string(phone);            // "60123456789"
cout << "+" + s.substr(0,2) + " " + s.substr(2,2) + "-" + s.substr(4);
// +60 12-3456789
```

---

## 2. find_first_of 家族（p4）

`find` 找的是**整个子字串**；`find_first_of` 找的是**字元集合里任何一个字元**。

| Function | 找什么 | `"Hello world"` 的例子 |
|---|---|---|
| `find_first_of("aeiou")` | 第一个出现的**任一**母音 | 1（'e'） |
| `find_last_of("aeiou")` | 最后一个出现的任一母音 | 7（world 的 'o'） |
| `find_first_not_of("Hel")` | 第一个**不在**集合里的字元 | 4（'o'） |
| `find_last_not_of(chars)` | 最后一个不在集合里的字元 | |

### Tutorial 2 Q3（已执行）
```cpp
string t = "Two dimensional array";
cout << t.find_first_of("aeiouAEIOU");   // 2  ('o' in Two)
cout << t.find_last_of("aeiouAEIOU");    // 19 ('a' in array)
```

---

## 3. stringstream（p5–6）

`#include <sstream>`。把字串当成 `cin` 一样**用 >> 拆开**，或当成 `cout` 一样**用 << 组合**。

```cpp
string text = "10 25.78 Hello";
int n; float m; string str;
stringstream ss(text);
ss >> n >> m >> str;              // 以空白分隔：10 | 25.78 | Hello
cout << n << " " << m << " " << str;   // 10 25.78 Hello

stringstream ss2;
ss2 << 45;                        // 把数字写进去
cout << ss2.str();                // "45"
```

### Tutorial 2 Q2：逗号分隔的数字求和（已执行）
```cpp
string data = "45.67, 23, 12.5, 10, 78.9";
stringstream ss(data);
string item;
double total = 0;
while (getline(ss, item, ','))    // 以 ',' 为分隔符读每一段
    total += stod(item);          // stod 会忽略前面的空白
cout << total;                    // 170.07
```

---

## 4. transform（p7–9）

`#include <algorithm>`。语法：`transform(begin, end, destination, function);`
- `begin, end`：要处理的范围；`destination`：结果写到哪里；`function`：`::toupper` / `::tolower`。

```cpp
string text1 = "Hello world!";
transform(text1.begin(), text1.end(), text1.begin(), ::toupper);
cout << text1;        // HELLO WORLD!
transform(text1.begin()+6, text1.end(), text1.begin()+6, ::tolower);
cout << text1;        // HELLO world!   (index 6 起才改)
```

⚠️ **Slide p9 的 bug**：
```cpp
string text2 = "    ";   // 只有 4 格
transform(text1.begin()+1, text1.end(), text2.begin(), ::tolower);  // 要写 11 个字元！
```
来源有 11 个字元，但 `text2` 只有 4 格 → **写出界 (undefined behaviour)**，slide 说输出 "ello" 是碰巧/错误的。正确做法：destination 必须够大，例如 `string text2(text1.size() - 1, ' ');`，结果是 `"ello world!"`（g++ 验证）。

### Tutorial 2 Q4（已执行）
目标：`"henry ford, good morning! Welcome to the HILTON HOTEL."` → `"Mr. HENRY FORD, good morning! Welcome to the Hilton Hotel."`
```cpp
string msg = "henry ford, good morning! Welcome to the HILTON HOTEL.";
transform(msg.begin(), msg.begin() + 10, msg.begin(), ::toupper);  // HENRY FORD
size_t h = msg.find("HILTON");
transform(msg.begin()+h+1, msg.begin()+h+6, msg.begin()+h+1, ::tolower);  // Hilton
transform(msg.begin()+h+8, msg.end()-1,   msg.begin()+h+8, ::tolower);    // Hotel（保留句点）
msg.insert(0, "Mr. ");
cout << msg;
```

---

## 5. Regex（p10–16）⭐

**Regular expression (regex)** = 在字串中做**模式比对 (pattern matching)** 的工具：定义一个搜寻模式，用来**找 (find)、验证 (validate)、处理 (manipulate)** 文字。`#include <regex>`。

### 5.1 四个常用成员（p11，图片内容）

| Function | Purpose |
|---|---|
| `regex_match` | 检查**整个字串**是否完全符合 pattern |
| `regex_search` | 检查字串中**任何一部分**是否符合 |
| `regex_replace` | 把符合的部分**取代** |
| `smatch` | 储存 search 的比对结果（`match[0]` 整段，`match[1]` 第一个括号…） |

### 5.2 Pattern 语法（p12，图片内容）

| Pattern | 意思 | 例子 |
|---|---|---|
| `\\d` | 一个数字 0–9 | `\\d\\d` 配 "42" |
| `\\w` | 一个 word character（字母、数字、_） | |
| `.` | 任何一个字元 | |
| `*` | 前一项 **0 次或以上** | `a*` |
| `+` | 前一项 **1 次或以上** | `\\d+` 配 "2025" |
| `^` | 字串开头 | |
| `$` | 字串结尾 | |
| `[a-z]` | 集合中的一个字元（extra） | 小写字母 |
| `{m,n}` | 前一项 m 到 n 次（extra） | `\\d{1,3}` |
| `\\.` | 真正的句点（extra） | |
| `( )` | 分组，给 smatch 抽取（extra 说明） | |

**为什么要写两个反斜线 `\\d`？** 在 C++ 字串里 `\` 是 escape 字元，`"\\d"` 才会变成 regex 看到的 `\d`。

### 5.3 match vs search：最常考的差别

| | `regex_match` | `regex_search` |
|---|---|---|
| 比对范围 | **整个字串**从头到尾都要符合 | 字串中**只要有一段**符合就 true |
| 用途 | **验证**格式（IP、密码、电话） | **寻找/抽取**资料（在句子里找金额） |
| `"My unit is A-1-25"` vs `\\d+` | false | true |

> 💬 **答题句 (EN)** — *regex_match returns true only if the entire string matches the pattern, so it is used for validation; regex_search returns true if any part of the string matches the pattern, so it is used to find or extract matching text.*

### ✅ 满分答法 — Jan 2026 Q3c(ii): What is the use of `regex_search`? (1 mark)
`regex_search` searches a string and returns true if **any part** of the string matches the given regex pattern (optionally storing the matched parts in an `smatch` object).

### ✅ 满分答法 — Oct 2025 Q2a(ii): Output and reason (2 marks)
```cpp
string text = "abc123?";
regex pattern("[a-z]+\\d+");
if (regex_match(text, pattern)) cout << "Full match!";
else                            cout << "No match!";
```
**Output: `No match!`** (1)
**Reason:** `regex_match` requires the **whole** string to match. The pattern `[a-z]+\d+` matches one or more lowercase letters followed by one or more digits ("abc123"), but the string ends with an extra character **`?`**, which is not covered by the pattern, so the full match fails. (1)
（g++ 验证：`regex_match` → false；若改用 `regex_search` → true。）

### 5.4 Slide 的四个例子（已执行）
```cpp
// a) regex_search — find a pattern
string text = "My unit number is A-1-25";
regex pattern1("\\d+");                       // one or more digits
if (regex_search(text, pattern1)) cout << "Number found!";      // ✔

// b) regex_match — the whole string
string text2 = "abc123";
regex pattern2("[a-z]+\\d+");
if (regex_match(text2, pattern2)) cout << "Full match!";        // ✔

// c) regex_replace
string pwd = "abc123";
string result = regex_replace(pwd, pattern2, "***");            // "***"
cout << "Password entered " << result << " is correct!";

// d) regex_search + smatch — extract parts
string t = "David is 18 years old";
regex twoPatterns("(\\w+) is (\\d+) years old");
smatch match;
if (regex_search(t, match, twoPatterns)) {
    cout << match[0] << endl;              // David is 18 years old
    cout << "Name is " << match[1] << endl; // David
    cout << "Age is "  << match[2] << endl; // 18
}
```

### Tutorial 2 Q6：从简讯抽出金额（regex_search，已执行）
```cpp
string sms = "Bill amount: RM128.50";
smatch m;
if (regex_search(sms, m, regex("RM\\d+\\.\\d{2}")))
    cout << "Please make the payment " << m[0] << " by the due date given in the bill";
```

### Tutorial 2 Q7：验证 IP（regex_match，已执行）
```cpp
string ip;
cout << "Enter IP address: "; cin >> ip;
regex ipPattern("\\d{1,3}\\.\\d{1,3}\\.\\d{1,3}\\.\\d{1,3}");
if (regex_match(ip, ipPattern)) cout << "Valid IP address";
else                             cout << "Invalid IP address";
// 192.168.10.2 → Valid；192.168.1 → Invalid；1234.1.1.1 → Invalid
```
（只检查"1–3 位数字"的格式，按题目要求；不检查每段 ≤ 255。）

---

## 6. format()（p17–24，C++20）

`#include <format>`；要在 Visual Studio 把 **C++ Language Standard** 设成 **ISO C++20**（Project → Properties → General → C++ Language Standard，p19–20 的截图）。g++ 用 `-std=c++20`。

| 用途 | Format code | 结果 |
|---|---|---|
| 插入变数 | `"Hello, {}"` | Hello, Lionel |
| 小数位 | `"{:.2f}"` | 3.14 |
| 补零 | `"{:05}"` | 00068 |
| 靠右 / 靠左 | `"{:>6}"` / `"{:<6}"` | `    68` / `68    ` |
| 多个变数 | `"{} is {} years old"` | Lionel is 20 years old |

```cpp
string name = "Lionel"; int age = 20;
cout << format("Hello, {} !", name);                 // Hello, Lionel !
cout << format("{} is {} years old", name, age);     // Lionel is 20 years old
cout << format("{} x {:.2f} = {:.2f}", 5, 3.14159, 5*3.14159);   // 5 x 3.14 = 15.71
cout << format("Zero padded: {:05}", 68);            // Zero padded: 00068
```
⚠️ Slide p22 写 `"{:.2f }"`（`f` 后多一个空格）→ **compile error**（format 字串不合法）；注释写 `5 * 3.14`，但 code 印的是 `x`。正确输出：`5 x 3.14 = 15.71`。

### Slide p25 练习：圆面积表（已执行）
```cpp
vector<string> items = {"coin","ring","wheel"};
vector<double> radius = {0.5, 0.7, 21.6};
const double PI = 3.14159;
cout << format("{:<8}{:<8}{}\n", "Items", "Radius", "Area");
for (size_t i = 0; i < items.size(); i++)
    cout << format("{:<8}{:<8}{:.2f}cm^2\n", items[i], radius[i], PI*radius[i]*radius[i]);
// coin 0.5 0.79 / ring 0.7 1.54 / wheel 21.6 1465.74  ← 与 slide 数字一致
```

### Tutorial 2 Q5：销售报表（已执行）
```cpp
vector<string> member = {"Howard Goh","Carrie Hank","Darren Smith"};
vector<double> sales  = {68950.35, 46900.20, 72195.75};
cout << format("Team: {}\nMonth: {}\n", "Great", "June");
cout << format("{:<15}{:>12}\n", "Member", "Sales (RM)");
for (size_t i = 0; i < member.size(); i++)
    cout << format("{:<15}{:>12.2f}\n", member[i], sales[i]);
```

---

## Closing the loop

电商后台的四个问题：产品编号用 `substr`/`insert` 重组；价格用 `stod` 变数字；格式验证用 `regex_match`、抽资料用 `regex_search`；报表用 `format` 对齐。

下一章处理"一笔资料有很多栏位"的情况：**structure**——把学生的名字、ID、分数绑成一个整体。

---

## ⚠️ Where the slides mislead

| Slide | Slide 写 | 更准确 |
|---|---|---|
| p9 | `text2 = "    "`（4 格）当 transform 目的地，输出 "ello" | 目的地太小，越界写入（UB）；要先让 text2 有足够长度，结果是 "ello world!" |
| p22 | `"{:.2f }"`；注释 `5 * 3.14 = 15.71` | 多的空格会 compile error；code 印 `5 x 3.14 = 15.71` |
| p2 | "concatenante" | 拼字：concatenate |
| p23–24 | 两页内容相同 | 重复页 |
| p26 | Exercise 页空白 | 没有内容 |

---

## Term table

| English | 中文 | 一句话说明 |
|---|---|---|
| `to_string` | 数字转字串 | 123 → "123" |
| `stoi` / `stod` / `stof` | 字串转 int / double / float | "12.75" → 12.75 |
| `substr(pos, len)` | 子字串 | 起点 + 长度 |
| `find_first_of` | 找集合中第一个字元 | |
| `stringstream` | 字串流 | 用 >> 拆、<< 组 |
| `getline(ss, item, ',')` | 以分隔符读 | 拆 CSV |
| `transform` | 转换 | 配 `::toupper`/`::tolower` |
| Regular expression (regex) | 正规表达式 | 模式比对 |
| `regex_match` | 完全比对 | 整串要符合 |
| `regex_search` | 部分搜寻 | 有一段符合即可 |
| `regex_replace` | 取代 | |
| `smatch` | 比对结果 | `match[0]`, `match[1]`… |
| `format` | 格式化（C++20） | `{:.2f}`, `{:>6}` |

---

## Cheat sheet

- `s.substr(start, length)`；`s.insert(pos, str)` 后 index 会移动。
- `to_string` / `stoi` / `stod` / `stof`。
- `find_first_of("aeiou")` = 任一字元第一次出现。
- `stringstream ss(text); ss >> a >> b;` ；`while (getline(ss, item, ','))`。
- `transform(s.begin(), s.end(), s.begin(), ::toupper);`（`<algorithm>`）
- Regex：`\\d` 数字、`\\w` 字、`.` 任意、`*` 0+、`+` 1+、`^` 开头、`$` 结尾、`[a-z]`、`{1,3}`。
- **match = 整串（验证）；search = 部分（寻找/抽取）**；replace 取代；smatch 存结果。
- `format("{} {:.2f} {:05} {:>6} {:<6}", …)`（`<format>`，C++20）。

---

## Practice (answers included)

### A. MCQ
1. `string s = "Kuala Lumpur"; s.substr(6, 3)` returns  (a) "Lum" (b) "Lump" (c) " Lu" (d) "a L"
2. Which is `true`?  (a) `regex_match("A-1-25", regex("\\d+"))` (b) `regex_search("A-1-25", regex("\\d+"))` (c) `regex_match("abc", regex("\\d+"))` (d) none
3. `stoi("42") + 8` equals  (a) "428" (b) 50 (c) "50" (d) error
4. `format("{:04}", 7)` gives  (a) "7000" (b) "0007" (c) "   7" (d) "7   "
5. Which header is needed for `transform`?  (a) `<string>` (b) `<sstream>` (c) `<algorithm>` (d) `<regex>`

**Answers:** 1-a, 2-b, 3-b, 4-b, 5-c

### B. Short answer
**B1.** Given `string d = "2025-10-08";`, extract year, month and day as integers.
```cpp
int year  = stoi(d.substr(0, 4));   // 2025
int month = stoi(d.substr(5, 2));   // 10
int day   = stoi(d.substr(8, 2));   // 8
```
**B2.** Write a regex to validate a Malaysian car plate of 1–3 uppercase letters followed by 1–4 digits (e.g. `VNT5129`), and state which function to use. → `regex plate("[A-Z]{1,3}\\d{1,4}");` with `regex_match` (the whole string must follow the format).
**B3.** What is printed? `regex_search(string("Order #A12 shipped"), regex("\\d+"))` → true (the substring "12" matches).

### C. Application
**C1.** A student ID `"25WMR12345"` must be displayed as `"25-WMR-12345"`.
```cpp
string id = "25WMR12345";
string out = id.substr(0,2) + "-" + id.substr(2,3) + "-" + id.substr(5);
```
**C2.** Extract the name and age from `"Aisyah is 21 years old"` using smatch → use the slide p16 pattern `"(\\w+) is (\\d+) years old"`; `match[1]` = Aisyah, `match[2]` = 21 (`stoi(match[2])` to get an int).
**C3.** Convert a user's full name to uppercase before saving.
```cpp
string name; getline(cin, name);
transform(name.begin(), name.end(), name.begin(), ::toupper);
```

### D. Thinking
**D1.** Why is `regex_match` preferred for validating a password format? — Because the entire input must conform; `regex_search` would accept a password that merely *contains* a valid part plus invalid characters.
**D2.** Why do regex patterns in C++ use `\\d` instead of `\d`? — In a C++ string literal, `\` is an escape character, so `\\` produces a single backslash that the regex engine then reads as `\d`.

---

## Slide index
| Note section | Slides |
|---|---|
| 0' Recap | p2 |
| 1 Conversion | p3 |
| 2 find_first_of | p4 |
| 3 stringstream | p5–6 |
| 4 transform | p7–9 |
| 5 regex | p10–16 |
| 6 format | p17–25 |

## Links to other chapters
- Strings inside structures (`string name`) and changing one letter (`deptName[0] = 'S'`) → **Ch3**
- Passing strings / char arrays to functions → **Ch6**
- Template that displays a formatted message → **Ch7**
- Recursive `countChar(str, ch, index)` → **Ch9**
