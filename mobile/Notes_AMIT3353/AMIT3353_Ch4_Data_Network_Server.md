# AMIT3353 — Chapter 4: Saving Data (4.1) · Network, Background Operations & Server Options (4.2)

> 4.1 的 slide 很多是图（Room 结构、ViewModel 架构、SharedPreferences 对照），文字几乎没有解释"什么时候用哪一种储存"；4.2 讲 AsyncTask 却没说它已 deprecated，BaaS 的定义也写错（写成 DevOps 的定义）。这份笔记给出选择储存方式的判断规则，画出考试要的 architecture 图，并把每个图的意思讲清楚。

---

## 0. 一句话总览

**App 的资料可以存在手机里（SharedPreferences/DataStore、Files、SQLite/Room），也可以存在 back end（经由 API 与网络）；耗时的工作（磁碟、网络）不能放在 UI thread，要交给 background thread（coroutine、WorkManager…）；back end 可以自己架（DIY）或订阅（BaaS/cloud）。**

| Part | 内容 | Slide |
|---|---|---|
| 4.1-1 | 三种储存：SharedPreferences / Files / SQLite | 4.1 p3 |
| 4.1-2 | SharedPreferences 与 DataStore | 4.1 p4–11 |
| 4.1-3 | Files：internal vs external storage | 4.1 p12–21 |
| 4.1-4 | SQLite & Room | 4.1 p22–35 |
| 4.1-5 | ViewModel、Repository、LiveData/StateFlow、Coroutine | 4.1 p36–63 |
| 4.1-6 | SQL vs NoSQL（Firebase） | 4.1 p64–65 |
| 4.2-1 | Mobile-to-server、architecture、REST/SOAP、JSON | 4.2 p3–13 |
| 4.2-2 | Main vs background thread、background solutions、AsyncTask | 4.2 p14–29 |
| 4.2-3 | Network operations、Volley、MQTT | 4.2 p19–47 |
| 4.2-4 | Mobile server options：DIY / Subscribe / BaaS | 4.2 p48–68 |

🎯 **这章回答的考题**
- **Oct 2025 Q3a** (i) Define Front End and Back End（4）(ii) Illustrate mobile system architecture using a diagram（5）
- **Oct 2025 Q3b** (i) Differentiate internal vs external storage in a table（6）(ii) One local storage method for both, with MyGOV content example（4）
- **Jan 2026 Q3a** (i) Define UI thread and background thread（4）(ii) TWO asynchronous tasks in ELSA（6）
- **Jan 2026 Q3b** SharedPreferences / Data Files / SQLite for (i) training progress (ii) preferred language (iii) business registration documents（3 × 3）

**Prerequisites**：Ch3 lifecycle（资料要在 onStop 存；转屏会重建 activity）。

---

# 4.1 Saving Data

## 1. 三种储存方式（4.1 p3）

| | **Shared Preferences** 🔑 | **Data Files** 📄 | **SQLite** 🛢 |
|---|---|---|---|
| Access | Private（or Public） | Private or Public | Private |
| 存什么 | **Key-value pairs**（小、简单的设定） | **Data files**：text、sound、images 等 | **Repeating and structured data**（很多笔、同样栏位的资料） |

### 判断规则（考题 Jan 2026 Q3b 就是这个）

> - 只有**一个值 / 少数设定**（语言、dark mode、是否已登入、音量）→ **SharedPreferences / DataStore**
> - 一个**档案**（PDF、照片、录音、影片、下载的文件）→ **Data Files**
> - **很多笔结构相同的记录**，要查询、排序、更新（交易记录、课程进度、联络人）→ **SQLite (Room)**

### ✅ 满分答法 — Jan 2026 Q3b Identify the data saving method for ELSA data（3 × 3 分）

> **(i) Training progress – SQLite.** Progress is structured, repeating data: for every learning module the app stores a module ID, title, percentage completed, quiz score and last-accessed date. A table (Room entity) lets ELSA query, sort and update many records, e.g. "show all modules not yet completed".
>
> **(ii) Preferred language used – Shared Preferences (DataStore).** It is a single primitive key-value setting, e.g. `language = "ms"`, that must persist across sessions even if the app is killed, and is read at start-up to load the correct string resources. A database or file would be unnecessary overhead.
>
> **(iii) Business registration documents – Data Files.** These are uploaded documents such as SSM certificates as PDF or image files, which are large binary files rather than key-value pairs or table rows. They are saved as files (private app-specific storage for confidentiality), and only the file path/URI is stored in the database if needed.

## 2. Shared Preferences 与 DataStore（p4–11）

- 存**私有 primitive 资料**的 **key-value pairs**：boolean、float、int、long、string。
- **跨 session 保存，就算 app 被关掉也还在**。
- 用 `SharedPreferences` class。

| Method | `getSharedPreferences(name, mode)` | `getPreferences(mode)` |
|---|---|---|
| 档案数量 | **多个**（用名字区分） | **单一** |
| 存取层级 | **Context**（整个 app 的任何 activity 都能用） | **Activity**（只有那个 activity 用） |

```
getSharedPreferences (Context 层级)        getPreferences (Activity 层级)
[Shared Pref 1] ─┬─ Activity 1             [Pref 1] ── Activity 1
[Shared Pref 2] ─┴─ Activity 2             [Pref 2] ── Activity 2
（交叉线：任何 activity 可读任何档）         （一对一）
```

**p11 答案**：1. 差别如上表。2. SharedPreferences 可被 **a. 多个 activities** 存取 ✓；**b. 多个 apps** ✗（它是 private，属于 app 的 sandbox）。

**Jetpack Compose → DataStore（p7–10）**：Compose 推荐用 **Preferences DataStore** 取代 SharedPreferences（非同步、用 Flow、不会卡 UI thread）。
```kotlin
// gradle: implementation("androidx.datastore:datastore-preferences:1.0.0")
val Context.dataStore: DataStore<Preferences> by preferencesDataStore(name = "settings")
val exampleKey = stringPreferencesKey("example_key")

suspend fun saveData(value: String, context: Context) {
    context.dataStore.edit { preferences -> preferences[exampleKey] = value }   // .edit{} 非同步写
}
val readData: Flow<String> = context.dataStore.data
    .map { preferences -> preferences[exampleKey] ?: "Default Value" }         // .map{} 读成 Flow
```
- `val Context.dataStore` = **extension property**，任何地方都能 `context.dataStore`。
- `by preferencesDataStore(name = "settings")` = **delegated property initialization**。

## 3. Files：Internal vs External Storage（p12–21）

File API 适合**大量资料**、图片、从网络交换的档案。

### ⭐ Internal vs External（p13）——Oct 2025 Q3b(i) 要画表

| Characteristics | **Internal** | **External** |
|---|---|---|
| **Removable** | No | Yes（possible，例 SD card） |
| **Availability** | **Always** available | **Not always**（用户可能卸载/拔出 SD card） |
| **Accessibility** | **Only your app** | **World-readable**（其他 app 与用户可读，例接电脑） |

- **Internal**：目录以 app 的 package name 命名；读写**不需要 permission**；写之前用 `getFreeSpace()` / `getTotalSpace()` 检查空间避免 IO error。
```kotlin
val file = File(context.filesDir, filename)                     // internal
file.bufferedWriter().use { out -> out.write(fileContent) }       // 写
if (file.exists()) { val content = file.bufferedReader().readText() }  // 读
```
- **External**：可以是可移除的 SD card，也可以是**不可移除但能被电脑存取**的内建储存。档案 world-readable、用户可修改、可能变得不可用。旧版要 `READ_/WRITE_EXTERNAL_STORAGE` permission。先检查：
```kotlin
fun isExternalStorageReadable() = Environment.getExternalStorageState() in
    setOf(Environment.MEDIA_MOUNTED, Environment.MEDIA_MOUNTED_READ_ONLY)
```
- External 里的两种档案：

| | Public | Private（app-specific，`getExternalFilesDir()`） |
|---|---|---|
| 谁能存取 | 所有 apps | 该 app（*） |
| 卸载 app 后 | **保留** | **删除** |

```kotlin
val file = File(context.getExternalFilesDir(Environment.DIRECTORY_PICTURES), albumName)  // private album
```

**新的 Storage guide（p20）**：

| Type | Content | 其他 app 能存取？ | 卸载时删除？ |
|---|---|---|---|
| App-specific files | 只给你的 app 用 | No | Yes |
| Media | 可分享的图片、音频、影片 | Yes（with permission） | No |
| Documents & other files | 其他可分享内容（含下载档） | Yes | No |
| App preferences | key-value | No | Yes |
| Database | structured data | No | Yes |

**p21 答案**：1. 想让用户用电脑存取的档案 → **External**。2. 内建、不可移除、但可接电脑存取的记忆体 → 属于 **external storage**（"external" 指的是可被外部/用户存取，不是指"可拔出"）。

### ✅ 满分答法 — Oct 2025 Q3b(i) Differentiate THREE differences between internal and external storage（6 分，表格）

> | Criteria | Internal storage | External storage |
> |---|---|---|
> | **Removability** | Not removable; it is built into the device. | May be removable, e.g. an SD card (or a non-removable shared partition). |
> | **Availability** | Always available to the app, since it cannot be unmounted. | Not always available; it becomes unavailable if the user unmounts or removes the SD card. |
> | **Accessibility** | Private: only the app itself can read/write its files (other apps and the user cannot), and no permission is required. | World-readable: files can be accessed by other apps and by the user (e.g. via a computer), and may require storage permission. |

### ✅ 满分答法 — Oct 2025 Q3b(ii) Provide ONE local storage method for both internal and external storage with a MyGOV content example（4 分）

> - **Internal storage – save a private file with `File(context.filesDir, name)` (or Shared Preferences/DataStore).** Example: MyGOV stores the user's cached MyDigital ID session/profile data and downloaded tax-filing drafts in internal storage, because this personal data must remain private to the app and always be available.
> - **External storage – save to app-specific/public external storage with `getExternalFilesDir(Environment.DIRECTORY_DOCUMENTS)` or the public Downloads/Media folder.** Example: MyGOV saves an ePayment receipt or road-tax e-certificate as a PDF in the public Downloads folder so the user can open it with other apps, print it or copy it to a computer.

## 4. SQLite 与 Room（p22–35）

**SQLite**：轻量的**关系式数据库**，用在 embedded devices、IoT、网站、文件格式、app 内部数据库（p22 图）。

| Type | 意思 |
|---|---|
| NULL | 空值 |
| INTEGER | 有号整数（1–8 bytes，按大小） |
| REAL | 8-byte 浮点数 |
| TEXT | 文字 |
| BLOB | Binary large object：图片、音频、媒体 |

**Room persistence library**：在 SQLite 上面的一层，让你用 Kotlin 物件操作数据库。三个部分：

| 部分 | 是什么 |
|---|---|
| **Database** | 数据库的存取点 |
| **Entity** | 一个 **table** |
| **DAO (Data Access Object)** | 存取数据库的 **methods**（查询、新增、删除） |

```
                [ Room Database ]
                 │ Get DAO    │
      ┌──────────┘            │
      ▼                       │
[Data Access Object] ──Get Entities from DB / Persist changes back to DB──▶ [The rest of the app]
                                                     [Entities] ◀── Get/Set field values ──┘
```
*重点：app 从 Database 拿到 DAO，经 DAO 读写 Entities；app 本身不写 SQL 字串去乱改 table。*

四个步骤：
```kotlin
// Step 1: Entity（一个 table）
@Entity
data class User(
    @PrimaryKey val uid: Int,
    @ColumnInfo(name = "first_name") val firstName: String?,   // String? = nullable
    @ColumnInfo(name = "last_name") val lastName: String?
)
// 复合主键：@Entity(primaryKeys = ["firstName","lastName"])；外键：foreignKeys = [ForeignKey(...)]

// Step 2: DAO
@Dao
interface UserDao {
    @Query("SELECT * FROM user") fun getAll(): List<User>
    @Query("SELECT * FROM user WHERE uid IN (:userIds)") fun loadAllByIds(userIds: IntArray): List<User>
    @Insert fun insertAll(vararg users: User)          // vararg = 可变数量参数
    @Delete fun delete(user: User)
}

// Step 3: AppDatabase abstract class
@Database(entities = [User::class], version = 1)
abstract class AppDatabase : RoomDatabase() { abstract fun userDao(): UserDao }

// Step 4: instance
val db = Room.databaseBuilder(applicationContext, AppDatabase::class.java, "database-name").build()
```
⚠️ **不要在 UI thread 上用 Room**，要在 background thread 非同步执行（第 5 节 coroutine）。
**Database Inspector**（p33）：Android Studio 里检查、查询、修改 app 的 database。

**p34 答案**：Entity = 定义一个 table 的 class；DAO = 提供存取数据的 methods；Room = SQLite 上的抽象层，管理数据库、编译时检查 SQL、减少样板代码。

**p35 答案（房产 agent app：上传媒体、留言、记录地点）两个技术**：(1) **Files**（照片/影片存档案，路径存 DB）；(2) **SQLite/Room**（留言、房产记录与经纬度这些结构化资料）；（另可加 back end/cloud 同步）。

## 5. ViewModel、Repository、LiveData、Coroutine（p36–63）

### 5.1 ViewModel——在转屏后存活

**问题（p38–40）**：传统做法 UI data 放在 activity 里；**转屏 (rotation event)** → activity 被销毁 → UI data 一起被**摧毁** → 重建 activity 时要**重新建立 data**（再查一次数据库/网络）。

**解决（p41–43）**：把 UI data 放在 **ViewModel**，ViewModel **在 configuration change 后存活**；新的 activity instance 连回同一个 ViewModel，资料还在。

```
传统：[Activity + UI Data] ─rotate─▶ 摧毁 UI Data ─▶ [新 Activity] 重建 UI Data
ViewModel：[Activity] ─rotate─▶ [新 Activity]
               └──────▶ [ViewModel: UI Data] ◀──┘   （同一个 ViewModel，资料不丢）
```

### 5.2 推荐架构（p44, p47–48）

```
[ User Interface (Activity / Fragment) ]   ← 画 UI、接收用户互动
               │
          [ ViewModel ]                     ← 保存 UI data
               │
          [ Repository ]                    ← 存取资料的 API；决定用哪个来源
           │          │
[ Room Database ]   [ Network ]
 Entity ─ DAO ─ SQL
```

| Component | 负责 |
|---|---|
| **Activity** | Drawing UI、receiving user interactions |
| **ViewModel** | Hold UI data（survive configuration changes） |
| **Repository** | API for saving and loading app data；管理**多个数据来源**（例：先从本地 cache 拿，没有才抓网络） |

### 5.3 LiveData 与 StateFlow（p50–57）

**LiveData** = 可被**观察**的资料容器；资料一变，**observer** 就被通知；而且是 **lifecycle-aware**（activity 不在前景时不会去更新它，避免 crash/leak）。

```
1. UI 的 Observer attach 到 ViewModel 里的 LiveData
2. New data 到达 LiveData
3. Observer 更新 UI
```
```kotlin
class UserProfileViewModel : ViewModel() {
    private val _user = MutableLiveData<User>()     // 内部可改
    val user: LiveData<User> get() = _user           // 外部只读
}
// Activity.onCreate:
userViewModel.user.observe(this) { user -> userNameTextView.text = user?.name }
// 更新：_user.value = newUser   （slide 写 user.setValue(newUser)）
```
**StateFlow**：Kotlin coroutines 的 hot、observable data stream；**完全相容 Compose**，是**推荐**的 UI state 管理方式（`collectAsState()`）。

### 5.4 Coroutine（p58–62）

**Coroutine** = Kotlin 的**非同步程式**工具：
- 处理可能**阻塞 main thread、让 app 冻结**的长时间工作；
- 提供 **main-safety**：可以从 main thread 安全地呼叫网络或磁碟操作（工作实际在别的 thread）。

p59 图：Main thread 一直往下跑，旁边分出一个 coroutine 做工作，做完把结果交回 main thread。

两种启动方式：
| | `launch` | `async` |
|---|---|---|
| 回传结果？ | **不回传**（fire and forget） | 回传结果（`await()`，suspend function） |

三种 scope（p61）：

| Scope | Lifetime | 资源 |
|---|---|---|
| **GlobalScope** | 整个 Application | High（容易 leak，少用） |
| **LifecycleScope** | Activity | Medium |
| **ViewModelScope** | ViewModel | Low（推荐） |

```kotlin
fun main() {
    GlobalScope.launch {          // 在背景开一个 coroutine
        delay(1000L)              // 非阻塞等待 1 秒
        println("World!")
    }
    println("Hello,")             // main thread 不等，继续跑
    Thread.sleep(2000L)           // 阻塞 main 2 秒，让 JVM 活着
}
// Output:
// Hello,
// World!
```
*为什么 "Hello," 先出现？* `launch` 不会阻塞 main thread；coroutine 要 delay 1 秒才印 "World!"，而 main 马上印 "Hello,"，再 sleep 2 秒等 coroutine 完成。（⚠️ Kotlin 没在这台电脑执行过；这个输出是 Kotlin 官方文档的经典例子。）

**p63 答案**：1. Coroutine 的目的：在不阻塞 main thread 的情况下做长时间工作（网络、数据库），并让代码写起来像顺序执行。2. 数据库操作不该在 main thread：它可能很慢，会让 UI 冻结（卡顿），太久还会触发 **ANR（Application Not Responding）**；Room 预设直接禁止在 main thread 查询。

## 6. SQL vs NoSQL（p64–65）

| SQL | NoSQL |
|---|---|
| **Table-based**（关系式，table 之间用 key 连结） | **Document**（JSON 文件）、**Key-Value**（Name → "Alice"）、**Graph**（节点与连线）、**Wide-column stores**（按栏存） |
| 固定 schema | 弹性 schema |
| 例：SQLite、MySQL | 例：**Firebase**（Realtime DB / Firestore） |

---

# 4.2 Network Operation, Background Operation & Mobile Server Options

## 7. Mobile-to-Server Communication（p3–13）

**大部分 mobile app 是 back-end 服务的 front-end 介面**，两者透过 **API** 沟通：
```
[Front-end (mobile app)] ◀──▶ [API] ◀──▶ [Back-end (server)]
```

### ✅ 满分答法 — Oct 2025 Q3a(i) Define Front End and Back End（4 分）

> - **Front end** – the client side of the system that users see and interact with: the mobile app's user interface running on the device (e.g. MyGOV's screens, buttons, forms and local database). It collects user input, displays data and sends requests to the back end through an API.
> - **Back end** – the server side that users do not see: the API/application server, web server and databases (on-premise or cloud) that process requests, apply business logic, store data and return responses (e.g. MyGOV's servers that verify MyDigital ID, process road-tax renewals and record ePayments).

### ⭐ Mobile System Architecture（p5 图）——Oct 2025 Q3a(ii) 5 分

```
   Front-end                          Back-end
 ┌──────────────┐           ┌─────────────────────────────────────────────┐
 │ Mobile Client│◀─────────▶(  Network  )◀──▶[API Server]◀──▶[Database   ]◀──▶(Cloud   )
 │  (MyGOV app) │           │  (Internet,     (REST/HTTPS,   Server]       Database)
 └──────┬───────┘           │   HTTPS)         JSON)                        │
        ▲▼                  └─────────────────────────────────────────────┘
 ( Local Database )
  (Room/SQLite cache)
```
**解释（考试写在图下）**：
> The mobile client (front end) stores some data in a local database for offline use and communicates over the network (Internet, HTTPS) with the back end. The API server receives requests (e.g. REST calls with JSON), applies business logic and authentication, and talks to the database server, which stores data in the cloud database. Responses travel back through the API to the app, which updates the UI.
> *MyGOV example*: the app sends a road-tax renewal request → API server verifies the MyDigital ID and payment → database server records it → the status is returned and cached in the local database.

**Do I even need a back-end?（p6）**：不需要——如果 app 只用本地资料（计算机、离线游戏）；需要——如果要多设备同步、多人共享、登入、付款、推播。

### 通讯协定（p7–13）

**HTTP** 是最常用的协定；沟通方式：**SOAP、REST、XML-RPC**。

| | **REST** (Representational State Transfer) | **SOAP** (Simple Object Access Protocol) |
|---|---|---|
| Data formats | HTML、XML、**JSON** | **只有 XML**（Envelope → Header + Body） |
| 操作 | 沿用 HTTP：**GET、POST、PUT、DELETE** | 可跑在 HTTP、SMTP、TCP、UDP 上 |
| 特点 | 轻量、快、mobile 最常用 | 严格、较重 |

**JSON vs XML**：同样的登入资料，JSON（`{"username": "my_username", ...}`）比 XML（`<username>my_username</username>...`）短、好读、解析快 → **大部分 Google API 用 JSON REST**；Android 用 **JSONObject** 读 JSON。

```kotlin
// JSON: {"records":[{"id":"1","name":"Alice","contact":"012"}, ...]}
val jsonContact = JSONObject(strResponse)
val jsonArray = jsonContact.getJSONArray("records")
for (i in 0 until jsonArray.length()) {
    val obj = jsonArray.getJSONObject(i)
    val user = User(obj.getString("id"), obj.getString("name"), obj.getString("contact"))
    list.add(user)
}
```

## 8. Main Thread vs Background Thread（p14–29）

- UI 与网络操作要在**不同 thread**。
- 在 UI thread 做网络 → Android 丢出 **`NetworkOnMainThreadException`**。

```
   User interactions               Long-running operations
 ┌──────────────────┐        ┌───────────────────────────────────┐
 │   Main Thread    │◀──────▶│ Background Thread ◀── Decode Bitmap │
 │   (UI thread)    │        │                  ◀── Disk I/O      │
 └──────────────────┘        │                  ◀── Network I/O   │
                             └───────────────────────────────────┘
```
*重点：main thread 只负责画画面与回应触控；解码图片、读写磁碟、网络这些慢工作都丢到 background thread。*

### ✅ 满分答法 — Jan 2026 Q3a(i) Define UI Thread and Background Thread（4 分）

> - **UI thread (main thread)** – the single thread created when the app starts that handles all user interactions and draws/updates the user interface. It must stay free and responsive; long operations on it freeze the screen and can cause an ANR, and network calls on it throw a NetworkOnMainThreadException. Results of background work must be posted back to it to update the UI.
> - **Background thread (worker thread)** – a separate thread used to run long-running or blocking operations such as network I/O, disk/database I/O and decoding bitmaps, so that the UI thread is not blocked; in Kotlin it is typically used through coroutines (`Dispatchers.IO`), WorkManager or services.

### ✅ 满分答法 — Jan 2026 Q3a(ii) Provide TWO asynchronous tasks performed by ELSA（6 分）

> 1. **Running the AI business diagnostic / fetching insights from the server** – when the SME submits its data, ELSA sends it to the back-end AI service and waits for the analysis. This network call runs on a background thread (coroutine with `Dispatchers.IO` / Volley); the UI shows a progress indicator and, when the result arrives, it is published on the UI thread to display strengths, weaknesses and recommendations.
> 2. **Submitting the financing application to ModalNiaga/ScoreXcess and syncing its status** – uploading the auto-filled form and business registration documents and polling/syncing approval status are long network and file operations. They run in the background (e.g. WorkManager so they complete even if the user leaves the screen, retrying when the network returns); the result is posted to the UI as a status update or notification.
>
> (Other valid examples: downloading training videos/learning modules; saving training progress to the local Room database; loading report charts.)

### Background solutions（p17–18 决策图）

```
Network I/O? ─Yes─▶ DownloadManager
   │No
Now or Never? ─Yes─▶ Foreground Service
   │No
Depends on system conditions? ─Yes─▶ WorkManager
   │No
Run at a specific time? ─Yes─▶ AlarmManager
   │No
   └──▶ WorkManager
```

| Class | 用途 |
|---|---|
| **DownloadManager** | 长时间的 HTTP 下载 |
| **Foreground service** | 用户发起、要**马上**跑、**一定要做完**的工作（显示持续通知，例导航、音乐） |
| **AlarmManager** | 在**指定时间**执行 |
| **WorkManager** | 可延后的背景工作，等**条件满足**（有网络、充电中）才跑 |

- Kotlin 传输资料：**WorkManager + coroutine**。定期、有效率但不需即时的传输：**Sync Adapters**。
- 网络 permission（manifest）：`android.permission.INTERNET`、`android.permission.ACCESS_NETWORK_STATE`。
- 最佳做法：**尽量少传敏感资料**；所有流量走 **SSL（HTTPS）**。
- **检查网络状态（Compose，p22–24）**：`ConnectivityManager.NetworkCallback`（onAvailable / onLost / onLosing / onUnavailable）+ `StateFlow<NetworkStatus>`（Available, Unavailable, Losing, Lost），UI 用 `collectAsState()` 显示 "Online / Offline / Losing connection"。

### AsyncTask（p25–28）

**AsyncTask** 在背景做事，**把结果发回 UI thread**：

```
UI Thread:     (1) onPreExecute()  ─────────────────────▶ (3) onPostExecute()
                        │                                        ▲
Async Task:             └──▶ (2) doInBackground() ────────────────┘
```
1. `onPreExecute()`（UI thread）：准备，例显示 loading。
2. `doInBackground(Params…)`（background）：真正的网络工作——**至少要 override 这个**。
3. `onPostExecute(Result)`（UI thread）：拿结果更新 UI。
必须是 **private subclass**；适合**短**工作。p27：Main Activity 的 Network Fragment 用 DownloadTask 开始 → doInBackground → DownloadCallback 回报。

⚠️ **AsyncTask 已在 API 30 deprecated**（容易 leak activity、转屏出错）。现代做法：**Kotlin coroutines**（`viewModelScope.launch(Dispatchers.IO) { ... }`）。考试问 AsyncTask 就照 slide 答三个步骤，可补一句现代替代。

**p29 答案**：1. App（front end）用 API 向 back end 要资料/送资料，back end 处理并存 DB，API 把结果回传。2. 网络延迟不可预测，放在 UI thread 会冻结画面/ANR，而且 Android 直接丢 NetworkOnMainThreadException。3. AsyncTask：onPreExecute 显示进度 → doInBackground 做网络 → onPostExecute 更新 UI。

## 9. Volley 与 MQTT（p30–47）

**Volley** = HTTP library，把资料填到 UI（例搜寻结果）；**不适合大下载或串流**；支援 string、image、JSON。
好处：**自动排程**网络请求、**多个并发连线**、**请求优先级**、**可取消请求**。

| Request | 回传 |
|---|---|
| StringRequest | raw string |
| JsonObjectRequest | JSONObject |
| JsonArrayRequest | JSONArray |

```kotlin
val queue = Volley.newRequestQueue(this)
val stringRequest = StringRequest(Request.Method.GET, url,
    { response -> textView.text = "Response is: ${response.substring(0, 500)}" },
    { textView.text = "That didn't work!" })
queue.add(stringRequest)
// 取消：stringRequest.tag = TAG；onStop(){ requestQueue?.cancelAll(TAG) }
```
- RequestQueue 需要 **network + cache**（`BasicNetwork(HurlStack())` + `DiskBasedCache(cacheDir, 1MB)`）。
- 常用网络 → 用 **singleton** 只建一个 RequestQueue（用 `applicationContext` 避免 leak activity）。

**MQTT (Message Queuing Telemetry Transport)**：跑在 **TCP/IP** 上、**machine-to-machine** 协定、**publish/subscribe** 模式。
```
[temperature sensor] ─publish "21°C" to topic "temperature"─▶ [MQTT Broker (HiveMQ)]
                                                              │ publish to subscribers
                                          [laptop] ◀──────────┤
                                          [mobile device] ◀───┘  (两者都 subscribe 了 "temperature")
```
**p47 答案：为什么 MQTT 适合 IoT app**：非常轻量（小 header、省频宽与电）、publish/subscribe 让一个装置的资料即时送到多个订阅者、能在不稳定网络下运作（有 QoS 与保持连线机制）。

## 10. Mobile Server Options（p48–68）

Mobile app = UI / front-end；back end 提供 databases、scripting（API）与架构——**把 DB 接到 app**。三大核心：**Application server、Web server、Database**。
流行 back-end 技术：Ruby on Rails、Express/Koa/Sails（Node.js）、Django（Python）、PHP MVC frameworks、Google Firebase。

| Option | 是什么 | 优点 | 缺点 |
|---|---|---|---|
| **DIY** | 自己建 server（API + DB） | **完全控制** hardware、software、network、services | 大量客制、复杂、安全漏洞、效能不足、可靠性差、功能差、technical debt；要花很多时间监控维护 |
| **Subscribe** | 订阅服务：BaaS、database、AI、QC、security | Cost efficiency、**Scalability**、Speed、Integration、Audit & compliance、Business continuity | Pay-for-use（持续付费）、Security（资料在第三方） |
| **Mix and bang** | DIY + Subscribe | 两者兼得 | 两边都要管 |

订阅的常见方式：
- **Cloud**：按用量付费、经网络提供的**按需运算资源**。
- **Container**：把 app **与执行环境抽离**的逻辑包装机制（例 Docker）。
- **Virtualisation**：在与实体硬件抽离的一层上跑**虚拟电脑**。
- **BaaS**（见下方 ⚠️ 修正定义）。

**BaaS 提供的服务（p57）**：Storage、Push notifications、Usage analytics、Dashboard、Social integration、User administration、Custom code integration。
例：**Firebase**（Develop：Realtime Database、Authentication、Cloud Messaging、Storage、Hosting、Test Lab、Crash Reporting；Grow：Notifications、Remote Config、App Indexing、Dynamic Links、Invites、AdWords；Earn：AdMob；中间 Analytics）、Azure、Apple CloudKit（Database、Messaging）、AWS、Alibaba Cloud、Huawei Cloud。

**p67 答案（Subscription vs DIY）**：DIY 完全控制但贵、复杂、要自己维护安全；Subscription 快、可扩展、成本按用量，但持续付费、资料安全依赖供应商。
**p68 答案（H&H 汽车零件：慢的搜寻、领域管理；要 push notification 与 chatbot，最安全又省钱）**：用 **BaaS / cloud subscription（例 Firebase + 云端 AI）**：Firebase Cloud Messaging 做推播、云端 AI 服务做 chatbot、云端数据库 + 索引加快搜寻、location 资料做 territory 分析；按用量付费、有内建安全认证与合规，比自己架 server 便宜又安全。

---

## ⚠️ Where the slides mislead

| Slide | Slide 写的 | 更准确的理解 |
|---|---|---|
| 4.2 p54 | BaaS = "a delivery model for a set of tools that facilitates collaboration between an organization's software development team and the operations team" | 这是 **DevOps** 的定义。BaaS = 云端提供现成的 back-end 服务（authentication、database、storage、push notifications、analytics），开发者透过 **API/SDK** 使用，不用自己架 server。 |
| 4.2 p25–28 | 用 AsyncTask 做网络 | AsyncTask 已在 **API 30 deprecated**；现代用 coroutines / WorkManager。 |
| 4.2 p9 | SOAP "works with application layer protocol e.g. HTTP, SMTP, TCP, or UDP" | HTTP、SMTP 是 application layer；**TCP、UDP 是 transport layer**。SOAP 可以跑在这些协定上。 |
| 4.1 p3 | SharedPreferences "Private or Public" | `MODE_WORLD_READABLE` 早已 deprecated（Android 7 起会 throw SecurityException），实际上只能 private。 |
| 4.1 p13, p17 | External = world-readable、要 READ/WRITE permission | Android 10+ 有 **scoped storage**：app-specific external 目录不需要 permission，其他 app 也不能随便读。考试照 slide 的表格答。 |
| 4.1 p56 | `user.setValue(newUser)` | `user` 是唯读的 `LiveData`；应该对 `_user`（MutableLiveData）呼叫 `setValue`（或 `_user.value = newUser`）。 |

---

## Term table

| English | 中文 | 一句话 |
|---|---|---|
| Shared Preferences / DataStore | 偏好设定 | 存 key-value 的小设定 |
| Internal / External storage | 内部 / 外部储存 | 私有且永远可用 / 可被存取、可能不可用 |
| Scoped storage | 分区储存 | Android 10+ 限制 app 存取外部储存 |
| SQLite / Room | 轻量数据库 / 持久化库 | 结构化资料；Entity、DAO、Database |
| Entity / DAO | 实体 / 资料存取物件 | table / 存取 methods |
| ViewModel | 视图模型 | 保存 UI data，转屏后存活 |
| Repository | 资料仓库 | 管理多个资料来源 |
| LiveData / StateFlow | 可观察资料 | 资料变 → 通知 UI |
| Coroutine | 协程 | 非同步，不阻塞 main thread |
| Front end / Back end | 前端 / 后端 | 用户看到的 / server 端 |
| API | 应用程式介面 | 前后端沟通的约定 |
| REST / SOAP | — | JSON+HTTP 动词 / 只用 XML |
| JSON | — | 轻量资料格式 |
| UI (main) thread / Background thread | 主线程 / 背景线程 | 画 UI / 做慢工作 |
| ANR | 应用无响应 | UI thread 被卡太久 |
| WorkManager / AlarmManager / DownloadManager / Foreground service | — | 背景工作的四种方案 |
| Volley | — | HTTP library |
| MQTT | — | IoT 的 publish/subscribe 协定 |
| BaaS | 后端即服务 | 现成的云端 back-end 服务 |

## Cheat sheet

| 项目 | 要点 |
|---|---|
| 选储存 | 小设定 → SharedPrefs/DataStore · 档案 → Files · 很多结构化记录 → SQLite/Room |
| getSharedPreferences vs getPreferences | 多档 / Context · 单档 / Activity |
| Internal vs External | Removable: No/Yes · Availability: Always/Not always · Access: your app/world-readable |
| External 种类 | Public（卸载后保留）· Private app-specific（卸载删除） |
| Room | Entity (table) · DAO (methods) · Database；不在 UI thread |
| SQLite types | NULL, INTEGER, REAL, TEXT, BLOB |
| Architecture | UI → ViewModel → Repository → Room / Network |
| LiveData | observable, lifecycle-aware；StateFlow 给 Compose |
| Coroutine | launch（无结果）/ async（await）；Global / Lifecycle / ViewModel scope |
| Architecture 图 | Mobile client + local DB ↔ Network ↔ API server ↔ DB server ↔ Cloud DB |
| REST vs SOAP | JSON/XML/HTML + GET/POST/PUT/DELETE · XML only, envelope |
| Threads | Main = UI；Background = network, disk, bitmap；NetworkOnMainThreadException |
| Background | Network I/O→DownloadManager · Now→Foreground Service · Conditions→WorkManager · Specific time→AlarmManager |
| AsyncTask | onPreExecute → doInBackground → onPostExecute（deprecated API 30） |
| Volley | StringRequest, JsonObjectRequest, JsonArrayRequest；queue + cache；singleton |
| MQTT | TCP/IP, M2M, publish/subscribe via broker |
| Server | DIY（控制 vs 维护负担）· Subscribe（成本、扩展 vs 付费、安全）· Mix |

---

## Practice（English, with answers）

### A. MCQ
1. Which is best for storing a user's dark-mode setting? (a) SQLite (b) Data file (c) Shared Preferences (d) External public file
2. Which Room component defines the methods that access the database? (a) Entity (b) DAO (c) RoomDatabase (d) Repository
3. Performing a network call on the main thread throws: (a) IOException (b) NetworkOnMainThreadException (c) SecurityException (d) NullPointerException
4. Deferrable work that should run only when the device is charging and on Wi-Fi uses: (a) AlarmManager (b) DownloadManager (c) WorkManager (d) Foreground service
5. Which data format does SOAP support? (a) JSON (b) XML (c) HTML (d) CSV

**Answers**：1 (c) · 2 (b) · 3 (b) · 4 (c) · 5 (b)

### B. Short answer
**B1.** Explain why a ViewModel is needed. (4 marks)
> On a configuration change such as rotation, the activity is destroyed and recreated, so data held in the activity is lost and must be reloaded. A ViewModel holds UI data outside the activity and survives configuration changes; the new activity instance reconnects to the same ViewModel, avoiding reloading from the database or network.

**B2.** Differentiate `launch` and `async` in coroutines. (2 marks)
> `launch` starts a coroutine that does not return a result ("fire and forget"); `async` starts a coroutine that returns a Deferred result obtained with the suspend function `await()`.

**B3.** State TWO advantages and TWO disadvantages of a DIY server. (4 marks)
> Advantages: complete control over hardware, software, network and services; full customisation. Disadvantages: complexity and heavy maintenance/monitoring; security vulnerabilities and reliability/performance problems if not managed well (technical debt).

### C. Application
**C1.** A clinic app stores: (a) the user's preferred font size, (b) scanned medical reports (PDF), (c) all appointment records. Choose a storage method for each. (3 × 2 marks)
> (a) Shared Preferences/DataStore – a single key-value setting. (b) Data files (internal/app-specific for privacy) – large binary documents. (c) SQLite/Room – many structured, repeating records that need querying.

**C2.** Draw and explain the recommended app architecture for an app that shows cached news offline and refreshes from the internet. (5 marks)
> UI (Activity/Compose) → ViewModel (holds news list as StateFlow/LiveData) → Repository → Room database (cached articles) and Network (REST API). The repository returns cached articles immediately, fetches new ones in a coroutine on a background thread, saves them to Room, and the observable data updates the UI automatically.

**C3.** A delivery app must upload its daily GPS log even if the app is closed, but only on Wi-Fi. Which background solution? Justify. (3 marks)
> WorkManager – the work is deferrable and depends on system conditions (unmetered network); WorkManager guarantees execution even after the app is closed or the device restarts, and schedules it when the constraints are met.

### D. Thinking
**D1.** Why is JSON preferred over XML for mobile apps?
> JSON is smaller (less markup), so it uses less bandwidth and battery on mobile networks, and it is faster and easier to parse (maps directly to objects), which is why most Google APIs use JSON REST.

**D2.** Why does the Repository pattern help an app work offline?
> It hides where data comes from: the ViewModel asks the repository, which can serve cached data from the local database when the network is unavailable and sync with the server when it returns, without changing UI code.

---

## Slide index
| Note section | Slides |
|---|---|
| 1 Three storage types | 4.1 p3 |
| 2 SharedPreferences/DataStore | 4.1 p4–11 |
| 3 Files | 4.1 p12–21 |
| 4 SQLite/Room | 4.1 p22–35 |
| 5 ViewModel, LiveData, Coroutine | 4.1 p36–63 |
| 6 SQL vs NoSQL | 4.1 p64–65 |
| 7 Mobile-to-server | 4.2 p3–13 |
| 8 Threads & background | 4.2 p14–29 |
| 9 Volley & MQTT | 4.2 p30–47 |
| 10 Server options | 4.2 p48–68 |

## Links to other chapters
- 什么时候存资料（onPause/onStop）、转屏 → **Ch3**
- Push notification 的 UI 端 → **Ch2.3**
- 存相机拍的照片、下载的媒体 → **Ch6**
- Location 的背景更新与 permissions → **Ch5**
