# AMIT3353 Mobile Application Development — 考前总攻略

> 先读这一页。它告诉你：考卷长什么样、每一题出自哪一章、哪些 slide 讲错了。
> 各章笔记：讲解用中文，**术语保持英文**，每个重要概念后面有 **💬 答题句 (EN)**（考卷上直接写的句子），过去考题有 **✅ 满分答法**。

---

## 1. 考卷格式

- **2 小时，4 题，全部要答，每题 25 分**（共 100 分）。
- **整份考卷围绕一个 case study**：
  - Oct 2025：**MyGOV Malaysia**（政府服务 app：MyDigital ID、ePayment、申请状态追踪、多语言）
  - Jan 2026：**ELSA**（SME Bank 的 AI 企业评估 app：AI 诊断、ModalNiaga 融资申请、ScoreXcess、培训模组）
- 👉 **每一个答案都要连到 case 里的 app**。只写定义、不提 MyGOV/ELSA，通常拿不到"apply / justify"的分。
- 常见 command verbs：*Examine, Justify, Adapt, Apply, Demonstrate, Identify … with a simple justification, Illustrate using a diagram, Differentiate … in a table format, Relate, Analyse, Explain, Interpret, Show, Choose*。

## 2. 两份 past year 的出题地图

| 题目 | Oct 2025 (MyGOV) | 分 | Jan 2026 (ELSA) | 分 | 哪一章 |
|---|---|---|---|---|---|
| Mobile OS 两种 + 为什么适合 | Q1a | 10 | — | | **Ch1** |
| Key mobile challenges | — | | Q1a(i) 4 个 | 12 | **Ch1** |
| Beyond mobile app（AI） | — | | Q1a(ii) | 4 | **Ch1** |
| **Jetpack Compose 优化 layout performance（3 个方法）** | Q1b | **15** | Q1b | **9** | **Ch2 (2.1)** |
| 服务不同种族 + 各种设备（Design for everyone） | Q2a | 10 | — | | **Ch2 (2.3)** |
| 通知用户有新 update | Q2b | 3 | — | | **Ch2 (2.3)** |
| 提交后给用户 feedback 的两个方法 | — | | Q2a | 6 | **Ch2 (2.2/2.3)** |
| **Explicit vs Implicit intent** | Q2c 四个 task | 12 | Q2c 两种 + 例子 | 10 | **Ch3** |
| **Activity lifecycle 图** | — | | Q2b | 9 | **Ch3** |
| Front end / Back end + **architecture 图** | Q3a | 9 | — | | **Ch4 (4.2)** |
| UI thread vs Background thread + 两个 async task | — | | Q3a | 10 | **Ch4 (4.2)** |
| Internal vs External storage（表格）+ 存储方法 | Q3b | 10 | — | | **Ch4 (4.1)** |
| SharedPreferences / Files / SQLite 选哪个 | — | | Q3b | 9 | **Ch4 (4.1)** |
| Foreground vs Background location | Q3c | 6 | — | | **Ch5** |
| GPS vs Wi-Fi/Cell tower | — | | Q3c | 6 | **Ch5** |
| Geocoding 的应用 | Q4a | 6 | — | | **Ch5** |
| **Camera**：两种方法 / manifest / 场景 / 其他 sensor | Q4b | 9 | Q4a | 16 | **Ch6** |
| Distribution（channel / App Bundles） | Q4c(i) | 5 | Q4b(i) | 4 | **Ch7** |
| **Monetisation model** | Q4c(ii) | 5 | Q4b(ii) | 5 | **Ch7** |

**两份都考、一定要准备的**：Compose layout performance、Intents、Camera、Monetisation、Distribution、Storage（两种问法）、Location（两种问法）。

**按章节的分数（两份合计 200 分）**：Ch3 ≈ 31 · Ch1 ≈ 26 · Ch6 ≈ 25 · Ch2.1 ≈ 24 · Ch2.2/2.3 ≈ 19 · Ch4.1 ≈ 19 · Ch4.2 ≈ 19 · Ch7 ≈ 19 · Ch5 ≈ 18。

## 3. 一天读书计划（约 9 小时）

| 时段 | 读什么 | 做什么 |
|---|---|---|
| 0:45 | **Ch1** | 背 4 challenges、Android vs iOS、4 components (ABCS) |
| 1:30 | **Ch2** | 背 Compose performance 3 方法（写满 15 分的版本）；Design for everyone；Toast/Snackbar/Notification/Dialog |
| 1:30 | **Ch3** | 默写 lifecycle 图 2 次；练习 explicit/implicit 判断 |
| 1:30 | **Ch4** | 画 architecture 图；背 storage 表格；UI thread vs background |
| 0:45 | **Ch5** | GPS vs Wi-Fi 表；foreground vs background；geocoding |
| 1:00 | **Ch6** | 相机两种方法 + manifest + 3 类 sensor |
| 0:45 | **Ch7** | App Bundles、分发渠道、7 种 monetisation |
| 1:15 | 全部 | 不看笔记，自己写两份 past year，再对 ✅ 满分答法 |

## 4. 答题技巧（这门课的评分习惯）

1. **分数 ÷ 3 ≈ 要写几点**：每一点 = 概念名 + 解释 + **套到 case app 的例子**。例：15 分 3 个方法 → 每个方法写 5 分（名称、原理、怎么做/代码片段、对 MyGOV 的好处）。
2. **"with a simple justification"（3 分）**：答案 1 分 + 理由 2 分。
3. **"in a table format"**：一定要画表，不画表会扣分。
4. **"using a diagram"**：图 + 标注 + 下面几句解释。
5. 写 Jetpack Compose 的名字（LazyColumn、composable、recomposition），因为题目指定 Compose。

## 5. Slide 里要小心的地方（详细见各章末尾）

| 位置 | 问题 |
|---|---|
| Ch5 p45 | Geocoding 定义写成"location → address"，但图其实是 address → coordinates（这才是 geocoding；反过来叫 reverse geocoding）。 |
| Ch6 p31 | Accelerometer "excluding gravity" 不准：accelerometer 包含重力，**linear acceleration** 才排除重力。 |
| Ch6 p10 | Camera Intent 写 "startActivity()"，但要拿回照片必须用 startActivityForResult / Activity Result API。 |
| Ch4.2 p54 | BaaS 的定义其实是 DevOps 的定义。 |
| Ch4.2 p25–28 | AsyncTask 已在 API 30 deprecated（考试照 slide 答，但可加一句 Coroutine 是现代做法）。 |
| Ch3 p49 | onStart 写"becomes interactive"——其实要到 onResume 才能互动。 |
| Ch2.3 p33 | string-array 应该用 `R.array`，不是 `R.string`。 |
| Ch2.3 p63 | 触控目标是 **48 × 48 dp**，slide 写 pixel。 |
| Ch2.1 p4 | "Constraint Layout … has been replaced by the Constraint Layout"——那一格其实在描述 RelativeLayout。 |

## 6. 这份笔记的限制（诚实说明）

- **Kotlin 代码没有实际执行**：这台电脑没有 Kotlin compiler。代码片段都按 Android 官方文档核对过语法与行为，但没有跑过。
- **所有 slide 图片都逐页看过**（包括 vector 画的图），图里的内容（architecture、lifecycle、intent 流程、表格）都已放进笔记。
- 两份 past year 都是扫描版，已用图片逐页读过。
