# AMIT3353 — Chapter 7: Mobile Application Packaging and Publication

> 这章 62 页里有 46 页是图（图标、截图、QR code），文字只剩关键词。这份笔记把每个图的意思写出来：launch checklist、签名流程、分发渠道、App Bundles 与 Dynamic Delivery、Android Go、Play Instant、7 种 monetisation 模式，并为 MyGOV 与 ELSA 写好满分答案。

---

## 0. 一句话总览

**发布前照 checklist 准备（政策、开发者帐号、本地化、兼容性、测试、上架资料），用自己的私钥签名，选择分发渠道（marketplace / email / website）与分发方式（App Bundles、Android Go、Instant、Chrome OS），再选一个赚钱模式。**

| Part | 内容 | Slide |
|---|---|---|
| 1 | Launch checklist（6 项） | p3–24 |
| 2 | Preparing for release、signing | p25–29 |
| 3 | Distribution channels | p31–32 |
| 4 | Distribution methods：Android Go、App Bundles、Play Instant、Chrome OS | p33–48 |
| 5 | Monetisation | p50–60 |
| 6 | Review questions | p61 |

🎯 **这章回答的考题**
- **Oct 2025 Q4c** (i) Analyse ONE distribution channel for MyGOV with reasons（5）(ii) Explain a suitable monetisation model for MyGOV（5）
- **Jan 2026 Q4b** (i) Justify TWO reasons App Bundles suit distributing ELSA to SMEs（4）(ii) Describe a suitable monetisation model for ELSA（5）

**Prerequisites**：Ch1（APK = code + data + resources；Android Go 与低端手机）；Ch2.3（按 density/语言准备的 resources——App Bundles 就是按这些拆分）。

---

## 1. Launch Checklist（p3–24）

p3–24 用一张 checklist 一项一项亮起来：

| # | Item | 重点 |
|---|---|---|
| 1 | **Developer Program Policies** | 五大政策（p4 图）：**Restricted content**、**Intellectual property**、**Privacy and security**、**Monetization and ads**、**Store listing and promotion** |
| 2 | **Developer Account** | 平台发给开发者的**发布帐号**，让你在平台上 post、展示、销售、分发 app |
| 3 | **Localization** | 让 app 符合某语言、文化或人群的 "look-and-feel"；**成功的本地化看起来像在当地开发的** |
| 4 | **Device Compatibility**（见 Ch2.3） | 确保 app 在不同配置、硬件、软件、OS 的设备上都跑得好 |
| 5 | **Quality Test; Alpha & Beta** | 确保 app 符合规范与标准、预防问题、让顾客满意 |
| 6 | **Store Listing** | 上架资料：分发地区、app 大小、平台、收费或免费 |

**Developer account 费用（p7）**：Google Play **USD 25（一次）**；Apple **USD 99（每年）**；Huawei、KaiOS、Tizen **FOC（免费）**。
**Google Play Console 功能（p11）**：All applications（安装数、评分、更新状态）、Game services（社交游戏功能）、Order management（付费 app / in-app 产品收入）、Download reports（app 表现，例按版本与国家的安装数，p12–13 图）。

**Localization 例子（p16–18）**：TED app 英文 "Get Inspired" vs 德文 "Lassen Sie…"；p17 沙漠里的人喝可乐——同一张图在不同文化意义可能不同（广告图要考虑文化）；p18 Jobs Abu Dhabi app 英文版 vs **阿拉伯文版整个 layout 左右镜像（RTL，right-to-left）**——本地化不只是翻译文字。

**Quality testing（p23 图）**：同心圆由内到外——**Developers' testing → α (Alpha) testing → β (Beta) testing**：
- **Alpha**：内部（公司内/小群测试者）在受控环境测试。
- **Beta**：发给一批真实外部用户，在真实环境收集回馈（Google Play 有 closed/open testing tracks）。

## 2. Preparing for Release & Signing（p25–29）

**Preparing for release** = 准备好 app，让用户能在设备上**安装并执行**；**release-ready 的 APK 要用你自己的 certificate 签名**。

最低需求（p26）：
1. **Cryptographic keys**（加密金钥）——开发者拥有的**数位签章**
2. **Application icon**
3. **End-user License Agreement (EULA)**
4. **Promotional and marketing materials**

**Signing in release mode（p27）**：
```
[Create a key store] ─▶ [Create a private key] ─▶ [Build your project] ─▶ [Sign your app]
   (保险箱)               (钥匙)                   (砌砖)                  (签名)
```

**Signing considerations（p28）**——为什么同一把 key 很重要：
- **Update**：更新版必须用**同一把 key** 签，系统才认得是同一个 app（掉了 key 就无法更新！）。
- **Modularity**：同一把 key 签的多个 app/模组可以在同一个 process 里跑。
- **Code/data sharing**：同一把 key 签名的 apps 可以用 signature-based permission 分享代码与资料（呼应 Ch1 的 shared user ID）。

**Store listing（p29）**：Distribution（地区）、**App size < 150 MB** + **expansion files up to 2 GB**、Platforms、Fee or Free。

## 3. Distribution Channels（p31–32）

商店（p31）：Microsoft Store、**Huawei AppGallery**、**Apple App Store**、**Google Play**。

| Channel | 说明 | 优点 | 缺点 |
|---|---|---|---|
| **Marketplace**（app store） | Google Play、App Store、AppGallery | 用户信任（审核、恶意程式扫描）、覆盖面最大、**自动更新**、评分与分析、付款系统 | 要遵守政策、审核时间、可能抽成 |
| **Email** | 把安装档寄给用户 | 适合内部少数测试者 | 用户要允许"未知来源"、不安全、没有自动更新 |
| **Website or server** | 从自己的网站下载 APK | 完全自主、不受商店规限 | 信任度低、安全风险、更新麻烦、iOS 基本不行 |

### ✅ 满分答法 — Oct 2025 Q4c(i) Analyse ONE distribution channel used to deliver MyGOV, with reasons（5 分）

> **Marketplace (official app stores – Google Play, Apple App Store and Huawei AppGallery).**
> - **Reach**: Android and iOS users in Malaysia already use these stores, and AppGallery covers newer Huawei phones without Google services, so almost every citizen can find and install MyGOV in one familiar place.
> - **Trust and security**: store apps are reviewed and scanned for malware, and the listing shows the verified developer (Government of Malaysia). This matters because MyGOV handles MyDigital ID logins and ePayments, and it reduces the risk of citizens installing fake government apps from unofficial websites.
> - **Automatic updates**: security patches and new services (the app integrates 100+ services) are pushed to all users automatically.
> - **Feedback and analytics**: ratings, reviews and download reports (by version and state/country) help the government improve the app.
> - Distributing by email or a website would require users to allow "unknown sources", which is unsafe and unsuitable for a national app.

## 4. Distribution Methods（p33–48）

### 4.1 Android Go（p34）

给**低端/入门手机**（Android Go edition）的 app，要求：
- Target **Oreo (API 26)**
- App size **< 40 MB**
- RAM 使用 **< 50 MB（apps）/ 150 MB（games）**
- **5 秒内启动**
（参考 Google 的 "Going Global Playbook"。）

### 4.2 ⭐ App Bundles（p35–37）

- 开发者上传 app 的**编译代码与 resources**（一个 `.aab`）到 Google Play。
- **APK 的产生与签名延后**由 Google Play 做。
- **Dynamic Delivery**：Google Play 用你的 app bundle **按每个用户的设备配置产生专属的、最佳化的 APK**。

```
[Upload an Android App Bundle] ─▶ [Google Play: Dynamic Delivery] ─▶ [Optimized APK for each device]
```
p36 图：一个 bundle =
```
┌──────┐  Screen density: xxxhdpi  xhdpi  mdpi  /  xxhdpi  hdpi  ldpi
│ Base │  CPU (ABI):      x86_64   arm64       /  x86     arm
│      │  Language:       fr  de  pt  /  en  zh  ja
└──────┘
例：一台 xhdpi + arm + 英文的手机，只会收到 Base + xhdpi + arm + en（图中亮色的格子）。
```
*重点：用户只下载自己手机用得到的那部分，所以 app 变小。*

### ✅ 满分答法 — Jan 2026 Q4b(i) Justify TWO reasons App Bundles are more suitable for distributing ELSA to SMEs（4 分）

> 1. **Smaller download and install size** – with Dynamic Delivery, Google Play generates an optimised APK for each SME's device that contains only the base code plus the resources it needs (its screen density, CPU architecture and language). An SME using an entry-level phone with limited storage and a slow or metered mobile connection therefore downloads a much smaller ELSA app, installs faster and is less likely to uninstall it.
> 2. **One upload that supports every device and language** – SME Bank uploads a single app bundle instead of building and signing many APKs; Google Play handles APK generation and signing for all device configurations. ELSA can include Bahasa Malaysia, English and Chinese resources and high-resolution assets for tablets, yet each user only receives what their device uses, and features such as training modules can be delivered on demand as dynamic feature modules. This simplifies release management and updates.

### 4.3 Google Play Instant（p38–42）

- **不用安装**就能用 native Android app；**点一个 URL 就执行**。
- 是现有 app 的**升级**，不是另一个独立 app；Android 5.0（API 21）以上；**由 App Bundles 支援**。
- 结构（p39）：**Base feature + Feature 1 + Feature 2**（按功能模组载入）。
- 两类：**Try**（在 Play Store 按 "Try Now" 试用）、**Instant Play**（在 Play Games app 完整玩游戏）。
- 限制：**app size ≤ 15 MB**；只能用 **API 的子集**。
- p41 图：Clash Royale 在 Play Store 有 **INSTALL** 与 **TRY NOW** 两个按钮。

**Benefits（Review Q1）**：用户不用下载安装就能试用 → 降低门槛、提高转换率；从网址/广告直接进入 app 功能；不占储存。

### 4.4 Chrome OS 与其他平台（p43–48）

- Chrome OS 支援 **Google Play Store 与 Android apps**。准备：manifest 声明触控萤幕**非必要**：
```xml
<uses-feature android:name="android.hardware.touchscreen" android:required="false" />
```
（否则没有触控萤幕的 Chromebook 在 Play Store 看不到你的 app。）
- **在 PC 用 mobile apps**：Microsoft Store（透过 Amazon Appstore）上架 Android apps；**Microsoft Link to Windows** 在 PC 上直接用手机里的 Android apps；**iPhone/iPad apps 可在 Apple silicon Mac 的 Mac App Store 上直接使用**，不需修改。

## 5. ⭐ Monetise Your App（p50–60）

### 5.1 四种主要模式

| Model | 用户付费？ | 怎么赚 | 例子（slide） |
|---|---|---|---|
| **Premium (paid)** | **Paid**（下载要钱） | 付费下载 + in-app billing；功能丰富；**市场小众** | Minecraft、NFL 2021、Toca Boca |
| **Freemium** | **Free** | 用 **in-app billing** 卖数位商品：**Durable**（买一次永久，例去广告、专业功能）· **Consumable**（用完再买，例游戏币） | Photoshop Express、Zoom、YouTube |
| **Subscription** | Free trial → 订阅 | **Free trial** + in-app billing + **subscription fee**（按月/年） | Spotify、星洲日报（图中另一个红色 "S" 图标无法确定是哪个 app） |
| **Ads** | **Free** | **AdMob + Google Mobile Ads SDK** 显示**产生收入的广告** | Pinterest、Facebook、TikTok |

### 5.2 Alternative monetisation options

| Model | 说明 | 例子 |
|---|---|---|
| **E-commerce** | Free；**B2C**；平台提供科技、物流与付款方案；赚**销售佣金 + 开店费** | Lazada、Shopee、Lelong |
| **Rewarded products** | Free；用户分享到社交媒体、做简单任务（扫 QR、答问卷）→ **拿免费东西或奖励** | 8coin、GigaGigs、Google Opinion Rewards |
| **Service** | Free；app 是**实体/线上服务的延伸**（钱从服务本身赚） | Caltex Go、Tenaga/myTNB（⚡）、Maybank MAE（老虎） |
| **Data collection** | Free；**合作/联盟 (partnership/affiliate)**；提供客服、促销、办抽奖等（收集用户资料的价值） | Facebook、Chrome、Twitter |

**p60 Q7.3 答案**：a. **PUBG Mobile** → **Freemium**（免费，in-app 卖皮肤、通行证 = consumable/durable）；b. **MyDigi** → **Service**（电讯服务的延伸，钱来自配套/话费）；c. **WhatsApp** → **Free / Data collection & business services**（个人免费，靠 WhatsApp Business API 与 Meta 生态）；d. **Touch 'n Go eWallet** → **Service / e-commerce 式交易佣金**（商家交易手续费、金融服务）。

### ✅ 满分答法 — Oct 2025 Q4c(ii) Explain a suitable monetisation model for MyGOV（5 分）

> **Service model (free app as an extension of government services).**
> MyGOV should be free to download with no ads and no in-app purchases, because it is a public-service app funded by the government under MyDIGITAL, and charging citizens or showing ads would reduce trust and exclude low-income and rural users. Its "value" comes from the services it extends: citizens pay the normal government charges (road tax, fines, licence renewals, income tax) through the built-in FPX/Touch 'n Go ePayment, exactly as they would at a counter. The government benefits indirectly through lower operating costs (fewer physical visits and counter staff), faster collection of fees and better service delivery; a small convenience/transaction fee could be charged by the payment gateway if needed, but the app itself remains free.

### ✅ 满分答法 — Jan 2026 Q4b(ii) Describe a suitable monetisation model for ELSA（5 分）

> **Service model (free app extending SME Bank's financing and advisory services)** – ELSA should be free for SMEs, since its purpose is to help 10,000 SMEs and connect them to financing. SME Bank earns revenue from the services ELSA channels: financing products (interest/profit on loans approved through the ModalNiaga integration), fee-based advisory and training programmes, and faster ScoreXcess credit scoring that lowers its processing costs. The AI diagnostics attract SMEs, and the app turns them into financing and advisory customers.
>
> *(Alternative: **Freemium** – basic diagnostics and learning modules are free, while premium features such as in-depth AI reports, one-to-one advisor sessions or certified training courses are sold through in-app billing or a subscription; justify that SMEs can try the value first before paying.)*

## 6. Review Questions（p61）答案

1. **Instant App 的好处**：不用安装即可使用/试玩；从 URL 直接开启；降低试用门槛、提高安装转换率；不占储存空间。
2. **Dynamic Delivery 的主要好处**：按用户设备配置（density、ABI、语言）产生**较小的最佳化 APK**，下载更小更快。
3. **junior developer：app 同时有一般与高解析度美术图，大于 100 MB（5 分）**：
> Publish the app as an **Android App Bundle** instead of a universal APK. With Dynamic Delivery, Google Play splits the resources by screen density, so phones download only the normal-resolution artwork and high-resolution tablets download only the high-resolution artwork; the ABI and language splits reduce size further. This keeps each download well below the 150 MB limit and speeds up installation. Large optional content (e.g. extra high-resolution packs) can be moved into on-demand feature/asset modules. (Compress images or use vector drawables for further reduction.)

---

## ⚠️ Where the slides mislead

| Slide | Slide 写的 | 更准确的理解 |
|---|---|---|
| p34 | Android Go "Target Oreo (API 26)" | Android Go edition 是从 **Android 8.1 (API 27)** 开始；数字差一版，考试照 slide 写即可。 |
| p29 | "App size < 150 MB + expansion up to 2 GB" | 150 MB 是旧 APK/基本下载上限（现在 AAB 基本模组压缩后 200 MB），expansion (OBB) files 已被 Play Asset Delivery 取代。考试照 slide。 |
---

## Term table

| English | 中文 | 一句话 |
|---|---|---|
| Developer Program Policies | 开发者政策 | 内容、知识产权、隐私、广告、上架规范 |
| Developer account | 开发者帐号 | 在平台发布 app 的帐号 |
| Localization | 本地化 | 让 app 像在当地开发的 |
| Alpha / Beta testing | 内测 / 公测 | 内部受控测试 / 真实用户测试 |
| Keystore / Private key | 金钥库 / 私钥 | 签名用；更新必须同一把 |
| EULA | 终端用户授权协议 | 使用条款 |
| Distribution channel | 分发渠道 | Marketplace / Email / Website |
| Android App Bundle (AAB) | 应用包 | 上传格式，Play 负责产生 APK |
| Dynamic Delivery | 动态分发 | 按设备配置给最佳化 APK |
| Google Play Instant | 免安装应用 | 点 URL 即用，≤ 15 MB |
| Android Go | 轻量版 | < 40 MB、RAM < 50 MB、5 秒启动 |
| Monetisation | 变现 | Premium, Freemium, Subscription, Ads, + alternatives |
| In-app billing | 应用内付费 | durable / consumable 商品 |

## Cheat sheet

| 项目 | 要点 |
|---|---|
| Checklist | Policies · Developer account · Localization · Compatibility · Quality test (α, β) · Store listing |
| Policies | Restricted content, IP, Privacy & security, Monetization & ads, Store listing & promotion |
| Account fees | Google USD 25 once · Apple USD 99/year · Huawei/KaiOS/Tizen free |
| Release needs | Cryptographic keys, icon, EULA, marketing materials |
| Signing | key store → private key → build → sign；同 key 才能 update/模组/分享 |
| Store listing | distribution, size < 150 MB (+2 GB expansion), platforms, fee/free |
| Channels | Marketplace · Email · Website/server |
| Android Go | API 26, < 40 MB, RAM < 50/150 MB, < 5 s start |
| App Bundles | 上传 code+resources；Play 产生并签 APK；Dynamic Delivery（density, ABI, language） |
| Instant | 免安装、URL 启动、API 21+、≤ 15 MB、API 子集；Try / Instant Play |
| Chrome OS | touchscreen required="false" |
| Monetisation | Premium · Freemium (durable/consumable) · Subscription · Ads (AdMob) |
| Alternatives | E-commerce · Rewarded products · Service · Data collection |

---

## Practice（English, with answers）

### A. MCQ
1. Which store charges USD 99 per year for a developer account? (a) Google Play (b) Apple App Store (c) Huawei AppGallery (d) KaiOS
2. Dynamic Delivery is a feature of: (a) Android Go (b) App Bundles (c) Email distribution (d) Chrome OS
3. Maximum size for a Google Play Instant app (per slide): (a) 4 MB (b) 15 MB (c) 40 MB (d) 150 MB
4. Buying extra lives in a game is which type of in-app product? (a) Durable (b) Consumable (c) Subscription (d) Premium
5. What must stay the same for an app update to be accepted? (a) Icon (b) EULA (c) Signing key (d) App name

**Answers**：1 (b) · 2 (b) · 3 (b) · 4 (b) · 5 (c)

### B. Short answer
**B1.** Differentiate alpha testing and beta testing. (4 marks)
> Alpha testing is done internally by the development team or a small group of in-house testers in a controlled environment, before the app is feature-complete. Beta testing releases a near-final version to a group of real external users in real environments to collect feedback and find remaining bugs before public launch.

**B2.** State the FOUR minimum requirements for preparing a release. (4 marks)
> Cryptographic keys (digital signature owned by the developer), application icon, end-user licence agreement (EULA), and promotional and marketing materials.

**B3.** Explain freemium with durable and consumable goods. (4 marks)
> A freemium app is free to download but earns money through in-app billing. Durable goods are bought once and kept permanently (e.g. removing ads, unlocking pro filters); consumable goods are used up and can be bought again (e.g. game coins, extra lives).

### C. Application
**C1.** A Malaysian news publisher wants recurring revenue from loyal readers while letting new readers sample articles. Recommend a model. (5 marks)
> Subscription with a free trial (or metered free articles): readers try content free for a period, then pay a monthly/annual subscription via in-app billing for unlimited access and ad-free reading; this gives predictable recurring income (like Sin Chew in the slides), and ads can remain for non-subscribers.

**C2.** A game studio's app is 180 MB because it includes art for all screen densities. Suggest a solution. (4 marks)
> Publish as an Android App Bundle so Dynamic Delivery gives each device only its density, ABI and language resources, reducing the download; move large optional assets into on-demand asset/feature modules.

**C3.** Why should a banking app not be distributed by email? (3 marks)
> Users must enable installation from unknown sources, which is insecure; the file could be tampered with or replaced by a fake app; and there are no automatic updates or store review, which is unacceptable for sensitive financial data.

### D. Thinking
**D1.** Why is losing the signing key a serious problem?
> Updates must be signed with the same key; without it, the developer cannot publish updates to the existing app—users would need to install a new app under a different package, losing data and ratings (Play App Signing reduces this risk by letting Google hold the app-signing key).

**D2.** Why might a government app choose the "service" model rather than ads?
> Ads would compromise trust, privacy and neutrality, and could expose citizens' data to advertisers; the app's value is reducing the cost of delivering public services, so it is funded by the government and remains free.

---

## Slide index
| Note section | Slides |
|---|---|
| 1 Checklist | p3–24 |
| 2 Release & signing | p25–29 |
| 3 Channels | p31–32 |
| 4 Methods | p33–48 |
| 5 Monetisation | p50–60 |
| 6 Review | p61 |

## Links to other chapters
- 本地化、多屏幕 resources → **Ch2.3**
- Mobile OS 与低端手机（Android Go 的动机）→ **Ch1**
- Firebase/AdMob（BaaS 的 Earn 部分）→ **Ch4.2**
