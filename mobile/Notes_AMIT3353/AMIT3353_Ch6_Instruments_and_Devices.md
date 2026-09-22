# AMIT3353 — Chapter 6: Specialized Instruments and Devices (Camera, Media, Sensors)

> 两份 past year 合计 25 分全在这章（相机方法、manifest、场景、其他 sensors）。Slide 的 manifest 清单从"3."开始，漏掉了第 1、2 项（camera feature 与 camera permission——正是考题要的）；camera intent 写成 startActivity() 却又要 onActivityResult()；accelerometer 被说成"excluding gravity"（其实是 linear acceleration 才排除重力）。这份笔记补齐并更正。

---

## 0. 一句话总览

**App 要用相机有三条路：从 Gallery 拿、用现有的相机 app（Intent，简单、免 permission）、或自己写相机（CameraX/Camera2，复杂、要 permission）；播放影音用 MediaPlayer 并记得 release()；手机的 sensors 分为 Motion、Environmental、Position 三类。**

| Part | 内容 | Slide |
|---|---|---|
| 1 | Camera considerations（决策图） | p3 |
| 2 | 三种方法：Gallery / 现有相机 app / 自己写 | p4–7 |
| 3 | Manifest declarations | p8–9 |
| 4 | Camera intent 拍照 | p10–11 |
| 5 | 相机能做什么（ML Kit、扫描、AR 导航）、decode scaled image、barcode | p12–19 |
| 6 | Audio streams、MediaPlayer、core media formats | p20–28 |
| 7 | Sensors：Motion / Environmental / Position | p29–46 |

🎯 **这章回答的考题**
- **Oct 2025 Q4b** (i) Describe TWO ways an app can handle a camera request（6）(ii) Most suitable way for MyGOV, with justification（3）
- **Jan 2026 Q4a** (i) Illustrate TWO manifest declarations for ELSA to use the camera（6）(ii) Show TWO scenarios ELSA can use the camera（4）(iii) Choose TWO other sensors to enhance user experience（6）

**Prerequisites**：Ch1 manifest 与 runtime permission；Ch3 implicit intent 与 Activity Result API（拿回拍到的照片）。

---

## 1. Camera Considerations（p3 决策图）

```
            ◇ Camera is a must?
         No │                │ Yes
            ▼                ▼
  [Get photo from      ◇ Need special processing?
   the Gallery]         No │               │ Yes
                           ▼               ▼
              [Use the existing     [Write your own
               camera app]           camera module]
```
*读图：先问"一定要现场拍吗？"——不一定就从相簿拿；要拍但不需特别处理 → 借用手机自带相机 app；要特别处理（即时扫描、滤镜、AR、控制对焦）→ 自己写。*

## 2. 三种方法（p4–7）

### 2.1 从 Gallery 拿（p4–5）

```kotlin
val selectedImageUri = remember { mutableStateOf<Uri?>(null) }
val launcher = rememberLauncherForActivityResult(
    contract = ActivityResultContracts.GetContent()
) { uri: Uri? -> selectedImageUri.value = uri }

Button(onClick = { launcher.launch("image/*") }) { Text("Pick Image from Gallery") }  // 打开 gallery
selectedImageUri.value?.let { uri ->
    Image(painter = rememberAsyncImagePainter(uri), contentDescription = null)       // Coil 显示图片
}
// manifest: READ_EXTERNAL_STORAGE（旧版）；gradle: io.coil-kt:coil-compose
```

### 2.2 用现有相机 app vs 自己写相机（p6–7）

| | **Use an existing camera app** | **Build your own camera function** |
|---|---|---|
| 做法 | **Intent**（`MediaStore.ACTION_IMAGE_CAPTURE` / `ACTION_VIDEO_CAPTURE`） | 用 **CameraX** 或 **Camera2** API 写 |
| 复杂度 | **简单** | **复杂** |
| 相机预览 | 由相机 app 负责 | 自己建 **camera preview** |
| Permission | **不需要 CAMERA permission** | **要请求 CAMERA permission** |
| 适合 | 普通拍照/录影 | 需要控制与特殊处理（即时扫描、滤镜、AR、自动对焦框） |

```
Intent 方式：   [Your App] ──Intent──▶ [Camera App] ──▶ (Camera)
                     ▲                      │
                     └──────── Photo ───────┘

自己写：        [Your App: Preview] ──Start Camera──▶ (Camera)
                        ▲                               │
                        └─ ─ ─ Camera Preview ─ ─ ─ ─ ─ ┘
```
*重点：Intent 方式里你的 app 只"收到一张照片"；自己写的方式里相机画面直接显示在你的 app 里。*

### ✅ 满分答法 — Oct 2025 Q4b(i) Describe TWO ways a mobile app can handle a camera request（6 分 = 2 × 3）

> 1. **Use an existing camera app through an Intent** – the app creates an implicit intent with `MediaStore.ACTION_IMAGE_CAPTURE` (or `ACTION_VIDEO_CAPTURE`) and launches it (with `ActivityResultContracts.TakePicture()` / `startActivityForResult`). The device's own camera app opens, the user takes the photo, and the result (image URI) is returned to the app. It needs very little code and no CAMERA permission, because the camera app handles the hardware and preview.
> 2. **Build your own camera function with the CameraX/Camera2 API** – the app controls the camera directly: it requests the CAMERA permission at runtime, shows its own camera preview inside the app, and captures images/videos with custom settings (focus, flash, overlays, real-time analysis). It is more complex to implement but gives full control and allows special processing such as document edge detection or barcode scanning.
> (A third option when the camera is not essential: let the user pick an existing photo from the Gallery with `ActivityResultContracts.GetContent()`.)

### ✅ 满分答法 — Oct 2025 Q4b(ii) Most suitable way for MyGOV（3 分）

> **Use the existing camera app via an Intent.** MyGOV only needs ordinary photos—e.g. snapping a copy of an IC, a medical receipt or a supporting document for an application—without special real-time processing. The intent approach is quick to implement, works on the wide variety of phones Malaysians use because each phone's own optimised camera app is used, and does not require MyGOV to request the sensitive CAMERA permission, which improves user trust and privacy. (Allow picking from the Gallery as an alternative for documents already photographed.)

## 3. Manifest Declarations（p8–9）⭐

| # | Declaration | 作用 |
|---|---|---|
| 1 | **Camera feature** `<uses-feature android:name="android.hardware.camera" />` | 防止 app 被安装到**没有相机**的设备（Play Store 会过滤）；加 `android:required="false"` 则相机为可选 |
| 2 | **Camera permission** `<uses-permission android:name="android.permission.CAMERA" />` | 自己写相机（CameraX/Camera2）时必须；Android 6+ 还要 **runtime** 请求 |
| 3 | **Storage permission** `WRITE_EXTERNAL_STORAGE`（`maxSdkVersion="18"`） | 把照片/影片存到外部储存（旧版） |
| 4 | **Audio recording permission** `RECORD_AUDIO` | 录影时录声音 |
| 5 | **Location permission** `ACCESS_FINE_LOCATION` | 在照片加 GPS 位置（geotag） |

⚠️ Slide p9 的清单从 **3.** 开始，第 1、2 项在 p8 只提到 camera feature，**CAMERA permission 写在 p6**（"Request Camera permission"）。考试的 "TWO manifest declarations" 就写 1 和 2。

### ✅ 满分答法 — Jan 2026 Q4a(i) Illustrate TWO manifest declarations ELSA needs to use the camera（6 分 = 2 × 3）

> 1. **Camera feature declaration**
> ```xml
> <uses-feature android:name="android.hardware.camera" android:required="true" />
> ```
> It declares that ELSA uses the camera hardware, so Google Play prevents the app from being installed on devices without a camera (or, with `required="false"`, lets it install but disables camera features). This ensures SMEs can always use camera-dependent features such as scanning documents.
>
> 2. **Camera permission declaration**
> ```xml
> <uses-permission android:name="android.permission.CAMERA" />
> ```
> It requests permission to access the camera directly (needed when ELSA builds its own scanner with CameraX). Because CAMERA is a dangerous permission, on Android 6+ ELSA must also ask the user at runtime (e.g. when they tap "Scan document") and handle denial gracefully.
>
> (Other acceptable: `WRITE_EXTERNAL_STORAGE` (maxSdkVersion 18) to save captured documents; `RECORD_AUDIO` for video pitches; `ACCESS_FINE_LOCATION` to geotag photos of business premises.)

## 4. Camera Intent 拍照（p10–11）

步骤（slide）：1. 建 Intent（`MediaStore.ACTION_IMAGE_CAPTURE` 或 `ACTION_VIDEO_CAPTURE`）→ 2. 执行 Intent → 3. 用 `onActivityResult()` 接收结果。
（⚠️ 第 2 步要拿回结果，必须用 `startActivityForResult()` 或新的 Activity Result API，不是普通 `startActivity()`。）

Compose 写法：
```kotlin
val photoFile = remember { File.createTempFile("photo_", ".jpg", context.cacheDir) }
val photoUri = FileProvider.getUriForFile(context, "${context.packageName}.fileprovider", photoFile)
val cameraLauncher = rememberLauncherForActivityResult(
    contract = ActivityResultContracts.TakePicture()     // 内部就是 ACTION_IMAGE_CAPTURE
) { success -> if (success) imageUri.value = photoUri }
// Button(onClick = { cameraLauncher.launch(photoUri) })
```
*为什么要 FileProvider？* 相机 app 在另一个 sandbox，不能直接写你 app 的档案；FileProvider 给它一个有暂时权限的 `content://` URI 来存照片。

## 5. 相机能做什么（p12–19）

- **ML Kit text recognition**（p12）：理赔车险（拍车损）、**支票存款**（拍支票）、搜寻、翻译（拍外文）、视力测验。
- **Scan document**（p13）：把纸本文件拍成清晰的数位档。
- **Recognize object and text**（p14）：对着餐厅招牌显示评分（Google Lens）。
- **Indoor navigation using Visual Positioning Systems (VPS)**（p15）：室内 AR 箭头导航（例车站里 "Turn Right 30m"）。
- **Barcode scanning**（p18）：ML Kit barcode API，**在设备上运行、不需网络**，支援 **2D（QR code）**。
- **Decode a scaled image**（p16–17）：显示太多大图会 **out of memory** → 按目标 view 的大小**缩小解码**（例 `reqWidth = 600, reqHeight = 600`）。

### ✅ 满分答法 — Jan 2026 Q4a(ii) Show TWO scenarios in which ELSA can utilise the camera（4 分 = 2 × 2）

> 1. **Scanning business documents for financing applications** – the SME uses the camera (with document scanning/ML Kit text recognition) to capture its SSM registration certificate, bank statements or invoices; ELSA extracts the text to auto-fill the ModalNiaga financing form, making submission faster and more accurate.
> 2. **Scanning QR codes for training and events** – SMEs scan a QR code at an SME Bank workshop or on printed material to register attendance, open a learning module or join an advisory session instantly (on-device barcode scanning, no typing).
> (Also acceptable: photographing business premises/products for assessment; video call with an advisor; profile photo verification.)

**p19 答案**：
1. "一定要 permission 才能用相机"——**不完全对**：用现有相机 app（Intent）**不需要** CAMERA permission；自己写相机才需要。（例外：如果你的 manifest 已宣告 CAMERA permission 但用户没同意，连 intent 也会被拒。）
2. 防止装到没相机的设备：manifest 加 `<uses-feature android:name="android.hardware.camera" />`。
3. 最快方式：**Camera Intent**（`ACTION_IMAGE_CAPTURE` / `ACTION_VIDEO_CAPTURE`）。

## 6. Audio & Video（p20–28）

- Android 为不同用途维持**不同的 audio stream**：音乐、闹钟、通知、来电铃声、系统声音、通话音量……大部分限系统事件使用；app 播**背景音乐或音效**用 **`STREAM_MUSIC`**。
- 两个 class：**MediaPlayer**（播放声音与影片）、**AudioManager**（管理音源与输出）。
- **MediaPlayer**：播**本地与串流**档案；支援平台与设备提供的 codecs；**建议用 core media formats**。
- Permissions：串流网络内容 → `INTERNET`；要保持萤幕不暗/CPU 不睡 → **`WAKE_LOCK`**。
- **Core media formats**：Audio .3gp .mp3 .mp4 .mid .wav .ogg · Picture .jpg .gif .png .bmp · Video .3gp .mp4。

```kotlin
val mediaPlayer = remember { MediaPlayer().apply { setDataSource(audioUrl); prepare() } }
Button(onClick = { if (mediaPlayer.isPlaying) mediaPlayer.pause() else mediaPlayer.start() }) { ... }
DisposableEffect(Unit) { onDispose { mediaPlayer.release() } }   // 离开 composition 时释放
```
⚠️ `prepare()` 是同步的，只适合本地/快速来源；网络串流应该用 `prepareAsync()`，否则会卡住 UI thread。

**p28 答案**：1. "可以用任何 codec"——不对：只能用 Android 平台或该设备支援的 codecs；为了兼容所有设备，应使用 core media formats。2. `WAKE_LOCK`：播放时防止萤幕变暗或处理器进入睡眠。3. **`release()`**。

## 7. Sensors（p29–46）

三大类：**Motion**（🏃）、**Environmental**（☀）、**Position**（📍）。

| Category | 量什么 | Sensors | 例子 |
|---|---|---|---|
| **Motion** | 沿三轴的**加速力与旋转力** | **Accelerometer**（速度变化率、感知轴向）· **Gravity sensor**（重力方向与强度、装置在空间的相对方向）· **Gyroscope**（帮 accelerometer 知道手机朝向，量角速度）· **Rotation vector**（量转动：azimuth、roll、pitch） | 赛车游戏倾斜手机转弯（p35）、计步/运动手表（p36）、摇一摇 |
| **Environmental** | 装置**附近**的环境参数：气温、气压、光照、湿度 | **Barometer**（气压；**提供高度资料让 GPS 更快锁定**）· **Photometer / light sensor**（环境光）· **Thermometer**（装置内温度，避免过热） | 自动亮度、楼层判断、天气 |
| **Position** | 装置的**实体位置/方向** | **Orientation sensor**（相对于正交座标系的方向：azimuth、pitch、roll）· **Magnetometer**（相对磁北的方向 = 指南针） | 地图方向箭头、AR |

其他常见（p38–39 图，Pixel 4 与 iPhone X 的瀏海）：**ambient light & proximity sensor**（通话时靠近脸关萤幕）、**IR camera / dot projector / flood illuminator**（**Face Unlock**）、**Soli radar**（Motion Sense 手势）、麦克风、前镜头。

### ✅ 满分答法 — Jan 2026 Q4a(iii) Choose TWO other sensors ELSA can utilise to enhance user experience（6 分 = 2 × 3）

> 1. **Ambient light sensor (photometer)** – it measures the amount of ambient light. ELSA can automatically adjust screen brightness or switch to dark theme when an SME owner reviews reports in a dim shop or at night, and warn "too dark" when scanning documents so photos are readable. This improves readability, comfort and battery life.
> 2. **Biometric/proximity sensors – e.g. fingerprint sensor or IR/face-unlock sensors** – ELSA handles confidential financial data, so it can use the fingerprint sensor or face unlock (IR camera + dot projector) for quick, secure login and to confirm financing submissions, instead of typing passwords. This improves both security and convenience.
> (Also acceptable: **Accelerometer** – shake to report a problem or refresh insights; **Magnetometer/GPS + barometer** – locate nearby SME Bank branches and events; **Gyroscope** – rotate charts or view AR business-premise assessments.)

**p46 答案（5 分，用 sensors 提供独特体验）**：例 fitness app——accelerometer + gyroscope 计步与判断活动类型、GPS 记录路线、barometer 计算爬楼层数、heart-rate sensor 显示心率；结合起来自动记录运动、不用手动输入。每个 sensor 写"量什么 + 在 app 中怎么用 + 对用户的好处"。

---

## ⚠️ Where the slides mislead

| Slide | Slide 写的 | 更准确的理解 |
|---|---|---|
| p9 | Manifest 清单从 "3. Storage 4. Audio 5. Location" 开始 | 第 1、2 项是 **camera feature**（p8）与 **CAMERA permission**（p6 提到）。 |
| p10 | "Execute the Intent using startActivity()" 然后 onActivityResult() | 要拿回结果必须用 **`startActivityForResult()`** 或 Activity Result API（`TakePicture()`）。 |
| p6 | Intent 方式 "No need permission" | 大致正确；但若 app 的 manifest 宣告了 CAMERA permission 却没被授予，`ACTION_IMAGE_CAPTURE` 会被拒绝（SecurityException）。 |
| p31 | Accelerometer "rate of change in velocity, **excluding gravity**" | Accelerometer 的读数**包含重力**；**linear acceleration sensor** 才排除重力。 |
| p42 | Thermometer "measures temperature within a mobile device" | 装置内温度 sensor（TYPE_TEMPERATURE）已 deprecated；Environmental 类的是 **ambient temperature**（周围空气温度）。考试照 slide 答"避免过热"也可。 |
| p26 | `prepare()` 用于串流 URL | 网络串流要用 `prepareAsync()`，否则阻塞 UI thread。 |

---

## Term table

| English | 中文 | 一句话 |
|---|---|---|
| Camera intent | 相机意图 | 借用系统相机 app 拍照 |
| CameraX / Camera2 | 相机 API | 自己写相机功能 |
| Camera preview | 相机预览 | app 内即时显示镜头画面 |
| uses-feature | 功能声明 | 过滤没有该硬件的设备 |
| uses-permission | 权限声明 | 请求存取硬件/资料 |
| FileProvider | 档案提供者 | 给其他 app 安全的 content:// URI |
| ML Kit | 机器学习套件 | 文字辨识、barcode 扫描 |
| MediaPlayer / AudioManager | 媒体播放器 / 音频管理 | 播放 / 管理音源与输出 |
| Codec | 编解码器 | 媒体格式的编码/解码 |
| WAKE_LOCK | 唤醒锁 | 防止萤幕暗/CPU 睡眠 |
| Accelerometer / Gyroscope | 加速度计 / 陀螺仪 | 加速度 / 角速度 |
| Barometer / Photometer / Magnetometer | 气压计 / 光感器 / 磁力计 | 气压(高度) / 光 / 磁北 |

## Cheat sheet

| 项目 | 要点 |
|---|---|
| 决策 | 不一定要拍 → Gallery · 要拍、无特殊处理 → 现有相机 app · 要特殊处理 → 自己写 |
| Intent 方式 | ACTION_IMAGE/VIDEO_CAPTURE；简单；免 CAMERA permission；TakePicture() |
| 自己写 | CameraX/Camera2；preview；要 CAMERA permission（runtime） |
| Manifest | uses-feature camera · CAMERA · WRITE_EXTERNAL_STORAGE(≤18) · RECORD_AUDIO · ACCESS_FINE_LOCATION |
| 相机应用 | 文字辨识、支票、文件扫描、物件辨识、VPS 室内导航、QR（离线） |
| 大图 | decode scaled（按 view 大小） |
| Audio | STREAM_MUSIC；MediaPlayer + AudioManager；INTERNET、WAKE_LOCK；release() |
| Core formats | 音 3gp mp3 mp4 mid wav ogg · 图 jpg gif png bmp · 影 3gp mp4 |
| Sensors | Motion（accelerometer, gravity, gyroscope, rotation vector）· Environmental（barometer, photometer, thermometer）· Position（orientation, magnetometer） |

---

## Practice（English, with answers）

### A. MCQ
1. Which approach needs no CAMERA permission? (a) CameraX (b) Camera2 (c) Camera intent (d) Custom preview
2. Which declaration stops installation on devices without a camera? (a) uses-permission CAMERA (b) uses-feature android.hardware.camera (c) queries (d) intent-filter
3. Which method frees a MediaPlayer's resources? (a) stop() (b) pause() (c) release() (d) reset()
4. Which sensor helps GPS lock faster by giving altitude? (a) Gyroscope (b) Barometer (c) Photometer (d) Magnetometer
5. A magnetometer is classified as a: (a) motion sensor (b) environmental sensor (c) position sensor (d) biometric sensor

**Answers**：1 (c) · 2 (b) · 3 (c) · 4 (b) · 5 (c)

### B. Short answer
**B1.** Compare using the existing camera app with building your own camera. (4 marks)
> Existing camera app: simple, uses an intent (ACTION_IMAGE_CAPTURE), the camera app provides the preview and returns the photo, no CAMERA permission needed. Own camera: complex, uses CameraX/Camera2, the app builds its own preview and controls settings, requires the CAMERA permission, but allows special processing.

**B2.** Why should images be decoded at a scaled size? (2 marks)
> Full-resolution photos use a lot of memory; displaying many of them can cause OutOfMemory errors. Decoding at the size of the destination view saves memory and speeds up loading.

**B3.** Name the three categories of sensors with one example each. (3 marks)
> Motion – accelerometer; Environmental – barometer; Position – magnetometer.

### C. Application
**C1.** An insurance app lets customers photograph car damage and automatically detect the plate number. Which camera approach? Justify. (3 marks)
> Build its own camera with CameraX: it needs special real-time processing (ML Kit text recognition on the plate, guide overlays for angles), which the existing camera app cannot provide; it must request the CAMERA permission.

**C2.** A music app keeps playing when the screen is off. State TWO permissions and ONE lifecycle concern. (3 marks)
> INTERNET (for streaming) and WAKE_LOCK (keep the CPU awake); playback should run in a (foreground) Service, and the MediaPlayer must be released when no longer needed.

**C3.** Suggest TWO sensors for a hiking app and how each is used. (4 marks)
> Barometer – measures pressure to estimate altitude/elevation gained and helps GPS lock faster; Magnetometer – compass direction so the map rotates to where the user faces. (Accelerometer for step counting also acceptable.)

### D. Thinking
**D1.** Why is on-device barcode scanning useful for a mobile app?
> It works without a network (rural areas, offline), is fast because no image is uploaded, and keeps data private on the device.

**D2.** Why is using the device's own camera app often better for varied devices?
> Each manufacturer's camera app is optimised for its own hardware (lenses, HDR, stabilisation), so photos are better and the developer avoids handling hundreds of camera configurations.

---

## Slide index
| Note section | Slides |
|---|---|
| 1 Considerations | p3 |
| 2 Three methods | p4–7 |
| 3 Manifest | p8–9 |
| 4 Camera intent | p10–11 |
| 5 Camera uses, scaled image, barcode | p12–19 |
| 6 Audio/video | p20–28 |
| 7 Sensors | p29–46 |

## Links to other chapters
- Implicit intent 与 Activity Result API → **Ch3**
- 存照片（files / external storage / FileProvider）→ **Ch4.1**
- Runtime permission、sandbox → **Ch1**
- GPS 与 barometer 合作定位 → **Ch5**
- 背景播放音乐用 Service → **Ch4.2**
