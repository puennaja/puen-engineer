# My Engineer pilot playbook — idea → working change → observable outcome (EN)

**Canonical Playbook:** This is the pilot workflow source of truth; tooling setup lives in [puen-stack](https://github.com/puennaja/puen-stack). See [model routing](../guides/codex-model-routing.th.md). Record real pilot evidence in product repo `docs/pilot/` throughout execution.


**Status:** v0.1 pilot guide, inspired by the explicit playbook routing of [pstack](https://github.com/cursor/plugins/tree/main/pstack), but **not** a copy of its Cursor-specific automation. My Engineer's [Accepted workflows](https://github.com/puennaja/puen-engineer/tree/master/workflows), human gates and local repository/company rules win when instructions conflict.

[ภาษาไทย](./idea-to-outcome.th.md) · [Plane Cloud read/write setup](https://github.com/puennaja/puen-stack/blob/master/setup/plane-cloud.md) · [Skill index](https://github.com/puennaja/puen-stack/blob/master/skills/README.md) · [Thai artifact templates](https://github.com/puennaja/puen-engineer/blob/master/artifacts/workflows/README.th.md)

## What this pilot must prove

Given only a product idea **or** a feature requirement, can Codex/Claude **get the problem right**, discover constraints, propose a defensible solution, implement the smallest approved slice, verify behavior rather than just compile code, document human decisions, produce a reviewable Draft MR, and faithfully report actual release/outcome status?

It must also be obvious **what happened and why** from the existing tracker, Git and MR records when the owner resumes tomorrow, without reading a long AI transcript or adding duplicate templates.

**Initial pilot:** choose a small, reversible feature in a **private, disposable one-repo greenfield application** with synthetic data and local/staging verification. A brownfield, three-repo feature is a **second pilot** after the one-repo workflow succeeds. The selected tracker is source of truth for Work Items and progress; GitHub (current pilot) or GitLab (optional) owns Git/PR-or-MR records. A successful local/staging pilot does **not** prove production readiness or business impact.

## Roles and authority

- **Human product/engineering owner** (may be the solo pilot operator): agrees the problem/outcome, reviews Design and Planning, approves each Work Item Implementation and Verification Gate, independently authorizes branch publication and each Draft MR, and controls MR Ready/Merge and each production release.
- **AI coordinator:** `work-item-runner`, tracking and routing only, with **bounded selected-tracker CRUD** in the specified pilot project if granted; AI comments must say they are not approvals.
- **AI specialists:** `codebase-explore` (read-only), `verify-work-item` (pre-code expectations, developer checks or eligible formal checks), `review-change` (read-only independent judgment), `git-commit` (local scoped commit) and `gitlab-draft-mr` for GitLab only, or a verified GitHub CLI/connector for Draft PR (only after human confirms **each** Draft PR/MR).
- **Actual security boundary:** Plane OAuth/project role, agent workspace/sandbox, GitHub/GitLab repo/branch protections and company policy. A Skill never elevates privileges.

**Accepted ordering update (2026-10-11) — Verify before publish by default:** After passing developer checks and the Human Implementation Gate, run all locally feasible **Formal Independent Verification** (separate review pass, test quality and authorized behavior checks) before publishing. The **Human Verification Gate** decides whether this evidence is sufficient for the next step. Only then seek separately scoped Push authorization and per-PR/MR human confirmation. **Exception:** when required proof depends on remote CI, a preview environment or other unavailable local integration, the human may explicitly authorize an earlier narrowly scoped branch Push (and Draft PR/MR only if necessary and separately approved). Pending remote-dependent checks stay `Not run/Blocked`; do not claim the Verification Gate passed until necessary post-publish evidence is verified, and revisit the Human Verification Gate if new evidence changes the decision. Remote CI and PR/MR review findings trigger scoped fixes, retests, independent re-verification and a renewed gate decision where applicable. Never mark Ready, Merge or Deploy automatically.

## Step 0 — Selectable bootstrap and product readiness

**Source of truth:** Engineering workflow and human gates live in `puen-engineer`; tooling and skills live in [puen-stack](https://github.com/puennaja/puen-stack). The workflow must not depend on one agent, tracker or Git host.

1. Clone/update `puen-stack`, run `npm run setup`, and choose an agent (Codex/Claude/Both), work tracker (Plane/Jira/GitHub Issues/None), Git host (GitHub/GitLab/None) and skill scope (Global/Project). The bootstrap is **not yet end-to-end verified**, and selecting a provider does not imply a working adapter.
2. The wizard inspects CLI tools, creates only missing model profile files and optionally offers official interactive authentication for selected integrations. Never overwrite user configuration, grant roles or write permissions, push, merge or deploy automatically. Reuse already installed global skills where valid.
3. Open the real product repository (Pilot #001: `fall-in-love-project`); verify Git root, remote host, existing `AGENTS.md`, branches and changes. Git is the version control system; GitHub uses Draft PR and GitLab uses Draft MR.
4. Invoke `pilot-recorder` **before** `setup-puen-stack` to establish/reuse evidence under `docs/pilot/`. Record observed events, problems, attempted solutions, meaningful test results and decisions. Then run repository readiness covering owners, Git flow, required checks, skills, permissions and selected tracker read/write.
5. Read the selected tracker first; obtain a specific human authorization before bounded create/update/comment, and verify writes by reading them back. Check Git hosting authentication and branch rules before proposing publication.
6. Start discovery when necessary readiness is confirmed, or clearly identify blockers/owner/next action. Do not claim untested integrations or deployment success.

```bash
git clone https://github.com/puennaja/puen-stack.git
cd puen-stack
npm run setup
# Then open the existing product repository
```

**Capability-based adapters:** Core skills request work-item list/read/create/update/comment and draft-change-request capabilities; selected provider integrations implement those details. If unavailable, report `Blocked/Not Verified`, not fabricated calls or a silent provider switch. Capture important milestones in `docs/pilot/`; don't log every tool call.

## Copy/paste kickoff prompt (new idea)

```text
Use work-item-runner to start the My Engineer v0.1 pilot from this IDEA:
<describe the product, user problem or feature>

Tracker: Plane Cloud. Ask me for the specific authorized workspace/project.
I authorize you to propose the parent Work Item first. Once I confirm that
project and creation, you may create/update/comment on the approved parent
and later approved child Work Items for routine evidence/progress tracking.
Do not delete anything, change Plane project settings, alter GitLab permissions,
or treat a Plane status/comment you wrote as a Human Gate decision.

Follow the Accepted My Engineer workflows and smallest useful artifacts.
Separate Confirmed/Assumed/Unknown, ask one critical question at a time,
stop at real Human Gates, and tell me what was actually written to Plane
with real links. No production deployment unless separately authorized.
```

## Workflow routing: step by step

| Step | Ask the agent / use Skill | Actual work and observable evidence | Plane write (when authorized) | Stop/decision to move on |
| --- | --- | --- | --- | --- |
| **1. Intake** | `work-item-runner` | Problem hypothesis, user, desired outcome, success signal, owner; distinguish solution suggestion from need | Search first, then create **one parent** Idea/Feature item; read back URL/ID | Human confirms **problem worth exploring** |
| **2. Requirement Discovery** | `work-item-runner` + [Discovery artifact](https://github.com/puennaja/puen-engineer/blob/master/artifacts/workflows/requirement-discovery.md) | Clarifying questions, user scenarios, non-goals, confirmed vs assumed, candidate acceptance | Edit parent summary or one labelled milestone comment; no ticket per phase | Human routes: investigate, prototype, ask more, defer/decline |
| **3. Technical Discovery** | `codebase-explore` | Greenfield feasibility **or** source-lined brownfield trace, contracts, repo boundaries, risks/unknowns, checks actually run | Evidence-linked Technical Discovery summary; Unknown for unopened repos | Human chooses sufficient for Design / bounded spike / Blocked |
| **4. Solution Design** | `work-item-runner` (+ `codebase-explore` if needed) | At least the meaningful options, trade-offs, selected interface/data invariants, test/release risks | Design proposal + link to decision; concise Artifact not repeated | **Feature-level Human Solution Design Gate** |
| **5. Delivery Planning** | `work-item-runner` | **Whole-feature map + progressively detailed next Work Item**, cross-repo contract and integration owner | Plan parent; after Human Planning Gate, create only approved child Work Items | **Feature-level Human Delivery Planning Gate** |
| **6. Pre-code acceptance** | `verify-work-item` (preparation mode) | Acceptance oracle, negative cases and safe test seams from approved requirement, not generated code | Child acceptance/test-intent and owner | Ambiguity resolved by human product/tech owner |
| **7. Implementation** | Assigned coding agent under `work-item-runner` | Scoped files, diff, no destruction of pre-existing changes, applicable unit tests, real lint/build results | Child evidence summary, real `Pass/Fail/Not run/Blocked` links | **Human Implementation Gate per Work Item** after applicable checks |
| **8. Formal independent Verification** | `review-change` + `verify-work-item` | **Only after developer-green & Implementation Gate**; fixed diff, test quality, real behavior/contract proof where safely authorized, residual risk | Independent findings, evidence, fix/retest loops | **Human Verification Gate per Work Item** |
| **9. Local commit** | `git-commit` | Confirm intended staged hunks, tests/hooks, scoped local commit, real SHA. Tracker ID from **Plane or Jira** | Link child to branch/commit, no fake success | Scoped local commit authorization; **not Push** |
| **10. Push + Draft PR/MR** | GitHub: `git-draft-pr` with verified GitHub CLI/integration; GitLab: `gitlab-draft-mr` | Check `git remote -v`, correct remote, base/head, branch permissions; authorize Push separately, then GitHub Draft PR or GitLab Draft MR | Real PR/MR URL, CI state, dependency | **Human confirms each Draft PR/MR**; **human only** Ready/Merge |
| **11. Release readiness** | `work-item-runner` + [Release artifact](https://github.com/puennaja/puen-engineer/blob/master/artifacts/workflows/release-operations.md) | Pinned build, included Work Items, environment, smoke/safety checks, observability, mitigation/recovery owner | Release-bound evidence and **actual** result; parent/child trace | **Human Go/No-Go for each real Production Release**; fresh human confirmation **for every anomaly intervention** |
| **12. Observe and learn** | `work-item-runner` | Technical deploy, service health, feature behavior and **business/user outcome** recorded as separate predicates; follow-ups, measurable pilot score | Final outcome note: Healthy/Degraded/Inconclusive/Not deployed; owner/date | Human confirms outcome or another iteration; never invent a metric |

**Iteration is expected.** A critical new unknown routes back to the affected Discovery/Design/Planning decision. Do not force a strict waterfall or repeat approved work unnecessarily. For a small low-risk change, combine notes inside existing Plane issues/MRs. A child can have separate tests and gate decisions; a parent is not complete until its accepted feature-level outcome and integrations are proven.

## Tiny example (fictional, not created for the user)

Idea: "I want a small tool to log daily expenses and see how much remains this week." Discovery may show that **the meaningful outcome** is answering "Can I afford a purchase?" rather than shipping a dashboard with arbitrary graphs.

- **Parent Plane Work Item (proposed):** `PILOT-<real-id>` — "Help a user understand weekly spendable amount"; Unknown until Plane actually creates it.
- **Design option A:** browser form + local storage and small calculations; option B: database + sync. Human decides from constraints and evidence.
- **First Work Item after Planning Gate:** add one expense and update the remaining amount; acceptance includes invalid input and negative balance; later import/export are explicitly out of scope.
- **Evidence:** real unit tests, approved local browser/API drive, captured expected/actual values and a Draft MR link if created.
- **Outcome:** "Local workflow demonstrated" if tests pass locally; **not** "production healthy" or "users adopted" without independent real evidence.

## How to observe what happened without reading chat

Use **Plane parent Work Item as the index** and its Activity/AI-labelled comments for a chronological summary. A good checkpoint note has:

```text
[My Engineer | AI progress, NOT HUMAN APPROVAL]
Stage / feature or child Work Item ID:
Observed evidence: real file/commit/test/MR/Plane links + timestamps
Decision requested: human role, exact gate, Pending or original human-authored link
Not run / inconclusive / unresolved: ...
Next: action, responsible owner, checkpoint
```

A human approval should live in an original human-authored Plane comment, review record or other reliable approved source. If recorded as spoken/chat confirmation by the operator, label its origin; **do not let the agent forge a human comment using OAuth as the signed-in user**. When a stage repeats, read back the current comment history and update only authorized fields to avoid duplicates.

At any moment ask:

```text
Use work-item-runner in RESUME / STATUS mode on <real Plane item URL>.
Read actual parent, children and latest comments, linked commits, MRs and
available test evidence. Show stages Passed / Pending human / Blocked /
Not executed, which agent wrote which entries, the next safest action,
and what still prevents claiming a working product. Do not write.
```

## Pilot scorecard and exit decision

Capture a **baseline where observable** (manual process duration/defect rate, or Unknown) before the run. Do not invent measurements just to show an improvement.

| Question to answer | How to demonstrate | Truthful result if missing |
| --- | --- | --- |
| Requirements interpreted correctly? | Human-approved problem, source-backed acceptance, review against actual behavior | Unknown / needs discovery |
| Every crucial decision traceable? | Original human gate evidence, not an agent-authored approval | Blocked |
| Verified behavior beyond unit green? | Safely run actual API/UI/CLI flow and documented negative case | Not run / Inconclusive |
| Changes reviewable and bounded? | Exact diff, tests, clean scoped commit and Draft MR | Pending / Blocked |
| Tracker writes observable and non-duplicated? | Plane Activity, readbacks and real links | Failed / Inconclusive |
| Actual delivery outcome? | Real environment/version, service health, changed behavior and owner | Local-only / Staging-only / Not deployed |
| Product/user outcome? | Predetermined signal observed at legitimate checkpoint | Not measured; owner + next date |
| Was work faster/easier? | Actual elapsed time, human correction count, token/runtime cost if available | Baseline unavailable / inconclusive |

**Classification:** `Pilot demonstrated (bounded scope)`, `Partial`, `Blocked` or `Inconclusive`. This is an honest pilot conclusion, **not** authorization to mark a Production Release healthy. Summarize the next small workflow/skill improvement for human review, without silently altering Accepted workflows.

## When to stop rather than simulate a pass

No approved tracker project, missing OAuth/write permission, ambiguous owner, critical source evidence missing, rejected Gate, failed required test, prohibited side-effecting verification, inaccessible contract, no MR rights, unapproved release, no safe monitoring: record **Blocker + Owner + Smallest Next Action**. Continue other authorized read-only work if helpful. Never replace a missing evidence source with an AI guess.

**Limitations:** This pilot runbook and associated Skills have not been executed end-to-end against real Plane + Codex + GitHub/GitLab; tests must be performed and evidence added to the actual pilot Work Item.
