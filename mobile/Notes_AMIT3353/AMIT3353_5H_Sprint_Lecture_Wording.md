# AMIT3353 — 5 小时冲刺笔记（老师用词版）

> 这份笔记只做一件事：**用老师 slide 上的原句**把两份 past year（Oct 2025 MyGOV、Jan 2026 ELSA）会考的内容全部过一遍。
> - 📖 **Slide 原句**：直接从 slide 抄下来的英文（标页码），考试时优先用这些字眼。
> - ✍️ **考试写法**：用 slide 原句组合成的满分答案，再加一句套到 case app（MyGOV / ELSA）。
> - ⚠️：slide 本身写错或写得不清楚的地方，只提一句，告诉你怎么写才两边都不扣分。
> - 中文只用来说明"怎么用"，不改老师的英文。
>
> 顺序按**两份考卷合计分数**由高到低排，时间不够就从上往下读。

---

## 0. 5 小时时间表

| 时间 | 读哪一节 | 两份卷合计分 | 做什么 |
|---|---|---|---|
| 0:00–0:40 | §1 Compose layout performance + §2 Intent | 24 + 22 | 背 3 个 method 的原句；练 explicit/implicit 判断 |
| 0:40–1:20 | §3 Camera + sensors | 25 | 背 2 methods、manifest 两条、3 类 sensors |
| 1:20–1:50 | §4 Activity lifecycle | 9 | **默写图 2 次** |
| 1:50–2:20 | §5 OS + challenges + beyond | 26 | 背 4 challenges 的字眼 |
| 2:20–2:40 | 休息 | | |
| 2:40–3:20 | §6 Storage + §7 Architecture / threads / server | 19 + 19 | 背 internal vs external 表；画 architecture 图 |
| 3:20–3:50 | §8 Location | 18 | 背 GPS vs Wi-Fi 表、foreground vs background 表 |
| 3:50–4:20 | §9 Distribution + monetisation + §10 Design for everyone / notifications | 19 + 19 | 背 8 种 monetisation 的关键字 |
| 4:20–5:00 | §11 最后 40 分钟 | | 不看笔记，把 §11 的 20 条默写一次 |

**答题规则（这门课的评分习惯）**：每一点 = **slide 原句的概念名 + slide 原句的解释 + 套到 MyGOV/ELSA 的一句**。分数 ÷ 3 ≈ 要写几点。题目说 "in a table format" 就画表，"using a diagram" 就画图。

---

## 1. Jetpack Compose — Layout Performance（Oct Q1b 15 分、Jan Q1b 9 分）⭐⭐⭐

📖 **Slide 原句（2.1 p10–13）**

Improving Layout Performance in traditional Android:
1. Optimizing Layout hierarchies
2. Re-using Layouts with `<include>` tag
3. Loading Views on demand

How Jetpack compose handle Performance issues?
1. **Optimizing Layout hierarchies**
   - "In Jetpack compose, there is no more traditional View hierarchy"
   - "Compose uses a flat tree of Composable and only recompose the parts that change."
   - "Compose layouts are lightweight and designed for performance"
2. **Re-using Layouts with `<include>` tag**
   - "Instead of using `<include>` tag, Jetpack compose uses composable functions"
   - "Best practice for Jetpack Compose is to break UI into small reusable Composable."
3. **Loading Views on demand**
   - "Jetpack Compose handles on-demand rendering natively with components like LazyColumn and smart recomposition."
   - "Only visible items are composed and drawn"

另外（2.1 p8）："Lazy Row or Lazy Column do not require adapters like traditional Android RecyclerView"

✍️ **考试写法（Oct Q1b，15 分 = 3 × 5，MyGOV）**
> **1. Optimizing Layout hierarchies** – In Jetpack Compose, there is no more traditional View hierarchy. Compose uses a flat tree of Composable and only recompose the parts that change, and Compose layouts are lightweight and designed for performance. *MyGOV:* when an application status changes from "Processing" to "Approved", only the status Composable is recomposed, not the whole service page.
>
> **2. Re-using Layouts (instead of the `<include>` tag)** – Instead of using the `<include>` tag, Jetpack Compose uses composable functions. Best practice is to break the UI into small reusable Composables. *MyGOV:* one `ServiceCard` Composable is reused for every government service (road tax, income tax, healthcare), keeping the design consistent.
>
> **3. Loading Views on demand** – Jetpack Compose handles on-demand rendering natively with components like LazyColumn and smart recomposition; only visible items are composed and drawn, and Lazy Row / Lazy Column do not require adapters like the traditional RecyclerView. *MyGOV:* the list of 100+ services and the payment history are shown with LazyColumn so the app opens fast on low-end phones.

Jan Q1b（9 分 = 3 × 3，ELSA）：同样 3 个标题 + 每个一句原句 + 一句 ELSA（AI diagnostic score 只 recompose 那一块；`ProgramCard` 重复用；LazyColumn 显示 learning modules）。

---

## 2. Intent（Oct Q2c 12 分、Jan Q2c 10 分）⭐⭐⭐

📖 **Slide 原句（3 p13–39）**

- "Intent is used to start an Activity"
- **Explicit**: "To start a component in your own app. You need to supply a target app's package name or a fully-qualified component class name"
- **Implicit**: "Declare a general action to perform, which allows a component from another app to handle it"
- "An Intent carries the following information that the Android System uses to perform the action": **Component Name, Action, Data, Category, Extra**
  - Component Name: "The exact name of the component to start"
  - Action: "It determines how the rest of the Intent is structured – data and extras. Common actions are: ACTION_VIEW and ACTION_SEND"
  - Data: "The Uniform Resource Identifier (URI) that references the data to be acted on and/or the … MIME type of that data"
  - Category: "Optional. Additional information about the kind of component that should handle the intent"
  - Extra: "Key-value pairs that carry additional information required to accomplish the requested action"
- Explicit 的步骤："1. Activity A creates an Intent 2. Android System searches all components of an app that matches the intent 3. The system start the matching Activity"
- Implicit 的步骤："1. Activity A creates an Intent 2. Android System searches all app for an **intent filter** that matches the intent 3. The system start the matching Activity"（没有 match → App Crash）
- "If multiple intent filters are compatible, the system displays a dialog so the user can pick which app to use"
- Intent Filter："To advertise which implicit intents your app can receive using the `<intent-filter>` element"

**判断规则**：目的地是**自己 app 里的画面** → Explicit；要**别的 app**（email、map、电话、分享、相机 app）来做 → Implicit。

✍️ **考试写法（Oct Q2c，4 × 3 分，MyGOV）**
> (i) **Home screen → Settings screen: Explicit intent.** It starts a component in MyGOV's own app, so the app supplies the fully-qualified component class name, e.g. `Intent(this, SettingsActivity::class.java)`.
> (ii) **Send an email to government support: Implicit intent.** MyGOV declares a general action to perform (`ACTION_SEND`) with the address and message as Extras, which allows a component from another app (an email app) to handle it.
> (iii) **Create a new driving licence renewal request: Explicit intent.** The renewal form is a component in MyGOV's own app; data such as the IC number is passed with `putExtra()`.
> (iv) **Open a map to the nearest UTC: Implicit intent.** MyGOV declares `ACTION_VIEW` with a `geo:` URI as the Data, and the Android System searches all apps for an intent filter that matches (e.g. Google Maps, Waze).

✍️ **Jan Q2c（2 × 5 分，ELSA）**：每种写 = slide 定义 + slide 的 3 步流程 + 一个 ELSA 例子（Explicit：从 dashboard 开 ModalNiaga 申请画面；Implicit：`ACTION_SEND` 分享 AI 报告 / `ACTION_VIEW` 开网页）。

---

## 3. Camera, Manifest, Sensors（Oct Q4b 9 分、Jan Q4a 16 分）⭐⭐⭐

📖 **Slide 原句（6 p3–10）**

- 选择流程（p3）："Camera is a must?" No → **Get photo from the Gallery**；Yes → "Need special processing?" No → **Use the existing camera app**；Yes → **Write your own camera module**
- **Use an existing camera app**: "Simple implementation - using Intent Class; Obtain a photo from the camera app; No need permission"
- **Build your own camera function**: "Complex implementation - write your own code using CameraX or Camera2 APIs; Create a camera preview; Request Camera permission"
- Using Camera Intent: "A quick way to enable taking pictures or videos in your app with very minimum coding efforts"
- **Manifest Declarations**:
  - **Camera Features** – "Prevents your app from being installed to devices that do not include a camera" → `<uses-feature android:name="android.hardware.camera" />`
  - **Camera Permission** → `<uses-permission android:name="android.permission.CAMERA" />`（slide 从第 3 项开始编号，第 1–2 项就是这两条）
  - 3. Storage Permission（`WRITE_EXTERNAL_STORAGE`）· 4. Audio Recording Permission（`RECORD_AUDIO`）· 5. Location Permission（`ACCESS_FINE_LOCATION`）
- 相机用途（p12–14）：Claim vehicle insurance、Cheque deposit、Search、Eye test、**Scan document**、**Recognize object and text**

✍️ **Oct Q4b(i) 两种方法（6 分）**
> 1. **Use an existing camera app** – a simple implementation using the Intent class (`MediaStore.ACTION_IMAGE_CAPTURE`); the app obtains a photo from the camera app, with very minimum coding efforts and no need for permission.
> 2. **Build your own camera function** – a complex implementation where the developer writes their own code using CameraX or Camera2 APIs, creates a camera preview inside the app, and must request Camera permission.

**(ii) 最适合 MyGOV（3 分）**：Use the existing camera app — MyGOV only needs to snap supporting documents (IC, receipts) with no special processing, so the Intent method is quick, needs no permission and works on every phone.

✍️ **Jan Q4a(i) 两条 manifest declaration（6 分）**
> 1. **Camera Features** – `<uses-feature android:name="android.hardware.camera" />` prevents ELSA from being installed to devices that do not include a camera.
> 2. **Camera Permission** – `<uses-permission android:name="android.permission.CAMERA" />` is required when ELSA builds its own camera function with CameraX to scan documents.

**(ii) 两个 scenario（4 分）**：**Scan document**（SSM 证书、发票 → 自动填融资表格）；**Recognize object and text**（扫 QR code 报名培训 / 读取文件文字）。

📖 **Sensors（6 p29–45）** — "Three categories: Motion, Environmental, Position"

| Category | Slide 原句 | Sensors（slide 原句） |
|---|---|---|
| **Motion** | "Measure acceleration forces and rotational forces along three axes" | **Accelerometer** – "Provides data on the rate of change in velocity … It senses axis orientation"；**Gravity sensor** – "measures the direction and intensity of gravity"；**Gyroscope** – "helps the accelerometer out with understanding which way your phone is orientated"；**Rotational vector** – "To monitor and measure turning movements" |
| **Environmental** | "Measure various environmental parameters, such as ambient air temperature and pressure, illumination, and humidity" | **Barometer** – "measures atmospheric pressure … assists the GPS chip … to get a faster lock by instantly delivering altitude data"；**Photometer / Light sensor** – "senses the amount of ambient light present"；**Thermometer** – "measures temperature within a mobile device … To avoid overheating" |
| **Position** | "Measure the physical position of a device" | **Orientation sensor** – "Measures the orientation of a device relative to an orthogonal coordinate frame"；**Magnetometer** – "Determines your location with respect to Magnetic North" |

⚠️ Slide 写 accelerometer "excluding gravity"——其实包含重力；考试可照写 "rate of change in velocity" 与 "senses axis orientation"，不写 "excluding gravity" 就不会错。

✍️ **Jan Q4a(iii) 两个 sensor 提升 user experience（6 分）**：
> 1. **Light sensor (Environmental)** – it senses the amount of ambient light present, so ELSA automatically adjusts brightness / dark theme when SME owners read reports in a dim shop.
> 2. **Accelerometer (Motion)** – it senses axis orientation, so ELSA switches charts of the AI diagnostic to landscape when the phone is rotated, making them easier to read.

---

## 4. Activity Lifecycle — 画图（Jan Q2b 9 分）⭐⭐

📖 **Slide 原图（3 p46）照抄标签**：

```
        Activity Launched
               │
               ▼
  ┌────────► onCreate()
  │            │
  │            ▼
  │         onStart() ◄───────────────────── onRestart()
  │            │                                  ▲
  │            ▼                                  │
  │         onResume() ◄─────────┐                │
  │            │                 │                │
  │            ▼                 │ (A)            │ (B)
  │      Activity Running        │ User navigates │ User navigates
  │            │                 │ to the activity│ to the activity
  │            │ Another activity comes           │
  │            │ into the foreground              │
  │            ▼                 │                │
  │         onPause() ───────────┘                │
  │            │                                  │
  │            │ The activity is no longer visible│
  │            ▼                                  │
  │         onStop() ─────────────────────────────┘
  │            │
  │ (C)        │ The activity is finishing or
  │ User       │ being destroyed by the system
  │ navigates  ▼
  │ to the   onDestroy()
  │ activity   │
  │            ▼
  │      Activity shut down
  │
  └──── App process killed ◄── Apps with higher priority need memory
                                (from onPause() or onStop())
```
*画图时一定要有：6 个 callback + onRestart、3 条回来的路——(A) onPause → onResume；(B) onStop → onRestart → onStart；(C) App process killed → onCreate——以及上面这些英文标签（都是 slide 原文）。*

📖 **每个 callback 的 slide 原句（p48–54）**

| Callback | Slide 原句 |
|---|---|
| **onCreate** | "It creates the Activity. Performs basic application startup logic that should happen only once for the entire life of the activity" |
| **onStart** | "Makes the Activity visible to the user. Activity enters the foreground" |
| **onResume** | "Activity comes to the foreground. Could be called several times … Initialize components that you release during onPause()" |
| **onPause** | "User is leaving an activity. Does not always mean the activity is being destroyed. The Activity is no longer in the foreground but still visible in multi-window mode … Execution must very brief" |
| **onStop** | "An activity is no longer visible to the user. Releases almost all resources that aren't needed. Performs relatively CPU-intensive shutdown operations E.g. save information to a database" |
| **onDestroy** | "It is called because the finish() is called or the system destroying the process containing the activity to save space … Releases all resources" |

⚠️ Slide p49 在 onStart 也写了 "Activity becomes interactive"。安全写法：onStart 写 "visible"，把 "interactive / user can interact" 写在 **onResume**（p47 的 States 表也显示只有 Resumed 状态 "User Interaction: Yes"）。

States 表（p47）：Resumed（visible, interaction yes）· Paused（partially visible, no interaction）· Stopped（not visible）· Destroyed。

---

## 5. Mobile OS · Key Challenges · Beyond Mobile App（Oct Q1a 10、Jan Q1a 16）⭐⭐

📖 **Slide 原句（1 p16–28）**：slide 在这部分只有**关键字**，所以考试就用这些关键字当骨架。

| OS | Slide 关键字 |
|---|---|
| **Android** | Linux kernel · Open Source |
| **iOS** | Unix kernel · （Apple 的 macOS 同源） |

✍️ **Oct Q1a（MyGOV）**：
> **Android** – an open-source mobile OS based on the Linux kernel, used by many device manufacturers at different price points, so MyGOV reaches the largest group of Malaysians. **iOS** – Apple's mobile OS based on the Unix kernel, running on a consistent set of iPhone devices, so MyGOV is stable and secure for MyDigital ID logins and ePayment. Together they cover almost all smartphones in Malaysia.

📖 **Key Mobile Challenges（p23–24）**："**Processing Power · Battery · Memory and storage · Network**"；比较图：Flagship（Octa-core, 12 GB RAM / 256 GB ROM, 6000 mAh, 4G/5G）↔ Entry/Basic（Quad-core, 2 GB RAM / 8 GB ROM, 2150 mAh, 3G/4G）。

✍️ **Jan Q1a(i) 四个 challenge（12 分 = 4 × 3，ELSA）**：每个 = slide 关键字 + 旗舰 vs 入门机的差距 + ELSA 要怎么做。
> 1. **Processing Power** – entry devices use quad-core processors while flagships use octa-core, so ELSA should run heavy AI diagnostics on the server, not on the phone.
> 2. **Memory and storage** – some SME owners use phones with only 2 GB RAM / 8 GB ROM, so ELSA must keep its app size and cached data small.
> 3. **Battery** – basic phones have as little as 2150 mAh, so ELSA should avoid continuous GPS and frequent background sync.
> 4. **Network** – rural SMEs may only have 3G/4G, so ELSA should work with slow or unstable connections, e.g. save forms locally and upload when the network returns.

📖 **Beyond Mobile App（p27–28）**："Internet of Things (IOT): Wearable, Smart Home Appliance, Smart Car Dashboard · Augmented Reality (AR) · Artificial Intelligent (AI) · Blockchain"

✍️ **Jan Q1a(ii) AI in ELSA（4 分）**：ELSA uses Artificial Intelligence to run the business diagnostic — it analyses the SME's data and gives strengths, weaknesses and recommendations, and matches the SME to suitable financing (ModalNiaga) and training.

---

## 6. Saving Data（Oct Q3b 10、Jan Q3b 9）⭐⭐

📖 **Slide 原句（4.1 p3–20）**

| Method | Slide 原句 |
|---|---|
| **Shared Preferences** | "Stores private primitive data in key-value pairs"；"Primitive data: boolean, float, int, long, and string"；"Data will persist across session, even if an app is killed"（Compose 用 DataStore） |
| **Data Files** | "A File APIs for reading and writing large amount of data"；"Suitable for image files or anything exchanged over a network"；p3："Data file; text, sound, images, and etc" |
| **SQLite** | p3："Repeating and structured data"；"SQLite can be accessed using the Room persistence library"：Database / Entity (table) / DAO (methods to access DB) |

**Internal vs External（p13，考 "in a table format"）**

| Characteristics | Internal | External |
|---|---|---|
| **Removeable** | No | Yes (Possible) |
| **Availability** | Always | Not always |
| **Accessibility** | Your app | World-readable |

补充原句：Internal – "No permission is required to perform read/write operations"。External – "Removable storage media (such as an SD card) or an internal (non-removable) storage. Files saved to the external storage are: world-readable; can be modified by the user; can become unavailable if the user unmount or remove the storage (SD Card)"。

✍️ **Jan Q3b（ELSA，3 × 3）**：
> (i) **Training progress → SQLite** – repeating and structured data (module, score, date for every module), accessed with Room.
> (ii) **Preferred language → Shared Preferences** – private primitive data in key-value pairs (`language = "ms"`) that will persist across sessions even if the app is killed.
> (iii) **Business registration documents → Data Files** – large files (PDF/images) read and written with the File APIs.

✍️ **Oct Q3b(ii)（MyGOV）**：Internal → 私人的 MyDigital ID session 资料（"Your app" only, no permission）；External → ePayment receipt PDF 存到 Downloads（"world-readable"，用户可以用其他 app 打开或用电脑复制）。

---

## 7. Architecture · Threads · Server Options（Oct Q3a 9、Jan Q3a 10）⭐⭐

📖 **Slide 原句（4.2 p4–5）**："Most mobile apps are the front-end interfaces of back-end services. The two components use Application Programming Interface (API) to communicate with each other"

**Mobile System Architecture（p5，照 slide 标签画）**
```
        Front-end                                   Back-end
  ┌──────────────────┐                      ┌───────────────────────┐
  │  Mobile Client   │                      │      API Server       │
  │                  │ ◄──── Network ────►  │          │            │
  │  Local Database  │       (API)          │   Database Server     │
  └──────────────────┘                      │          │            │
                                            │    Cloud Database     │
                                            └───────────────────────┘
```

✍️ **Oct Q3a(i) Front end / Back end（4 分）**：
> **Front-end** – the Mobile Client (the MyGOV app with its UI and Local Database) that users interact with; it sends requests through the API. **Back-end** – the services behind the app: API Server, Database Server and Cloud Database, which process the requests and return data. "Most mobile apps are the front-end interfaces of back-end services" and the two communicate using an API over the network (HTTP, REST with JSON).

📖 **Threads（p15–18）**
- "UI and network operations should be implemented on different thread"
- "Android throws a NetworkOnMainThreadException if you perform network operations on UI"
- "Two types of thread: Main – UI; Background"；图：**Main Thread = User interactions**；**Background Thread = Long-running operations: Decode Bitmap, Disk I/O, Network I/O**
- Background solutions（p18）："**DownloadManager** – Perform long-running HTTP downloads · **Foreground services** – User-initiated work that need to run immediately and must execute to completion · **AlarmManager** – To do the job at the time you specify · **WorkManager** – Runs deferrable background work when the work's conditions (like network availability and power) are satisfied"

✍️ **Jan Q3a（ELSA）**：(i) Main/UI thread = user interactions；Background thread = long-running operations (network I/O, disk I/O, decode bitmap)。(ii) 两个 asynchronous tasks：① 把资料送到 server 做 AI diagnostic（Network I/O，结果回来再更新 UI）；② 上传融资文件并同步申请状态（**WorkManager**："runs deferrable background work when … network availability … satisfied"）。

📖 **Server Options（p51–56）**："**DIY** – Build your own servers, API + DB；**Subscribe** – Back-End as a Service (BaaS), Database, AI, Quality control, Security；**Mix and bang** – DIY + Subscribe"
- DIY Advantages: "Complete control – Hardware, Software, Network, Services"；Disadvantages: "Heavy customisation, Complexity, Security vulnerabilities, Insufficient performance …"
- Subscribe Advantages: "Cost efficiency, Scalability, Speed, Integration, Audit and compliance, Business continuity planning"；Disadvantages: "Pay-for-use, Security"
- BaaS services: "Storage, Push Notifications, Usage analytics, Dashboard, Social integration, User administration, Custom code integration"（例：Firebase、Apple CloudKit）

⚠️ Slide 给 BaaS 的定义（"a delivery model … collaboration between … software development team and the operations team"）其实是 DevOps 的定义。若考 BaaS，用 p57 的**服务列表**来解释最安全："BaaS provides ready back-end services such as storage, push notifications, usage analytics and user administration."

---

## 8. Location（Oct Q3c 6 + Q4a 6、Jan Q3c 6）⭐⭐

📖 **GPS vs Wi-Fi or cell towers（5 p4，表格照抄）**："Two ways to obtain a user location: GPS and Wi-Fi or cell towers"

| Type | GPS | Wi-Fi or cell towers |
|---|---|---|
| **Data Accuracy** | High | Low |
| **Speed** | Slow | Fast |
| **Power Consumption** | High | Low |
| **Environment** | **Outdoor** | **Indoor and outdoor** |

（小心：Environment 这一行很多人写反。）

📖 **Foreground vs Background（p32–35，表格照抄）**

| Task | Foreground Location | Background Location |
|---|---|---|
| **Share Location** | Once / Predefined period | Constant |
| **Visibility of UI** | Visible | Not visible |
| **Show persistent notification** | Yes | No |

例子原句：Foreground – "Within a navigation app, a feature allows users to get turn-by-turn directions"；Background – "Within a family location sharing app, a feature allows users to continuously share location with family members"。Background 要 `ACCESS_BACKGROUND_LOCATION`（"For Android 10 (API 29) and above"）。

✍️ **Oct Q3c（MyGOV）**：(i) 导航去最近的 UTC → **Foreground**（like turn-by-turn directions; UI visible; for a predefined period）。(ii) 附近政府活动的提醒 → **Background**（constant location sharing even when the UI is not visible）。

📖 **Geocoding（p45–47）**
- ⚠️ Slide 定义："Geocoding = converting a geographic location to an address"（这句其实是 reverse geocoding；一般定义是 address → coordinates。考试可写："Geocoding converts between an address and geographic coordinates"，两种说法都涵盖。）
- "Geocoding APIs can compensate for ambiguous addresses — ones that are misspelled or inaccurate"
- "For example, street addresses can change; address coordinates won't"
- "Geocoding integrates and aligns address information with geographic codes to verify the address"
- Possible errors: "No location data provided · Invalid latitude or longitude used · No geocoder available · No address found"

✍️ **Oct Q4a（MyGOV，3 × 2）**：① 找最近的 UTC/JPJ（地址 → 座标 → 地图）；② 验证用户填的地址（"compensate for ambiguous addresses — ones that are misspelled or inaccurate"）；③ 依所在地显示州属的援助与活动（"integrates and aligns address information with geographic codes"）。

---

## 9. Distribution & Monetisation（Oct Q4c 10、Jan Q4b 9）⭐⭐

📖 **Slide 原句（7 p32–58）**
- **Distribution Channels**: "Marketplace · Email · Website or server"
- **Distribution Methods**: "1. Android Go 2. App Bundles 3. Google Play Instant 4. Support Chrome OS"
- **App Bundles**: "Allows developer to upload app's compiled code and resources to Google Play. APK generation and signing of app are deferred. **Dynamic Delivery** – Google Play uses your app bundle to generate an APK according to user's device configuration"
- Android Go："Target Oreo (API 26) · App size less than 40 MB · RAM usage below 50 MB (apps) · Start your app under 5 seconds"

✍️ **Oct Q4c(i) 一个 distribution channel（5 分）**：**Marketplace**（Google Play / App Store）— official store 让市民相信 MyGOV 是真的政府 app、商店会审核与扫描恶意程式、自动更新、容易搜到。

✍️ **Jan Q4b(i) App Bundles 的两个理由（4 分）**：
> 1. **Dynamic Delivery** – Google Play uses the app bundle to generate an APK according to each user's device configuration, so SMEs with low-end phones download a smaller ELSA.
> 2. **One upload** – SME Bank only uploads the compiled code and resources once; APK generation and signing are deferred to Google Play, making releases and updates simpler.

📖 **Monetize Your App（p51–58，8 种，全部用 slide 关键字）**

| Model | Slide 关键字 |
|---|---|
| **Premium Apps** | Paid · In-app Billing · Extensive Features · Narrow niche in the market |
| **Freemium Apps** | Free · Uses In-app Billing · Digital goods: Durable, Consumable |
| **Subscription** | Free trial · In-app billing · Subscription fee |
| **Ads** | Free · AdMob + Google Mobile Ads SDK · Shows income-generating ads |
| **E-Commerce** | Free · B2C · Technology, logistics and payment solutions · Sales commissions + Setup fees |
| **Rewarded Products** | Free · Share content on social media, perform simple tasks (e.g. scan QR code, answer survey) · Get free stuff, earn rewards |
| **Service** | Free · **An extension of physical/online services** |
| **Data Collection** | Free · Partnership/Affiliate · Provide customer service, promotion, run a giveaway |

✍️ **Oct Q4c(ii) MyGOV（5 分）/ Jan Q4b(ii) ELSA（5 分）→ 都选 Service model**：
> **Service model** – the app is Free and is an extension of physical/online services. *MyGOV:* free for all citizens; it extends government counter services (road tax, licence renewal, ePayment), and the value comes from reducing counter visits and processing costs, not from charging users. *ELSA:* free for SMEs; it extends SME Bank's financing and advisory services, and the bank earns from the financing and advisory programmes that SMEs take up through the app.

---

## 10. Design for Everyone · Notifications（Oct Q2a 10 + Q2b 3、Jan Q2a 6）⭐

📖 **Design for Everyone（2.3 p23–57）**
- "Android devices come in many shapes and sizes all around the world. Device configurations: **Languages · Screen · Versions of Android**"
- Techniques: "**Externalize resources · Maintain resources independently**"
- Languages: "Extract UI strings from your app code and keep them in an external file. The res/values/strings.xml holds your string values"；"At runtime, the Android system uses the appropriate set of string resources based on the locale currently set for the user's device"（`values-ms/`, `values-zh/` …）
- Screens: "Declare different layouts and bitmaps for different screens. Place these alternative resources in separate directories"；"The system loads the layout file from the appropriate layout directory based on screen size/orientation"；"Provide alternative bitmap resources for good graphical quality and performance"（drawable-ldpi/mdpi/hdpi/xhdpi）
- Versions: "minSdkVersion = the lowest API level with which your app is compatible · targetSdkVersion = the highest API level against which you've designed and tested your app"

✍️ **Oct Q2a（MyGOV 服务不同种族 + 各种设备，2 × 5）**：
> 1. **Supporting Different Languages** – extract UI strings from the app code and keep them in external files (`res/values/strings.xml`, `values-ms/`, `values-zh/`, `values-ta/`). At runtime, the Android system uses the appropriate set of string resources based on the locale of the user's device, so Malay, Chinese and Indian users see MyGOV in their own language.
> 2. **Supporting Different Screens** – declare different layouts and bitmaps for different screens and place them in separate directories (`layout-large/`, `drawable-hdpi/`); the system loads the right layout based on screen size/orientation, so MyGOV works on small phones and tablets.

📖 **Toast / Snackbar / Notification / Dialog（2.3 p16–20、2.2 p42–43）**

| Component | Slide 原句 |
|---|---|
| **Toast** | "Simple feedback about an operation in a small popup. It only fills the amount of space required for the message" |
| **Snackbar** | "Displays a brief message. Action could be added"；Priority **Low**；"It disappears automatically" |
| **Notification** | "A message displays outside your app's UI. Purposes: Reminder, Communication from other people, Other timely information from your app"；Priority **Medium**；"It remains until dismissed by the user" |
| **Dialog** | "Use dialog for: critical information, make decisions …"；Priority **High**；User action **Required**；"It blocks app usage until the user takes a dialog action" |

✍️ **Oct Q2b 通知有新 update（3 分）**：**Notification** – a message displays outside MyGOV's UI to give "other timely information from your app"; it remains until dismissed by the user, so users see it even when MyGOV is closed.

✍️ **Jan Q2a 提交融资申请后的两个方法（6 分）**：① **Snackbar / Toast** – simple feedback about the operation ("Application submitted") in a small popup that disappears automatically；② **Notification** – timely information from the app when the approval status changes, displayed outside ELSA's UI.（或 ② **Dialog** – critical information，显示申请编号，用户必须确认。）

---

## 11. 最后 40 分钟：默写这 20 条

1. Compose performance：**Optimizing Layout hierarchies / Re-using Layouts (composable functions instead of `<include>`) / Loading Views on demand (LazyColumn, only visible items are composed and drawn)**
2. Explicit = "To start a component in your own app … fully-qualified component class name"
3. Implicit = "Declare a general action to perform, which allows a component from another app to handle it"
4. Intent 5 parts：**Component Name, Action, Data, Category, Extra**
5. Camera：**existing camera app (Intent, no need permission)** vs **own camera (CameraX/Camera2, camera preview, request Camera permission)**
6. Manifest：**Camera Features `<uses-feature>`** — "Prevents your app from being installed to devices that do not include a camera"；**Camera Permission**
7. Sensors：**Motion / Environmental / Position**
8. Lifecycle：onCreate → onStart → onResume → **Activity Running** → onPause → onStop → onDestroy → **Activity shut down**；onRestart；3 条回来的路
9. onStop："Performs relatively CPU-intensive shutdown operations E.g. save information to a database"
10. Challenges：**Processing Power, Battery, Memory and storage, Network**
11. Beyond：**IoT, AR, AI, Blockchain**
12. Shared Preferences："private primitive data in key-value pairs … persist … even if an app is killed"
13. Internal vs External：**Removeable No/Yes · Availability Always/Not always · Accessibility Your app/World-readable**
14. SQLite："Repeating and structured data"（Room：Database, Entity, DAO）
15. "Most mobile apps are the front-end interfaces of back-end services … use API to communicate"
16. Main thread = user interactions；Background = long-running operations（Network I/O, Disk I/O, Decode Bitmap）
17. GPS vs Wi-Fi：**Accuracy High/Low · Speed Slow/Fast · Power High/Low · Outdoor / Indoor and outdoor**
18. Foreground vs Background：**Once·predefined/Constant · Visible/Not visible · Notification Yes/No**
19. App Bundles："APK generation and signing … deferred"；"**Dynamic Delivery** … generate an APK according to user's device configuration"
20. Monetisation 8 种：Premium, Freemium, Subscription, Ads, E-Commerce, Rewarded Products, **Service ("An extension of physical/online services")**, Data Collection

---

需要更详细的解释或例子时，再去看同一资料夹里的各章笔记（`AMIT3353_Ch1`–`Ch7`）；这份冲刺笔记的英文都以 slide 原句为准。
