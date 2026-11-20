# `cstack` — AI Software Engineering Skills

*Discover the domain. Design the product. Evolve the software.*

`cstack` is a focused collection of AI software engineering skills following the open **Agent Skills (`SKILL.md`)** standard—directly usable across **Cursor**, **Codex**, **Antigravity**, **Claude Code**, and **Pi** with zero build step.

Instead of re-implementing generic coding, testing, review, prototyping, or CI/CD workflows, `cstack` concentrates on the four high-judgment cognitive modes of software engineering:

- **Explore (`cs-explore`)** — Understand the real-world problem and domain, capture proposed solutions faithfully, and uncover hidden rules via Design Tree Frontier grilling.
- **Spec (`cs-spec`)** — Creatively design the target product model, role responsibilities, external behaviors, Solution Coverage, and `Out of Scope` guardrails.
- **Design (`cs-design`, Phase 2)** — Apply CMP (Context Minimization Principle) and Clean Architecture to turn product behavior into executable boundaries and tasks.
- **Reflect (`cs-reflect`, Phase 3)** — Harvest context-acquisition friction to repair architectural routing and merge realized changes back into Living Knowledge.

See [`docs/PRD.md`](docs/PRD.md) for the full product requirements document.

---

## 1. Current Status: MVP (`cs-explore` + `cs-spec`)

| Skill / Primitive | Role | Status |
| :--- | :--- | :--- |
| **`/cs-explore`** | Problem & Domain Discovery (Frontier Grilling + Proposed Solution Capture) | **MVP Ready** |
| **`/cs-spec`** | Product Design & Behavior Specification (Solution Coverage + Out of Scope + External Prototype Handoff) | **MVP Ready** |
| **`/cs-design`** | Context-Cost Driven Software Design (CMP + Clean Architecture) | Planned (Phase 2) |
| **`/cs-reflect`** | Post-Task Design Reflection + Delta-to-Living Knowledge Merge | Planned (Phase 3) |
| **`cstack-explorer`** | Shared read-only context discovery subagent | Active Primitive |
| **`cstack-friction-tracker`** | Shared ambient side-band friction tracker (`.cstack/friction.log`) | Active Primitive |

---

## 2. Minimal Knowledge Contract (Strict Delta + Merge)

`cstack` imposes only two lightweight anchors and leaves the rest of your repository documentation completely unrestricted:

1. **Routing Index (default `.cstack/index.md`)**: Maps where your **Living Knowledge** lives (any user-defined layout), where `changes/` live, and which changes are active.
2. **Single-File Change (`<changes-dir>/<change-id>.md`)**: Holds all in-flight `Explore` and `Spec` deltas for one change in a single Markdown file.

- **Living Knowledge** reflects current realized reality; users can edit it directly at any time to fix inaccurate descriptions of current state.
- **Change Knowledge** stays isolated inside `<changes-dir>/<change-id>.md` until implementation completes and merges back into Living Knowledge.

---

## 3. Installation

Install directly via the standard Agent Skills CLI (works across Cursor, Codex, Antigravity, Claude Code, Pi, etc.):

```bash
npx skills add /path/to/cstack
```

Or copy `skills/` (and optionally `agents/`, `rules/`) into your agent's skills directory.

---

## 4. Repository Layout

```text
cstack/
├── README.md
├── docs/
│   └── PRD.md                          # Product Requirements Document (v0.2)
├── skills/                             # Standard Agent Skills (edit directly)
│   ├── cs-explore/SKILL.md
│   └── cs-spec/SKILL.md
├── agents/
│   └── cstack-explorer.md              # Shared read-only context slicer subagent
└── rules/
    └── cstack-friction-tracker.md      # Shared ambient side-band friction tracker
```
