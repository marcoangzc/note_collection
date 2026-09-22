# AMCS2123 — Chapter 9: Recursion

> 考题固定两种：**画 activation frames 追踪递回**（Jan 2026 Q3a，5 分：sum = 1 + 2 + 3）和**找 base case 并解释用途**（Oct 2025 Q1f，2 分）。Slide 的 activation frame 图（p13、p15、p17）是图片，而 **p15 的 Factorial 追踪写的是 `3 + Factorial(3-1)`（用加号！）**，而且停在 `Factorial(1)`——但 function 的 base case 是 `n == 0`，实际会再呼叫一次 `Factorial(0)`。照 slide 画会被扣分。这份笔记给一个固定的 activation frame 画法（下去时写呼叫、上来时写回传值），并用 g++ 验证 slide 与 Tutorial 9 的所有答案。

---

## 0. 一句话总览

**Recursion = function 呼叫自己去解一个"更小的同类问题"，直到碰到不用再递回就能直接回答的 base case；每次呼叫都有自己的 activation frame（参数、local 变数、return address），答案从最深的一层一路回传上来。**

| Part | 内容 | Slide |
|---|---|---|
| 1 | Iteration vs recursion | p3–4, p18 |
| 2 | 定义、三条规则（base case、general case、finite） | p5–8 |
| 3 | 递回 function 的特性与设计步骤 | p9–10 |
| 4 | **Activation frames** | p11 |
| 5 | 例子：Sum、Factorial、Fibonacci | p12–17 |

🎯 **这章回答的考题**
- **Jan 2026 Q3a** 用递回 function 求 sum = 1 + 2 + 3，**画 activation frames** (5)
- **Oct 2025 Q1f** `countdown(int seconds)` 的 base case 是什么、为什么需要 (2)

**Prerequisites**：Ch5 function 呼叫与 return；Ch6 local 变数（每次呼叫各自一份）。

---

## Scene：俄罗斯娃娃

你想知道一组俄罗斯娃娃总共有几个。你打开最外面那个，里面还有一个——"总数 = 1 + 里面那一组的总数"。你用**同样的方法**处理里面那一组……直到打开的娃娃是**实心的**，没有里面了——"这一组只有 1 个"。然后你把答案一层一层加回去。

- "里面那一组"= **更小的同类问题**（general case）；
- "实心娃娃"= **base case**（不用再打开就知道答案）；
- 每一层你都要记住"我正在第几层、等里面回答" = **activation frame**。

---

## 1. Iteration vs Recursion（p3–4, p18）

同一个问题通常可以用 **iteration（loop）** 或 **recursion** 解。选哪个，看问题本质和**效率**。

| | Iteration | Recursion |
|---|---|---|
| 易懂 | 较直观 | 若问题本身是递回的，code 更短、更简洁 |
| 速度 / 记忆体 | **较快、较省记忆体** | 较慢、较耗记忆体（每次呼叫一个 frame） |
| 适合 | 一般重复工作 | 本质递回的问题：**factorial、Fibonacci、divide & conquer（排序、搜寻）** |
| 风险 | 无限 loop | 没 base case → **stack overflow** |

> 💬 **答题句 (EN)** — *Recursion is more elegant and natural for inherently recursive problems, but it uses more memory because each call creates an activation frame; iteration is usually faster and uses less memory but can be less readable.*

---

## 2. 定义与三条规则（p5–8）

- **Recursion**：用**解一个更小的同类问题**来解原问题；不断缩小，直到到达 **base case（stopping case）**。
- 递回 function 可以用**不同参数**呼叫自己，所以可当作 iteration 的替代品。

| Rule | 意思 |
|---|---|
| **a) Base case / stopping case** | 至少一个**不用递回**就能解的情况；它产生的值就是停止值 |
| **b) General case** | 用更小版本的自己来表达问题；每次呼叫都必须**朝 base case 前进** |
| **c) Finite** | 问题可以在有限步之内化约到 base case |

**通用演算法（p7）**
```
if the base case is reached then
    solve it                              // base case
else
    redefine the problem using recursion  // general case
```

### ✅ 满分答法 — Oct 2025 Q1f (2 marks)
```cpp
void countdown(int seconds) {
    if (seconds == 0) {                  // ← base case
        cout << "Time's up!" << endl;
        return;
    }
    cout << seconds << " seconds left..." << endl;
    countdown(seconds - 1);              // general case, moves towards 0
}
```
- **Base case:** `if (seconds == 0)` — it prints "Time's up!" and returns without calling itself again. (1)
- **Purpose:** the base case stops the recursion. Each call reduces `seconds` by 1 until it reaches 0; without a base case the function would call itself forever (infinite recursion) until the program runs out of stack memory and crashes (stack overflow). (1)

---

## 3. 递回 function 的特性与设计（p9–10）

- 把递回 function 想成有**无限多个自己的副本**。
- **每一次呼叫**都有：自己的 code、自己的一组**参数与 local 变数**。
- 某次呼叫结束后，控制权回到**呼叫它的那一次**（上一层），并从**递回呼叫之后的那一点**继续。

**设计三步**：① 理解问题需求；② 找出 **base case(s)** 并给出解；③ 找出 **general case(s)**，用"更小版本的自己"表达。

---

## 4. Activation frames（p11）⭐

**Activation frame** = 每次 function 被呼叫时建立在（stack 上）的一块记忆体，存放管理该次呼叫所需的一切，直到它执行完：

| 内容 | 说明 |
|---|---|
| **Return address** | function 结束后程式要回到哪里继续 |
| **Parameters / arguments** | 传进来的值 |
| **Local variables** | function 内宣告的变数 |
| **Saved state** | 例如前一个 stack pointer、registers，让执行能正确恢复 |

### 4.1 画 activation frame 的固定画法（考试照这样画）

> 1. **每一次呼叫画一个方框**，往右下方叠（越深越下面）。
> 2. 方框里写：**function 名 + 实际参数值**、if 判断的结果（true/false）、那一行 return 的式子（**代入数字**）。
> 3. **往下的箭头**标"呼叫"；到 base case 后，**往上的箭头**标"回传的值"。
> 4. 最上面写最终结果。

### ✅ 满分答法 — Jan 2026 Q3a: sum = 1 + 2 + 3 with activation frames (5 marks)

**Recursive function（2 分）**
```cpp
int sum(int n)
{
    if (n == 1)                    // base case
        return 1;
    else
        return n + sum(n - 1);     // general case
}
// call: int total = sum(3);
```
**Activation frames（3 分）**
```
 total = sum(3)                                      ▲ returns 6
 ┌──────────────────────────────┐                    │
 │ sum(3)                        │────────────────────┘
 │ n = 3                         │
 │ if (n == 1) → false           │
 │ return 3 + sum(2)  = 3 + 3 = 6│◄──────────┐
 └───────────────┬──────────────┘           │ returns 3
                 │ call sum(2)               │
                 ▼                            │
        ┌──────────────────────────────┐     │
        │ sum(2)                        │─────┘
        │ n = 2                         │
        │ if (n == 1) → false           │
        │ return 2 + sum(1)  = 2 + 1 = 3│◄──────────┐
        └───────────────┬──────────────┘           │ returns 1
                        │ call sum(1)               │
                        ▼                            │
               ┌──────────────────────────────┐     │
               │ sum(1)                        │─────┘
               │ n = 1                         │
               │ if (n == 1) → true            │
               │ return 1        (base case)   │
               └──────────────────────────────┘
```
*看图重点：往下是呼叫（3 → 2 → 1），往上是回传（1 → 3 → 6）。* **sum = 6**（g++ 验证）。

---

## 5. Slide 的三个例子（p12–17）

### 5.1 Sum 1..N（p12–13）
```cpp
int Sum(int n) {
    if (n == 1) return 1;
    else        return n + Sum(n - 1);
}
```
Sum(3)：3 + Sum(2) → 2 + Sum(1) → 1；回传 1 → 3 → **6**。（图同 §4.1）

### 5.2 Factorial（p14–15）⚠️
```cpp
int Factorial(int n) {
    if (n == 0) return 1;
    else        return n * Factorial(n - 1);
}
```
**正确的 activation frames**（Factorial(3)）：
```
 Factorial(3): n=3, n==0 false → return 3 * Factorial(2) = 3 * 2 = 6   ▲ 6
   Factorial(2): n=2, false    → return 2 * Factorial(1) = 2 * 1 = 2   ▲ 2
     Factorial(1): n=1, false  → return 1 * Factorial(0) = 1 * 1 = 1   ▲ 1
       Factorial(0): n=0, true → return 1  (base case)                 ▲ 1
```
⚠️ **Slide p15 的错误**：frames 写成 `return (3 + Factorial(3-1))`（**加号**），而且只画到 `Factorial(1)` 就回传 1。但 code 是**乘号**，base case 是 `n == 0`，所以会**多一层 Factorial(0)**。slide 的数字（1, 2, 6）刚好对，但过程写错。考试请照上面画 **4 个 frame、用乘号**。

### 5.3 Fibonacci（p16–17）
```cpp
int Fibonacci(int n) {
    if (n <= 0)      return 0;
    else if (n == 1) return 1;
    else             return Fibonacci(n - 1) + Fibonacci(n - 2);
}
```
Fibonacci(3) 的呼叫**树**（一次呼叫分两支）：
```
                        Fibonacci(3)
                 n<=0 false, n==1 false
                 return Fibo(2) + Fibo(1) = 1 + 1 = 2
                  /                              \
          Fibonacci(2)                        Fibonacci(1)
   n<=0 false, n==1 false                  n<=0 false, n==1 true
   return Fibo(1) + Fibo(0) = 1 + 0 = 1     return 1
        /                \
  Fibonacci(1)        Fibonacci(0)
  n==1 true           n<=0 true
  return 1            return 0
```
**Result: Fibonacci(3) = 2**（g++ 验证）。注意 Fibonacci(1) 被**重复计算**两次——这是递回较没效率的例子。

---

## 6. Tutorial 9 答案（全部 g++ 验证）

**Q1** `func(n)`: `if (n == 0 || n == 1) return 1; else return n * func(n - 1);`
(a) n = 4：
```
 func(4): n=4 → return 4 * func(3) = 4 * 6 = 24   ▲ 24
   func(3): n=3 → return 3 * func(2) = 3 * 2 = 6  ▲ 6
     func(2): n=2 → return 2 * func(1) = 2 * 1 = 2 ▲ 2
       func(1): n==1 true → return 1               ▲ 1
```
**Result = 24**。(b) 它计算 **factorial n!**。(c) 限制：**负数**输入永远到不了 base case → 无限递回、stack overflow；n 太大时每层一个 frame 会耗尽 stack，且 `int` 在 13! 就 **overflow**（超过约 21 亿）。

**Q2** `sequence(1)`：
```
 sequence(1): 1 < 13 true → print 1, call sequence(5)
   sequence(5): 5 < 13 true → print 5, call sequence(9)
     sequence(9): 9 < 13 true → print 9, call sequence(13)
       sequence(13): 13 < 13 false → do nothing, return
```
Output：`1` `5` `9`（各一行）。（原码 `void main()` 里 `return 0;` 矛盾，应为 `int main()`。）

**Q3** Fibonacci(3) = **2**（见 §5.3）。

**Q4** `strange(4)`（`n/2` 是整数除法）：
```
 strange(4): n≠1 → ans = 1 + strange(2) = 1 + 1 = 2   ▲ 2
   strange(2): n≠1 → ans = 1 + strange(1) = 1 + 0 = 1 ▲ 1
     strange(1): n==1 → ans = 0                      ▲ 0
```
(b) **result = 2**（它其实在算 log₂n，extra）。

**Q5 findMax**
```cpp
int findMax(int arr[], int n) {
    if (n == 1) return arr[0];                          // base case
    return max(arr[n - 1], findMax(arr, n - 1));        // #include <algorithm>
}
int main() {
    int arr[] = {3, 9, 4, 7};
    cout << "Max = " << findMax(arr, 4);                // 9
}
```
**Q6 sumOfDigit**
```cpp
int sumOfDigit(int n) {
    if (n < 10) return n;                  // single digit → base case
    return n % 10 + sumOfDigit(n / 10);    // last digit + sum of the rest
}
```
```
 sumOfDigit(1234): 4 + sumOfDigit(123) = 4 + 6 = 10   ▲ 10
   sumOfDigit(123): 3 + sumOfDigit(12) = 3 + 3 = 6    ▲ 6
     sumOfDigit(12): 2 + sumOfDigit(1) = 2 + 1 = 3    ▲ 3
       sumOfDigit(1): 1 < 10 → return 1               ▲ 1
```
Output：**10**。

**Q7 countChar**
```cpp
int countChar(string str, char ch, int index) {
    if (index == str.length()) return 0;                       // passed the end
    return (str[index] == ch ? 1 : 0) + countChar(str, ch, index + 1);
}
// countChar("programming", 'm', 0) → 2
```

---

## Closing the loop

俄罗斯娃娃：base case = 实心娃娃（`n == 1` 或 `n == 0`），general case = "1 + 里面的"。画 activation frames 就是把每一层写下来：下去记呼叫、上来填回传值。

下一章回到最底层：数字在电脑里是一串 bits，**bitwise operators** 直接操作这些 bits。

---

## ⚠️ Where the slides mislead

| Slide | Slide 写 | 更准确 |
|---|---|---|
| p15 | Factorial frames 用 `3 + Factorial(3-1)`，停在 Factorial(1) | 应为 `3 * Factorial(2)`，并多一层 Factorial(0)（base case `n==0`） |
| p4, p18 | "Iteraction" | typo：Iteration |
| p4 | "An iterative solution is more obvious…" 又说 "Coding for recursion is shorter, simpler and easier to understand" | 两句看似矛盾：对一般问题 iteration 较直观；对**本质递回**的问题 recursion 较简洁 |
| p11 | "created on the whenever" | 缺字：created **on the stack** whenever… |

---

## Term table

| English | 中文 | 一句话说明 |
|---|---|---|
| Recursion | 递回 | function 呼叫自己 |
| Iteration | 迭代 | 用 loop 重复 |
| Base case / stopping case | 基本情况 / 停止条件 | 不再递回，直接回答 |
| General case | 一般情况 | 用更小的自己表达 |
| Activation frame | 活动框架（堆叠框） | 每次呼叫的记忆体区块 |
| Return address | 返回地址 | 结束后回到哪里 |
| Stack overflow | 堆叠溢位 | 递回太深/无限 |
| Divide and conquer | 分治法 | 拆成小问题再合并 |

---

## Cheat sheet

- 每个递回 function：**base case（先写）+ general case（往 base case 靠近）**。
- Base case 的作用：**停止递回**，避免无限呼叫 / stack overflow。
- Frame 内容：return address、parameters、local variables、saved state。
- 画法：一层一框、代入数字、下去写呼叫、上来写回传。
- Sum(3) = 6；Factorial(3) = 6（4 层，到 0）；Fibonacci(3) = 2；func(4) = 24；strange(4) = 2；sumOfDigit(1234) = 10。
- Recursion：简洁、自然，但耗记忆体；Iteration：快、省记忆体。

---

## Practice (answers included)

### A. MCQ
1. Which is REQUIRED in every recursive function?  (a) a loop (b) a base case (c) a global variable (d) two parameters
2. `int f(int n){ if(n<=0) return 0; return n + f(n-2); }` f(5) =  (a) 9 (b) 15 (c) 8 (d) 6
3. What is stored in an activation frame?  (a) only the return value (b) parameters, local variables, return address (c) global variables (d) the whole program
4. Missing base case usually leads to  (a) compile error (b) stack overflow (c) wrong type (d) nothing
5. Which problem is naturally recursive?  (a) printing 1–10 (b) Fibonacci (c) reading a file line by line (d) summing a fixed array of 3

**Answers:** 1-b, 2-a (5+3+1+0), 3-b, 4-b, 5-b

### B. Activation frames
Draw frames for `power(2, 3)` where
```cpp
int power(int b, int e) { if (e == 0) return 1; return b * power(b, e - 1); }
```
**Answer:**
```
 power(2,3): e≠0 → 2 * power(2,2) = 2 * 4 = 8   ▲ 8
   power(2,2): e≠0 → 2 * power(2,1) = 2 * 2 = 4 ▲ 4
     power(2,1): e≠0 → 2 * power(2,0) = 2 * 1 = 2 ▲ 2
       power(2,0): e==0 → return 1               ▲ 1
```

### C. Application
**C1.** Write a recursive `printReverse(int n)` that prints n, n−1, …, 1.
```cpp
void printReverse(int n) { if (n == 0) return; cout << n << " "; printReverse(n - 1); }
```
**C2.** Write a recursive `sumArray(int a[], int n)` returning the sum of the first n elements.
```cpp
int sumArray(int a[], int n) { if (n == 0) return 0; return a[n-1] + sumArray(a, n - 1); }
```

### D. Thinking
**D1.** Why is recursive Fibonacci inefficient? — It recomputes the same values many times (Fibonacci(1) twice for n = 3; the number of calls grows exponentially), and every call uses a stack frame.
**D2.** Rewrite `Sum(n)` iteratively and compare. — `int s = 0; for (int i = 1; i <= n; i++) s += i;` — uses one frame and no call overhead, so it is faster and uses less memory.

---

## Slide index
| Note section | Slides |
|---|---|
| 1 Iteration vs recursion | p3–4, p18 |
| 2 Definition & rules | p5–8 |
| 3 Characteristics & design | p9–10 |
| 4 Activation frames | p11 |
| 5 Sum / Factorial / Fibonacci | p12–17 |

## Links to other chapters
- Function call & return → **Ch5**
- Local variables (auto, each call its own) → **Ch6**
- Passing arrays / strings to a recursive function → **Ch6**
