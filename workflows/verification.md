# Verification Workflow — v1.0 Release Candidate

- **Status:** Review Ready / Release Candidate — awaiting owner approval; not Accepted
- **Decision:** Separate Implementation and Verification with one shared execution loop (2026-10-10)
- **Date:** 2026-10-10
- **Track:** A — Build My Engineer
- **Related:** [Implementation](./implementation.md), [Delivery Planning v1.0](./delivery-planning.md)
- **Foundation:** [Feature Delivery Lifecycle v1.0](./feature-delivery-lifecycle.md)
- **Thai companion:** [Verification (Thai)](./verification.th.md)
- **Next:** Release & Operations (to be designed)

## Purpose, timing, separation

Establish **credible evidence that the change meets acceptance, respects important system boundaries and has been critically reviewed**. Verification is a distinct workflow with its own responsibilities and exit decision. It may begin **during implementation** and repeatedly send findings back; it is not a final waterfall phase.

Implementation owns making the change and developer checks. Verification owns evaluating acceptance, failures, cross-component behavior and review findings. Roles can overlap for solo work, but distinguish an independent assessment from an implementer's self-check. AI review alone is not equivalent to independent human judgment.

## Shared execution loop — normative coordination contract (v1.0 candidate)

The two workflows remain **separate responsibilities**, but use **one iterative execution loop** for each coherent increment:

```text
Delivery Planning (agreed increment)
   ↓
Implementation: orient → change → developer checks
   ⇄ Verification: acceptance / tests / contracts / independent review
   ↳ Findings → implementation fixes → targeted re-verification
   ↓
Verification decision → release review (a separate workflow)
```

**Start early:** Verification may begin with acceptance examples, test strategy, API/contract or partial diff; a "Ready for Verification" label is a convenience, **not** a mandatory wait-for-all-code gate. Developer tests remain part of Implementation; independent evaluation and evidence judgment are Verification's responsibility.

**One shared handoff in the existing issue/MR** (not a new obligatory artifact):
- **Identity & scope:** increment, acceptance IDs, repo/diff links and known exclusions.
- **Changes & risks:** touched boundaries, contract/data changes, migration and critical failure cases.
- **Evidence:** actual executed checks, outcomes, environment, explicitly *not run / blocked* checks.
- **Feedback:** findings with severity/evidence/owner; changed-file/test impact; retest outcomes.
- **Decision:** continue implementing, ready for further verification, fix & reverify, blocked or verified for release review; authorized human owns residual-risk acceptance.

**Loop rules:** Only the touched behavior and dependent contracts need proportional re-verification after a fix; revisit broader checks when the change has wider impact. Any newly discovered high-impact unknown returns to Discovery/Design/Planning. Neither workflow can authorize deployment, silently alter acceptance or override required human review.

**Roles:** Solo engineers may perform both with deliberate separate passes; medium/high-risk changes should seek a genuinely independent reviewer when feasible or required by local policy. AI models can suggest code/findings, but agreement among agents is not evidence that checks executed or humans approved.

**Scaling:** A trivial reversible change may record the whole loop in a short MR note; a multi-repo or security/data-critical feature needs explicit contract, test, review and release-risk evidence. Avoid duplicate tickets/docs.

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
| 1. Trace acceptance | Map acceptance and exclusions to changed behavior and tests | Scenario → check → observed outcome |
| 2. Validate tests | Inspect test assertions and run suitable checks in an authorized environment | Actual command, environment, timestamp/context, pass/fail/not run |
| 3. Probe critical failures | Check permission, malformed input, concurrency, retry/idempotency, error/timeout, boundary behavior as relevant | Negative-case evidence and residual risk |
| 4. Validate integration | Verify contracts/API/schema, producer-consumer compatibility, migration and version skew | Integration/contract tests or owned gap |
| 5. Review independently | Review diff for correctness, maintainability, unintended change, safety and test gaps | Finding severity, file/evidence, impact, recommendation |
| 6. Reconcile findings | Send findings to Implementation; retest affected behavior after fixes | Findings resolved, accepted with owner or blocked |
| 7. Decide handoff | Determine verification state, residual risks and release-readiness handoff | Explicit decision and responsible human |

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

No requirement to finish all implementation tasks before starting verification; each coherent increment can pass through several loops.

## Approval checklist — v1.0 Release Candidate

1. Are the two workflow responsibilities clear while enabling review during implementation?
2. Can the shared handoff fit existing Jira/MR without separate mandatory artifacts?
3. Are evidence, independence, risk acceptance and release authority distinguished?
4. Are AI permissions left to separate explicit approval?

## Pilot and open decisions

Pilot tiny reversible change, medium integrated feature and a simulated multi-repo contract change. Observe defect detection, rework, integration surprises, false-positive findings, time-to-feedback and paperwork burden.

**Review:** What qualifies as independent review for solo work? Which medium/high-risk situations need explicit human sign-off? What exact evidence fits existing GitLab/Jira fields? When should AI-assisted diff review be used?

**Release Candidate pending owner approval.** No release authority, automated merge, repository policy or AI skill created.
