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
| Q2 — Coding permissions | **B: Scoped autonomy** | Within an explicitly approved increment/scope, AI may make focused code edits and run authorized, non-destructive checks without seeking permission for every line. New scope, architecture changes, privileged/destructive actions, external publishing and access expansion require appropriate approval. Git rights are specified in Q8; command allowlists and repository-specific enforcement still require local policy. |
| Q3 — Human checkpoints | **A: Fixed gates** | For this AI-assisted flow, a human explicitly approves **Solution Design → Delivery Planning → Implementation → Verification** at each corresponding checkpoint before proceeding to the next responsibility. These are **stage-boundary approvals**, not per-file or per-test approvals; Implementation ↔ Verification can exchange early *read-only or provisional* feedback, but advancing past a required approval is not implicit. Follow any stricter local/company policy. |

**Important tension to resolve in later rounds:** the shared Implementation/Verification loop remains iterative, while the selected Fixed Gates require formal stage completion approval. Distinguish early review and developer checks from formal entry/exit decisions; decide how findings that force rework affect previously granted approvals. A fixed gate is not release authorization; merge/deploy permissions remain separate.

## Track A design decisions — grill-me round 2 (agreed 2026-10-10)

**Decision status:** Confirmed for further design; **not** approval of the AI-assisted workflow or either Implementation/Verification Release Candidate. These decisions refine the AI-assisted execution adapter while preserving round 1's Fixed Human Checkpoints.

| Decision | Chosen option | Consequence for design |
| --- | --- | --- |
| Q4 — Independent review | **B: Risk-based independent review** | Low-risk changes may use deliberate self-review when local policy permits; medium-risk changes use a separate review pass; high-risk changes require appropriate human peer/domain review under local policy. Independent AI review can supplement, never replace required human sign-off. **The fixed human Verification gate still applies for every increment.** |
| Q5 — Verification evidence | **B: Evidence by change type** | Verify changed observable behavior on the relevant CLI, API, UI, data or other execution surface, plus appropriate automated checks and risk-driven negative/integration scenarios. Record actual commands/results and **not run / blocked / inconclusive** gaps. No universal requirement for full E2E/video on every change. |
| Q6 — GitLab Draft MR timing | **A: After Implementation Gate; per-MR human confirmation required (clarified 2026-10-10)** | Human approves the Implementation Gate first. AI then checks the actual repository/work-item branching and merge strategy and **asks the human every time before creating each Draft MR**, explicitly confirming the source branch, target branch and relevant task/release context. Only after that specific human approval and the separately authorized publish rights (Q8 still pending) may AI create that Draft MR. Never assume `master`, `main`, or `develop` as target; no prior/general approval substitutes for per-MR confirmation. The Draft MR is not merge or deployment approval. |

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
- **Clarify implementation boundaries in the v1.0 candidates:** who has authority to sign each gate under the actual team policy; whether an accepted feature contains multiple separately gated increments; what constitutes a material rework that reopens earlier approval; and where approval/evidence is recorded. Use the existing Jira/MR, not an additional mandatory document.
- **Repository-specific, not framework-global decisions:** actual target branch/Git Flow, reviewer requirements, mandatory CI checks, commit convention, allowed push credentials and target branch protections. Discover from the repo/task when executing; **ask the human if absent or ambiguous**. Do not assume them in a public generic workflow.
- **Separate future work, not a blocker for these two v1.0 workflow reviews:** detailed Release & Operations workflow, exact GitLab connector/permission enforcement, and design/evaluation of `puen-stack` skills. Nothing here changes real GitLab settings or grants access.

## Proposed stages
1. **Intake** — read requirement, identify ambiguity, establish acceptance criteria and use cases.
2. **Explore** — identify impacted repositories, architecture, dependencies, and existing patterns.
3. **Plan** — split tasks, compare feasible approaches, enumerate edge cases, create implementation/test plan.
4. **Implement** — apply focused changes and track assumptions.
5. **Verify** — unit/integration tests as applicable, independent checks, and relevant manual validation.
6. **Review** — separate diff review; developer independently validates findings and final behavior.
7. **Deliver** — agreed commit format, draft merge/pull request, and traceability to the work item.

## Multi-repository consideration
When a feature spans multiple repositories, maintain one shared feature design but identify per-repository changes, version/contract dependencies, and test responsibilities. Do not assume a tool can automatically see repositories outside its workspace.

## Agent instructions
Prefer a short project-level `AGENTS.md` describing local conventions and entry points. Keep procedural skills outside the file, loaded on demand from `puen-stack`.

## Open decisions
- Which actions should be automated versus require approval?
- Which skills should be implemented first?
- How will skills be installed or synchronized into individual workspaces?
- What commit conventions and PR/MR template should be used?
