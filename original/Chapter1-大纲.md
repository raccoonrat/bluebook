

下面我直接开始 **Chapter 1. Unified AI Security Control Plane: Problem Statement and System Boundary** 的细化，给你一版**可直接进入蓝皮书正文的章节骨架 + 扩展正文草稿**。这一章的核心任务，不是列举已有能力，而是先把**问题定义、系统对象、责任边界、非目标边界、以及理论升格方向**一次性立住。你上传的材料已经给出了这一章最关键的基础：**DTL 不应被理解为若干 detector 或一个孤立 evaluation 平台，而应被理解为一个统一的 AI Security Control Plane；其中 Evaluation 负责风险测量与证据治理，Runtime Kernel 负责策略执行与可解释防护，Chinese & Multilingual Intelligence 负责 geo-specific semantics，xCloud 负责 execution / orchestration / SLA，而整个体系的理论突破点在于 risk-adaptive control selection、composable defense cascade、compliance-aligned safety semantics。** [[统一 AI 安全控制平面 | PowerPoint]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/_layouts/15/Doc.aspx?sourcedoc=%7BC99A9A8B-A736-4945-830E-C325133B6A4B%7D&file=%E7%BB%9F%E4%B8%80%20AI%20%E5%AE%89%E5%85%A8%E6%8E%A7%E5%88%B6%E5%B9%B3%E9%9D%A2.pptx&action=edit&mobileredirect=true)

* * *

Chapter 1. Unified AI Security Control Plane: Problem Statement and System Boundary

1.0 Chapter Objective（本章目标）

本章的目标，是把整个蓝皮书的中心问题从“我们有哪些安全能力”重新定义为“我们在构建什么样的系统”。根据现有设计，问题的核心并不是 detector 数量、单个 guard model 能力、或一个独立 evaluation 平台的完备性，而是：**安全判断、评测治理、运行时执行、证据链与发布放行，能否在同一个统一控制平面中形成可持续运作的系统边界。** [[统一 AI 安全控制平面 | PowerPoint]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/_layouts/15/Doc.aspx?sourcedoc=%7BC99A9A8B-A736-4945-830E-C325133B6A4B%7D&file=%E7%BB%9F%E4%B8%80%20AI%20%E5%AE%89%E5%85%A8%E6%8E%A7%E5%88%B6%E5%B9%B3%E9%9D%A2.pptx&action=edit&mobileredirect=true)

更具体地说，本章需要立住四个根本判断：第一，DTL 的中心对象是 **Security Control Plane**，而不是若干零散能力；第二，Evaluation 与 Runtime 不是两套割裂系统，而是从 benchmark 到 judgment、再到 runtime API 和 evidence chain 的连续体系；第三，中国与多语安全能力不是外围适配，而是统一控制平面中的 intelligence 源头；第四，xCloud 与 DTL 之间必须存在清晰责任边界：**DTL 提供 security judgment / evidence，xCloud 提供 execution / orchestration / SLA。** [[统一 AI 安全控制平面 | PowerPoint]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/_layouts/15/Doc.aspx?sourcedoc=%7BC99A9A8B-A736-4945-830E-C325133B6A4B%7D&file=%E7%BB%9F%E4%B8%80%20AI%20%E5%AE%89%E5%85%A8%E6%8E%A7%E5%88%B6%E5%B9%B3%E9%9D%A2.pptx&action=edit&mobileredirect=true)

* * *

1.1 Problem Statement（问题定义）

### 1.1.1 核心问题不是“工具够不够多”，而是“系统边界是否成立”

在传统叙事中，企业级 AI 安全往往被拆解为若干相互平行的子问题，例如：内容检测、攻击检测、越狱识别、评测平台、审计报表、合规适配、多语言支持等。然而，现有设计明确指出，这种叙事方式是不充分的，因为它把本质上应被统一的对象拆散为一组工程模块，从而模糊了**谁负责安全判断，谁负责平台执行，评测、控制、证据、xCloud 之间如何组成统一 control plane** 这一根本问题。 [[统一 AI 安全控制平面 | PowerPoint]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/_layouts/15/Doc.aspx?sourcedoc=%7BC99A9A8B-A736-4945-830E-C325133B6A4B%7D&file=%E7%BB%9F%E4%B8%80%20AI%20%E5%AE%89%E5%85%A8%E6%8E%A7%E5%88%B6%E5%B9%B3%E9%9D%A2.pptx&action=edit&mobileredirect=true)

因此，本蓝皮书不把问题定义为“构建更多安全功能”，而把问题定义为：**如何建立一个统一的 AI Security Control Plane，使风险测量、策略判断、运行时执行、证据输出与发布放行形成闭环系统。** 这一问题定义直接来自当前设计对系统边界的重新组织：上方是 **Evaluation & Benchmark Governance**，中心是 **Security Control Plane**，左下是 **Chinese & Multilingual Safety Intelligence**，右侧是 **xCloud / Execution Environment**，右下是 **Evidence & Release Gating**。 [[统一 AI 安全控制平面 | PowerPoint]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/_layouts/15/Doc.aspx?sourcedoc=%7BC99A9A8B-A736-4945-830E-C325133B6A4B%7D&file=%E7%BB%9F%E4%B8%80%20AI%20%E5%AE%89%E5%85%A8%E6%8E%A7%E5%88%B6%E5%B9%B3%E9%9D%A2.pptx&action=edit&mobileredirect=true)

### 1.1.2 当前行业常见误区

从本项目当前材料反复强调的内容看，最需要避免的不是“能力不足”，而是“把控制平面误写成能力列表”。现有设计已经明确指出，真正需要突出的关键技术不是某个 detector，而是**从 benchmark → judgment → runtime API → evidence chain 的统一系统化能力**；同样，运行时安全也不能被简化为“有一个 guard model”，而应被理解为从 language gate、preprocess、lite detectors、router、heavy detectors 到 decision layer 的多阶段流水线。 [[统一 AI 安全控制平面 | PowerPoint]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/_layouts/15/Doc.aspx?sourcedoc=%7BC99A9A8B-A736-4945-830E-C325133B6A4B%7D&file=%E7%BB%9F%E4%B8%80%20AI%20%E5%AE%89%E5%85%A8%E6%8E%A7%E5%88%B6%E5%B9%B3%E9%9D%A2.pptx&action=edit&mobileredirect=true)

换言之，如果本章只从功能组件出发，蓝皮书就会退化为工程能力说明书；而只有从统一控制平面的角度定义问题，后续章节中的双轨评测、运行时内核、风险驱动闭环与统一研发框架，才会成为同一体系中的不同切面，而不是相互平行的研发条线。 [[统一 AI 安全控制平面 | PowerPoint]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/_layouts/15/Doc.aspx?sourcedoc=%7BC99A9A8B-A736-4945-830E-C325133B6A4B%7D&file=%E7%BB%9F%E4%B8%80%20AI%20%E5%AE%89%E5%85%A8%E6%8E%A7%E5%88%B6%E5%B9%B3%E9%9D%A2.pptx&action=edit&mobileredirect=true)

* * *

1.2 Why a Control Plane?（为什么必须是控制平面）

### 1.2.1 因为安全判断不能脱离 benchmark 与 acceptance contract

现有设计明确指出，上方的 **Evaluation & Benchmark Governance** 不只是一个测评组件，而是所有安全判断的前提条件，因为安全判断不能脱离 benchmark 与 acceptance contract。也就是说，系统中的每一个防护动作、每一种阻断解释、每一次发布放行，都应能够回溯到 benchmark、profile、decision logic 和 evidence。 [[统一 AI 安全控制平面 | PowerPoint]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/_layouts/15/Doc.aspx?sourcedoc=%7BC99A9A8B-A736-4945-830E-C325133B6A4B%7D&file=%E7%BB%9F%E4%B8%80%20AI%20%E5%AE%89%E5%85%A8%E6%8E%A7%E5%88%B6%E5%B9%B3%E9%9D%A2.pptx&action=edit&mobileredirect=true)

这意味着，Evaluation 的价值不在于“给一个分数”，而在于为整个系统提供**风险测量与证据治理**。如果没有这一层，Runtime 只能执行局部规则；而一旦有了这一层，Runtime 才能成为受证据驱动、受风险约束、并最终可被审计的执行内核。 [[统一 AI 安全控制平面 | PowerPoint]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/_layouts/15/Doc.aspx?sourcedoc=%7BC99A9A8B-A736-4945-830E-C325133B6A4B%7D&file=%E7%BB%9F%E4%B8%80%20AI%20%E5%AE%89%E5%85%A8%E6%8E%A7%E5%88%B6%E5%B9%B3%E9%9D%A2.pptx&action=edit&mobileredirect=true)

### 1.2.2 因为运行时防护本质上是判断与执行的耦合系统

现有材料对 Runtime Security Kernel 的描述非常明确：系统不是一个单点 detector，而是一条工业化流水线，包括 `detect_languages()`、Normalization、Sanitization、Lite Detectors、Intelligent Gate、Heavy Detectors、Decision Layer，并最终输出 Scoring、Aggregation、Allow / Block / Redact / Degrade。 [[统一 AI 安全控制平面 | PowerPoint]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/_layouts/15/Doc.aspx?sourcedoc=%7BC99A9A8B-A736-4945-830E-C325133B6A4B%7D&file=%E7%BB%9F%E4%B8%80%20AI%20%E5%AE%89%E5%85%A8%E6%8E%A7%E5%88%B6%E5%B9%B3%E9%9D%A2.pptx&action=edit&mobileredirect=true)

这说明运行时防护不是一个静态判断器，而是一个由多阶段判断、路由升级、动作执行与解释输出共同组成的系统。因此，Runtime 只有放在统一控制平面中，才能与 Evaluation、Evidence、Release Gating 形成真正闭环；否则它只能被理解为一个局部防护链路，而非企业级 AI 安全系统的执行中枢。 [[统一 AI 安全控制平面 | PowerPoint]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/_layouts/15/Doc.aspx?sourcedoc=%7BC99A9A8B-A736-4945-830E-C325133B6A4B%7D&file=%E7%BB%9F%E4%B8%80%20AI%20%E5%AE%89%E5%85%A8%E6%8E%A7%E5%88%B6%E5%B9%B3%E9%9D%A2.pptx&action=edit&mobileredirect=true)

### 1.2.3 因为中国与多语能力必须进入主系统，而不是边缘适配

现有设计明确把 **Chinese & Multilingual Safety Intelligence** 放在统一控制平面的核心构成中，而不是放在“本地化支持”或“区域化适配”的边缘位置。材料明确强调：中文与多语安全不是外围适配，而是 intelligence 源头；同时中国监管评测并非薄薄一层分类，而是拥有大规模中文用例、细分风险类别、独立 test suite、aggregation logic 和报告模板的完整轨道。 [[统一 AI 安全控制平面 | PowerPoint]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/_layouts/15/Doc.aspx?sourcedoc=%7BC99A9A8B-A736-4945-830E-C325133B6A4B%7D&file=%E7%BB%9F%E4%B8%80%20AI%20%E5%AE%89%E5%85%A8%E6%8E%A7%E5%88%B6%E5%B9%B3%E9%9D%A2.pptx&action=edit&mobileredirect=true)

因此，控制平面的建立还有一个根本动机：**让 geo-specific semantics 能够进入统一语义、统一评测、统一控制与统一证据体系。** 如果这一点做不到，中国能力就只能停留在支持层，而无法成为全球安全程序中的核心 intelligence 单元。 [[统一 AI 安全控制平面 | PowerPoint]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/_layouts/15/Doc.aspx?sourcedoc=%7BC99A9A8B-A736-4945-830E-C325133B6A4B%7D&file=%E7%BB%9F%E4%B8%80%20AI%20%E5%AE%89%E5%85%A8%E6%8E%A7%E5%88%B6%E5%B9%B3%E9%9D%A2.pptx&action=edit&mobileredirect=true)

* * *

1.3 System Boundary（系统边界）

### 1.3.1 本系统的五个核心对象

根据现有设计，本系统至少包含五个核心对象：

1. **Evaluation & Benchmark Governance**：负责风险测量、benchmark constitution、acceptance contract 与证据治理的起点。 [[统一 AI 安全控制平面 | PowerPoint]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/_layouts/15/Doc.aspx?sourcedoc=%7BC99A9A8B-A736-4945-830E-C325133B6A4B%7D&file=%E7%BB%9F%E4%B8%80%20AI%20%E5%AE%89%E5%85%A8%E6%8E%A7%E5%88%B6%E5%B9%B3%E9%9D%A2.pptx&action=edit&mobileredirect=true)
2. **Security Control Plane**：作为系统中枢，承担 routing、scoring、policy judgment 等核心控制职责。 [[统一 AI 安全控制平面 | PowerPoint]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/_layouts/15/Doc.aspx?sourcedoc=%7BC99A9A8B-A736-4945-830E-C325133B6A4B%7D&file=%E7%BB%9F%E4%B8%80%20AI%20%E5%AE%89%E5%85%A8%E6%8E%A7%E5%88%B6%E5%B9%B3%E9%9D%A2.pptx&action=edit&mobileredirect=true)
3. **Chinese & Multilingual Safety Intelligence**：作为 geo-specific semantics 与中文/多语安全能力的 intelligence 源头。 [[统一 AI 安全控制平面 | PowerPoint]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/_layouts/15/Doc.aspx?sourcedoc=%7BC99A9A8B-A736-4945-830E-C325133B6A4B%7D&file=%E7%BB%9F%E4%B8%80%20AI%20%E5%AE%89%E5%85%A8%E6%8E%A7%E5%88%B6%E5%B9%B3%E9%9D%A2.pptx&action=edit&mobileredirect=true)
4. **xCloud / Execution Environment**：负责 execution、orchestration、容器/API 交付与 SLA 保障。 [[统一 AI 安全控制平面 | PowerPoint]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/_layouts/15/Doc.aspx?sourcedoc=%7BC99A9A8B-A736-4945-830E-C325133B6A4B%7D&file=%E7%BB%9F%E4%B8%80%20AI%20%E5%AE%89%E5%85%A8%E6%8E%A7%E5%88%B6%E5%B9%B3%E9%9D%A2.pptx&action=edit&mobileredirect=true)
5. **Evidence & Release Gating**：将判断结果转化为可追踪的证据、放行决策与审计对象。 [[统一 AI 安全控制平面 | PowerPoint]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/_layouts/15/Doc.aspx?sourcedoc=%7BC99A9A8B-A736-4945-830E-C325133B6A4B%7D&file=%E7%BB%9F%E4%B8%80%20AI%20%E5%AE%89%E5%85%A8%E6%8E%A7%E5%88%B6%E5%B9%B3%E9%9D%A2.pptx&action=edit&mobileredirect=true)

这五个对象并不是简单拼接关系，而是围绕统一控制平面形成“评测输入—控制判断—平台执行—证据输出”的连续系统结构。现有设计已明确要求把视觉与逻辑重点都放在这一统一结构上，而不是列功能清单。 [[统一 AI 安全控制平面 | PowerPoint]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/_layouts/15/Doc.aspx?sourcedoc=%7BC99A9A8B-A736-4945-830E-C325133B6A4B%7D&file=%E7%BB%9F%E4%B8%80%20AI%20%E5%AE%89%E5%85%A8%E6%8E%A7%E5%88%B6%E5%B9%B3%E9%9D%A2.pptx&action=edit&mobileredirect=true)

### 1.3.2 DTL 与 xCloud 的责任边界

本章必须明确写清 DTL 与 xCloud 的责任边界。现有设计已经反复强调：**DTL 提供 security judgment / evidence，xCloud 提供 execution / orchestration / SLA。** 这意味着 DTL 的重点不是替代平台侧的执行环境，而是为平台提供安全判断、风险评分、策略建议、阻断解释与可审计证据；相应地，xCloud 的重点不是定义安全判断逻辑，而是承接这些判断并将其转化为平台层的执行与交付。 [[统一 AI 安全控制平面 | PowerPoint]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/_layouts/15/Doc.aspx?sourcedoc=%7BC99A9A8B-A736-4945-830E-C325133B6A4B%7D&file=%E7%BB%9F%E4%B8%80%20AI%20%E5%AE%89%E5%85%A8%E6%8E%A7%E5%88%B6%E5%B9%B3%E9%9D%A2.pptx&action=edit&mobileredirect=true)

这一边界非常关键，因为它直接决定后续章节中哪些内容属于 Runtime Kernel，哪些内容属于平台交付；哪些内容属于证据治理，哪些内容属于 orchestration 与 SLA；以及为什么统一控制平面必须是“安全判断与证据治理的系统”，而不是一个泛化的平台替代方案。 [[统一 AI 安全控制平面 | PowerPoint]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/_layouts/15/Doc.aspx?sourcedoc=%7BC99A9A8B-A736-4945-830E-C325133B6A4B%7D&file=%E7%BB%9F%E4%B8%80%20AI%20%E5%AE%89%E5%85%A8%E6%8E%A7%E5%88%B6%E5%B9%B3%E9%9D%A2.pptx&action=edit&mobileredirect=true)

### 1.3.3 本系统的输入、输出与闭环

根据现有设计，本系统的输入不只是用户提示词或模型请求，还包括 benchmark、run profile、test profile、模型能力画像与地域/合规语义；系统的中间态包括 scoring、aggregation、policy judgment、risk profile 与 control selection；系统的输出则包括 allow / block / redact / degrade 等动作，以及 evidence、release gating 与 audit artifact。 [[统一 AI 安全控制平面 | PowerPoint]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/_layouts/15/Doc.aspx?sourcedoc=%7BC99A9A8B-A736-4945-830E-C325133B6A4B%7D&file=%E7%BB%9F%E4%B8%80%20AI%20%E5%AE%89%E5%85%A8%E6%8E%A7%E5%88%B6%E5%B9%B3%E9%9D%A2.pptx&action=edit&mobileredirect=true)

更重要的是，这些输入与输出并不是单向链路。材料在“Risk Adaptation”部分已经明确提出闭环结构：**Measure → Profile → Select → Enforce**，右侧连接 **Evidence & Release Gating**，再反向反馈到 Measure，用于 regression 与持续改进。也就是说，系统边界本身就包含一个持续强化的反馈结构，而不是一次性的决策流水线。 [[统一 AI 安全控制平面 | PowerPoint]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/_layouts/15/Doc.aspx?sourcedoc=%7BC99A9A8B-A736-4945-830E-C325133B6A4B%7D&file=%E7%BB%9F%E4%B8%80%20AI%20%E5%AE%89%E5%85%A8%E6%8E%A7%E5%88%B6%E5%B9%B3%E9%9D%A2.pptx&action=edit&mobileredirect=true)

* * *

1.4 What the System Is Not（系统不是什么）

### 1.4.1 不是 detector 集合

本系统不应被描述为“若干 detector 的集合”。现有设计已明确指出，真正应突出的是统一语义层、统一判断层与统一证据层，而不是某个 detector 本身。即使在运行时内核内部，系统也是由 language gate、preprocess、lite detector shelf、router、heavy model block 与 decision layer 共同构成，而不是依赖一个单点组件。 [[统一 AI 安全控制平面 | PowerPoint]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/_layouts/15/Doc.aspx?sourcedoc=%7BC99A9A8B-A736-4945-830E-C325133B6A4B%7D&file=%E7%BB%9F%E4%B8%80%20AI%20%E5%AE%89%E5%85%A8%E6%8E%A7%E5%88%B6%E5%B9%B3%E9%9D%A2.pptx&action=edit&mobileredirect=true)

### 1.4.2 不是孤立的 evaluation 平台

本系统也不应被描述为一个孤立的评测平台。材料明确指出，Evaluation 的职责不是停留在 run / target / result store 层，而是服务于 release decision、责任边界和后续审计；同时，Evaluation 还必须与 Runtime Control、Evidence、Release Gating 构成统一体系。 [[统一 AI 安全控制平面 | PowerPoint]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/_layouts/15/Doc.aspx?sourcedoc=%7BC99A9A8B-A736-4945-830E-C325133B6A4B%7D&file=%E7%BB%9F%E4%B8%80%20AI%20%E5%AE%89%E5%85%A8%E6%8E%A7%E5%88%B6%E5%B9%B3%E9%9D%A2.pptx&action=edit&mobileredirect=true)

### 1.4.3 不是本地支持或区域适配的附属层

本系统中的 Chinese & Multilingual Safety Intelligence 不应被写成区域化 support。现有设计明确说明，它是统一控制平面的 intelligence 源头之一，并且中国监管轨道拥有独立 test suite、aggregation logic、规模化中文用例与细分风险类别，因此它属于系统核心，而不是附属能力。 [[统一 AI 安全控制平面 | PowerPoint]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/_layouts/15/Doc.aspx?sourcedoc=%7BC99A9A8B-A736-4945-830E-C325133B6A4B%7D&file=%E7%BB%9F%E4%B8%80%20AI%20%E5%AE%89%E5%85%A8%E6%8E%A7%E5%88%B6%E5%B9%B3%E9%9D%A2.pptx&action=edit&mobileredirect=true)

* * *

1.5 Research Elevation（研究升格）

### 1.5.1 从系统架构到理论问题

现有设计已经明确指出，本系统最适合升格成“AI Security Control Plane”的理论表达，其理论价值不在于“多个模块怎么连”，而在于三个问题：

* 如何形式化 **安全判断** 与 **执行编排** 的责任边界； [[统一 AI 安全控制平面 | PowerPoint]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/_layouts/15/Doc.aspx?sourcedoc=%7BC99A9A8B-A736-4945-830E-C325133B6A4B%7D&file=%E7%BB%9F%E4%B8%80%20AI%20%E5%AE%89%E5%85%A8%E6%8E%A7%E5%88%B6%E5%B9%B3%E9%9D%A2.pptx&action=edit&mobileredirect=true)
* 如何把 benchmark 证据、运行时策略与审计记录放入同一个可组合框架； [[统一 AI 安全控制平面 | PowerPoint]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/_layouts/15/Doc.aspx?sourcedoc=%7BC99A9A8B-A736-4945-830E-C325133B6A4B%7D&file=%E7%BB%9F%E4%B8%80%20AI%20%E5%AE%89%E5%85%A8%E6%8E%A7%E5%88%B6%E5%B9%B3%E9%9D%A2.pptx&action=edit&mobileredirect=true)
* 如何把 geo-specific intelligence 纳入统一控制平面。 [[统一 AI 安全控制平面 | PowerPoint]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/_layouts/15/Doc.aspx?sourcedoc=%7BC99A9A8B-A736-4945-830E-C325133B6A4B%7D&file=%E7%BB%9F%E4%B8%80%20AI%20%E5%AE%89%E5%85%A8%E6%8E%A7%E5%88%B6%E5%B9%B3%E9%9D%A2.pptx&action=edit&mobileredirect=true)

这意味着 Chapter 1 不只是系统总览章，更是全书理论升格的开章：后续关于双轨评测、运行时内核、风险自适应控制与证据图谱的所有章节，都要被解释为这一统一理论对象的不同展开。 [[统一 AI 安全控制平面 | PowerPoint]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/_layouts/15/Doc.aspx?sourcedoc=%7BC99A9A8B-A736-4945-830E-C325133B6A4B%7D&file=%E7%BB%9F%E4%B8%80%20AI%20%E5%AE%89%E5%85%A8%E6%8E%A7%E5%88%B6%E5%B9%B3%E9%9D%A2.pptx&action=edit&mobileredirect=true)

### 1.5.2 本章应立住的三条理论主线

本章建议明确埋下三条理论主线，且这些主线都已在现有设计中被点名：

* **Risk-adaptive control selection**：说明控制不是固定配置，而是受风险、成本、时延与容忍度约束的决策问题。 [[统一 AI 安全控制平面 | PowerPoint]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/_layouts/15/Doc.aspx?sourcedoc=%7BC99A9A8B-A736-4945-830E-C325133B6A4B%7D&file=%E7%BB%9F%E4%B8%80%20AI%20%E5%AE%89%E5%85%A8%E6%8E%A7%E5%88%B6%E5%B9%B3%E9%9D%A2.pptx&action=edit&mobileredirect=true)
* **Composable defense cascade**：说明运行时防护由多阶段异构组件构成，其组合方式本身是理论问题。 [[统一 AI 安全控制平面 | PowerPoint]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/_layouts/15/Doc.aspx?sourcedoc=%7BC99A9A8B-A736-4945-830E-C325133B6A4B%7D&file=%E7%BB%9F%E4%B8%80%20AI%20%E5%AE%89%E5%85%A8%E6%8E%A7%E5%88%B6%E5%B9%B3%E9%9D%A2.pptx&action=edit&mobileredirect=true)
* **Compliance-aligned safety semantics**：说明法规、地域与语义差异并非外部条件，而是系统核心建模对象。 [[统一 AI 安全控制平面 | PowerPoint]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/_layouts/15/Doc.aspx?sourcedoc=%7BC99A9A8B-A736-4945-830E-C325133B6A4B%7D&file=%E7%BB%9F%E4%B8%80%20AI%20%E5%AE%89%E5%85%A8%E6%8E%A7%E5%88%B6%E5%B9%B3%E9%9D%A2.pptx&action=edit&mobileredirect=true)

* * *

1.6 可直接放入蓝皮书的 Chapter Summary（本章结论）

本章的核心结论是：**Unified AI Security Control Plane** 不是一个由 detector、evaluation、区域适配与报表系统组成的工程集合，而是一个围绕安全判断、风险测量、运行时执行、证据治理与发布放行建立的统一系统。现有设计已经明确给出了这一系统的边界：Evaluation 负责 benchmark governance 与风险测量，Security Control Plane 负责 routing、scoring 与 policy judgment，Chinese & Multilingual Safety Intelligence 负责 geo-specific semantics，xCloud 负责 execution / orchestration / SLA，Evidence & Release Gating 则负责把判断转化为可追踪、可审计、可放行的结果。 [[统一 AI 安全控制平面 | PowerPoint]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/_layouts/15/Doc.aspx?sourcedoc=%7BC99A9A8B-A736-4945-830E-C325133B6A4B%7D&file=%E7%BB%9F%E4%B8%80%20AI%20%E5%AE%89%E5%85%A8%E6%8E%A7%E5%88%B6%E5%B9%B3%E9%9D%A2.pptx&action=edit&mobileredirect=true)

因此，后续章节的任务不是分别描述若干能力，而是在这一系统边界内，进一步展开双轨评测底座、运行时安全内核、风险自适应控制、证据图谱与中国/多语 intelligence 的统一研发逻辑。换言之，本章确立的不是“模块分工图”，而是整个蓝皮书的**第一性系统对象**。 [[统一 AI 安全控制平面 | PowerPoint]](https://lenovo-my.sharepoint.com/personal/wangyh43_lenovo_com/_layouts/15/Doc.aspx?sourcedoc=%7BC99A9A8B-A736-4945-830E-C325133B6A4B%7D&file=%E7%BB%9F%E4%B8%80%20AI%20%E5%AE%89%E5%85%A8%E6%8E%A7%E5%88%B6%E5%B9%B3%E9%9D%A2.pptx&action=edit&mobileredirect=true)

* * *
