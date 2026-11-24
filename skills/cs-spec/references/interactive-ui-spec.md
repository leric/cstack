# Interactive UI & Experience Specification Guide

Load this reference **only** when the Change includes a user-facing interactive interface (Web, Desktop, Mobile, or rich TUI). Skip it for headless APIs, background jobs, CLIs, or pure data/infrastructure changes.

> **Why This Lens Exists**: Step-by-step transaction lists (`1 -> 2 -> 3`) and basic `Given-When-Then` scenarios describe backend workflows well, but on the frontend they push coding agents toward naive **database-form UX**: modal fatigue, full-screen blocking spinners, lost scroll/selection context, flat 20-field forms, and cryptic error toasts. Specify the **interactive experience** deliberately before moving to `/cs-design`.

---

## 1. Anchor in Existing UI Conventions (`Facts First`)

Before proposing layouts, interactions, or HTML sketches:
- Check `.cstack/index.md` and the codebase for an existing **Design System, component library, design tokens, route/layout shell, and interaction patterns** (e.g., how tables, drawers, dialogs, forms, and toasts already behave in this product).
- Reuse established product interaction idioms unless there is a clear product reason to introduce a new pattern.

## 2. Four Experience Dimensions to Grill & Specify

Raise only the choices that materially affect usability for this Change (following Frontier Grilling):

### A. Spatial Continuity & Context Preservation
- **Avoid unnecessary context jumps**: Decide deliberately between **inline action/editing**, **split-view / drawer / popover**, and **modal or full-page navigation**.
- Preserve the user's spatial anchor (scroll position, active filters, selection state, open workspace context) across routine edits and inspections instead of forcing full-list reloads or page bounces.

### B. Feedback Immediacy, Forgiveness & Ergonomics
- **Optimistic & non-blocking by default for fast/reversible actions**: Prefer instant optimistic feedback + **Undo** over repetitive *"Are you sure?"* confirmation modals and screen-blocking spinners. Reserve blocking confirmation strictly for destructive, irreversible, or high-stakes actions.
- **Input preservation**: Never wipe half-completed user input when validation or network submission fails; surface errors inline next to the offending field or item with a clear recovery action.
- **Smart defaults & keyboard flow**: Define sensible defaults, initial focus placement, primary `Enter` / `Esc` semantics, and high-frequency bulk or keyboard shortcuts where relevant.

### C. Information Hierarchy & Progressive Disclosure
- Do **not** mirror backend database schemas into flat forms or overcrowded tables.
- Separate **primary signals & frequent actions** (immediately visible) from **secondary/advanced configuration** (progressively disclosed on demand).

### D. Complete UI State & Navigation Matrix
Specify observable behavior across non-happy-path states wherever non-trivial:
- **Empty states**: Distinguish *first-time / zero-data onboarding* (with clear call-to-action) from *no-match filter/search results* (with quick reset).
- **Loading & refresh states**: Initial skeleton/placeholder vs. background refresh (`stale-while-revalidate`) vs. long-running progress/streaming feedback; double-submit prevention.
- **Partial failure & degradation**: What remains usable if one panel, row action, or secondary query fails?
- **Overflow & viewport adaptation**: Behavior on long text/titles, large item counts, and target viewport sizes (desktop split-pane vs. narrow/mobile collapse).
- **URL, refresh & history semantics**: Which view states (filters, active tab, selected item, open drawer/step) belong in the URL so refresh, deep-linking, and the browser Back button behave predictably, and when unsaved drafts need a leave guard?

## 3. Capture UX-Driven Cross-End Expectations (For `/cs-design`)

Great frontend UX often requires backend contract cooperation. Without prescribing internal code or raw SQL/DTO schemas in `Spec`, explicitly record external behavior expectations that constrain the backend/API seam:
- **Zero-waterfall workspace views** (expecting cohesive view data rather than sequential client round-trips);
- **Optimistic / autosave / reorder semantics** (requiring safe retries, conflict detection, or undo windows);
- **Actionable capability & error semantics** (distinguishing why an action is unavailable and mapping failures to specific fields/items rather than generic failure messages);
- **Authority vs. visibility** (distinguishing UX hiding/disabling of controls from authoritative server-side permission enforcement).

## 4. Disposable HTML Sketch Discipline

When text leaves a layout, comparison, or multi-state transition ambiguous:
- Build a minimal single-file HTML sketch in scratch space to align on **spatial layout, information hierarchy, and state transitions** (clicking through Empty / Loading / Active / Error states when helpful).
- Reflect the repository's existing component concepts rather than inventing conflicting widget patterns.
- Treat sketch markup and any temporary CDN styling (such as Bootstrap utilities) strictly as **disposable discussion aids**—write all agreed interaction behaviors and state rules into Change Section 3 so implementation uses the project's real UI stack and Design System.
