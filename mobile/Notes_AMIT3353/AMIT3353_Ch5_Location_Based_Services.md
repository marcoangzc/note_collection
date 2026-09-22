# AMIT3353 — Chapter 5: Location-based Services

> Slide 用很多一格一格"逐步出现"的表格，但没讲清楚 accuracy / frequency / latency 三个参数怎么一起决定耗电；geocoding 的文字定义还和它自己的图相反（文字写 location → address，图是 address → coordinates）。这份笔记补上每个参数的意义、选 location model 的方法，并更正 geocoding 定义。

---

## 0. 一句话总览

**手机可以用 GPS（准但慢、耗电、只在户外）或 Wi-Fi/基地台（快、省电、不准）定位；透过 Google Play services 的 Fused Location Provider，开发者设定 accuracy、frequency、latency 来平衡精准度与耗电，并申请 foreground / background location permission。**

| Part | 内容 | Slide |
|---|---|---|
| 1 | Location strategies：GPS vs Wi-Fi/cell | p3–6 |
| 2 | Battery drain：Accuracy、Frequency、Latency | p7–17 |
| 3 | 新服务（紧急定位、地震警报） | p18–19 |
| 4 | Location best practices | p20–24 |
| 5 | Google Play services、permissions、foreground vs background | p25–39 |
| 6 | Location updates 流程、runtime permission、mock location | p40–44 |
| 7 | Geocoding | p45–47 |
| 8 | Review questions | p48–50 |

🎯 **这章回答的考题**
- **Oct 2025 Q3c** Classify MyGOV uses under foreground or background location：(i) directions to nearest UTC（3）(ii) alerts for nearby government events（3）
- **Oct 2025 Q4a** Relate the applications of geocoding to MyGOV users（6）
- **Jan 2026 Q3c** Differentiate GPS and Wi-Fi/Cell towers in accuracy, speed and power（6）

**Prerequisites**：Ch3 lifecycle（在 onStart/onResume 开始定位、在 onPause/onStop 停止）；Ch1 runtime permission（Android 6+）。

---

## 1. Location Strategies：GPS vs Wi-Fi / Cell Towers（p3–6）

手机随身携带 → **location awareness** 可提供**情境化 (contextual)** 的体验。Android 用 **Google Play services 的 Location APIs**。

两种取得位置的方法：
- **GPS**：接收卫星讯号计算位置。要看得见天空、要等卫星锁定。
- **Wi-Fi 或 cell towers（基地台）**：根据附近的 Wi-Fi 热点与基地台位置估算。

| Type | **GPS** | **Wi-Fi or cell towers** |
|---|---|---|
| **Data accuracy** | **High** | Low |
| **Speed** | **Slow**（要锁定卫星） | **Fast** |
| **Power consumption** | **High** | Low |
| **Environment** | **Outdoor** only | **Indoor and outdoor** |

**类比**：GPS 像用望远镜看星星定方位——准，但要到户外、要花时间；Wi-Fi/基地台像问路人"这附近是哪里"——马上有答案，室内也行，但只知道大概在哪一区。

### ✅ 满分答法 — Jan 2026 Q3c Differentiate GPS and Wi-Fi/Cell towers for ELSA（6 分，最好画表）

> | Criteria | GPS | Wi-Fi / Cell towers |
> |---|---|---|
> | **Accuracy** | High – uses satellite signals and can locate the device within a few metres, e.g. pinpointing the exact SME premises for a site visit. | Low – estimates the position from nearby Wi-Fi access points and cell towers, accurate only to a city block (≈100 m) or city level (≈10 km). |
> | **Speed** | Slow – needs time to acquire a satellite lock (especially on first fix), and works only outdoors with a clear view of the sky. | Fast – returns a location almost immediately from already-known network signals, and works indoors (e.g. inside an SME's shop or office). |
> | **Power consumption** | High – the GPS chip drains the battery quickly when used continuously. | Low – uses radios that are already on, so it saves battery; suitable for ELSA's general features such as suggesting nearby SME Bank branches or events. |

💬 **答题句 (EN)** — *GPS gives high accuracy but is slow, consumes more power and works only outdoors, whereas Wi-Fi/cell-tower positioning is fast, low-power and works indoors and outdoors, but with lower accuracy.*

---

## 2. Battery Drain：Accuracy, Frequency, Latency（p7–17）

定位耗电受三个面向影响：**Accuracy（多准）、Frequency（多常更新）、Latency（多快要交付）**。

### 2.1 Accuracy（p8–12）

Accuracy = 位置资料的精准度。**越准，越耗电**。

| Priority | Constant | Description | Precision | Hardware | Power |
|---|---|---|---|---|---|
| **High Accuracy** | `PRIORITY_HIGH_ACCURACY` | 最准；启用 GPS、Wi-Fi、cell | 最精准 | **GPS** | **High** |
| **Balanced Power** | `PRIORITY_BALANCED_POWER_ACCURACY` | 够准；可能用 GPS，通常用 Wi-Fi 与 cell | **City block（≈100 m）** | Wi-Fi / cell | Less |
| **Low Power** | `PRIORITY_LOW_POWER` | 主要靠 cell towers | **City level（≈10 km）** | Wi-Fi / cell | Less |
| **No Power** | `PRIORITY_NO_POWER` | 从**其他 app** 的定位结果拿资料（被动） | 看别的 app | None | Very minimum |

### 2.2 Frequency（p13–14）

| Method | 意思 |
|---|---|
| `setInterval()` | 计算位置的**间隔**（毫秒）；实际更新可能更快、更慢、甚至没有。**背景定位用尽量大的值；前景用小值** |
| `setFastestInterval()` | 最快速率 / **上限**（其他 app 也在定位时，你最快多久接收一次）；**一定要设**，避免 UI 闪烁或资料溢出 |
| `setPriority()` | 设定上面的 priority |

### 2.3 Latency（p15）

`setMaxWaitTime()`：**延迟交付**位置——系统把多个位置**批次**一起给你；设成 `setInterval()` 的**好几倍**。

```kotlin
val locationRequest = LocationRequest.create().apply {
    interval = 10000            // 每 10 秒算一次（10000 ms）
    fastestInterval = 5000      // 最快每 5 秒收一次
    maxWaitTime = 60 * 1000     // 最多等 1 分钟批次交付（= 6 个 interval）
    priority = LocationRequest.PRIORITY_HIGH_ACCURACY
}
```

### p17：选 location model（accuracy / frequency / latency）

| Use case | Accuracy | Frequency | Latency |
|---|---|---|---|
| a. **Mapping / navigation app** | **High**（GPS） | **高频**（例每 1–5 秒） | **低**（马上要，不批次） |
| b. **Weather app** | **Low power**（city level 就够） | **低频**（例每 30–60 分钟） | **高**（可批次、可被动） |
| c. **Retail proximity alert**（接近商店时通知） | **Balanced**（city block ≈100 m） | 中低频 | 中（可批次；或用 geofencing） |
| d. **Fitness tracking**（跑步路线） | **High** | **高频** | 可稍批次（记录用，不一定要即时画） |

---

## 3. 新的 Android Location Services（p18–19）

- **Android Emergency Location Service (ELS)**：拨打紧急电话时自动把精确位置送给救援单位。
- **Android Earthquake Alerts System**：用手机的加速度感测器侦测地震、利用位置向附近用户发警报。

---

## 4. Location Best Practices（p20–24）

1. **Remove location updates**：离开画面时停止更新。

| Lifecycle | Call |
|---|---|
| `onStart()` / `onResume()` | `requestLocationUpdates()` |
| `onPause()` / `onStop()` | `removeLocationUpdates()` |

2. **Set timeouts**：`setExpirationDuration()`（从呼叫起多少毫秒后失效）、`setExpirationTime()`（从开机起的某时间点失效）。
3. **Batch requests**：多个请求合并，适合**非前景**用途。例：每 10 分钟算一次，最多 1 小时交付一次：
```kotlin
request.setInterval(10 * 60 * 1000)     // 600,000 ms = 10 分钟
request.setMaxWaitTime(60 * 60 * 1000)  // 3,600,000 ms = 1 小时 → 一次收到约 6 个位置
```
4. **Passive location updates**：从**另一个前景 app** 拿位置。例：自己每 15 分钟算一次，但别的 app 在定位时最快每 2 分钟也收一次：
```kotlin
request.setInterval(15 * 60 * 1000)        // 15 分钟
request.setFastestInterval(2 * 60 * 1000)  // 2 分钟
```

---

## 5. Google Play Services 与 Permissions（p25–39）

**Last known location**：目前位置 ≈ 最后已知位置。设定：SDK Manager 装 Google Play services、project 加 library。

```
Device                                        Google Play services
┌──────────────────────────────┐             ┌─────────────────────┐
│  Google Play services library │───────────▶│  Location service    │
│    [ Google API Client ]      │───────────▶│  Map service         │
│             ▲                 │───────────▶│  Other services      │
│         Your app              │             └─────────────────────┘
└──────────────────────────────┘
```
*重点：app 不直接碰 GPS 硬件，而是透过 Google Play services 的 client 去问 location service（Fused Location Provider 会自动混合 GPS/Wi-Fi/cell）。*

```kotlin
private lateinit var fusedLocationClient: FusedLocationProviderClient
override fun onCreate(savedInstanceState: Bundle?) {
    fusedLocationClient = LocationServices.getFusedLocationProviderClient(this)   // onCreate 建 client
}
// onStart：
fusedLocationClient.lastLocation.addOnSuccessListener { location: Location? ->
    // 拿到最后已知位置；少数情况会是 null
}
```

### Permissions（p27–30）

| Permission | 意思 |
|---|---|
| **Coarse location** (`ACCESS_COARSE_LOCATION`) | **大概**位置，精准度约 **city block** |
| **Fine location** (`ACCESS_FINE_LOCATION`) | **精确**位置 |
| **Background** (`ACCESS_BACKGROUND_LOCATION`) | **Android 10（API 29）以上**，app 不在前景时也要定位 |

- Android 6+：在 manifest 声明 + **runtime** 向用户请求（p42 图：按 "Share location" → 对话框 "While using the app / Only this time / Deny"）。
- **Android 11（API 30）以上**：background location 要用户**到设定页**自己选 "Allow all the time"（p43）。

### ⭐ Foreground vs Background Location（p31–36）

| Task | **Foreground location** | **Background location** |
|---|---|---|
| Share location | **Once**，或一段**预设时间** | **Constant**（持续） |
| Visibility of UI | **Visible**（用户正在用 app） | **Not visible** |
| Show persistent notification | **Yes**（若用 foreground service） | **No** |
| 例子（slide） | 导航 app 的**逐步路线**；聊天 app **分享目前位置**给朋友 | 家人**持续分享位置**；IoT app：**离家时关电器、回家时开**（geofencing） |

Foreground 若用 service，要在 manifest 声明 `android:foregroundServiceType="location"`（Android 10+）。

### ✅ 满分答法 — Oct 2025 Q3c Classify MyGOV uses（3 + 3 分）

> **(i) Get direction to the nearest UTC – Foreground location.** The user opens MyGOV and actively requests directions, so the location is needed only while the app (or navigation screen) is visible and being used, for a limited period until they arrive. As in a navigation app's turn-by-turn feature, access "while using the app" is enough, and a notification is shown if navigation continues via a foreground service.
>
> **(ii) Alerts for nearby government events – Background location.** MyGOV must know when the user comes near an event venue even when the app is closed and not visible, so it needs constant location access in the background (e.g. geofences around event locations). This requires the ACCESS_BACKGROUND_LOCATION permission (Android 10+), which the user must enable as "Allow all the time" in settings (Android 11+).

💬 **答题句 (EN)** — *Foreground location is used while the app is visible and the user is interacting with a location feature, once or for a limited period; background location is used continuously when the app is not visible, requires the ACCESS_BACKGROUND_LOCATION permission and shows no UI.*

**p39 答案**：
1. a) Mapping app → **Fine**（要精确）；b) Weather app → **Coarse**（city 级就够）。（持续追踪才需要 Background。）
2. 在 onStart()/onResume() 开始定位：因为那时 activity 可见/在前景，用户才需要位置；也配对了在 onPause/onStop 关闭。
3. 在 **onPause() 或 onStop()** 关掉（`removeLocationUpdates()`）——不然看不见的画面还在耗电。

---

## 6. Location Updates 流程、Challenges、Mock Location（p40–44）

**决定用户位置的挑战（p40）**：**多种 location 来源**（GPS、Wi-Fi、cell 各有优缺点）、**用户会移动**（要持续更新）、**准确度会变化**（新的位置不一定比旧的准）。

**Location updates 流程（p41 flowchart）**：
```
START → Connect to location service → Make a location request
   ┌──────────────────────────────────────────────────────┐
   ▼                                                      │
 ◇ Location changed? ─Yes─▶ Update UI ─┐                  │
   │No                                 ▼                  │
   └──────────────────────────────▶ ◇ Pause? ─No──────────┘
                                       │Yes
                                       ▼
                               Stop location updates → STOP
```
*重点：只要没 pause 就一直循环检查；一 pause 就停止更新（对应 onPause/onStop）。*

**Mock location data（p44）**：测试用——**注入假的 GPS 资料**；在 emulator 的 Extended controls（或旧的 DDMS）设定 AVD 的经纬度，不用真的出门就能测试。

---

## 7. Geocoding（p45–47）

**Geocoding = 把地址（或地名）转成地理座标（经纬度）**；反过来（座标 → 地址）叫 **reverse geocoding**。

p45 图：用户输入 "fenway park" → 比对到地址 "Fenway Park, 4 Jersey St, Boston, MA 02215" → 转成座标 **42.346454, -71.097347** → 在地图上放 marker。

**Use case（p46）**：
- 可补救**模糊的地址**（拼错、不准确）。
- 街道地址可能改变，**座标不会**。
- 把地址资讯与地理座标对齐，用来**验证地址**。

**可能的错误（p47）**：No location data provided · Invalid latitude or longitude · No geocoder available · No address found。

### ✅ 满分答法 — Oct 2025 Q4a Relate the applications of geocoding to their relevance and benefits for MyGOV users（6 分 = 3 × 2）

> 1. **Finding the nearest government office (UTC, JPJ, LHDN)** – MyGOV geocodes the addresses of government offices into coordinates and compares them with the user's current location, so users can see the nearest UTC on a map and get directions instead of searching manually. *Benefit*: saves time and travel, especially for users unfamiliar with the area.
> 2. **Verifying and auto-filling user addresses** – when a user enters a home address for services such as healthcare support or cash-aid applications, geocoding converts it to coordinates and validates it, correcting misspelled or ambiguous addresses (street names may change, but coordinates do not); reverse geocoding can auto-fill the address from the user's GPS position. *Benefit*: fewer rejected applications and less typing.
> 3. **Location-based eligibility and alerts** – coordinates tell MyGOV which state/district the user is in, so it can show state-specific aid programmes, local government events and relevant notices. *Benefit*: users receive services and information relevant to where they live, including rural users.

---

## 8. Review Questions（p48–50）答案

**5.3-1 影响定位准确度的因素**：定位来源（GPS vs Wi-Fi/cell）、环境（室内、高楼、地下、天气挡住卫星）、Wi-Fi/基地台的密度、用户移动速度、设定的 priority 与更新频率、硬件品质。

**5.3-2 "为 location app 建立最佳效能模型很重要" 评论**：同意——accuracy、frequency、latency 直接决定耗电；太准太频繁会快速耗电、太松则资料没用。要按用途选（导航 high + 高频；天气 low power + 低频），并用 batching、passive updates、timeouts、离开画面就移除更新。

**Q4 "确保定位准确最好的方法是用实体设备测试" 评论**：部分同意——实体设备能反映真实的 GPS 讯号、室内外差异与耗电；但 **mock location** 能重复测试特定地点与路线、测试边界情况（没讯号、跨国），两者要一起用。

**Q5 用 location awareness 提升用户体验（6 分）**：例 Grab——自动侦测上车地点（免输入地址）、显示附近司机与实时到达时间、行程中分享位置给家人（安全）。每点写"功能 + 怎么用位置 + 对用户的好处"。

**Q6 MyChild.com：GPS 的三个限制（6 分）+ 解决技术（4 分）**：
> Limitations: (1) GPS works poorly indoors/underground and near tall buildings because it needs a clear view of satellites; (2) it is slow to get a first fix, so the child's position may be delayed; (3) it consumes a lot of battery, so a child's device may run out quickly.
> Technique: use the **Fused Location Provider** (hybrid positioning) that combines GPS with Wi-Fi and cell-tower positioning: Wi-Fi/cell gives fast, low-power, indoor locations, GPS is used only when high accuracy is needed outdoors, and battery is saved with balanced priority, batching and geofences (alert parents when the child leaves the school zone).

---

## ⚠️ Where the slides mislead

| Slide | Slide 写的 | 更准确的理解 |
|---|---|---|
| p45 | "Geocoding = converting a geographic location to an address" | 这是 **reverse geocoding**。**Geocoding = address → coordinates**（p45 自己的图就是 "fenway park" → 42.346454, -71.097347）。考试可以写："Geocoding converts an address into geographic coordinates; reverse geocoding converts coordinates into an address." |
| p16, p23 | `LocationRequest.create()` / `LocationRequest()` + setter | 新版 API 用 `LocationRequest.Builder(priority, interval)`；`create()` 已 deprecated。考试照 slide 写即可。 |
| p38 | "Connect to service in onStart()" | Fused Location Provider 不需要手动 connect（那是旧的 GoogleApiClient 做法）；直接呼叫 `lastLocation` / `requestLocationUpdates`。 |

---

## Term table

| English | 中文 | 一句话 |
|---|---|---|
| Location-based service (LBS) | 定位服务 | 依据位置提供情境化功能 |
| GPS | 全球定位系统 | 卫星定位，准、慢、耗电、户外 |
| Cell tower / Wi-Fi positioning | 基地台 / Wi-Fi 定位 | 快、省电、室内也行、较不准 |
| Fused Location Provider | 融合定位 | Google Play services 自动混合多种来源 |
| Accuracy / Frequency / Latency | 精准度 / 频率 / 延迟 | 三个耗电因素 |
| Priority (high / balanced / low / no power) | 优先级 | 精准度与耗电的取舍 |
| Interval / Fastest interval / Max wait time | 间隔 / 最快间隔 / 最长等待 | 频率与批次 |
| Coarse / Fine location | 粗略 / 精确位置 | city block / 精确 |
| Foreground / Background location | 前景 / 背景定位 | 可见时一次性 / 不可见时持续 |
| Geofencing | 地理围栏 | 进入/离开某区域时触发 |
| Geocoding / Reverse geocoding | 地理编码 / 反向 | 地址 → 座标 / 座标 → 地址 |
| Mock location | 模拟位置 | 测试用的假 GPS 资料 |

## Cheat sheet

| 项目 | 要点 |
|---|---|
| GPS vs Wi-Fi/cell | Accuracy High/Low · Speed Slow/Fast · Power High/Low · Outdoor/Indoor+outdoor |
| 耗电三因素 | Accuracy · Frequency · Latency |
| Priorities | High (GPS, 最准) · Balanced (≈100 m) · Low power (≈10 km, cell) · No power (其他 app) |
| Frequency | setInterval（背景大、前景小）· setFastestInterval（上限，必设）· setPriority |
| Latency | setMaxWaitTime = interval 的数倍（批次） |
| Best practices | remove updates (onPause/onStop) · timeouts · batch · passive |
| Permissions | Coarse (city block) · Fine (精确) · Background (API 29+; API 30+ 在设定页开) |
| Foreground vs Background | once/period, visible, notification · constant, not visible, no notification |
| Geocoding | address → coordinates；reverse = coordinates → address；errors: no data, invalid lat/long, no geocoder, no address |
| 测试 | mock location（emulator extended controls）+ 实机 |

---

## Practice（English, with answers）

### A. MCQ
1. Which location source works indoors and uses little power? (a) GPS (b) Wi-Fi/cell towers (c) Barometer (d) Magnetometer
2. `PRIORITY_BALANCED_POWER_ACCURACY` gives precision of about: (a) 1 m (b) 100 m (c) 10 km (d) none
3. Which method batches location deliveries? (a) setInterval (b) setFastestInterval (c) setMaxWaitTime (d) setPriority
4. Background location permission was introduced in Android: (a) 6 (b) 8 (c) 10 (d) 12
5. Converting "Menara KL" to 3.1528, 101.7038 is: (a) reverse geocoding (b) geocoding (c) geofencing (d) mock location

**Answers**：1 (b) · 2 (b) · 3 (c) · 4 (c) · 5 (b)

### B. Short answer
**B1.** Explain the three factors that affect location battery drain. (6 marks)
> Accuracy – higher precision uses GPS and drains more battery (high accuracy vs balanced vs low power). Frequency – how often locations are computed (setInterval/setFastestInterval); more frequent updates use more power. Latency – how quickly locations must be delivered (setMaxWaitTime); allowing delay lets the system batch updates and save power.

**B2.** Why should location updates be removed in onPause()/onStop()? (3 marks)
> When the activity is no longer in the foreground the user does not see the location UI, so continuing to request updates wastes battery; removing them in onPause/onStop and requesting again in onStart/onResume matches updates to the lifecycle.

**B3.** State FOUR possible geocoding errors. (4 marks)
> No location data provided; invalid latitude or longitude used; no geocoder available; no address found.

### C. Application
**C1.** A campus shuttle-tracking app shows the bus position live on a map. Choose accuracy, frequency and latency. (3 marks)
> High accuracy (GPS) for precise bus position; high frequency (e.g. every few seconds) so the marker moves smoothly; low latency (no batching) because riders need real-time positions.

**C2.** Classify as foreground or background: (a) a food app detects your location once to set the delivery address; (b) a parental app tracks a child all day. (4 marks)
> (a) Foreground – used once while the user is using the app, UI visible. (b) Background – continuous tracking while the app is not visible; needs ACCESS_BACKGROUND_LOCATION.

**C3.** A weather app requests high-accuracy updates every second. Critique and improve it. (3 marks)
> It wastes battery and GPS for data that only needs city-level accuracy and changes slowly. Use coarse location / PRIORITY_LOW_POWER, a long interval (e.g. 30–60 min), passive updates and batching.

### D. Thinking
**D1.** Why might a newer location fix be less trustworthy than an older one?
> Accuracy varies by source: a new fix from cell towers (≈km) can be much less precise than a GPS fix taken a minute ago, so apps should compare the accuracy radius and timestamp rather than always using the latest reading.

**D2.** Why does Android 11 require users to enable background location on a settings page?
> Continuous background tracking is highly privacy-sensitive; forcing a deliberate step in settings (instead of a quick dialog) ensures users knowingly allow "all the time" access and reduces apps tracking people without their awareness.

---

## Slide index
| Note section | Slides |
|---|---|
| 1 GPS vs Wi-Fi | p3–6 |
| 2 Accuracy/Frequency/Latency | p7–17 |
| 3 New services | p18–19 |
| 4 Best practices | p20–24 |
| 5 Play services & permissions | p25–39 |
| 6 Updates flow, mock | p40–44 |
| 7 Geocoding | p45–47 |
| 8 Review | p48–50 |

## Links to other chapters
- 开地图导航用 implicit intent（`geo:`）→ **Ch3**
- Lifecycle 开关定位 → **Ch3**
- 背景持续工作（foreground service、WorkManager）→ **Ch4.2**
- Barometer 帮 GPS 更快锁定高度、magnetometer 指南针 → **Ch6**
