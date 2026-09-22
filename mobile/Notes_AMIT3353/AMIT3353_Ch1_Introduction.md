# AMIT3353 — Chapter 1: Introduction to Mobile Application Development

> Slide 大部分是图标和关键词（OS logo、4 个 challenge 图标），没有解释"为什么"。这份笔记补上：每个 OS 的特点怎么用来回答"为什么适合"，4 个 challenge 怎么写满 12 分，以及 Android 的 sandbox 安全模型和 4 大 components 的完整解释。

---

## 0. 一句话总览

**一个 mobile app 可以用 3 种方式开发，跑在几种 mobile OS 上，受到手机硬件的 4 种限制；Android app 由 4 种 components 组成，彼此用 Intent 沟通，全部登记在 Manifest 里。**

| 部分 | 内容 | Slide |
|---|---|---|
| 1 | Types of mobile apps（Native / Hybrid / Web） | p5–7 |
| 2 | Languages & tools | p9–12 |
| 3 | Mobile OS | p14–21 |
| 4 | Key mobile challenges | p23–25 |
| 5 | Beyond mobile app（IoT, AR, AI, Blockchain） | p27–31 |
| 6 | Android fundamentals：sandbox、least privilege、permissions | p33–42 |
| 7 | 4 app components (ABCS) + Intent + Manifest + Resources | p43–60 |

🎯 **这章回答的考题**
- Oct 2025 Q1a(i) Examine TWO mobile OS that support MyGOV（6）；(ii) Justify why they help it run effectively（4）
- Jan 2026 Q1a(i) Interpret FOUR key challenges in developing ELSA（12）；(ii) How AI is implemented in ELSA（4）

**Prerequisites**：无。只要知道 "app 跑在手机上、手机有 CPU / RAM / 电池 / 网络" 就够。

---

## 1. Types of Mobile Apps（p5–7）

想象你要做 MyGOV app，有三条路：

- **Native**：为某个 OS 单独写（Android 用 Kotlin/Java，iOS 用 Swift）。像请两位裁缝各做一套合身衣服——最合身，但要付两份钱。
- **Mobile Web**：其实就是网站（HTML/CSS/JS），用浏览器打开，不用安装。
- **Hybrid**：Web 技术写一份，外面包一层 native 外壳，放上 app store（Web + Native）。

| | Native | Hybrid | Mobile Web |
|---|---|---|---|
| Cost | **High** | Low | Low |
| Performance | **Fast** | Depends on network speed | Depends on network speed |
| Distribution | App stores | App stores | **None**（直接用网址） |
| Device features（相机、GPS、sensor） | **Wide** | Limited | Very limited |
| Code maintenance | **Multiple codebase** | Single codebase | Single codebase |

**为什么 native 最快？** 代码直接编译成 OS 能跑的机器码，直接呼叫硬件 API；hybrid/web 要经过浏览器引擎或 bridge。

**Slide p7 问题**（答案）：
- a. Bernama 新闻社 → **Mobile web / Hybrid**：以内容为主、要快速更新、不太需要硬件功能。
- b. Super Mario（游戏）→ **Native**：需要最高 performance 与图形运算。
- c. Setapak Central 商场 → **Hybrid**：预算有限、需要上 app store、要一点 GPS/通知功能即可。

💬 **答题句 (EN)** — *A native app is built for one specific OS, giving the fastest performance and full access to device features, but at higher cost and with multiple codebases; hybrid and mobile web apps share a single codebase at lower cost but with limited device access.*

---

## 2. Programming Languages & Tools（p9–12）

| 类型 | Languages | Tools |
|---|---|---|
| Native | **Kotlin**, Java, C/C++（Android）; **Swift**（iOS） | Android Studio, Xcode, Tizen Studio, Visual Studio, Huawei Quick App |
| Web | HTML5, CSS3, JavaScript | 任何 web 工具 |
| Cross-platform / Hybrid | Web + Native | React Native, **Flutter**, Xamarin, PhoneGap, Apache Weex, NativeScript, Jasonette, Ionic |

**Kotlin Multiplatform Mobile (KMM)**（p12 图）：**Shared code**（business logic and core）写一次；**View（UI）**仍各自 native；下面还有 iOS-specific APIs 和 Android-specific APIs。
→ 意思是：逻辑共享、界面 native——介于 native 与 hybrid 之间。

---

## 3. Mobile Operating Systems（p14–21）

p14 的图：2022–2023 全球市占率，**Android ≈ 70%**，**iOS ≈ 28%**，其他（Samsung、KaiOS、Windows…）接近 0。

| OS | Kernel | Open source? | 用在哪 |
|---|---|---|---|
| **Android** (Google) | **Linux kernel** | **Open source** | 手机、平板、手表、电视；+ Chrome OS（Linux-based, open source）可跑 Android app |
| **iOS** (Apple) | **Unix kernel**（Darwin/XNU） | Closed | iPhone、iPad、Apple Watch；+ macOS（Apple silicon Mac 可跑 iPhone app） |
| Tizen (Samsung) | Linux kernel | Open source | 手机、手表、电视 |
| KaiOS | Linux kernel | Open source | **Feature phones**（按键手机，能跑 WhatsApp） |
| HarmonyOS (Huawei) | **Microkernel** | Open source | 手机、手表、电视 |
| Fuchsia (Google) | **Microkernel** | Open source, **module architecture** | ARM-based，Nest Hub |

### ✅ 满分答法 — Oct 2025 Q1a(i) Examine TWO types of mobile OS that support the MyGOV app（6 分）

> 1. **Android** – Android is an open-source mobile OS developed by Google and built on the Linux kernel. It holds the largest market share worldwide (about 70%) and runs on phones from many manufacturers (Samsung, Xiaomi, Huawei, etc.) across all price ranges, from entry-level to flagship. MyGOV is distributed for Android through Google Play, allowing the majority of Malaysians, including low-income and rural users with budget phones, to install it.
> 2. **iOS** – iOS is Apple's closed-source mobile OS built on a Unix-based kernel, running only on iPhone and iPad. It holds the second-largest share (about 28%). Because Apple controls both hardware and software, MyGOV for iOS (distributed through the Apple App Store) runs on a small, consistent set of devices, which makes it stable and secure for handling MyDigital ID logins and ePayments.

### ✅ 满分答法 — Oct 2025 Q1a(ii) Justify why these two OS help MyGOV run effectively（4 分）

> - **Coverage**: Android and iOS together cover almost 98% of smartphones in Malaysia, so a single government app can reach nearly every citizen and reduce physical visits to offices.
> - **Device features and security**: both OS provide native access to the features MyGOV needs—secure biometric login for MyDigital ID, push notifications for application status, the camera for document upload and GPS for finding the nearest UTC—plus the security of app sandboxing and runtime permissions, which protects personal data and ePayment transactions.
> - (extra) Both have official app stores with review processes and automatic updates, so MyGOV can push security fixes quickly.

---

## 4. Key Mobile Challenges（p23–25）

Slide 只有 4 个图标：**Processing power, Memory and storage, Battery, Network**。p24 用手机比较说明差距：

| | Flagship | … | Basic / Entry |
|---|---|---|---|
| Processing power | Octa-core (SD 888), Hexa-core (A12) | → | Quad-core |
| Memory & storage | 12 GB RAM / 256 GB ROM | → | 2 GB RAM / 8 GB ROM |
| Battery | 6000 mAh | → | 2150 mAh |
| Network | 4G/5G | → | 3G/4G |

**为什么是"挑战"？** 你的 app 要在**最差**的那台手机上也能跑。用户不会因为"我手机便宜"原谅你的 app 卡、耗电、闪退。

**怎么应对（p25 问题：为新兴市场做 app）**：
- Processing → 把重运算放到 server（AI 诊断在 cloud 做），app 只显示结果；避免复杂动画。
- Memory/storage → app 小（App Bundles、Android Go < 40 MB）、图片压缩与按需加载（LazyColumn）、少用 cache。
- Battery → 减少 GPS 与背景工作、批次处理网络请求（WorkManager）、dark theme。
- Network → offline mode（本地 Room DB）、压缩数据（JSON）、重试与缓存、低流量模式。

### ✅ 满分答法 — Jan 2026 Q1a(i) Interpret FOUR key challenges in developing the ELSA app（12 分 = 4 × 3）

> 1. **Processing power** – Many SME owners use entry-level phones with quad-core processors, while ELSA performs AI-driven business diagnostics. If heavy AI analysis ran on the phone, low-end devices would lag or freeze. ELSA should run the AI models on the cloud back end and send only the results to the app, keeping the UI responsive on all devices.
> 2. **Memory and storage** – Low-end phones may have only 2 GB RAM and 8 GB storage. ELSA contains training modules, knowledge resources and financing documents; storing everything locally would fill the phone and cause crashes. It should keep the app small (e.g. using App Bundles), stream or download training content on demand, and load lists lazily.
> 3. **Battery** – Budget phones have smaller batteries (≈2000 mAh vs 6000 mAh). Continuous background syncing of real-time insights and financing status would drain the battery. ELSA should batch background work (e.g. WorkManager when charging / on Wi-Fi) and rely on push notifications instead of constant polling.
> 4. **Network** – SMEs in rural areas may only have slow 3G/4G or unstable connections, yet ELSA depends on ModalNiaga and ScoreXcess online services. The app should cache data locally, allow forms to be filled offline and submitted later, compress data (JSON), and show clear error/retry messages when the connection drops.

💬 **答题句 (EN)** — *The four key mobile challenges are limited processing power, limited memory and storage, limited battery life and unreliable network connectivity, which vary greatly between flagship and entry-level devices.*

---

## 5. Beyond Mobile App（p27–31）

Mobile 不只是手机 app，还延伸到：
- **IoT 相关**：Wearable（手表）、Smart home appliance、Smart car dashboard、Internet of Things。
- **AR (Augmented Reality)**：p30 图——平板镜头对着马达，画面上叠加每个部件的温度（thermal monitoring）。
- **AI (Artificial Intelligence)**：p29 图——**Google Lens** 对着一只狗，辨认出品种 "Labradoodle"。
- **Blockchain**（p31）。

### ✅ 满分答法 — Jan 2026 Q1a(ii) Discuss how AI technology is implemented in the ELSA app（4 分）

> - **AI-driven business diagnostics**: ELSA uses AI/machine-learning models (running on the server/cloud) to analyse an SME's performance data, identify strengths, weaknesses and business gaps, and generate improvement recommendations that are shown instantly in the app as real-time insights.
> - **Automation and matching**: the diagnostic results are used to auto-fill financing application forms and, through ScoreXcess, to speed up credit scoring; the AI also matches each SME to suitable advisory, training or financing programmes, reducing manual work and approval waiting time.

---

## 6. Android Application Fundamentals（p33–42）

### 6.1 APK 的内容（p34）

Android app 用 Java / Kotlin / C++ 写，打包成 **Android package (APK)**，里面有 3 样东西：**Compiled code + Data + Resources**。

**p42 Q1：为什么 APK 要有这三部分？** Code 是逻辑；Resources（layout、string、image）与 code **分开**，才能按语言/屏幕自动换资源；Data 是 app 自带的资料。三者打包在一起，安装一次就完整。

### 6.2 Security sandbox（p35–36）

每个 Android app 都住在自己的**安全沙盒 (security sandbox)** 里：
- 每个 app 对 OS 来说是**不同的 user**；
- 系统给每个 app 一个**独一无二的 Linux user ID (UID)**；
- 系统为 app 的所有文件设定 permission（只有该 UID 能读）；
- 每个 app 跑在**自己的 Linux process**；
- 每个 process 有**自己的 virtual machine (VM)**。

```
                 App 1 (UID 100)        App 2 (UID 101)
Phone Memory  |  [Virtual Machine]   |  [Virtual Machine]   |
Phone CPU     |  [   Process     ]   |  [   Process     ]   |
Phone Storage |  [    Data       ]   |  [    Data       ]   |
                 ↑ 两个直栏互相隔离：不同 UID、不同 process、不同 VM
```
*图的重点：两个 app 从内存、CPU 到储存都是分开的，一个 app 崩溃或被攻击不会直接影响另一个。*

**类比**：公寓大楼——每户有自己的门牌（UID）、自己的钥匙（file permission），住户不能随便进别人家。

### 6.3 Principle of Least Privilege（p37）

每个 app 默认**只能使用完成工作所需的 components，不多给**；没有被授予 permission 的系统部分，app 就不能访问。

### 6.4 两个例外：分享与系统服务

- **分享数据（p38–39）**：两个 app 可以**共享同一个 Linux user ID**（例：两个都是 UID 100），就能读对方的文件；条件是两个 app **用同一个 certificate 签名**。好处：跑在同一个 process、共用 VM，节省系统资源。
  ⚠️ 这个做法（`sharedUserId`）从 Android 10 起已被标为 deprecated，现代做法是 Content Provider。考试照 slide 答即可。
- **使用系统服务（p40–41）**：app 向**用户**请求 permission 才能读 contacts、SMS、storage、camera、Bluetooth、location 等。
  - **Android 5 以下**：安装时 (install time) 一次过全部授予；
  - **Android 6 以上**：**运行时 (runtime)** 用到时才问。

**p42 Q2 答案（两种保护方式）**：(1) **Sandbox**：每个 app 有独立 UID、process、VM，文件只有自己能读；(2) **Permissions**：访问敏感数据/硬件必须经用户同意（Android 6+ 是 runtime permission），配合 least privilege。

💬 **答题句 (EN)** — *Each Android app runs in its own security sandbox: it has a unique Linux user ID, its own process and its own virtual machine, and by the principle of least privilege it can only access the components and data it has been granted permission to use.*

---

## 7. App Components — ABCS（p43–55）

四种 components，口诀 **ABCS**：**A**ctivities、**B**roadcast Receivers、**C**ontent Providers、**S**ervices。

| Component | 图标 | 有没有 UI | 做什么 | 例子 |
|---|---|---|---|---|
| **Activity** | 👁 眼睛 | **有**（一个 activity = 一个 screen） | 画 UI 的窗口；app 可有多个；其中一个是 **main activity**（launcher） | 登录画面 |
| **Broadcast Receiver** | 📡 天线 | **没有**（但可发 status bar notification） | 接收系统或 app 广播的事件；**要先 register** | 电量低、开机完成、新 email |
| **Content Provider** | 🗄 数据库 | 没有 | 管理一个**中央数据仓库**的存取；数据可在 file system、SQLite、web/cloud；其他 app **有 permission** 可查询甚至修改 | User dictionary、Contacts |
| **Service** | 🤲 托手 | **没有**，在 background 跑 | 长时间运行的工作；可由其他 component 启动 | 背景播音乐、从网络抓资料 |

### Broadcast Receiver 流程（p46–47 图）

```
例 1：WhatsApp 与开机                      例 2：Gmail 新邮件
 (1) WhatsApp ──Register──▶ Android OS     (1) Gmail ──Register──▶ Android OS
 (2) Phone reboot ─────────▶ Android OS    (2) New email ────────▶ Android OS
 (3) OS ──Broadcast phone reboot──▶ WhatsApp (3) OS ──Broadcast new email──▶ 通知栏
 (4) WhatsApp ──Request new message──▶ Server (4) 用户点通知 ──Open email──▶ Gmail
```
*重点：app 必须**先 register**，OS 才会把事件广播给它。*

### Content Provider（p49 图）

```
      [Application] [Application] [Application]
                     │  (query / modify with permission)
              [ Content Provider ]
               │                │
        [Data Storage]    [Data Storage]
```
*重点：别的 app 不直接碰数据，而是透过 Content Provider 这个"窗口"存取。*

### 怎么启动别的 app 的 component？（p52–54）

每个 app 在自己的 sandbox 里，文件互相隔离——那 WhatsApp 怎么打开 Gallery 选照片？
答案：app **不直接呼叫**，而是**请 Android OS 代为启动**。Android app **没有单一入口**（没有 `main()`），任何 component 都能被启动。

```
[WhatsApp: Chat, Profile] ──(Profile 要换头像)──▶ [Android OS] ──▶ [Gallery: Pick a photo]
[Browser:  View Web, Download] ──(下载完要看图)──▶ [Android OS] ──▶ [Gallery: View a photo]
```

### Intent（p55）

**Intent = 一个"请求启动某 component"的讯息**。用来启动 **Activities、Services、Broadcast Receivers**（Content Provider **不是**用 Intent 启动，而是用 ContentResolver 查询）。详细在 Ch3。

```
            ┌─ Intent ─▶ [Activity]
[Activity] ─┼─ Intent ─▶ [Service]
            └─ Intent ─▶ [Broadcast Receiver]
```

**p51 问题答案**：a. Login screen → **Activity**；b. Sync data to server → **Service**；c. Manage shared app data on local storage → **Content Provider**；d. Schedule an alarm to post a notification → **Broadcast Receiver**（接收 alarm 事件后发通知）。

💬 **答题句 (EN)** — *An Android app is made of four components: Activities (UI screens), Services (background work without UI), Broadcast Receivers (respond to system-wide events) and Content Providers (manage shared data); Activities, Services and Broadcast Receivers are activated by Intents.*

---

## 8. Manifest File & Resources（p56–60）

**Manifest (`AndroidManifest.xml`)**：Android 启动 app component 之前**第一个读的文件**。
- 让系统知道 app 有哪些 components——**没在 manifest 登记的 component，系统看不见、永远不能跑**。
- 内容：**Permissions**、**Minimum API level**、**Hardware & software features used**（例 camera）、**App components**、**Linked libraries** 等。
- **Permission** = 限制对 **code** 与 **device 上的 data** 的访问，保护重要数据与代码不被滥用或破坏。

**App Resources**（p59）：Animation/Image、Menu、Style、Colour、String、Layout (UI)… 与 code 分开存放（Ch2.3 的多语言、多屏幕就靠这点）。

**p60 问题答案**：
1. Intent 的目的：发出讯息请系统启动某 component（同 app 或别的 app）。
2. 三个常见用途：**start an Activity**、**start a Service**、**deliver a Broadcast**。
3. Manifest 重要：系统靠它知道 components、所需 permissions、最低 API level 与硬件要求；没登记就不能运行，Play Store 也用它过滤不兼容的设备。

---

## ⚠️ Where the slides mislead

| Slide | Slide 写的 | 更准确的理解 |
|---|---|---|
| p38 | 两个 app 共享 Linux user ID 来分享数据 | `sharedUserId` 已 deprecated（Android 10），现代做法是 Content Provider / FileProvider。考试照 slide 答。 |
| p17 | iOS "Unix kernel" | 精确是 XNU（Darwin，Unix-like）kernel。简化说法，考试可照写。 |
| p55 | "Use Intent to activate Activities, Services, and Broadcast Receivers" | 正确；注意 **Content Provider 不用 Intent 启动**（用 ContentResolver）。 |

考试策略：题目引用 slide 的说法时照 slide 答，但可以多加一句现代做法。

---

## Term table

| English | 中文 | 一句话 |
|---|---|---|
| Native app | 原生 app | 为单一 OS 编写，最快、硬件功能最全 |
| Hybrid app | 混合 app | Web 代码 + native 外壳，单一 codebase |
| Mobile web app | 移动网页 | 浏览器里跑，无需安装 |
| Kernel | 内核 | OS 最底层，管理硬件 |
| Microkernel | 微内核 | 只保留最核心功能的 kernel（HarmonyOS、Fuchsia） |
| Security sandbox | 安全沙盒 | 每个 app 独立 UID / process / VM |
| Principle of least privilege | 最小权限原则 | 只给完成工作所需的权限 |
| Runtime permission | 运行时权限 | Android 6+ 用到时才请求 |
| APK | 安装包 | Compiled code + data + resources |
| Activity / Service / Broadcast Receiver / Content Provider | 四大组件 | ABCS |
| Intent | 意图 | 请求启动 component 的讯息 |
| Manifest | 清单文件 | 系统第一个读的文件，登记 components 与 permissions |

## Cheat sheet

| 项目 | 要点 |
|---|---|
| 3 types | Native（贵、快、硬件全、多 codebase）· Hybrid（Web+Native，单 codebase，上 store）· Web（无安装、无 store） |
| Android | Linux kernel, open source, ~70% |
| iOS | Unix kernel, closed, ~28% |
| Tizen / KaiOS | Linux, open source（KaiOS = feature phone） |
| HarmonyOS / Fuchsia | Microkernel, open source（Fuchsia: modular, Nest Hub） |
| 4 challenges | Processing power · Memory & storage · Battery · Network |
| Beyond | IoT (wearable, smart home, car), AR, AI, Blockchain |
| APK | code + data + resources |
| Sandbox | unique Linux UID, own process, own VM |
| Permission | ≤ Android 5: install time · ≥ Android 6: runtime |
| ABCS | Activity (UI) · Broadcast Receiver (events) · Content Provider (shared data) · Service (background) |
| Manifest | permissions, min API, features, components, libraries；未登记 = 不能运行 |

---

## Practice（English, with answers）

### A. MCQ
1. Which app type gives the widest access to device features? (a) Mobile web (b) Hybrid (c) Native (d) Progressive web
2. Which OS is built on a microkernel? (a) Android (b) Tizen (c) KaiOS (d) HarmonyOS
3. From which Android version are permissions requested at runtime? (a) 4 (b) 5 (c) 6 (d) 10
4. Which component has no UI and runs long operations in the background? (a) Activity (b) Service (c) Content Provider (d) Intent
5. A component not declared in the manifest will: (a) run with limited permissions (b) never run (c) run only in background (d) crash after install

**Answers**：1 (c) · 2 (d) · 3 (c) · 4 (b) · 5 (b)
> 中文提示：4 要分清 Service（背景做事）和 Broadcast Receiver（接收事件）。

### B. Short answer
**B1.** Explain TWO ways Android protects app data and user security. (4 marks)
> (1) Security sandbox – each app is treated as a different Linux user with a unique UID, runs in its own process and VM, and its files can only be accessed by that UID. (2) Permissions and least privilege – an app can only use components it needs; access to sensitive data or hardware (contacts, camera, location) must be granted by the user, at runtime from Android 6.
> 中文：两点各写"机制 + 效果"。

**B2.** Differentiate a Service from a Broadcast Receiver. (4 marks)
> A Service performs long-running work in the background (e.g. playing music, fetching data) and keeps running while started. A Broadcast Receiver has no long-running work; it is registered to listen for system or app events (e.g. battery low, reboot) and reacts briefly, e.g. by posting a notification or starting a Service.

**B3.** State FOUR items declared in the manifest file. (4 marks)
> Permissions required (e.g. CAMERA), minimum API level, hardware/software features used (e.g. android.hardware.camera), the app components (activities, services, receivers, providers), and linked libraries.

### C. Application
**C1.** A food-delivery start-up has a small budget and wants its app on both Android and iOS within 3 months, with GPS tracking. Recommend an app type and justify. (5 marks)
> Hybrid/cross-platform (e.g. Flutter or React Native): one codebase for both OS lowers cost and development time, it is still distributed through the app stores, and cross-platform frameworks provide plugins for GPS and notifications. Native would give better performance but requires two codebases and higher cost; mobile web cannot be listed in app stores and has very limited device access.

**C2.** Identify the component for each: (a) a screen showing a list of restaurants; (b) uploading a large order history to a server; (c) reacting when the phone connects to Wi-Fi. (3 marks)
> (a) Activity – it is a UI screen. (b) Service – long-running background network work. (c) Broadcast Receiver – it is registered to receive the connectivity-change system event.

**C3.** Suggest how an e-wallet app can cope with users on 3G networks. (3 marks)
> Compress/minimise data sent (small JSON payloads), cache recent balances and transactions locally so the app opens without network, and queue failed requests to retry automatically with clear status messages.

### D. Thinking
**D1.** Why is it risky for an app to request every permission at install time?
> It breaks the principle of least privilege: users cannot judge why each permission is needed, the app holds access it may never use, and a compromised app could misuse it. Runtime permissions let users grant access only when a feature is actually used, with context.

**D2.** Why does Android have no `main()` entry point for apps?
> Because any component can be the entry point: the system can start an app's Activity, Service or Receiver directly in response to an Intent or event (e.g. Gallery's "pick a photo" screen launched from WhatsApp). This lets apps reuse each other's components without sharing code.

---

## Slide index
| Note section | Slides |
|---|---|
| 1 Types of apps | p5–7 |
| 2 Languages & tools | p9–12 |
| 3 Mobile OS | p14–21 |
| 4 Challenges | p23–25 |
| 5 Beyond mobile app | p27–31 |
| 6 Fundamentals | p33–42 |
| 7 Components & Intent | p43–55 |
| 8 Manifest & resources | p56–60 |

## Links to other chapters
- Intent 的细节（explicit / implicit、intent filter）→ **Ch3**
- Resources 与多语言/多屏幕 → **Ch2 (2.3)**
- Service / background work / 网络 → **Ch4 (4.2)**
- Camera/location permissions → **Ch5, Ch6**
- App 大小与分发（App Bundles, Android Go）→ **Ch7**
