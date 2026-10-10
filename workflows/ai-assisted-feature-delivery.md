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
| Q2 — Coding permissions | **B: Scoped autonomy** | Within an explicitly approved increment/scope, AI may make focused code edits and run authorized, non-destructive checks without seeking permission for every line. New scope, architecture changes, privileged/destructive actions, external publishing and access expansion require appropriate approval. Exact git/command permissions are still undecided. |
| Q3 — Human checkpoints | **A: Fixed gates** | For this AI-assisted flow, a human explicitly approves **Solution Design → Delivery Planning → Implementation → Verification** at each corresponding checkpoint before proceeding to the next responsibility. These are **stage-boundary approvals**, not per-file or per-test approvals; Implementation ↔ Verification can exchange early *read-only or provisional* feedback, but advancing past a required approval is not implicit. Follow any stricter local/company policy. |

**Important tension to resolve in later rounds:** the shared Implementation/Verification loop remains iterative, while the selected Fixed Gates require formal stage completion approval. Distinguish early review and developer checks from formal entry/exit decisions; decide how findings that force rework affect previously granted approvals. A fixed gate is not release authorization; merge/deploy permissions remain separate.

## Track A design decisions — grill-me round 2 (agreed 2026-10-10)

**Decision status:** Confirmed for further design; **not** approval of the AI-assisted workflow or either Implementation/Verification Release Candidate. These decisions refine the AI-assisted execution adapter while preserving round 1's Fixed Human Checkpoints.

| Decision | Chosen option | Consequence for design |
| --- | --- | --- |
| Q4 — Independent review | **B: Risk-based independent review** | Low-risk changes may use deliberate self-review when local policy permits; medium-risk changes use a separate review pass; high-risk changes require appropriate human peer/domain review under local policy. Independent AI review can supplement, never replace required human sign-off. **The fixed human Verification gate still applies for every increment.** |
| Q5 — Verification evidence | **B: Evidence by change type** | Verify changed observable behavior on the relevant CLI, API, UI, data or other execution surface, plus appropriate automated checks and risk-driven negative/integration scenarios. Record actual commands/results and **not run / blocked / inconclusive** gaps. No universal requirement for full E2E/video on every change. |
| Q6 — GitLab Draft MR timing | **A: After Implementation Gate** | Human reviews and approves completion of the Implementation increment first; then, **with appropriate publish authorization (exact permissions pending Q8)**, AI may commit/push/open a **Draft MR** linked to the work item against the agreed target branch. Formal Verification can then use MR/CI and reviewers; early provisional/read-only feedback during implementation remains possible. Draft MR is not merge or deploy authorization. |

**Consistency note:** Q3 Fixed Gates and Q4 Risk-based Review answer different questions. The **human approval checkpoint is fixed**, while **verification/reviewer depth is risk-proportional**. Risk-based self-review does not silently waive the fixed human gate. Q6 does not imply unrestricted push rights; this is the subject of round 3.

**Pending round 3:** Q7 rework and whether approval resets; Q8 local commit vs push/publish authority and re-push; Q9 Draft MR completion/readiness, required CI/reviewer checks and who may merge. Neither AI workflow has been Accepted.

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
