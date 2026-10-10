# Implementation & Verification Workflow — v0.1

- **Status:** Proposed / Draft — pending owner review; **not Accepted**
- **Date:** 2026-10-10
- **Track:** A — Build My Engineer v1.0
- **Foundation:** [Feature Delivery Lifecycle v1.0](./feature-delivery-lifecycle.md)
- **Upstream:** [Delivery Planning v1.0](./delivery-planning.md), [Solution Design v1.0](./solution-design.md)
- **Related (not approved policy):** [AI-Assisted Feature Delivery](./ai-assisted-feature-delivery.md)
- **Thai companion:** [Thai](./implementation-verification.th.md)
- **Next:** Release & Operations (to be designed)
- **Scope:** Solo, team and cross-repository increments; tool-neutral and AI-optional

## 1. Purpose, boundaries and guiding decisions

Transform an agreed, sufficiently understood increment into **working and independently evidenced software**, maintaining correctness and changeability while keeping human responsibility explicit.

This workflow covers implementation, developer verification, integration verification and independent review; it prepares evidence for a separate release decision. **Green tests do not automatically authorize release.** Nor does an AI implementation or review count as independently validated merely because a second model agreed.

Non-goals: prescribe TDD for every change; mandate line coverage, particular IDEs, models, languages, a second AI agent, fixed test ratios, CI vendors, or a company-wide commit convention. Avoid duplicating Jira issues with large markdown tracking files.

## 2. Inputs and stop/go contract

For the **next increment** (not every distant task), establish:
- Linked acceptance cases and meaningful exclusions; owner for product decisions.
- Solution direction and impacted code/service boundaries supported by exploration.
- Critical interfaces, invariants, authorization and data-migration implications.
- A verification strategy proportionate to risk: unit, component, integration, contract, end-to-end, manual, or other appropriate evidence.
- Branch/merge arrangements and existing repository conventions; expected reviewers.
- Explicit unknowns and consequences if assumptions fail.

**Stop or return to discovery/design/planning** if a critical security, data integrity, contract or ownership ambiguity prevents safe work. A bounded spike may be the increment instead.

## 3. Adaptive execution loop

| Activity | Action | Evidence / checkpoint |
| --- | --- | --- |
| **1. Orient** | Read relevant local instructions, changed-path neighbors, test conventions, linked requirements and design; check working tree first | Known baseline, current behavior and boundaries; no speculative file inventory |
| **2. Bound the increment** | Choose the smallest coherent outcome and observable acceptance/test examples; separate feature and cleanup changes when practical | Focused change intent, dependencies and verification plan |
| **3. Change intentionally** | Follow existing architecture unless evidence justifies altering it; keep diffs reviewable; handle failures, compatibility and migrations consciously | Code with concise rationale for non-obvious decisions |
| **4. Verify locally** | Run relevant format/lint/type/build/tests; add regression tests for behavior being changed; exercise failure/security paths by risk | Command, result, scope and known gaps; never report tests as run if not executed |
| **5. Integrate across boundaries** | Verify producer/consumer API contracts, data compatibility, flags, version skew and migration sequence where relevant | Relevant integration/contract tests or a clearly owned blocker |
| **6. Independently review** | Another engineer or a distinct review pass examines diff against requirement, contracts, design, tests, security and operational risks | Actionable findings, severity, resolution or accepted risk |
| **7. Reconcile and prepare handoff** | Re-run affected checks after fixes; align tickets/design if behavior changed; commit and open draft PR/MR when appropriate | Traceable work item → code → verification → review; unresolved issues explicit |

This loop is **iterative**, not a forced seven-step waterfall. Small reversible changes can use one concise pass; critical changes require deeper review and evidence.

## 4. Implementation guidelines: correctness before automation

- Prefer the smallest change consistent with acceptance and domain invariants; avoid unrelated cleanup that obscures review.
- Respect existing language/toolchain/style and production compatibility; exceptions require rationale.
- Tests should encode desired observable behavior, especially a reported regression, boundaries and relevant negative cases. Test design is chosen by risk and feedback value, not a blanket unit-test percentage.
- Isolate ambiguous or non-repeatable experiments from production behavior. Never quietly treat AI guesses about an unfamiliar codebase as verified facts.
- Keep secrets, credentials, regulated/customer data and internal company artifacts out of public repositories and unauthorized AI contexts.
- When uncertainty changes the design, revisit Solution Design or Technical Discovery rather than forcing the plan.
- Developer may choose test-first, test-after or parallel testing, provided significant changed behavior is adequately verified.

## 5. Verification evidence and review contract

A claim that an increment is **verified** should have a traceable combination of:
1. Acceptance examples checked and observed behavior, with links or concise result.
2. Executed checks with actual outcomes, environment and relevant scope; say **Not run** or **Blocked** when applicable.
3. Negative / edge / concurrency / permission / data integrity cases selected proportionately to risk.
4. Cross-repository and integration evidence where changed contracts cross boundaries.
5. Independent review, addressed findings and any accepted risks, with an accountable human owner.
6. Remaining release, observability, rollback or operational work flagged for the next workflow.

**Evidence levels:** *Proposed* (AI or developer suggestion) → *Executed* (real command or manual observation) → *Reviewed* (someone assesses results) → *Accepted* (authorized human accepts remaining risks). These are not substitutes for each other.

**Never conflate:** “unit tests passed” with “feature accepted”; “AI reviewed” with “human-approved”; “mergeable” with “safe to deploy.”

## 6. Review checklist (scale to risk)

Examine: requirement/acceptance fit; compatibility and data transitions; domain invariants; authorization/privacy; error handling, timeout/retry/idempotency; observability; test quality and false positives; design consistency/readability; migrations and rollback; non-obvious performance or concurrency regressions.

Record findings as: **Severity · evidence/file location · impact/scenario · recommendation · resolution**. Review should identify substantive risks, not demand personal style changes. If a reviewer has no findings, that is not proof of correctness.

## 7. Multiple repositories and workspaces

Maintain **one feature-level work item/design** and link per-repository implementation work. Declare which repo owns each contract and how compatible versions are deployed. Confirm that an agent opened on one repository **does not automatically inspect sibling repositories**.

For a cross-repo change, use a dependency matrix when useful:

| Boundary | Provider / consumer | Contract or migration issue | Verification | Release order |
| --- | --- | --- | --- | --- |
| Service ↔ BFF | Assign from verified repository evidence | API response/permission compatibility | Contract or integration test | Decide per actual backward compatibility |
| BFF ↔ Web | Assign from verified repository evidence | Mapping, error states, UI expectations | Component + integrated scenario | Decide per actual backward compatibility |

Do not predeclare which repository must be deployed first. Compatible additive APIs may permit flexible order; incompatible changes require an explicit migration strategy.

## 8. AI assistance — permission boundaries

**Allowed to propose (within authorized access):** targeted code changes, tests, change summaries, edge cases, design alternatives, static review and draft commit/MR descriptions.

**Must be confirmed by responsible engineer:** changed acceptance, ownership, architectural deviations, destructive operations, secrets handling, pushing/merging, privileged command execution and release decisions. Exact automation permissions are **not yet approved**; humans should inspect proposed commands and diffs.

**Recommended trial rather than mandate:** have an implementation agent perform a focused change, then an independent review pass with a separately constructed prompt/context. A different model may reveal blind spots but cannot guarantee independence. Human inspection and actual tests remain necessary.

**AGENTS.md:** Keep concise repository-specific navigation, commands and hard constraints; link to canonical workflow instead of copying it. Skills in `puen-stack` should be extracted **only after repeatable pilot evidence**, not because the workflow is lengthy.

## 9. Core artifact: Verification & Review Record (lean)

Prefer a concise section in an existing issue or MR, not a standalone file unless risk or team coordination justifies it:

```markdown
## Increment and acceptance
Work item / target behavior / scope exclusions:

## Changes
Repositories and main contract changes (link diffs):

## Evidence
Check | executed? | result | environment | link
Acceptance examples / negative tests / integration evidence:

## Independent review
Reviewer or separate review pass / notable findings / decisions:

## Gaps and risks
Not-run checks, waived findings, remaining blockers, owner:

## Handoff
Ready for release review? Required migration/monitoring/rollback work:
```

## 10. Exit decision and adaptation

- **Verified and ready for release review:** acceptance evidence and appropriate tests/review exist, and residual risk is visible. The separate Release Workflow still decides deployment.
- **Needs fixes/re-verification:** defects, failed checks or findings remain.
- **Blocked — more discovery/design:** a critical unexpected fact invalidates assumptions.
- **Stop / defer:** unacceptable risk or insufficient value.

For a low-risk one-file edit, a short MR description plus relevant test output can satisfy this workflow. For a payment, security or data-migration change, require more extensive evidence and domain-owner review.

## 11. Proposed pilot and learning signals

Use **fictional or expressly authorized non-sensitive** work first: (a) tiny isolated regression; (b) medium feature with tests and peer review; (c) simulated cross-repo contract change. Check time-to-first-feedback, escaped regressions, review findings, integration surprises, test execution honesty, extra process overhead and AI token/tool cost. Do not score engineers by AI-generated lines, commits or ticket volume.

## Open owner review questions

1. Should Implementation and Verification remain one workflow with separate checkpoints, or be split into two files?
2. Should an independent AI review pass be recommended by default only for medium/high-risk work, or for every code change?
3. Which minimal evidence is practical to store in GitLab MR/Jira without duplicated documentation?
4. Which write/run/commit/MR actions may AI agents perform with consent, and which always need explicit approval?
5. What first **real pilot** will validate this workflow before extraction to `puen-stack`?

**Draft only. No changes to AGENTS.md, repo permissions, company process, or skills are approved by this document.**
