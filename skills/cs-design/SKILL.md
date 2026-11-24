---
name: cs-design
description: Context-cost driven software design planner for cstack. Uses the Context Minimization Principle (CMP) together with on-demand domain architecture cases (Clean Architecture for backend/services, Frontend Architecture for interactive UI, and Cross-Boundary Seam rules for full-stack changes) to acquire sufficient context, evaluate modification closure and boundary trust, surface unresolved human-owned design gates, and produce an inside-out implementation and verification plan in conversation.
---

# `/cs-design` — Context-Cost Driven Software Design Planner

Plan non-trivial software changes so product behavior is expressed cleanly in code while keeping future modifications cheap to understand and route.

> **Core Stance — Plan in Conversation; Express Design in Code**
> - **Code is the primary design artifact.** Print the design plan in conversation as a decision-focused alignment tool—do **not** write a separate design document that duplicates code structure.
> - Once unresolved What-level gates are cleared, the agent can proceed directly into executing the plan.
> - Skip trivial formatting-only, comment-only, mechanical dependency-bump, or generated-only changes.

## Mental Model: CMP Kernel + Domain Architecture Cases

Apply the **Context Minimization Principle (CMP)** as the universal design judgment:

> For the modifications a system must realistically support, prefer the design whose sufficient context is cheaper to acquire.

Analyze context cost along two axes:
- **Depth**: How much context must be read—and how many untrustworthy boundaries crossed—to reason locally about a behavior.
- **Breadth**: Whether a modifier can reliably discover the complete **Modification Closure** (every artifact that must change or be verified together).

Shape context cost through four **Context Transformations**:
- **Reduce**: Consolidate a scattered decision or duplicated state into one clear owner to shrink Modification Closure.
- **Relocate**: Place volatile mechanism details behind a trustworthy contract so traversal stops at the boundary.
- **Index**: Establish a discoverable route (naming, registry, key factory, or explicit closure index) when co-changing locations cannot be merged.
- **Check**: Turn implicit assumptions into static types, exhaustive unions, contract/unit tests, or lint/dependency guards.

Weigh added **Boundary Cost / Carrying Depth / Conceptual Surface** against saved context cost, and balance **Wrong-shape Risk** (premature or ill-fitting abstraction) against **Unindexed-closure Risk** (scattering co-changing decisions without a route).

Across every domain, separate **Policy & Contracts** (stable rules, invariants, state transitions, client-owned boundaries) from **Mechanism** (volatile delivery, persistence, external SDKs, transport DTOs, UI primitives, host APIs), keeping dependencies pointing toward stable policy and contracts.

### On-Demand CMP Architecture Cases (Sharp Domain Guidance, Zero Dilution)

Generic abstractions make weak engineering guidance. Instead of forcing one vocabulary onto every stack, **select and read only the CMP Case(s) matching the touched scope**:

- **Backend, Services, APIs, Workers & Domain Modules** → Read [`references/cases/clean-architecture.md`](references/cases/clean-architecture.md)
- **Frontend & Interactive Client UI** → Read [`references/cases/frontend-architecture.md`](references/cases/frontend-architecture.md)
- **Full-Stack / Cross-Boundary Tasks (Backend + Frontend together)** → Read **both** cases and apply the **Cross-Boundary Seam Rules** below.
- **Repo-Specific Conventions & Lightweight Fallback (`No Architecture Dogma`)**: Honor established architecture rules registered in `.cstack/index.md` / Architecture Maps unless proposing a `What-level` change. For small scripts, standalone utilities, or thin glue modules where heavy layering only adds Carrying Depth, skip specialized cases and apply the four CMP transformations directly.

### Cross-Boundary Seam Rules (When a Task Spans Backend & Frontend)

When loading both backend and frontend cases in a single task, treat the **Backend ↔ Frontend Seam** as a first-class boundary rather than an afterthought:
1. **Single Contract Anchor & Closure**: Anchor both ends on one canonical contract (`packages/contracts`, OpenAPI/Protobuf/GraphQL/tRPC, or an explicit cross-repo `Modification Closure Index` when ungenerated).
2. **Authoritative Policy vs. UI Projection**: Keep business invariants and permission enforcement authoritative on the server; project capability affordances and structured field/domain error codes across the contract so the UI never duplicates backend domain conditionals (`UI hiding ≠ authorization`).
3. **UX-Driven Seam Semantics**: Shape read/write contracts to serve the Spec's interaction model (cohesive view read models vs. client request waterfalls; idempotency, versioning, or undo semantics for optimistic UI and autosave; field-actionable errors).
4. **Contract-Hinged Inside-Out Execution**: Lock core domain policy + the cross-end contract first, then build backend inside-out and frontend inside-out against that locked contract, closing with wired end-to-end and live-browser verification.

## Keep the Two Routes Distinct

- **Discovery route**: Start from the task's natural entry point (Change spec, failing test, route/component, endpoint, domain rule, schema, event, or use case). Follow the strongest local routing signals until ownership, coordinates, modification closure, and trustworthy stopping boundaries are clear. Never force a fixed layer-reading order.
- **Implementation route**: Express **policy and contracts before volatile mechanisms (inside-out)**, following the concrete sequence of the active CMP Case(s).

The discovery route may move outside-in. The implementation route stays inside-out. Do not confuse them.

## Authority Boundary

Treat a human design gate strictly as an **unresolved What-level decision**, not merely the presence of a new file or symbol.

Consider a decision **already resolved** when the user request, an approved Change Spec (`changes/<change-id>.md`), an existing architectural rule, or an established owner clearly supplies it. Do not ask the user to re-approve settled decisions.

- **How-level**: Work within an authorized behavior, owner, boundary, contract, state host, or placement rule (see active Case for domain-specific examples).
- **What-level**: Decide product behavior, capability/use-case boundaries, state ownership host (URL vs. cache vs. store vs. local), port or cross-end contract semantics, shared Design System primitives/tokens, cross-module policy placement, dependency direction, or Greenfield structural topology.

When a What-level decision remains unresolved, present concrete options with CMP trade-offs and stop before implementing that decision. Independent How-level work may continue when safe.

## Planning Modes

- **Quick plan**: Use when intent and ownership are clear, the change is mechanism-only or policy-local, existing boundaries, state hosts, and contracts stay intact, and no unresolved gate exists. Return intent, active Case(s), placement, inside-out steps, and verification inline in conversation.
- **Full plan**: Use when policy, state ownership, contracts, or boundaries change; Modification Closure crosses layers or the Backend↔Frontend seam; Greenfield structure is being chosen; ownership/placement is unclear; or an unresolved gate exists. Read and fill [`references/design-plan-template.md`](references/design-plan-template.md) in conversation, omitting empty rows and non-decisions.

Inspect enough context to choose accurately; do not trigger Full plan merely because the repository is large.

## Planning Workflow

1. **Load cstack Knowledge & Clarify Intent**:
   - Check `.cstack/index.md` (when present) to locate Living Knowledge (including registered Architecture Maps, Cross-End Contracts, or Design System docs) and the active Change file (`<changes-dir>/<change-id>.md`—including any deferred `[R-*]` Refactor Proposal from Section 4.1).
   - State the intended behavior change, behavior to preserve, `Out of Scope` guardrails (from Spec), settled decisions, and genuine ambiguities.
   - **Spec Feedback Loop**: If unresolved product behavior, interaction state semantics, or domain policy ambiguity blocks design, do **not** guess—flag it explicitly and recommend resolving it via `/cs-spec` (or gate it as an unresolved What-level decision).
2. **Start from the Natural Entry Point & Select CMP Case(s)**:
   - Read task-relevant Architecture Map entries (if registered) and verify them against code; follow local signals (names, imports, types, query keys, tests, registries, contracts).
   - For broad codebase exploration, prefer delegating read-only discovery to `cstack-explorer` so raw search noise stays out of the main context window.
   - Load the matching Case file(s) from `references/cases/` (`clean-architecture.md`, `frontend-architecture.md`, both for full-stack changes, or direct CMP fallback for tiny standalone modules).
3. **Acquire Sufficient Context & Record Friction**:
   - Inspect only what is needed to determine ownership, coordinates, closure, contracts, and verification surface. Stop at trustworthy boundaries and note why excluded regions stay out.
   - If an expected route is missing/misleading, a boundary is untrusted, or a capability/state lacks a single owner, report it in the plan and append a 1-line entry to `.cstack/friction.log` (when friction tracking is active).
4. **Locate Coordinates & Classify Concerns**:
   - Assign vertical owner and horizontal layer/concern tags using the loaded Case's taxonomy (plus `ownership-needed` when vertical/state ownership is missing and `placement-review` when placement is unresolved).
5. **Confirm Modification Closure & CMP Transformations**:
   - List every artifact that must change, be checked, or stay consistent (including cross-end contracts, cache invalidation keys, URL schemas, or theme/i18n tokens where applicable). If closure is still unknown, return *"inspect more context first"* instead of speculating.
   - Note where the design applies **Reduce / Relocate / Index / Check** and check for Wrong-shape vs. Unindexed-closure trade-offs.
6. **Check Boundary Trust**:
   - Rate each touched boundary/contract (including the Backend↔Frontend seam when full-stack) as `trusted`, `verify`, or `untrusted` using the active Case's trust criteria.
   - Restoring an established boundary = How-level; changing contract/state-ownership semantics or redrawing a boundary = What-level gate.
7. **Plan Contracts, Boundaries & Semantic Comments**:
   - Keep provider, ORM, transport DTO, or imperative DOM quirks isolated outward behind clean domain/UI contracts and primitives.
   - Specify **Semantic Comments** only where programming constructs cannot express: (a) **Semantic Contracts** (invariants, side effects, CSS stacking/layout preconditions, failure semantics), or (b) **Modification Closure Indexes** (co-change pointers invisible to static types). Never plan restatement or excuse comments.
8. **Surface Unresolved Gates Only**:
   - List only What-level decisions still unresolved after checking the request, Change Spec, and existing architecture.
9. **Produce Inside-Out Execution & Owning-Level Verification Plan**:
   - Order steps inside-out per the active Case(s) (contract-hinged when spanning Backend + Frontend; drop untouched steps).
   - Pair verification with the owning level: pure unit tests for invariants/derivations/state machines, fake-port/hook/contract tests for policies & cache boundaries, integration/adapter tests, static type/lint/dependency checks, and **live browser verification** (Playwright / DevTools) whenever UI layout, CSS stacking, responsive behavior, routing, or hydration changes.
10. **Defer Routing-Index & Living Knowledge Updates**:
    - Never edit Architecture Maps or Living Knowledge merely to produce a plan, and never settle a new owner/boundary through documentation alone.
    - If authorized implementation will add or move a durable structural route, note it in the plan so `/cs-reflect` records the structural route clue in Change Section 4 and `/cs-merge` settles it into Living Knowledge at merge time.
    - Only if the user explicitly authorizes bootstrapping Architecture Map(s) as its own task, follow the topology rules in [`references/architecture-map-template.md`](references/architecture-map-template.md) (one sectioned map for unified single-app repos vs. separate backend/frontend maps + seam anchor for physically split apps/monorepos) and register their path(s) in `.cstack/index.md`.

## Final Check

Before printing the plan, verify:
1. Intent, preserved behavior, `Out of Scope` guardrails, active CMP Case(s), and settled authority are explicit.
2. Context was gathered from a natural entry point and stopped at trustworthy boundaries.
3. Ownership (including Single Source of Truth for frontend state), Modification Closure, Cross-Boundary Seam (if full-stack), and CMP trade-offs are evidence-backed.
4. Policy/contracts and volatile mechanisms are cleanly separated in the active domain's native idiom without dogmatic boilerplate.
5. Only genuinely unresolved What-level decisions are gated.
6. Implementation steps move inside-out, verification sits at the owning level (including live browser checks for visual/runtime UI changes), and Living Knowledge updates remain deferred to post-implementation governance.
