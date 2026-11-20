# CStack — Product Requirements Document

**Version:** 0.2 Draft  
**Status:** MVP Ready (Explore + Spec)  
**Category:** AI Software Engineering Skills  
**Tagline:** *Discover the domain. Design the product. Evolve the software.*

---

## 1. Product Overview

### 1.1 产品定位

cstack 是一套面向 AI Coding Agents 的软件工程 Skills，整合成熟的软件工程方法与 AI 驱动的工作流程，帮助开发者完成从业务知识发现、产品设计到软件设计及持续演化的工作。

cstack 不试图取代 Coding Agent，也不重新实现通用的编码、测试、Review、原型制作（Prototype）和交付能力。

它专注于软件开发中更依赖经验、判断力和知识积累的四个认知动作：

- **Explore：** 理解问题，发现并提取现实业务领域的知识。
- **Spec：** 根据产品目标创造性地设计软件系统的职责、工作方式与预期行为。
- **Design：** 运用 CMP 和成熟架构原则，将产品行为转化为可执行的软件设计骨架、边界契约与修改计划。
- **Reflect：** 从实际开发过程中捕获设计摩擦与上下文轨迹，维护软件结构健康，并将已实现的变化合并回长期知识。

四项能力组成一个可以往复迭代的工程系统，而不是必须依次执行的流水线。

### 1.2 核心理念

软件并非现实业务的直接数字化，而是为了特定目标，对现实世界进行有选择的抽象、建模和制度化。

开发软件需要完成三种不同的认知活动：

1. **理解现实：** 发现业务中的概念、关系、规则、工作方式和真实问题。
2. **设计产品：** 根据产品目标重新组织工作、定义职责、设计行为，形成一种新的业务运行方式。
3. **构造软件：** 将产品设计表达为可执行、可验证、可持续修改的软件结构。

这三种活动高度关联，但各自的评价标准不同。

AI 的价值不仅是生成代码，更是承担知识搜索、组织、分析、比较和提取工作，降低开发者在这些认知活动中的负担。

### 1.3 核心价值主张

**Make software engineering knowledge discoverable, actionable, and maintainable.**

cstack 帮助开发者：

- 发现过去依赖经验和隐性认知的领域知识。
- 将模糊的问题、既有解决方案与用户预期转化为明确的产品设计。
- 以统一的设计方法论指导软件结构的构造与演化。
- 将一次开发工作中发现的有价值知识保存下来，避免未来重复支付相同的认知成本。

---

## 2. Problem Statement

### 2.1 AI 降低了代码生产成本，却没有自动解决软件工程问题

Coding Agents 已经能够高效完成大量代码编写工作，但软件工程仍面临多种认知成本：

- 开发者需要花费大量时间理解业务和产品背景。
- 客户常常直接提出解决方案，而不是明确说明背后的问题。
- 领域知识散落在文档、代码、对话和专家经验中。
- 产品需求在开发中不断被重新理解和修正。
- 软件架构在连续修改过程中逐渐偏离原有设计。
- 关键设计经验随着任务结束和 Agent Context 清空而丢失。

这些问题不能仅靠提高代码生成能力解决。

### 2.2 现有 Skills 的不足

pstack、gstack、Matt Pocock Skills（`grill-me` / `grill-with-docs` / `domain-modeling` / `to-spec`）等项目已经提供了大量值得借鉴的成熟实践，包括 Grilling、Domain Modeling、Product Planning、Architecture、Verification 和 Reflection。

但仍存在可以进一步改善的空间：

- **知识发现：** 如何系统地发现领域概念、关系、约束与隐藏假设，并妥善对待客户直接提出的方案，而不是简单收集需求？
- **产品设计：** 如何从现实业务和客户提出的方案出发，创造新的工作方式，而不是直接复制现有流程？
- **软件设计：** 如何用统一的判断依据（Context Cost）比较架构方案、设计边界，并控制未来修改成本？
- **知识演化：** 如何通过严格的 Delta + Merge 机制让认知变化可靠沉淀，并防止软件结构与项目知识同步腐化？

cstack 将吸收现有实践（尤其是 `grill-with-docs` 的决策树前沿审问与实时落盘机制），并使用 Ontology 和 CMP 建立贯穿这些活动的思维框架。

---

## 3. Target Users

### 3.1 主要用户

**具备软件工程经验、使用 Coding Agent 开发真实产品的开发者。**

典型特征：

- 独立开发者或小型产品团队。
- 参与业务需求分析和产品设计。
- 使用 Cursor、Claude Code 等 Agent 开发工具。
- 需要长期维护产品，而不是只生成一次性 Demo。
- 希望将自己的工程判断力与 Agent 的执行能力结合起来。

### 3.2 典型使用场景

- **Greenfield Development：** 从一个产品想法出发，探索业务领域、明确目标、设计产品并建立初始软件架构。
- **Brownfield Development：** 在已有产品中增加功能或调整业务规则，利用现有领域知识和产品规格完成增量设计与修改。
- **Continuous Evolution：** 在日常开发过程中，根据新的业务认知、技术反馈和实际修改经验，持续调整产品知识与软件设计。

### 3.3 非目标用户

cstack 不以完全不了解软件工程的用户一键生成完整产品为主要目标。

它的设计假设是：**AI 负责执行大量知识劳动，人仍然对关键的业务判断、产品选择和工程决策负责。**

---

## 4. Product Philosophy

### P1. Knowledge belongs where it is best expressed

知识应该保存在最适合表达它的载体中，而不是在不同开发阶段重复描述。

这是一种覆盖代码、文档和整个工程过程的 DRY 原则。

领域知识适合通过概念、关系和规则表达；产品设计适合通过行为规格表达；软件设计优先通过代码、类型、接口、测试及架构索引表达。

### P2. Discover before assuming

Agent 应主动识别未知知识和隐性假设。

客户提出的解决方案既是有效的需求输入，也是发现业务知识的重要线索。

不能盲目执行，也不应习惯性否定。

### P3. Model the domain, design the product

领域建模追求对现实业务的忠实理解。

产品设计则是一项创造性工作，可以改变既有业务的角色分工、操作方式和工作流程。

领域专家是业务知识的重要来源，但不必是产品设计的最终决策者。

### P4. Design for modification

软件开发是一系列连续的 Modification。

运用 Context Minimization Principle（CMP），使未来正确修改软件所需的充分上下文更容易获取。

架构原则是实现目标的手段，不是必须遵守的教条。

### P5. Knowledge evolves through strict Delta + Merge

从 Greenfield 到 Brownfield，软件和产品知识始终处于持续修改过程中。

所有围绕变更产生的探索与设计增量都隔离在单次 Change 文件中，待实现确认后统一 Merge 回 Living Knowledge；同时允许用户直接编辑 Living Knowledge 以修正对现状的错误描述。

### P6. Skills are iterative, not sequential

Explore、Spec、Design 和 Reflect 是具有明确职责的不同工作模式，不是固定的开发工序。

新的发现可以使任何阶段重新开启。

### P7. Minimize human cognitive burden

Agent 应尽可能自行完成信息搜索、分析、归纳和一致性检查。

只有真正影响业务事实、产品决策或实施方向的关键问题，才需要寻求人类判断。

---

## 5. Core Product Capabilities

### 5.1 Explore — Problem & Domain Discovery

**目标：理解用户问题及其所在的现实业务领域。**

#### 方法论

以 Matt Pocock 的 `grill-with-docs`（决策树前沿审问 + 事实自查 + 实时落盘）为交互起点，融合 Ontology、DDD Strategic Design、Ubiquitous Language、EventStorming 的有效思想。

不要求完整使用任何特定框架或术语。

#### 核心能力

- **EXP-01 Problem Discovery：** 识别用户面临的问题、目标、利益相关者、现有解决方式和约束。
- **EXP-02 Domain Knowledge Discovery：** 发现领域中的核心概念、关系、业务规则、事件、状态和工作方式；与领域专家或可信材料确认重要的领域事实。
- **EXP-03 Proposed Solution Capture：** 完整记录客户提出的解决方案（编号记录 `PS-*`），包括功能要求、工作流程和操作方式；理解方案背后的业务动机，不默认它必须原样实现，也不居高临下地否定。
- **EXP-04 Knowledge Gap Detection & Frontier Grilling：** 优先通过已有知识库和代码库自行解决事实问题；将未决的领域缺口、术语冲突和隐藏假设组织成决策树，按前沿（Frontier）分轮追问，每题附带推荐答案与依据。
- **EXP-05 Interactive Knowledge Refinement：** 通过具体实例、反例和边界情况验证领域模型；随着沟通进展实时更新当前 Change 文件，而不是在访谈结束后一次性回忆生成。

#### 输出知识（写入当前 Change 文件）

- Problem Definition & Constraints
- Proposed Solutions (`PS-*`)
- Domain Concepts & Relationships Delta
- Business Rules & Constraints Delta
- Existing Workflows & Practices
- Stakeholders & Responsibilities
- Confirmed Facts vs. Open Questions & Assumptions

**完成标准：** 当前产品设计所依赖的重要问题、领域事实和约束已得到足够理解，剩余不确定性明确可见，且与人类达成明确确认。

---

### 5.2 Spec — Product Design & Behavior Specification

**目标：基于领域知识与产品目标，创造性地设计软件介入之后的工作系统与外部行为。**

Spec 不是单纯的需求文档生成，也不是被动记录客户提出的功能清单。它需要创造性地决定产品应该如何解决问题。

#### 方法论

以 `grill-with-docs` 的决策树审问与 `to-spec` 的外部行为导向、显式拒绝清单（Out of Scope）为基础，结合 Product Design、BDD、Example Mapping 和业务流程设计。

#### 核心能力

- **SPEC-01 Product Model Design：** 根据产品目标选择和组织领域概念，构建产品自身的概念模型；允许引入并不存在于原有业务中的产品抽象。
- **SPEC-02 Responsibility & Workflow Design：** 设计软件、用户、Agent 和其他参与者之间的职责与协作方式，定义目标工作流程、交互行为、决策点和权限约束。
- **SPEC-03 Behavior Specification & Out of Scope：** 明确外部可观测功能行为、业务规则、状态转换、异常情况与验收条件（使用自然语言、具体实例和必要的 Given-When-Then，不强制特定 BDD 文件格式）；同时明确列出 **Out of Scope（显式拒绝清单）**，防止实现阶段范围膨胀。
- **SPEC-04 Solution Coverage：** 逐条对照 Explore 阶段记录的客户原案（`PS-*`），明确每一项的处理方式：
  - 原样满足
  - 使用其他方案实现相同目标
  - 经讨论调整或取消
  - 尚未解决
  
  产品不能未经确认就静默丢弃客户提出的重要要求。
- **SPEC-05 Product Decision Making & External Prototype Handoff：** 对候选方案进行分析和比较，区分已确认领域事实、产品设计选择与尚未证实的产品假设。对于纯文字难以判断体验/视觉/交互直觉的假设，显式标记为适合交给**外部 Prototype 工具**验证的条目；cstack 本身不内置原型工具，但确保 Change Spec 足够自包含以便直接喂给外部工具，并支持将验证结论回写进 Spec。
- **SPEC-06 Brownfield Fast-Path：** 对于小规模增量变更，允许直接从 `/cs-spec` 进入，在读取知识库索引后内联完成轻量知识缺口检查；仅当发现陌生领域、深层概念冲突或复杂隐藏约束时，显式建议切回 `/cs-explore`。

#### 输出知识（写入当前 Change 文件）

- Product Model & Target Workflows
- Features & Capabilities
- Behavior Specifications & Acceptance Scenarios
- Solution Coverage Table (`PS-*`)
- Product Decisions, Trade-offs & External Prototype Candidates
- Out of Scope (Explicit Refusals)
- Preliminary Living Knowledge Merge Notes

**完成标准：** 产品应该提供的外部行为足够明确，能够据此开展软件设计；所有重要客户原案均有明确去向；关键产品决策、Out of Scope 及其依据得到记录并获人类确认。

---

### 5.3 Design — Context-Cost Driven Software Design

**目标：运用 CMP 和 Clean Architecture，将产品行为转化为清晰的软件设计边界、可执行契约与实现计划，使系统在未来持续修改过程中保持较低的 Context Cost。**

#### 与通用 Coding Agent 的职责边界

Design 聚焦于架构判断、边界划分与可执行契约定义（Entities / Domain Types / Use Cases / Ports / 依赖约束 / 垂直切片 Implementation Tasks），而非重新实现通用编码、测试与 CI 流程。具体的 Mechanism/Adapter 填充与测试执行可交由通用 Coding Agent 在 Design 锁定的边界与任务切片内完成。

#### 方法论

Design 以 **Context Minimization Principle（CMP）** 为设计评价框架，以 **Clean Architecture** 为主要设计实践。

CMP 的核心判断标准是：

> For the modifications a system must realistically support, a design is better when the sufficient context required for correct modification is cheaper to acquire.

CMP 从两个维度分析这种成本：

- **Depth：** 理解一个软件元素的行为，需要获取多少相关上下文，以及必须穿越多少不可靠的抽象边界。
- **Breadth：** 为完成一次正确修改，能否可靠地找到所有必须同时考虑的代码和其他 Artifact，即完整的 Modification Closure。

Clean Architecture 通过分离 **Policy（What）** 与 **Mechanism（How）** 建立可靠的责任边界，使核心业务决策能够独立于具体实现机制被理解和修改。

#### 核心能力

- **DES-01 Context Cost Analysis：** 从当前 Change Spec 和已有代码库出发，分析哪些行为和规则需要改变、哪些必须保持不变；识别 Depth/Breadth 成本、现有边界能否可靠阻断遍历，以及 Modification Closure 是否完整可发现。
- **DES-02 Policy & Mechanism Separation：** 采用由内向外的设计顺序（**Entities → Use Cases → Ports → Adapters**）。Ports 契约由使用方需求定义，不照搬数据库、框架或外部 SDK 接口。
- **DES-03 Context Transformation：** 分析设计方案如何通过 **Reduce / Relocate / Index / Check** 四种变换改变 Context Cost；权衡新增的 Boundary Cost / Carrying Depth / Conceptual Surface，以及过早抽象（Wrong-shape Risk）与推迟设计（Unindexed-closure Risk）。
- **DES-04 Architecture as Context Routing：** 以 Use Case 确定业务行为责任归属，以 Layer 区分 Policy 与 Mechanism 位置，通过静态依赖检查守卫 Dependency Rule。Clean Architecture 并非所有系统的强制模板，最终选择仍由 CMP 决定。
- **DES-05 Executable Design & Implementation Planning：** **Code is the primary design artifact.** 优先将已确认的设计决策直接表达为 Domain Types、Entities、Use Cases、Ports、静态依赖约束、验证测试骨架及 Architecture Map 更新，并将剩余 Mechanism 实现分解为目标清晰的垂直切片任务。
- **DES-06 Semantic Comments & Context Indexing：** 注释是显式上下文载体，仅用于编程语言无法充分表达的两类信息：（1）**Semantic Contracts**（不变量、副作用、失败语义等局部推理契约）；（2）**Modification Closure Indexes**（无法由静态类型推导出的协同修改关联）。禁止复述代码行为或为架构捷径找借口的注释。

#### 输出

- 直接体现在代码结构中的核心设计契约（Domain Models、Use Cases、Ports、可执行约束）。
- 目标清晰的 Implementation Tasks。
- 必要的 Architecture Map 更新（或登记于 Merge Notes）。

**完成标准：** 产品行为的软件责任边界、核心契约与依赖约束已明确表达，且引入的 Context Cost 与其为当前及未来修改带来的收益相符。

---

### 5.4 Reflect — Post-Task Design Reflection & Knowledge Merge

**目标：利用每次软件修改留下的 Context Acquisition Trace，发现并修复 Context Routing 问题防止架构腐化，并将本次已实现的变化可靠合并回 Living Knowledge。**

#### 方法论

Reflect 基于 CMP 的 **Post-Task Design Reflection**，并辅以**旁路摩擦采集（Side-band Friction Tracking）**机制：在任务执行过程中静默记录搜索绕路、边界摩擦与孤儿能力信号，防止 Context Compaction 或跨会话切换导致一线执行轨迹丢失。

#### 核心能力

- **REF-01 Missing Path → Locality Repair：** 识别 Agent 难以发现的上下文、误导路径或断裂的 Modification Closure，通过改善索引、语义注释、引用关系或约束修复上下文发现路径。
- **REF-02 Unauthorized Shortcut → Boundary Guard：** 识别为了当前便利而绕过或破坏既有架构边界的实现（如 Policy 引入基础设施细节、越权访问内部状态），将修改恢复到既有边界内并强化机械约束。
- **REF-03 Unowned Capability → Detect & Escalate：** 识别缺乏明确责任归属而导致无法复用或产生重复实现的能力。能依据既有架构归属的直接归位；若反映出架构缺乏合理承载点，必须 **Flag & Stop** 交由人类与 Design 重新评估，禁止 Reflect 擅自新建抽象。
- **REF-04 Delta-to-Living Knowledge Merge：** 在变更实现并验证完成后，依据当前 Change 文件中的变更内容与 Merge Notes，将已确认落地的领域概念、产品行为与架构索引变更合并回 Living Knowledge，并将该 Change 标记为 `merged`。

#### 职责边界与人类架构师主权（Stay Out of the Architect's Chair）

Reflect 是范围刻意收窄的反馈与闭环机制，不是通用 Code Review 或自动架构重设计：

- **可自动处理（安全局部卫生）：** 更新知识索引、补齐语义注释/关闭修改索引、恢复明显越界的局部依赖、清理已消费的旁路摩擦日志。
- **必须 Flag & Stop（人类架构师拍板）：** 改动公共命名、挪动模块目录/边界、新建抽象或 Port、重划能力归属、以及将 Change 合并回 Living Knowledge 时的冲突裁决。

若本次轨迹未暴露有价值的设计问题，Reflect 不必强行提出改进。

---

## 6. Knowledge & Artifact Management

### 6.1 极简契约：总体 Index + 单文件 Change + 其余结构不限

cstack 不强行规定用户代码库的文档目录体系，但为避免 Agent 每次启动都陷入盲目搜索（自造 `Missing Path`），约定**两个极简锚点**（强默认 + 可覆写）：

1. **总体 `index` 文件（默认 `.cstack/index.md`）：** 作为整个知识库的 **Context Routing Index**，仅描述：
   - **Living Knowledge 地图：** 现有领域知识、产品行为、架构索引分别位于仓库何处（用户完全自由组织，如 `docs/`、`GLOSSARY.md`、代码注释、ADR 等，不做结构限制）。
   - **Changes 目录位置：** 默认 `.cstack/changes/`（可在 `index` 中指定其他路径）。
   - **Active Changes 列表：** 当前进行中的 Change 文件及其状态（`exploring` | `speccing` | `ready-for-design` | `merged`）。
2. **单文件 Change（`<changes-dir>/<change-id>.md`）：** 一次变更的所有增量内容（Problem & Proposed Solutions、Domain Delta、Product Design & Behavior Spec、Merge Notes）集中放在**一个 Markdown 文件**里，局部化上下文足迹，便于跨阶段往复、直接交给外部 Prototype 工具、以及最终统一合并。

### 6.2 两种生命周期

- **Living Knowledge：** 持续描述当前已落地的产品与系统状态（领域事实、当前产品行为、架构索引等）。**用户随时可以直接手工编辑 Living Knowledge**——这种直接编辑视为对“现状描述错误”的即时修正，无需走 Change 流程。
- **Change Knowledge：** 围绕一次具体变更形成的增量知识，完全隔离在对应的单文件 `changes/<change-id>.md` 中。

### 6.3 严格 Delta + Merge

所有由变更驱动的开发工作严格遵循 Delta + Merge：

1. **Read Current State：** 通过 `index` 读取当前 Living Knowledge 与相关代码。
2. **Discover & Define Delta：** 在 `changes/<change-id>.md` 中持续记录并打磨 Explore 与 Spec 增量（**变更实现前，不直接改动 Living Knowledge**，防止把计划中设计误当成当前事实）。
3. **Design & Implement：** 基于锁定的 Change Spec 完成软件设计与实现验证。
4. **Merge（挂载于 Reflect / 收尾阶段）：** 因为单次 Change 实现周期较短，验证完成后立即将已落地的变化 Merge 回 Living Knowledge，并更新 `index` 状态为 `merged`。

Greenfield 是初始 Living Knowledge 为空的特例，首次写入时懒创建极简 `index` 即可，无需独立流程。

### 6.4 Knowledge Ownership

| 知识 | 主要权威表达 |
|---|---|
| 知识库路由与变更索引 | `index` (`.cstack/index.md`) |
| 一次变更的增量探索与产品设计 | Single-File Change (`changes/<change-id>.md`) |
| 领域事实和概念（当前态） | Living Domain Knowledge（结构不限，在 `index` 登记） |
| 当前产品行为（当前态） | Living Behavior Knowledge（结构不限，在 `index` 登记） |
| 实现责任、类型与规则 | Code / Types / Tests |
| 架构定位与边界 | Architecture Map（结构不限，在 `index` 登记） |

其他 Artifact 可以引用这些知识，但不应独立维护重复定义。

### 6.5 Lightweight Traceability

通过轻量编号与引用保持关键追踪链：

**Problem & Proposed Solutions (`PS-*`) → Domain Delta → Product Spec & Solution Coverage → Implementation & Merge**

不建立重型需求数据库，仅对重要跨阶段依赖保留显式引用。

---

## 7. Interaction Model

### 7.1 独立 Skill + 灵活协作

核心工作模式对应 4 个主要 Skill（统一使用 `cs-` 前缀）：

- `Explore`（`cs-explore`）
- `Spec`（`cs-spec`）
- `Design`（`cs-design`，第二阶段）
- `Reflect`（`cs-reflect`，第三阶段）

Skill 之间不存在强制线性顺序；同时底层可共享只读上下文探索子 Agent（`cstack-explorer`）和旁路摩擦采集规则（`cstack-friction-tracker`），而不增加用户可见的命令负担。

### 7.2 Iterative Knowledge Refinement

- 当 Spec 发现领域知识不足时，可直接在同一 Change 文件内返回 Explore。
- 当 Design 发现行为规格不明确时，可返回 Spec。
- 当 Reflect 发现代码复杂性的根源在于产品设计时，可重新开启 Spec。
- 跨阶段反馈只修改受影响的知识与决策，不重跑全部工作。

### 7.3 AI Initiative & Human Authority

Agent 应主动开展知识发现和设计推理，而不是被动罗列空泛清单：

- **事实先行：** 优先通过 `index`、Living Knowledge 和代码库自行查证事实，不问代码能回答的问题。
- **前沿推进：** 按依赖关系组织决策树，每轮只抛当前 Frontier 问题，附带推荐答案与关键取舍（防范人类陷入无脑 `agreed` 的橡皮图章效应）。
- **实例检验：** 优先使用具体实例与反例检验规则与假设。
- **实时落盘：** 对话中每敲定一项事实或决策，立即写入当前 Change 文件。
- **显式确认：** 不得擅自把未确认假设升级为领域事实，也不得未经确认自动跨越阶段边界。

### 7.4 Respect Proposed Solutions

当客户直接提出解决方案时，Agent 应将其作为有效输入完整捕获（`PS-*`）。不强制客户改写为抽象问题，也不通过连续的“为什么”质疑其专业判断；应从方案中的具体行为、例外和风险入手发现隐藏规则。若最终产品设计偏离原始设想，必须在 Solution Coverage 中解释差异并取得确认。

---

## 8. End-to-End User Scenario

以设备安全评估产品为例。

- **用户输入：** “我们希望用户选择设备类型后，系统自动推荐适用标准，并据此生成评估任务。”
- **Explore（写入 `changes/auto-recommend-standards.md` 第 1–2 节）：** 读取 `index` 与已有知识后，完整捕获原案 `PS-1`；通过前沿审问与实例确认：设备存在不同类型与配置、标准适用性受条件约束、“适用标准”与“实际评估范围”是两个不同概念、评估任务需专业人员结合项目确认。
- **Spec（写入同一 Change 文件第 3 节）：** 基于领域事实设计新工作方式——用户输入设备类型与配置 → 系统识别候选标准及适用条件并解释依据 → 用户确认最终评估范围 → 系统生成评估任务。明确覆盖 `PS-1`（以“推荐+可解释+人工确认”替代全自动静默生成并说明理由），列出 `Out of Scope`；若交互展示形式存在体感不确定性，标为 `Prototype Candidate` 供用户拿该文件去外部原型工具验证后回写结论。
- **Design：** 读取已确认的 Change Spec 与现有代码库，基于 CMP 划定 Policy/UseCase/Port 契约与任务切片，交由 Agent 完成实现与验证。
- **Reflect：** 发现标准适用性判断在报价与评估两处重复实现（`Unowned Capability`），Flag & Stop 请架构师决策归属；同时将已落地的“适用标准 vs 评估范围”概念与行为变更 Merge 回 Living Knowledge，标记该 Change 为 `merged`。

---

## 9. Non-Goals

cstack 不计划：

- 构建新的通用 Coding Agent 或 Harness。
- 自行实现通用 TDD、Code Review、QA、CI/CD、Shipping 工作流。
- **内置 Prototype 工具或 UI 原型生成器**（Prototype 属于 Spec 的外部消费工具，用户可携带 Change Spec 对接外部工具，最终沉淀的仍是 Spec 本身）。
- 强制使用完整的 DDD、EventStorming、BDD 或 Clean Architecture 模板。
- 强制规定 Living Knowledge 的文件目录结构。
- 将领域知识完整复制到软件 Domain Model，或在文档中复述代码已表达的设计。
- 自动替用户作出重要的业务、产品或架构所有权决策。

---

## 10. Product Delivery

### 10.1 产品形态

cstack 作为可独立安装的跨 Harness 软件工程 Skills 集合分发（通过 `src/` 单源编译至 Cursor、Claude Code 及 Universal Agent Skills）。

### 10.2 底层共享原语

在不增加用户可见工作流命令的前提下，4 个核心 Skill 可共享两项底层基础设施：

- **只读上下文切片子 Agent（`cstack-explorer`）：** 吸收搜索噪声，仅向主上下文返回最小充分上下文切片。
- **旁路摩擦采集规则（`cstack-friction-tracker`）：** 在任务中途静默记录摩擦信号，保障 `Reflect` 跨上下文压缩不失忆。

### 10.3 版本规划

- **MVP — Explore + Spec + 极简知识库契约（当前阶段）**
  - 实现 `cs-explore`、`cs-spec` 以及共享的 `index` + 单文件 `changes/<change-id>.md` + 严格 Delta/Merge 契约。
  - 以 Matt `grill-with-docs` 的交互机制为起点，验证真实项目中的知识发现与产品规格质量。
- **第二阶段 — Design**
  - 基于 CMP / Clean Architecture 实现 `cs-design`，理顺与通用 Coding Agent 的契约交接。
- **第三阶段 — Reflect**
  - 实现 `cs-reflect`，串联旁路摩擦采集、三类 CMP 透镜与 Change-to-Living Knowledge 的 Merge 闭环。
- **第四阶段 — Integration**
  - 完善 4-Skill 协作体验、多 Harness 打包与长期连续修改实验验证。

---

## 11. Success Criteria

| 维度 | 评价重点 |
|---|---|
| Knowledge Discovery | 是否发现重要的隐藏约束、歧义和错误假设 |
| Cognitive Burden | 用户是否减少重复解释、信息整理和上下文重建 |
| Product Quality | 产品规格是否清晰、行为完整、原案覆盖可追溯、Out of Scope 明确 |
| Knowledge Consistency | 领域知识、产品行为和实现是否通过 Delta + Merge 保持一致 |
| Software Modifiability | 后续修改能否更便宜地获得充分上下文 |
| Design Evolution | 连续修改是否得到有效的设计反馈，避免架构持续退化 |

---

## 12. Open Questions（待 MVP 实测收敛）

1. **Explore 的知识形式化程度：** Change 文件里的 Domain Delta 保持纯语义 Markdown 还是局部使用表格/状态图，才能兼顾推理价值与低维护成本？
2. **Frontier 追问节奏：** 每轮抛出多少个前沿问题、推荐答案详略到什么程度，最能兼顾收敛效率与反橡皮图章效果？
3. **Spec 到外部 Prototype 工具的衔接：** Change Spec 第 3 节的哪些信息最能直接被外部原型工具高效消费，而不增加日常无原型任务时的书写负担？
4. **Merge 粒度与冲突体验：** 当用户在 Change 进行期间直接手工修正了 Living Knowledge，Reflect 做 Merge 时如何最轻量地检测并提示差异？
5. **效果验证方法：** 如何在真实连续修改实验中区分模型本身能力与 cstack 方法论带来的收益？

---

## 13. Product Vision

**Explore helps us understand the world.**  
**Spec helps us design a better way to work.**  
**Design makes that design executable and evolvable.**  
**Reflect keeps the software healthy as our understanding grows.**

最终目标是让人把注意力放在值得解决的问题和重要的设计判断上，让 AI 承担大量知识获取、组织、验证与持续维护工作。
