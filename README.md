# `cstack`

**English** | [简体中文](README.zh-CN.md)

*Discover the domain. Design the product. Evolve the code.*

`cstack` is a lightweight set of **4 AI software engineering skills** built on **Domain Ontology**, the **[Context Minimization Principle (CMP)](https://www.contextcost.dev/cmp/)**, and **Reflective Bootstrapping**. Following the open Agent Skills (`SKILL.md`) standard, it works out of the box with your coding agents.

---

## The Hard Problem of AI Coding

With modern coding agents, generating implementation code, tests, and refactors is becoming nearly free. Yet complex software projects haven't gotten any easier to evolve. When code becomes cheap, **coding is no longer the hard problem—three deeper problems take over**:

### 1. Modeling Reality (Building the Wrong Thing at 10x Speed)
In real brownfield systems, requirements rarely arrive clean. Domain concepts drift, terms mean different things across modules, and older parts of the codebase still encode obsolete business constraints. When implementation is cheap, conceptual ambiguity becomes expensive: an eager coding agent will faithfully patch outdated abstractions, preserve dead assumptions, and ship polished code for the wrong mental model.

### 2. Compounding Complexity (Architectural Decay in Fast-Forward)
Cheap code accelerates entropy. Every fast, locally reasonable AI change adds another special case, quietly bypasses a boundary, or leaves a legacy flow half-alive beside a new one. Over time, **structural complexity compounds**: making even a small change requires understanding dozens of scattered files just to stay safe, and routine edits begin breaking distant invariants.

### 3. The Broken Learning Loop (Delegation Cuts Off the Source of Knowledge)
**Practice produces knowledge.** In complex software engineering, deep understanding rarely comes from upfront design alone—it emerges when plans collide with reality: a missing prerequisite, an unexpected constraint, a leaky boundary, or an undocumented convention discovered only while doing the work.

When we delegate execution to coding agents, **the agent sits where plans meet reality, while the human only sees the finished diff.** Little of what was learned survives in the final commit: a working patch doesn't show the misleading doc that sent the agent down the wrong path, the hidden coupling it had to work around, or the legacy flow still overlapping the new one. Once the chat closes, that hard-won knowledge evaporates. Future agents repeat the same friction, Living Knowledge rots, and—most dangerously—**the human owner's mental model quietly detaches from the real system.**

---

## How `cstack` Tackles It: Ontology + CMP + Reflective Bootstrapping

> **Better design, lower context cost.**
>
> *Context Minimization Principle (CMP): For the modifications a system must realistically support, prefer the design whose sufficient context is cheaper to acquire.*

`cstack` does not reinvent coding, TDD, or code review. Instead, it brings three complementary disciplines into four lightweight working modes—where *context cost* isn't about token spend, but how much of the system a human or agent must understand and inspect to make a safe change:

| The Remaining Hard Problem | How `cstack` Handles It | Skill |
| :--- | :--- | :--- |
| Ambiguous goals & obsolete assumptions | **Domain Ontology**: Audits your codebase first, captures proposals faithfully (`PS-*`), and runs short dependency-ordered frontier grilling with concrete examples to nail down real domain rules & terminology. | **`/cs-explore`** |
| Scope creep & muddy product models | **Product Specification**: Defines target workflows, human/software/agent responsibilities, concrete acceptance scenarios, `PS-*` Solution Coverage, and explicit `Out of Scope` guardrails before coding. | **`/cs-spec`** |
| Compounding structural complexity | **CMP Design**: Plans inside-out right in conversation (Modification Closure, Policy vs. Mechanism, Boundary Trust) and surfaces unresolved *What-level* human decisions—with zero redundant design docs. | **`/cs-design`** |
| Broken learning loop & silent decay | **Reflective Bootstrapping**: Examines friction in the live execution trace right after implementation (`Missing Path`, `Boundary Bypass`, `Unowned Capability`) and runs human-reviewed **two-way knowledge synchronization**—bringing discoveries back to the human owner while merging realized Knowledge Deltas into Living Knowledge. | **`/cs-reflect`** |

---

## Design Philosophy

1. **Model the domain, design the product, build for modification** — Understanding business reality (`Explore`), creatively designing a better workflow (`Spec`), and structuring evolvable code (`Design`) are distinct cognitive activities. Mixing them creates muddy abstractions.
2. **Knowledge belongs where it is best expressed** — Code, types, interfaces, and tests are the primary design artifacts. `cs-design` plans in conversation rather than generating disposable design docs.
3. **Practice feeds Living Knowledge (Strict Delta + Merge)** — In-flight changes live in a single self-contained file (`.cstack/changes/<change-id>.md`) until implemented, then discoveries from execution merge back into Living Knowledge during reflection. No forced doc hierarchy.
4. **Human review as governance *and* knowledge synchronization** — AI handles codebase reconnaissance, structured grilling, option comparison, and friction detection. Humans own unresolved *What-level* decisions—and review is where lessons from delegated practice flow back into human understanding.

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
| **`/cs-explore`** | Starting a fuzzy requirement, unfamiliar domain, legacy overhaul, or user-proposed solution | Code-grounded reconnaissance + dependency-ordered frontier grilling + concrete examples → Sections 1–2 of `.cstack/changes/<change-id>.md` (Problem, `PS-*` proposals, Domain Delta) |
| **`/cs-spec`** | Ready to design how the product should work (or handling a small Brownfield feature directly) | Product model, role division, behavior scenarios, `PS-*` Solution Coverage, and `Out of Scope` guardrails → Sections 3–4 of the same Change file |
| **`/cs-design`** | Before writing non-trivial code | Conversational Quick or Full CMP / Clean Architecture plan: Modification Closure, Policy/Mechanism placement, unresolved *What-level* gates, and an inside-out implementation & test route |
| **`/cs-reflect`** | Immediately after finishing a non-trivial implementation (while the live trace is still in context) | Read-only reflection on `Missing Path` / `Boundary Bypass` / `Unowned Capability` + a reviewable proposal to synchronize discoveries with you and merge realized deltas into Living Knowledge (writes only after you approve) |

---

### Typical Workflow (New or Complex Feature)

1. **Explore the domain (`/cs-explore`)**
   ```text
   /cs-explore Customers want the app to automatically recommend compliance standards
   when they pick an equipment type and generate assessment tasks.
   ```
   - Checks `.cstack/index.md` and your codebase first so it never asks facts it can look up.
   - Captures the original proposal (`PS-1`) faithfully without dismissing it, grills you in short dependency-ordered rounds with recommended answers, and updates `.cstack/changes/<change-id>.md` inline.

2. **Design the product behavior (`/cs-spec`)**
   ```text
   /cs-spec
   ```
   - Builds on the same Change file to define target workflows, user/software/agent responsibilities, acceptance scenarios, `Solution Coverage` (accounting for every `PS-*`), and explicit `Out of Scope` guardrails.
   - *HTML sketches when useful*: If a layout, interaction, or state transition is hard to picture in text, the agent can draw it in a standalone HTML file during the discussion. Agreed behavior and decisions go back into the Change file.

3. **Plan the software design (`/cs-design`)**
   ```text
   /cs-design
   ```
   - Reads the active Change spec and your codebase, separates Policy from Mechanism, checks boundary trust, and prints a **Quick** or **Full** design plan right in chat.
   - Resolve any flagged *What-level* decision, then execute the plan inside-out.

4. **Reflect & synchronize knowledge (`/cs-reflect`)**
   ```text
   /cs-reflect
   ```
   - Run right after implementation while the execution trace is still fresh.
   - Surfaces routing friction (`Missing Path`), shortcuts (`Boundary Bypass`), and ownership gaps (`Unowned Capability`), explains what practice revealed, and proposes exact updates to merge the completed Change back into your Living Knowledge.
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
├── index.md                    # Thin map to your Living Knowledge (any layout) + Active Changes
├── changes/
│   └── <change-id>.md          # Single self-contained file for Explore + Spec + Merge Notes
└── friction.log                # Optional side-band log (if friction-tracker rule is enabled)
```

Your existing `docs/`, `GLOSSARY.md`, `architecture-map.md`, or ADRs stay wherever you want—just register them in `.cstack/index.md`.

---

## Further Reading

- [Context Minimization Principle (CMP)](https://www.contextcost.dev/cmp/) — The context-cost design foundation behind `cstack`
- [Reflective Bootstrapping](https://www.airic.dev/research/reflective-bootstrapping/): How Agents Turn Practice into Institutional Knowledge — The theory behind `/cs-reflect` and human-AI knowledge synchronization
- [`docs/prd.md`](docs/prd.md) — Full Product Requirements Document & methodology rationale
- [`skills/`](skills/) — Canonical `SKILL.md` definitions (`cs-explore`, `cs-spec`, `cs-design`, `cs-reflect`)
- [`agents/cstack-explorer.md`](agents/cstack-explorer.md) & [`rules/cstack-friction-tracker.md`](rules/cstack-friction-tracker.md) — Optional shared read-only explorer subagent and compaction-safe friction tracker
