---
name: cs-merge
description: Stateless merge-time Living Knowledge governance and Change closeout skill for cstack. Maps a Change file and its Section 4 trace clues onto Living Knowledge targets via .cstack/index.md, reconciles against the final git diff to apply local garbage collection (ADD, RESOLVE, COMPRESS, PRUNE) and confirm-first reminders strictly within the touched slice, routes deferred refactors, and cleans up completed Change artifacts.
---

# `/cs-merge` — Living Knowledge Governance & Change Closeout

Settle a completed Change into canonical **Living Knowledge** when it is ready to land (or right as it lands) while performing **strict local garbage collection (Local GC)** so the knowledge base stays compact, current, and free of accumulated slop.

> **Core Stance — Living Knowledge as Primary Context; Fight Knowledge Entropy**
> - **Balanced context partition (CMP applied to `cstack` itself)**: Where `/cs-reflect` centers its context window on the **execution trajectory**, `/cs-merge` drops the noisy coding trajectory and centers its context window on **Living Knowledge + Change file + final `git diff`**—dedicating full attention to clean synthesis and subtraction.
> - **Single owner of Living Knowledge routing**: Upstream skills (`Explore`, `Spec`, `Build`, `Reflect`) never waste effort pre-planning documentation layouts. `/cs-merge` alone maps realized domain, behavior, visual, and architecture changes onto `.cstack/index.md` targets.
> - **Stateless by design**: `/cs-merge` requires **no** memory of the original coding session. It operates cleanly in a fresh session from `.cstack/index.md`, `<changes-dir>/<change-id>.md` (Sections 1–4), and the final `git diff`.
> - **Final code is ground truth (Diff-Verified)**: Review rounds after `/cs-reflect` may alter behavior or revert refactors. Verify every Spec claim and Section 4 clue against the final `git diff` and current code/tests before touching Living Knowledge.
> - **Touched-Slice Local GC only**: Inspect **only** Living Knowledge sections referenced in the Change file (`Related Living Knowledge`, Section 4.2/4.3) or mapped from `.cstack/index.md` for concepts/modules touched by the diff. Never scan the whole knowledge base.

## Operating Modes & Write Authority

- **Proposal mode is the default (zero writes).** Reconcile the Change file against the final diff, map onto the touched Living Knowledge slice, and present a compact grouped governance plan (`ADD`, `RESOLVE`, `COMPRESS`, `PRUNE`, `REMIND` advisories, and deferred refactor/topic dispositions).
- **Apply & Close mode requires explicit user authorization.** Execute only approved Living Knowledge edits, route any deferred items so nothing is lost silently, update `.cstack/index.md`, delete the completed Change artifact by default (relying on Git history), and clear consumed `.cstack/friction.log` entries.

## Governance Actions & Advisory Rule

Use [`references/governance-actions.md`](references/governance-actions.md) to classify every proposed touch to Living Knowledge:

- **`ADD` (Minimal Why & Realized Delta)**: Route newly realized domain concepts/rules, external product behaviors, durable Architecture Map routes, and non-obvious practice constraints (`4.3 Spec Deviations & Hidden Why`) to the right Living Knowledge target. **Never restate code structure, signatures, or diff history** (`P1: Knowledge belongs where it is best expressed`).
- **`RESOLVE` (Conflict Arbitration)**: Where Living Knowledge clashes with the final code, newly confirmed domain truth, or concurrent direct human edits to Living Knowledge, present both sides with file/line evidence and a recommended verdict for human decision.
- **`COMPRESS` (Consolidate & Sink)**: When a touched section has accumulated 3+ piecemeal exceptions/caveats or reads like layered patches, rewrite it into one cohesive current-state rule/table—and flag fragile prose checklists that should sink into types, tests, or lint rules (`CMP Check`).
- **`PRUNE` (Verified Subtraction)**: Propose direct deletion **only** for entries verified against final code/tests as:
  1. **Obsolete (`stale / dead`)** — describes removed workflows, deleted concepts, or superseded behavior;
  2. **Contradicted (`superseded`)** — the losing side of a resolved contradiction;
  3. **Internalized into Code (`subsumed`)** — former prose warnings ("remember to update B when changing A") now enforced mechanically by encapsulation, types, tests, or static checks (`4.3 Code-Internalized Guardrails`).
- **`REMIND` (Confirm-First Advisory)**: For other observations—such as `4.2 Read & Unused (Zero-Utility)` docs that are **not** proven obsolete, contradicted, or internalized (e.g., valid domain docs loaded only because an `.cstack/index.md` pointer is too coarse)—**do not propose automatic deletion**. Surface a concise reminder first and modify, re-index, or prune **only if the user explicitly confirms**.

## Workflow

1. **Load Stateless Inputs.**
   - Read `.cstack/index.md` to locate `Living Knowledge` mappings and target Change artifact (use an explicit `<change-id>` or `<topic-id>/<NN>-<slice-id>` argument when multiple active changes exist).
   - Read the active Change file in full (Sections 1–4 for a single-file Change; or Sections 3–4 of `<changes-dir>/<topic-id>/<NN>-<slice-id>.md` **plus** its `Parent Overview` `overview.md` for a multi-slice topic) and `.cstack/friction.log` (if present).
   - Inspect `git diff <base>...HEAD` (or the target commit/branch diff) to establish what actually shipped.
   - *Graceful Degradation*: If Section 4 is absent (`/cs-reflect` was skipped), reconcile from Sections 1–3 (and `overview.md` if multi-slice) + `.cstack/friction.log` + final `git diff`, and note explicitly that live-trace `Zero-Utility` and hot-friction clues were unavailable.
2. **Reconcile Against Final Diff & Concurrent Human Edits.**
   - Cross-check `Domain Delta` (Section 2, or **only the subset realized by this slice** in `overview.md`), `Product Spec` (Section 3), and `Trace Clues` (Section 4.3) against the final diff. Never promote unrealized domain concepts belonging solely to queued slices into Living Knowledge. Honor intentional `Spec Deviations` recorded in `4.3`, and drop or adjust any item altered or reverted during PR review.
   - For every candidate `PRUNE`, verify directly in final code/tests that the constraint is genuinely enforced or the old flow is truly gone.
   - Check touched Living Knowledge files (`git log` / current text) for concurrent direct human edits made while the Change was in flight so manual corrections are surfaced via `RESOLVE`, never silently overwritten.
3. **Map Targets & Plan Touched-Slice Governance.**
   - Using `.cstack/index.md`, `Related Living Knowledge` (plus `overview.md` if multi-slice), and `Section 4.2 / 4.3`, identify and read **only** the Living Knowledge files/sections inside the Change's Touched Slice.
   - Draft concise, exact edits grouped by `ADD`, `RESOLVE`, `COMPRESS`, and `PRUNE`, plus `REMIND` advisories for non-dead `Zero-Utility` or coarse-routing items (see [`references/governance-actions.md`](references/governance-actions.md)).
4. **Check Deferred Refactors (`[R-*]`) & Deferred Topics (`[DT-*]`).**
   - **Deferred Refactors (`Section 4.1`)**: Inspect Section `4.1` for any Refactor Proposal whose disposition is still `pending` or `defer-to-followup` (or whose `fixed-in-branch` code change was reverted). Ensure **zero silent loss** before Change file cleanup by proposing an explicit destination for each:
     - Spawn a focused follow-up Change stub (`<changes-dir>/<refactor-id>.md`, registered in `.cstack/index.md`);
     - Record under `Known Boundary Debts` or `Open Ownership Questions` in a registered Architecture Map;
     - Hand off to an external Issue tracker (confirmed by user); or
     - Close explicitly as `wont-fix`.
   - **Deferred Explore Topics (`[DT-*]` from Section 1 / `overview.md`)** *(on single-file closeout or final-slice closeout of a multi-slice topic)*: Inspect `Deferred Topics` parked during `/cs-explore` and propose an explicit destination for each:
     - Spawn a new Change stub (`<changes-dir>/<next-topic>.md` with `Status: exploring`, registered in `.cstack/index.md`) ready for a fresh `/cs-explore`;
     - Hand off to an external Issue tracker (confirmed by user); or
     - Close explicitly as `wont-do`.
   - **Multi-Slice Final Check** *(when closing the last remaining slice, or closing a topic early)*: Verify every `PS-*` in `overview.md` was covered by a merged slice, routed via `[DT-*]`, or explicitly dropped.
5. **Human Approval Gate (Proposal Mode).**
   - Present the grouped plan using [`references/merge-report-template.md`](references/merge-report-template.md) with compact IDs (`M-1`, `M-2`, ..., `A-1` reminders, `R-*` refactor routes, `DT-*` topic routes) so the user can approve selectively (e.g., `all`, `M-1, M-3`, `skip M-2`, `confirm A-1`, `DT-1 -> new explore stub`).
6. **Apply & Close Lifecycle (Authorized Only).**
   - Apply all approved `ADD`, `RESOLVE`, `COMPRESS`, and `PRUNE` edits—plus any `REMIND` actions the user explicitly confirmed—to Living Knowledge.
   - Execute approved routing for deferred `[R-*]` refactors and `[DT-*]` Explore topics (e.g., creating follow-up Change stubs or Architecture Map debt entries).
   - Close out Change artifacts and `.cstack/index.md` (registering any newly created Living Knowledge paths):
     - **Single-File Change**: Remove `<change-id>` from `Active Changes` in `.cstack/index.md` and **delete `<changes-dir>/<change-id>.md` by default** (Git history preserves the audit trail; move to `.cstack/archive/` or retain **only** if the user explicitly asks).
     - **Intermediate Multi-Slice Change (`queued` slices remain in `overview.md`)**: Mark this slice `merged` in `overview.md`'s Slice Map (and append a 1-line note under `Practice Notes from Merged Slices` if `4.3` revised shared assumptions for later slices—do **not** rewrite all of Section 2, as the next `/cs-spec` reads fresh Living Knowledge first). **Delete `<changes-dir>/<topic-id>/<NN>-<slice-id>.md`** and remove its indented line from `.cstack/index.md`, while **keeping `<changes-dir>/<topic-id>/overview.md`** and its parent topic line in `.cstack/index.md`. Remind the user which queued slice is next for `/cs-spec`.
     - **Final Multi-Slice Change (no `queued` slices remain, or user explicitly closes remaining slices)**: Remove `<topic-id>/` (and its slice) from `Active Changes` in `.cstack/index.md` and **delete the entire `<changes-dir>/<topic-id>/` directory** (including `overview.md`) by default.
   - Clear consumed entries from `.cstack/friction.log` (if present).

## Final Check

Before finishing, verify that:
1. Target Living Knowledge files were mapped cleanly from `.cstack/index.md` and every proposed `ADD`, `RESOLVE`, `COMPRESS`, and `PRUNE` was cross-checked against the **final** `git diff` and current code/tests (with unrealized queued-slice concepts kept out of Living Knowledge).
2. Inspection stayed strictly within the **Touched Knowledge Slice** (no full-repo doc audit).
3. `PRUNE` is proposed **only** for obsolete, contradicted, or code-internalized entries; other `Zero-Utility` or routing observations are presented as `REMIND` advisories requiring explicit user confirmation.
4. Every `pending` or `defer-to-followup` refactor in Section `4.1`—and, on single-file or final-slice closeout, every parked `[DT-*]` topic and `overview.md` `PS-*`—has an explicit user-confirmed disposition before artifacts are deleted.
5. Proposal mode performs **zero writes**, and Living Knowledge reads as clean current-state truth rather than an append-only changelog.
