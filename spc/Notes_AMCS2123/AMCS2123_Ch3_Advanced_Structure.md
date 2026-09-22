# AMCS2123 — Chapter 3: Advanced Structure (Record)

> 这一章在两份考卷都拿走 **约 20 分**（Oct 2025 Q2b 21 分、Jan 2026 Q1c + Q2a 20 分），题型固定："define struct → declare variable → 改某一格 → nested struct → array of struct → 改某人某天某格"。Slide 把 **array in struct**、**struct in array**、**struct in struct** 分三段讲，却从来没把三者**放在一起比较**，也没教你怎么一层一层写出 `siblings[1].mealExpenses.dinner[5]` 这种"点点点括号括号"的存取路径。另外 slide p18 用 `strcpy(employee[0].deptID, "IT999")`，但 struct 里的栏位叫 `deprtID` 而且是**单一 char**——这行不能 compile。这份笔记补一个"由外往内读"的存取规则，所有答案都 g++ 跑过。

---

## 0. 一句话总览

**Struct 把不同类型的栏位绑成一笔记录；用 `.` 存取栏位；栏位本身可以是 array（array in struct）、struct（nested struct）；也可以把很多笔记录放进 array（array of struct）。存取路径永远是"由外往内：变数 → [第几笔] → .栏位 → [第几格]"。**

| Part | 内容 | Slide |
|---|---|---|
| 1 | Struct recap：定义、宣告、`.` 存取、初始化、整体 assignment、I/O | p2–11 |
| 2 | **Arrays in structs** | p12–15 |
| 3 | **Structs in arrays**（array of structure） | p16–19 |
| 4 | **Structs in structs**（nested structure） | p20–24 |

🎯 **这章回答的考题**
- **Jan 2026 Q1c** Array in structure vs array of structure + code (5)
- **Jan 2026 Q2a** `Expenses`（3 个 7 天 array）→ 宣告 → 改星期四午餐 → nested `Siblings` → 5 个 siblings 的 array → 第二个 sibling 星期六晚餐 +5.85（3+2+2+2+2+4 = 15）
- **Oct 2025 Q2b** `Mark`（3 科 × 10 学生）→ 宣告 → 输入全部 → 改第 7 位学生 Science → `Faculty` → nested → 宣告 `marks` → 改 deptName 第一个字母（3+2+6+2+2+2+1+3 = 21）
- 另外 **Oct 2025 Q4d / Jan 2026 Q4c**（把 struct 传进 function）→ 见 **Ch6**

**Prerequisites**：Ch1 array（index 从 0；第 n 个 = n-1）；Ch2 string（`name[0]` 是第一个字母）。

---

## Scene：陈家五兄妹的伙食费

陈家有 5 个兄弟姐妹，妈妈要记录每个人一星期 7 天的早、午、晚餐花费。

- 一个人的一餐一周有 7 个数字 → **array**；
- 一个人有三餐 → 把三个 array 放进一个 **struct Expenses**（array **in** struct）；
- 每个人还有名字 → 另一个 struct **Siblings**，里面包着 Expenses（struct **in** struct）；
- 五个人 → `Siblings siblings[5]`（array **of** struct）。

这就是 Jan 2026 Q2a 的完整剧情。先从最基本的 struct 开始。

---

## 1. Struct recap（p2–11）

- **Struct**：一组**固定数量**的元件，用**一个名字**存取；元件可以是**不同类型**；元件叫 **members**；`struct` 是保留字。

```cpp
struct studentType {          // ← 定义一个 structure（像设计图，不占记忆体）
    char firstName[10];
    char lastName[10];
    char courseGrade;
    int testScore;
    int programmingScore;
    double GPA;
};                            // ← 分号不能漏！
studentType newStudent, student;   // ← 宣告两个 structure 变数（真的占记忆体）
```

```
 newStudent                    student
 ┌──────────────┬────────┐     ┌──────────────┬────────┐
 │ firstName    │        │     │ firstName    │        │
 │ lastName     │        │     │ lastName     │        │
 │ courseGrade  │        │     │ courseGrade  │        │
 │ testScore    │        │     │ testScore    │        │
 │ programmingSc│        │     │ programmingSc│        │
 │ GPA          │        │     │ GPA          │        │
 └──────────────┴────────┘     └──────────────┴────────┘
```
*看图重点：同一个 struct 类型宣告出的每个变数都有**一整套自己的栏位**。*

**存取**：`structVariable.memberName`（`.` 叫 **member access operator**）
```cpp
strcpy(newStudent.firstName, "Dennis");   // char array 不能用 = 直接给字串
newStudent.courseGrade = 'B';
newStudent.testScore = 100;
cin >> newStudent.GPA;
cout << newStudent.lastName;
```
⚠️ `newStudent.firstName = "Dennis";` **不行**，因为 `firstName` 是 **char array**。若栏位宣告成 `string firstName;`，那 `=` 就可以——考试题目用 `string` 时直接 `=`。

**初始化**：按栏位顺序
```cpp
studentType newStudent = {"Lily", "Ng", 'A', 70, 88, 3.20};
```

**整体 assignment（p9–10）**：同类型的 struct 变数可以整个复制：`student = newStudent;`（等于把每个栏位逐一复制）。

**I/O（p11）**：**不能** `cin >> newStudent;` 或 `cout << newStudent;`，必须**一个栏位一个栏位**读写。

---

## 2. Arrays in structs（p12–15）

**栏位本身是一个 array**：一笔记录里有一整排资料。

```cpp
struct listType {
    int listCode[1000];   // array member
    int listLength;
};
listType intList;
```
```
 intList
 ┌────────────┬─────────────────────────────┐
 │ listCode   │ [0] [1] [2] ... [999]       │  ← 一个变数里面有 1000 格
 │ listLength │                             │
 └────────────┴─────────────────────────────┘
```
```cpp
intList.listLength = 0;
intList.listCode[0] = 12;            // 变数.栏位[格]
intList.listCode[1] += 5;
for (int i = 0; i < 1000; i++) cin >> intList.listCode[i];
cout << intList.listCode[10];        // 第 11 个
```

### Slide p15 练习（已执行）
```cpp
struct Student {
    string name;
    int year;
    float cw[5];                   // 5 coursework marks
};
Student s;
cout << "Name: ";  getline(cin, s.name);
cout << "Year: ";  cin >> s.year;
float total = 0;
for (int i = 0; i < 5; i++) {
    cout << "Coursework " << i+1 << ": ";
    cin >> s.cw[i];
    total += s.cw[i];
}
cout << s.name << " (Year " << s.year << ") Total = " << total
     << ", Average = " << total / 5 << endl;
```

---

## 3. Structs in arrays（array of structure，p16–19）

**Array 的每一格是一整笔 struct 记录**：很多个人，每个人一套栏位。

```cpp
struct employeeType {
    char firstName[10], lastName[10];
    int personID;
    char deptID[10];            // ← 见下方 ⚠️
    double yearlySalary, monthlySalary, yearToDatePaid, monthlyBonus;
};
employeeType employee[50];
```
```
 employee
  [0] ─┐
  [1]  │   each cell is a whole record:
  [2] ─┼──>  employee[2]: firstName | lastName | personID | deptID |
  ...  │                   yearlySalary | monthlySalary | yearToDatePaid | monthlyBonus
  [49]─┘
```
*看图重点：先用 `[i]` 选**第几个人**，再用 `.` 选**哪个栏位**。*

```cpp
strcpy(employee[0].deptID, "IT999");      // 第 1 个员工
employee[19].monthlySalary += 1200.00;    // 第 20 个员工
employee[49].yearToDatePaid = 84500.00;   // 最后一个
for (int i = 0; i < 50; i++) cin >> employee[i].monthlyBonus;
cout << employee[35].firstName[4];        // 第 36 个员工名字的第 5 个字母
```
⚠️ **Slide p16/p18 的错**：struct 里宣告的是 `char deprtID;`（拼错 + 单一 char），但 p18 用 `strcpy(employee[0].deptID, "IT999")`——栏位名不存在、而且单一 char 装不下字串，**compile error**。要改成 `char deptID[10];`（如上）。

### Slide p19 练习：出勤（已执行）
```cpp
struct Employee { int empID; string name; int daysPresent; };
Employee Employees[3];
for (int i = 0; i < 3; i++) {
    cout << "ID, name, days present: ";
    cin >> Employees[i].empID;
    cin.ignore();                              // 丢掉 ID 后的换行
    getline(cin, Employees[i].name);
    cin >> Employees[i].daysPresent;
}
for (int i = 0; i < 3; i++)
    cout << Employees[i].empID << "  " << Employees[i].name << "  "
         << Employees[i].daysPresent * 100.0 / 24 << "%" << endl;   // 24 working days
```

---

## 4. Structs in structs（nested structure，p20–24）

**栏位本身是另一个 struct**：把大记录拆成小类别。

```cpp
struct nameType    { char first[10], middle[10], last[10]; };
struct dateType    { int month, day, year; };
struct addressType { char address1[20], address2[20], city[15], state[12]; int zip; };
struct contactType { char phone[15], cellphone[15], fax[12], pager[12], email[30]; };

struct employeeType {
    nameType    name;
    char        empID[10];
    addressType address;
    dateType    hireDate, quitDate;
    contactType contact;
    char        deptID[10];
    double      salary;
};
employeeType newEmployee;
```
⚠️ Slide p20 的四个 struct 结尾**都漏了分号** `};`——照抄会 compile error。

**存取：一路点下去**
```cpp
strcpy(newEmployee.name.first, "John");    // 变数.子struct.栏位
newEmployee.address.zip = 53300;
cin >> newEmployee.hireDate.month;
cout << newEmployee.contact.email;
```
**规则**：内层的 struct 必须**先定义**（写在外层前面），否则 compiler 不认识这个类型。

### Slide p24 练习：车主与车（已执行）
```cpp
struct Car   { string brand, model; int yearMade; };
struct Owner { string name; int ownerIC; Car car; };   // nested
Owner owner[3];                                        // array of nested struct
for (int i = 0; i < 3; i++) {
    cout << "Owner name: ";        getline(cin, owner[i].name);
    cout << "IC: ";                cin >> owner[i].ownerIC;
    cout << "Brand model year: ";  cin >> owner[i].car.brand >> owner[i].car.model >> owner[i].car.yearMade;
    cin.ignore();
}
for (int i = 0; i < 3; i++)
    cout << owner[i].name << " | " << owner[i].ownerIC << " | "
         << owner[i].car.brand << " " << owner[i].car.model << " (" << owner[i].car.yearMade << ")\n";
```
⚠️ 马来西亚 IC 有 12 位数，`int` 装不下（最大约 21 亿）——实际应该用 `string ownerIC;`。题目指定 int 时照做，但知道这个限制。

---

## 5. 存取路径规则 + 三者比较（slide 没有，考试必用）

### 5.1 由外往内读

> **变数名 → `[第几笔]`（若是 array of struct）→ `.栏位` → `.子栏位`（若 nested）→ `[第几格]`（若栏位是 array）**
> 每遇到一个 array 就加 `[ ]`，每进入一个 struct 就加 `.`。

例：**第二个 sibling 的星期六晚餐**
```
siblings      [1]          .mealExpenses        .dinner      [5]
 array of    第 2 个人       nested struct         array       Saturday
 Siblings                   (Expenses)            member      (Mon=0)
```
→ `siblings[1].mealExpenses.dinner[5]`

### 5.2 Array in structure vs Array of structure

| | **Array in structure** | **Array of structure** |
|---|---|---|
| 意思 | struct 的**某个栏位是 array** | 一个 **array，每格是一个 struct** |
| 几笔记录 | **一笔**记录里有一排值 | **很多笔**记录 |
| 存取 | `var.member[i]` | `arr[i].member` |
| `[]` 在哪 | **在后面**（栏位之后） | **在前面**（变数之后） |
| 例子 | 一个学生的 5 次作业分数 | 50 个员工，每人一套资料 |

> 💬 **答题句 (EN)** — *An array in a structure means one member of the structure is an array, so a single record holds a list of values (accessed as var.member[i]). An array of structures means each element of the array is a whole structure record, so the array holds many records (accessed as arr[i].member).*

### ✅ 满分答法 — Jan 2026 Q1c: Difference between array in structure and array of structure, with C++ code. (5 marks)
**Array in structure (2½):** one member of a structure is itself an array, so a single structure variable stores a list of values of that member.
```cpp
struct Student {
    string name;
    int marks[5];            // array inside the structure
};
Student stud;
stud.marks[2] = 80;          // 3rd mark of ONE student → var.member[index]
```
**Array of structure (2½):** an array whose every element is a complete structure record, so it stores many records of the same structure type.
```cpp
struct Employee {
    string name;
    double salary;
};
Employee emp[50];            // 50 employee records
emp[4].salary = 3500;        // salary of the 5th employee → arr[index].member
```

---

## 6. 过去考题完整满分答法

### ✅ Jan 2026 Q2a（陈家伙食费，15 marks）— g++ 已 compile
```cpp
// (i) Define structure Expenses (3 marks)
//     each meal stores 7 days; index 0 = Monday ... 6 = Sunday
struct Expenses {
    double breakfast[7];
    double lunch[7];
    double dinner[7];
};

// (ii) Declare variable expenses (2 marks)
Expenses expenses;

// (iii) Lunch on Thursday (index 3) = 12.95 (2 marks)
expenses.lunch[3] = 12.95;

// (iv) Structure Siblings with name and nested mealExpenses (2 marks)
struct Siblings {
    string name;
    Expenses mealExpenses;     // nested structure (Expenses must be defined before)
};

// (v) Array for 5 siblings of the Chan family (2 marks)
Siblings siblings[5];

// (vi) Add 5.85 to Saturday dinner of the 2nd sibling (4 marks)
siblings[1].mealExpenses.dinner[5] += 5.85;
//       ↑ 2nd sibling  ↑ nested    ↑ dinner ↑ Saturday
```
评分重点：(i) 三个 `double` array 各 7 格；(iii) Thursday = **3**；(vi) 四个部分各 1 分：`siblings[1]`、`.mealExpenses`、`.dinner[5]`、`+= 5.85`。

### ✅ Oct 2025 Q2b（成绩 + 学院，21 marks）— g++ 已 compile
```cpp
// (i) Define structure Mark (3 marks): each subject stores marks of 10 students
struct Mark {
    int math[10];
    int science[10];
    int English[10];
};

// (ii) Declare variable mark (2 marks)
Mark mark;

// (iii) Prompt for all marks of the 10 students (6 marks)
for (int i = 0; i < 10; i++) {
    cout << "Student " << i + 1 << endl;
    cout << "  Math   : ";  cin >> mark.math[i];
    cout << "  Science: ";  cin >> mark.science[i];
    cout << "  English: ";  cin >> mark.English[i];
}

// (iv) Science of the 7th student = 95 (2 marks)
mark.science[6] = 95;

// (v) Define structure Faculty (2 marks)
struct Faculty {
    string facultyName;
    string deptName;
};

// (vi) Rewrite Mark with nested member faculty (2 marks)
//      (Faculty must now be defined BEFORE Mark)
struct Mark {
    int math[10];
    int science[10];
    int English[10];
    Faculty faculty;
};

// (vii) Declare structure variable marks (1 mark)
Mark marks;

// (viii) Change first letter of deptName to 'S' (3 marks)
marks.faculty.deptName[0] = 'S';
```
说明：
- (iii) 6 分 = loop 10 位学生 (2) + 三科各 prompt & cin (3) + index 用对 (1)。
- (vi) 在真实程式里同一个名字 `Mark` 不能定义两次；考题要你"rewrite"，意思是**取代**原本的定义。实际写程式时只留新版本，并把 `Faculty` 放前面。
- (viii) `deptName` 是 string，`deptName[0]` 就是第一个字元，直接用 `'S'`（单引号，字元）。若题目用 `char deptName[20]`，写法完全一样。g++ 验证："Computing" → "Somputing"。

---

## 7. Tutorial 3 精选答案

> ⚠️ 文件夹中的 `Tutorial 3 - Advanced Structure (Student).pdf.crdownload` 是**未下载完成**的档案：文字有缺漏（例如 Q1(i) 的初始值只剩 "…150 and 20.0"）。以下按可读部分作答，缺的数值用合理假设并标明。

**Q1 House**（假设初始 area 为 "Kuta Damansara"，因为 (iii) 要把第 2 个字母改成 'o' 得到 "Kota Damansara"）
```cpp
struct House { char area[20]; int squareft; double rental; };
House hse = {"Kuta Damansara", 150, 20.0};    // (i) 初始值依题目（原档不完整）
hse.rental = 230.0;                           // (ii)
hse.area[1] = 'o';                            // (iii) 2nd letter → "Kota Damansara"
House hses[5];                                // (iv)
for (int i = 0; i < 5; i++) hses[i].rental = 30.0;          // (v)
for (int i = 0; i < 5; i++)                                   // (vi)
    cout << hses[i].area << "  " << hses[i].squareft << "  " << hses[i].rental << endl;
```

**Q2 Myfriends**（nested `Date dob`）
```cpp
schoolFriends.name[4] = 'G';                  // a) 5th character
schoolFriends.dob.year += 3;                  // b)
strcpy(schoolFriends.name, "Peter Chan");     // c) char array → strcpy
schoolFriends.dob.day = 15;
Myfriends schoolMates[20];                    // d)
cout << left << setw(15) << "Name" << "DOB\n";            // e)
for (int i = 0; i < 20; i++)
    cout << left << setw(15) << schoolMates[i].name
         << schoolMates[i].dob.day << "/" << schoolMates[i].dob.month << "/" << schoolMates[i].dob.year << endl;
```

**Q3 汽车维修（vector of nested struct）**
```cpp
struct Date { int day, month, year; };
struct Car { string plateNo, brand, model; int year; };
struct Customer { string name, contact; Car car; Date appointment; };
vector<Customer> customer;                                   // a) dynamic array
int n; cout << "How many customers? "; cin >> n;             // b)
for (int i = 0; i < n; i++) {
    Customer c;
    cin >> c.name >> c.contact >> c.car.plateNo >> c.car.brand >> c.car.model >> c.car.year
        >> c.appointment.day >> c.appointment.month >> c.appointment.year;
    customer.push_back(c);
}
customer[1].appointment = {20, 8, 2025};                     // c) direct assignment
cin >> customer[2].car.plateNo >> customer[2].car.brand      // d) 3rd customer's new car
    >> customer[2].car.model >> customer[2].car.year;
for (Customer c : customer)                                  // e)
    cout << "Name: " << c.name << ", Car: " << c.car.plateNo << " " << c.car.brand << " "
         << c.car.model << ", Appointment: " << c.appointment.day << "/"
         << c.appointment.month << "/" << c.appointment.year << endl;
```

**Q6 SportsType**（把 struct 传进 function，→ Ch6）
```cpp
SportsType soccer[20];
void printSports(SportsType s) {
    cout << s.sportName << ", " << s.teamName << ", " << s.numberOfPlayers
         << ", " << s.teamPayroll << ", " << s.coachSalary << endl;
}
for (int i = 0; i < 20; i++) printSports(soccer[i]);
```

---

## Closing the loop

陈家伙食费：`Expenses` 是 array in struct，`Siblings` 是 struct in struct，`siblings[5]` 是 array of struct；存取路径由外往内读：`siblings[1].mealExpenses.dinner[5]`。

但 struct 变数在记忆体里住在哪里？function 要改它的时候，是改"副本"还是"本人"？下一章 **Pointers** 讲地址——这是理解 Ch5–6 pass by address 的基础。

---

## ⚠️ Where the slides mislead

| Slide | Slide 写 | 更准确 |
|---|---|---|
| p16 vs p18 | 宣告 `char deprtID;`，却用 `strcpy(employee[0].deptID, "IT999")` | 名字不一致 + 单一 char 装不下字串 → compile error；改 `char deptID[10];` |
| p20 | 四个 struct 结尾没有 `;` | 每个 struct 定义都要 `};` |
| p7 | `newStudent .firstName`（点前有空格） | 能 compile，但标准写法无空格 |
| p24 | `ownerIC` 用 int | 12 位 IC 超过 int 范围，实务上用 string |
| p15/19/24 | 练习只有题目 | 本笔记补上可执行答案 |

---

## Term table

| English | 中文 | 一句话说明 |
|---|---|---|
| Structure (struct) / record | 结构 / 记录 | 不同类型栏位的组合 |
| Member | 成员 / 栏位 | struct 里的元件 |
| Member access operator (`.`) | 成员存取运算子 | `var.member` |
| Structure variable | 结构变数 | 依 struct 宣告出的实体 |
| Array in structure | 结构中的阵列 | 栏位是 array：`var.member[i]` |
| Array of structure | 结构阵列 | 每格是 struct：`arr[i].member` |
| Nested structure | 巢状结构 | 栏位是另一个 struct |
| `strcpy` | 字串复制 | char array 给字串用 |

---

## Cheat sheet

- `struct Name { type m1; type m2; };` ← 分号！
- 宣告：`Name var;` / `Name arr[N];` / 初始化 `Name v = {…};`（按顺序）
- `char[]` 栏位用 `strcpy`；`string` 栏位用 `=`。
- 同类型可整体 `a = b;`；不能整体 `cin`/`cout`。
- Nested：内层 struct **先定义**。
- **路径**：`变数[第几笔].栏位.子栏位[第几格]`；第 n 个 = n-1；Mon=0…Sun=6。
- Array **in** struct：`var.member[i]`；array **of** struct：`arr[i].member`。
- 字串第一个字母：`s[0] = 'S';`（单引号）。

---

## Practice (answers included)

### A. MCQ
1. `struct P { string name; int score[3]; }; P team[4];` The 2nd score of the 3rd player is  (a) `team[3].score[2]` (b) `team[2].score[1]` (c) `team.score[2][1]` (d) `team[1].score[2]`
2. Which is INVALID for `struct S { char name[10]; }; S s;`?  (a) `strcpy(s.name,"Ali");` (b) `s.name = "Ali";` (c) `s.name[0] = 'A';` (d) `cin >> s.name;`
3. Given `S a, b;` of the same struct type, `a = b;` is  (a) invalid (b) copies all members (c) copies only the first member (d) compares them
4. In a nested struct, the inner struct must be defined  (a) after (b) before (c) inside main (d) anywhere
5. `cout << emp;` where `emp` is a struct variable  (a) prints all members (b) compile error (c) prints the first member (d) prints the address

**Answers:** 1-b, 2-b, 3-b, 4-b, 5-b

### B. Short answer
**B1.** Define `struct Flight` with `code` (string), `seats` (int array of 3 classes) and declare an array for 12 flights. Set business-class (index 1) seats of the 5th flight to 30.
```cpp
struct Flight { string code; int seats[3]; };
Flight flights[12];
flights[4].seats[1] = 30;
```
**B2.** Given `struct Date {int d,m,y;}; struct Staff {string name; Date join;}; Staff st[10];` — increase the joining year of the last staff by 1. → `st[9].join.y += 1;`

### C. Application
**C1.** Weekly temperature for 3 cities: define `City {string name; double temp[7];}`, declare `City city[3]`, and display each city's average.
```cpp
for (int c = 0; c < 3; c++) {
    double sum = 0;
    for (int d = 0; d < 7; d++) sum += city[c].temp[d];
    cout << city[c].name << ": " << sum / 7 << endl;
}
```
**C2.** For Jan 2026's `siblings`, write a loop to display the total **breakfast** spending of every sibling for the whole week.
```cpp
for (int s = 0; s < 5; s++) {
    double total = 0;
    for (int d = 0; d < 7; d++) total += siblings[s].mealExpenses.breakfast[d];
    cout << siblings[s].name << ": RM" << total << endl;
}
```

### D. Thinking
**D1.** Why split `employeeType` into `nameType`, `addressType`, `dateType`? — Grouping related fields makes the record easier to read and maintain, and the small structs (e.g. `dateType`) can be reused for `hireDate` and `quitDate` or in other records.
**D2.** When would you prefer `vector<Customer>` over `Customer customer[100]`? — When the number of records is unknown or changes at run time (customers added/removed); vector grows as needed and knows its own size.

---

## Slide index
| Note section | Slides |
|---|---|
| 1 Struct recap | p2–11 |
| 2 Arrays in structs | p12–15 |
| 3 Structs in arrays | p16–19 |
| 4 Structs in structs | p20–24 |

## Links to other chapters
- Arrays & vector → **Ch1**；string letter access → **Ch2**
- Pointer to struct (`s->cgpa`) → **Ch4 / Ch6**
- Passing / returning structures to functions → **Ch6**
- `typedef struct {…} Passenger;` → **Ch8**
