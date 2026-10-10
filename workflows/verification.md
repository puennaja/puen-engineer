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

Establish **credible evidence that the change meets acceptance, respects important system boundaries and has been critically reviewed**. Verification is a distinct workflow with its own responsibilities and exit decision. **Verification-first preparation and preliminary feedback** may begin before or during Implementation and repeatedly return findings; **formal Independent Verification** begins only after passing applicable developer checks and the Human Implementation Gate. This is an iterative responsibility loop, not a mandatory waterfall for each edit.

Implementation owns making the change and developer checks. Verification owns evaluating acceptance, failures, cross-component behavior and review findings. Roles can overlap for solo work, but distinguish an independent assessment from an implementer's self-check. AI review alone is not equivalent to independent human judgment.

## Shared execution loop — normative coordination contract (v1.0 candidate)

The two workflows remain **separate responsibilities**, but use **one iterative execution loop** for each coherent work item:

```text
Agreed Work Item within approved Feature/Story decisions
   ↓
Verification-first: source-backed acceptance + test intent (before code)
   ↓
Implementation: slices + optional TDD → unit tests PASS + developer checks
   ↓
HUMAN IMPLEMENTATION GATE: scope, diff and test evidence
   ↓
Scoped publish / Draft MR if needed (human approves EACH MR)
   ↓
FORMAL INDEPENDENT VERIFICATION: code review + behavior + test quality
   ↳ Findings → scoped fix → rerun affected unit tests → reverify
   ↓
HUMAN VERIFICATION GATE → separate MR completion / release decisions
```

**Verification-first preparation starts before code:** define trustworthy expected behavior, examples and observable proof from confirmed requirements. **Preliminary feedback** may assess API/contracts, test strategy and partial diffs before the Implementation Gate; this is **not formal Independent Verification**. A "Ready for Verification" label is useful for handoff but does not bypass the formal entry rule below. Developer tests remain Implementation-owned; formal independent evidence judgment follows the agreed entry condition.

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

## Verification execution-safety preflight — required before running side-effecting checks (v1.0 review amendment)

**Preflight is a safety check, not a new Human Gate.** Before any live API call, integration test, database write/migration, job, queue/event publication, external notification or cleanup, identify and confirm:

1. **Scope and target:** the agreed Work Item, exact service/repository, environment/endpoint/database and whether each proposed check is read-only or has side effects. Confirm where data, events and logs will flow, including downstream services and other users.
2. **Authorization and environment:** use a **specifically authorized local/isolated test or non-production environment** with synthetic/sanitized fixtures by default. Access to an environment or possession of credentials is **not** permission to mutate it. Do **not** execute production or production-connected tests (even ostensibly read-only probes) without separate explicit authorization and a team-approved safe procedure. Production changes, migrations, real payments, external communications and privileged/destructive commands require their own permitted owner-approved process; this workflow grants none.
3. **Isolation and data safety:** avoid real financial/personal records, secrets, uncontrolled external integrations, destructive fixtures and shared-state collisions. For write/async tests, verify safe test identities, transaction/rollback or cleanup plan, and downstream effects. **Cleanup only resources created/owned by this verification run**; never delete unknown or pre-existing data.
4. **Stop condition:** if the target environment, permissions, data ownership, side effects or cleanup boundary are unclear, **do not execute** the unsafe check. Report **Blocked / Not run**, why, and who must authorize or provide a safe alternative. Continue independent **read-only** spec/diff review where legitimately authorized, without claiming behavioral proof.

Record the checked target, permissions/limitations, fixtures, commands, observed results and side-effect/cleanup status in the existing Jira/MR evidence. Do not copy credentials or sensitive payloads into public reports. Follow stricter repository/company policies and the [Accepted Implementation Working-tree Safety](./implementation.md) for any related edits.

## Independent Verification v1.0 — four agreed safety nets (baseline design decision, 2026-10-10)

After the applicable unit/regression tests and required developer checks **actually pass** and the authorized human approves the Implementation Gate, formal Independent Verification evaluates these **four distinct questions** for the assigned Work Item. **All four dimensions are considered; the checks and depth within each depend on risk and applicability.** Passing unit tests grants admission, **not** independent acceptance.

| Safety net | Question and minimum output | BE / BFF examples |
| --- | --- | --- |
| **1. Requirement verification** | Does the work-item behavior match confirmed acceptance and parent-Story contracts? Map important scenarios to evidence; identify omissions, disputed expectations and out-of-scope changes. | Activity-log actor/action/time; response/error semantics; unauthorized access expectations |
| **2. Independent code review** | Is the diff correct, safe and consistent with the repo? Review spec alignment and maintainability/architecture **as separate axes**, with actionable findings and impact. Inspect developer-test quality, not just green counts. | Transactions, data integrity, error handling, security, side effects, excessive coupling, weak assertions |
| **3. Risk-based behavior/integration proof** | What actually works on the changed observable boundary? Execute the **strongest proportionate** API/DB/contract/integration or other real-surface checks; record what could not be exercised. | Update status → persisted log read-back; BFF ↔ BE mapping/contract; permission and retry/failure scenarios as relevant |
| **4. Evidence and risk assessment** | What was actually tested, what failed or remained untested, and what risks remain? Provide concise **Pass / Fail / Not run / Blocked / Inconclusive** results, links, responsible owners and a decision recommendation for the human. | Exact test commands/results/environment, behavior observations, findings/resolution, externally owned integration gaps |

**Proportionate depth:** Low-risk changes need acceptance mapping, meaningful test-quality/diff review and a traceable evidence decision; execute focused behavior checks where an applicable seam exists. Medium-risk changes warrant stronger real-behavior/contract or integration proof and deliberate independent review. High-risk changes (authorization, sensitive data, concurrency, migrations, money, destructive or hard-to-reverse effects) need tailored negative/security/data/integration checks and the qualified human reviewers required by risk/team policy. A three-line authorization change can be high-risk; diff size alone is not risk.

**No checkbox theater:** Build/lint/green unit tests are **developer evidence**, not a substitute for these four independent judgments. Tests whose assertions cannot detect a relevant wrong result must produce a finding. Mutation, property-based, fuzz, load and deep security testing are **optional targeted techniques**, not mandatory on every Work Item. A reviewer must not infer unexecuted behavior from an AI narrative or declare the parent Story complete when its cross-owner integration is unverified.

**Decision boundary:** Independent Verification may recommend **Verified for release review / Fix and reverify / Blocked / Stop or defer**; the **per-Work-Item Human Verification Gate** accepts or rejects the evidence and residual risk. No automation, MR Ready transition, merge or deploy permission follows merely from a green report. Record this in the existing Jira/MR rather than adding a mandatory report file.

## Entry and risk selection

**Preparation / preliminary review** may start as soon as an acceptance example, test plan, contract or reviewable partial diff exists. **Formal Independent Verification** starts **only after** actual passing applicable unit/regression tests and required developer checks **and** the Human Implementation Gate, subject to the documented no-unit-test-seam exception in the entry rule above. Before formal checks, review source-backed acceptance, a **fixed diff/commit base**, observed developer-check outputs, dependencies and risk map; **complete the execution-safety preflight defined above** before running live or side-effecting checks.

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

**Independent AI review:** Different prompt/context or model is a useful technique, **not a claim of genuine independent human review**. Follow the separate-review-inputs contract above. The authorized human owns high-impact conclusions, risk acceptance and the Verification Gate.

## Independent assessment separation and reviewer inputs (v1.0 review amendment)

**Before a formal review pass**, provide the reviewer with (a) confirmed Work Item/parent Story acceptance and excluded scope, (b) actual **diff and fixed base/commit**, (c) relevant repository contracts/standards, (d) developer-check outputs **as evidence to challenge**, and (e) changed risk boundaries. **Do not substitute the implementer's summary, recommendations, or “all tests pass” assertion for these primary inputs.** Check important expected outputs directly against the approved source, not AI-generated test expectations.

Use **two explicit review axes**: **spec/behavior correctness** (including test assertion gaps, unauthorized behavior, contract failure) and **codebase quality/risk** (maintainability, security, integration, data/operational effects). Distinguish observed defects with reproducible evidence from hypotheses/style preferences. If the same engineer or agent is both implementer and reviewer, conduct a **deliberate separate skeptical review pass** and record that limitation; separate prompts/models can reduce shared assumptions but do not guarantee real independence. **Low risk:** separate self-review may be permitted by local policy. **Medium risk:** deliberate separate reviewer/pass proportionate to risk; **high risk:** qualified human peer/domain review when required by policy/risk. The **Human Verification Gate always remains**.

## Cross-repo verification

Use one feature-level view with per-repo MR links. Validate changed producer/consumer behavior and release compatibility from actual contracts. An agent or tester with one repo checked out has **not** automatically seen other repos. Plan explicit integration test ownership; do not invent the safe deploy order.

## Lightweight record (Jira/MR first)

```markdown
## Acceptance coverage
Scenario | evidence / executed result | gap

## Checks
Required/optional + risk reason | command or procedure | authorized target/environment + fixture | expected vs observed | Pass/Fail/Not run/Blocked/Inconclusive | evidence link

## Independent findings
Severity/blocking? | spec or quality axis | file/diff-base/evidence | impact | resolution | owner

## Contract / rollout
Compatibility, integration, migration, remaining verification and external owner
Execution preflight: target, permissions, side effects, owned cleanup, data safety

## Decision
Recommended: Verified for release review | Fix & reverify | Blocked | Stop/defer
Required-check gaps / policy-approved equivalent evidence (if any):
Residual risks / owners / limitations:
Human Verification Gate owner / approval or rejection / date:
```

## Required evidence, blocking defects and outcome rules (v1.0 review amendment)

**Before running formal checks**, identify which acceptance scenarios/checks are **required** for the Work Item's actual risk, security/data impact and repository/team CI/review policy, versus optional probes. Record expected outcomes and the applicable owner/policy. Do not downgrade a required check merely because the environment or test harness is unavailable.

| Recommendation | Minimum condition / what to record |
| --- | --- |
| **Verified for release review** | Confirmed acceptance and **all required safety/behavior/contract checks** have credible executed or policy-authorized **equivalent** evidence; no unresolved blocking defect; remaining non-blocking gaps/risks are documented with owners for the **Human Verification Gate**. This is **not** full Story acceptance or release permission. |
| **Fix & reverify** | Actual failed acceptance, regression or blocking finding that can be fixed in the authorized Work Item scope; return to Implementation → rerun affected developer tests → independent recheck. |
| **Blocked** | A required check/evidence, environment, access, acceptance decision or qualified reviewer is missing/unavailable; record **Not run / Inconclusive**, blocker owner and safe next action. **Do not label the Work Item Verified** merely because other checks pass. |
| **Stop / defer** | Unacceptable/unbounded safety or correctness risk, or material design/scope conflict requiring higher-level decision; stop the unsafe direction and escalate appropriately. |

**Exceptions must be explicit:** when an ordinarily required check cannot run, only an **authorized decision maker under the actual team/repository policy** may approve a valid alternative proof or exception, with rationale, evidence limits and residual-risk owner recorded. Such approval **cannot waive a mandatory policy or convert unexecuted checks into "Pass"**. Without policy-authorized equivalent evidence, keep the result **Blocked / Stop** rather than "Verified". An optional test marked **Not run** does not automatically block if its risk is demonstrably covered by adequate evidence and the gap is documented.

The independent reviewer **recommends** a status; only the authorized human decides the per-Work-Item **Verification Gate**. Never auto-accept CI, a green test suite, an empty findings list or an implementer narrative as a decision.

## Exit decisions and loopback

- **Verified for release review:** required evidence is complete (or explicitly replaced by a policy-permitted authorized equivalent), blocking findings resolved, residual non-blocking risks owned. **Does not authorize deployment or imply full Story acceptance.**
- **Fix & reverify:** return to [Implementation](./implementation.md), then re-run impacted checks.
- **Blocked:** missing required evidence, unresolved acceptance/contract decision, unsafe/unavailable environment or insufficient authorization; record owner and safe next action.
- **Stop/defer:** unacceptable risk or a material unresolved design/scope conflict; halt the unsafe direction and escalate.

No requirement to finish an entire Feature/Story before **preliminary feedback or formal verification of an individually ready Work Item**. Formal assessment of that Work Item still requires its actual green developer checks and Human Implementation Gate; scoped fixes loop back through affected tests before re-verification.

## Approval checklist — v1.0 Release Candidate

1. Are Feature Design/Planning and Work Item Human Implementation/Verification Gates clear, and is **Preliminary vs Formal Verification** unambiguous?
2. Does **Execution Safety Preflight** require authorized environments, fixture isolation, data/side-effect limits and safe ownership-based cleanup without adding a gate?
3. Are **required check / blocked outcome / documented exception** rules strict enough to prevent false Verified decisions?
4. Are all **four safety nets** covered at risk-appropriate depth, and does a **separate skeptical review** start from source-backed acceptance and fixed diff, not implementer claims?
5. Are Jira/MR evidence and Human Gate decisions retained without duplicate artifacts, while GitLab permissions, per-MR confirmation, release and AI Skill creation remain separately authorized?

## Pilot and open decisions

Pilot tiny reversible change, medium integrated feature and a simulated multi-repo contract change. Observe defect detection, rework, integration surprises, false-positive findings, time-to-feedback and paperwork burden.

**Final review focus:** Confirm acceptance expected results are source-backed, not generated from implementation; TDD is preferred where useful but not mandatory. Name the actual human Verification Gate owner per team and agreed work item; make evidence/risks accessible through existing Jira/MR; honor repository-specific required CI, reviewers and branch policies. Pilot the proposed solo/medium/high-risk review depth before turning it into an enforced Skill.

**Release Candidate pending owner approval.** No release authority, automated merge, repository policy or AI skill created.
