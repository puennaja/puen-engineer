# Verification / Independent Review — Artifact Template (EN)

> **Per assigned Work Item, not automatic Story acceptance.** Can be a Jira/MR section; do not create a second mandatory report. Verification-first preparation and preliminary feedback may happen before coding, but **Formal Independent Verification starts only after actual passing applicable developer checks and the Human Implementation Gate** (including the documented no-unit-test-seam alternative, where explicitly authorized).
>
> Workflow: [Verification v1.0 (Accepted)](../../workflows/verification.md) · [Thai version](./verification.th.md)

- **Jira Work Item / Parent Story acceptance:** <source links>
- **Status:** Verification-first prep | Preliminary review | Formal review | Fix & reverify | Blocked | Verified for release review (human-gated)
- **Implementation Human Gate approval + actual developer checks:** <links, date, explicit outcomes>
- **Fixed review diff base → head / relevant repos:** <commit SHAs and scope; not an implementer summary>
- **Independent reviewer / risk level / policy:** <person or deliberate separate pass; human peer when required>

## 1. Source-backed acceptance and independent oracle

| Scenario / requirement | Confirmed expected behavior from requirement/contract | Scope (Work Item vs external Story) | Evidence needed |
| --- | --- | --- | --- |
| <case> | <expected result, link> | <owner> | <observable proof> |

- **Negative / boundary / permission cases:** <specific expected outcomes>
- **Non-goals / external integration owner:** <explicit exclusions>
- **Preliminary feedback (before formal entry):** <scenario/contract/partial diff suggestions, not a formal verification result>

## 2. Execution-safety preflight — before live / side-effecting checks

- **Authorized test target:** <specific environment, service, database, endpoint and access owner>
- **Permission:** <explicit authorization / reference; credentials alone do not grant permission>
- **Data/fixtures:** <synthetic/sanitized identities; safe read-only or isolated test resources>
- **Potential side effects:** <writes, events, messages, payments, downstream integrations; no uncontrolled prod testing>
- **Owned-resource cleanup / rollback boundary:** <only test-created resources; never delete unknown/pre-existing data>
- **Unsafe or unavailable checks:** <do not execute; mark Not run/Blocked and owner; continue authorized read-only review>

## 3. Four Independent Verification safety nets

| Safety net | Reviewer question and evidence | Pass / Fail / Not run / Blocked / Inconclusive |
| --- | --- | --- |
| **1. Requirement correctness** | <compare actual change to source acceptance, not AI-generated expectations> | <status> |
| **2. Independent diff + test quality** | <review fixed diff for spec & codebase quality, inspect assertions that should detect wrong behavior> | <status> |
| **3. Risk-scaled behavior / integration proof** | <actual API/DB/contract/CLI/UI behavior as owned; external services need named integration owner> | <status> |
| **4. Evidence / residual-risk judgment** | <reproducible outcomes, gaps, findings, traceability and decision owner> | <status> |

**Reviewer separation:** <primary requirement sources, fixed diff, evidence; implementer's claims challenged; separate skeptical pass and its limits; qualified human for high risk per policy>

## 4. Checks and evidence — executed results only

| Required / optional (risk reason) | Command / check | Authorized environment + fixture | Expected vs observed | Pass / Fail / Not run / Blocked / Inconclusive | Raw evidence |
| --- | --- | --- | --- | --- | --- |
| <required?> | <actual action> | <env> | <result> | <status> | <link> |

- **Required checks not executed / equivalent evidence:** <gap, reason, policy-authorized human and explicitly valid alternative, or Blocked>
- **Shared cross-repo contracts:** <affected producer/consumer/version, what was tested and who owns remaining integration>
- **Real-surface proof vs proxy:** <build/unit green is an entry condition, not actual behavior proof>
- **Executed evidence limitations:** <timestamps, missing logs, inaccessible env, inconclusive data>

## 5. Review findings and scoped rework

| Severity / blocking? | Axis: spec / codebase quality | Diff location / scenario / evidence | Impact | Fix / disposition | Owner |
| --- | --- | --- | --- | --- | --- |
| <finding or None observed> | <axis> | <path/line/proof> | <risk> | <action> | <owner> |

**Reverification after a fix:** <changed scope, affected developer tests actually rerun and independent checks rerun; update fixed diff base if necessary>

**Material new risk / requirement:** <reopen only invalidated upstream human gate(s) and record owner>

## 6. Human Verification Gate — per Work Item

- **Reviewer recommendation:** Verified for release review | Fix & reverify | Blocked | Stop/defer
- **Blocking checks/findings:** <None with evidence, or named items; unavailable mandatory proof is NOT Verified>
- **Remaining non-blocking risks / owner / next checkpoint:** <explicit>
- **Human Gate decision / authorized person / date / link:** <Approved / Rejected / Pending with actual evidence>
- **Handoff to release coordination:** <Work Item proof, external integration gaps, contract/schema/release concerns and MR links>

**Authority boundary:** Human Verification Gate acceptance does **not** establish full Story acceptance, authorize Draft MR Ready/Merge, production deploy, feature exposure or changes to repository policy. Test statuses are never invented.
