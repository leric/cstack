# `cstack`

**English** | [简体中文](README.zh-CN.md)

*Discover the domain. Design the product. Evolve the software.*

`cstack` is a lightweight set of **4 AI software engineering skills** built around the **[Context Minimization Principle (CMP)](https://www.contextcost.dev/)** and **Domain Ontology**. Following the open Agent Skills (`SKILL.md`) standard, it works out of the box with **Cursor**, **Codex**, **Antigravity**, **Claude Code**, and **Pi**—with zero build step.

Coding agents are already great at generating code and running tests. `cstack` does not reinvent coding, TDD, code review, or prototyping. Instead, it brings **CMP** into daily engineering so your codebase stays easy to understand, cheap to modify, and resistant to architectural decay as changes accumulate.

---

## Grounded in CMP (Context Minimization Principle)

> *For the modifications a system must realistically support, prefer the design whose sufficient context is cheaper to acquire.*

Most AI-assisted codebases degrade not because models cannot write code, but because **context cost keeps growing**: domain concepts drift, decisions scatter without routes, boundaries become untrustworthy, and every new change requires reading more files just to stay safe.

`cstack` turns CMP into four iterative working modes:

- **Low Context Cost in Knowledge**: One thin `.cstack/index.md` routes agents to your existing docs (any layout you prefer), while each in-flight change lives in a single self-contained file (`.cstack/changes/<change-id>.md`).
- **Low Context Cost in Code**: `cs-design` applies CMP (Depth, Breadth / Modification Closure, `Reduce / Relocate / Index / Check`) with Clean Architecture as its primary context-routing practice—never as dogma.
- **Continuous Anti-Decay**: `cs-reflect` observes the agent's live context-acquisition trace right after a change to catch `Missing Path`, `Boundary Bypass`, and `Unowned Capability` before debt compounds.

---

## Design Philosophy

1. **Model the domain, design the product, build for modification** — Understanding business reality (`Explore`), creatively designing a better workflow (`Spec`), and structuring evolvable code (`Design`) are distinct cognitive activities.
2. **Knowledge belongs where it is best expressed** — Code, types, interfaces, and tests are the primary design artifacts. `cs-design` plans in conversation instead of writing redundant design docs.
3. **Strict Delta + Merge** — In-flight changes stay isolated in `.cstack/changes/<change-id>.md` until implemented, then merge back into your Living Knowledge during reflection. You can still edit Living Knowledge directly at any time to fix inaccuracies about current state.
4. **Human in the Architect's Chair** — AI handles searching, grilling, comparing options, and spotting friction; humans decide unresolved *What-level* product and architectural questions.

---

## Install

Install directly with the standard Agent Skills CLI:

```bash
npx skills add leric/cstack
```

Or copy `skills/` (and optionally `agents/` and `rules/`) directly into your agent's configuration directory.

---

## Quick Start: How to Use `cstack`

### The 4 Skills at a Glance

| Skill | When to Invoke | What You Get |
| :--- | :--- | :--- |
| **`/cs-explore`** | Starting a fuzzy requirement, unfamiliar domain, or customer-proposed solution | Multi-round frontier grilling + concrete example testing → Sections 1–2 of `.cstack/changes/<change-id>.md` (Problem, `PS-*` proposals, Domain Delta) |
| **`/cs-spec`** | Ready to design how the product should work (or handling a small Brownfield feature directly) | Product model, role division, external behavior scenarios, `PS-*` Solution Coverage, `Out of Scope` guardrails, and optional `Prototype Candidate` flags → Sections 3–4 of the same Change file |
| **`/cs-design`** | Before writing non-trivial code | Conversational Quick or Full CMP/CA plan: Modification Closure, Policy/Mechanism placement, unresolved What-level gates, and an inside-out implementation & test route |
| **`/cs-reflect`** | Immediately after finishing a non-trivial implementation (while the live trace is still in context) | Concise read-only report on `Missing Path` / `Boundary Bypass` / `Unowned Capability` + a proposal to merge realized deltas back into Living Knowledge (writes only after you approve) |

---

### Typical Workflow (New or Complex Feature)

1. **Explore the domain (`/cs-explore`)**
   ```text
   /cs-explore Customers want the app to automatically recommend compliance standards
   when they pick an equipment type and generate assessment tasks.
   ```
   - The agent checks `.cstack/index.md` and your codebase first (so it won't ask facts it can look up).
   - It captures the user's original proposal (`PS-1`) without dismissing it, grills you in short dependency-ordered rounds with recommended answers, and updates `.cstack/changes/<change-id>.md` inline.

2. **Design the product behavior (`/cs-spec`)**
   ```text
   /cs-spec
   ```
   - Builds on the same Change file to define target workflows, user/software/agent responsibilities, acceptance scenarios, `Solution Coverage` (accounting for every `PS-*`), and an explicit `Out of Scope` list.
   - *Optional external prototyping*: If a UX or interaction choice feels hard to judge in text, the agent flags it as a `Prototype Candidate`. Take the self-contained Change file to your favorite prototyping tool, test it, and bring the verdict back to `/cs-spec`.

3. **Plan the software design (`/cs-design`)**
   ```text
   /cs-design
   ```
   - Reads the active Change spec and your codebase, separates Policy from Mechanism, checks boundary trust, and prints a **Quick** or **Full** design plan right in chat.
   - Resolve any flagged *What-level* decision, then simply tell your coding agent to execute the plan inside-out.

4. **Reflect & merge knowledge (`/cs-reflect`)**
   ```text
   /cs-reflect
   ```
   - Run right after implementation while the task trace is still fresh.
   - Reviews routing friction (`Missing Path`), shortcuts (`Boundary Bypass`), and ownership gaps (`Unowned Capability`), and proposes exact updates to merge the completed Change back into your Living Knowledge.
   - Nothing is modified until you explicitly approve which repairs or merges to apply.

---

### Fast Paths (Iterative, Not a Rigid Pipeline)

You do **not** have to run all four skills every time:

- **Small Brownfield feature**: `/cs-spec` (inlines a quick gap check) → `/cs-design` → implement → `/cs-reflect`
- **Clear bug fix or refactor**: `/cs-design` → implement → `/cs-reflect`
- **Mid-flight discovery**: Hit a product ambiguity in `/cs-design`? Jump back to `/cs-spec`. Uncover an unexpected domain rule in `/cs-spec`? Jump back to `/cs-explore`.

---

## What `cstack` Creates in Your Repo

`cstack` never forces a documentation folder structure. On first use, it lazily adds only two lightweight anchors:

```text
.cstack/
├── index.md                    # Maps your Living Knowledge (any layout) + Active Changes
├── changes/
│   └── <change-id>.md          # Single file holding Explore + Spec + Merge Notes for one change
└── friction.log                # Optional side-band log (if friction-tracker rule is enabled)
```

Your existing `docs/`, `GLOSSARY.md`, `architecture-map.md`, or ADRs stay wherever you want—just register them in `.cstack/index.md`.

---

## Further Reading

- [Context Minimization Principle (CMP)](https://www.contextcost.dev/) — The theoretical foundation behind `cstack`
- [`docs/prd.md`](docs/prd.md) — Full Product Requirements Document & methodology rationale
- [`skills/`](skills/) — Direct canonical `SKILL.md` definitions (`cs-explore`, `cs-spec`, `cs-design`, `cs-reflect`)
- [`agents/cstack-explorer.md`](agents/cstack-explorer.md) & [`rules/cstack-friction-tracker.md`](rules/cstack-friction-tracker.md) — Optional shared read-only explorer subagent and compaction-safe friction tracker
