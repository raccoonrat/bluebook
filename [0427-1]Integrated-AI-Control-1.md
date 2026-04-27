

下面给你一版 **“Controls-core 子模块如何逐段嵌入 Chapter 3 / 4 / 5 / 6 / 8 的可直接替换稿”**。我按**最少侵入、最大增益**来设计：不改变你当前 bluebook 的总对象与章节主句，只把 Control-design.pdf 里的高价值工程逻辑提升为 **Decoupled Safety Kernel 的工程实例、双时间尺度控制的实现锚点、Mechanism Evidence Pack 的来源、以及 bounded deployment contract 的工程前提**。当前蓝皮书已经把 Chapter 3 定义为 join 拓扑与 kernel 接口、Chapter 4 定义为 computable envelope 与 Fail-Safe / ORA、Chapter 5 定义为 evidence engine、Chapter 6 定义为 guarantee grammar 与 bounded deployment contract、Chapter 8 定义为 theory–system–benchmark–release operating program。 [[Control-design | PDF]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/Documents/Microsoft%20Copilot%20%E8%81%8A%E5%A4%A9%E6%96%87%E4%BB%B6/Control-design.pdf)

> **使用方式**
> 
> * 每一段都标明了“建议放置位置”。
> * 我给的是**可直接替换/增补的正文段落**，你可以原文插入或轻微改字。
> * 我尽量避免把子模块写成“产品说明书”，而是把它提升为 bluebook 的工程锚点。

* * *

1）Chapter 3：作为 “DECOUPLED SAFETY KERNEL 的工程实例化” 融入

建议放置位置

放在 **3.6 DECOUPLED SAFETY KERNEL 最小接口** 之后，新加一个小节：

### **3.7 Controls-core：DECOUPLED SAFETY KERNEL 的工程实例化（建议新增）**

可直接替换稿

当前 controls-core 子模块表明，DECOUPLED SAFETY KERNEL 可以首先以一个最小工程实例出现，而不必一开始就展开为完整控制平面。其核心抽象已被收敛为 **types & interfaces、detection、scoring、prevention** 四类对象，并通过 **controls-core-lite / controls-core-advanced / controls-core-full** 三档构建，以及 **controls-local-service / controls-cloud-service** 两类部署承载，形成能力包与 deployment regime 的基础分层。换言之，kernel 的工程实例并不以“拥有多少 detector”为定义，而以“是否存在稳定的事件接口、可分层的执行 tier、可切换的能力包和可治理的服务承载方式”为定义。 [[Control-design | PDF]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/Documents/Microsoft%20Copilot%20%E8%81%8A%E5%A4%A9%E6%96%87%E4%BB%B6/Control-design.pdf)

从本书对象语言看，这一设计正对应于 kernel 的最小工程化：types & interfaces 对应 event contract 与 detector contract，detection / scoring / prevention 对应 decision synthesis 与受控动作生成，lite / advanced / full 对应能力包分层，local / cloud service 则对应 deployment regime 的具体承载形式。更重要的是，该子模块已明确支持 **registry 自动发现、配置启停、API / worker 承接、DB / object storage / logging / post-analysis** 等机制，这说明 kernel 并不是一组静态规则，而是一个可以被版本化、被部署裁剪、被证据化并被后续 governance 引用的运行时 artifact。由此，controls-core 不是整个控制面的替代物，但它提供了一个可以直接映射到本章 kernel 接口、双平面切分与 Chapter 6 bounded deployment contract 的工程实例。 [[Control-design | PDF]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/Documents/Microsoft%20Copilot%20%E8%81%8A%E5%A4%A9%E6%96%87%E4%BB%B6/Control-design.pdf)

* * *

2）Chapter 4：作为 “可计算外壳 / 热路径级联 / 冷路径 ORA” 的实证锚点融入

建议放置位置 A

放在 **4.1 可计算外壳：从开放语义到有限协议对象** 之后，作为一个加强段。可直接替换稿（用于 4.1 后补段）

controls-core 的执行策略说明，所谓“可计算外壳”并非抽象隐喻，而可以被实现为一个**时延受限的多级执行级联**：detectors 按估计 latency 被划分为优先级 tiers，同一 tier 内并行执行，每一 tier 结束后计算 cumulative score，并在启用 fail-fast 时于阈值越界后直接跳过剩余 tiers。其最轻量路径从 **language detection、exact matches / compiled regex、structural heuristics** 开始，逐步升级至 **classic ML / ONNX、小型 advanced heuristics、relatively-fast SLM & LLM**，而 **LLM-as-Judge** 仅在 borderline 区间触发。这样的设计将开放语义空间压缩为有限的 event 流、有限的条件升级路径与有限的动作触发条件，使热路径上的 policy 求值第一次具备了明确的预算边界与协议边界。 [[Control-design | PDF]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/Documents/Microsoft%20Copilot%20%E8%81%8A%E5%A4%A9%E6%96%87%E4%BB%B6/Control-design.pdf)建议放置位置 B

放在 **4.8 带外受控适应闭环（ORA）** 末尾，作为“工程对应物”说明。可直接替换稿（用于 4.8 后补段）

同一子模块设计也进一步说明，热路径与冷路径的切分不是抽象方法论，而是实现约束。controls-core 已将 prompt、response 与 dataset 明确列为可接受输入对象，并把 logging、post-analysis、ML training pipeline、dataset scanning、streaming detector 设计、strict evaluation & resource testing protocol、update mechanism 等问题保留在热路径之外的冷路径空间。这意味着系统已经自然分裂为两种时间尺度：一类是毫秒至秒级的在线级联与 fail-fast 决策，另一类是围绕日志、数据集、策略更新、资源测试与部署配置的离线适应与验证。因此，ORA 在本书中不是附加优化，而是对 controls-core 这类 runtime kernel 所必需的冷路径治理补完。 [[Control-design | PDF]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/Documents/Microsoft%20Copilot%20%E8%81%8A%E5%A4%A9%E6%96%87%E4%BB%B6/Control-design.pdf)建议放置位置 C

如你希望进一步强化 **4.9 双时间尺度风险自适应控制**，可在末尾增加一个收口句。可直接替换稿（用于 4.9 末句补强）

controls-core 的当前设计已经从工程侧验证了这一双时间尺度结构：快时间尺度对应 tiered detector cascade、conditional escalation 与 fail-fast；慢时间尺度则对应 logging、post-analysis、dataset scanning、config refresh、evaluation protocol 与 update mechanism。换言之，本书的方法论心脏并非事后总结，而是在子模块设计中已表现为可部署的时间尺度分离。 [[Control-design | PDF]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/Documents/Microsoft%20Copilot%20%E8%81%8A%E5%A4%A9%E6%96%87%E4%BB%B6/Control-design.pdf)

* * *

3）Chapter 5：作为 Mechanism Evidence Pack 与 decision-signal compiler 的具体来源融入

建议放置位置 A

放在 **5.2 从测量到控制选择：评测输出作为决策信号（decision signals）** 末尾。可直接替换稿

controls-core 的当前设计表明，decision signals 并不必然来源于统一总分，而可以来源于运行时级联内部的结构化中间量：例如 language detection 的过滤结果、各 execution tier 的命中与跳过情况、cumulative score 的演化、fail-fast 的触发位置，以及 advanced pipeline 与 LLM-as-Judge 的条件调用区间。只要这些信号能够被稳定写入 evidence graph，并被回指到 policy_version、deployment regime 与具体输入对象类型（prompt / response / dataset），它们就不仅是内部调试数据，而是可以被编译为 control selection 与 release governance 输入的 decision signals。 [[Control-design | PDF]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/Documents/Microsoft%20Copilot%20%E8%81%8A%E5%A4%A9%E6%96%87%E4%BB%B6/Control-design.pdf)建议放置位置 B

放在 **5.9 Minimum Mechanism Evidence Pack** 的正文中，作为机制证据的具体化说明。可直接替换稿

对于以 controls-core 为代表的 tiered runtime kernel，Mechanism Evidence Pack 可以进一步细化为运行时级联证据：三平面字段的缺失率与 join 成功率、不同 execution tier 的命中率与跳过率、fail-fast 触发频率、advanced pipeline 与 LLM-as-Judge 的条件调用比例、以及 local / cloud deployment regime 下 detector capability 的裁剪差异。这些条目本身并不直接构成 release 结论，但它们决定系统是否真的按所声明的 kernel contract、latency budget 与 capability package 被执行，因此是 Track B 机制证明与 Track A release defense 之间不可缺少的底座。 [[Control-design | PDF]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/Documents/Microsoft%20Copilot%20%E8%81%8A%E5%A4%A9%E6%96%87%E4%BB%B6/Control-design.pdf)建议放置位置 C

放在 **5.10 Minimum Release Evidence Pack** 的 (A)/(C)/(D) 之后，作为 release 证据与配置治理的桥接句。可直接替换稿

若运行时内核采用 controls-core 这类分 tier 执行模式，则 Release Evidence Pack 还应至少记录其部署变体与能力包裁剪结果，例如 lite / advanced / full 的实际启用形态、local / cloud service 所承接的 detector family、以及 config 是否通过受控方式启停 detector。其意义不在于暴露实现细节，而在于保证 release statement 所引用的能力边界与运行时真实能力集一致；否则，组织只能对一个“想象中的安全配置”做出保证，而不能对线上真实配置做出保证。 [[Control-design | PDF]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/Documents/Microsoft%20Copilot%20%E8%81%8A%E5%A4%A9%E6%96%87%E4%BB%B6/Control-design.pdf)

* * *

4）Chapter 6：把 open questions 升格为 bounded deployment contract 的正式条款来源

建议放置位置 A

放在 **6.1.5 Preconditions（前提条件）** 末尾，作为工程前提补充。可直接替换稿

从当前 controls-core 子模块的开放问题可以看出，deployment preconditions 不应停留在抽象层：例如 cloud deployment 的 gateway 选择、rate limiting 与 shared storage 语义，local deployment 中的防盗、防绕过、防篡改能力，logs 是否上送 cloud DB、local DB 是否存在、config 是否需要外部签名，以及 dataset scanning 的规模、push/pull 与 async API 形态，都会直接影响 bounded guarantees 能否被成立。换言之，这些并非实现团队的后续优化项，而是 contract 是否允许系统对外陈述可部署保证的前提条件。 [[Control-design | PDF]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/Documents/Microsoft%20Copilot%20%E8%81%8A%E5%A4%A9%E6%96%87%E4%BB%B6/Control-design.pdf)建议放置位置 B

放在 **6.2 Bounded deployment contract** 的 “证据义务 / 变更控制” 之间。可直接替换稿

对于以 controls-core 为内核的部署，bounded deployment contract 还应覆盖一组工程条款：配置对象是否存在且是否签名、日志是否本地轮转或上送云端、local / cloud 两类部署是否共用或分离 DB、共享存储与模型资源如何被保护、以及 update mechanism 如何确保 detector family、策略包与配置变体的版本单调。若这些条款未被写入 contract，则系统即使拥有 detection / scoring / prevention，也无法对配置完整性、日志完整性与更新完整性做出可辩护陈述。 [[Control-design | PDF]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/Documents/Microsoft%20Copilot%20%E8%81%8A%E5%A4%A9%E6%96%87%E4%BB%B6/Control-design.pdf)建议放置位置 C

放在 **6.4 企业运作模型** 末尾，作为 cadence 的工程化来源。可直接替换稿

同样，controls-core 当前提出的 strict evaluation and resource testing protocol、dataset scanning semantics、streaming output detector 策略、以及 local/cloud 安全基线问题，也说明 operating cadence 不能只围绕 benchmark 与 release 展开，还必须围绕 kernel 的资源画像、deployment profile 与 update discipline 展开。换言之，企业运作模型不仅要审阅 evidence pack，更要审阅支撑这些 evidence 的 runtime kernel 是否仍在其声明的资源、时延、配置与更新边界内运行。 [[Control-design | PDF]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/Documents/Microsoft%20Copilot%20%E8%81%8A%E5%A4%A9%E6%96%87%E4%BB%B6/Control-design.pdf)

* * *

5）Chapter 8：把 controls-core 写成 F2 / F3 / F4 的桥接 artifact

建议放置位置 A

放在 **8.3 四个一级因子：职责与核心 artifact** 中，补强 F2。可直接替换稿（接在 F2 后）

在工程上，controls-core 子模块可被视为 F2（System & Kernel）的一个典型 artifact 家族：其以 types & interfaces、detection、scoring、prevention 为核心对象，以 lite / advanced / full 为能力包裁剪，以 local / cloud service 为 deployment regime 承载，并通过 registry、config enable/disable、API / worker pipeline 与 DB / object storage 形成最小可部署控制内核。这说明 F2 的核心产物不应被理解为“若干 detector 代码”，而应被理解为可被发布、可被裁剪、可被回放、可被 contract 引用的 kernel artifact。 [[Control-design | PDF]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/Documents/Microsoft%20Copilot%20%E8%81%8A%E5%A4%A9%E6%96%87%E4%BB%B6/Control-design.pdf)建议放置位置 B

同样放在 **8.3** 中，补强 F3。可直接替换稿（接在 F3 后）

对于 F3（Benchmark & Evidence），controls-core 同样提供了一个现实锚点：其已将 prompt、response 与 dataset 定义为一等输入对象，并将 dataset scanning、post-analysis、evaluation protocol、resource testing 与 logging 明确为系统演化问题。这意味着 benchmark 与 evidence 并非只消费 runtime 输出结果，它们同时消费 kernel 的输入宇宙、资源边界与冷路径评估协议。由此，F3 的 artifact 也应包含 kernel-aware 的 suite 设计，而不只是通用 benchmark 列表。 [[Control-design | PDF]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/Documents/Microsoft%20Copilot%20%E8%81%8A%E5%A4%A9%E6%96%87%E4%BB%B6/Control-design.pdf)建议放置位置 C

继续放在 **8.3** 中，补强 F4。可直接替换稿（接在 F4 后）

对于 F4（Release & Operations），controls-core 的 logging、PostgreSQL / post-analysis、config management 与 update mechanism 设计表明，release 与 operations 的核心并不是简单“接受评测结果”，而是监督 runtime kernel 的配置、日志、资源与更新链是否仍满足 bounded deployment contract。换言之，F4 的 release checklist 应同时面向 evidence packs 与 kernel state，而不只是面向外显模型输出。 [[Control-design | PDF]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/Documents/Microsoft%20Copilot%20%E8%81%8A%E5%A4%A9%E6%96%87%E4%BB%B6/Control-design.pdf)建议放置位置 D

放在 **8.5 如何协同运行：避免割裂团队并行推进** 末尾。可直接替换稿

controls-core 的设计还说明，统一 operating program 的必要性并非抽象组织要求，而是工程事实：detector family 的扩展、config 启停、deployment regime 的裁剪、dataset scanning、resource testing 与 update mechanism 已经跨越了 F2 / F3 / F4 三个因子。若这些对象由割裂团队分别定义各自语言与节律，系统就会失去同一 policy_version、同一 evidence chain 与同一 release gate，从而再次退化为局部优化的模块集合。 [[Control-design | PDF]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/Documents/Microsoft%20Copilot%20%E8%81%8A%E5%A4%A9%E6%96%87%E4%BB%B6/Control-design.pdf)

* * *

六、你可以直接采用的最简方案（如果你不想改太多）

如果你想**最少改动**，我建议只插入 5 段，效果已经很强：

1. **Chapter 3.7（新增 1 小节）**：controls-core 作为 kernel 实例
2. **Chapter 4.1 后补 1 段**：tiered cascade = computable envelope
3. **Chapter 5.9 后补 1 段**：Mechanism Evidence Pack 的级联证据
4. **Chapter 6.2 后补 1 段**：config / logs / update mechanism = contract 条款
5. **Chapter 8.3 后补 1 段**：controls-core = F2/F3/F4 的桥接 artifact

这样你就能把这份子模块设计真正“升格”进 bluebook，而不会把正文拉成实现说明书。

* * *

最后一句判断

> **Controls-core 子模块最值得融入 bluebook 的，不是它有哪些 detector，而是它已经把 bluebook 的核心科学对象——可计算外壳、tiered runtime cascade、deployment regime layering、artifact-based extensibility、evidence-governed updates——做成了工程雏形。****正确的融入方式，是把它写成 Decoupled Safety Kernel 的工程实例化，并把其中的 open questions 正式编译进 bounded deployment contract 与 operating program，而不是把它作为“一个安全组件设计”附会进正文。** [[Control-design | PDF]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/Documents/Microsoft%20Copilot%20%E8%81%8A%E5%A4%A9%E6%96%87%E4%BB%B6/Control-design.pdf)

如果你愿意，我下一步可以直接继续给你：**“把 Controls-core 融入后的 Chapter 3/4/5/6/8 整体连贯版（不是分段，而是已经串起来
