# AMIT3353 — 考前总结（知识点 + 关键字 + 关键字解释）

> 这份是聊天复习的总结。每节有 3 样东西：
> - **关键字**：考试要写出来的英文（slide 原句）
> - **意思**：关键字用中文怎么理解
> - **比喻 / 口诀**：帮你记住
>
> 更完整的 slide 原句和满分答案，请看 `AMIT3353_5H_Sprint_Lecture_Wording.md`。

---

## 0. 考试答题方法

**每一点 = 名字 + 理由（关键字）+ 套进 case**

- 分数 ÷ 3 ≈ 要写几点（6 分 → 2 点，9 分 → 3 点）
- 想不出理由 → **把表格里那一行抄出来当理由**
- 没读过的题 → 用常识写 + 套进 case，**不要留白**
- 题目说 "in a table format" 就画表；"using a diagram" 就画图

**Scenario 怎么读**：先扫 2–3 分钟 → 看题目 → 回去 scenario 划线（用户是谁、问题是什么、功能有哪些）→ 用 scenario 的字写 case 句子。

| Scenario 写了 | 提示你答 |
|---|---|
| 不同种族 / 语言 | Languages（§10） |
| 各种手机、平板 | Screen（§10）、App Bundles（§9） |
| 乡下、网络不稳 | Network challenge（§5）、WorkManager（§7） |
| 便宜手机 | Processing Power、Memory（§5） |
| 拍 IC、收据 | Use an existing camera app（§3） |
| 找最近的分行 | GPS、Geocoding（§8） |
| 免费的政府 / 银行服务 | Service model（§9） |

---

## 1. Intent（两份卷合计 22 分）

| 关键字 | 意思 |
|---|---|
| **Explicit intent** | 指名道姓，开**自己 app** 里的画面 |
| "start a component **in your own app**" | 目标在自己 app 里 |
| "**fully-qualified component class name**" | 写清楚要开哪个画面（例如 `SettingsActivity`） |
| **Implicit intent** | 只说要做什么，让**别的 app** 来做 |
| "**declares a general action to perform**" | 只写动作，不写目标 |
| "allows **a component from another app** to handle it" | 别的 app 来处理 |
| **ACTION_VIEW** | 看东西：开地图、开网页 |
| **ACTION_SEND** | 送东西：发 email、分享 |
| **Intent filter** | 别的 app 登记"我能做什么"的地方："To advertise which implicit intents your app can receive" |

**Intent 的 5 个部分**（还没练过，背名字就好）：

| 部分 | 意思 |
|---|---|
| **Component Name** | 要开的画面名字（Explicit 才有） |
| **Action** | 要做的动作（ACTION_VIEW / ACTION_SEND） |
| **Data** | 要处理的资料地址（**URI**），例如 `geo:` 地图坐标 |
| **Category** | 额外说明哪种 component 来处理（Optional） |
| **Extra** | 附带的资料，**key-value pairs**，例如 email 内容 |

**3 步流程**（Explicit 和 Implicit 只差第 2 步）：
1. Activity A creates an Intent.
2. Android System searches **all components of an app**（Explicit）/ **all apps for an intent filter**（Implicit）that matches the intent.
3. The system starts the matching Activity.

**万用句**：
> **[功能]: Explicit intent.** It starts a component in [app]'s own app, so the app supplies the fully-qualified component class name of [画面].
>
> **[功能]: Implicit intent.** [app] declares a general action to perform ([ACTION]), which allows a component from another app ([别的 app]) to handle it.

---

## 2. Compose Layout Performance（24 分）

**口诀：只改有变的、积木重复用、只画看得到的**

| 关键字（3 个标题） | 意思 | 要写的关键句 |
|---|---|---|
| **Optimizing Layout hierarchies** | 画面不用一层包一层，只更新有变的部分 | "flat tree of Composable"、"**only recompose the parts that change**" |
| **Re-using Layouts** | 做一块积木，到处重复用 | "break UI into small **reusable Composable**" |
| **Loading Views on demand** | 列表只画看得到的那几行 | "**LazyColumn**"、"**Only visible items are composed and drawn**" |

| 名词 | 意思 |
|---|---|
| **Composable** | Compose 里的一个 UI 零件（一个 function） |
| **Recompose** | 资料改变时，重新画那一块 |
| **LazyColumn** | 会"偷懒"的列表，滑到才画 |

---

## 3. Camera · Manifest · Sensors（25 分）

**相机 2 种方法：借别人的相机 / 自己做相机**

| 关键字 | 意思 | Permission |
|---|---|---|
| **Use an existing camera app** | 用 **Intent** 打开手机相机 app，拍完拿照片回来；"Simple implementation" | **不要**（"No need permission"） |
| **Build your own camera function** | 自己写相机（**CameraX / Camera2**），画面在自己 app 里；"Complex implementation" | **要** |

判断：只是拍照 / 上传 → 借；自己做 / 在 app 里显示镜头 / 扫描 → 自己做

**Manifest 2 行**（口诀：**"有没有"找 feature，"准不准"找 permission**）

| 关键字 | 意思 |
|---|---|
| **Camera Feature** `<uses-feature android:name="android.hardware.camera" />` | 手机**有没有**相机；没有就**不能安装** |
| **Camera Permission** `<uses-permission android:name="android.permission.CAMERA" />` | 用户**准不准** app 用相机 |

**Sensors 3 类**

| 类别 | 问什么 | Sensor 例子 | 生活例子 |
|---|---|---|---|
| **Motion** | 手机在动吗？ | **Accelerometer**（"rate of change in velocity"）、Gyroscope | 摇一摇、手机转横 |
| **Environmental** | 周围环境怎样？ | **Light sensor**（"senses the amount of ambient light"）、Thermometer、Barometer | 自动调亮度 |
| **Position** | 手机朝哪个方向？ | **Magnetometer**（"Magnetic North"） | 指南针 |

⚠️ 写全名 **Accelerometer**，不要缩写。

---

## 4. Activity Lifecycle（9 分，要画图）

**口诀：生（Create）→ 看（Start）→ 玩（Resume）→ 停（Pause）→ 藏（Stop）→ 死（Destroy）**

| Callback | 关键字 | 手机情况 |
|---|---|---|
| **onCreate** | creates the Activity，只做一次 | 打开 app |
| **onStart** | **visible** | 看得到 |
| **onResume** | **foreground**，可以操作 | 可以按 |
| **onPause** | **User is leaving**，还看得到一部分 | 跳出小窗口 |
| **onStop** | **no longer visible** | 按 Home |
| **onDestroy** | **Releases all resources** | 按返回键关掉 |
| **onRestart** | 从 onStop 回来 | 再打开 app |

**简化图**：
```
Activity Launched
      ↓
  onCreate()  ◄──────────────── (C) App process killed
      ↓
  onStart()   ◄──── onRestart() ◄──┐
      ↓                            │
  onResume()  ◄──┐                 │
      ↓          │ (A)             │ (B)
Activity Running │                 │
      ↓          │                 │
  onPause()  ────┘                 │
      ↓                            │
  onStop()  ───────────────────────┘
      ↓
  onDestroy()
      ↓
Activity shut down
```

**3 条回来的路**：
- (A) onPause → onResume：小窗口关掉
- (B) onStop → onRestart → onStart：按 Home 再回来
- (C) App process killed → onCreate：内存不够被系统杀掉，重新开始

---

## 5. Mobile OS · 4 Challenges（26 分）

| 关键字 | 意思 |
|---|---|
| **Android**：**Linux kernel**、**Open Source** | 很多品牌在用 → 用户最多 |
| **iOS**：**Unix kernel** | 只有 iPhone → 稳定、安全 |

**4 Challenges（口诀：脑、仓、电、网）**

| 关键字 | 便宜机的问题 | app 要怎么做 |
|---|---|---|
| **Processing Power** | CPU 慢（quad-core） | 重的工作交给 **server** |
| **Memory and storage** | 2 GB RAM / 8 GB ROM | app 和缓存要小 |
| **Battery** | 电池小（2150 mAh） | 少用 GPS、少在后台同步 |
| **Network** | 乡下只有 3G/4G | **save data locally and upload when the network returns** |

**Beyond Mobile App**：**IoT**（Wearable、Smart Home、Smart Car）· **AR** · **AI** · **Blockchain**

---

## 6. Saving Data（19 分）

| 关键字 | 比喻 | 存什么 | slide 关键句 |
|---|---|---|---|
| **Shared Preferences** | 便利贴 📝 | 小设定（语言、dark mode） | "private primitive data in **key-value pairs**"、"persist … even if an app is killed" |
| **Data Files** | 文件夹 📁 | 大文件（图片、PDF） | "**large amount of data**"、"image files" |
| **SQLite** | Excel 表格 📊 | 很多笔、格式一样的资料 | "**Repeating and structured data**"，用 **Room** |

| 名词 | 意思 |
|---|---|
| **key-value pair** | 一个名字配一个值，例如 `language = "ms"` |
| **primitive data** | 简单资料：boolean、int、string 等 |
| **Room** | 让 app 比较容易用 SQLite 的工具 |

**Internal vs External**（比喻：私人抽屉 vs 公用柜子 / SD card）

| Characteristics | Internal | External |
|---|---|---|
| **Removeable** | No | Yes (Possible) |
| **Availability** | Always | Not always |
| **Accessibility** | Your app | **World-readable** |

**World-readable** = 大家（别的 app、电脑）都能读 → 收据 PDF 要打印就存 External

---

## 7. Architecture · Threads · Server（19 分）

**比喻：餐厅**

| 关键字 | 比喻 | 包括 |
|---|---|---|
| **Front-end** | 前台（客人看得到） | **Mobile Client**、**Local Database** |
| **Back-end** | 厨房（客人看不到） | **API Server**、**Database Server**、**Cloud Database** |
| **API** | 服务员（传话） | 前台和厨房之间沟通 |

关键句："Most mobile apps are the **front-end interfaces of back-end services**. The two components use **API** to communicate with each other"

**Threads**（比喻：收银员 vs 后面搬货的员工）

| 关键字 | 意思 |
|---|---|
| **Main (UI) Thread** | 负责 **User interactions**（按钮、画面） |
| **Background Thread** | 负责 **Long-running operations**：Network I/O、Disk I/O、Decode Bitmap |
| **NetworkOnMainThreadException** | 在 Main Thread 上网络 → Android 报错（因为画面会卡住） |
| **WorkManager** | 等**有网络、有电**时才做（"network availability"） |
| **DownloadManager** | 下载大文件 |
| **AlarmManager** | 指定时间做 |
| **Foreground services** | 马上做，一定要做完 |

**Server 3 种选择**

| 关键字 | 比喻 | 优点 | 缺点 |
|---|---|---|---|
| **DIY** | 自己盖厨房 | **Complete control** | 复杂、安全风险 |
| **Subscribe (BaaS)** | 租别人的厨房（Firebase） | **Cost efficiency, Scalability, Speed** | **Pay-for-use**、安全 |
| **Mix and bang** | 两个都用 | DIY + Subscribe | |

敏感资料、要完全控制 → **DIY**

---

## 8. Location（18 分）

**GPS vs Wi-Fi**（GPS = 卫星，要看得到天空）

| Type | GPS | Wi-Fi or cell towers |
|---|---|---|
| **Data Accuracy** | High | Low |
| **Speed** | Slow | Fast |
| **Power Consumption** | High | Low |
| **Environment** | **Outdoor** | **Indoor and outdoor** |

口诀：GPS **准、慢、耗电、只能户外**；Wi-Fi 全部相反。
室内为什么用 Wi-Fi → "GPS needs signals from **satellites**, which cannot reach **indoors**"

**Foreground vs Background**

| Task | Foreground（你在看，像 Waze 导航） | Background（关着也追踪，像家人定位） |
|---|---|---|
| **Share Location** | Once / Predefined period | Constant |
| **Visibility of UI** | Visible | Not visible |
| **Show persistent notification** | Yes | No |

**Geocoding** = 地址 ↔ 坐标互相转换；"**compensate for ambiguous addresses** — ones that are **misspelled** or inaccurate"（地址打错也找得到）

---

## 9. Distribution · Monetisation（19 分）

| 关键字 | 意思 |
|---|---|
| **Service model** | "**Free**, an **extension of physical/online services**"（把柜台服务搬到手机，**MyGOV 和 ELSA 都选这个**） |
| **Freemium** | 免费 + 买道具（Digital goods） |
| **Premium** | 付费下载 |
| **Subscription** | 月费 |
| **Ads** | 广告（AdMob） |
| **E-Commerce** | 网购，赚佣金 |
| **Rewarded Products** | 做任务拿奖励 |
| **Data Collection** | 合作伙伴、推广 |

| 关键字 | 意思 |
|---|---|
| **Distribution Channels** | **Marketplace**（Google Play / App Store）· Email · Website |
| 选 Marketplace 的理由 | 用户**信任**官方商店、会**扫描恶意程式**、**自动更新** |
| **App Bundles** → **Dynamic Delivery** | Google Play 按**每支手机的配置**生成 APK → 便宜手机下载的 app 比较小 |

---

## 10. Design for Everyone · Notifications（19 分）

**3 种差异**：**Languages · Screen · Versions of Android**
**核心做法**："**Externalize resources**" = 东西放外面的文件，手机自动选

| 关键字 | 怎么做 | 比喻 |
|---|---|---|
| **Languages** | 文字放 `strings.xml`，每种语言一个文件夹（`values-ms/`、`values-zh/`、`values-ta/`）；系统按 **device locale** 自动选 | 同一份菜单印几种语言 |
| **Screen** | 不同屏幕放不同 layout 文件夹（`layout-large/`、`drawable-hdpi/`），系统自动载入 | 衣服 S / M / L |
| **minSdkVersion** | 最低支持的版本 | 电影分级：几岁以上 |
| **targetSdkVersion** | 测试过的最高版本 | |

**locale** = 手机设定的语言 / 地区

**4 种提醒方式（看多重要来选）**

| 关键字 | 重要度 | 会自己消失吗 | 例子 |
|---|---|---|---|
| **Toast** | 最低 | 会 | "已保存" |
| **Snackbar** | Low | 会，可以加按钮 | "已删除 [复原]" |
| **Notification** | Medium | 不会，**在 app 外面**，app 关着也看得到 | 申请批准了 |
| **Dialog** | High | 不会，**用户一定要按**（"critical information"） | "确定付 RM50？" |

记法：Toast、Snackbar **闪一下就走**；Notification **在 app 外面**；Dialog **挡住你**

---

## 11. 考前 10 分钟默写

1. Explicit = **own app** + class name；Implicit = **declares a general action** + **component from another app**
2. Intent 5 部分：**Component Name, Action, Data, Category, Extra**
3. Compose：**Optimizing Layout hierarchies / Re-using Layouts / Loading Views on demand**
4. 相机：借（Intent，不要 permission）/ 自己做（CameraX，要 permission）
5. Manifest：有没有 = **feature**；准不准 = **permission**
6. Sensors：**Motion / Environmental / Position**
7. Lifecycle：**生看玩停藏死** + 3 条回来的路
8. Challenges：**Processing Power, Memory and storage, Battery, Network**
9. Storage：便利贴 / 文件夹 / 表格；Internal vs External 3 行
10. Front-end / Back-end / API；Main vs Background thread；**WorkManager**；DIY = Complete control
11. GPS：准、慢、耗电、Outdoor；Geocoding = 地址打错也找得到
12. **Service model**："Free, an extension of physical/online services"；**Dynamic Delivery**
13. Languages → `values-ms/` + device locale；Toast / Snackbar / Notification / Dialog
