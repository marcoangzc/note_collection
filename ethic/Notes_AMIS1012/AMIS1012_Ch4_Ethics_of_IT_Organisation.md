# AMIS1012 — Chapter 4: Ethics of IT Organisation

> 这章 slide 的内容大多是"规则清单"：organisational ethics 的一堆 do's and don'ts、US 法律表、whistle-blowing 八个步骤、ICT code of conduct 七条、green procurement 五个原则。问题有三个：① "Organisation ethics" 和 "Organisational ethics" 两个词只差一个字母，slide 没有解释为什么要分；② whistle-blowing 的条件和 non-whistle-blowing 的例子放在不同页，没有一个**判断规则**，而 Tutorial 4 Q5 就是要你判断；③ p23 的"坏掉的 forklift"例子，slide 的 "Why it's not whistle-blowing" 直接复制了上一个例子的理由，不对。这份笔记补上判断规则、把八个步骤画成流程，并完整回答 Tutorial 4 和 TechNova 情境题。

---

## 0. 一句话总览

**一个 IT 组织对外要讲 organisation ethics（怎样对待社会、客户），对内要有 organisational ethics（员工怎样行事）；法律（US federal acts、Malaysia Whistleblowers Protection Act 2010）划出底线；当内部出现严重的违法或危险行为时，员工可以 whistle-blow，公司则要以 corporate accountability 负责，并用 ICT code of conduct 和 green procurement policy 让日常运作合乎道德。**

| Part | 内容 | Slide |
|---|---|---|
| 4.1 | Organisation ethics vs organisational ethics；组织内的道德规范 | p4–7 |
| 4.2 | Rules & law of IT organisation：US federal acts、Australia/NZ、China、Malaysia WPA 2010 | p8–13 |
| 4.3 | **Whistle-blowing**（条件、八步流程、non-whistle-blowing）与 **corporate accountability** | p14–26 |
| 4.4 | Employer's responsibilities to whistle-blowing | p27–29 |
| 4.5 | **ICT code of conduct** | p30–34 |
| 4.6 | Green procurement policy | p35–37 |

🎯 **这章回答的考题**
- 五份 past year **没有直接考 Ch4**。但 **Tutorial 4** 与 **TechNova 情境题**（4+6+4+6 = 20 分，已经是考卷格式）完全以这章为主；May 2026 Q1a "ethical issues faced by IT users" 也可引用本章的 confidentiality 与 code of conduct。→ 重点准备 whistle-blowing 判断、corporate accountability、ICT code of conduct。

**Prerequisites**：Ch1 的 ethics vs law、code of ethics；Ch2 的 malicious insider；Ch3 的 PDPA。

---

## Scene：Amir 的两难（TechNova 情境）

TechNova Sdn. Bhd. 为金融机构开发银行 app，储存客户个人资料、财务记录和系统设计。

Amir（IT 部门）发现：
- 同事**未经批准**把几千笔客户资料下载到**私人 USB**，说是"backup"；
- 公司已经**两年没有**做 cybersecurity awareness training；
- 好几个员工**共用密码**以便"快一点"登入。

Amir 报告给经理，经理说："不要**制造不必要的问题**，我们正准备一个重要的合作案。"Amir 现在考虑**向外部举报**。

他该不该？公司应该怎么做？这一章就是为了回答这两个问题。

---

## 4.1 Organisation Ethics vs Organisational Ethics（p4–7）

两个名词只差一个 "-al"，但方向相反：

| | **Organisation ethics** | **Organisational ethics** |
|---|---|---|
| 方向 | **向外** | **向内** |
| 定义 | 组织怎样**回应外部世界**（社会、客户、环境、政府） | 一套**决定员工在职场怎样行事**的规则与标准 |
| TechNova 例子 | 如实告诉合作银行曾发生资料外泄 | 员工不可把客户资料拷到私人 USB、不可共用密码 |

> 💬 **答题句 (EN)** — *Organisation ethics refers to how an organisation responds to the external world, such as customers and society, while organisational ethics refers to the set of rules and standards that determine how employees should behave in the workplace.*

**组织内的道德规范（p4–7）**：
- **篡改资料 (tampering with data)** 是不诚实、不专业的。
- **不可向第三方泄露公司机密资讯**，也不要把公司规则或建议透露给任何人；**避免与亲友讨论公务**，机密资讯绝不能外泄。
- 金钱交易与贸易要**完全公正**；**禁止因身份、外貌、年龄、家庭背景歧视员工**。
- 女性员工要受到尊重（不要期待她们工作到很晚）；**员工只应按工作表现被评价**。
- **员工不可被公司利用**：要为加班、付出给予补偿；薪水、津贴、报销要**准时**发放。
- **员工安全优先**：不可让员工处于危险之中。
- **不可对客户作虚假宣传**；广告要如实；不要作出做不到的承诺——要客户忠诚就要对他们诚实。

### ✅ 满分答法 — Tutorial 4 Q2: "Employees should not divulge any of the company's confidential information to third parties." Why? Discuss the logic.
1. **Protects competitive advantage** – confidential information such as source code, product plans, pricing and R&D gives the company its edge; if leaked to competitors, the company loses market position and revenue.
2. **Protects customers and legal compliance** – the company holds customers' personal and financial data; disclosing it breaches privacy laws such as the PDPA 2010, leading to fines, lawsuits and loss of trust.
3. **Maintains trust and security** – leaks can expose security designs to attackers and damage relationships with partners and clients who shared information in confidence.
4. **Duty of loyalty/employment contract** – employees accept confidentiality as part of their employment (often through NDAs); breaking it is dishonest and unprofessional.

---

## 4.2 Rules and Law of IT Organisation（p8–13）

**US 的特色（p8–9）**：电子保护与隐私法律体系**可能是全世界最大、最全面、最有效的**；但隐私机制较依赖**临时的政府合规要求和私人诉讼**；cyber security policy 由**行政部门指引 + 立法**组成。

**US federal acts（p10–11）**：

| Act | 重点 |
|---|---|
| **HIPAA 1996** | 这三条法律要求**医疗机构、金融机构、政府部门**保护其流程与资料——但**不是万无一失**，只要求 **"fair"（合理）程度的安全** |
| **Gramm–Leach–Bliley Act 1999** | （同上组） |
| **FISMA**（Federal Information Security Management Act，含于 2002 年 Homeland Security Act） | （同上组） |
| **Cybersecurity Information Sharing Act (CISA)** | 通过**加强网络安全风险的资讯交换**来强化美国网络安全；政府与科技/制造公司**分享 Internet traffic 资讯** |
| **Cybersecurity Improvement Act 2014** | **自愿**的公私合作：加强网络安全研究与发展、人才培养与教育、公众认知与准备 |
| **Federal Exchange Data Breach Disclosure Act 2015** | 健康保险交易平台发生 security breach 时，必须**尽快、最迟 60 天内**通知个人资料被取得/存取的客户 |

⚠️ *Extra*：2014 年那条法律的正式名称是 **Cybersecurity *Enhancement* Act of 2014**（slide 写 "Improvement"）；CISA (2015) 的资讯分享在法律上是**自愿**的，slide 说 "mandates" 不太准确。考试照 slide 写即可。

**其他国家（p12–13）**：

| Country | Law | 重点 |
|---|---|---|
| Australia & NZ | **Freedom of Information (FOI) laws**（早在 1982 年） | 之后持续修订更新 |
| China | **PIPL**（2021 年 11 月 1 日生效，中国第一部全面的资料保护法） | 处理个人资讯必须遵守 **legality, justice, integrity, minimum necessity, openness and transparency**；目的要明确合理 |
| **Malaysia** | **Whistleblowers Protection Act 2010 (Act 711)** | 鼓励揭发**贪污和其他不当行为**，保护揭发者免受不利后果 |

**Malaysia WPA 2010 的四种保护（p13）**：
1. **Confidentiality**：揭发者身份与所提供资讯保密，**即使在法庭审讯中**也不披露；
2. **Immunity**：揭发者**不承担**因揭发而产生的民事、刑事或纪律责任；
3. **Protection from detrimental action**：保护揭发者免受任何因揭发而引起的不利行为（报复）；
4. **Protection extends to associates**：与揭发者**有关系/关联的人**也受保护。

### ✅ 满分答法 — Tutorial 4 Q3: Describe THREE acts related to IT organisation controlled by the US federal government.
1. **HIPAA (1996)** – requires healthcare organisations to safeguard patients' health information; it requires a "fair" (reasonable) level of security rather than being foolproof.
2. **Cybersecurity Information Sharing Act (CISA)** – strengthens US cyber security by improving the sharing of information about cyber threats, including Internet traffic information, between the government and technology and manufacturing companies.
3. **Federal Exchange Data Breach Disclosure Act (2015)** – requires a health insurance exchange to notify any customer whose personal information was obtained or accessed in a security breach as soon as possible, and no later than 60 days after discovering the breach.

（也可写 GLBA 1999、FISMA、Cybersecurity Improvement Act 2014。）

---

## 4.3 Whistle-blowing & Corporate Accountability（p14–26）⭐

### 4.3.1 定义与条件

- **Whistle-blowing** = 员工**揭露有关贪污/不当行为的事实**；又称 "making a disclosure" 或 "blowing the whistle"。通常是在**工作中看到**的。
- 要受 whistle-blowing 法律保护，员工必须**合理地相信两件事**：
  1. 他是为了**群体（公众）的最佳利益**而行动；
  2. 揭露的资讯显示**实际、现在或很可能发生的不当行为**，属于以下一类或多类：
     - **Illegal violations**（如 fraud 等财务不当）
     - **Failure to comply with a lawful duty**（未履行法定责任）
     - **Miscarriages of justice**（司法不公）
     - **Endangering someone's health and welfare**（危害健康与安全）
     - **Environmental harm**（环境破坏）
     - **Covering up misconduct** in any of the above（掩盖上述行为）
- UK：Employment (Rights) Act 1996 中的 whistle-blowing 条文（由 **Public Interest Disclosure Act 1998** 修订）。
- 若因揭发而**在工作中受虐待或失去工作**，可向 **employment tribunal** 投诉。

### 4.3.2 判断规则：Whistle-blowing 还是 non-whistle-blowing？

> **问两个问题：① 这件事是不是违法、不道德或危险，影响的是公众/他人？② 它是不是只是个人的、可以用一般公司流程解决的日常问题？**
> ① 是 → **whistle-blowing**；② 是 → **non-whistle-blowing**。

**Non-whistle-blowing（p21–24）**：在公司内报告或处理**不涉及违法、不道德或危险活动**的问题——例行投诉、意见分歧、改善建议，可通过**标准公司程序**解决。

| Slide 例子 | 为什么不是 whistle-blowing |
|---|---|
| **Emma** 申请把班次从晚上改到早上 | 个人对工作条件的请求 |
| **Carlos** 报告 forklift 坏了，维修部修好 | **例行的设备维修与安全程序**（不是揭发公司故意的违法/不道德行为） |
| **Lena** 发现加班费算错，找 payroll 更正 | 影响她个人的薪资错误，内部可解决 |

⚠️ p23 Carlos 的 "Why It's Not Whistle-Blowing" 被复制成 "a personal request related to work conditions"——不对，他不是在提个人请求。更准确：*it is a routine safety/maintenance report handled through standard procedures; there is no illegal or unethical conduct being exposed.*（如果公司**明知** forklift 危险还**故意**叫员工继续用，那就变成 endangering health and safety → whistle-blowing。）

### ✅ 满分答法 — Tutorial 4 Q4 & Q5

**Q4. What is whistle-blowing?** Whistle-blowing is when an employee discloses information about corruption, illegal activity or other serious misconduct in the organisation (e.g. fraud, danger to health and safety, environmental harm) to people who can act on it, believing it is in the public interest.

**Q5. Determine whistle-blowing or non-whistle-blowing:**

| Scenario | Answer | Reason |
|---|---|---|
| Patrick reports **financial fraud** committed by a manager | **Whistle-blowing** | Illegal violation (fraud) |
| Working environment of a factory **endangering employees' lives** | **Whistle-blowing** | Endangering health and welfare |
| Company operation **causes pollution** to the nearby river | **Whistle-blowing** | Environmental harm |
| Leslie's salary is lower than last month; he asks HR | **Non-whistle-blowing** | Personal payroll query resolved through normal procedures (like Lena) |

### 4.3.3 Whistle-blowing 的八个步骤（p17–20）

```
 (1) Determine severity ─> (2) Begin documentation ─> (3) Try to fix internally
                                                              │ (fails)
                                                              v
 (6) Develop action plan <─ (5) Consider implications <─ (4) Take it to next level
     with experienced            of being a whistleblower     inside the company
     resources (lawyer)
        │
        v
 (7) Put the action plan into action ─> (8) Accept the consequences
```
*看图重点：前四步都在**公司内部**；只有内部失败后，才考虑后果、找律师、对外行动。*

| Step | 说明 |
|---|---|
| 1. **Determine the severity** | 要有**明确证据**显示公司/同事不道德，且对**公众利益构成直接危险** |
| 2. **Begin the documentation** | 收集足够证据：事件、事实、自己的观察，**全部记录** |
| 3. **Fix the situation internally** | 以**书面摘要**交给相关主管；重点是揭露真相及对他人的影响 |
| 4. **Take it to the next level inside** | 第一次内部尝试可能失败 → 向更高层反映 |
| 5. **Consider the implications** | 退一步想：愿不愿意继续？可能要付**高额律师费**对抗资源多得多的公司 |
| 6. **Develop an action plan with experienced resources** | 找熟悉 whistle-blowing 诉讼的**律师**，确定适用的法律 |
| 7. **Put the action plan into action** | 与律师一起提告；若想保持匿名，**匿名向媒体披露** |
| 8. **Accept the consequences** | 小心**报复**：被同事抹黑、骚扰、陷害；管理层可能以迟到早退等小事为由**调职、降级、开除**——要有清楚计划和好律师 |

### 4.3.4 Corporate Accountability（p25–26）

**Corporate accountability** = 公司必须为**自己所做的一切**负责——决定、行动，以及对员工、客户、投资者、社会的影响；也必须**诚实、公开**其表现。

| Key principle | 意思 | Slide 例子 | TechNova 可以怎么做 |
|---|---|---|---|
| **Transparency** | 分享正确清楚的资讯 | 公布真实财报，不隐藏亏损 | 如实向合作银行和受影响客户说明风险 |
| **Responsibility** | 接受决定的后果 | 产品有缺陷就召回并道歉 | 承认两年没做培训的失误并改正 |
| **Compliance** | 遵守法律与道德标准 | IT 公司遵守 **PDPA** | 按 PDPA 评估是否需要通报 breach |
| **Stakeholder protection** | 保护员工、客户、投资者、公众 | 工厂提供防护装备 | 立即追回 USB 资料、保护客户 |
| **Board oversight** | 领导层检查管理层做得对不对 | 董事会调查经理是否涉及诈骗 | 董事会调查经理为何压下 Amir 的报告 |

---

## 4.4 Employer's Responsibilities to Whistle-blowing（p27–29）

- 最佳做法：建立**包容、清楚、健康**的工作环境，让员工**安心发声 (speak up)**。
- 法律**不强制**公司设 whistle-blowing program，但设立它**显示雇主愿意聆听**。
- 建立**透明的规则与处理程序**，表示组织**重视**被带到管理层面前的资讯。
- 员工往往是**最先发现诈骗**的人；他们的发现可以**阻止贪污**，保护组织声誉与业绩，甚至**救人**。
- 若组织不透明、不包容，员工会因怕报复而不敢报告。
- **揭发者面对的两大障碍**：① **害怕报复 (fear of retribution)**；② **认为报告了也不会有任何行动**。

---

## 4.5 ICT Code of Conduct（p30–34）

- **Code of conduct** = 一套规则与指引，说明对个人、团体或组织的**期望**。
- **IT code of conduct** = 一般指**电脑与 Internet 等技术应如何被使用**。
- **ICT code of conduct** 说明组织内**合乎道德与负责任的技术使用**；提供 guidelines、policies、security，以及**合乎道德且有效率地使用 IT 基础设施**的规范。

**七个例子（p32–34）**：

| # | Element | 内容 |
|---|---|---|
| 1 | **Fair and honest business** | 以公平诚实的方式经营，符合最佳商业实践 |
| 2 | **Legal compliance** | 遵守营运所在国家的法律法规，包括员工、第三方和环境健康安全标准 |
| 3 | **Data security** | 保护敏感与机密资讯免受未授权存取；用强密码，**不与他人分享登入资料**；**立即报告** security breach 或漏洞 |
| 4 | **Integrity** | 尊重他人与组织；不以数位方式骚扰、霸凌、歧视；尊重他人隐私与机密 |
| 5 | **Usage** | ICT 资源（电脑、Internet、email、软件）**只用于授权目的**；未经授权不存取、修改、披露资料；不以组织资源谋私利或做违法活动 |
| 6 | **Software and licensing** | 只用有 license 的软件，遵守 license 协议；不在公司设备安装未经授权的软件 |
| 7 | **Monitoring and compliance** | 知悉组织可能**监控** ICT 使用以确保遵守守则；用户有责任理解并遵守相关法律与政策 |

### ✅ 满分答法 — Tutorial 4 Q6: Explain TWO examples of ICT code of conduct in an organisation.
1. **Data security** – employees must protect sensitive and confidential information from unauthorised access by using strong passwords, never sharing login credentials, and immediately reporting any security breach or vulnerability. This prevents data leaks and account misuse.
2. **Usage of ICT resources** – computers, Internet, email and software may be used only for authorised work purposes; employees must not access, modify or disclose data without authorisation or use company resources for personal gain or illegal activities. This protects company resources and data.

---

## 4.6 Green Procurement Policy（p35–37）

- **Green procurement** = 选择在**整个生命周期**（生产、用电、维护、弃置）中**减少环境伤害**的 IT 硬件、软件与数位服务；确保组织作出**合乎道德的采购决定**。
- 例：选择**节能笔电**，以及**由再生能源驱动的云端服务**，而不是高耗能的本地 server。

| Key principle | 意思 | 例子 |
|---|---|---|
| **Environmental sustainability** | 选耗电少、碳排放低的 IT 系统 | 用低功耗处理器的笔电取代高耗能桌机 |
| **Life cycle thinking** | 考虑制造、使用、弃置、回收的环境影响 | 旧电脑送去**认证的 e-waste 回收**而不是丢弃 |
| **Ethical supplier selection** | 选择遵守伦理生产的供应商 | 向避免使用 **conflict minerals** 的厂商购买设备 |
| **Energy & resource efficiency** | 减少电力、冷却、运算资源使用 | 用 **virtualization** 减少实体 server 数量 |
| **Regulatory & ethical compliance** | 遵守环境与 IT 治理标准 | 符合 **ISO 14001**（环境管理）与组织 IT 伦理政策 |

→ 与 Ch5 的 **Green IT** 连结：Green IT 是整体做法（设计、使用、管理），green procurement 是其中的**采购**环节。

---

## TechNova 情境题 — 满分答法（20 分）

### ✅ a) Identify TWO ethical issues and explain why they are concerns. (4 marks)
1. **Unauthorised copying of customer data to a personal USB drive** – even if claimed as "backup", it breaches organisational ethics and data security rules; customer financial data outside the company's control can be lost, stolen or misused, violating customers' privacy and the PDPA.
2. **Password sharing among employees (and no security training for two years)** – sharing credentials breaks the ICT code of conduct, removes accountability (actions cannot be traced to a person) and makes unauthorised access easy; lack of training means employees do not understand these risks.
（Also acceptable: the manager suppressing a legitimate security concern for business reasons.）

### ✅ b) Should Amir proceed with whistle-blowing? (6 marks)
**Yes — but only after following the proper process.**
- **The issue qualifies:** unauthorised removal of thousands of customer records and weak security practices indicate a failure to comply with a lawful duty (PDPA security principle) and potential harm to customers; the manager's attempt to hide it is a cover-up. Amir is acting in the public interest.
- **Employee responsibilities:** employees must protect confidential organisational information and report security concerns, as TechNova's own policy requires. Amir has already documented the issue and tried internally, but his manager dismissed it.
- **Before going external:** he should escalate to the next level inside — senior management, the security/compliance officer, the board or the internal whistle-blowing channel — keeping written evidence. Only if this also fails should he consider external channels (e.g. regulators), after weighing the consequences and seeking legal advice.
- **Protection:** under Malaysia's Whistleblowers Protection Act 2010, if he discloses to an enforcement agency in good faith, his identity is kept confidential and he is protected from detrimental action.
- **Why it matters:** protecting customer and system information maintains trust with financial institutions and prevents greater harm; staying silent would make him complicit.

### ✅ c) How should TechNova demonstrate corporate accountability? Provide TWO actions. (4 marks)
1. **Responsibility and stakeholder protection** – immediately investigate the USB incident, recover or securely destroy the copied data, assess whether a data breach occurred, and notify affected parties/regulators as required, instead of hiding it for the business partnership.
2. **Board oversight and transparency** – the board should review why the manager dismissed the report, protect Amir from retaliation, and be transparent with the prospective partner about the risk and corrective actions.

### ✅ d) Explain how ICT policies and monitoring practices can prevent similar incidents. (6 marks)
1. **ICT code of conduct / data security policy** – clearly prohibit password sharing and copying data to personal devices; require unique accounts and strong passwords; make employees sign acknowledgement. This sets expectations and gives grounds for disciplinary action.
2. **Technical controls and monitoring** – block or encrypt USB storage, apply least-privilege access and data loss prevention, and log and monitor data downloads and logins so unusual activity triggers alerts. Employees are told that ICT use is monitored, which deters misuse.
3. **Regular awareness training and a whistle-blowing channel** – annual cyber security training so staff understand the risks, plus a confidential reporting channel that goes beyond direct managers, so concerns reach people who will act.

---

## Closing the loop

Amir 的故事说明：code of conduct 写得再好，如果**员工共用密码、主管压下举报**，就等于没有。有 whistle-blowing 保护、有 corporate accountability，组织才真正"有道德"。

下一章把视角从组织拉到整个社会：IT 如何改变**医疗、隐私、身份、社交媒体与环境**——也就是 **Impact of IT on Quality of Life**。

---

## ⚠️ Where the slides mislead

| Slide | Slide 说 | 更准确的理解 |
|---|---|---|
| p8 & p9 | 两页几乎重复 | 同一内容，p8 多一句"依赖临时合规与私人诉讼" |
| p10 | FISMA "included in the Homeland Security Act of 2002" | FISMA 一般指 **E-Government Act of 2002 的 Title III**（Homeland Security Act 也有一版）；考试照 slide 写即可 |
| p11 | "The 2014 Cybersecurity **Improvement** Act" | 正式名称 **Cybersecurity Enhancement Act of 2014** |
| p11 | CISA "mandates" 分享 | CISA 的资讯分享主要是**自愿**的 |
| p15 | "Employment Protection Act of 1996" | UK 的是 **Employment Rights Act 1996**（由 PIDA 1998 修订） |
| p23 | Carlos 的例子理由写 "personal request related to work conditions" | 复制错误：应是"例行的设备安全/维修报告，没有违法或不道德行为" |

---

## Term table

| English | 中文 | 一句话说明 |
|---|---|---|
| Organisation ethics | 组织（对外）伦理 | 组织如何回应外部世界 |
| Organisational ethics | 组织（内部）伦理 | 员工在职场的行为规范 |
| Confidential information | 机密资讯 | 不可泄露给第三方 |
| HIPAA / GLBA / FISMA | — | 要求 "fair" 程度安全的三条 US 法律 |
| CISA | 网络安全资讯分享法 | 政府与企业分享威胁资讯 |
| Whistleblowers Protection Act 2010 (Act 711) | 马来西亚揭发者保护法 | 保护揭发贪污与不当行为者 |
| Whistle-blowing | 吹哨/揭发 | 揭露组织内的严重不当行为 |
| Non-whistle-blowing | 非揭发情况 | 例行投诉/个人问题 |
| Employment tribunal | 劳资仲裁庭 | 受报复者投诉的地方 |
| Retaliation / retribution | 报复 | 调职、降级、开除、骚扰 |
| Corporate accountability | 企业问责 | 公司为其行为与影响负责 |
| Board oversight | 董事会监督 | 领导层监督管理层 |
| Code of conduct | 行为守则 | 对个人/组织的期望 |
| ICT code of conduct | 资讯通讯科技行为守则 | 合乎道德地使用技术 |
| Green procurement | 绿色采购 | 选择环境伤害较小的 IT 产品 |
| Life cycle thinking | 生命周期思维 | 从生产到弃置考虑影响 |
| Conflict minerals | 冲突矿产 | 来自冲突地区的矿物 |
| ISO 14001 | — | 环境管理体系标准 |

---

## Cheat sheet

- **Organisation ethics** = 对外；**organisational ethics** = 员工行为规则。
- 组织内规范：不篡改资料、**不泄密**、不歧视、尊重女性员工、按表现评价、准时付薪、员工安全、不虚假宣传。
- **US**：HIPAA / GLBA / FISMA（只要求 "fair" security）；CISA（分享威胁资讯）；Cybersecurity Improvement Act 2014（自愿公私合作）；Federal Exchange Data Breach Disclosure Act 2015（**60 天内**通知）。
- **China PIPL**（2021-11-01）；**AU/NZ FOI**（1982）；**Malaysia WPA 2010 (Act 711)**：保密、免责、防报复、保护关联人。
- **Whistle-blowing 条件**：public interest + misconduct（illegal, lawful duty, miscarriage of justice, health & safety, environment, cover-up）。
- **判断**：违法/不道德/危险 → WB；个人例行问题 → non-WB（Emma 改班、Carlos forklift、Lena 薪资）。
- **8 steps**：severity → document → fix internally → next level → implications → action plan (lawyer) → act → accept consequences。
- **Corporate accountability**：transparency, responsibility, compliance, stakeholder protection, board oversight。
- **Employer**：开放文化；两大障碍 = 怕报复 + 认为没用。
- **ICT code of conduct（7）**：fair & honest, legal compliance, data security, integrity, usage, software & licensing, monitoring & compliance。
- **Green procurement（5）**：sustainability, life cycle, ethical supplier, energy efficiency, compliance (ISO 14001)。

---

## Practice (answers included)

### A. MCQ

1. Which scenario is non-whistle-blowing?
   (a) Reporting that the finance director falsified accounts (b) Reporting toxic waste dumped into a river (c) Requesting a change of desk because of glare (d) Reporting that safety alarms are deliberately switched off
2. The first step in the whistle-blowing process is:
   (a) contact the media (b) determine the severity of the situation (c) hire a lawyer (d) accept the consequences
3. Using virtualisation to reduce physical servers applies which green procurement principle?
   (a) Ethical supplier selection (b) Energy & resource efficiency (c) Life cycle thinking (d) Compliance
4. Under the Federal Exchange Data Breach Disclosure Act, customers must be notified no later than:
   (a) 24 hours (b) 7 days (c) 30 days (d) 60 days
5. A company recalls a defective product and apologises. Which corporate accountability principle is this?
   (a) Transparency (b) Responsibility (c) Board oversight (d) Compliance

**Answers:** 1-c, 2-b, 3-b, 4-d, 5-b

### B. Short answer

**B1. Distinguish organisation ethics and organisational ethics. (4 marks)** See §4.1 答题句 + one example each.

**B2. State FOUR protections given by the Whistleblowers Protection Act 2010. (4 marks)** Confidentiality of the whistleblower's identity and information; immunity from civil, criminal or disciplinary action for the disclosure; protection against detrimental action (retaliation); protection extended to persons related to or associated with the whistleblower.

**B3. What are the two main barriers faced by whistleblowers, and how can employers reduce them? (4 marks)** Fear of retribution, and the belief that no action will be taken. Employers can provide confidential/anonymous reporting channels with clear non-retaliation policies, and transparent procedures that investigate reports and give feedback.

### C. Application

*Case: CloudNest Sdn. Bhd., a software house.*

**C1.** A developer discovers that the company knowingly ships a payment app with a security flaw that exposes card numbers, and management refuses to fix it before launch. Is this whistle-blowing? Outline the first FOUR steps she should take. (6 marks)
Yes — it involves failure to comply with a lawful duty (protecting personal data) and harm to customers. Steps: (1) determine severity — confirm the flaw and its danger to customers; (2) document — keep test results, emails and dates; (3) raise it internally in a written summary to her supervisor; (4) if ignored, take it to the next level (CTO, compliance officer, board).

**C2.** Suggest TWO ICT code of conduct rules CloudNest should add after finding pirated software on developers' PCs. (4 marks)
Software and licensing: only licensed software may be used and no unauthorised software may be installed. Monitoring and compliance: the company will audit devices and monitor ICT use to ensure compliance, and staff are responsible for complying.

**C3.** CloudNest needs 50 new laptops. Apply THREE green procurement principles. (6 marks)
Environmental sustainability — choose energy-efficient, low-power models. Life cycle thinking — select a supplier with a take-back programme and send old laptops to certified e-waste recycling. Ethical supplier selection — buy from manufacturers that avoid conflict minerals and follow ethical labour practices.

### D. Thinking

**D1.** Should whistleblowers go straight to the media? (4 marks)
Generally no. Internal channels should be tried first because the organisation may fix the problem quickly, and going public early can harm innocent people and the whistleblower's legal protection (WPA protects disclosures to enforcement agencies). However, if internal channels fail or are part of the cover-up and the public faces serious harm, external disclosure — ideally with legal advice — may be justified.

**D2.** Is having a code of conduct enough to make an organisation ethical? (4 marks)
No. A code only states expectations; it must be supported by leadership example, training, monitoring, enforcement and a safe reporting culture. TechNova had policies, but password sharing and a manager suppressing reports show that without accountability the code is ineffective.

---

## Slide index

| Note section | Slides |
|---|---|
| 4.1 Organisational ethics | p4–7 |
| 4.2 Rules & law | p8–13 |
| 4.3 Whistle-blowing & corporate accountability | p14–26 |
| 4.4 Employer's responsibilities | p27–29 |
| 4.5 ICT code of conduct | p30–34 |
| 4.6 Green procurement | p35–37 |
| TechNova answers | *Scenario Question tutorial 4.pdf* |

## Links to other chapters
- Code of ethics, IT use policy → **Ch1**
- Malicious insider → **Ch2**
- PDPA, HIPAA, GLBA, PIPL → **Ch3**
- Green IT → **Ch5**
- IT security policies (acceptable use, incident response) → **Ch6**
