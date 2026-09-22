# AMIT3353 — Chapter 3: Mobile Application Models (Activity, Intent, Lifecycle)

> 考试最重的一章（两份 past year 合计 31 分）。Slide 的 lifecycle 图挤在一页、没有讲每个 callback 为什么存在；onStart 被写成 "becomes interactive"（其实要到 onResume）；onPause 同时说"要保存数据"又说"必须很短"，互相矛盾。这份笔记补上一个完整例子贯穿整个 lifecycle，并给出 explicit/implicit 的判断规则。

---

## 0. 一句话总览

**Activity 是一个画面，放在一个 stack 里；用 Intent（explicit = 指名道姓、implicit = 只说要做什么）启动别的 activity；activity 一生经历 onCreate → onStart → onResume → onPause → onStop → onDestroy，开发者在对的 callback 里开/关资源、保存资料。**

| Part | 内容 | Slide |
|---|---|---|
| 1 | Activity 与 activity stack | p3–12 |
| 2 | Intent 五个部分 | p13–19 |
| 3 | Explicit intent | p20–27 |
| 4 | Implicit intent 与 intent filter | p28–39 |
| 5 | Getting a result from an activity | p40–44 |
| 6 | Activity lifecycle（callbacks + states） | p45–55 |
| 7 | Compose 与 lifecycle | p56–58 |
| 8 | Saving & restoring instance state | p60–63 |

🎯 **这章回答的考题**
- **Oct 2025 Q2c** Identify intent type (explicit/implicit) for 4 MyGOV tasks with justification（4 × 3 = 12）
- **Jan 2026 Q2b** Use a **diagram** to explain the activity lifecycle（9）
- **Jan 2026 Q2c** Explain TWO intent types with an example each for ELSA（10）

**Prerequisites**：Ch1 的 Activity（= 一个 screen）、Manifest（component 必须登记）、sandbox（app 之间不能直接互相呼叫，要透过 OS）。

---

## 1. Activity 与 Activity Stack（p3–12）

- **Activity = UI（一个画面）**。"main" activity 是 app 的第一个画面。
- Activity 可以被同一个 app 或**别的 app** 启动。
- **必须在 Manifest 登记**才能被系统存取：
```xml
<manifest ...>
  <application ...>
    <activity android:name=".ExampleActivity" />
  </application>
</manifest>
```
- 关闭：`finish()` 关掉**目前**的 activity；`finishActivity()` 关掉**之前启动的另一个** activity。

**Activity stack（back stack，LIFO = last in, first out）**：每开一个新 activity，前一个被 **stopped** 并留在 stack 里；按 **Back**，最上面的被 pop（destroy），下面那个 **resume**。

```
Start A1     Start A2      Start A3      Back(Destroy A3)  Back(Destroy A2)
[A1 Main]    [A2     ]     [A3     ]     [A2     ]         [A1 Main]
             [A1 Main]     [A2     ]     [A1 Main]
                           [A1 Main]
```
*重点：永远只有最上面那个看得见；Back 就是把最上面那个丢掉。*

**p12 答案**：1. Activity 的目的：提供一个画面让用户互动。2. 登记在 manifest：否则系统不知道它存在、不能启动。3. 资料结构：**stack（LIFO）**。4. 用户离开 activity：它被 **paused → stopped**，留在 stack 里（state 被保存），内存不够时可能被系统 destroy。

---

## 2. Intent 的五个部分（p13–19）

**Intent** = 一个讯息物件，请 Android 系统去启动 component（activity / service / broadcast）。像一个**公事包**，里面装五样东西：

| 部分 | 意思 | 例子 |
|---|---|---|
| **Component name** | 要启动的 component 的**确切名字**。有它 = **explicit** intent。启动 **Service 时一定要指定**（安全） | `ComposeActivity::class.java` |
| **Action** | 要做的**动作**；决定其他部分（data、extras）怎么组织 | `ACTION_VIEW`、`ACTION_SEND`、`ACTION_DIAL` |
| **Data** | 要处理的资料的 **URI**，和/或它的 **MIME type** | `tel:031234567`、`geo:47.6,-122.3`、`http://...`、`text/plain`、`image/jpeg` |
| **Category** | （Optional）哪一类 component 该处理 | `CATEGORY_BROWSABLE`、`CATEGORY_LAUNCHER` |
| **Extras** | 额外资料的 **key-value pairs**；`putExtra(key, value)` 放进、`getExtras()`/`getStringExtra()` 取出 | `EXTRA_EMAIL`、`EXTRA_SUBJECT` |

p17 的 Action + Data 表：

| Action | Data | Type | 结果 |
|---|---|---|---|
| ACTION_VIEW | content://contacts/people/1 | | 看联络人 1 |
| ACTION_VIEW | tel:031234567 | | 打开拨号 |
| ACTION_VIEW | geo:47.6,-122.3 | | 打开地图 |
| ACTION_VIEW | http://www.example.com | | 打开浏览器 |
| ACTION_SEND | This is my text to send | text/plain | 分享文字 |
| ACTION_SEND | (URI to image) | image/jpeg | 分享图片 |

---

## 3. Explicit Intent（p20–27）

**Explicit = 指名道姓**：明确写出要启动哪个 component（通常是**自己 app 里的** activity）。

```
               1. Activity A creates an Intent
[App: Activity A] ──▶ [Intent] ──▶ [Android System] ──▶ [Start] ──▶ [App: Activity B]
                                   2. 在这个 app 的 components     3. 启动符合的 Activity
                                      里找符合的
```
例（p23–24）：Gmail 主画面按 ✏️ → 开 **Gmail Compose**（同一个 app 的另一个画面）。

```kotlin
// Gmail main activity
val intent = Intent(context, ComposeActivity::class.java)   // 明确的 class
startActivity(intent)

// 带资料过去
const val EXTRA_MESSAGE = "com.example.myfirstapp.MESSAGE"
val intent = Intent(this, DisplayMessageActivity::class.java).apply {
    putExtra(EXTRA_MESSAGE, message)
}
startActivity(intent)

// 接收端
val message = intent.getStringExtra(EXTRA_MESSAGE)
```

---

## 4. Implicit Intent 与 Intent Filter（p28–39）

**Implicit = 只说"我要做什么"**（action + data），不指定是谁；让**其他 app** 中能做这件事的 component 来处理。

```
                1. 建 Intent          2. 在"所有 app"里找 intent filter 符合的
[App A: Activity] ─▶ [Intent] ─▶ [Android System] ─▶ ◇ Match? ─Y─▶ [Start] ─▶ [App B: Activity]
                                                       │N
                                                       ▼
                                                   [App Crash]
```
*重点：explicit 只在"一个 app"里找名字；implicit 在"所有 app"里比对 intent filter；找不到会 crash。*

例（p31–32）：在 The Star 网页上点编辑的 email 地址 → 开 **Gmail Compose**（另一个 app）。

```kotlin
// 分享 / 寄信
val emailIntent = Intent(Intent.ACTION_SEND).apply {
    type = "text/plain"                                   // 没有 URI，所以给 MIME type
    putExtra(Intent.EXTRA_EMAIL, arrayOf("jon@example.com"))
    putExtra(Intent.EXTRA_SUBJECT, "Email subject")
    putExtra(Intent.EXTRA_TEXT, "Email message text")
}
// 拨号
val callIntent = Intent(Intent.ACTION_DIAL, Uri.parse("tel:5551234"))
// 地图
val mapIntent = Intent(Intent.ACTION_VIEW, Uri.parse("geo:0,0?q=1600+Amphitheatre+Parkway"))
```

- **多个 app 都符合** → 系统显示 **chooser dialog** 让用户选（p35：Chrome、Hover Browser、Internet、Link Bubble，按 "Always" 或 "Just once"）。
- **没有 app 能处理** → 会 crash，所以先检查：
```kotlin
if (sendIntent.resolveActivity(packageManager) != null) startActivity(sendIntent)
else { /* 告诉用户没有 app 可处理 */ }
```
- **API 30+（package visibility）**：要在 manifest 用 `<queries>` 声明你会找哪类 app（browser、camera `IMAGE_CAPTURE`、gallery `GET_CONTENT`）。

**Intent filter（p39）**：一个 app 在 manifest 里**宣告"我能接收哪些 implicit intents"**：
```xml
<activity android:name="ShareActivity">
  <intent-filter>
    <action android:name="android.intent.action.SEND"/>
    <category android:name="android.intent.category.DEFAULT"/>
    <data android:mimeType="text/plain"/>
  </intent-filter>
</activity>
```
→ ShareActivity 会出现在"分享文字"的 chooser 里。

### 判断规则：Explicit 还是 Implicit？

> **问：我知道（而且拥有）目标画面的 class 名吗？**
> - **是，是我 app 自己的画面** → **Explicit**
> - **否，我只要"某个 app 做某个动作"（寄信、打电话、开地图、分享、开网页、拍照）** → **Implicit**

**p38 答案**：1. 差别：explicit 指定 component 名字（通常是自己 app 的）；implicit 只声明 action/data，由系统找其他 app 处理。2. a. 分享网站 URL → **Implicit**（ACTION_SEND，让用户选 WhatsApp/Telegram…）；b. 听下载的音频档 → **Implicit**（ACTION_VIEW + audio MIME，交给音乐播放器）；c. 把 app 的音效设定改成静音 → **Explicit**（打开**自己 app 的** settings 画面）。

### ✅ 满分答法 — Oct 2025 Q2c Identify the suitable intent type for each MyGOV task（4 × 3 分）

> **(i) Navigating from the home screen to the settings screen – Explicit intent.** Both screens belong to MyGOV itself, so the app knows the exact component name and starts it directly, e.g. `Intent(this, SettingsActivity::class.java)` (or `navController.navigate("settings")` in Compose). No other app is involved.
>
> **(ii) Sending an email to government support – Implicit intent.** MyGOV does not send email itself; it declares the general action `ACTION_SEND`/`ACTION_SENDTO` with the support address, subject and message as extras, and Android finds an email app (Gmail, Outlook) whose intent filter matches, letting the user choose.
>
> **(iii) Creating a new request for driving licence renewal – Explicit intent.** The renewal form is a screen inside MyGOV, so the app starts its own `LicenceRenewalActivity` by name and can pass data such as the user's IC number with `putExtra()`.
>
> **(iv) Opening a map to navigate to the nearest UTC – Implicit intent.** MyGOV uses `ACTION_VIEW` with a `geo:` URI (e.g. `geo:0,0?q=UTC+Kuala+Lumpur`), and the system hands it to an installed map app such as Google Maps or Waze, which provides turn-by-turn navigation.

### ✅ 满分答法 — Jan 2026 Q2c Explain TWO Android intent types with an example each for ELSA（10 分 = 2 × 5）

> **1. Explicit intent** – An explicit intent specifies the exact component (fully-qualified class name) to start, so it is used to launch a component within the same app. The Android system looks only for that named component and starts it; data can be passed with `putExtra()` and read with `getStringExtra()`.
> *ELSA example*: after the AI diagnostic is complete, the user taps "Apply for financing"; ELSA starts its own `FinancingApplicationActivity` with `Intent(this, FinancingApplicationActivity::class.java).putExtra("SME_ID", smeId)` so the form can be auto-filled with the diagnostic results.
>
> **2. Implicit intent** – An implicit intent does not name a component; it declares a general action (and data/MIME type), e.g. `ACTION_VIEW`, `ACTION_SEND`, `ACTION_DIAL`. The system searches all installed apps for an intent filter that matches; if several match, a chooser is shown, and if none match the app must handle it (check `resolveActivity()`) to avoid crashing.
> *ELSA example*: to share an SME's business report with a partner, ELSA creates `Intent(Intent.ACTION_SEND)` with type `application/pdf` and the report URI, letting the user choose WhatsApp, Gmail or Telegram; or it uses `ACTION_DIAL` with `tel:` to call an SME Bank advisor.

💬 **答题句 (EN)** — *An explicit intent names the exact component to start and is used within the same app; an implicit intent only declares an action and data, and the system starts a component from another app whose intent filter matches.*

---

## 5. Getting a Result from an Activity（p40–44）

有时开了另一个 activity，还要**拿回结果**（例：选一个联络人、选一张相片）。

```
普通：   [Activity A] ─Intent─▶ startActivity() ─▶ [Activity B]

要结果：  [Activity A] ─Intent + Request Code─▶ startActivityForResult() ─▶ [Activity B]
              ▲                                                            │
              └──── onActivityResult() ◀── Request Code + Result Code + Data ┘
```
- **Request code**：分辨是哪一个请求回来的。
- **Result code**：`RESULT_OK` 或 `RESULT_CANCELED`。
- **Data**：回传的 Intent（例 contact 的 URI）。

旧方法 `startActivityForResult()` + `onActivityResult()` 已 **deprecated**；新方法 **Activity Result API**：
```kotlin
val getContent = registerForActivityResult(ActivityResultContracts.GetContent()) { uri: Uri? ->
    imageView.setImageURI(uri)          // 处理回传的 Uri
}
getContent.launch("image/*")            // 打开 gallery 选图片
```

---

## 6. ⭐ Activity Lifecycle（p45–55）

**为什么要 lifecycle？** 手机资源有限，用户随时会切换 app、接电话、转屏幕。系统用 callbacks 通知 activity："你要被遮住了 / 你看不见了 / 你要被毁掉了"，让你在对的时候**开启与释放资源**、**保存资料**。

### 6.1 Lifecycle 图（照 slide p46 画——考试 Jan 2026 Q2b 9 分）

```
                    [Activity launched]
                            │
          ┌───────────▶ onCreate()
          │                 │
          │             onStart() ◀──────────── onRestart() ◀──────┐
          │                 │                                      │
          │             onResume() ◀──────────────────┐            │
          │                 │                         │            │
          │         [Activity running]                │            │
          │                 │                         │ User       │ User
          │  Another activity comes into the          │ navigates  │ navigates
          │  foreground                               │ to the     │ to the
          │                 ▼                         │ activity   │ activity
 [App process  ◀──┐     onPause() ────────────────────┘            │
  killed]         │         │                                      │
    ▲             │  The activity is no longer visible             │
    │ User navigates        ▼                                      │
    │ to the activity   onStop() ──────────────────────────────────┘
    │ (→ onCreate)  │         │
    └── Apps with higher      │ The activity is finishing or being
        priority need memory  │ destroyed by the system
        (from onPause/onStop) ▼
                           onDestroy()
                                │
                        [Activity shut down]
```
*读图重点：三个回圈——(1) onPause → 用户回来 → **onResume**；(2) onStop → 用户回来 → **onRestart → onStart**；(3) 进程被系统杀掉（内存不足）→ 用户回来 → 从 **onCreate** 重新开始。*

### 6.2 六（七）个 callbacks

用一个例子贯穿：**Spotify-like 音乐 app 的播放画面**。

| Callback | 什么时候 | 做什么（slide） | 音乐 app 例子 |
|---|---|---|---|
| **onCreate()** | activity 被**建立**，一生只一次 | 只做一次的启动逻辑：设 UI（`setContent{}`）、建 ViewModel/LiveData/state、注册 listeners、检查 permission | 建立画面、ViewModel，读播放清单 |
| **onStart()** | activity 变得**可见** | 注册 BroadcastReceivers（UI 需要更新时）、开始动画或 background services、载入轻量 UI 资料 | 注册"耳机拔出"的 receiver |
| **onResume()** | activity 到**前景**，可以互动；可能被叫多次（onStart 后第一次；onPause 后用户回来） | 初始化在 onPause 释放掉的东西；例 **开始 camera preview** | 恢复进度条动画、重新取得 audio focus |
| **onPause()** | 用户**正在离开**（不一定会被 destroy）；不在前景但在 multi-window 仍**部分可见** | 必须**很短**：停止耗 CPU 的东西、暂停动画/媒体/实时更新、释放只在前景用的 sensor；提交**轻量**的未保存修改 | 暂停进度条动画（视频 app 在此暂停播放） |
| **onStop()** | 完全**看不见** | 释放几乎所有不需要的资源；做**较耗 CPU 的关闭工作**，例**存到 database** | 把"最后播放位置"写进 Room DB |
| **onRestart()** | 从 stopped 回来前 | — | — |
| **onDestroy()** | `finish()` 被呼叫、系统为了省空间毁掉进程、或 **configuration change**（转屏） | 释放**所有**资源；⚠️ 系统**可能跳过**这个 callback | 释放 MediaPlayer |

### 6.3 Lifecycle states（p47）

| State | Resumed ▶ | Paused ⏸ | Stopped ⏹ | Destroyed ✖ |
|---|---|---|---|---|
| Visible | Yes | Yes (partially) | No | No |
| User interaction | Yes | No | No | No |
| Code execution | Yes | No | No | No |
| Instance state | Saved | Saved | Saved | **Destroyed** |

### 6.4 Configuration change（p55）

转屏、改语言、接上键盘 = **configuration change** → 目前 activity 被**摧毁再重建**：onPause → onStop → onDestroy → onCreate → onStart → onResume。
→ 这就是为什么输入到一半的文字会不见——要用 **instance state**（第 8 节）或 **ViewModel**（Ch4.1）保住。

### ✅ 满分答法 — Jan 2026 Q2b Use a diagram to explain the activity lifecycle for ELSA（9 分）

> *(Draw the diagram in 6.1: onCreate → onStart → onResume → Activity running → onPause → onStop → onDestroy → shut down, with the three return paths: onPause → onResume; onStop → onRestart → onStart; process killed → onCreate.)*
>
> - **onCreate()** – called once when ELSA's dashboard activity is created: set up the Compose UI, create the ViewModel and check permissions.
> - **onStart()** – the dashboard becomes visible; ELSA registers a receiver for connectivity changes and loads lightweight data such as the SME's latest score.
> - **onResume()** – the activity is in the foreground and interactive; ELSA starts refreshing real-time insights. It is called again every time the user returns from onPause.
> - **onPause()** – the user is leaving (e.g. a phone call arrives or another app opens in multi-window); ELSA pauses the live refresh and animations and quickly keeps unsaved form input.
> - **onStop()** – the activity is no longer visible; ELSA releases resources and saves the partially-filled financing application to its local database. If the user returns, onRestart() → onStart() is called.
> - **onDestroy()** – called when the user finishes the activity, on a configuration change (e.g. rotating the phone), or when the system kills the process; ELSA releases all remaining resources. If the process was killed because higher-priority apps needed memory, returning to ELSA starts again from onCreate().

**p59 答案**：
1. onCreate 重要：做一次性的初始化（UI、ViewModel）；onPause 重要：用户离开的第一个信号，是停止耗资源工作、保住未保存资料的最后可靠机会之一。
2. onPause 与 onStop **不会被叫**的情况：系统在 activity 还没到 paused/stopped 前就**杀掉进程**（例如 app 在 onCreate 中呼叫 `finish()` → 直接 onDestroy）；或手机突然没电/崩溃。
3. a. 播放背景音乐 → **onStart/onResume 开始，但真正背景播放应交给 Service**；b. 保存游戏关卡 → **onPause（轻量）/ onStop（写入 DB）**；c. 连线到 server → **onStart / onResume**（离开时在 onStop 断开）。
4. 存 database 的两个 callbacks：**onPause() 与 onStop()**（重的写入放 onStop）。

---

## 7. Jetpack Compose 与 Lifecycle（p56–58）

Compose 跟随 activity 的 lifecycle，但 composable 有自己的生命周期（进入/离开 composition、recomposition），**没有直接的 callback**，改用 effects：

```kotlin
@Composable
fun MyScreen() {
    LaunchedEffect(Unit) { Log.d("Lifecycle", "onStart equivalent") }   // 进入 composition 时执行一次

    val lifecycleOwner = LocalLifecycleOwner.current
    LaunchedEffect(lifecycleOwner) {
        lifecycleOwner.lifecycle.addObserver(object : DefaultLifecycleObserver {
            override fun onResume(owner: LifecycleOwner) { Log.d("Lifecycle", "Screen Resumed") }
            override fun onPause(owner: LifecycleOwner)  { Log.d("Lifecycle", "Screen Paused") }
        })
    }
}
// 要做 lifecycle 相关的非同步工作（例 fetch data）→ rememberCoroutineScope() + launch { ... }
```
⚠️ 正式写法应该用 `DisposableEffect` 在离开时 `removeObserver`，避免 leak（slide 省略了）。

---

## 8. Saving & Restoring Instance State（p60–63）

**Instance state** = 系统用来**恢复先前状态**的已保存资料，是存在 **Bundle** 里的 **key-value pairs**（例：用户在 text field 输入的值）。

p61 图的故事：
```
1. 正在打讯息 "Let us meet at 9 am"（还没送出）
        │ Save instance state ──▶ [Bundle: unsent message]
2. 妈妈来电，电话 app 盖上来（你的 activity 可能被销毁）
        │ Restore instance state ◀── [Bundle]
3. 回来时讯息还在
```

```kotlin
override fun onSaveInstanceState(outState: Bundle) {
    outState.putString(MSG_VIEW_KEY, textViewMsg.text.toString())
    super.onSaveInstanceState(outState)            // 让系统也保存 view hierarchy
}
override fun onRestoreInstanceState(savedInstanceState: Bundle) {
    super.onRestoreInstanceState(savedInstanceState)
    textViewMsg.text = savedInstanceState.getString(MSG_VIEW_KEY)
}
// Compose：rememberSaveable { mutableStateOf("") } 会自动存进 Bundle
```

**p63 答案（multi-window / 多工时不丢资料）**：在 **onPause** 停止更新并保留轻量资料；在 **onStop** 把重要资料存进 database/DataStore；用 **onSaveInstanceState / rememberSaveable** 保存 UI 状态（输入到一半的文字、滚动位置）；用 **ViewModel** 保存会在 configuration change 后存活的资料；回来时在 onCreate/onRestoreInstanceState 或从 ViewModel 恢复。

---

## ⚠️ Where the slides mislead

| Slide | Slide 写的 | 更准确的理解 |
|---|---|---|
| p49 | onStart："Activity enters the foreground; Activity becomes interactive" | onStart 只是**可见**；要到 **onResume** 才在前景并可互动（p50 与 p47 的 state 表也这样说）。 |
| p52 vs p53 | onPause "commit any changes… (save unsaved data)" 但又 "execution must be very brief"；onStop "save information to a database" | 不矛盾的理解：onPause 只做**很轻**的保存；**写 database 这类重工作放 onStop**（官方建议）。 |
| p33 | `Intent emailIntent = Intent(Intent.ACTION_SEND)` / `HTTP.PLAIN_TEXT_TYPE` / `EXTRA_EMAIL, "jon@..."` | Java 与 Kotlin 混写；`EXTRA_EMAIL` 应该放 **String array**；MIME type 直接写 `"text/plain"`。 |
| p13 | Explicit = "to start a component in your own app" | 通常如此；也可以用明确 package/class 名启动别的 app 的 component（例如知道 package 名时），但一般考试答"own app"。 |
| p57 | 用 LaunchedEffect 加 observer | 缺少移除 observer；正确要用 `DisposableEffect { ...; onDispose { removeObserver } }`。 |

---

## Term table

| English | 中文 | 一句话 |
|---|---|---|
| Activity stack / back stack | 活动堆叠 | LIFO，Back 会 pop 最上面的 |
| Intent | 意图 | 请求启动 component 的讯息 |
| Explicit intent | 显式意图 | 指定 component 名字 |
| Implicit intent | 隐式意图 | 只声明 action + data |
| Intent filter | 意图过滤器 | 宣告 app 能接收哪些 implicit intents |
| Action / Data / Category / Extras | 动作 / 资料 / 类别 / 附加资料 | Intent 的组成 |
| URI / MIME type | 资源标识 / 资料类型 | `tel:`, `geo:` / `text/plain` |
| Chooser | 选择器 | 多个 app 都能处理时让用户选 |
| Activity Result API | 结果 API | 取代 startActivityForResult |
| Lifecycle callback | 生命周期回调 | onCreate … onDestroy |
| Configuration change | 配置变更 | 转屏、改语言 → activity 重建 |
| Instance state / Bundle | 实例状态 | key-value 保存 UI 状态 |

## Cheat sheet

| 项目 | 要点 |
|---|---|
| Stack | LIFO；新 activity → 旧的 stopped；Back → pop + destroy |
| Close | `finish()` 自己；`finishActivity()` 之前开的另一个 |
| Intent 5 parts | Component name · Action · Data (URI/MIME) · Category · Extras |
| Explicit | 名字已知、自己 app；System 在该 app 找 |
| Implicit | action+data；System 在所有 app 找 intent filter；多个 → chooser；无 → crash（先 `resolveActivity`） |
| API 30+ | `<queries>` 声明 package visibility |
| Result | request code + result code + data；新：`registerForActivityResult` |
| Lifecycle | onCreate → onStart → onResume → [running] → onPause → onStop → onDestroy；onRestart |
| 回圈 | pause→resume；stop→restart→start；killed→create |
| States | Resumed (visible, interact) · Paused (partly visible) · Stopped (not visible) · Destroyed |
| 存资料 | onPause 轻量 · onStop 写 DB · onSaveInstanceState 存 UI 状态 |
| Compose | LaunchedEffect、lifecycle observer、rememberCoroutineScope、rememberSaveable |

---

## Practice（English, with answers）

### A. MCQ
1. Which callback is called only once in an activity's life? (a) onStart (b) onResume (c) onCreate (d) onPause
2. An intent that uses `ACTION_DIAL` with `tel:` is: (a) explicit (b) implicit (c) broadcast (d) pending
3. In which state is an activity partially visible but not interactive? (a) Resumed (b) Paused (c) Stopped (d) Destroyed
4. Rotating the screen causes the activity to: (a) only call onPause (b) be destroyed and recreated (c) call onRestart (d) nothing
5. Which part of an Intent carries key-value data? (a) Action (b) Category (c) Extras (d) Component name

**Answers**：1 (c) · 2 (b) · 3 (b) · 4 (b) · 5 (c)

### B. Short answer
**B1.** What happens if an implicit intent has no matching app, and how do you prevent a crash? (3 marks)
> The system cannot find any activity whose intent filter matches, so `startActivity()` throws an exception and the app crashes. Check `intent.resolveActivity(packageManager) != null` before starting it and inform the user otherwise (and declare `<queries>` on API 30+).

**B2.** Explain the purpose of an intent filter with an example. (4 marks)
> An intent filter, declared in the manifest, advertises which implicit intents a component can receive (by action, category and data type). E.g. an activity with `<action SEND/>`, `<category DEFAULT/>` and `<data mimeType="text/plain"/>` will appear in the share chooser whenever another app shares text.

**B3.** Differentiate onPause() and onStop(). (4 marks)
> onPause() is called when the user starts leaving the activity; it may still be partially visible (multi-window) and must be brief—pause animations/media, release foreground-only sensors. onStop() is called when the activity is no longer visible; it releases almost all resources and performs heavier work such as saving data to a database.

### C. Application
**C1.** A food-ordering app: identify explicit/implicit for (a) opening the cart screen, (b) calling the restaurant, (c) sharing a promo code to WhatsApp. (3 × 2 marks)
> (a) Explicit – the cart is the app's own activity. (b) Implicit – `ACTION_DIAL` with `tel:` handled by the phone app. (c) Implicit – `ACTION_SEND` with `text/plain`, user picks WhatsApp in the chooser.

**C2.** A user is writing a long review when a call comes in; after the call the text is gone. Explain why and fix it. (4 marks)
> The activity went to the background and was destroyed (low memory) or recreated, so the unsaved UI state was lost. Save the text in `onSaveInstanceState()` (or use `rememberSaveable` / a ViewModel) and restore it in `onRestoreInstanceState()`/onCreate; also save a draft in onStop.

**C3.** Trace the callbacks when the user opens an app, presses Home, then returns. (3 marks)
> onCreate → onStart → onResume (running) → Home: onPause → onStop → return: onRestart → onStart → onResume.

### D. Thinking
**D1.** Why should a camera preview start in onResume() and stop in onPause()?
> The camera is an exclusive, battery-heavy resource only needed while the activity is in the foreground. Releasing it in onPause lets other apps (e.g. the phone's camera app in multi-window) use it and saves battery; onResume re-acquires it each time the user returns.

**D2.** Why can't you rely on onDestroy() to save data?
> The system may kill the process without calling onDestroy (e.g. under memory pressure), so data saved only there can be lost. Data should be saved in onPause/onStop, which are reliably called before the activity becomes killable.

---

## Slide index
| Note section | Slides |
|---|---|
| 1 Activity & stack | p3–12 |
| 2 Intent parts | p13–19 |
| 3 Explicit | p20–27 |
| 4 Implicit & filter | p28–39 |
| 5 Result | p40–44 |
| 6 Lifecycle | p45–55, p59 |
| 7 Compose | p56–58 |
| 8 Instance state | p60–63 |

## Links to other chapters
- Activity/Fragment 与 navigation stack → **Ch2.2**
- ViewModel（在转屏后存活）、Room（onStop 存资料）→ **Ch4.1**
- 背景播放音乐应该用 Service → **Ch1, Ch4.2**
- Camera intent / Activity Result API 拍照 → **Ch6**
- 地图 implicit intent、location 在 onStart/onStop 开关 → **Ch5**
