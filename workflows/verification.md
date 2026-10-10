# Verification Workflow — v1.0 Release Candidate

- **Status:** Review Ready / Release Candidate — awaiting owner approval; not Accepted
- **Decision:** Separate Implementation and Verification with one shared execution loop (2026-10-10)
- **Date:** 2026-10-10
- **Track:** A — Build My Engineer
- **Related:** [Implementation](./implementation.md), [Delivery Planning v1.0](./delivery-planning.md)
- **Foundation:** [Feature Delivery Lifecycle v1.0](./feature-delivery-lifecycle.md)
- **Thai companion:** [Verification (Thai)](./verification.th.md)
- **Next:** Release & Operations (to be designed)

## Unit of verification: assigned Work Item (Track A decision)

**Work Item = existing Jira Story, Sub-task, Bug or Task that the engineer actually owns**, with explicit acceptance and observable proof. A backend engineer may own only Merchant Service (BE), only Admin BFF, or an agreed BE+BFF task; FE may belong to another developer. Verification **must assess the assigned Work Item**, not silently expand the actor's coding responsibility to FE or all sibling repos.

**Before code:** derive expected behavior, negative cases and a realistic proof seam from the parent Story's approved acceptance/contracts, but make assertions specific to the owned Work Item. A BE-only task can verify persistence/API semantics, access control, idempotency and the exposed contract **without pretending that FE integration ran**. A BFF task can verify mapping, error semantics and consumer/provider contracts with controlled fixtures or actual integration as appropriate.

**After code:** local Work Item verification includes actual behavior, unit-test quality, scoped integration/contract evidence and independent diff review proportionate to risk. **Story-level integration verification** of BE ↔ BFF ↔ FE is a separate **coordination/evidence responsibility** when applicable, owned by the relevant team and scoped through Jira; it is **not a universal extra Human Gate** and does not automatically force the backend engineer or AI to implement unassigned FE work. Record **Not run / Blocked / external owner** for missing end-to-end evidence. A passed Work Item **does not** mean the full Story is accepted.

**Fixed approvals:** Feature/Story Design and Planning provide shared direction; **each agreed Work Item** receives Implementation and Verification Human Gates under Track A. When a Work Item is itself a standalone Story, record distinct decisions against that same Jira item; accepted Delivery Planning may still use *increment* as a delivery-slice planning term. See [Track A decisions](./ai-assisted-feature-delivery.md).

## Purpose, timing, separation

Establish **credible evidence that the change meets acceptance, respects important system boundaries and has been critically reviewed**. Verification is a distinct workflow with its own responsibilities and exit decision. It may begin **during implementation** and repeatedly send findings back; it is not a final waterfall phase.

Implementation owns making the change and developer checks. Verification owns evaluating acceptance, failures, cross-component behavior and review findings. Roles can overlap for solo work, but distinguish an independent assessment from an implementer's self-check. AI review alone is not equivalent to independent human judgment.

## Shared execution loop — normative coordination contract (v1.0 candidate)

The two workflows remain **separate responsibilities**, but use **one iterative execution loop** for each coherent work item:

```text
Approved Feature design / plan → agreed Work Item
   ↓
Verification-FIRST: acceptance examples, expected outcomes, test seams
   ↓
Implementation: vertical slice → TDD when useful → developer checks
   ⇄ Verification: independent behavior proof, test quality, review
   ↳ Findings → scoped fix → targeted re-verification
   ↓
Human Work Item Verification Gate → separate release decisions
```

**Start before code:** Verification first defines trustworthy expected behavior, example cases and observable proof based on confirmed requirements; it may then review API/contracts, test strategy or partial diff; a "Ready for Verification" label is a convenience, **not** a mandatory wait-for-all-code gate. Developer tests remain part of Implementation; independent evaluation and evidence judgment are Verification's responsibility.

**One shared handoff in the existing issue/MR** (not a new obligatory artifact):
- **Identity & scope:** work item, acceptance IDs, repo/diff links and known exclusions.
- **Changes & risks:** touched boundaries, contract/data changes, migration and critical failure cases.
- **Evidence:** actual executed checks, outcomes, environment, explicitly *not run / blocked* checks.
- **Feedback:** findings with severity/evidence/owner; changed-file/test impact; retest outcomes.
- **Decision:** continue implementing, ready for further verification, fix & reverify, blocked or verified for release review; authorized human owns residual-risk acceptance.

**Loop rules:** Only the touched behavior and dependent contracts need proportional re-verification after a fix; revisit broader checks when the change has wider impact. Any newly discovered high-impact unknown returns to Discovery/Design/Planning. Neither workflow can authorize deployment, silently alter acceptance or override required human review.

**Roles:** Solo engineers may perform both with deliberate separate passes; medium/high-risk changes should seek a genuinely independent reviewer when feasible or required by local policy. AI models can suggest code/findings, but agreement among agents is not evidence that checks executed or humans approved.

**Scaling:** A trivial reversible change may record the whole loop in a short MR note; a multi-repo or security/data-critical feature needs explicit contract, test, review and release-risk evidence. Avoid duplicate tickets/docs.

## Verification-first ownership (agreed design direction; v1.0 candidate)

**Before Implementation:** Given confirmed requirements, domain rules and approved contracts, Verification defines observable **acceptance scenarios, expected results, negative/boundary cases, test seams and proof surfaces**. These are an independent oracle, not outcomes inferred from generated code. Discuss ambiguity with the authorized decision owner before tests encode it.

When a reliable test harness exists, Verification may create a **small executable acceptance or contract test** ahead of implementation and demonstrate that it fails *for the intended missing behavior*. If no meaningful executable seam exists, use a reviewable example (e.g., Given–When–Then), trace or manual repro instead. A failing run caused by setup, missing credentials or broken fixtures is **Blocked/Inconclusive**, not a valid red test.

**During Implementation:** Implementation owns code, unit/regression tests and the optional **TDD red → green → refactor** loop in small vertical slices. Verification may challenge test assertions, contracts and partial changes at any point without imposing a new gate or taking over developer tests.

**After changes:** Verification executes the strongest proportionate checks on the real CLI/API/UI/data surface, tests negative cases and reviews the diff independently against the acceptance oracle and repo standards. Green unit tests or compiling are useful, **not sufficient proof alone**. Review must check whether tests would catch the actual wrong behavior. Findings return to scoped implementation/retest under Q7; material changes to acceptance or design reopen affected human gates.

**Human authority:** Verification-first describes **when to reason/test**, not permission to bypass the existing per-Work-Item Implementation and Verification Human Gates. Record scenario/expected result/check/evidence in existing Jira/MR. No mandatory new document or Skill.

**References:** [mattpocock TDD](https://github.com/mattpocock/skills/blob/main/skills/engineering/tdd/SKILL.md), [pstack verify-and-ship](https://github.com/cursor/plugins/blob/main/pstack/docs/guide/06-verify-and-ship.md).

## Formal Independent Verification entry: unit tests green first (agreed 2026-10-10)

**Two different moments:** Early Verification-first work defines source-backed acceptance, scenarios and test seams **before code**; provisional feedback may review contracts or partial diffs **during Implementation**. This is preparation, **not a formal independent acceptance judgment**.

**Formal Independent Verification begins only after Implementation has demonstrated passing applicable unit/regression tests and required local developer checks, with actual command/results/environment, and after the agreed Human Implementation Gate.** If unit tests fail, send the Work Item back to Implementation; if they did not run or the runner is unavailable, report **Not run / Blocked**. Do not label an unexecuted check green. For a change with no meaningful unit-test seam, require an explicit, justified alternative check accepted under the existing Implementation Gate, rather than a silent exception.

Once admitted, **challenge the green tests**: confirm assertions reflect agreed expected behavior, check critical negative/edge cases, exercise appropriate real API/DB/BFF contracts, and independently review the diff. **Green unit tests are necessary feedback where applicable, never sufficient proof of correctness.** When Verification finds a defect, use scoped fix → **rerun impacted developer unit tests** → renewed Independent Verification evidence; the normal Human Verification Gate still applies. This adds no gate or bypass to existing approval policy.

## Entry and risk selection

May start once a reviewable slice, test plan or interface contract exists. Read requirements/acceptance, solution constraints, diff, actual developer-check outputs, dependencies and relevant risk map.

Select depth according to impact, reversibility, data sensitivity, concurrency, external contracts and operational risk:
- **Low:** inspect relevant diff, focused tests and acceptance behavior; concise MR record.
- **Moderate:** add negative cases, integration checks and a deliberate independent review.
- **High:** explicit security/data/contract/migration scenarios and accountable domain/release reviewers; preserve formal evidence when required.

Unclear implementation state or missing credentials are recorded as blockers, not filled with AI assumptions.

## Verification activities (repeat when changes land)

| Activity | What to check | Evidence |
| --- | --- | --- |
| 1. Define acceptance first | Before coding, derive expected outcomes, negative cases and test seams from confirmed requirements/contracts; map them to checks | Source-backed scenario → expected outcome → proof surface |
| 2. Validate test design and runs | Optionally create a small executable acceptance test; assess implementer test assertions and execute relevant checks | Intended failure vs harness failure; actual commands/results/environment |
| 3. Probe critical failures | Check permission, malformed input, concurrency, retry/idempotency, error/timeout, boundary behavior as relevant | Negative-case evidence and residual risk |
| 4. Validate integration | Verify contracts/API/schema, producer-consumer compatibility, migration and version skew | Integration/contract tests or owned gap |
| 5. Review independently | Review diff for correctness, maintainability, unintended change, safety and test gaps | Finding severity, file/evidence, impact, recommendation |
| 6. Reconcile findings | Send findings to Implementation; retest affected behavior after fixes | Findings resolved, accepted with owner or blocked |
| 7. Decide handoff | Determine verification state, residual risks and release-readiness handoff | Explicit decision and responsible human |

## Prove it works — pstack and mattpocock adaptation (candidate for v1.0)

Adopt the **evidence discipline** from [pstack Verify & Ship](https://github.com/cursor/plugins/blob/main/pstack/docs/guide/06-verify-and-ship.md), [pstack's adversarial review](https://github.com/cursor/plugins/blob/main/pstack/skills/interrogate/SKILL.md), [pstack verification-skill generator](https://github.com/cursor/plugins/blob/main/pstack/skills/create-verification-skill/SKILL.md) and [mattpocock two-axis code review](https://github.com/mattpocock/skills/blob/main/skills/engineering/code-review/SKILL.md). **These are techniques, not required vendor tools.**

### 1. Prove the changed behavior on the matching surface

Define the observable finish condition **before** implementation. For verification, distinguish proxies (build/typecheck/unit-test-only pass) from evidence through the relevant user or system interface:

| Change | Stronger behavior evidence |
| --- | --- |
| CLI / job | Execute the real command/job using representative safe input; compare literal output, exit status and side effects |
| API / data | Exercise contract/authorization/error semantics and read back the stored outcome when safe |
| Web / UI | Walk the changed live flow and capture key states/errors; consider screenshots/video when they add actual evidence |
| Refactor / migration | Replay representative before/after inputs and examine contract/data compatibility |
| Performance | Vet baseline versus result under comparable runtime/configuration, repetitions, bottleneck and end-to-end impact |

The required depth scales to risk; **not every change needs browser video or a full E2E suite**. When real execution is inaccessible, report **Inconclusive / Blocked / Not run** with why and who can resolve it; never replace it with an invented success claim.

### 2. Review against two distinct axes (plus consequential risks)

**Spec / Acceptance:** Does the diff actually satisfy the originating issue and observable scenarios without unauthorized scope expansion?

**Repository Standards / Quality:** Does it respect documented repo patterns and relevant maintainability constraints? Separate verifiable violations from judgment calls; do not turn style preferences or generic code smells into mandatory bans.

Add risk-focused lenses only when relevant: auth, privacy, data correctness, concurrency, retries, error handling, observability, migrations and cross-service contracts. Use a **fixed diff base and source-backed spec**; missing spec or unavailable environment is a gap, not something reviewers should hallucinate.

An independent skeptical pass or second model can supplement a human reviewer where useful. Categorize findings as **Act on / Consider / Noted / Dismissed**, retaining rationale and evidence; no finding is auto-applied merely because multiple agents agree. Human reviewers determine applicability and sign-off under local policy.

### 3. Make verification repeatable only when justified

When repeated runs require manual UI/CLI/API operation, evaluate a **project-specific verification harness or Skill** built on existing tools. Candidate contract: **Launch → Doctor/health check → Drive real behavior → Capture Evidence → Cleanup owned resources**. Prefer repo-local existing test tools and safe seed fixtures; isolate parallel runs; prove generated instructions work once end-to-end before relying on them. Maintain a feature map only if it brings value.

Do **not** automatically generate a `.cursor/skills` tree or mandate daily maintenance for My Engineer. A future `puen-stack` skill requires approved access, a successful pilot, ownership/maintenance plan and an evaluation against current verification practice. A skill that merely wraps the same unreliable steps does not create evidence.

## Approval granularity and safe loopback (agreed for Track A)

**Feature-level human gates:** Solution Design approves the agreed technical direction/major contracts; Delivery Planning approves whole-feature scope, critical risks/dependencies and progressive detail of upcoming work items. **Per-Work-Item human gates:** Implementation approves each reviewable change scope/diff before formal publication; Verification approves **each Work Item's** actual acceptance evidence, review and residual risks.

For later work items within the **approved Feature scope**, progressively refine acceptance and verification checks without reopening Design or Planning just because the next work item starts. A Work Item can cover coordinated changes across multiple repositories: retain one feature-level work view with repo-specific diffs/MRs and evidence of integration.

**Q7=B loopback:** Verification findings inside that Work Item's approved scope/design/risk can go to Implementation for fixes and proportionate retesting, **without reapproval of each edit or another feature-level gate**. The human still decides the Work Item Verification Gate. When a finding materially alters approved requirements/scope, architecture, critical API/data contract, security/data risk or delivery constraints, **reopen only the invalidated upstream human gate(s)** before proceeding.

Record Feature and Work Item approvals and risk owners in existing Jira/MR links. **Each Draft MR** must be individually confirmed by the human after checking repo/task-specific Git Flow, regardless of the number of repos in the Work Item. A verified work item is not permission to mark the MR Ready, merge or deploy; those decisions remain human-owned.

## Fixed Verification Gate, risk-based review and scoped rework (AI-assisted v1.0 candidate)

**Q4=B — Risk-based independent review:** Low-risk changes may use a distinct self-review pass if the local team permits it; moderate changes benefit from an independent diff review; high-risk changes require qualified human peer/domain review when policy or risk demands. The **human Verification Gate is required for every agreed Work Item**, regardless of review depth.

**Q5=B — Evidence by change type:** Show acceptance behavior on the appropriate API, UI, CLI, stored-data or other real surface, with applicable automated and negative/integration checks. Missing runs remain **Not run / Blocked / Inconclusive**, never silently treated as passes. Review the actual diff and the originating specification as two distinct axes.

**Q7=B — Scoped Rework:** Reviewer findings within the approved Implementation scope/design/risk envelope return to Implementation for focused fixes and proportionate re-verification. Do **not** reapprove every edit; the human confirms residual findings/evidence at the Verification Gate. A material change to requirements, scope, contracts, design or security/data risk must **reopen affected earlier Fixed Gate(s)** before that new direction proceeds.

**Q6=A / Q8=B / Q9=A — MR sequence:** Formal verification and MR/CI review follow the human-approved Implementation Gate. AI may push scoped follow-up fixes to the specifically authorized non-protected branch under its recorded publish permission; **every Draft MR creation requires a separate, explicit human confirmation of repo-specific source/target Git Flow**. No assumption of `master`. **Only a human** may mark a Draft MR Ready or Merge, and release/deploy still needs separate authorization.

**Use existing Jira/MR evidence:** record actual checks, review findings, resolution/owners and gate approval without a duplicate required document. Consult [AI-assisted delivery decisions](./ai-assisted-feature-delivery.md) for Q1–Q9.

## Evidence discipline

Distinguish **Proposed** (suggested check), **Executed** (real test/observation), **Reviewed** (assessed evidence) and **Accepted** (authorized risk acceptance). Do not present one as another.

A credible record answers:
- What behavior was checked? Against which acceptance case?
- How and where was it checked? What did the test actually assert?
- What failed or was not run? Why?
- Were changed contracts exercised in combination, including realistic failure modes?
- Who reviewed and resolved findings, and who accepts remaining risk?

For a minor reversible edit, concise MR notes plus execution results suffice. For high-impact financial or authorization changes, deepen evidence and independent review.

## Independent review contract

Look at requirements; contract and schema compatibility; domain invariants; privacy/authorization; errors/timeouts/retries; concurrency and data integrity; tests and false positives; observability and recovery; relevant performance; deployment concerns.

Record actionable findings as **Severity | location/evidence | failure scenario | recommendation | resolution/owner**. Prefer substantive risk over stylistic preference. No findings never proves correctness.

**Independent AI review:** A separate prompt/context or model can be a useful *candidate* technique, not a guaranteed independent audit. A human engineer must assess high-impact conclusions and own risk acceptance.

## Cross-repo verification

Use one feature-level view with per-repo MR links. Validate changed producer/consumer behavior and release compatibility from actual contracts. An agent or tester with one repo checked out has **not** automatically seen other repos. Plan explicit integration test ownership; do not invent the safe deploy order.

## Lightweight record (Jira/MR first)

```markdown
## Acceptance coverage
Scenario | evidence / executed result | gap

## Checks
Command or manual procedure | environment | pass/fail/not run | link

## Independent findings
Severity | file/evidence | impact | resolution | owner

## Contract / rollout
Compatibility, integration, migration, remaining verification

## Decision
Verified for release review | Fix & reverify | Blocked | Stop/defer
Human decision owner / known limitations:
```

## Exit decisions and loopback

- **Verified for release review:** proportionate evidence exists, critical findings addressed, residual risks assigned. **Does not authorize deployment.**
- **Fix & reverify:** return to [Implementation](./implementation.md), then re-run impacted checks.
- **Blocked — discovery/design:** contradictory requirements, unsupported contracts or unsafe unknowns.
- **Stop/defer:** unacceptable risk or insufficient basis for acceptance.

No requirement to finish all implementation tasks before starting verification; each coherent work item can pass through several loops.

## Approval checklist — v1.0 Release Candidate

1. Is Feature-level Design/Planning versus per-Work-Item Implementation/Verification approval clear, without unnecessary repeat approvals or bypasses?
2. Are the two workflow responsibilities clear while enabling review during implementation?
3. Can the shared handoff fit existing Jira/MR without separate mandatory artifacts?
4. Are evidence, independence, risk acceptance and release authority distinguished?
5. Are real repository permissions and AI Skill creation separate authorizations?

## Pilot and open decisions

Pilot tiny reversible change, medium integrated feature and a simulated multi-repo contract change. Observe defect detection, rework, integration surprises, false-positive findings, time-to-feedback and paperwork burden.

**Final review focus:** Confirm acceptance expected results are source-backed, not generated from implementation; TDD is preferred where useful but not mandatory. Name the actual human Verification Gate owner per team and agreed work item; make evidence/risks accessible through existing Jira/MR; honor repository-specific required CI, reviewers and branch policies. Pilot the proposed solo/medium/high-risk review depth before turning it into an enforced Skill.

**Release Candidate pending owner approval.** No release authority, automated merge, repository policy or AI skill created.
