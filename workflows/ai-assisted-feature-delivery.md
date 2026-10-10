# AI-assisted feature delivery

- Status: Draft — pending review
- Created: 2026-10-09
- Scope: personal engineering workflow (generic example; no company-confidential material)

## Goal
Build features efficiently with AI assistance while retaining developer ownership and correctness.

## Track A design decisions — grill-me round 1 (agreed 2026-10-10)

**Decision status:** Confirmed by project owner for **further design**; this document and the Implementation/Verification v1.0 release candidates remain **Draft / not Accepted**. These AI-specific preferences do not retroactively change the already Accepted, adaptable Feature Delivery Lifecycle.

| Decision | Chosen option | Consequence for design |
| --- | --- | --- |
| Q1 — Skill selection | **C: Hybrid routing** | AI recommends/selects task-appropriate procedures/skills from goal, done check, proof and risk; the engineer can override and must approve consequential choices at applicable gates. No obligatory skill chain. |
| Q2 — Coding permissions | **B: Scoped autonomy** | Within an explicitly approved work item/scope, AI may make focused code edits and run authorized, non-destructive checks without seeking permission for every line. New scope, architecture changes, privileged/destructive actions, external publishing and access expansion require appropriate approval. Git rights are specified in Q8; command allowlists and repository-specific enforcement still require local policy. |
| Q3 — Human checkpoints | **A: Fixed gates** | For this AI-assisted flow, a human explicitly approves **Solution Design → Delivery Planning → Implementation → Verification** at each corresponding checkpoint before proceeding to the next responsibility. These are **stage-boundary approvals**, not per-file or per-test approvals; Implementation ↔ Verification can exchange early *read-only or provisional* feedback, but advancing past a required approval is not implicit. Follow any stricter local/company policy. |

**Loop and gate interpretation (resolved in Q7):** preliminary review and developer checks may iterate before formal exit; the Implementation and Verification Gate approvals remain fixed. Rework within approved scope does not require reapproval for each edit; material change reopens affected earlier gate(s). Fixed gates do not authorize merge/deploy.

## Track A design decisions — grill-me round 2 (agreed 2026-10-10)

**Decision status:** Confirmed for further design; **not** approval of the AI-assisted workflow or either Implementation/Verification Release Candidate. These decisions refine the AI-assisted execution adapter while preserving round 1's Fixed Human Checkpoints.

| Decision | Chosen option | Consequence for design |
| --- | --- | --- |
| Q4 — Independent review | **B: Risk-based independent review** | Low-risk changes may use deliberate self-review when local policy permits; medium-risk changes use a separate review pass; high-risk changes require appropriate human peer/domain review under local policy. Independent AI review can supplement, never replace required human sign-off. **The fixed human Verification gate still applies for every work item.** |
| Q5 — Verification evidence | **B: Evidence by change type** | Verify changed observable behavior on the relevant CLI, API, UI, data or other execution surface, plus appropriate automated checks and risk-driven negative/integration scenarios. Record actual commands/results and **not run / blocked / inconclusive** gaps. No universal requirement for full E2E/video on every change. |
| Q6 — GitLab Draft MR timing | **A: After Implementation Gate; per-MR human confirmation required (clarified 2026-10-10)** | Human approves the Implementation Gate first. AI then checks the actual repository/work-item branching and merge strategy and **asks the human every time before creating each Draft MR**, explicitly confirming the source branch, target branch and relevant task/release context. Only after that specific human approval and the separately authorized publish rights (Q8: scoped publish authorization) may AI create that Draft MR. Never assume `master`, `main`, or `develop` as target; no prior/general approval substitutes for per-MR confirmation. The Draft MR is not merge or deployment approval. |

**Consistency note:** Q3 Fixed Gates and Q4 Risk-based Review answer different questions. The **human approval checkpoint is fixed**, while **verification/reviewer depth is risk-proportional**. Risk-based self-review does not silently waive the fixed human gate. Q6 does not imply unrestricted push rights; Q8 limits pushing to authorized scope and work branches.

### Mandatory Draft MR preflight — explicitly agreed refinement of Q6

1. **Investigate without guessing:** inspect repository contribution/branching rules, work-item/Jira context, current branch/upstream, destination release or integration branch, and cross-repository dependency/stacking if relevant. **Use evidence from the actual repo and task**; if rules conflict or are absent, surface the uncertainty.
2. **Present a concrete MR proposal:** repo, work item, source branch, **proposed target branch**, why that target matches this task's Git Flow, Draft status, and any ordering/dependent MR constraints. The human is the final authority on the target.
3. **Ask and wait for explicit confirmation for every single MR creation**, even when the target seems obvious, the Implementation Gate was approved, or previous MRs used the same branch. Human confirmation is MR-specific, not blanket permission.
4. **Create only the confirmed Draft MR** after the required Implementation Gate and publish permissions; link the task and report the MR URL. If human rejects or changes the branch, re-evaluate before creation. A target-branch change later also requires fresh human confirmation.
5. **No bypass:** Do not silently use `master` or the repo's default branch as an assumed target; do not create another MR as a workaround; do not auto-merge or deploy.

**Permission distinction:** The approval to open an MR is **separate from** local editing, commit and push rights. Those rights are defined by scoped autonomy in Q8, without waiving the per-MR confirmation. This explicit per-MR question is required even if Q8 later permits scoped Git autonomy. This policy is an agreed design decision for the pending AI-assisted flow, **not yet an Approved implementation/verification workflow**.

## Track A design decisions — grill-me round 3 (agreed 2026-10-10)

**Decision status:** Confirmed by project owner as design constraints; **not** approval of Implementation v1.0, Verification v1.0, repository automation settings, or release authority.

| Decision | Chosen option | Consequence for design |
| --- | --- | --- |
| Q7 — Rework and Fixed Gates | **B: Scoped Rework** | After the Implementation Gate, findings that can be fixed **inside the already agreed scope/design and risk envelope** may go through Implementation ↔ Verification without reapproving each edit. Rerun impacted checks, preserve finding/fix/retest evidence, and require **human approval at the Verification Gate**. Material changes to scope, accepted design, contracts, security/data risk or delivery plan **reopen the affected prior gate(s)**; do not silently extend the original authorization. |
| Q8 — Git Commit / Push | **B: Scoped Git Autonomy** | AI may make local commits on the agreed work branch within approved scope and repo conventions. AI may push to the specifically authorized non-protected work branch **only after explicit publish authorization at the Implementation Gate**; subsequent normal pushes for scoped rework on that branch may remain covered, subject to company policy. No unapproved force-push, history rewrite, protected-branch update, new remote destination or destructive Git operation. Push rights **do not** imply permission to open an MR. |
| Q9 — MR Completion | **A: Human Controlled** | Only an authorized **human** changes a Draft MR to Ready and performs Merge. AI can report CI/review status, suggest fixes and prepare a readiness summary. No automatic Ready transition, merge or deployment, even if CI is green or the Verification Gate is approved. Local branch-protection, approver and release policies remain authoritative. |

### Harmonized execution contract (rounds 1–3)

1. **AI selects task-appropriate skills** within the approved goal and task; engineer overrides. No forced pstack/grill-me skill chain.
2. **Fixed Human Gates** apply to the AI-assisted flow's Design, Planning, Implementation and Verification exits; gates are approvals of agreed deliverables, **not** permission prompts for each code edit or test. Risk-based review changes depth, **not** whether a human gate exists.
3. **After Implementation approval**, a specifically authorized publish operation may push the approved work branch. **Before each Draft MR**, AI investigates the **actual Git Flow of the repo/work item**, proposes source + target + reason + Jira link and **waits for an explicit per-MR human confirmation**; do not assume `master`, `main` or `develop`. A separate permission for push does not bypass MR-specific confirmation.
4. **During Verification**, provisional review and scoped fixes can cycle; each fix requires proportional retesting and a human Verification Gate decision. Material departures reopen relevant upstream gates.
5. **After Verification**, AI provides evidence and readiness summary; **only a human** can mark MR Ready or Merge. Merge and production release remain separate decisions.

### Consistency review — matters for v1.0 review, not unresolved choices yet

- **No direct conflict** among Q1–Q9 after distinguishing: *fixed human gate* versus *risk-proportionate evidence*, *scoped push* versus *per-MR permission*, and *preliminary review* versus *formal Verification Gate*.
- **Gate granularity updated for Jira ownership:** Design/Planning approvals are at Feature/Story level; Implementation/Verification approvals are at Work Item level (Story, Sub-task, Bug or Task as assigned). Material change reopens only invalidated prior gate(s). Human approver identity and repository-specific evidence storage are resolved per actual team/work item, reusing Jira/MR.
- **Repository-specific, not framework-global decisions:** actual target branch/Git Flow, reviewer requirements, mandatory CI checks, commit convention, allowed push credentials and target branch protections. Discover from the repo/task when executing; **ask the human if absent or ambiguous**. Do not assume them in a public generic workflow.
- **Separate future work, not a blocker for these two v1.0 workflow reviews:** detailed Release & Operations workflow, exact GitLab connector/permission enforcement, and design/evaluation of `puen-stack` skills. Nothing here changes real GitLab settings or grants access.

## Track A final gate-granularity decision — agreed 2026-10-10

**Owner decision:** In the **AI-assisted Track A workflow**, **Solution Design and Delivery Planning receive explicit human approvals at the Feature level**, while **Implementation and Verification receive explicit human approvals for each agreed, independently verifiable Work Item**. This resolves the remaining gate-granularity question. It does **not** approve the Implementation or Verification v1.0 Release Candidates.

| Gate | Approval unit | What the human agrees to |
| --- | --- | --- |
| Solution Design | **Feature** | Overall technical direction, relevant boundaries/contracts, major risks and decision ownership |
| Delivery Planning | **Feature** | Whole-feature scope, critical dependencies/integration and rollout constraints, initial implementable work item and the **progressive-detail rule for later work items** |
| Implementation | **Each Work Item** | Its agreed scope, actual diff and developer checks, known gaps and readiness for authorized publishing and formal verification |
| Verification | **Each Work Item** | Its acceptance/evidence, contract and review findings, resolved/blocking issues and owned residual risks |

**Subsequent Work Items:** Before implementing a work item, refine its details against the **already approved Feature plan**, documenting its acceptance, impacted contracts, responsible owner and verification approach in the existing work item. This **does not require repeating the Feature Design/Planning gates** when it is a faithful elaboration within approved boundaries. If readiness, scope or ownership is ambiguous, ask the responsible human rather than claiming that old approval covers new requirements.

**Material change / reopen rule:** If new evidence alters approved feature scope, outcome, architecture, critical API/data contract, security/privacy/risk assumptions, dependencies or release constraints **materially**, stop the affected change and obtain renewed approval of **only the upstream Gate(s) whose decisions are invalidated**. Do not restart every gate merely because a scoped bug fix, test addition or planned work item progresses. Scoped findings after an Implementation Gate remain fixable under Q7, with targeted retest and the normal Work Item Verification Gate.

**Cross-repo / multiple-MR rule:** A Work Item may span more than one repository. Implementation/Verification evidence may be coordinated at the Work Item level with repo-specific links and integration outcomes, but **each separate GitLab Draft MR still requires its own explicit human confirmation after investigating that repository/work item's Git Flow**. Publication, Ready/Merge and release permissions remain separate.

**Recording rule:** Capture Feature approvals and individual Work Item gate decisions in the existing Jira/issue/MR links with owner, scope, date and important residual risk. No mandatory duplicate document, global approver identity or assumed branch convention.

## Track A terminology decision — Work Item (agreed 2026-10-10)

**Owner decision:** Use **Work Item**, not *Increment*, as the primary unit of AI-assisted **Implementation and Verification**. A Work Item is an **assigned, bounded unit of engineering work with an observable completion/verification contract**. It maps to an **existing Jira Story, Sub-task, Bug or Task**, depending on what the engineer actually owns; it is **not** a new Jira issue type, a mandatory new document, or necessarily a deployable end-to-end feature slice.

**Typical ownership:** An engineer may own just a Merchant Service (BE) sub-task, just an Admin BFF sub-task, or both; frontend (FE) can belong to another engineer. Default the AI's write/edit/test scope to the **assigned Work Item and authorized repository/repositories**, rather than automatically implementing FE or traversing all repositories. A single Work Item can involve multiple repositories **only when its agreed scope actually requires that**. Keep the overall Story's shared contracts, acceptance and integration dependencies visible.

**Execution and gates:**
- **Feature/Story level:** human approves the shared Solution Design and Delivery Planning direction (when applicable), including interface contracts and ownership.
- **Work Item level:** Verification-first defines source-backed behavior/expected outcomes and proof for that assigned work; Implementation writes code/developer tests (TDD where useful); Verification independently challenges actual results. Human **Implementation and Verification Gates** apply to each agreed Work Item under the Track A fixed-gate policy.
- **Story integration:** test the cross-repository/cross-owner acceptance and contracts when relevant, with responsibility assigned across the team. This is **not an automatic extra global gate** and does not imply one engineer must implement unassigned FE/BFF/BE work. If end-to-end checks need unavailable repos/services, record **Not run / Blocked / externally owned** and the integration owner; do not declare the full Story accepted solely because a backend Work Item passed.
- **Boundary of Work Item:** agree observable outcome, non-goals, risk, impacted repos/contracts, evidence and Jira reference. If a Jira sub-task is vague (e.g. "Implement BE"), derive local acceptance from the parent Story with the appropriate human/contract owner, not from AI guesses.

**Terminology scope:** Previously accepted [Delivery Planning v1.0](./delivery-planning.md) may still use the Agile term *increment* for an iterative delivery slice. **Do not retroactively edit Accepted workflows.** For the current **AI-assisted Implementation/Verification Release Candidates**, *Work Item* supersedes the previous term *Increment* as the unit of ownership and human gate approval.

**All earlier guardrails remain:** Hybrid skill choice; scoped edits/rework and scoped Git permissions; human approval before **every** Draft MR after inspecting actual per-repo Git Flow; humans alone mark MR Ready or Merge; no automatic deployment. This terminology decision **does not** mark either workflow Accepted or change real repository permissions.

## Track A — Verification-first and TDD-enabled loop (agreed direction, 2026-10-10)

**Owner design decision:** Verification is a distinct responsibility that may **lead before code exists**, not merely a post-implementation QA step. Within the approved Feature design and plan, begin each Work Item with **source-backed acceptance examples, observable expected outcomes, negative/boundary cases, test seams and an evidence strategy** proposed by Verification.

**Execution sequence (iterative, no new gate):** Verification defines test intent → Implementation delivers small vertical slices with developer-owned unit/regression tests, using **TDD Red → Green → Refactor where a reliable test seam makes it worthwhile** → Verification independently executes actual behavior proof, evaluates test quality and challenges the diff → Findings return to scoped implementation/retest. An executable pre-code acceptance/contract test is useful when feasible but not required; a broken harness is not a valid red test. Do not let AI derive expected outputs only from its generated implementation.

**Responsibilities remain distinct:** Verification owns *what must be proven* and *whether evidence is trustworthy*; Implementation owns *how code is written* and *developer tests*. A given person or AI may participate in both, but independent assessment must challenge author assumptions; multi-model agreement alone is not proof. Avoid forced TDD for low-signal/high-setup tasks.

**Previously agreed gates and GitLab rules are unchanged:** Feature-level human Design/Planning approvals; per-Work-Item human Implementation/Verification gates; Scoped Rework and Git permissions; investigate real repo Git Flow and ask for **each Draft MR**; human-only Ready/Merge. Verification-first preparation does not itself pass any gate. Do not create Skills, change AGENTS.md or mark candidate workflows Accepted by recording this direction.

## Track A — Unit-test-green before formal Independent Verification (agreed 2026-10-10)

**Owner design decision:** For a code-changing Work Item, **Implementation must first run and pass the applicable unit/regression tests** and required local developer checks (such as lint/build/typecheck per repo), with observable command/results/environment, **before formal Independent Verification begins**. Failed tests go back to Implementation. Tests not run or unavailable are **Not run / Blocked**, never assumed passing. When a Work Item truly has no suitable unit-test seam, record why and agree on risk-appropriate alternative developer evidence at the existing Human Implementation Gate.

**Preserve Verification-first:** Defining source-backed acceptance and test strategy ahead of coding, and optional provisional feedback during coding, **do not require passing unit tests**; they are distinct from the formal independent quality/evidence assessment. Implementation owns the TDD/developer-check loop. After unit-green **and the existing Human Implementation Gate**, Independent Verification assesses test quality, actual behavior/contracts, diff and risk. Green tests are **an entry condition, not a correctness guarantee**.

**Findings loop:** Return defects to Implementation for scoped fixes and rerun affected developer unit tests/checks before formal independent re-verification. Retain the agreed per-Work-Item Human Verification Gate, human-specific Draft MR confirmation, scoped Git permissions and human-only MR Ready/Merge. **No new gate or autonomous release permission** is created by this ordering.

## Track A — Independent Verification safety nets (agreed baseline for v1.0 design, 2026-10-10)

**Owner decision:** After passing applicable developer-owned unit/regression tests and required local checks (with actual results), and the existing Human Implementation Gate, formal **Independent Verification** evaluates these **four baseline safety nets** for the assigned **Work Item**:

1. **Requirement verification:** Compare actual implementation/behavior to source-backed Work Item acceptance and parent Story contracts; identify gaps, unexpected effects or changed requirements.
2. **Independent code review:** Independently challenge the diff on both **spec correctness and codebase quality**, including architecture, critical edge/security/data cases and whether developer-test assertions genuinely detect incorrect behavior.
3. **Risk-based behavioral/integration verification:** Execute appropriate checks against the changed observable API/DB/CLI/contract/integration boundary, with negative/failure scenarios proportional to risk. Record unavailable cross-repo integration as an explicitly owned gap, not a claimed pass.
4. **Evidence and risk assessment:** Present reproducible commands, environments, actual outcomes, links, findings and remaining risks, including **Pass / Fail / Not run / Blocked / Inconclusive**; propose an explicit disposition for the authorized human.

**Coverage is baseline; test intensity is risk-based.** Low risk gets focused acceptance, test-quality/diff review and trustworthy evidence (with appropriate behavior checks where useful). Medium risk adds stronger real-behavior/contract/integration checks. High risk warrants targeted security/concurrency/data/migration/failure checks and required accountable human reviewers. No universal Mutation Testing, fuzzing, load-testing or video mandate. A test suite passing does **not** alone prove correctness.

**Responsibility/approval:** Verification may recommend Verified / Fix and reverify / Blocked / Stop; **the human retains the Work Item Verification Gate** and authority over residual risk. This is agreement on **the four-part baseline for the pending Verification v1.0**, **not acceptance of the whole workflow**, automation rights, MR Ready/Merge authority or a new Story-level gate. Reuse Jira/MR evidence records. See [Verification RC](./verification.md) and [Thai RC](./verification.th.md).

## Proposed AI-assisted sequence — aligned with agreed decisions

1. **Intake and evidence** — clarify outcome, done checks, constraints and current behavior; AI routes relevant procedures.
2. **Feature-level Solution Design Gate** — human accepts the feature's major technical/contract decisions.
3. **Feature-level Delivery Planning Gate** — human agrees feature-wide scope/risks, a detailed initial work item and progressive refinement of later work items.
4. **Verification-first for each Work Item** — before writing code, define source-backed scenarios, expected results, a suitable test seam and proof strategy; this is preparation, not a new gate.
5. **Per-Work-Item Implementation** — AI works within the agreed work item, **passes applicable unit/regression tests and required developer checks with actual evidence before formal Independent Verification**, and may make local commits; **Human Implementation Gate for that work item** reviews changed scope/diff/evidence.
6. **Scoped publish + Draft MR** — with specific work-branch publish authorization, AI can push; **inspect actual Git Flow, propose source/target and ask human before creating each Draft MR**. No automatic branch assumptions.
7. **Per-Work-Item Verification ↔ scoped rework** — actual evidence, independent review proportionate to risk, CI/MR feedback, fixes and targeted retests; **Human Verification Gate for that work item** confirms outcome. Only material changes reopen invalidated upstream gate(s).
8. **Human MR completion** — human alone marks Draft as Ready and performs Merge. Release/operations approval is separate.

This sequence is the proposed **AI-assisted adapter**, not a new mandatory waterfall for all engineering work. Tests and provisional reviewer feedback may begin during implementation, and revisions loop back as needed.

## Multi-repository consideration
When a feature spans multiple repositories, maintain one shared feature design but identify per-repository changes, version/contract dependencies, and test responsibilities. Do not assume a tool can automatically see repositories outside its workspace.

## Agent instructions
Prefer a short project-level `AGENTS.md` describing local conventions and entry points. Keep procedural skills outside the file, loaded on demand from `puen-stack`.

## Execution-specific inputs and deferred decisions

The **Q1–Q9 design choices are now recorded**. Before piloting in an actual team or repository, determine from the team and work item:

- **Human gate owner and approval record** for the feature/work item, including how formal approvals are recorded in existing Jira/MR. Default policy is not assumed from a person's job title.
- **Work-item Git Flow**, actual source/target branch and MR dependencies, required CI checks and peer-review rules. These are discovered per repository/task, and MR target requires per-MR human confirmation.
- **Publish authority/credentials**, permitted work branch and execution environment; document the scope rather than assuming a generic bot may push.
- **Team-specific commit convention and MR content**, from repository rules, without imposing a global template.

Deferred and **not required to approve the two workflow documents**: `puen-stack` skill authoring/evaluation, GitLab automation integrations and Release & Operations workflow. Approval of workflow documents does **not** grant real repository permissions.

**Status:** Draft / design decisions agreed for further review; Implementation and Verification v1.0 remain awaiting explicit owner approval.
