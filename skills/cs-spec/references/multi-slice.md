# On-Demand Multi-Slice Topic Upgrade

Load this guide **only** when designing a product solution in `/cs-spec` reveals that delivering the full scope in one `/cs-build` would create excessive context cost, unwieldy workflow/state complexity, or too large a PR—or when resuming an existing multi-slice topic directory to spec its next queued slice.

For normal-sized changes, stay single-file (`<changes-dir>/<change-id>.md`).

## 1. Upgrade Protocol (With Explicit User Confirmation)

When proposing the split, confirm the vertical slice sequence with the user first, then upgrade `<changes-dir>/<change-id>.md` into `<changes-dir>/<topic-id>/`:

1. **Create `overview.md`**: Move the existing Explore content (Sections 1–2, including the full `PS-*` ledger and any `DT-*`) into `<changes-dir>/<topic-id>/overview.md` and append a thin **Slice Map** (below).
2. **Just-in-Time Spec (Anti-Waterfall)**: Materialize **only the next executable slice** (`01-<slice-id>.md`) into a full Section 3 Behavior Spec. Never pre-write detailed specs for queued slices—earlier slices produce practice discoveries during `/cs-build`, `/cs-reflect`, and `/cs-merge` that refine later slices.
3. **Register with Indentation in `.cstack/index.md`**: Keep the parent topic visible under `Active Changes` even between slices:
   ```markdown
   - `changes/<topic-id>/` *(multi-slice topic)* — `overview.md`
     - `01-<slice-id>.md` (`speccing`)
   ```

## 2. Resuming the Next Queued Slice

When `/cs-spec` is invoked for the next queued slice of `<changes-dir>/<topic-id>/overview.md`:
1. Read **fresh Living Knowledge** first (now reflecting earlier merged slices) alongside `overview.md` (including `Practice Notes from Merged Slices`).
2. Create only the next active slice file `<changes-dir>/<topic-id>/<NN>-<slice-id>.md`, set its row to `active` in `overview.md`, and add its indented line under `<topic-id>/` in `.cstack/index.md`.

## 3. File Templates

### Appended to `<changes-dir>/<topic-id>/overview.md`
```markdown
## Slice Map (Just-in-Time Delivery Plan)
| Slice | Goal | Assigned `PS-*` | Depends On | Status |
| :--- | :--- | :--- | :--- | :--- |
| `01-<slice-id>.md` | <1–2 line deliverable value slice> | `PS-1, PS-2` | `—` | `active` |
| `02-<slice-id>` | <1–2 line next slice> | `PS-3` | `01` | `queued` |

### Practice Notes from Merged Slices
<!-- Appended concisely by /cs-merge when a merged slice revises shared assumptions for queued slices -->
- (None yet)
```

### Active Slice File (`<changes-dir>/<topic-id>/<NN>-<slice-id>.md`)
Do **not** duplicate Section 2 from `overview.md`. Reference it in the header, account for assigned `PS-*` in `Solution Coverage`, and explicitly list later-slice capabilities under `Out of Scope`:

```markdown
# Change Slice: <topic-id> / <NN>-<slice-id>

- **Status**: `speccing`
- **Parent Overview**: `overview.md` *(shared Sections 1–2, `PS-*` master ledger, `DT-*`)*
- **Related Living Knowledge**: <paths/links from index, or "Inherited from overview.md">
- **Assigned Proposals**: `PS-1, PS-2`

## 3. Product Design & Behavior Spec
...
```
