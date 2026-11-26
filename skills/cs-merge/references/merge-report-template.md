# Living Knowledge Governance & Merge Closeout Plan

Keep the proposal compact so the human can review and approve in seconds. Perform **zero writes** until approved.

## 1. Reconciliation Summary

- **Active Change**: `<changes-dir>/<change-id>.md` *(or `<changes-dir>/<topic-id>/<NN>-<slice-id>.md`)* (`Status: <status>`)
- **Parent Overview** *(multi-slice only; omit for single-file Changes)*: `<changes-dir>/<topic-id>/overview.md` (`<N>` queued slice(s) remaining)
- **Diff Baseline**: `<base>...HEAD` *(or commit range)*
- **Trace Handoff (`Section 4`)**: `Present` | `Missing — degraded to Sections 1–3 + final diff`
- **Touched Knowledge Slice Inspected**: `<paths#sections>` *(or `None registered yet`)*
- **Shipped vs. Spec Deviations**: `None` *(or brief list of spec items adjusted/dropped in final diff)*

## 2. Touched-Slice Governance Proposals (`[M-*]`)

*(Omit empty action groups; write `None — Living Knowledge already matches shipped state` if no edits are needed.)*

### `ADD` (New Canonical Truth)
- **`[M-1]` `<target-doc#section>`**: <Concise addition — domain rule, behavior, Why, or routing entry>

### `RESOLVE` (Conflicts Requiring Verdict)
- **`[M-2]` `<target-doc#section>`**:
  - *Doc says*: <...>
  - *Final code / reality (`<file#L..>`)*: <...>
  - *Recommended verdict*: <...>

### `COMPRESS` (Consolidate / Sink)
- **`[M-3]` `<target-doc#section>`**: <Replace N scattered caveats with compact rule/table; note optional code/test/lint check>

### `PRUNE` (Obsolete / Contradicted / Code-Internalized)
- **`[M-4]` `<target-doc#section>`** *(`obsolete` | `contradicted` | `internalized in <file/test>`)*: <Exact stale rule/passage to delete and verification evidence>

## 3. Advisory Reminders (`[A-*]` — Confirm Before Modifying)

*(For non-dead `Zero-Utility` context, coarse `.cstack/index.md` pointers, or unverified staleness; `None` if none.)*

- **`[A-1]` `<target-doc or .cstack/index.md>`**: <Why surfaced + options: `keep as-is (default)` | `narrow index.md pointer` | `prune / edit`>

## 4. Deferred Refactors (`[R-*]`) & Deferred Explore Topics (`[DT-*]`)

*(List any `pending` or `defer-to-followup` refactors from Section 4.1—and, on single-file or final-slice closeout, any parked `[DT-*]` Explore topics from Section 1 / `overview.md`—so nothing vanishes when artifacts are deleted; write `None` if all resolved.)*

### Deferred Refactors (`[R-*]`)
- **`[R-1]` `<title>`** (`<file/module>`):
  - *Recommended route*: `Create follow-up Change stub` | `Add to Architecture Map -> Known Boundary Debts` | `External Issue` | `Close as wont-fix`

### Deferred Explore Topics (`[DT-*]`) *(single-file or final-slice closeout only)*
- **`[DT-1]` `<parked sub-problem>`** *(associated `PS-*`: `<...>`)*:
  - *Recommended route*: `Create new Change stub for /cs-explore` | `External Issue` | `Close as wont-do`

## 5. Closeout Actions & Approval Gate

Upon approval, `/cs-merge` will:
1. Apply approved `[M-*]` proposals (and any explicitly opted-in `[A-*]` reminders) to Living Knowledge;
2. Route `[R-*]` deferred refactors and `[DT-*]` deferred Explore topics per your choice;
3. Update `.cstack/index.md` (register any new Living Knowledge files; remove completed Change/slice from `Active Changes`—or remove the whole `<topic-id>/` entry on final-slice closeout);
4. **Close Change artifacts** *(say `archive` or `keep` to override)* and clear consumed `.cstack/friction.log` entries:
   - *Single-file Change*: **Delete `<changes-dir>/<change-id>.md`**;
   - *Intermediate slice*: Mark slice `merged` in `overview.md` (with any practice note for queued slices), **delete `<changes-dir>/<topic-id>/<NN>-<slice-id>.md`**, keep `overview.md` + parent topic in `index.md`, and flag next queued slice `<next-slice>`;
   - *Final slice*: **Delete `<changes-dir>/<topic-id>/`** (including `overview.md`).

**Reply to approve** (e.g., `all`, `M-1, M-3, M-4`, `skip M-2`, `apply A-1 (narrow index)`, `R-1 -> Known Boundary Debts`, `DT-1 -> new explore stub`).
