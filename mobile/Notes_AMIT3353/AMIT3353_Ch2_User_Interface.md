# AMIT3353 — Chapter 2: User Interface (2.1 Layouts · 2.2 Navigation · 2.3 Menus, Notifications & Design for Everyone)

> Slide 混用了旧的 XML View 系统与新的 Jetpack Compose，常常没说清楚哪个是哪个；"Layout performance" 只有三行标题，但考试要写 9–15 分。这份笔记把三个方法写成满分版本，把 Toast / Snackbar / Notification / Dialog 的选择规则讲清楚，并补上 slide 的几个错误（R.string vs R.array、48dp、RelativeLayout 描述错位）。

---

## 0. 一句话总览

**UI 用 Compose 的 Row/Column/Box 与 Lazy 列表搭出来；用 NavHost + NavController 在画面之间导航；用 Toast/Snackbar/Notification/Dialog 给用户 feedback；用外部化 resources 支持不同语言、屏幕和 Android 版本，并照顾无障碍 (accessibility)。**

| Part | 内容 | Slide |
|---|---|---|
| 2.1 | Compose 基础 layout、ViewGroup、Lazy list、**layout performance**、UI components、Style/Theme、Dark theme | 2.1 p3–37 |
| 2.2 | Navigation principles、Fragment、Navigation component、Compose navigation、Dialog、Navigation drawer、Tabs | 2.2 p4–57 |
| 2.3 | Menus/TopAppBar、Up button、Toast / Snackbar / Notification、Design for everyone（语言/屏幕/版本）、Accessibility | 2.3 p4–77 |

🎯 **这章回答的考题**
- **Oct 2025 Q1b** Adapt THREE methods to optimise layout performance in Jetpack Compose（**15**）
- **Jan 2026 Q1b** Apply THREE methods to optimise layout performance（**9**）
- **Oct 2025 Q2a** Apply TWO methods for MyGOV to serve different races and a variety of devices（10）
- **Oct 2025 Q2b** Demonstrate how MyGOV notifies users when a new update is available（3）
- **Jan 2026 Q2a** Demonstrate TWO methods ELSA updates users after financing submission（6）

**Prerequisites**：Ch1 的 Activity（一个 screen）与 Resources（与 code 分开的 string/layout/image）。一点 Kotlin：`@Composable` 函数就是"画一块 UI 的函数"；`remember { mutableStateOf(...) }` 是会让 UI 自动更新的变量。

---

# 2.1 Layouts, Components and Input Events

## 1. Compose 的基本积木（2.1 p3）

Jetpack Compose 是**声明式 (declarative)** UI：你描述"画面应该长怎样"，状态改变时 Compose 自动重画。UI 是**把简单 layout 一层一层嵌套**出来的。

| 用途 | 元素 |
|---|---|
| 核心积木 | **Row**（横排）、**Column**（直排）、**Box**（叠放） |
| 控制 padding、size、alignment | **Modifier**（例 `.padding(16.dp).fillMaxWidth()`） |
| 分隔空间 | **Divider** |

## 2. ViewGroup：Static vs Dynamic content（p4–8）

**Static content（内容固定）**：
1. **Row & Column** – 把 children 排成单一横列或直行（p4 图：几个并排的长方块）。
2. **Constraint Layout** – 用**约束**指定每个 child 相对于其他 view 或 parent 的位置；能做**大而复杂、但 view hierarchy 扁平 (flat)** 的 layout；Android 2.3（API 9）以上。**每个 view 至少要一个 horizontal 和一个 vertical constraint**（p6 图：A、B、C 三块，C 的上方约束连到 A，红圈标出）。
3. **Web layout** – 显示网页（WebView）。

**Dynamic content（数量不定、会滚动）**：
4. **Lazy List**（LazyColumn / LazyRow）– 可滚动的单列列表。
5. **Lazy Grid** – 可滚动的格子（行 × 列）。

```kotlin
val items = listOf("Apple", "Banana", "Cherry")
LazyColumn {
    items(items) { item ->
        Text(text = item)          // 每个 item 画一行
    }
}
```
→ LazyRow/LazyColumn **不需要 adapter**（旧的 RecyclerView 需要 Adapter + ViewHolder）。

### ListView vs LazyList vs RecyclerView（p15–16 图）

```
ListView                          LazyList / RecyclerView
所有 records 一次全部载入           只载入"看得见"的 records
[rec][rec][rec][rec] ← 屏幕内      [rec][rec][rec][rec] ← 屏幕内
[rec][rec][rec] ← Not visible      [ ] ← Not visible：还没建立
[rec][rec][rec]   但已占内存
```
*重点：列表有 1000 笔资料时，ListView 占 1000 份内存；Lazy 只建约 10 份。*

**RecyclerView（p16）**：item 滚出屏幕后，它的 view 被**回收 (recycled)**，填入新滚进来的内容再用——像回转寿司的盘子，空盘收回去再装新寿司。

**Card-based layout（p17）**：把资讯放进卡片，跨平台外观一致（例：旅游 app 每个景点一张卡）。

**p9 问题：为 app 选 layout**：A（YouTube 首页：一直往下滚的影片清单）→ **LazyColumn**；B（Google Play：分类区块，每区横向滚动的 app 图标）→ **LazyColumn 包着多个 LazyRow**（或 LazyGrid）。

## 3. ⭐ Layout Performance（p10–13）——考试 15 分

传统 Android 有三招改善 layout performance，Compose 各有对应做法：

| 传统 View 系统（p10） | Jetpack Compose 怎么做（p11–13） |
|---|---|
| 1. Optimizing layout hierarchies（减少嵌套层数） | 没有传统 View hierarchy；**flat tree of composables**，**只 recompose 有改变的部分**；composable 很轻量 |
| 2. Re-using layouts with `<include>` | 用**可重用的 composable functions** 取代；把 UI **拆成小的、可重用的 composables** |
| 3. Loading views on demand（ViewStub） | **LazyColumn/LazyRow** + **smart recomposition**：**只 compose 并画出看得见的 items** |

**为什么嵌套会慢？** 系统画画面要先 measure、再 layout、再 draw，每一层都要算；层数越多、同一个 view 被量越多次。Compose 的 layout 模型**每个 child 只 measure 一次**，所以扁平又快。

**什么是 recomposition？** 状态（state）改变时，Compose 重新执行受影响的 composable 来更新画面。"Smart" = 只重跑**读取了那个 state 的 composable**，其他跳过。

### ✅ 满分答法 — Oct 2025 Q1b Adapt THREE methods to optimise layout performance for MyGOV（15 分 = 3 × 5）

> **1. Optimise the layout hierarchy (flat composable tree and smart recomposition)**
> In traditional Android, deeply nested ViewGroups force the system to measure and lay out the same views repeatedly, which slows rendering. Jetpack Compose has no traditional view hierarchy: it builds a flat tree of lightweight composables and each child is measured only once. It also recomposes only the parts of the UI whose state has changed. For MyGOV, the application-status screen can keep the status text in its own small composable, so when an application changes from "Processing" to "Approved" only that text is recomposed instead of redrawing the whole page with the service categories, banners and payment buttons. This keeps the app smooth even on low-end phones.
>
> **2. Re-use layouts with small reusable composable functions (instead of `<include>`)**
> Traditional Android re-used XML layouts with the `<include>` tag. In Compose, the best practice is to break the UI into small, reusable composable functions. MyGOV shows over 100 government services with the same card design, so a single `ServiceCard(title, icon, onClick)` composable can be written once and reused for road tax renewal, income tax filing, healthcare and cash aid. This reduces duplicated code, keeps the design consistent across Health, Transport, Education and Welfare categories, and because each card is small and stable, Compose can skip recomposing cards whose inputs have not changed.
>
> **3. Load views on demand with lazy layouts (LazyColumn / LazyRow / LazyGrid)**
> Loading every item at once (like the old ListView) wastes memory and slows start-up. Compose's LazyColumn, LazyRow and LazyVerticalGrid compose and draw only the items that are currently visible on screen, and compose new items as the user scrolls, without needing a RecyclerView adapter. MyGOV's list of 100+ services and a user's long transaction/payment history should be displayed with LazyColumn (and the category grid with LazyVerticalGrid), so the app opens quickly, scrolls smoothly and uses little memory on entry-level devices.

**Jan 2026 Q1b（9 分 = 3 × 3）**：写同样三个方法，每个"方法名 + 原理 + ELSA 例子"各一句即可。例子：(1) 只 recompose 更新了的 AI diagnostic score composable；(2) 可重用的 `ProgramCard` composable 显示培训/融资方案；(3) 用 LazyColumn 显示长长的 learning modules 与申请记录。

💬 **答题句 (EN)** — *Jetpack Compose optimises layout performance by (1) using a flat tree of composables that only recomposes the parts that change, (2) breaking the UI into small reusable composable functions instead of `<include>`, and (3) loading items on demand with lazy layouts such as LazyColumn, which only compose visible items.*

## 4. UI Components（p14–32）

**View = widget**（例 Button、TextView）；p14 图：email 画面上 "To"、"Subject" 两个输入框和 "Send" 按钮都是 view。

| Component | 用途 | Compose |
|---|---|---|
| **Button** | 按下触发动作；两种：**Button**（文字+icon）、**ImageButton**（只有 icon） | `Button(onClick = { ... }) { Text("Click Me") }` —— onClick 用 **lambda expression** 处理事件 |
| **Text Field** | 输入文字；用 **keyboardType** 指定键盘 | `TextField(value, onValueChange, keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.Email))` |
| **Check Box** | 从一组中选**一个或多个**；直列排列 | `Checkbox(checked, onCheckedChange)` |
| **Radio Button** | 从一组中**只选一个**（互斥），用户需要并排看到所有选项时用 | `RadioButton(selected, onClick)` |
| **Toggle** | 开/关 | `Switch` |
| **Spinner (DropDownMenu)** | 从一组快速选一个；平时显示当前值，点开显示其他选项 | `ExposedDropdownMenuBox` + `DropdownMenuItem` |

**Keyboard types（p24）**：`text`（一般）、`textEmailAddress`（多了 @）、`textUri`（多了 /）、`number`（数字键盘）、`phone`（电话键盘）。
→ 选对键盘 = 少打错字、更快，也是好的 UX。

**Checkbox vs Radio vs Spinner**：可多选 → Checkbox；单选且选项少、要一眼看全 → Radio；单选但选项多、要省空间 → Spinner。

## 5. Style, Theme & Dark Theme（p33–37）

- **Style**：一组外观属性，套在**单一 view** 上。
- **Theme**：一组 styles，套在**整个 app 或 activity** 上（p35 图：Activity Class ↔ Layout(View, View)，View ↔ 单一 Style；Layout ↔ Theme(Style, Style)）。
- Compose：`MaterialTheme(colorScheme = ..., typography = ..., shapes = ...) { /* composables */ }`。
- **Dark theme**：Android 10（API 29）以上。好处：**省电**（OLED 屏幕黑色像素不发光）、**改善可见度**（对弱视者）、**低光环境更舒服**。支援方法：theme 继承 **DayNight**（`Theme.AppCompat.DayNight` 或 `Theme.MaterialComponents.DayNight`）。

---

# 2.2 Navigation, Fragment and Dialogs

## 6. Navigation Principles（2.2 p4–11）

1. **Fixed start destination** – 每次从 launcher 开 app，都从同一个起点画面开始。
2. **Navigation state = a stack of destinations** – A → B → C 就是一叠卡片，最上面那张可见（p6 图）。
3. **Up and Back are identical within your app's task** – 在自己 app 里，按 Up（左上角 ←）与按 Back 效果一样：回到上一个画面。
4. **The Up button never exits your app** – 在 start destination 没有 Up 按钮；只有 Back 能离开 app。
5. **Deep linking simulates manual navigation** – 从外部链接直接跳到深层画面（p10 图：桌面 → 植物清单 → Apple 详情），back stack 要像用户自己一步步点进去那样建立，按 Back 会回到清单，而不是直接离开。

```
p9 图：Up 永远不离开 app
  C  ─Up→  B  ─Up→  A (start) ─Back→ 离开 app（回到 launcher）
```

**p11 Q：为什么 Back 和 Up 要不同？** Up 只在 app 内的层级往上走，永远不离开 app；Back 按时间顺序倒退，在起点按 Back 会离开 app 回到之前的地方（例如另一个 app）。

## 7. Fragment（p12–20）

**Fragment = Layout + Code 的可重用 UI 模块**，必须放在 **Fragment Activity** 里；一个 activity 可放多个 fragments，也可在运行时替换（p13 图：Fragment A ⇄ Fragment B）。

**Multi-pane（p14–18 图）**：
```
平板 Landscape（一个 activity 放两个 fragment）   手机 Portrait（两个 activity）
┌────────────┬─────────────────┐                ┌──────────┐   ┌──────────────┐
│ Fragment 1 │ Fragment 2      │                │Fragment 1│ → │ Fragment 2   │
│ Item 1  ◀──┼ Details of      │                │ Item 1   │   │ Details of   │
│ Item 2     │ Item 1          │                │ Item 2   │   │ Item 1       │
└────────────┴─────────────────┘                └──────────┘   └──────────────┘
```
*重点：同两个 fragments，平板并排显示、手机分两页——写一次，适配不同屏幕。*

**加入 fragment layout 的顺序（p19）**：1. Fragment 的 **onCreateView** 载入 fragment layout → 2. **Inflate** 进 activity layout 的 **container** → 3. Activity 的 **onCreate** 载入 activity layout。

**Fragments vs Jetpack Compose（p20）**

| Feature | Fragments (Old) | Jetpack Compose (New) |
|---|---|---|
| UI definition | XML layouts + Kotlin | Pure Kotlin composables |
| Navigation | FragmentManager + NavController | NavHost + navController |
| Performance | Slower (view inflation) | Faster (composables are lightweight) |
| State handling | ViewModel + SavedStateHandle | remember + State |
| Lifecycle complexity | Fragment lifecycle (onCreate, onStart…) | Simpler lifecycle with composables |

**p40 Q 答案**：Fragment 是 activity **里面**的一部分 UI，不能单独存在；Activity 是完整画面。Fragment 优点：**可重用**、**做 multi-pane（适配平板）**、在同一 activity 内切换比开新 activity 更轻。

## 8. Navigation Component（p21–39）

三个部分（p23、27、30 图）：

| 部分 | 是什么 |
|---|---|
| **Navigation Graph** | XML resource，包含所有导航资讯：有哪些 destinations、之间有哪些 actions、start destination |
| **NavHost** | 空的 container，用来**显示** graph 里的 destination；`app:defaultNavHost="true"` 让它**拦截系统 Back 按钮**（只能有一个 default）；`app:navGraph` 连到 graph |
| **NavController** | 在 NavHost 里**管理导航**的对象（`navigate()`、`popBackStack()`） |

旧做法（XML）：graph 里 `app:startDestination="@+id/nav_home"`，每个 `<fragment>` 下有 `<action android:id="@+id/homeToGallery" app:destination="@id/nav_gallery"/>`；程序里 `findNavController(...).navigate(R.id.action_...)`。

**Compose 做法（p32–39）**：
```kotlin
// 1. gradle: implementation(libs.androidx.navigation.compose)
// 2. MainActivity: setContent { MyAppNavHost(navController = rememberNavController()) }
@Composable
fun MyAppNavHost(navController: NavHostController) {
    NavHost(navController, startDestination = "home") {     // start destination
        composable("home")    { HomeScreen(navController) }     // route = 字串
        composable("details") { DetailScreen(navController) }
    }
}
// 前往：navController.navigate("details")
// 返回：navController.popBackStack()
// 带资料：composable("details/{itemId}") { entry -> entry.arguments?.getString("itemId") }
//        navController.navigate("details/123")
```
Bottom navigation：`BottomNavigationItem(icon, label, onClick = { navController.navigate("home") })`。

## 9. Dialogs（p41–50）

用 dialog 于：**critical information**、**做决定**、把多个任务包在一个独立流程里。**少用**，因为它会打断用户。

### Snackbar vs Notification vs Dialog（p43，⭐ 考试选择题的依据）

| Component | Priority | User action | 什么时候消失 |
|---|---|---|---|
| **Snackbar** | Low | Optional | **自动消失** |
| **Notification** | Medium | Optional | 直到用户 dismiss，或引起通知的状况已解决 |
| **Dialog** | High | **Required** | **挡住 app**，直到用户选一个动作或退出 dialog |

四种 dialog（p44–45）：**Alert**（例 "Discard draft?" CANCEL / DISCARD）、**Simple**（选一项，例 set backup account）、**Confirmation**（选一项后按 OK 确认，例电话铃声）、**Full-screen**（例新增行事历事件，右上 SAVE）。

AlertDialog 结构（p47）：1. Title（optional）2. Content area（讯息/清单/自订）3. Action buttons（**不超过三个**）。
避免：dialog 里再开 dialog、alert 里放可滚动内容（例外：full-screen dialog 可以开 picker）。
**Pickers（p49）**：选时间 / 日期。

**p50 Q1 哪个 AlertDialog 比较好？** A（"Discard draft?" → **CANCEL / DISCARD**）比 B（**NO / YES**）好：按钮文字直接说明动作，用户不用回头读问题。

## 10. Navigation Drawer vs Tabs（p51–57）

| | Navigation Drawer | Tabs |
|---|---|---|
| 是什么 | 从左边滑出的主要导航选单 | 顶部一排标签，切换同一层级的内容组 |
| 适合 | **≥ 5 个 top-level destinations**、**≥ 2 层**导航、在**不相关**的 destinations 之间快速切换 | 内容**相关**且在**同一层级** |
| 类型 | — | **Fixed tabs**（少数，平均分）/ **Scrollable tabs**（很多，可横滑） |

**p57 答案**：新闻 app → **Tabs**（本地、国际、体育…同层级、相关）；天气 app → **Tabs**（今天/每小时/10 天）；联络人 app → **Tabs**（联络人/最近/常用）或 drawer 放设定。理由写"内容是否相关、同层级、destination 数量"即可。

---

# 2.3 Menus, Notifications and Design for Everyone

## 11. Menus / Toolbar（p3–14）

Toolbar（app bar）呈现动作与选项（p4 图）：**Drawer**（≡）· **Title** · **Action buttons**（常用，例搜寻）· **Overflow menu**（⋮，不常用）。
决定哪些放 action button：**FIT** —— **F**requent、**I**mportant、**T**ypical。
**Context-sensitive menu（p7）**：选中项目后 toolbar 变成针对选中项目的动作（例选了 2 封邮件，出现删除、封存）。

XML：`app:showAsAction="ifRoom"`（有空位就显示为按钮）/ `"never"`（永远在 overflow）。
Compose：`TopAppBar(title, navigationIcon = { IconButton{ Icon(Icons.Default.Menu) } }, actions = { ...Search, Settings... })`，**放在 Scaffold 的 topBar 里**。Icon 用 Material icons：`Icon(Icons.Default.Search, contentDescription = "Search")`。

**Up button（p12–13）**：帮用户回到 app 主画面；传统做法要在 manifest 里声明 child activity 的 **parent**；Compose：`navigationIcon = { IconButton(onClick = { navController.navigateUp() }) { Icon(Icons.Default.ArrowBack, ...) } }`。

**p14 答案**：Toolbar 重要：把最常用的动作固定在同一位置，让用户随时找到（一致性），也显示目前位置（title）；Up button 目的：在 app 层级里往上回到父画面/主画面，不会离开 app。

## 12. Toast, Snackbar, Notification（p15–21）

| | Toast | Snackbar | Notification |
|---|---|---|---|
| 在哪显示 | App 画面上的小弹窗 | App 画面底部 | **App UI 之外**：status bar icon、notification drawer、heads-up（盖在前景 app 上） |
| 可以加 action？ | ❌ | ✅（例 UNDO） | ✅（例 REPLY、ARCHIVE） |
| 消失 | 自动 | 自动（或按 action） | 直到用户 dismiss / 状况解决 |
| 用途 | 简单操作 feedback（"Sending message…"） | 简短讯息 + 可反悔的动作（"1 item removed — UNDO"） | 提醒、别人的讯息、及时资讯——**即使 app 没开也看得到** |

```kotlin
Toast.makeText(context, "Hello toast!", Toast.LENGTH_SHORT).show()
// Compose snackbar：Scaffold(snackbarHost = { SnackbarHost(snackbarHostState) })
//   coroutineScope.launch { snackbarHostState.showSnackbar("Item deleted", actionLabel = "Undo") }
```

**p21 答案**：Toast 比 Notification 适合：用户**正在使用 app**、只需简短确认、不需保留（例 "Draft saved"）。
- a. 手机 internal storage 不足 → **Notification**（系统层级、app 外也要看到、需保留）
- b. 删除了一则讯息 → **Snackbar**（在 app 里、可 UNDO）
- c. 有新 update → **Notification**（用户可能没开 app）

### ✅ 满分答法 — Oct 2025 Q2b Demonstrate how MyGOV notifies users when a new update is available（3 分）

> MyGOV should use a **Notification**, because users may not have the app open. When a new version is published, the back end sends a push message (e.g. via Firebase Cloud Messaging); the app builds a notification that appears as an icon in the status bar, as a heads-up banner such as "MyGOV update available – new services added", and stays in the notification drawer until the user dismisses it or updates. Tapping it opens the MyGOV page in Google Play / App Store (or the in-app update prompt) so the user can install the update.

### ✅ 满分答法 — Jan 2026 Q2a Demonstrate TWO methods ELSA updates users after the financing application is submitted（6 分）

> 1. **Snackbar / Toast (immediate in-app confirmation)** – as soon as the user taps "Submit", ELSA shows a Snackbar at the bottom of the screen, e.g. "Financing application submitted successfully" with a "VIEW STATUS" action. It gives instant, non-blocking feedback that the submission worked while the user is still in the app, and disappears automatically.
> 2. **Notification (status updates outside the app)** – because approval through ModalNiaga/ScoreXcess takes time, ELSA sends a push notification when the status changes, e.g. "Your application is under review" or "Approved – RM50,000". It appears in the status bar/notification drawer even when the app is closed and remains until the user opens or dismisses it; tapping it opens the application-status screen.
>
> (Alternative second method: a **confirmation Dialog** showing the reference number, which the user must acknowledge – suitable because submission is a critical action.)

## 13. Design for Everyone（p22–58）⭐

Android 设备有各种**语言、屏幕、Android 版本**。技巧：**Externalize resources, maintain them independently**——把 image、string、layout、style 从 source code 抽出来（p25 图：Source Code → Image / String / Layout / Style）。

### 13.1 Languages（p26–33）

- UI 字串放在 **`res/values/strings.xml`**，不写死在 code。
- 每种语言一个 **locale 目录**：`values/`（默认，英文）、`values-ms/`（马来文）、`values-zh/`（中文）、`values-fr/`、`values-es/`…；**string 名字一样，内容不同**。
- **运行时，系统按设备的 locale 自动挑对应的 strings**（p29 图：Source code → UI → English / Chinese / Malay strings）。
- 其他资源也能按地区换：`mipmap-b+es+ES/country_flag.png`（p28）。
- Language codes：en、en_GB、en_US、zh、zh_CN、**ms（Bahasa Melayu）**。
- 使用：`getString(R.string.hello)`；string array：`resources.getStringArray(R.array.states_array)`。

```xml
<!-- values/strings.xml -->            <!-- values-ms/strings.xml -->
<string name="title">Road Tax</string> <string name="title">Cukai Jalan</string>
```

### 13.2 Screens（p34–52）

Android 用两个属性分类屏幕：**Size**（small / normal / large / xlarge）与 **Density**（ldpi / mdpi / hdpi / xhdpi …）。解析度例子：HD 720×1280、FHD 1080×1920、QHD 1440×2560。

- **不同 layouts**：`layout/main.xml`（默认 portrait）、`layout-land/`（横向）、`layout-large/`、`layout-large-land/`——**文件名完全一样**，系统按屏幕大小/方向自动选（p42 图：手机 layout/ 两栏格子；平板 layout-land/ 左边清单右边格子）。
- **Compose**：用 **WindowSizeClass**：
```kotlin
when (windowSizeClass.widthSizeClass) {
    WindowWidthSizeClass.Compact  -> PhoneLayout()    // Column
    WindowWidthSizeClass.Medium   -> TabletLayout()   // Row
    WindowWidthSizeClass.Expanded -> DesktopLayout()  // Box
}
```
- **Display cutouts**（p43–47）：tear drop、notch、punch hole（中/左/右）；Android 9（API 28）起，`windowLayoutInDisplayCutoutMode` = **default / shortEdges / never**。还有弹出式/翻转式镜头、屏下镜头。
- **Foldables**（p48–49）：Window Manager + FoldingFeature。
- **Bitmaps（p50–52）**：为不同 density 提供不同尺寸的图：

| Density | mdpi | hdpi | xhdpi | xxhdpi | xxxhdpi |
|---|---|---|---|---|---|
| Scale | **1x（baseline）** | 1.5x | 2x | 3x | 4x |
| ≈ DPI | 160 | 240 | 320 | 480 | 640 |

放在 `drawable-mdpi/`、`drawable-hdpi/`… 同名文件。p52 图：同一个字母 "a"，低解析图在高密度屏幕上看起来有锯齿，用对应 density 的图就清晰。

**p58 Q3 "为了兼容所有屏幕，图片要做成最高画质" —— 评论**：不对。一张超高画质图放在低端手机上会**浪费内存与储存、拖慢加载**（系统要缩小它），放大的低画质图又会模糊。正确做法是为每个 density 提供对应大小的 bitmap（或用 vector drawable）。

### 13.3 Versions of Android（p53–57）

- **minSdkVersion** = app 能跑的最低 API level → 设低一点 = **支援更多设备**。
- **targetSdkVersion** = 设计与测试所针对的最高 API level → 设最新 = **用到最新 OS 功能与行为**。
- 运行时检查：`if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.HONEYCOMB) { ... }`，旧手机就跳过新功能。

### ✅ 满分答法 — Oct 2025 Q2a Apply TWO methods for MyGOV to serve users of different races and support the wide variety of devices（10 分 = 2 × 5）

> **1. Support different languages (localisation with externalised string resources)**
> Malaysia's citizens are Malay, Chinese, Indian and other races, so MyGOV should not hard-code text in the source code. All UI text is placed in `res/values/strings.xml` (default English), with locale-specific folders such as `values-ms/strings.xml` (Bahasa Malaysia), `values-zh/strings.xml` (Chinese) and `values-ta/strings.xml` (Tamil) using the same string names, e.g. `<string name="road_tax">Road Tax</string>` and `<string name="road_tax">Cukai Jalan</string>`. At runtime Android automatically loads the strings that match the user's device locale, so each user sees services such as "renew road tax" or "apply for cash aid" in their own language without separate apps, and new languages can be added later just by adding a folder.
>
> **2. Support different screens and Android versions (alternative layouts/bitmaps and SDK settings)**
> Citizens use everything from cheap small phones to tablets and foldables. MyGOV should provide adaptive layouts: in Compose, use WindowSizeClass (Compact → single-column list of services on phones; Medium/Expanded → a list-detail two-pane layout on tablets), or alternative `layout/`, `layout-land/` and `layout-large/` resources; and supply bitmaps for each density (`drawable-mdpi`, `-hdpi`, `-xhdpi`, `-xxhdpi`) so icons are sharp but not wasteful. It should also set a low `minSdkVersion` so older phones in rural areas can install it, a recent `targetSdkVersion` to use the latest security features, and check `Build.VERSION.SDK_INT` at runtime before using newer APIs.

## 14. Accessibility（p59–77）

**Accessibility** = 不论能力如何，用户都能成功**导航、理解、使用** app。三个考量：**Navigation · Readability · Guidance and Feedback**。

| # | 做法 | 细节 |
|---|---|---|
| 1 | **Support screen readers**（Navigation） | 给 UI 元素 label：`android:contentDescription="@string/share"`（Compose：`contentDescription`）；把相关内容**分组**：`android:screenReaderFocusable="true"` |
| 2 | **Easy-to-follow navigation** | 支援键盘/手势；不要让元素**过一段时间自动消失**；用**扁平 (flat)** 的导航结构（p62：宽而浅 ✓，深而窄 ✗） |
| 3 | **Large touch targets** | 至少 **48 × 48 dp**，元素间距至少 **8 dp**（食指要能完整按进去） |
| 4 | **Colour contrast**（Readability） | 足够对比（蓝配黄 ✓，橙配黄 ✗）；**不要只靠颜色**传达资讯（加 ✓/✗ 图示或文字 "Going / Can't go"）——色盲者（Ishihara test，p70–72）分不出红绿 |
| 5 | **Accessible media** | 提供暂停/停止影音的控制；提供字幕/transcript |
| 6 | **Clear, discoverable controls**（Guidance & Feedback） | 控件有文字 label、tooltip 或 placeholder；全 app 用词一致 |

**Touch zone（p65–66）**：单手持机时拇指容易按到的是屏幕**下方中间**（EASY），顶部角落最难（HARD）；屏幕越大，HARD 区越大 → 重要按钮放底部（这也是 bottom navigation 流行的原因）。
**Gesture navigation（p67–68）**：Android 10 起全手势导航；app 要 **edge-to-edge** 显示内容（导航列/状态列透明），并处理与系统手势的冲突。

**p76 答案**：1. Accessibility：让残障人士也能用；重要是因为包容更多用户、有些地方是法律要求、对所有人都更好用。2. 桌面 vs 手机导航：手机屏幕小、用手指、常单手操作 → 导航要扁平、目标要大、放在拇指区；桌面有滑鼠与大屏幕，可以有多层选单。3. 颜色的作用：建立层次与品牌、吸引注意、表达状态——但不能是唯一的资讯来源。

---

## ⚠️ Where the slides mislead

| Slide | Slide 写的 | 更准确的理解 |
|---|---|---|
| 2.1 p4 | "2. Constrain Layout – Enables you to specify the location of child objects relative to each other *Has been replaced by the Constraint Layout" | 这格描述的其实是 **RelativeLayout**（已被 ConstraintLayout 取代）。标题与内容错位。 |
| 2.1 p4 | Row & Column "creates a scrollbar if the length … exceeds the screen" | Compose 的 Row/Column **不会自动滚动**，要加 `Modifier.verticalScroll()` 或改用 LazyColumn。 |
| 2.1 p15 | LazyList "load records that are visible … using ViewHolder" | ViewHolder 是 RecyclerView 的概念；LazyColumn 不用 ViewHolder/adapter（p8 自己也这样说）。 |
| 2.2 p43 | "the state that caused the officiation" | 打错字：notification。 |
| 2.3 p31 | "Malay (Indonesia): in" | `in` 是印尼文（Bahasa Indonesia）的旧代码（新代码 `id`）；马来文是 **ms**。 |
| 2.3 p33 | "Refer to a string-array resource with the syntax R.string" | 应为 **`R.array`**；而且例子定义 `states_array` 却读 `planets_array`。 |
| 2.3 p63–64 | touch target "48 x 48 pixel" | Android 规范是 **48 × 48 dp**（density-independent pixels）。 |

考试策略：题目引用 slide 时照 slide 答；但自己写代码时用正确的 `R.array`、`dp`。

---

## Term table

| English | 中文 | 一句话 |
|---|---|---|
| Jetpack Compose | 声明式 UI 框架 | 用 Kotlin 函数描述 UI |
| Composable | 可组合函数 | 画一块 UI 的 `@Composable` 函数 |
| Recomposition | 重组 | 状态改变时只重跑受影响的 composable |
| Modifier | 修饰符 | 设定 padding、size、alignment、点击 |
| LazyColumn / LazyRow | 惰性列表 | 只 compose 可见的 items |
| ConstraintLayout | 约束布局 | 扁平、相对定位的复杂 layout |
| Fragment | 片段 | activity 里可重用的 UI 模块 |
| NavHost / NavController / Navigation graph | 导航容器 / 控制器 / 导航图 | 显示 / 控制 / 定义导航 |
| Back stack | 返回栈 | 画面的堆叠 |
| Deep link | 深层链接 | 从外部直接进入内部画面 |
| Snackbar / Toast / Notification / Dialog | — | feedback 的四种方式 |
| Localization | 本地化 | 按语言/文化调整 app |
| Locale | 语言区域 | 例 ms、zh_CN |
| Density (dpi) | 像素密度 | mdpi 1x … xxxhdpi 4x |
| minSdkVersion / targetSdkVersion | 最低 / 目标 API | 兼容范围 / 最新功能 |
| Accessibility | 无障碍 | 所有能力的人都能用 |
| contentDescription | 内容描述 | 给 screen reader 读的 label |

## Cheat sheet

| 主题 | 要点 |
|---|---|
| Compose 积木 | Row, Column, Box + Modifier + Divider |
| Static vs dynamic | Row/Column, ConstraintLayout, WebView · LazyList, LazyGrid |
| **Performance 3 方法** | flat tree + smart recomposition · reusable composables（取代 include）· lazy loading（只画可见的） |
| Components | Button/ImageButton, TextField(keyboardType), Checkbox(多选), Radio(单选), Toggle, Spinner |
| Dark theme | API 29+，省电、可见度、低光；DayNight theme |
| Nav principles | fixed start · stack · Up = Back 在 app 内 · Up 不离开 app · deep link 模拟手动 |
| Navigation component | Graph（XML 定义）· NavHost（容器，拦 Back）· NavController（navigate/popBackStack） |
| Priority | Snackbar low · Notification medium · Dialog high（required） |
| Dialog types | Alert, Simple, Confirmation, Full-screen；≤ 3 buttons |
| Drawer vs Tabs | ≥5 destinations / ≥2 levels / unrelated · related, same level |
| Toolbar | Drawer, Title, Action buttons, Overflow；FIT |
| Languages | res/values-xx/strings.xml，同名不同内容，系统按 locale 选 |
| Screens | size + density；layout-land/large；WindowSizeClass；drawable-mdpi…xxxhdpi（1,1.5,2,3,4x） |
| Versions | minSdk 低 = 更多设备；targetSdk 新 = 新功能；SDK_INT 检查 |
| Accessibility | screen reader labels & grouping · flat nav · 48dp/8dp · contrast & not colour-only · captions · clear labels |

---

## Practice（English, with answers）

### A. MCQ
1. Which component only composes items that are visible on screen? (a) Column (b) LazyColumn (c) Box (d) ConstraintLayout
2. Which should be used to let the user undo deleting an email? (a) Toast (b) Dialog (c) Snackbar (d) Notification
3. The Up button: (a) exits the app from the start destination (b) never exits the app (c) is identical to Home (d) only works in dialogs
4. Where should Bahasa Malaysia strings be stored? (a) res/values-my/ (b) res/values-ms/ (c) res/layout-ms/ (d) res/drawable-ms/
5. Minimum recommended touch-target size: (a) 24 dp (b) 32 dp (c) 48 dp (d) 64 dp

**Answers**：1 (b) · 2 (c) · 3 (b) · 4 (b) · 5 (c)

### B. Short answer
**B1.** Differentiate a Snackbar and a Dialog. (4 marks)
> A Snackbar is low priority: it shows a brief message at the bottom of the screen, the user's action is optional and it disappears automatically. A Dialog is high priority: it interrupts the user and blocks the app until the user chooses an action or dismisses it, so it is used for critical information or decisions.
> 中文：从 priority、是否必须互动、是否自动消失三方面比较。

**B2.** Explain the purpose of the NavHost and the NavController. (4 marks)
> The NavHost is a container that displays the current destination (screen) from the navigation graph and, as the default NavHost, intercepts the system Back button. The NavController is the object that manages navigation within the NavHost, e.g. `navigate("details")` to go forward and `popBackStack()` to go back, maintaining the back stack.

**B3.** Why should minSdkVersion be low and targetSdkVersion be the latest? (4 marks)
> A low minSdkVersion lets the app install on older devices, reaching more users; a latest targetSdkVersion means the app is designed and tested against the newest OS, so it can use the newest features and security behaviour. Runtime checks of `Build.VERSION.SDK_INT` keep new features from crashing old phones.

### C. Application
**C1.** A hospital appointment app shows a list of 2,000 doctors. Suggest how to display it efficiently in Compose. (3 marks)
> Use a LazyColumn with a reusable `DoctorCard` composable: only the visible cards are composed and drawn, cards are composed as the user scrolls, and each card is small so recomposition is cheap.

**C2.** For each, choose Toast, Snackbar, Notification or Dialog: (a) payment will be charged RM300, confirm? (b) appointment tomorrow at 9 am; (c) "Profile saved". (3 marks)
> (a) Dialog – critical decision requiring the user's action. (b) Notification – reminder that must reach the user outside the app. (c) Toast (or Snackbar) – brief feedback while the user is in the app.

**C3.** List THREE accessibility improvements for a bus-ticket app used by elderly users. (3 marks)
> Large touch targets (≥ 48 dp with 8 dp spacing), high colour contrast with text/icons not relying on colour alone (e.g. "Seat available ✓"), and content descriptions for icons so screen readers can read them.

### D. Thinking
**D1.** Why does externalising resources make localisation cheap?
> Because the code refers only to resource names (`R.string.title`); adding a language means adding one folder of translated strings with the same names—no code change, no separate app, and the system picks the right file automatically at runtime.

**D2.** Why is recomposition "smart" a performance feature and not just a convenience?
> Redrawing the whole screen on every state change wastes CPU and battery; recomposing only the composables that read the changed state keeps frames fast (smooth UI) especially on low-end devices, which is exactly the performance goal of flattening hierarchies in the old View system.

---

## Slide index
| Note section | Slides |
|---|---|
| 1–2 Layouts | 2.1 p3–9, p14–17 |
| 3 Performance | 2.1 p10–13 |
| 4 Components | 2.1 p18–32 |
| 5 Style/Theme | 2.1 p33–37 |
| 6 Nav principles | 2.2 p4–11 |
| 7 Fragment | 2.2 p12–20, p40 |
| 8 Navigation | 2.2 p21–39 |
| 9 Dialogs | 2.2 p41–50 |
| 10 Drawer & Tabs | 2.2 p51–57 |
| 11 Menus | 2.3 p3–14 |
| 12 Toast/Snackbar/Notification | 2.3 p15–21 |
| 13 Design for everyone | 2.3 p22–58 |
| 14 Accessibility | 2.3 p59–77 |

## Links to other chapters
- Activity 与 back stack 的关系 → **Ch3**（Activity stack / lifecycle）
- Notification 需要的 push / background work → **Ch4.2**
- App 大小、App Bundles 按 density/语言分发资源 → **Ch7**
