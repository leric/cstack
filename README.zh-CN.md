# `cstack`

[English](README.md) | **简体中文**

*理解领域，设计产品，演化软件。*

`cstack` 是一套基于 **[CMP（Context Minimization Principle，上下文最小化原则）](https://www.contextcost.dev/)** 与 **领域本体论（Domain Ontology）** 的轻量级 AI 软件工程 Skills，遵循开放的 Agent Skills（`SKILL.md`）标准，零构建即可直接用于 **Cursor**、**Codex**、**Antigravity**、**Claude Code** 和 **Pi**。

如今 Coding Agent 已经很擅长写代码和跑测试。`cstack` 不重复造编码、TDD、Code Review 或原型工具的轮子，而是把 **CMP** 落到日常工程动作中，帮助代码库在持续迭代中保持**低理解成本、低修改成本、不腐化**。

---

## 与 CMP 的关系

> *对于系统必须面对的真实修改，越容易以更低成本获取「充分上下文」的设计，就是越好的设计。*

AI 辅助开发的代码库逐渐失控，往往不是因为模型写不出代码，而是因为**上下文成本（Context Cost）不断攀升**：领域术语漂移、业务决策散落各处却没有发现路径、架构边界失信，导致每次修改都要翻阅越来越多文件才敢动手。

`cstack` 将 CMP 落为四个可往复迭代的工程模式：

- **知识层低上下文成本**：用极薄的 `.cstack/index.md` 做全局路由索引（不限制你已有文档的目录结构），单次变更的全部探索与规格集中放在一个文件 `.cstack/changes/<change-id>.md` 里。
- **代码层低上下文成本**：`cs-design` 以 CMP（Depth、Breadth / Modification Closure、`Reduce / Relocate / Index / Check`）为判断依据，以 Clean Architecture 为主要上下文路由实践，而非教条模板。
- **持续防架构腐化**：`cs-reflect` 在任务刚完成时直接观察当前会话的一线上下文获取轨迹，在债务堆积前识别 `Missing Path`、`Boundary Bypass` 与 `Unowned Capability`。

---

## 核心设计哲学

1. **理解现实、设计产品、构造软件各司其职** —— 忠实建模业务现状（`Explore`）、创造性设计新工作方式（`Spec`）与规划可持续修改的代码结构（`Design`）是三种不同的认知活动。
2. **知识保存在最适合表达它的载体中** —— 代码、类型、接口与测试是软件设计的第一载体。`cs-design` 在对话中产出决策导向计划，不额外维护与代码重复的设计文档。
3. **严格 Delta + Merge** —— 进行中的变更全部隔离在 `.cstack/changes/<change-id>.md`，实现完成后在反思阶段合并回 Living Knowledge；用户平时仍可直接手工编辑 Living Knowledge 以修正对现状的描述错误。
4. **人类坐镇架构师椅子（Human in the Architect's Chair）** —— AI 承担检索、前沿追问、方案对比与摩擦捕获；未决的 *What-level* 业务、产品与架构归属决策始终交由人类拍板。

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
| **`/cs-explore`** | 需求模糊、进入陌生业务领域，或客户直接丢来一个解决方案时 | 按决策树前沿分轮追问 + 具体实例验证 → 实时写入 `.cstack/changes/<change-id>.md` 第 1–2 节（问题、原案 `PS-*`、领域增量） |
| **`/cs-spec`** | 准备创造性设计产品工作方式（或直接处理小型 Brownfield 增量需求） | 产品模型、角色职责分工、外部行为验收场景、`PS-*` 覆盖表（`Solution Coverage`）、`Out of Scope` 拒绝清单、外部原型候选标记 → 写入同一 Change 文件第 3–4 节 |
| **`/cs-design`** | 准备动手写非平凡代码之前 | 在对话中输出 Quick 或 Full 级别 CMP/CA 设计计划：Modification Closure、Policy/Mechanism 归属、未决 What-level 门控、由内向外的实现与验证路线 |
| **`/cs-reflect`** | 完成一次非平凡开发后立刻运行（趁一线执行轨迹还在上下文中） | 默认只读输出简洁报告：诊断 `Missing Path` / `Boundary Bypass` / `Unowned Capability`，并给出将本次变更合并回 Living Knowledge 的提案（经你授权后才修改文件） |

---

### 完整工作流示例（新特性 / 复杂需求）

1. **探索领域与原案（`/cs-explore`）**
   ```text
   /cs-explore 客户希望用户选择设备类型后，系统自动推荐适用标准并直接生成评估任务。
   ```
   - Agent 会先读 `.cstack/index.md` 和代码库自行查证事实，不拿代码能回答的问题打扰你。
   - 完整捕获客户原案（`PS-1`），通过具体场景与反例分轮追问（每题附推荐答案），并实时更新 `.cstack/changes/<change-id>.md`。

2. **设计产品行为规格（`/cs-spec`）**
   ```text
   /cs-spec
   ```
   - 在同一个 Change 文件里定义目标工作流、用户/软件/Agent 职责边界、验收场景、逐条对照原案的 `Solution Coverage` 以及明确的 `Out of Scope`。
   - *对接外部原型工具（可选）*：遇到靠文字难判断体感的交互/界面假设，Agent 会标为 `Prototype Candidate`。你可以直接把这份自包含的 Change 文件交给外部原型工具试效果，再把结论带回 `/cs-spec` 更新。

3. **规划软件设计并执行（`/cs-design`）**
   ```text
   /cs-design
   ```
   - 读取活跃 Change 规格与代码库，分离 Policy 与 Mechanism、检查边界可信度，并在对话中直接打印 Quick 或 Full 设计计划。
   - 拍板其中标出的未决 *What-level* 决策后，直接交代你的 Coding Agent 按计划由内向外编码即可。

4. **设计反思与知识合并（`/cs-reflect`）**
   ```text
   /cs-reflect
   ```
   - 代码写完验证通过后立即调用，直接基于当前会话轨迹做三大 CMP 检查，同时对比已实现代码给出 Living Knowledge / Architecture Map 合并提案。
   - 默认只出报告不改文件；你确认哪些局部修复或知识合并需要执行后，Agent 再落盘并将该 Change 标为 `merged`。

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

- [CMP（Context Minimization Principle）官网](https://www.contextcost.dev/) — `cstack` 背后的软件设计理论基础
- [`docs/prd.md`](docs/prd.md) — 完整产品需求文档与方法论阐述
- [`skills/`](skills/) — 四个核心 Skill 源码（`cs-explore`、`cs-spec`、`cs-design`、`cs-reflect`）
- [`agents/cstack-explorer.md`](agents/cstack-explorer.md) 与 [`rules/cstack-friction-tracker.md`](rules/cstack-friction-tracker.md) — 可选共享原语（只读上下文探索子 Agent 与防压缩旁路摩擦采集规则）
