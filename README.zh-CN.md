# `cstack`

[English](README.md) | **简体中文**

`cstack`是一套轻量级的**4个AI软件工程Skills**，构建在**Domain Ontology**、**[上下文最小化原则（CMP）](https://www.contextcost.dev/cmp/)**之上。遵循开放的Agent Skills（`SKILL.md`）标准，可在你的Coding Agent中开箱即用。

---

## AI 编程真正棘手的问题

有了Coding Agent，生成实现代码、补齐测试和做局部重构正变得近乎免费。但复杂软件系统的演进并没有因此变简单。当代码本身变得极其廉价之后，**编码不再是核心难题，真正棘手的是剩下的三个深层问题**：

### 1. 现实建模（以10倍速把错误的东西做出来）
在真实的存量系统（Brownfield）中，需求很少是干净清晰的。领域概念会漂移，同一个词在不同模块里含义打架，旧代码里往往还固化着早已过时的业务约束。当实现成本趋近于零时，概念模糊的代价反而被无限放大：勤快的Coding Agent会忠实地在过时抽象上打补丁、延续死去的假设，迅速交付一套看似工整、实则建立在错误心智模型上的代码。

### 2. 复杂度复利（快进版的架构腐化）
廉价代码会加速系统熵增。每一次局部看起来都合理的快速改动，都在悄悄堆叠特例分支、绕开既有边界，或让旧流程与新实现半死不活地长期并存。久而久之，**结构复杂度不断滚雪球**：哪怕改一个小需求，也得先通读几十个散落各处的文件才敢下手，常规改动开始频繁震碎远处的不变量。

### 3. 断裂的知识闭环（把执行外包后，实践出真知的回路断了）
**实践出真知。** 在复杂软件工程里，真正深刻的理解很少单靠事前设计得来，而是在计划与现实碰撞的一线涌现出来的——某个漏写的前置条件、意外暴露的隐性耦合、失信的模块边界，或是文档里从未提过的真实约束，往往只有动手干活时才会撞见。

当我们把执行工作外包给Coding Agent，**Agent站在了计划与现实碰撞的一线，而人类只看到了最终的Diff。** 过程中辛苦换来的真知几乎不会留在最终提交里：一个能跑通的补丁，不会告诉你哪份过时文档曾把Agent引向歧途、它为了绕开隐性耦合做了什么妥协，也不会提醒你旁边还有一套旧流程正在与新能力重叠。对话一关，这些认知随之蒸发。后续的Agent反复踩同样的坑，架构逐渐腐烂失真，而最危险的是——**作为系统负责人的人类，其心智模型正悄然与真实代码库脱节。**

---

## `cstack` 如何破局：Ontology + CMP + Reflective Bootstrapping

> **Better design, lower context cost.**
>
> *上下文最小化原则（CMP）：对于系统必须面对的真实修改，越容易以更低成本获取“充分上下文”的设计，就是越好的设计。*

`cstack` 不重复造编码、TDD或Code Review的轮子，而是把三套互补的方法论浓缩为四个轻量的工作模式。这里的**context cost**并不是指token消耗，而是指**人或Agent为了安全完成一次修改，必须理解和查证的系统范围与认知负担**：

| 软件开发的核心难题 | `cstack` 的应对方式 | Skill |
| :--- | :--- | :--- |
| 需求模糊与过时假设 | **Domain Ontology（领域本体建模）**：先查代码库再发问，忠实捕获原案（`PS-*`），按依赖顺序做短轮次前沿追问与具体实例验证，把真实业务规则与术语边界钉死。 | **`/cs-explore`** |
| 范围蔓延与产品模型混乱 | **Product Specification（产品行为规格）**：在写代码前明确目标工作流、人/软件/Agent 职责边界、具体验收场景、原案覆盖表（`Solution Coverage`）与显式`Out of Scope`拒绝清单。 | **`/cs-spec`** |
| 结构复杂度不断复利 | **CMP Design（上下文成本驱动设计）**：直接在对话中做由内向外的设计规划（Modification Closure、Policy 与 Mechanism 分离、Boundary Trust），把未决的*What-level*决策交给人类拍板，不写冗余设计文档。 | **`/cs-design`** |
| 知识闭环断裂与静默腐化 | **Reflective Bootstrapping**：任务刚结束时趁一线轨迹尚热，检测架构腐化信号（`Missing Path`、`Boundary Bypass`、`Unowned Capability`）并暴露实践中的预期偏差与新知，经人工审阅完成**双向认知同步**，将确认后的Knowledge Delta合并回Living Knowledge。 | **`/cs-reflect`** |

---

## 核心设计哲学

1. **理解现实、设计产品、构造软件各司其职** —— 忠实建模业务现状（`Explore`）、创造性设计新工作方式（`Spec`）与规划可持续演进的代码结构（`Design`）是三种不同的认知活动，混为一谈只会产生浑浊的抽象。
2. **知识保存在最适合表达它的载体中** —— 代码、类型、接口、测试以及代码里的注释是软件设计的第一载体。`cs-design`在对话中做决策导向的规划，不额外制造用完即废的设计文档。
3. **以实践滋养Living Knowledge（严格 Delta + Merge）** —— 进行中的变更集中在单个自包含文件`.cstack/changes/<change-id>.md`，实现完成后在反思阶段将计划增量与实践新知一并合并回Living Knowledge，不强加任何文档目录规范。
4. **人类审阅既是治理，也是双向认知同步** —— AI负责代码库侦察、结构化追问、方案对比与摩擦捕捉；人类掌握未决的*What-level*决策权，并通过审阅把外包执行中产生的实践认知真正收回到自己脑中。

---

## 安装

使用标准 Agent Skills CLI 一键安装：

```bash
npx skills add leric/cstack
```

或者直接将本仓库的 `skills/`（以及可选的 `agents/`、`rules/`）复制到你的 Agent 配置目录中。

---

## 快速上手：怎么用起来

### 四个核心 Skill 一览

| Skill | 什么时候用 | 产出什么 |
| :--- | :--- | :--- |
| **`/cs-explore`** | 需求模糊、进入陌生业务领域、存量系统重构，或用户直接丢来一个解决方案时 | 代码查证 + 按依赖顺序的前沿追问 + 具体实例验证 → 实时写入 `.cstack/changes/<change-id>.md` 第 1–2 节（问题、原案 `PS-*`、领域增量） |
| **`/cs-spec`** | 准备设计产品工作方式（或直接处理小型 Brownfield 增量需求） | 产品模型、角色职责分工、行为验收场景、`PS-*` 覆盖表（`Solution Coverage`）与 `Out of Scope` 护栏 → 写入同一 Change 文件第 3–4 节 |
| **`/cs-design`** | 准备动手写非平凡代码之前 | 在对话中输出 Quick 或 Full 级别的 CMP / Clean Architecture 设计计划：Modification Closure、Policy/Mechanism 归属、未决 *What-level* 门控、由内向外的实现与验证路线 |
| **`/cs-reflect`** | 完成一次非平凡开发后立刻运行（趁一线执行轨迹还在上下文中） | 默认只读输出简洁反思报告：检测 `Missing Path` / `Boundary Bypass` / `Unowned Capability`，汇报实践中的预期偏差与新知，并给出合并回 Living Knowledge 的提案（经你授权后才落盘） |

---

### 完整工作流示例（新特性 / 复杂重构）

1. **探索领域与原案（`/cs-explore`）**
   ```text
   /cs-explore 客户希望用户选择设备类型后，系统自动推荐适用标准并直接生成评估任务。
   ```
   - Agent 会先读 `.cstack/index.md` 和代码库自行查证事实，不拿代码能回答的问题打扰你。
   - 忠实捕获原始提案（`PS-1`），通过具体场景与反例分轮追问（每题附推荐答案），并实时更新 `.cstack/changes/<change-id>.md`。

2. **设计产品行为规格（`/cs-spec`）**
   ```text
   /cs-spec
   ```
   - 在同一个 Change 文件里定义目标工作流、用户/软件/Agent 职责边界、验收场景、逐条对照原案的 `Solution Coverage` 以及明确的 `Out of Scope`。
   - *按需画 HTML 草图*：布局、交互或状态变化用文字讲不清时，Agent 可以在讨论中画一个可直接打开的 HTML 草图。讨论确认的行为和决策写回 Change 文件。

3. **规划软件设计并执行（`/cs-design`）**
   ```text
   /cs-design
   ```
   - 读取活跃 Change 规格与代码库，分离 Policy 与 Mechanism、检查边界可信度，并在对话中直接打印 **Quick** 或 **Full** 设计计划。
   - 拍板其中标出的未决 *What-level* 决策后，直接交代你的 Coding Agent 按计划由内向外编码即可。

4. **反思与认知同步（`/cs-reflect`）**
   ```text
   /cs-reflect
   ```
   - 代码写完验证通过后立即调用，趁执行轨迹还在上下文中。
   - 暴露路由摩擦（`Missing Path`）、边界捷径（`Boundary Bypass`）与归属缺失（`Unowned Capability`），反馈实践揭示的预期差异与新知，并给出将变更合并回 Living Knowledge 的精确提案。
   - 默认只出报告不改文件；只有你明确授权后，才执行对应的修复或知识合并。

---

### 轻量快路径（按需组合，不是僵硬流水线）

不需要每次都跑完四个命令：

- **小型增量需求（Brownfield）**：`/cs-spec`（自动内联轻量知识缺口检查）→ `/cs-design` → 实现 → `/cs-reflect`
- **明确的 Bug 修复 / 重构**：`/cs-design` → 实现 → `/cs-reflect`
- **随时回流**：`/cs-design` 发现产品行为含糊就回到 `/cs-spec`；`/cs-spec` 撞见未知业务规则就回到 `/cs-explore`。

---

## 它会在你的项目里生成什么？

`cstack` 不强制你的文档目录规范，首次使用时仅懒创建两个极简锚点：

```text
.cstack/
├── index.md                    # 登记你现有的 Living Knowledge 路径（结构不限）与活跃变更
├── changes/
│   └── <change-id>.md          # 单文件承载一次变更的 Explore + Spec + Merge Notes
└── friction.log                # 可选旁路摩擦日志（启用 friction-tracker 规则时记录）
```

项目原本的 `docs/`、`GLOSSARY.md`、`architecture-map.md` 或 ADR 放哪都可以，只需在 `.cstack/index.md` 里登记路径即可。

---

## 更多参考

- [Context Minimization Principle (CMP)](https://www.contextcost.dev/cmp/) — `cstack` 背后的上下文成本软件设计理论
- [Reflective Bootstrapping: How Agents Turn Practice into Institutional Knowledge](https://www.airic.dev/research/reflective-bootstrapping/) — `/cs-reflect` 与人机双向知识同步背后的理论基础
- [`docs/prd.md`](docs/prd.md) — 完整产品需求文档与方法论阐述
- [`skills/`](skills/) — 四个核心 Skill 源码（`cs-explore`、`cs-spec`、`cs-design`、`cs-reflect`）
- [`agents/cstack-explorer.md`](agents/cstack-explorer.md) 与 [`rules/cstack-friction-tracker.md`](rules/cstack-friction-tracker.md) — 可选共享原语（只读上下文探索子 Agent 与防压缩旁路摩擦采集规则）
