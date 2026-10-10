# Delivery Planning Workflow — v1.0

- **Status:** Accepted — explicitly approved by repository owner on 2026-10-10
- **Decision agreed:** Hybrid — Full Scope, Progressive Detail is the default planning depth, 2026-10-10
- **Thai companion:** [Delivery Planning (Thai)](./delivery-planning.th.md)
- **Date:** 2026-10-10
- **Track:** A — Build My Engineer
- **Foundation:** [Feature Delivery Lifecycle v1.0](./feature-delivery-lifecycle.md)
- **Upstream:** [Requirement Discovery v1.0](./requirement-discovery.md), [Technical Discovery v1.0](./technical-discovery.md), [Solution Design v1.0](./solution-design.md)
- **Next:** [Implementation v1.0 Release Candidate](./implementation.md) ↔ [Verification v1.0 Release Candidate](./verification.md) (shared execution loop) (iterative, not waterfall)
- **Scale:** Solo → team → product / cross-team
- **Example:** Fictional personal-finance application; public repository, no company-confidential data

## 1. Why this workflow exists

**Problem:** Even when a solution is understood, delivery can fail through oversized tasks, hidden dependencies, unowned decisions, unrealistic estimates, overcommitment, unclear verification, and invisible blockers. Some teams mistake filling a sprint for making a credible plan.

**Goal:** Convert an agreed *solution direction* into a feasible, adaptive plan of small deliverable increments, with owners, sequence, capacity assumptions, verification and risk response.

**Non-goals:** Mandate Scrum, fixed-length sprints, story points, detailed schedules for uncertain work, or AI-generated commitments. Product leadership owns value and priority; delivery teams own feasibility and execution planning.

**Key distinction:** Planning is continuous. A plan expresses today's best decision with known assumptions; it is **not** a promise that uncertainty is gone.

## 2. Inputs and planning boundary

Inputs may be links rather than copies:
- Desired outcome, agreed scope boundaries, acceptance criteria (or explicit gaps).
- Solution direction, design constraints, contracts, data invariants and relevant tests.
- Technical dependencies, risk assessment, known uncertainties and spike results.
- Priority and desired timing from requester/product owner, and team capacity/availability if relevant.
- Delivery context: solo/team, release restrictions, cross-team ownership, current work in progress.

If an unresolved issue makes effort unknowable or would materially change scope, schedule a bounded **discovery/spike task** before committing full delivery. Do not turn weak discovery into falsely precise estimates.

## 2A. Planning depth policy — Hybrid: Full Scope, Progressive Detail

**Default:** Understand the whole feature at a meaningful level, including boundaries, outcomes, major dependencies, integration/release risks and decision ownership, but detail work **progressively** in proportion to risk, uncertainty and proximity to execution. Do not require a fully elaborated task inventory for every future increment before implementation of a sufficiently understood first increment.

Three nested levels clarify what "planned" means:

| Level | Minimum useful view | When greater detail is warranted |
| --- | --- | --- |
| **Feature / end-to-end scope** | Outcome, acceptance boundaries, known/unknown impacted components, cross-repo contracts, important dependencies, risk, owners, integration/release approach and milestones | Strong external coordination, fixed cutover, regulatory obligations, irreversible changes |
| **Increment / next deliverable** | Observable acceptance/learning goal, owner, required interfaces and dependencies, appropriate verification and rollback/recovery implications | Near-term delivery; high-impact or uncertain increments |
| **Task / execution steps** | Concrete steps and completion evidence where useful to coordinate or execute; exact file lists only if supported by discovery | Increment starting now, hand-offs between engineers, non-routine migrations, compliance traceability |

**Full breakdown is allowed and often efficient** for small, well-understood changes or externally constrained work. It is **not mandatory** for distant, volatile work. Neither a complete high-level scope nor a complete ticket inventory implies perfect knowledge.

### Depth decision table

| Risk / uncertainty | Suggested planning depth | Safety boundary |
| --- | --- | --- |
| Low risk, stable requirements, reversible | Full breakdown can be faster; one issue may suffice | Basic acceptance and verification remain explicit |
| Moderate risk or changing details | Full feature-level scope; detailed first increment; outline later increments | Record unknowns and review triggers |
| High-risk / multi-team / migration / committed cutover | Detailed dependency, compatible-contract, release/rollback and ownership plan **across the whole affected change**; progressively refine implementation subtasks | No implementation/deployment that crosses an unresolved critical safety or coordination dependency |
| High uncertainty affecting solution/scope | A bounded spike or discovery increment first | Do not promise detailed effort or task count without evidence |

**Risk dimensions:** impact of failure; reversibility; privacy/security/compliance; data correctness; integration breadth; external dependencies; date constraints; maturity of tests/observability. A tiny-looking change can be high risk.

**Replanning triggers:** verified new dependencies; contract changes; spike findings; failed acceptance/quality evidence; significant priority or capacity shifts; newly discovered migration or production risks. Replan with the relevant decision owners; update the same shared work view.

**Anti-patterns:** full breakdown built on guesses; rolling-wave without end-to-end contract/release awareness; delaying a safe spike until every ticket is defined; silently pushing necessary test or operational work to the end.

## 3. Seven activities (repeat as conditions change)

| Activity | Practical questions | Lean outcome | Problem reduced |
| --- | --- | --- | --- |
| **1. Confirm delivery target** | What outcome, acceptance and smallest useful slice matter first? What is fixed vs negotiable: scope, date, capacity, quality? | Explicit goal, initial slice and constraints | Building work nobody can accept |
| **2. Slice the work** | Can users or systems verify an end-to-end increment? What must be done to ship safely, including tests/monitoring/migration? | Small vertical slices with acceptance | Large, invisible and late-integrating tasks |
| **3. Map dependencies & sequence** | Which work truly depends on what; which contracts or external approvals are required; what can run concurrently? | Dependency/critical-path sketch and integration milestones | Blocked work and cross-repo surprises |
| **4. Estimate uncertainty & effort** | What is known vs speculative? Is an estimate useful here? What assumptions, ranges and confidence apply? | Relative size or range with assumptions, or explicit no-estimate rationale | False precision and unjustified deadlines |
| **5. Match capacity, ownership & priority** | Who does what, who decides, who reviews? What existing commitments, leave, interruptions and bottlenecks constrain us? | Feasible candidate plan with owner and realistic WIP | Overcommitment and diffusion of responsibility |
| **6. Agree delivery & verification strategy** | How will work merge, test, integrate, deploy and recover? What defines done? Where should approvals or demos occur? | Definition of Done, test/release approach, checkpoints | Work "done" before usable and safe |
| **7. Execute, inspect & replan** | What's completed, blocked, changed or learned? Is the plan still credible? What outcome was delivered? | Updated work view, risks, forecast and decisions | Stale plans, silent slippage, status theater |

Activities are **not a waterfall**. Slice, test assumptions, implement, learn, then refine the next slice. A single small issue may encompass all relevant activities.

## 4. Work decomposition: value first, components second

Default approach: **vertical slicing** — small, integrated increments that validate a user outcome or a critical technical assumption. A slice need not ship to all customers immediately; it should be demonstrably integrated and testable.

Avoid making the only progress units "backend complete", "frontend complete", "QA later". Those can be **subtasks/ownership** inside a verifiable feature slice, but are weak indicators of customer value by themselves.

Each actionable work item should minimally answer:
- **Why / outcome:** what acceptance or learning goal does it advance?
- **What / boundary:** expected behavior and important exclusions, linked to design.
- **Done evidence:** tests, review, integration or demonstration required.
- **Owner:** one accountable driver and collaborating/reviewing roles where needed.
- **Dependencies / unknowns:** blockers or decisions that must precede work.

**Spike rule:** A spike has a bounded question, time/effort limit and evidence/decision output, **not** an implied commitment to implement the trial code.

Include non-feature work when essential: security, testing, migrations, observability, accessibility, documentation, rollout and support. Do not append invisible quality work after the capacity has already been committed.

## 5. Dependencies and parallel work

For multi-repository/multi-team delivery:
1. Identify the feature-level source of truth and contract owners.
2. Record affected repos/services and contracts only when supported by discovery evidence.
3. Agree compatible API/event/schema changes, consumer/producer ordering and integration tests.
4. Plan deploy/migration/rollback sequence where needed (e.g., expand → migrate → contract).
5. Make external coordination/permissions visible as dependencies with named owners.
6. Explicitly mark unknown repo boundaries rather than inventing a complete system map.

Parallel work is safe only when interface assumptions and integration checkpoints are explicit. Avoid optimizing everyone for 100% utilization at the expense of flow.

## 6. Estimation, forecasting and capacity

**Estimates express uncertainty; they are not performance ratings or guarantees.**

Choose a method useful to the situation:
- **Small/solo:** order work, use rough size or timebox; no story points required.
- **Team:** discuss ranges, reference similar completed work and record important assumptions. Story points are optional and team-local; never convert them mechanically to developer productivity.
- **Larger product:** use throughput/cycle-time history, probability ranges, capacity and cross-team dependencies to forecast; distinguish forecast from external commitment.

If history is insufficient, say so, use a range/scenario and plan an early checkpoint. Do not fabricate numeric estimates or assume all developer hours are uninterrupted delivery capacity.

Account for reviews, testing, coordination, interrupts, support/on-call, time off and learning work. Keep slack for uncertainty, especially high-risk changes.

**Change handling:** if scope, dependency or confidence changes materially, make the trade-off explicit with the product owner/team: adjust scope, timing, resources or risk assumptions. Never silently trade away essential quality to preserve a date.

## 7. Delivery adapters: Scrum, Kanban, personal flow

The **plan** (slice, owner, evidence, dependencies, risk) is portable. The scheduling/coordination adapter varies.

| Model | Planning and inspection | Avoid |
| --- | --- | --- |
| **Personal** | Ranked small queue, few items in progress, self-review and checkpoint when assumptions change | Writing detailed sprint artifacts for a one-person task |
| **Scrum** | Sprint Goal, appropriately sized selected backlog, daily adaptation, review, retrospective. Scope negotiated through the team; release not necessarily bound to sprint end | Treating story-point totals as commitments or quality evidence |
| **Kanban** | Visualize flow, limit WIP, manage blockers, replenish/inspect based on flow and service expectations | Unlimited parallel work, treating a board as the whole process |
| **Multi-team/product** | Integrate team-level execution with roadmap milestones, shared contracts and ownership, and dependency/risk reviews | Centralized task micromanagement and fake precision |

Team events are tools for coordination, not universal engineering activities. Work can be discovered and replanned within an iteration. No mandatory sprint length.

## 8. One core artifact: Delivery Plan / Shared Work View

**Consumers:** Engineer knows next action and acceptance; reviewer/QA knows verification duties; Tech Lead sees risk/dependencies; Product Lead sees slice outcomes and trade-offs; affected owners see their decisions.

Prefer the team's issue tracker with links to earlier summaries. A giant parallel Markdown plan is not required. For personal work, one issue with a few subtasks may be enough.

```markdown
# Delivery Plan — <Feature>
Status: Proposed | Agreed | Active | Replanning | Complete
Links: requirement, technical discovery, solution design
Delivery goal / current smallest valuable increment:
Product decision owner / engineering delivery owner:
Time/capacity assumptions (if applicable):

## Planning depth and next checkpoint
Mode: Hybrid — Full Scope, Progressive Detail (default); exceptions with reason.
Feature-wide known scope, critical unknowns and cross-team integration/release dependencies:
Next detailed increment / readiness checkpoint:
Later increments (outline, assumptions, refinement trigger):

## Work slices
Slice | outcome / acceptance | owner | dependencies | verification | status

## Integration and release
Important contracts / integration checkpoints / migration or rollout ordering.

## Estimates / forecast (optional)
Method, range/confidence, assumptions, next review point.
Never invent precision.

## Risks & active blockers
Risk / consequence | mitigation or next experiment | owner | review trigger

## Decisions and changes
Scope/time/capacity trade-offs, decider, date and rationale.

## Progress / outcome
What is integrated and verified? Next deliverable? What changed?
```

Create separate tickets for work items **when they help ownership, execution or traceability**. Maintain a single linked source of truth rather than duplicated issue lists and documents.

## 9. Planning readiness and checkpoints

Planning is **credible enough to start the next safe increment** when the **whole-feature scope and critical risks have been scanned**, and:
1. A valuable or informative increment and its acceptance/learning goal are explicit.
2. Its owner, key reviewers, actual dependencies and major unknowns are visible.
3. The selected solution/contract boundaries are understood enough for that increment.
4. Appropriate verification, integration and release implications are included.
5. Capacity and timing claims, if made, explain assumptions and uncertainty.
6. A product/engineering decision owner has resolved critical scope and technical conflicts, or assigned a bounded follow-up.
7. Critical future integration, migration, security or rollout constraints are visible at feature level even if later task details remain provisional.
8. Planning mode and a trigger for refining the next increments are clear.

**Outcomes:**
- **Agreed for first increment:** begin implementation and continuously inspect.
- **Start with spike:** test a high-impact unknown before making broader commitments.
- **Return to solution/technical/requirement discovery:** missing evidence changes the plan.
- **Defer or de-scope:** value or available capacity does not justify current investment.

**Important:** This checkpoint is not a promise that an entire product is implementation-ready. Safety, code review, tests and authorized deployment have their own subsequent FDL gates.

## 10. People and decision boundaries

- **Product owner/lead:** business priority, value, scope trade-offs, target outcomes.
- **Delivery team/engineers:** decomposition, technical estimates, implementation and quality work.
- **Tech Lead:** cross-component sequencing, technical risk, design coherence, coaching and escalation; avoid becoming sole approver.
- **Engineering Manager / delivery manager (when present):** staffing, availability, competing obligations, people support.
- **QA/security/operations/service owners (as relevant):** verification constraints, policies and release readiness.

Assign one accountable owner per action but do not equate *accountability* with doing all the work. Surface dependency conflicts early and designate a decision maker; empower engineers within agreed boundaries.

Progress tracking should describe **flow, outcomes and blocked work**, not rank people by tickets, hours, points or commit counts.

## 11. Fictional personal finance application example

**Candidate change:** Add bank statement import to help a user understand monthly spending. This is a learning example, **not** validated MVP scope or an architecture decision.

**Conditional assumption for planning exercise:** An upstream design review has tentatively selected CSV import with preview/confirmation for the first experiment. Actual supported formats and user willingness remain unproven.

Illustrative vertical slices:
1. **Feasibility spike:** Use *synthetic* CSV fixtures; assess parse reliability for dates, amounts, malformed rows and known format variants; output decision evidence.
2. **Preview increment:** Parse a supported synthetic format and display normalized transactions and errors **without persisting canonical transactions**; validate UI comprehension.
3. **Confirmed import increment:** After explicit user confirmation, persist import with deduplication rules and tests for retry/partial failure.
4. **Financial-summary verification increment:** Ensure transfers, refunds and internal account movements do not inflate income/expenses, with evidence and clear labeling of incomplete cash data.
5. **Operational readiness:** Access controls, storage/retention behavior, monitoring, error recovery and release checks suited to privacy/data risk.

These slices may be reordered/refined after experiments; they are not guaranteed independent releases.

**Possible cross-functional ownership:** One engineer drives ingest, one works on preview UI, both agree on API contracts; QA/peer verifies critical scenarios and technical owner reviews data correctness. In solo mode, the same person may hold multiple roles.

**Do not assert:** real bank compatibility, supported CSV formats, fixed story points, person-days, or guarantees of balance accuracy without evidence.

## 12. AI opportunities (no Skill yet)

AI may **suggest** slices, dependency questions, risk checklists, test cases, and summarize an authorized work board. It may compare scope changes against the agreed design and flag missing owners.

AI must not invent estimates, make sprint commitments, assign people without consent, change agreed priority silently, or claim an untested slice is done. Human owners confirm task allocation, capacity, business priority, release readiness and forecast communication.

Create a `puen-stack` Skill only when pilot work demonstrates recurring toil that a reusable procedure can actually reduce.

## 13. Evaluate and refine

Pilots should include:
- **Tiny reversible task**: was planning light enough?
- **Medium-risk integrated feature**: did slices expose integration/verification work early?
- **Cross-repo or cross-team scenario**: were dependency, ownership and contract sequencing visible?

Measure: blocked time, WIP, unplanned scope changes, integration surprises, forecast reliability where meaningful, review/rework and overhead to maintain plan. Assess at the team/system level, not individual ranking.

## Approval review checklist — v1.0 Release Candidate

1. Does Hybrid — Full Scope, Progressive Detail expose the full risk/contract map while avoiding speculative detailed tickets?
2. Are low-risk full breakdowns and high-risk cutover exceptions sufficiently clear?
3. Does **Agreed for first increment** block unresolved critical dependencies without requiring every later task?
4. Are Jira-first source-of-truth, forecast uncertainty, and named decision owners practical across solo/team?
5. Can cross-repo integrations share one feature-level view with linked per-repo issues, without double maintenance?

**Release Candidate pending explicit owner approval.** No automated staffing, sprint commitments, Jira integration, or new Skill is approved by this document.
