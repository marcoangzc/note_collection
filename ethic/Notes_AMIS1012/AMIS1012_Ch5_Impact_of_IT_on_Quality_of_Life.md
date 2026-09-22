# AMIS1012 — Chapter 5: Impact of IT on Quality of Life

> 92 页、15 个 topic，是最长的一章，但 slide 大多只列"好处"，**很少讲伦理问题**——而 Tutorial 5 和 SmartRetail KBAT 题几乎每一题都问 "ethical considerations / privacy risks / benefits and drawbacks"。另外 p15 的 telemedicine 定义说"可以不预约就拿药……for emergency care"，和 p12、p14 说 "only for non-emergency" 互相矛盾。这份笔记把 15 个 topic 串成一条线（医疗 → 资料 → 隐私 → 身份 → 环境），每个 topic 补上"伦理问题"，并完整回答 Tutorial 5 和 SmartRetail 五题。

---

## 0. 一句话总览

**IT 提高了效率与生活品质——尤其在医疗（telemedicine、mobility、EHR、big data、cloud）——但它同时把我们的生活变成资料（digital footprint、cookies、online identity、生物特征），带来隐私、公平（social equity）、身份验证与环境（Green IT）的伦理问题。**

| Part | 内容 | Slide |
|---|---|---|
| 1 | ICT 的定义与重要性 | p4–8 |
| 2 | Telemedicine & telehealth | p9–17 |
| 3 | Mobility | p18–21 |
| 4 | Social equity in telehealth & mobility | p22–30 |
| 5 | Electronic Health Records (EHR) | p31–35 |
| 6 | Big data & cloud computing | p36–46 |
| 7 | Privacy in digital technology（accuracy, property, access） | p47–52 |
| 8 | Internet & the web（IP、deep/dark web、history、cache、**cookies**） | p53–59 |
| 9 | Online identity | p60–61 |
| 10 | Facial recognition & privacy rights | p62–70 |
| 11 | Fingerprint & applied ethics | p71–74 |
| 12 | Smartphone applications | p75–76 |
| 13 | Social media（identity verification） | p77–85 |
| 14 | Major laws on privacy | p86 |
| 15 | Green IT | p87–89 |

🎯 **这章回答的考题**
- 五份 past year **没有直接考 Ch5**。但 **Tutorial 5（10 题）** 和 **SmartRetail KBAT（5 题：AI 就业、facial recognition、algorithmic bias、IoT 员工监控、cloud 安全）** 都以这章为主，而且是 "evaluate / justify / recommend" 的高阶题型。→ 重点练习"好处 vs 伦理问题 + 建议"的答题结构。

**Prerequisites**：Ch3 的 privacy laws（HIPAA、GDPR、PDPA）、workplace monitoring、consumer profiling；Ch4 的 green procurement；Ch6 的 CIA（cloud 安全题会用到）。

---

## Scene：Mak Cik Siti 的一天

Mak Cik Siti 住在吉兰丹乡下，有糖尿病。
- 早上她用手机 **video call** 看城里的医生（telemedicine），血糖机的读数**自动上传**（remote patient monitoring + mobility）。
- 医生打开她的 **EHR**，看到三年来的记录；医院的 **big data** 系统预测她的风险。
- 但她的孙子说："阿嬷，你的资料都在**云端**，会不会被偷？"而且村里很多老人**没有智能手机、不会用 app**。
- 下午她去超市，**摄像头认出她的脸**给她折扣；晚上她看 Facebook，发现广告全是糖尿病产品——**cookies** 记得她搜过什么。

同一套科技，让她生活更好，也让她的资料无所不在。这一章就是沿着她的一天走。

---

## 1. ICT（p4–8）

- **引言**：IT 鼓励创造力、提高效率；管理层用 IT 推出产品、流程、服务的改进；生产力提升很容易计算（例：引入电子 payroll 后裁掉一些会计员）。← 这句话本身就带出**就业**的伦理问题（→ SmartRetail Q1）。
- **ICT** = 把**电话与电脑网络整合**在单一布线设备中，让**资料收集、利用、控制、检索**变得简单。（p5 图：Information + Communication + Technology）

**ICT 的重要性（p6–8）**：

| 领域 | 重要性 |
|---|---|
| 教育 | 加强学生 **critical thinking**（研究并找出解决方案）；鼓励**创意与创新**；**e-learning** 平台与线上图书馆（资讯更易传播） |
| 资料 | 储存与检索资料 |
| 网络 | 改善电脑网络（Internet、intranet） |
| 经济 | **促进国家经济发展**；创造高收入工作 |
| 学术 | 让本地与国际学者交流想法 |
| 全球 | **Globalisation** 的关键 |
| 政府 | 各部门保存政府运作与行政记录 |

### ✅ 满分答法 — Tutorial 5 Q1: How does ICT enhance students' critical thinking, and what are the implications for their future problem-solving?
ICT gives students access to vast online resources, databases and e-learning platforms, so they must search, compare and evaluate information from many sources rather than memorise one textbook; simulations, coding and collaborative tools let them test ideas and see results. This builds skills in analysing problems, judging source credibility and creating innovative solutions. **Implications:** students become more independent, adaptable problem-solvers who can use digital tools to research and solve unfamiliar real-world problems at work — but they also need digital literacy to avoid misinformation and over-reliance on technology (e.g. copying AI answers without thinking).

### ✅ 满分答法 — Tutorial 5 Q2: How has ICT impacted the efficiency of modern businesses compared to traditional methods?
- **Automation** – tasks once done manually (payroll, inventory, billing) are processed automatically, faster and with fewer errors (e.g. electronic payroll replacing manual accounting).
- **Instant communication and collaboration** – email, video conferencing and cloud tools replace letters and physical meetings, allowing global teams to work in real time.
- **Data storage and retrieval** – digital databases replace paper files, making information searchable instantly and supporting data-driven decisions.
- **Wider market reach** – e-commerce lets businesses sell 24/7 worldwide without physical branches.
Overall, ICT reduces cost and time and increases productivity, although it requires investment in systems, training and security.

---

## 2. Telemedicine & Telehealth（p9–17）

- **Telemedicine** = 病人**不必亲自去诊所/医院**，透过科技（video、web portal、email）与医疗人员**实时**讨论病情。
- **目的（p10）**：**Get diagnosis**、**get information on medical choices**、**get prescription**。

**三种 telemedicine（p11）**：

| Category | 说明 | Mak Cik Siti |
|---|---|---|
| **Immersive (live) telemedicine** | 医生与病人**实时**互动 | Video call 看医生 |
| **Remote patient monitoring** | 护士用手持医疗设备记录**血压、血糖**等 | 血糖读数自动上传 |
| **Store and forward** | 把病人资料**转给其他医生或专家** | 家庭医生把她的眼底照片传给眼科专家 |

- **适合**：**非紧急**情况（小毛病、覆诊）；例：怀疑伤口感染、strep throat、心理治疗（psychotherapy）、tele-dermatology（痣、疹）、感冒、虫咬、喉咙痛、腹泻、红眼。
- **不适合**：**紧急情况**、需要 X-ray/夹板/石膏的情况（心脏病、中风、伤口撕裂、骨折）——任何需要**紧急、亲手处理**的情况都要**亲自就医**。
- **Mental health** 是 telemedicine **最有益**的领域：一键连线精神科医生，成本只是门诊的一小部分。

**Telemedicine vs Telehealth（p15）**：

| Telemedicine | Telehealth |
|---|---|
| **临床**服务：远程诊断、治疗、开药 | **更广**：包括 telemedicine **加上非临床活动**——预约准备、医学继续教育、专科训练 |

⚠️ p15 对 telemedicine 的描述是 "Patient get medication without having to have an appointment with a doctor or visit their clinic **for emergency care**"——与 p12/p14 "只适合非紧急情况"矛盾。正确理解：telemedicine 让病人**不必亲自到诊所**就能看诊拿药，**但不适用于紧急情况**。

**Telemedicine 的好处（p16）**：

| Advantage | 说明 |
|---|---|
| **Comfort and convenience** | 不用开车去诊所；时间紧的人更容易安排 |
| **Control of infectious illness** | 医生可预先筛查传染病，减少他人接触 |
| **Better assessment** | 医生看到病人的**家居环境**（如找出过敏源、心理评估） |
| **Family connections** | 家人可一起参与：提供资讯、提问、记录医生的回答 |

### ✅ 满分答法 — Tutorial 5 Q3: How have telemedicine and telehealth transformed the doctor–patient relationship, and what ethical considerations arise?
**Transformation:** consultations move from face-to-face clinic visits to video calls, portals and messaging; patients can reach doctors (including specialists far away) quickly and more often, family members can join, and doctors see patients in their home environment. The relationship becomes more continuous and patient-centred, but with less physical examination and personal touch.
**Ethical considerations:**
1. **Privacy and confidentiality** – health data is transmitted and stored online and may be intercepted or leaked; platforms must be secure (encryption, HIPAA/PDPA compliance).
2. **Quality of care / misdiagnosis** – without physical examination, some conditions may be missed; doctors must recognise when in-person care (emergencies) is needed.
3. **Informed consent** – patients must understand the limits and risks of remote consultation and how their data is used.
4. **Equity of access** – elderly, rural or low-income patients without devices, Internet or digital skills may be excluded (digital divide).

---

## 3. Mobility（p18–21）

- Smart devices 与 apps 正在**改变病人与医疗提供者的关系**。
- **Mobility + cloud access** 扩大了医患之间的连接：医院、保险公司、诊所把病人资料存在**云端**取代纸本病历，病人可 **24/7** 在线看检查结果。
- Mobility 是 telemedicine 的**推动者 (enabler)**：连线设备实时串流医疗资料、手持设备的高清音讯、**IoT 医疗设备**的资讯分享。
- **Mobility 的好处（p20）**：**improving efficiencies**；改善病人**院内外的体验**；改善病人与医疗人员的**联系与协调**；以手机**追踪病人**有望**大幅节省医疗成本**。
- 记录、交换、查看、协作医疗知识都变得非常简单 → 加快 telemedicine 普及；医疗 app 越来越多 → 医院更有效、病人状况改善。

### ✅ 满分答法 — Tutorial 5 Q4: How do mobility and cloud computing influence patient care and healthcare provider efficiency?
**Patient care:** patients can view test results and records online 24/7, receive reminders, consult doctors through apps, and be monitored remotely through IoT devices (e.g. glucose meters), so problems are detected earlier and chronic diseases are managed better at home.
**Provider efficiency:** doctors access up-to-date records from any location instead of searching paper charts; cloud storage removes the cost of maintaining servers and provides backup; data can be shared instantly between hospitals, labs and insurers; remote monitoring reduces unnecessary visits and hospital stays, saving costs. *Ethical caution:* data in the cloud must be secured and access controlled.

---

## 4. Social Equity in Telehealth & Mobility（p22–30）

- **Social equity** = 确保**所有人**——不论收入、年龄、地点、残障或数位技能——都能**公平地**受惠于科技化的医疗与交通服务；目标是科技进步**不让某些群体处于劣势**。
- **为什么重要**：乡村与低收入群体网络差、缺设备 → 医疗上的 **digital divide**；没有智能手机、流量、电子支付的人难以使用这些服务 → 限制出行机会。

**四个挑战 → 结果（p23–26）**：

| Challenge | 说明 |
|---|---|
| **Digital divide** | 两者都依赖 Internet、智能手机、数位平台；低收入与乡村人口连线差、缺设备 |
| **Digital literacy** | 需要基本数位技能；**长者**与缺乏经验的用户会有困难 |
| **Accessibility issues** | 有些平台没有为**残障人士**设计 |
| **Result** | 虽然方便有效，但可能**无意中排除**某些群体 → 在医疗与交通等必要服务上**不平等** |

**四个解决方法：以 ethical computing 促进 social equity（p27–30）**：

| Solution | 做法 | 例子 |
|---|---|---|
| **Inclusive design** | User-centred、universal design | Telehealth app 有 **screen reader** 与**多语言**；交通 app 介面简单方便长者 |
| **Affordable access** | 降低设备、网络、服务费用门槛 | **补贴网络/低价流量**；为低收入通勤者提供便宜交通选择 |
| **Fair algorithms** | 确保演算法不制造或加深不平等 | Telehealth 排程**不只优先精通科技的用户**；**ride-hailing 不应在低收入地区一直加价** |
| **Digital education** | 提升数位素养 | 社区培训教长者用 telehealth；工作坊教用交通 app 与电子支付 |

→ Mak Cik Siti 的村子正是 digital divide + digital literacy 的例子。

---

## 5. Electronic Health Records（p31–35）

- **EHR** = 病人**纸本病历的电子版**；**实时、以病人为中心**，让**授权用户**方便安全地取得资讯。它比传统诊所报告更完整，呈现病人照护的全貌。
- **功能（p32）**：储存病史、诊断、处方、康复计划、**疫苗日期、过敏**、放射影像、检验结果；提供**实证资源**支援临床决策；简化医生工作流程。

**EHR 的好处（p33–35）**：

| Benefit | 说明 |
|---|---|
| **Better quality of care** | 资讯易取得 → 更有效治疗；**health analytics** 帮医生找模式、预测诊断、建议治疗；病人可在 portal 看记录、与医生传讯、视讯 |
| **Interoperability (willingness to communicate)** | 与 **EMR** 等系统整合，提升治疗品质；对慢性病、转到居家照护的病人尤其关键 |
| **Increased productivity** | 更精准的诊断；减少候诊时间；医生能看更多病人 |
| 其他 | 增加病人参与、改善照护协调、改善诊断与结果、提高效率与节省成本；**随时随地**取得资讯 |

*Extra*：**EHR vs EMR** —— EMR 是**单一诊所内**的电子病历；EHR 可**跨医疗机构共享**。

### ✅ 满分答法 — Tutorial 5 Q6: Discuss the potential privacy risks of widespread EHR adoption and how they might be mitigated.
**Risks:**
1. **Data breaches and hacking** – centralised EHR databases containing sensitive health data are attractive targets for cybercriminals and ransomware.
2. **Unauthorised internal access** – staff may view records of patients they are not treating (e.g. a celebrity's or neighbour's).
3. **Secondary use and sharing without consent** – records may be shared with insurers, employers or marketers, leading to discrimination.
4. **Inaccurate data spreading** – errors copied across interoperable systems can harm patients.
**Mitigation:**
- Role-based access control (least privilege) with strong authentication (MFA), and audit logs that record every access, reviewed regularly.
- Encryption of data at rest and in transit; secure cloud configuration; regular security assessments.
- Clear consent and privacy policies complying with laws such as HIPAA/PDPA; patients can view who accessed their record and request corrections.
- Staff training on confidentiality, with disciplinary action for misuse.

---

## 6. Big Data & Cloud Computing（p36–46）

- **Big data** 成为热门词，因为能从**多种来源产生并收集大量资料**；资料用于 **analytics → 预测未来**（例：**及早发现疾病爆发**，最终减少死亡）。
- **Cloud 在医疗的角色**：资料管理更有生产力与可及性、减少浪费；支援**新疗法与药物研发**；大量研究与临床资料交换；**不用自己维护 server** 就有可靠低成本的储存、**backup 与 recovery**。
- **为什么 cloud 最适合 big data（p39）**：**flexibility, security, parallel processing, scalability, resource virtualisation**；降低自动化、资讯、基础设施维护成本，提升效能与用户存取。

**Cloud computing 特性（p40）**：

| Characteristic | 说明 |
|---|---|
| **Uses Internet technologies to offer elastic services** | 动态取得运算资源，支援变动的工作量；服务商维护庞大基础设施 |
| **Metered resources** | 资源用量可计量，**用多少付多少** |
| **User convenience** | 维护与安全由服务商负责 |

**Cloud computing 总览图（p41，照 slide 结构）**：

```
   Delivery models                            Deployment models
   ┌───────────────────────────┐             ┌──────────────────┐
   │ SaaS  Software as a Service│             │ Public cloud     │
   │ PaaS  Platform as a Service│             │ Private cloud    │
   │ IaaS  Infrastructure aaS   │             │ Community cloud  │
   │ DBaaS Database as a Service│             │ Hybrid cloud     │
   └─────────────┬─────────────┘             └────────┬─────────┘
                 └──────────┐        ┌────────────────┘
                            v        v
                      ┌──────────────────┐
                      │ Cloud computing  │
                      └──┬──────┬──────┬─┘
             ┌───────────┘      │      └─────────────┐
             v                  v                    v
   Infrastructure          Resources            Defining attributes
   - Distributed infra     - Compute & storage  - Massive infrastructure
   - Resource              - Networks           - Utility computing,
     virtualization        - Services             pay-per-usage
   - Autonomous systems    - Applications       - Accessible via Internet
                                                - Elasticity
```
*看图重点：四组东西都围绕中心——"怎么交付"（SaaS/PaaS/IaaS/DBaaS）、"部署在哪"（public/private/community/hybrid）、"底层是什么"、"有什么特性"。*

**Cloud 如何影响医疗（p42–46）**：

| Impact | 说明 |
|---|---|
| **Cost-cutting** | 不必预先购买硬件；可扩展、有效处理大量病人资料 |
| **Interoperability ease** | 连接多个来源的资料，改善规划与协作；药厂、保险、支付系统之间资料共享更顺畅 |
| **Links to specialised analytical methods** | 汇整分析结构化与非结构化资料；结合 big data 与 AI → 研究与**个人化治疗** |
| **Data possession by the patient** | 资料去中心化，病人参与决策；快速储存检索影像；自动备份（**但仍有安全顾虑**） |
| **Skills of telemedicine** | 远程存取资料 → 改善 telemedicine、出院后照护、虚拟服药追踪 |

### ✅ 满分答法 — Tutorial 5 Q5: How does Big Data analytics transform healthcare, and what ethical considerations arise?
**Transformation:** analysing massive data from EHRs, wearables, labs and populations allows early detection and prediction of disease outbreaks, identification of high-risk patients, personalised treatment plans, faster drug research and better resource planning in hospitals.
**Ethical considerations:**
1. **Privacy and consent** – patients may not know their data is being analysed or shared; data must be anonymised and used with consent.
2. **Data security** – large datasets in the cloud are attractive to attackers.
3. **Bias and fairness** – algorithms trained on incomplete data may give worse predictions for minority or rural groups.
4. **Ownership and accountability** – who owns the data and who is responsible if an algorithm's prediction harms a patient?

---

## 7. Privacy in Digital Technology（p47–52）

- 手机有相机、麦克风、各种感应器，**随时可分享你的生活**。在一切都被记录的社交媒体世界，隐私意味着什么？**谁拥有这些回忆？谁有权分享、删除、从中获利？**
- **Privacy** 关心**个人资料的收集与使用**。三大 privacy issues：

| Issue | 意思 |
|---|---|
| **Accuracy** | 收集资料的人有责任确保资料**正确** |
| **Property** | 谁**拥有**资料 |
| **Access** | 持有资料的人有责任**控制谁能使用** |

- 几乎所有事都被数位记录：打电话（打给谁、何时、在哪、甚至内容）、上传影片、戴智能手表 → 巨量储存 → **big data**。
- 大机构持续收集我们的资料：美国联邦政府有 2,000+ 资料库；信用卡公司（消费、付款、信用）；超市扫描器（买什么、何时、多少、价格）；银行；**搜寻引擎**（搜寻历史）；社交网站（每一则贴文）。
- 把这些资料串起来 → **digital footprint**（非常详细的个人生活记录）；**information resellers / information brokers** 收集、分析、贩卖个人资料。
- **Digital footprint 引发的问题（p52）**：① 收集**公开但可识别个人**的资讯；② **未经同意**散播资讯；③ 散播**不正确**资讯。

---

## 8. Internet and the Web（p53–59）

- **IP address**：Internet 上每台电脑的**唯一号码**；可**追踪活动来源**，协助执法调查未授权存取、非法分享版权档案等。
- **Deep web vs dark web**：

| | Deep web | Dark web |
|---|---|---|
| 定义 | **不被一般搜寻引擎索引**的网站（p54：允许安全匿名的通讯） | Deep web 中的一部分**隐藏网站**，需**特殊软件**（如 Tor），**隐藏用户 IP**，几乎无法识别使用者 |
| 例子 | 网上银行帐户页、学校成绩系统、医院 EHR、付费内容（需登入） | 匿名市场、举报平台 |
| 风险 | 一般合法 | 匿名性吸引**罪犯**（卖毒品、非法野生动物贸易），但也被记者、异见者用来保护隐私 |

- **浏览器会在你不知道的情况下储存资料**：
  - **History files**：最近访问的网址（在网址列、History 页显示）。
  - **Temporary Internet files (browser cache)**：网页内容与显示指示；再访问时用来**快速重新显示**。
- **Cookies**：网站存放在硬碟上的**小资料档**，用来监控网络活动；可依浏览器设定**接受或封锁**。好处：**个人化体验**。本身无害，但因为储存了你的资讯、偏好、浏览习惯而成为隐私风险。

| | First-party cookie | Third-party cookie |
|---|---|---|
| 谁产生/读取 | **只由你正在访问的网站** | 通常由与网站合作的**广告公司** |
| 目的 | 记住 session、偏好、在站内的活动 → 个人化 | **跨网站追踪**你的活动 → 又叫 **tracking cookies** |
| Slide 例子 | 购物车没结帐就离开，回来时还记得购物车内容 | 访问 3 个汽车网站后，第 4 个网站（搜寻引擎）出现汽车广告 |
| 隐私风险 | 较低（只在该网站） | **较高**（建立跨网站的行为 profile） |

- **管理方法**：删除浏览记录；**privacy mode**（Chrome **Incognito**、Safari **Private Browsing**）不把浏览活动存进硬碟。

### ✅ 满分答法 — Tutorial 5 Q7: How do first-party vs third-party cookies affect privacy, and what can users do?
**First-party cookies** are created and read only by the website being visited; they store session data and preferences (e.g. shopping-cart contents), so the privacy impact is limited to that site. **Third-party cookies** are created by advertising companies affiliated with many websites and track the user's activity from site to site, building a detailed profile of browsing habits for targeted advertising — a much greater privacy risk because the user often does not know who is collecting the data.
**Measures:** block third-party cookies in browser settings; regularly delete cookies and browsing history; use privacy mode (Incognito/Private Browsing); reject non-essential cookies in consent banners; use privacy-focused browsers or tracker-blocking extensions.

### ✅ 满分答法 — Tutorial 5 Q9: How does the deep web differ from the dark web, and what are the implications for privacy and security?
The **deep web** is all web content not indexed by standard search engines, such as online banking pages, email inboxes and subscription content behind logins; it is mostly legitimate and protects users' private data. The **dark web** is a small hidden part of the deep web that requires special software (e.g. Tor) which hides users' IP addresses, making users almost impossible to identify.
**Implications:** the deep web supports privacy by keeping personal accounts out of public search. The dark web offers strong anonymity, which protects journalists and whistleblowers, but also attracts criminals selling drugs, stolen data and malware, making investigation difficult; stolen personal data from breaches often ends up for sale there, increasing identity-theft risk.

---

## 9. Online Identity（p60–61）

- **Online identity** = 人们**自愿**在网上发布的关于自己的资讯。
- 社交网站、博客、照片影片分享让很多人**不考虑后果**地发布私密细节；网络的**存档与搜寻**功能让这些资讯**无限期地**对任何人开放。
- 很多人**因社交网站贴文而丢工作**：老师（粗言秽语、喝酒照片）、大公司 CFO（讨论公司交易与财务资料）；大学毕业生因 Facebook 贴文**被拒聘**。

### ✅ 满分答法 — Tutorial 5 Q10: Evaluate the implications of online identity management on personal and professional reputation.
**Positive:** a carefully managed online identity (e.g. a professional LinkedIn profile, portfolio, constructive posts) builds credibility, helps employers and clients find and trust a person, and creates career opportunities.
**Negative:** because the web archives and makes content searchable indefinitely, careless posts — offensive language, party photos, confidential work details — can be discovered years later by employers, leading to rejected job applications or dismissal (as in the teacher and CFO cases). Oversharing personal details also increases the risk of identity theft and harassment.
**Evaluation:** online identity is effectively a permanent public record; individuals should think before posting, use privacy settings, separate personal and professional accounts, and regularly search and clean up their digital footprint.

---

## 10. Facial Recognition & Privacy Rights（p62–70）

- **Biometric facial recognition** = 脸部测绘 (facial mapping) 的数位化延伸，用**演算法**比较**脸部特征的排列**（与指纹辨识类似）。

**五个步骤（p63–66，照 slide 的流程图）**：

```
 01 Capture ─────> 02 Processing ────> 03 Modelling
 take a digital     scale & align the    quantify features into
 photograph         face to a baseline   a contour map → digital
                    position             template
                                               │
                                               v
                    05 Decision making <── 04 Comparison
                    numerical score =      compare pairs of
                    probability they are   digital templates
                    the same person
```
*看图重点：最后得到的是**机率分数**，不是"是/否"——所以一定会有 false positive / false negative。*

- **Verification = one-to-one matching**：把**现场的脸**与**身份证件里的 template** 比对（例：在边境出示护照）。
- *Extra*：**Identification = one-to-many**：把一张脸与整个资料库比对（例：商场摄像头找出"回头客"——SmartRetail 就是这种），隐私风险更高。

**限制（p67）**：

| Limitation | 说明 |
|---|---|
| **Accuracy** | 准确度不如**指纹或 DNA** |
| **Facial features** | 某些脸部特征在人口中出现的**频率未知** |
| **Image quality** | 影像品质影响比对 |
| **Stored images** | 资料库影像的**数量**会影响准确度 |
| **Changes over time** | 脸会随时间改变 → false positive / negative |

**Privacy rights（p68–70）**：
- 生物特征脸部辨识引发伦理顾虑，尤其是 **security vs individual privacy** 的冲突。
- **Privacy** 是人相对于他人、国家与组织的权利，关于：
  - **(a) Informational privacy**：他人/组织**持有关于自己的资讯**（包括脸部影像，例如 biometric 资料库）；
  - **(b) Observational privacy**：**被他人观察**，包括行踪与人际关系。
- **Facial recognition 同时牵涉 informational 和 observational 两种隐私问题**（存了你的脸 + 看着你去哪里）。

---

## 11. Fingerprint & Applied Ethics（p71–74）

- **Biometric identification** = **测量人体的物理特征**：皮肤纹路、皮下血管网络、基因编码、脸部外观（眼鼻口的距离）。
- **为什么用 biometrics（p72）**：**每个人独特**；**不随时间改变**；可用演算法**数位化**；可整合进**自动化资料库储存与搜寻**；相对于钥匙、身份证、密码（可能遗失、被偷、被忘记）。
- **Fingerprint identification**：基于**脊纹 (ridges)** 标准图案中的差异；分为 **arches（弓）、loops（箕）、whorls（斗）**；图案中心叫 **core**，偏离点叫 **delta**（p73–74 图标示 Core 与 Delta）。
- 已广泛用于执法以外：首个重大发展是把指纹（与脸部辨识）**整合进护照与边境管制系统**。
- *Extra — 伦理重点*："unchanging" 也是缺点：**密码被偷可以改，指纹被偷改不了**，所以生物特征资料外泄的后果更严重。

---

## 12. Smartphone Applications（p75–76）

- 指纹与脸部辨识广泛用于**解锁手机**。
- 因为安全性高，**持有一部登记在某人名下的手机**已成为那个人**身份或位置的代理 (proxy)**：
  - 当作 **tap-and-pay** 工具（取代信用卡/debit card）；
  - 用 **QR code 扫描**记录某人**出现在某地**（如疫情期间的 check-in）；
  - 进入社交媒体与其他网上帐户。
- 所以手机已成为**"自我"在身份识别上的延伸**。

---

## 13. Social Media（p77–85）

- 手机的生物辨识同时控制**社交媒体帐户**的存取 → 成为网上身份的关键指标；脸部辨识也被用来在平台上**识别与连结个人**（如 Facebook 的 tagging）。
- **Social media 的定义**：参与者之间有**互动**、以**非阶层式**协作；可发布**自产内容**、建立 profile、留言/按赞、与相同兴趣者建立网络。
- Google、Facebook 因握有数十亿用户的网络活动资料而**极具权力**；资讯的呈现方式能**影响社会观点与趋势**；与传统媒体相比**缺乏中央控制** → 谣言容易传播。

**Identity verification for social media（p80–85）**：
- 一些国家提议：开或维持社交媒体帐户须提供**身份证明**（护照、驾照）。
- **目的**：处理用匿名帐户骚扰和虐待（**technology-facilitated abuse**）或其他犯罪。
- 匿名环境中恶毒留言普遍；针对种族、民族、宗教、性取向的**网上骚扰**普遍——调查显示**三分之一**的人经历过，影响健康、安全、生产力。
- 也可能减轻 **fake news**（常靠 **botnets** 自动传播，例：COVID-19 错误资讯）与**针对性格和社会观点的政治广告**（影响总统选举投票）。
- 支持论点：社交媒体**"太有影响力，不应匿名"**——就像开车、拥有枪械需要登记一样。
- **至今尚无**法律强制社交媒体身份验证；它**可能**遏止骚扰、仇恨言论、假讯息，并便于调查起诉。
- **潜在问题**：**Data security**——把护照、驾照交给已掌握大量用户资料（包括位置 metadata）的跨国公司，会成为**犯罪集团的目标**，大幅提高已有敏感资料的风险。

### ✅ 满分答法 — Tutorial 5 Q8: How might identity verification for social media affect misinformation and harassment? Benefits and drawbacks?
**Effect:** if users must verify their real identity, people are less likely to post abusive content or spread deliberate misinformation because they can be identified and held accountable; bot and fake accounts used to spread fake news would be harder to create.
**Benefits:** deters online harassment and hate speech; reduces fake/bot accounts and coordinated disinformation; makes investigation and prosecution of online crimes easier.
**Drawbacks:** (1) **data security** – platforms would hold copies of passports and licences in addition to existing personal data, creating a high-value target for hackers and organised crime; (2) **loss of anonymity** can silence whistleblowers, activists, abuse victims and minorities who rely on anonymity for safety; (3) people without official ID could be excluded; (4) verification cannot stop verified users from spreading misinformation.
**Conclusion:** it may reduce abuse but must be balanced against privacy and free expression — e.g. verification held by a trusted third party rather than the platform.

---

## 14. Major Laws on Privacy（p86）

| Law | 保护 |
|---|---|
| **Gramm-Leach-Bliley Act** | 个人**财务**资料 |
| **HIPAA** | **医疗**记录 |
| **FERPA** (Family Educational Rights and Privacy Act) | 限制**教育记录**的披露 |

- **私人组织收集的大多数资料**并**不受现有法律**保护；但随着人们越来越关心谁有权取得与使用个人资料，企业与立法者将会回应。（→ Ch3 的 GDPR、PDPA、CCPA 就是这种回应。）

---

## 15. Green IT（p87–89）

- **Green IT** = 以**环境可持续**的方式**设计、使用与管理** IT 系统。
- IT 在三个阶段消耗能源与资源：**Manufacturing**（硬件生产）→ **Operation**（运算、储存、网络）→ **Disposal**（e-waste）。Green IT 在**每一阶段**减少伤害。

| Importance | 说明 |
|---|---|
| **Environmental sustainability** | ICT 是全球碳排放的重要来源；Green IT 减少能耗与污染 |
| **Growing e-waste problem** | 科技快速升级 → 电子垃圾增加；不当弃置 → 有毒的环境与健康影响 |
| **High energy consumption** | 数据中心、云端、网络耗电巨大，且随数位转型持续增加 |

| Challenge | 说明 |
|---|---|
| **High implementation cost** | 节能系统与绿色基础设施需要高初期投资 |
| **Rapid technological obsolescence** | 设备很快过时 → e-waste 增加 |
| **Lack of awareness** | 很多组织**重性能、轻环保** |
| **E-waste management issues** | 许多国家回收系统不完善 |

---

## SmartRetail Sdn. Bhd. KBAT — 满分答法

> 结构建议（每题约 8–10 分）：**① 找出伦理问题（引用 slide 概念）→ ② 两面评估（好处 vs 风险）→ ③ 立场/结论 → ④ 具体建议**。

### ✅ Q1. AI & Employment — Evaluate the ethical implications of replacing customer service staff with AI chatbots, and recommend TWO measures.
**Ethical implications:**
- *Benefits:* lower cost, 24/7 service, faster answers to simple questions, consistent responses — ICT increases productivity (as with electronic payroll replacing accountants).
- *Employees:* job losses and loss of income for staff who served the company; this raises fairness and corporate responsibility concerns (stakeholder protection, Ch4).
- *Customers:* customers with complex problems, the elderly and those with low digital literacy may struggle to get help — a social-equity issue; chatbots may give wrong answers without accountability; conversations create more customer data to protect.
**Evaluation:** replacing staff entirely prioritises cost over people; a balanced approach is more ethical and better for service quality.
**Measures:**
1. **Hybrid model with human escalation** – use chatbots for routine queries but always provide an easy option to reach a human agent for complex or sensitive issues.
2. **Reskill and redeploy staff** – retrain affected employees as chatbot supervisors, complaint handlers or data analysts rather than dismissing them, and communicate changes transparently with fair compensation for any unavoidable redundancies.

### ✅ Q2. Facial Recognition & Privacy — Analyse the ethical concerns and evaluate whether SmartRetail should continue.
**Concerns:**
1. **Lack of informed consent and transparency** – customers did not know their faces were scanned; biometric data is personal data (GDPR lists biometric data; PDPA requires notice and consent).
2. **Both informational and observational privacy are affected** – SmartRetail holds a database of faces (informational) and tracks customers' visits and movements (observational).
3. **Accuracy and discrimination** – facial recognition is less accurate than fingerprints/DNA; image quality and changes over time cause false matches, which may give wrong discounts or wrongly flag people.
4. **Security** – unlike passwords, faces cannot be changed if the biometric database is breached.
**Evaluation / decision:** SmartRetail **should not continue in its current form**. The business benefit (personalised discounts) does not justify covert collection of sensitive biometric data. It may continue **only if** it becomes opt-in: clear signage and explicit consent, a membership app for customers who want discounts, strict data minimisation (store templates not photos, delete after a set period), encryption and compliance with the PDPA. Otherwise it should switch to non-biometric loyalty methods (QR or member card).

### ✅ Q3. Big Data & Algorithmic Bias — Evaluate the fairness of algorithm-based discounts and recommend measures.
**Evaluation:** personalised promotions can benefit customers and the business, but giving different prices for similar products based on browsing and purchase profiles can be unfair when customers do not know how prices are set; the algorithm may learn from biased data and consistently disadvantage certain groups (e.g. charging more in some locations or to people less likely to shop around), similar to the slide's warning that ride-hailing algorithms should not raise prices in lower-income areas. Lack of transparency damages trust. It is fair only if based on transparent, non-discriminatory criteria.
**Measures:**
1. **Fair algorithm design and regular bias audits** – test outcomes across demographic groups and locations; remove proxies for protected characteristics.
2. **Transparency** – tell customers that promotions are personalised and the general criteria (e.g. loyalty points), and give a standard price available to all.
3. **Consent and data minimisation** – let customers opt out of profiling and use only data needed for promotions, in line with PDPA.
4. **Human oversight and complaint channel** – staff review unusual pricing outcomes and customers can query their offers.

### ✅ Q4. IoT & Employee Monitoring — Is IoT monitoring of employee productivity ethically justified?
**Company's interests:** IoT sensors legitimately improve inventory management, warehouse safety and efficiency; the company has a right to ensure work is done and assets are protected.
**Employees' privacy rights:** using the same sensors to track individuals' movements all day was not the stated purpose (function creep); continuous observation affects observational privacy, creates stress and distrust, and data may be used unfairly in evaluations without context (like Ch1: visiting a website does not show motive).
**Judgement:** monitoring is **justified only if** it is transparent, proportionate and limited to legitimate purposes. Covert, continuous tracking of individuals is **not** justified.
**Conditions/recommendations:** a written monitoring policy communicated to staff; use aggregated data for process improvement rather than individual surveillance; no monitoring during breaks or in private areas; restricted access to data and a retention limit; consult employees and allow them to see and challenge their data.

### ✅ Q5. Cloud Computing & Data Security — Should SmartRetail continue using cloud services? Recommend THREE measures.
**Evaluation:** Yes, it should continue. Cloud computing provides scalability, cost savings, backup and recovery, and supports Big Data analytics. The incident was caused by **stolen login credentials** — an authentication weakness, not a failure of cloud technology itself. Moving back on-premises would be costly and would not solve the credential problem. Under the shared-responsibility model, SmartRetail must strengthen its own controls.
**Three measures:**
1. **Multi-factor authentication and strong identity management** for both customer and staff accounts, with login anomaly detection and account lockout.
2. **Encryption and access control** – encrypt customer data at rest and in transit, manage encryption keys securely, and apply least-privilege, role-based access.
3. **Vendor due diligence and monitoring** – choose a provider certified to ISO 27001, set security obligations in the contract (SLA, breach notification), enable logging and continuous monitoring, and run regular risk assessments and backups.

---

## Closing the loop

Mak Cik Siti 的一天说明：IT 让医疗更近、更快、更便宜，但每一项便利都把她变成资料——EHR、云端、cookies、脸部 template、社交媒体 profile。谁能看、谁拥有、准不准确（accuracy, property, access），以及没有手机的邻居会不会被落下（social equity），都是伦理问题。

下一章回到组织：面对这些资料与风险，公司怎样**系统性地评估风险**并建立 **trustworthy computing**——CIA triad、risk assessment、security policies、ISO 27001、NIST CSF。

---

## ⚠️ Where the slides mislead

| Slide | Slide 说 | 更准确的理解 |
|---|---|---|
| p15 | Telemedicine: patient get medication without appointment… "**for emergency care**" | 与 p12/p14 矛盾；telemedicine **不适用于紧急情况** |
| p5 | ICT = integration of telephone and computer networking "in a single cabling device" | 这是狭义定义；一般 ICT 泛指所有资讯与通讯技术 |
| p54 | Deep web "allow communication in a secure and anonymous manner" | Deep web 主要只是**未被索引**的内容（如登入后的页面）；**匿名**是 dark web 的特点 |
| p62, p68 | 小标题写 "9.1 / 9.2"、"10.1"，但章节编号是 10、11 | 编号沿用旧版，不影响内容 |
| p86–87 | "14. Major laws" 与 "14. Green IT" | 大纲里 Green IT 是第 15 项 |

---

## Term table

| English | 中文 | 一句话说明 |
|---|---|---|
| ICT | 资讯通讯科技 | 电话与电脑网络的整合 |
| Telemedicine / Telehealth | 远程医疗 / 远程健康 | 临床远程服务 / 含非临床活动 |
| Immersive (live) telemedicine | 实时远程医疗 | 医患实时互动 |
| Remote patient monitoring | 远程病人监测 | 设备记录血压血糖 |
| Store and forward | 储存转发 | 把资料传给其他医生 |
| Mobility | 移动化 | 智能设备与 app 改变医疗 |
| Social equity | 社会公平 | 所有人公平受惠于科技 |
| Digital divide | 数位落差 | 有无网络设备的差距 |
| Digital literacy | 数位素养 | 使用数位工具的能力 |
| Inclusive design | 包容性设计 | 让所有人都能用 |
| EHR / EMR | 电子健康纪录 / 电子病历 | 跨机构 / 单一机构 |
| Interoperability | 互通性 | 系统间交换资料 |
| Big data analytics | 大数据分析 | 从大量资料预测 |
| SaaS / PaaS / IaaS / DBaaS | 软件/平台/基础设施/资料库即服务 | Cloud delivery models |
| Public / private / community / hybrid cloud | 公有/私有/社群/混合云 | Deployment models |
| Elasticity | 弹性 | 动态增减资源 |
| Accuracy / Property / Access | 准确 / 所有权 / 存取 | 三大隐私问题 |
| Digital footprint | 数位足迹 | 网上活动的完整记录 |
| Information broker / reseller | 资讯中介 | 收集贩卖个人资料的公司 |
| IP address | IP 地址 | 电脑的唯一号码 |
| Deep web / Dark web | 深网 / 暗网 | 未索引 / 匿名隐藏 |
| Browser cache | 浏览器快取 | 暂存网页内容 |
| First-party / third-party (tracking) cookie | 第一方 / 第三方（追踪）cookie | 站内 / 跨站追踪 |
| Privacy mode (Incognito) | 隐私模式 | 不储存浏览记录 |
| Online identity | 网络身份 | 自愿发布的个人资讯 |
| Facial recognition | 脸部辨识 | Capture→processing→modelling→comparison→decision |
| Verification (1:1) / Identification (1:N) | 验证 / 辨识 | 与证件比 / 与资料库比 |
| Informational / observational privacy | 资讯隐私 / 观察隐私 | 持有资料 / 被观察 |
| Biometric identification | 生物特征识别 | 测量身体特征 |
| Arches / loops / whorls; core / delta | 弓 / 箕 / 斗；核心 / 三角点 | 指纹图案 |
| Technology-facilitated abuse | 科技助长的虐待 | 网上骚扰 |
| FERPA | 家庭教育权利与隐私法 | 保护教育记录 |
| Green IT / e-waste | 绿色 IT / 电子垃圾 | 可持续的 IT |

---

## Cheat sheet

- **ICT 重要性**：critical thinking、creativity、储存检索、networking、经济、就业、学术交流、e-learning、globalisation、政府记录。
- **Telemedicine**：目的 = diagnosis / medical choices / prescription；3 类 = **immersive, remote monitoring, store & forward**；**只适合非紧急**；mental health 最有益；好处 = comfort, infection control, better assessment, family connections。
- **Telehealth** = telemedicine + 非临床（训练、预约、教育）。
- **Mobility**：cloud 24/7 看结果；IoT；好处 = efficiency, patient experience, coordination, cost saving。
- **Social equity**：挑战 = digital divide, digital literacy, accessibility；解法 = **inclusive design, affordable access, fair algorithms, digital education**。
- **EHR**：纸本病历电子化；好处 = quality of care, interoperability, productivity。
- **Big data + cloud**：预测疫情；cloud 特性 = elastic, metered, convenient；医疗影响 = cost-cutting, interoperability, analytics, patient data possession, telemedicine。
- **Privacy 三问题**：accuracy, property, access；digital footprint；information brokers。
- **Cookies**：first-party（站内）vs third-party（tracking）；对策 = 删除、封锁、privacy mode。
- **Deep web**（未索引）vs **dark web**（Tor、匿名、犯罪）。
- **Facial recognition**：capture → processing → modelling → comparison → decision；verification 1:1；5 限制；informational + observational privacy。
- **Biometrics**：unique, unchanging, digitisable, database-ready；指纹 arches/loops/whorls、core/delta。
- **Social media ID verification**：减少骚扰/假新闻 vs **data security** 风险。
- **Privacy laws**：GLBA（财务）、HIPAA（医疗）、FERPA（教育）。
- **Green IT**：manufacturing → operation → disposal；importance（sustainability, e-waste, energy）；challenges（cost, obsolescence, awareness, e-waste mgmt）。

---

## Practice (answers included)

### A. MCQ

1. A nurse uses a handheld device to record a patient's blood pressure from home. This is:
   (a) Immersive telemedicine (b) Remote patient monitoring (c) Store and forward (d) Telehealth training
2. Which is NOT suitable for telemedicine?
   (a) Follow-up appointment (b) Skin rash consultation (c) Suspected broken leg (d) Psychotherapy
3. A cookie created by an advertising network to follow you across websites is a:
   (a) first-party cookie (b) session cookie (c) third-party (tracking) cookie (d) cache file
4. Comparing a traveller's live face with the photo in her passport is:
   (a) one-to-many identification (b) one-to-one verification (c) store and forward (d) modelling
5. "Who owns the data" refers to which privacy issue?
   (a) Accuracy (b) Property (c) Access (d) Security

**Answers:** 1-b, 2-c, 3-c, 4-b, 5-b

### B. Short answer

**B1. Differentiate telemedicine and telehealth. (4 marks)** Telemedicine is the delivery of clinical services (diagnosis, treatment, prescriptions) remotely through technology. Telehealth is broader: it includes telemedicine plus non-clinical activities such as appointment preparation, continuing medical education and specialist training.

**B2. State FOUR limitations of facial recognition. (4 marks)** Lower accuracy than fingerprint or DNA; unknown frequency of facial features in the population; image quality affects comparison; size of the stored database affects accuracy; faces change over time causing false matches (any four).

**B3. Explain the three primary privacy issues. (6 marks)** Accuracy — those who collect data are responsible for ensuring it is correct. Property — concerns who owns the data. Access — those who hold data are responsible for controlling who can use it.

### C. Application

*Case: MyKlinik, a telehealth app for rural Malaysians.*

**C1.** Identify TWO social equity challenges MyKlinik faces and propose a solution for each. (6 marks)
Digital divide — many rural users lack reliable Internet or smartphones → partner with telcos for subsidised data plans or provide kiosks at community centres (affordable access). Digital literacy — elderly users struggle with apps → simple interface with large buttons, Malay/Chinese/Tamil voice guidance, and community training sessions (inclusive design + digital education).

**C2.** MyKlinik wants to store patient EHRs in a public cloud. Give TWO benefits and TWO ethical risks. (6 marks)
Benefits: lower cost with no server hardware and scalable storage; records accessible anywhere with automatic backup and recovery. Risks: breach or misconfiguration exposing sensitive health data; sharing/secondary use of data without patient consent (e.g. with insurers).

**C3.** MyKlinik proposes fingerprint login. Discuss one advantage and one ethical concern. (4 marks)
Advantage: fingerprints are unique and unchanging, so login is secure and convenient for users who forget passwords. Concern: if the biometric template database is breached, fingerprints cannot be changed like passwords, so the harm is permanent; patients must consent and templates must be encrypted.

### D. Thinking

**D1.** "Technology improves quality of life for everyone." Discuss. (6 marks)
Technology improves quality of life for many — telemedicine, EHRs and big data improve healthcare access and outcomes. But not for everyone equally: the digital divide, low digital literacy and inaccessible design exclude rural, elderly, poor and disabled people, and the same technologies create privacy risks (digital footprints, tracking cookies, facial recognition). Benefits are widespread only when ethical computing principles — inclusive design, affordable access, fair algorithms and digital education — are applied.

**D2.** Should social media require real-name verification? (4 marks)
See Tutorial 5 Q8 answer: a balanced view — it may reduce abuse and fake news, but concentrates sensitive ID data in platforms and removes protective anonymity; a better approach is optional or third-party verification with strong data protection.

---

## Slide index

| Note section | Slides |
|---|---|
| 1 ICT | p4–8 |
| 2 Telemedicine & telehealth | p9–17 |
| 3 Mobility | p18–21 |
| 4 Social equity | p22–30 |
| 5 EHR | p31–35 |
| 6 Big data & cloud | p36–46 |
| 7 Privacy | p47–52 |
| 8 Internet & web, cookies | p53–59 |
| 9 Online identity | p60–61 |
| 10 Facial recognition | p62–70 |
| 11 Fingerprint | p71–74 |
| 12 Smartphone apps | p75–76 |
| 13 Social media | p77–85 |
| 14 Privacy laws | p86 |
| 15 Green IT | p87–89 |
| SmartRetail answers | *Extra question for Tutorial 5.pdf* |

## Links to other chapters
- HIPAA, GLBA, GDPR (biometric data), PDPA, workplace monitoring, consumer profiling → **Ch3**
- Green procurement → **Ch4**
- CIA triad & cloud security measures, ISO 27001 → **Ch6**
- Botnets spreading fake news → **Ch2**
