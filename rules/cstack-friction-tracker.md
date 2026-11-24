---
name: cstack-friction-tracker
description: Ambient side-band friction tracker that appends search, boundary, and ownership friction to .cstack/friction.log so episodic architectural signals survive context compaction.
globs: "**/*"
alwaysApply: true
---

# Ambient Side-Band Friction Tracker (`cstack`)

Context compaction erases first-hand search and edit friction before post-task reflection (`/cs-reflect`) runs. To preserve episodic structural signals with near-zero token overhead, immediately append a **single-line entry (<50 words)** to `.cstack/friction.log` (create the `.cstack/` directory and file if needed) whenever you experience any of the following during a task:

1. **Search Friction (`[MISSING_PATH]`)**:
   - Trigger: $>3$ failed or unrelated file reads/greps to locate a domain concept, handler, or coupled co-change site.
   - Format: `<ISO-timestamp> [MISSING_PATH] <searched concept> -> <detour taken> -> <actual location>`
2. **Boundary Friction (`[BOUNDARY_BYPASS]`)**:
   - Trigger: Backing out of an architectural interface, reaching across layers (e.g., Domain/UseCase importing Infrastructure/ORM, UI views importing raw backend DTOs or bypassing Design System primitives/tokens, or syncing computable derived state via `useEffect`).
   - Format: `<ISO-timestamp> [BOUNDARY_BYPASS] <source file/layer> -> <target file/layer>: <why boundary felt obstructive>`
3. **Orphan Creation (`[UNOWNED_CAPABILITY]`)**:
   - Trigger: Creating a helper, validator, mapper, formatter, policy rule, or duplicated UI state/overlay logic without an obvious single canonical owner.
   - Format: `<ISO-timestamp> [UNOWNED_CAPABILITY] <symbol@file>: <capability created without clear canonical owner>`

Do not narrate friction logging to the user—append silently to `.cstack/friction.log` and continue working.

### Lifecycle
- **Consumed by `/cs-reflect`**: Read alongside the live task trace to draft `[R-*]` Refactor Proposals and Change File Section 4 (`Reflection & Merge Handoff`).
- **Cleared on closeout**: Cleared by `/cs-merge` when closing the Change lifecycle (or by `/cs-reflect` directly on zero-overhead Fast-Path tasks that require no Change file).
