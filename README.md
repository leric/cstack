# `cstack`

**English** | [简体中文](README.zh-CN.md)

*Discover the domain. Design the product. Evolve the code.*

`cstack` is a lightweight set of **5 AI software engineering skills** built on **Domain Ontology**, the **[Context Minimization Principle (CMP)](https://www.contextcost.dev/cmp/)**, and **Reflective Bootstrapping**. Following the open Agent Skills (`SKILL.md`) standard, it works out of the box with your coding agents.

---

## The Hard Problem of AI Coding

With modern coding agents, generating implementation code, tests, and refactors is becoming nearly free. Yet complex software projects haven't gotten any easier to evolve. When code becomes cheap, **coding is no longer the hard problem—three deeper problems take over**:

### 1. Modeling Reality (Building the Wrong Thing at 10x Speed)
In real brownfield systems, requirements rarely arrive clean. Domain concepts drift, terms mean different things across modules, and older parts of the codebase still encode obsolete business constraints. When implementation is cheap, conceptual ambiguity becomes expensive: an eager coding agent will faithfully patch outdated abstractions, preserve dead assumptions, and ship polished code for the wrong mental model.

### 2. Compounding Complexity (Architectural Decay in Fast-Forward)
Cheap code accelerates entropy. Every fast, locally reasonable AI change adds another special case, quietly bypasses a boundary, or leaves a legacy flow half-alive beside a new one. Over time, **structural complexity compounds**: making even a small change requires understanding dozens of scattered files just to stay safe, and routine edits begin breaking distant invariants.

### 3. The Broken Learning Loop & Knowledge Entropy (Evaporating Insights, Rotting Docs)
**Practice produces knowledge.** In complex software engineering, deep understanding rarely comes from upfront design alone—it emerges when plans collide with reality: a missing prerequisite, an unexpected constraint, a leaky boundary, or an undocumented convention discovered only while doing the work.

When we delegate execution to coding agents, **the agent sits where plans meet reality, while the human only sees the finished diff.** Once the chat closes, hard-won discoveries evaporate—and **the human owner's mental model quietly detaches from the real system.** Worse still, even teams that write docs fall into the **append-only trap**: stale warnings, contradicted rules, and piecemeal caveats pile up in the knowledge base, turning yesterday's notes into tomorrow's context bloat and AI-amplified slop.

---

## How `cstack` Tackles It: Ontology + CMP + Reflective Bootstrapping

> **Better design, lower context cost.**
>
> *Context Minimization Principle (CMP): For the modifications a system must realistically support, prefer the design whose sufficient context is cheaper to acquire.*

`cstack` does not reinvent coding, TDD, or code review. Instead, it brings three complementary disciplines into five lightweight working modes—where *context cost* isn't about token spend, but how much of the system a human or agent must understand and inspect to make a safe change:

| The Remaining Hard Problem | How `cstack` Handles It | Skill |
| :--- | :--- | :--- |
| Ambiguous goals & obsolete assumptions | **Domain Ontology**: Audits your codebase first, captures proposals faithfully (`PS-*`), and runs short dependency-ordered frontier grilling with concrete examples to nail down real domain rules & terminology. | **`/cs-explore`** |
| Scope creep & muddy product models | **Product Specification**: Defines target workflows, human/software/agent responsibilities, on-demand UI cases (**Interactive UX** for usability + **Visual Design** for 0→1 aesthetics & anti-style-slop), concrete acceptance scenarios, `PS-*` Solution Coverage, and explicit `Out of Scope` guardrails before coding. | **`/cs-spec`** |
| Compounding structural complexity | **CMP Build**: Pairs the universal CMP kernel with on-demand domain cases (**Clean Architecture** for backend, **Frontend Architecture** for interactive UI, plus **Cross-Boundary Seam** rules for full-stack tasks), gates only unresolved *What-level* decisions, and executes inside-out implementation & verification—with zero disposable design docs. | **`/cs-build`** |
| Broken learning loop & in-branch structural drift | **Reflective Bootstrapping**: Examines friction in the live execution trace right after coding (`Missing Path`, `Boundary Bypass`, `Unowned Capability`), executes approved refactors, syncs discoveries with the human owner, and records handoff notes into the Change file without prematurely mutating trunk docs. | **`/cs-reflect`** |
| Knowledge entropy & append-only doc rot | **Diff-Verified Knowledge Governance (Local GC)**: Statelessly reconciles the Change file against the final `git diff` at merge time to curate Living Knowledge via **`ADD` / `RESOLVE` / `COMPRESS` / `PRUNE`** (+ confirm-first reminders) strictly within the touched slice, routes deferred refactors, and cleans up retired Change files. | **`/cs-merge`** |

---

## Design Philosophy

1. **Model the domain, design the product, build for modification** — Understanding business reality (`Explore`), creatively designing a better workflow and experience (`Spec`), and constructing evolvable working software (`Build`) are distinct cognitive activities. Mixing them creates muddy abstractions.
2. **Knowledge belongs where it is best expressed** — Code, types, interfaces, and tests are the primary structural artifacts. `/cs-build` plans structure in conversation rather than generating disposable design docs, and prose guardrails retire once internalized into code or tests.
3. **Strict Delta + Merge with Local GC** — In-flight changes live in a single self-contained file (`.cstack/changes/<change-id>.md`). `/cs-reflect` captures hot execution-trace signals into the Change file right after coding; `/cs-merge` atomically curates and prunes Living Knowledge when landing, then deletes the completed Change file so dead specs never become search noise.
4. **Human review as governance *and* knowledge synchronization** — AI handles codebase reconnaissance, structured grilling, option comparison, friction detection, and touched-slice GC proposals. Humans own unresolved *What-level* decisions—and review is where lessons from delegated practice flow back into human understanding.

---

## Install

Install directly with the standard Agent Skills CLI:

```bash
npx skills add leric/cstack
```

Or copy `skills/` (and optionally `agents/` and `rules/`) directly into your agent's configuration directory.

---

## Quick Start: How to Use `cstack`

### The 5 Skills at a Glance

| Skill | When to Invoke | What You Get |
| :--- | :--- | :--- |
| **`/cs-explore`** | Starting a fuzzy requirement, unfamiliar domain, legacy overhaul, or user-proposed solution | Code-grounded reconnaissance + dependency-ordered frontier grilling + concrete examples → Sections 1–2 of `.cstack/changes/<change-id>.md` (Problem, `PS-*` proposals, Domain Delta) |
| **`/cs-spec`** | Ready to design how the product should work or look (or handling a small Brownfield feature directly) | Product model, role division, on-demand `interactive-ux` / `visual-design` specs, `PS-*` Solution Coverage, and `Out of Scope` guardrails → Section 3 of the same Change file |
| **`/cs-build`** | Ready to architect, implement & verify non-trivial code | Conversational Quick or Full CMP plan using on-demand domain cases (`clean-architecture`, `frontend-architecture`, or both with Cross-Boundary Seam) → pauses **only** on unresolved *What-level* gates, otherwise builds & verifies inside-out directly |
| **`/cs-reflect`** | Immediately after finishing a non-trivial implementation (while the live trace is still in context) | CMP friction diagnosis (`Missing Path` / `Boundary Bypass` / `Unowned Capability`) + `[R-*]` Refactor Proposals (executed in-branch once you approve, or deferred) + human sync + Section 4 (`Reflection & Merge Handoff`) written to the Change file |
| **`/cs-merge`** | When ready to merge / land the change (works statelessly even in a fresh session) | Diff-verified Touched-Slice Local GC (`ADD` / `RESOLVE` / `COMPRESS` / `PRUNE` + confirm-first advisories) on Living Knowledge, explicit routing for any deferred refactors, and cleanup of the completed Change file |

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
   - *HTML sketches & Style Tiles when useful*: If a layout, state transition, or 0→1 visual aesthetic is hard to picture in text, the agent draws a standalone HTML wireframe or native CSS-variable Style Tile during discussion. Agreed behavior, visual pillars, and decisions go back into the Change file.

3. **Build & verify for modification (`/cs-build`)**
   ```text
   /cs-build
   ```
   - Reads the active Change spec and your codebase, loads only the matching CMP architecture case(s) (Clean Architecture for backend, Frontend Architecture for interactive UI, or both with Cross-Boundary Seam rules for full-stack changes), and prints a concise **Quick** or **Full** CMP plan in chat.
   - Pauses **only** if an unresolved *What-level* decision needs your judgment; otherwise proceeds directly into inside-out implementation and owning-level verification (including live browser verification when UI layout or runtime behavior changes).

4. **Reflect, refactor & capture trace signals (`/cs-reflect`)**
   ```text
   /cs-reflect
   ```
   - Run right after implementation while the execution trace is still hot.
   - Surfaces routing friction (`Missing Path`), shortcuts (`Boundary Bypass`), and ownership gaps (`Unowned Capability`), syncs unexpected practice discoveries with you, executes approved `[R-*]` refactors in-branch (or marks them `defer-to-followup` to avoid PR scope creep), and records `Section 4 (Reflection & Merge Handoff)` into the Change file without prematurely touching trunk docs.

5. **Govern Living Knowledge & close out (`/cs-merge`)**
   ```text
   /cs-merge
   ```
   - Run before or upon landing the PR—even days later in a fresh agent session.
   - Cross-checks the Change file against the final `git diff`, proposes **`ADD` / `RESOLVE` / `COMPRESS` / `PRUNE`** (plus confirm-first reminders for unused/coarse-routed docs) strictly within the touched Living Knowledge slice, routes any deferred refactors so nothing is lost, updates `.cstack/index.md`, and deletes the retired Change file.

---

### Fast Paths (Iterative, Not a Rigid Pipeline)

You do **not** have to run all five skills every time:

- **Small Brownfield feature**: `/cs-spec` (inlines a quick gap check) → `/cs-build` → `/cs-reflect` → `/cs-merge`
- **Clear bug fix or refactor**: `/cs-build` → `/cs-reflect` *(keeps zero file overhead if clean; lazily writes a compact Change file for `/cs-merge` only when deferred refactors or Living Knowledge deltas exist)*
- **Tiny CSS / pixel tweaks within an existing visual system**: Iterate directly with live browser feedback when domain rules, state ownership, visual tokens, and boundaries stay untouched *(use `/cs-spec` with the `visual-design` case for 0→1 aesthetics or visual redesigns)*
- **Mid-flight discovery**: Hit a product ambiguity in `/cs-build`? Jump back to `/cs-spec`. Uncover an unexpected domain rule in `/cs-spec`? Jump back to `/cs-explore`.

---

## What `cstack` Creates in Your Repo

`cstack` never forces a documentation folder structure. On first use, it lazily adds only lightweight anchors:

```text
.cstack/
├── index.md                    # Thin map to your Living Knowledge (any layout) + Active Changes
├── changes/
│   └── <change-id>.md          # Single in-flight file (Explore + Spec + Reflection Handoff; deleted on /cs-merge)
└── friction.log                # Optional side-band log (consumed by /cs-reflect, cleared on closeout)
```

Your existing `docs/`, `GLOSSARY.md`, `DESIGN.md`, backend/frontend `architecture-map` files, or ADRs stay wherever you want—just register them in `.cstack/index.md`.

---

## Further Reading

- [Context Minimization Principle (CMP)](https://www.contextcost.dev/cmp/) — The context-cost design foundation behind `cstack`
- [Reflective Bootstrapping](https://www.airic.dev/research/reflective-bootstrapping/): How Agents Turn Practice into Institutional Knowledge — The theory behind `/cs-reflect` and human-AI knowledge synchronization
- [`docs/prd.md`](docs/prd.md) — Full Product Requirements Document & methodology rationale
- [`skills/`](skills/) — Canonical `SKILL.md` definitions (`cs-explore`, `cs-spec`, `cs-build`, `cs-reflect`, `cs-merge`)
- [`agents/cstack-explorer.md`](agents/cstack-explorer.md) & [`rules/cstack-friction-tracker.md`](rules/cstack-friction-tracker.md) — Optional shared read-only explorer subagent and compaction-safe friction tracker
