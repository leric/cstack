# CStack — Product Requirements Document

**Version:** 0.3  
**Status:** Active (Explore + Spec + Build + Reflect + Merge)  
**Category:** AI Software Engineering Skills  
**Tagline:** *Discover the domain. Design the product. Evolve the software.*

---

## 1. Product Overview

### 1.1 产品定位

cstack 是一套面向 AI Coding Agents 的软件工程 Skills，整合成熟的软件工程方法与 AI 驱动的工作流程，帮助开发者完成从业务知识发现、产品设计、软件设计到执行后反思重构与长期知识库治理的完整闭环。

cstack 不试图取代 Coding Agent，也不重新实现通用的编码、测试、Review、原型制作和交付工具。

它专注于软件开发中更依赖经验、判断力和知识治理的五个工程动作：

- **Explore：** 理解问题，发现并提取现实业务领域的知识。
- **Spec：** 根据产品目标创造性地设计软件系统的职责、工作方式与预期行为。
- **Build：** 运用 CMP 与按需加载的领域架构最佳实践 Case，在对话中完成软件结构规划、门控未决 *What-level* 决策，并直通执行由内向外的编码与验证。
- **Reflect：** 在编码刚完成、一线轨迹尚热时诊断结构摩擦、按授权就地执行重构、同步实践新知，并将热现场信号固化进 Change 文件（不直接污染主干知识库）。
- **Merge：** 在合入主干前后无状态核验最终代码差异，严格在触碰切片内对 Living Knowledge 执行增删改收敛（局部 GC），分流延后重构项并收口清理已完成 Change 文件。

五项能力组成一个可以往复迭代的工程系统，而不是僵硬死板的单向流水线。

### 1.2 核心理念

软件并非现实业务的直接数字化，而是为了特定目标，对现实世界进行有选择的抽象、建模和制度化。

开发软件需要完成三种不同的认知活动，并配套可持续的演化闭环：

1. **理解现实：** 发现业务中的概念、关系、规则、工作方式和真实问题。
2. **设计产品：** 根据产品目标重新组织工作、定义职责、设计行为，形成一种新的业务运行方式。
3. **构造与演进软件：** 将产品设计表达为可执行、可验证、可持续修改的软件结构，并在连续变更中同步治理代码结构与长期知识库，防止两者发生熵增腐化。

AI 的价值不仅是生成代码，更是承担知识搜索、组织、分析、比较、提取与局部垃圾回收工作，降低开发者在这些认知活动中的负担。

### 1.3 核心价值主张

**Make software engineering knowledge discoverable, actionable, and maintainable.**

cstack 帮助开发者：

- 发现过去依赖经验和隐性认知的领域知识。
- 将模糊的问题、既有解决方案与用户预期转化为明确的产品设计。
- 以统一的设计方法论（CMP）指导软件结构的构造与重构。
- 将一次开发工作中涌现的有价值真知可靠沉淀下来，同时及时剪枝失效、矛盾与已被代码内化的冗余文档，防止知识库自身膨胀成新的上下文负担。

---

## 2. Problem Statement

### 2.1 AI 降低了代码生产成本，却没有自动解决软件工程问题

Coding Agents 已经能够高效完成大量代码编写工作，但软件工程仍面临多种深层认知成本：

- 开发者需要花费大量时间理解业务和产品背景。
- 客户常常直接提出解决方案，而不是明确说明背后的问题。
- 领域知识散落在文档、代码、对话和专家经验中。
- 产品需求在开发中不断被重新理解和修正。
- 软件架构在连续修改过程中逐渐偏离原有设计。
- 关键实践认知随着任务结束和 Agent Context 清空而蒸发，导致人类负责人的心智模型与真实代码脱节。
- 即便维护项目文档，也很容易陷入“只增不减”的熵增陷阱——过时防呆提示、互相矛盾的规则和层层打补丁的特例不断堆积，形成误导后续 Agent 的上下文噪音（Slop）。

这些问题不能仅靠提高代码生成能力解决。

### 2.2 现有 Skills 的不足

pstack、gstack、Matt Pocock Skills（`grill-me` / `grill-with-docs` / `domain-modeling` / `to-spec`）等项目已经提供了大量值得借鉴的成熟实践，包括 Grilling、Domain Modeling、Product Planning、Architecture、Verification 和 Reflection。

但仍存在可以进一步深化的空间：

- **知识发现：** 如何系统地发现领域概念、关系、约束与隐藏假设，并妥善对待客户直接提出的方案，而不是简单收集需求？
- **产品设计：** 如何从现实业务和客户提出的方案出发，创造新的工作方式，而不是直接复制现有流程？
- **软件设计：** 如何用统一的判断依据（Context Cost）比较架构方案、设计边界，并控制未来修改成本？
- **诊断与沉淀解耦及知识库局部 GC：** 如何把“趁执行轨迹还热做代码设计诊断与重构”同“合入主干时的知识库增删改治理”正交解耦，既打破对单次聊天会话的绑定，又让 Living Knowledge 具备减法与收敛能力？

cstack 吸收现有实践（尤其是 `grill-with-docs` 的决策树前沿审问与实时落盘机制），并使用 Ontology、CMP 与 Reflective Bootstrapping 建立贯穿全生命周期的工程框架。

---

## 3. Target Users

### 3.1 主要用户

**具备软件工程经验、使用 Coding Agent 开发真实产品的开发者。**

典型特征：

- 独立开发者或小型产品团队。
- 参与业务需求分析和产品设计。
- 使用 Cursor、Claude Code、Codex、Antigravity 等 Agent 开发工具。
- 需要长期维护产品，而不是只生成一次性 Demo。
- 希望将自己的工程判断力与 Agent 的执行能力结合起来。

### 3.2 典型使用场景

- **Greenfield Development：** 从一个产品想法出发，探索业务领域、明确目标、设计产品并建立初始软件架构。
- **Brownfield Development：** 在已有产品中增加功能或调整业务规则，利用现有领域知识和产品规格完成增量设计与修改。
- **Continuous Evolution：** 在日常开发过程中，根据新的业务认知、一线实现摩擦和最终合入变更，持续演进软件设计并对长期知识库做局部垃圾回收。

### 3.3 非目标用户

cstack 不以完全不了解软件工程的用户一键生成完整产品为主要目标。

它的设计假设是：**AI 负责执行大量知识劳动，人仍然对关键的业务判断、产品选择和工程决策负责。**

---

## 4. Product Philosophy

### P1. Knowledge belongs where it is best expressed

知识应该保存在最适合表达它的载体中，而不是在不同开发阶段重复描述。

这是一种覆盖代码、文档和整个工程过程的 DRY 原则：领域知识适合通过概念、关系和规则表达；产品设计适合通过行为规格表达；软件设计优先通过代码、类型、接口、测试及架构索引表达。**一旦某条自然语言防呆规则已被重构固化进代码、类型或测试，该文档描述就应及时剪枝退场。**

### P2. Discover before assuming

Agent 应主动识别未知知识和隐性假设。

客户提出的解决方案既是有效的需求输入，也是发现业务知识的重要线索。不能盲目执行，也不应习惯性否定。

### P3. Model the domain, design the product

领域建模追求对现实业务的忠实理解；产品设计则是一项创造性工作，可以改变既有业务的角色分工、操作方式和工作流程。领域专家是业务知识的重要来源，但不必是产品设计的最终决策者。

### P4. Design for modification

软件开发是一系列连续的 Modification。运用 Context Minimization Principle（CMP），使未来正确修改软件所需的充分上下文更容易获取。架构原则是实现目标的手段，不是必须遵守的教条。

### P5. Strict Delta + Merge with Local GC

从 Greenfield 到 Brownfield，软件和产品知识始终处于持续修改过程中：

- 所有围绕在途变更产生的探索、规格、重构提案与热轨迹信号都隔离在单次 Change 文件中（合入前绝不提前污染 Living Knowledge）。
- 合入主干时对照最终代码执行局部垃圾回收（`ADD` / `RESOLVE` / `COMPRESS` / `PRUNE`），防止知识库无限膨胀，并在收口后清理使命完成的 Change 文件以消除搜索噪音。
- 同时始终允许用户直接手工编辑 Living Knowledge 以即时修正对现状的错误描述。

### P6. Skills are modular and iterative, not a rigid pipeline

Explore、Spec、Build、Reflect 和 Merge 是具有清晰职责边界的工作模式（其中 Reflect 与 Merge 通过 Change 文件解耦会话状态），新的发现随时可以使上游阶段重新开启。

### P7. Minimize human cognitive burden

Agent 应尽可能自行完成信息搜索、分析、归纳、一致性核验与局部 GC 提案。只有真正影响业务事实、产品决策、架构归属或知识裁决的关键问题，才需要寻求人类判断。

---

## 5. Core Product Capabilities

### 5.1 Explore — Problem & Domain Discovery

**目标：理解用户问题及其所在的现实业务领域。**

#### 方法论

以 Matt Pocock 的 `grill-with-docs`（决策树前沿审问 + 事实自查 + 实时落盘）为交互起点，融合 Ontology、DDD Strategic Design、Ubiquitous Language、EventStorming 的有效思想。不要求完整使用任何特定框架或术语。

#### 核心能力

- **EXP-01 Problem Discovery：** 识别用户面临的问题、目标、利益相关者、现有解决方式和约束（若输入横跨多个松耦合问题域，可引导聚焦单一子域，将剩余子问题暂存为 `DT-*` 供收口后开启新探索；一次 Explore 仍只维护一个 Change 文件）。
- **EXP-02 Domain Knowledge Discovery：** 发现领域中的核心概念、关系、业务规则、事件、状态和工作方式；与领域专家或可信材料确认重要的领域事实。
- **EXP-03 Proposed Solution Capture：** 完整记录客户提出的解决方案（编号记录 `PS-*`），包括功能要求、工作流程和操作方式；理解方案背后的业务动机，不默认它必须原样实现，也不居高临下地否定。
- **EXP-04 Knowledge Gap Detection & Frontier Grilling：** 优先通过已有知识库和代码库自行解决事实问题；将未决的领域缺口、术语冲突和隐藏假设组织成决策树，按前沿（Frontier）分轮追问，每题附带推荐答案与依据。
- **EXP-05 Interactive Knowledge Refinement：** 通过具体实例、反例和边界情况验证领域模型；随着沟通进展实时更新当前 Change 文件第 1–2 节。

#### 输出知识（写入当前 Change 文件 Sections 1–2）

- Problem Definition, Stakeholders & Proposed Solutions (`PS-*`)
- Domain Concepts, Relationships, Business Rules, Existing Workflows
- Confirmed Facts vs. Open Questions & Assumptions

**完成标准：** 当前产品设计所依赖的重要问题、领域事实和约束已得到足够理解，剩余不确定性明确可见，且与人类达成明确确认。

---

### 5.2 Spec — Product Design & Behavior Specification

**目标：基于领域知识与产品目标，创造性地设计软件介入之后的工作系统与外部行为。**

#### 方法论

以 `grill-with-docs` 的决策树审问与 `to-spec` 的外部行为导向、显式拒绝清单（Out of Scope）为基础，结合 Product Design、BDD、Example Mapping 和业务流程设计。

#### 核心能力

- **SPEC-01 Product Model Design：** 根据产品目标选择和组织领域概念，构建产品自身的概念模型；允许引入并不存在于原有业务中的产品抽象。
- **SPEC-02 Responsibility & Workflow Design：** 设计软件、用户、Agent 和其他参与者之间的职责与协作方式，定义目标工作流程、交互行为、决策点和权限约束。
- **SPEC-03 Behavior, UX & Visual Specification + Out of Scope：** 明确外部可观测功能行为、业务规则、状态转换、异常情况与验收条件；针对前端界面提供两个正交的按需加载场景——**`interactive-ux`（好不好用）** 在所有界面功能中审视空间连续性、乐观反馈/撤销体验、渐进式披露、全状态矩阵与跨端接口诉求，**`visual-design`（好不好看）** 仅在 0→1 新产品、视觉改版或品牌展示页中加载以按产品属性校准审美调性与信息密度、锁定四大视觉支柱并建立反样式 Slop 护栏（成熟存量项目日常迭代默认不加载，直接继承 `DESIGN.md` / Token）；同时明确列出 **Out of Scope（显式拒绝清单）**，防止范围膨胀。
- **SPEC-04 Solution Coverage：** 逐条对照 Explore 阶段记录的客户原案（`PS-*`），明确每一项的处理方式（原样满足 / 替代方案达成动机 / 协商调整或取消 / 明确归入后续切片或 `DT-*` / 尚未解决），绝不静默丢弃重要诉求。
- **SPEC-05 Product Decisions, HTML Wireframes & Style Tiles：** 对候选方案进行分析和比较；当文字难以表达布局、交互或视觉调性时，用单个可直接打开的临时 HTML 辅助拍板（视觉定调时采用原生 CSS 变量 + 真实业务数据的 Style Tile，杜绝 Bootstrap 模板污染审美；存量交互流采用贴合既有组件语汇的轻量草图），确认后将行为、视觉支柱及理由写回 Change Spec。
- **SPEC-06 Brownfield Fast-Path：** 对于小规模增量变更，允许直接从 `/cs-spec` 进入并内联完成轻量知识缺口检查；仅当发现深层领域冲突时建议切回 `/cs-explore`。（若设计方案时发现改动超出单次 `/cs-build` 的合理规模，可按需升级为多切片目录 `<topic-id>/` 并按切片即时展开 Spec，详见 6.1。）

#### 输出知识（写入当前 Change 文件 Section 3）

- Product Model, Role Division & Target Workflows
- Behavior Specifications & Acceptance Scenarios
- Solution Coverage Table (`PS-*`), Product Decisions & Out of Scope
*(注：Spec 阶段不提前规划文档目录或更新目标，避免增加冗余负担；Living Knowledge 的目标路由统一由 `/cs-merge` 负责。)*

**完成标准：** 产品外部行为足够明确以驱动软件设计；所有重要客户原案均有明确去向；关键产品决策与 Out of Scope 获人类确认。

---

### 5.3 Build — CMP-Driven Software Construction & Verification

**目标：运用普适的 CMP 内核与按需加载的领域架构最佳实践 Case，将产品行为（或已批准的延后重构提案）转化为清晰的软件结构计划，门控未决 *What-level* 分歧，并直通完成由内向外的编码实现与归属层验证，使系统在未来持续修改中保持低 Context Cost。**

#### 方法论

以 **Context Minimization Principle（CMP）** 为跨领域统一评价框架（沿 **Depth** 与 **Breadth / Modification Closure** 两个维度衡量上下文获取成本，通过 **Reduce / Relocate / Index / Check** 四种变换优化结构），采用 **“CMP 元内核 + 按场景按需加载具象架构 Case”** 的模块化组织方式（避免用空泛抽象词稀释工程约束）：
- 服务端/领域模块加载 **Clean Architecture Case**（`Entities → Use Cases → Ports → Adapters`）；
- 前端/富交互模块加载 **Frontend Architecture Case**（垂直特性内聚、五类单一状态真源归属、DTO 隔离与缓存失效闭包、无业务感知 UI 原语、真实浏览器闭环验证）；
- 跨前后端全栈变更同时加载双端 Case 并激活 **Cross-Boundary Seam（跨端接缝规则）**（锁定唯一契约锚点、服务端权威策略与客户端可操作性投射、体验驱动接口语义、契约铰链式双端由内向外推进）；
- `Architecture Map` 按仓库物理拓扑决定拆分粒度（前后端分离/Monorepo 维护两份专属地图 + 跨端契约指针；紧凑全栈单体先维护一份分节地图，待触发粗粒度路由噪音提醒时再拆分）。

#### 核心能力

- **BLD-01 Context Cost Analysis & Case Routing：** 分析哪些行为改变、哪些保持不变，按触碰技术面按需挂载具象架构 Case（支持未来继续扩展数据流水线、Agent 系统、SDK 等领域 Case），评估 Depth/Breadth 成本与现有边界可信度。
- **BLD-02 Policy, State & Mechanism Separation：** 采用各领域原生的由内向外构造顺序（后端分离领域/用例策略与基础设施机制；前端明确单一状态宿主、分离纯计算/状态机与视图原语；全栈先锁跨端契约与领域策略）。
- **BLD-03 Context Transformation：** 权衡 Boundary Cost / Carrying Depth，平衡过早抽象（Wrong-shape Risk）与散落失联（Unindexed-closure Risk）。
- **BLD-04 Architecture as Context Routing：** 明确能力垂直归属、状态真源与水平边界，守卫依赖方向与跨端契约闭包。
- **BLD-05 Gate-Driven Plan & Direct Execution：** 在对话中输出 Quick 或 Full 构造计划（不制造冗余设计文档）；存在未决 What-level 分歧时暂停请人类拍板，无门控时直接由内向外完成编码与归属层验证（涉及 UI 布局/样式/运行时必须包含真实浏览器核验）。
- **BLD-06 Semantic Comments & Context Indexing：** 注释仅承载语言无法直接表达的 **Semantic Contracts** 与 **Modification Closure Indexes**，严禁复述代码或为架构捷径找借口。

**完成标准：** 软件责任边界、核心契约与依赖约束明确表达，且引入的 Context Cost 与其为当前及未来修改带来的收益相符。

---

### 5.4 Reflect — Post-Task Design Reflection, Refactoring & Trace Capture

**目标：在编码刚完成、一线执行轨迹（Live Implementation Trace）尚热时，诊断代码结构摩擦、按用户授权就地执行重构、完成人机双向认知同步，并将单凭 `Sections 1–3 + git diff` 无法看到的易失线索固化到 Change 文件 Section 4。**

#### 为什么与 Merge 正交解耦

Reflect 面向的是**分支内的代码结构健康与执行期认知同步**；而 Merge 面向的是**主干真相（Canonical Truth）的融合与生命周期治理**。将两者正交拆分并以 Change 文件 Section 4 作为无状态契约，本质上是把 **CMP 应用到了 cstack 自身的上下文分配上**：

1. **主上下文与认知负担的均衡切分：** `/cs-reflect` 以**执行轨迹（Trajectory）+ 代码**为主上下文，无需分心去通读和编排存量文档；`/cs-merge` 则甩掉了冗长嘈杂的编码过程轨迹，以**触碰切片内的 Living Knowledge + Change 文件 + 最终 Diff** 为主上下文，专注思考如何把新认知紧凑融合进去并做减法。人类审阅时也不必在“代码结构好不好”和“长期文档怎么措辞”两种思维模式间来回切换。
2. **保证代码与主干知识的原子性：** PR 在合入前随时可能返工或废弃，提前修改 Living Knowledge（尤其是因重构内化约束而删文档）会造成主干失真。
3. **打破单次聊天会话依赖与防范围蔓延：** `/cs-reflect` 在编码刚结束时跑，真正的合入可能隔了数天、换了全新会话；同时 Section 4.1 让人既可在 `/cs-reflect` 当场执行重构（`fixed-in-branch`），也可冷静标为 `defer-to-followup` 避免当前 PR 失控膨胀。

#### 核心能力

- **REF-01 Missing Path / Boundary Bypass / Unowned Capability 诊断：** 基于当前会话的 Live Task Context（辅以 `.cstack/friction.log` 防上下文压缩丢失），筛出高信噪比的上下文路由断裂、边界旁路与无主能力。
- **REF-02 Structured Refactor Proposals & In-Branch Execution (`[R-*]`)：** 对每个实质性问题起草结构化重构方案（症状证据、CMP 根因、目标边界与改动面、`how-level | what-level` 级别）；经人类拍板后，**直接在 `/cs-reflect` 环节执行批准的重构并运行验证**（标为 `fixed-in-branch`），或按决策标为 `defer-to-followup` / `wont-fix`。
- **REF-03 Two-Way Human Knowledge Sync：** 将实现中撞见的隐性业务约束、工具前提或预期偏差显式反馈给人类，防止人类心智模型与真实代码脱节。
- **REF-04 Ephemeral Trace Clue Extraction（写入 Change Section 4）：** 不在 Reflect 阶段提前规划文档布局，仅趁上下文清空前补齐 `Sections 1–3 + git diff` 看不见的易失线索——`4.2 Touched Knowledge Slice`（`Read & Used`、`Contradicted / Stale`、`Read & Unused`）与 `4.3 Trace Clues`（编码期对 Spec 的合理微调与隐性 *Why*、实战证伪的旧声明、已被代码/测试/封装内化的防呆规则、零散特例聚集点及新建架构边界）。Fast Path 无遗留项时保持零文件开销；**严格禁止直接修改任何 Living Knowledge 文件。**

---

### 5.5 Merge — Diff-Verified Knowledge Governance & Local GC

**目标：在变更准备合入或刚合入主干时，唯一负责将变更映射至 `.cstack/index.md` 中的目标文档，无状态核验最终代码差异，严格在触碰切片内对 Living Knowledge 执行增删改收敛（局部垃圾回收），确认延后重构项去向，并清理已完成使命的 Change 文件。**

#### 核心设计原则

1. **以最终代码为事实基线（Diff-Verified Truth）：** PR 在 Reflect 后可能历经多轮 Review 调整。`/cs-merge` 必须将 Change 文件 Sections 1–4 与最终 `git diff` 及当前代码/测试交叉核对——尤其是因“代码内化”提议的剪枝，必须确认约束确实已落进最终代码或测试后才允许删减文档。
2. **严格局部 GC（Touched-Slice Only）：** 仅审计本次 Change 关联及 `4.2 Touched Knowledge Slice` 涉及的文档段落，严禁全库扫描 Living Knowledge，使高频演化区域自然越磨越干净，同时零噪音对待冷门区域。
3. **减法优先与确认护栏：** 鼓励把自然语言防呆规则下沉到代码/类型/测试并清理旧文档；对无法直接证明已失效的其他未使用上下文，先提出提醒、经用户确认后再改，杜绝误删对其他特性仍有效的领域知识。

#### 核心能力

- **MRG-01 Stateless Target Mapping & Reconciliation：** 仅凭 `.cstack/index.md`、`<changes-dir>/<change-id>.md` 与最终 `git diff` 无状态定位目标文档并完成核验（若用户跳过了 `/cs-reflect`，自动退化为基于 Sections 1–3 + `friction.log` + 最终 Diff 做轻量同步），同时检测期间人类对 Living Knowledge 的直接手工编辑以避免静默覆盖。
- **MRG-02 Four Governance Actions + Confirm-First Reminders：**
  - **`ADD`（补充）：** 写入已落地的新领域概念、外部产品行为、架构索引路由及非显而易见的 *Why*（严禁复述代码签名与 Diff 流水账）。
  - **`RESOLVE`（消歧）：** 并排列出“现有文档声明 vs. 最终代码/新现实（含人类手工编辑差异）”及代码证据，请人类拍板裁决。
  - **`COMPRESS`（收敛与下沉）：** 当触碰章节积累了 3 条以上零散特例或补丁式说明时，重写为紧凑的当前态规则/表格，并提示可下沉为静态检查或测试的条目。
  - **`PRUNE`（验证后剪枝）：** 对经最终代码核实属于**失效（obsolete）**、**矛盾淘汰（contradicted）**或**已被代码/类型/测试内化（internalized）**的旧提示与过时段落，直接提议删除。
  - **`REMIND`（提醒确认）：** 对其他观察信号（如 `4.2 Read & Unused` 中未证实失效、仅因 `index.md` 路由粒度过粗而被连带读取的有效文档），不自动归入删除，而是先作为提醒展示选项（保持不动 / 收紧 `index.md` 路由指针 / 删改），**仅在用户明确确认后才执行修改**。
- **MRG-03 Deferred Item Routing：** 扫描 Section `4.1` 中未闭环的延后重构项（`[R-*]`），以及（在单文件或最终切片收口时）由 Explore 暂存的跨问题子域（`[DT-*]`），在清理 Change 产物前强制确认去向（新建后续 Change stub、记入 Architecture Map、转外部 Issue 或显式关闭），确保架构债与暂存问题绝不静默丢失。
- **MRG-04 Clean Lifecycle Closeout：** 执行获批的 Living Knowledge 与 `.cstack/index.md` 更新，默认删除已完成的 `<changes-dir>/<change-id>.md`（多切片主题合入中间切片时更新 `overview.md` 并仅删除当前切片文件，最终切片合入后删除整个 `<topic-id>/` 目录），依托 Git 历史留痕，并清空已消费的 `.cstack/friction.log` 记录。

---

## 6. Knowledge & Artifact Management

### 6.1 极简契约：总体 Index + 单文件在途 Change（大方案按需升级）+ 其余结构不限

cstack 不强行规定用户代码库的文档目录体系，仅约定两个极简锚点（强默认 + 可覆写）：

1. **总体 `index` 文件（默认 `.cstack/index.md`）：** 作为整个知识库的 **Context Routing Index**，仅描述：
   - **Living Knowledge 地图：** 现有领域知识、产品行为、架构索引分别位于仓库何处（用户完全自由组织，如 `docs/`、`GLOSSARY.md`、代码注释、ADR 等）。
   - **Changes 目录位置：** 默认 `.cstack/changes/`。
   - **Active Changes 列表：** 当前在途的 Change 文件及其状态（`exploring` | `speccing` | `ready-to-build` | `reflected`；经 `/cs-merge` 收口后从活跃列表移除）。当大方案升级为多切片目录时，采用缩进层级登记父目录 `changes/<topic-id>/` 与其下活跃切片，确保切片衔接空档期父主题依然可见。
2. **在途 Change 契约（默认单文件，大方案按需升级）：**
   - **默认单文件（`<changes-dir>/<change-id>.md`）：** 常规变更从探索到合入前的全部增量契约集中在**单个 Markdown 文件**的 4 个章节中：
     - `1. Problem & Proposed Solutions`（含可选 `DT-*` 暂存子问题）& `2. Domain Delta`（`/cs-explore`）
     - `3. Product Design & Behavior Spec`（`/cs-spec`）
     - `4. Reflection & Merge Handoff`（`/cs-reflect`：含 `4.1 Design Friction & Refactor Proposals`、`4.2 Touched Knowledge Slice`、`4.3 Trace Clues for /cs-merge`）
   - **多切片主题目录（按需升级 `<changes-dir>/<topic-id>/`）：** 仅当 `/cs-spec` 发现单次交付过大时按需升级，由 `overview.md`（共享 Sections 1–2、完整 `PS-*` 总账、`DT-*` 与薄 `Slice Map`）和按需实例化的当前切片 `<NN>-<slice-id>.md`（Sections 3–4）组成。

### 6.2 两种生命周期

- **Living Knowledge（长期主干真相）：** 持续描述当前已合入主干的产品与系统状态，保持紧凑的当前态表达。用户随时可以直接手工编辑 Living Knowledge 以即时修正对现状的错误描述。
- **Change Knowledge（在途增量契约）：** 围绕一次具体变更形成的临时增量与跨会话交接状态，在 `/cs-merge` 完成沉淀与延后项分流后即行删除退场（由 Git 历史留痕）。

### 6.3 严格 Delta + Reflect + Merge 闭环

1. **Read Current State：** 通过 `index` 读取当前 Living Knowledge 与相关代码。
2. **Discover & Define Delta：** 在 `changes/<change-id>.md` 第 1–3 节中持续打磨 Explore 与 Spec 增量（实现合入前绝不改动 Living Knowledge）。
3. **Build & Verify（门控驱动构造）：** 运行 `/cs-build` 基于锁定的 Change Spec 做对话内 CMP 结构规划，解决未决 *What-level* 分歧后直通完成由内向外的编码与验证。
4. **Reflect & Refactor（趁轨迹尚热）：** 运行 `/cs-reflect` 诊断摩擦、按授权就地重构，并将重构记录与单靠 Diff 看不见的易失线索写入第 4 节。
5. **Merge & Local GC（合入收口）：** 运行 `/cs-merge` 映射目标文档并对照最终 Diff 在触碰切片内执行 `ADD` / `RESOLVE` / `COMPRESS` / `PRUNE`（及确认后的提醒调整），分流未执行的延后重构项，并清理 Change 文件与已消费摩擦日志。

### 6.4 Knowledge Ownership

| 知识 | 主要权威表达 |
|---|---|
| 知识库路由与在途变更索引 | `index` (`.cstack/index.md`) |
| 一次在途变更的探索、规格与反思交接契约 | Single-File Change (`changes/<change-id>.md`, Sections 1–4) 或按需升级的多切片目录 (`changes/<topic-id>/overview.md` + `<NN>-<slice-id>.md`) |
| 领域事实和概念（当前态） | Living Domain Knowledge（结构不限，在 `index` 登记） |
| 当前产品行为（当前态） | Living Behavior Knowledge（结构不限，在 `index` 登记） |
| 实现责任、类型、不变量与防呆约束 | Code / Types / Tests / Semantic Comments |
| 架构定位、边界契约与已知边界债 | Architecture Map（结构不限，在 `index` 登记） |

### 6.5 Lightweight Traceability

通过轻量编号与章节契约保持关键追踪链：

**Problem & Proposed Solutions (`PS-*`) → Domain Delta → Product Spec & Solution Coverage → Implementation & Refactor (`[R-*]`) → Diff-Verified Governance (`[M-*]`)**

---

## 7. Interaction Model

### 7.1 五个独立 Skill + 共享底层原语

核心工作模式对应 5 个主要 Skill（统一使用 `cs-` 前缀）：

- `Explore`（`cs-explore`）
- `Spec`（`cs-spec`）
- `Build`（`cs-build`）
- `Reflect`（`cs-reflect`）
- `Merge`（`cs-merge`）

底层共享两项可选基础设施而不增加命令负担：只读上下文切片子 Agent（`cstack-explorer`）和防压缩旁路摩擦采集规则（`cstack-friction-tracker`）。

### 7.2 Iterative Knowledge Refinement

- 当 Spec 发现领域知识不足时，可直接在同一 Change 文件内返回 Explore。
- 当 Build 发现行为规格不明确时，可返回 Spec；当针对延后重构项启动下一轮改造时，可直接读取相应重构提案进入 Build。
- 当 Reflect 发现结构扭曲源于产品或领域模型缺陷时，可重新开启 Spec 或 Explore；对于批准在本分支解决的设计摩擦，直接在 Reflect 内完成重构验证。
- 跨阶段反馈只修改受影响的知识与决策，保留其他已有章节，不重跑全部工作。

### 7.3 AI Initiative & Human Authority

- **事实先行：** 优先通过 `index`、Living Knowledge 和代码库自行查证事实，不问代码能回答的问题。
- **前沿推进：** 按依赖关系组织决策树，每轮只抛当前 Frontier 问题，附带推荐答案与关键取舍。
- **实例检验：** 优先使用具体实例与反例检验规则与假设。
- **实时落盘与授权写入：** Explore/Spec 对话敲定的增量实时更新至 Change 文件；Reflect 的代码重构与 Section 4 落盘、以及 Merge 对主干 Living Knowledge 的治理与文件清理，均默认先输出报告/清单，获人类明确确认后方可执行。

### 7.4 Respect Proposed Solutions

当客户直接提出解决方案时，Agent 将其作为高信号输入完整捕获（`PS-*`），从具体场景、例外和风险入手发现隐藏规则；若最终产品设计偏离原始设想，必须在 Solution Coverage 中解释差异并取得确认。

---

## 8. End-to-End User Scenario

以设备安全评估产品为例：

- **用户输入：** “我们希望用户选择设备类型后，系统自动推荐适用标准，并据此生成评估任务。”
- **Explore（写入 `changes/auto-recommend-standards.md` 第 1–2 节）：** 读取 `index` 与已有知识后完整捕获原案 `PS-1`；通过前沿审问确认“适用标准”与“实际评估范围”是两个不同概念，评估任务需专业人员结合项目确认。
- **Spec（写入同一文件第 3 节）：** 设计“推荐候选标准并解释依据 → 用户确认评估范围 → 生成评估任务”的工作流，覆盖 `PS-1` 并列出 `Out of Scope`（无需提前规划文档修改目标）。
- **Build：** 运行 `/cs-build` 基于 CMP 划定 Policy/UseCase/Port 契约与由内向外的实现切片，无未决门控时直接完成编码与测试验证。
- **Reflect（趁轨迹尚热，写入第 4 节）：** 发现标准适用性判断在报价与评估两处重复（`Unowned Capability`），起草 `[R-1]` 重构方案；经架构师拍板归属后在分支内直接完成抽取重构并标为 `fixed-in-branch`，同时将旧文档中已被新类型约束内化的手工同步提示记入 Section 4.3 的 `Code-Internalized Guardrails`。
- **Merge（合入主干前后）：** 在新会话中通过 `.cstack/index.md` 定位目标文档并无状态核验最终 `git diff`，在触碰切片内将“适用标准 vs 评估范围”正式 `ADD` 入领域文档、把已被代码内化的旧防呆提示 `PRUNE` 删除，更新 `.cstack/index.md` 并删除已完成使命的 `changes/auto-recommend-standards.md`。

---

## 9. Non-Goals

cstack 不计划：

- 构建新的通用 Coding Agent 或 Harness。
- 自行实现通用 TDD、Code Review、QA、CI/CD、Shipping 工作流。
- 内置重型专用原型工具。
- 强制使用完整的 DDD、EventStorming、BDD 或 Clean Architecture 模板。
- 强制规定 Living Knowledge 的文件目录结构，或每次对全量文档做耗时的全局扫描。
- 在文档中复述代码已清晰表达的实现细节，或未经确认擅自作出业务、产品、架构归属与知识删改决策。

---

## 10. Product Delivery

### 10.1 产品形态

cstack 作为遵循开放 `SKILL.md` 标准的跨 Harness 软件工程 Skills 集合分发（支持 `npx skills add leric/cstack` 或直接复制安装至 Claude Code、Cursor、Codex、Antigravity 等环境）。

### 10.2 底层共享原语

在不增加用户可见工作流命令的前提下，5 个核心 Skill 共享两项可选原语：

- **只读上下文切片子 Agent（`cstack-explorer`）：** 吸收搜索噪声，仅向主上下文返回最小充分上下文切片。
- **旁路摩擦采集规则（`cstack-friction-tracker`）：** 在任务中途静默记录摩擦信号，由 `/cs-reflect` 提炼进 Change Section 4，并在 `/cs-merge` 收口时清理。

### 10.3 演进里程碑

- **Phase 1 — Explore + Spec：** 建立基于前沿审问、原案追踪（`PS-*`）与单文件 Change 的领域发现及行为规格机制。
- **Phase 2 — Build：** 基于 CMP 与按需领域架构 Case 建立门控驱动的由内向外软件构造与验证机制。
- **Phase 3 — Reflect：** 串联旁路摩擦采集、三类 CMP 检查、就地授权重构与人机双向认知同步。
- **Phase 4 — Merge & Local GC（当前阶段）：** 将主干知识治理拆分为独立无状态的 `cs-merge`，通过 `ADD / RESOLVE / COMPRESS / PRUNE` + 提醒确认机制赋予 Living Knowledge 自我净化与抗熵增能力。

---

## 11. Success Criteria

| 维度 | 评价重点 |
|---|---|
| Knowledge Discovery | 是否发现重要的隐藏约束、歧义和错误假设 |
| Cognitive Burden | 用户是否减少重复解释、信息整理和上下文重建 |
| Product Quality | 产品规格是否清晰、行为完整、原案覆盖可追溯、Out of Scope 明确 |
| Knowledge Hygiene | Living Knowledge 是否通过局部 GC 保持精炼准确，避免退化为只增不减的陈旧噪音 |
| Software Modifiability | 后续修改能否以更低上下文成本获取完整的 Modification Closure |
| Design Evolution | 连续修改中暴露的边界摩擦是否通过就地重构或可追踪的延后路由得到闭环治理 |

---

## 12. Open Questions（持续实测调优）

1. **Explore 的知识形式化程度：** Change 文件里的 Domain Delta 保持纯语义 Markdown 还是局部使用表格/状态图，才能兼顾推理价值与低维护成本？
2. **Frontier 追问节奏：** 每轮抛出多少个前沿问题、推荐答案详略到什么程度，最能兼顾收敛效率与反橡皮图章效果？
3. **HTML 草图的使用时机：** 哪类布局、交互和状态变化问题值得在讨论中画出来？用多少细节就能帮助用户作出当前决策？
4. **局部 GC 信号灵敏度：** `COMPRESS`（如同一章节积累 3 条以上特例）与 `REMIND`（索引过粗导致零收益读取）在真实大型仓库长期迭代中的最佳触发阈值是什么？
5. **效果验证方法：** 如何在真实多轮连续修改基准（如类似 SlopCodeBench 的演进场景）中量化 cstack 对架构复杂度与知识库信噪比的改善？

---

## 13. Product Vision

**Explore helps us understand the world.**  
**Spec helps us design a better way to work.**  
**Build turns that product spec into evolvable software.**  
**Reflect heals structural friction while practice is still fresh.**  
**Merge keeps canonical knowledge lean, true, and free of decay.**

最终目标是让人把注意力放在值得解决的问题和重要的设计判断上，让 AI 承担大量知识获取、组织、验证与持续治理工作。
