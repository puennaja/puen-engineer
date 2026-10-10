# Implementation Workflow — v1.0 Release Candidate

- **Status:** Review Ready / Release Candidate — not Accepted
- **Decision (2026-10-10):** separate Implementation and Verification workflows; one shared iterative execution loop.
- **Date:** 2026-10-10
- **Track:** A — Build My Engineer
- **Upstream:** [Delivery Planning v1.0](./delivery-planning.md), [Solution Design v1.0](./solution-design.md)
- **Companion:** [Verification Workflow](./verification.md)
- **Thai:** [Implementation (Thai)](./implementation.th.md)
- **Scope:** Solo to cross-team; tool-neutral and AI-optional

## Purpose and boundary

Turn one agreed, sufficiently understood increment into **reviewable code** with its developer-owned checks and clear handoff to independent verification. Implementation and Verification are distinct **responsibilities**, not sequential waterfall phases. They run in short feedback loops; tests and reviews can begin before code is complete.

**Inputs:** acceptance scenarios, current slice from the Delivery Plan, approved design decisions, verified repo boundaries, key contracts, risks, local coding conventions and access approvals.

**Stop and revisit discovery/design/planning** when unverified assumptions could cause unsafe authorization, data loss, incompatible contracts, or incorrect domain behavior. A bounded spike is valid implementation work if it has a question and evidence output.

## Shared execution loop

Implementation and [Verification](./verification.md) are distinct responsibilities working in a shared iterative loop, **not sequential waterfall gates**:

```text
Delivery Planning → Implementation (code + developer checks)
                     ↕
                 Verification (acceptance + independent review)
                     ↓ findings → implement fixes → reverify
                     ↓ sufficient evidence → release review (separate)
```

Verification may start on acceptance examples, contracts, test strategy or partial diffs; it need not wait for implementation to finish. Handoff information belongs in the **same issue/MR**, not mandatory duplicate documents:

- Scope and links: increment, acceptance, repository diffs.
- Change boundaries: contracts, schema, critical invariants and operational risks.
- Actual evidence: commands executed, environment, observed results, not-run checks.
- Feedback: findings, severity, evidence, owner, resolution and relevant retest.
- Human decision: continue, fix-and-reverify, blocked or ready for release review.

Scale the checks and independent review to risk. Solo work may use separate self-check and review passes; AI agreement alone is not independent human approval. After a fix, reverify affected behavior and dependencies, widening checks if impact expands. Neither workflow authorizes deployment.

## Activities (repeat as necessary)

| Activity | Action | Minimal evidence |
| --- | --- | --- |
| 1. Orient | Read relevant code, AGENTS.md where available, test conventions, relevant requirement/design, current Git state | Existing behavior, known constraints and evidence-backed affected paths |
| 2. Bound | State increment outcome, acceptance cases, non-goals and expected integration boundaries | Linked task/plan and short change intention |
| 3. Plan local edits | Select minimal changes consistent with existing architecture; note tests, migrations, error semantics | Clear next change and critical unknowns |
| 4. Implement | Make focused and reversible edits; keep unrelated refactors separate; maintain security and compatibility | Reviewable diff and rationale for non-obvious deviations |
| 5. Developer checks | Run suitable local tests, lints, typechecks and builds; write or update behavioral/regression tests | Actual commands/results, not AI-claimed outcomes |
| 6. Handoff or iterate | Fix local findings; communicate contracts, unrun checks, migrations and dependencies to Verification | Diff link, acceptance mapping, test evidence and known gaps |

**Do not wait until the end to test.** Developer checks are inside implementation for fast feedback, but **independent verification** is defined in a separate workflow. An implementer can request it during any iteration.

## Evidence-driven implementation — pstack + grill-me adaptation (candidate for v1.0)

The following **adapts ideas**, not mandatory plugin commands. See [pstack guide](https://github.com/cursor/plugins/blob/main/pstack/docs/guide/README.md), [Understand](https://github.com/cursor/plugins/blob/main/pstack/docs/guide/03-understand.md), [Design](https://github.com/cursor/plugins/blob/main/pstack/docs/guide/04-design.md), [Build](https://github.com/cursor/plugins/blob/main/pstack/docs/guide/05-build-and-clean.md) and [mattpocock grilling](https://github.com/mattpocock/skills/blob/main/skills/productivity/grilling/SKILL.md). **Skills are optional implementations of the workflow**, not the workflow itself.

### 1. Goal contract before AI writes code

State, or retrieve from the existing issue/plan: **goal**, observable **done check**, required **proof**, known facts/code references and **constraints** (including read-only or human-checkpoint boundaries). A natural-language task is enough when these are already linked; do not copy specs into every prompt. Ask the agent to restate ambiguous requests before editing.

### 2. Ground in evidence and challenge consequential unknowns

Start with read-only tracing of **how** the affected code actually behaves, then inspect **why** relevant ownership, contracts or patterns exist when changes threaten them. Distinguish confirmed source facts, inferred history, assumptions and unresolved questions. Do not use a plausible root-cause theory as a substitute for reproduction.

Inspired by `grill-me`: for **materially ambiguous / expensive-to-reverse decisions**, explore a dependency-ordered decision tree; ask the human only the questions that really require a product/engineering decision, with a recommended option and trade-off. Resolve observable codebase facts through investigation instead of interviewing the user. The upstream accepted Solution Design workflow still owns consequential design choices. An agent must **not** treat a grilling session as design approval or edit approved architecture without the authorized decision owner.

### 3. Select a task-shaped execution path

| Work type | Evidence-focused default | When to escalate |
| --- | --- | --- |
| Bug | Reproduce with the closest real surface, trace root cause, preserve failing scenario, implement smallest justified fix, rerun original repro | If behavior cannot be reproduced, report **inconclusive** and investigate rather than shipping guesses |
| Feature | Acceptance examples + data/contracts first; implement one small verifiable vertical slice at a time | If module boundaries or data shape are high-risk, return to Solution Design; consider multiple options/prototypes |
| Refactor | Capture current behavior; change structure within stated scope; show equivalent external behavior | Expose unexplained changes in outputs/contracts; separate feature work |
| Performance | Measure representative baseline and bottleneck before optimizing; compare equivalent executions | Question harness validity and confounding variables before claiming improvement |

Use a focused **change → developer check → inspect → adjust** loop, not a one-shot generation of the whole feature. The closest reliable feedback may be test-first, test-after, a real CLI/API/UI run, or a targeted experiment; TDD is a useful option, **not a universal mandate**.

### 4. Reviewability and handoff

Before a review pass, remove unrelated changes, dead compatibility scaffolding and unsupported defensive code where safe. **Do not adopt pstack's blanket comment-removal preference**: retain comments that explain non-obvious invariants, externally imposed constraints and genuinely useful API contracts. Keep human-reviewed decisions and proof in the existing Jira/MR record.

**Candidate future skill boundaries (not yet created):** a task router (goal/done/evidence → selected procedure), codebase-grounding, implementation-by-slice and a decision-interview helper. Do not copy `/poteto-mode`, `/architect` or `/grill-me` into a mandatory `AGENTS.md` chain. Validate any eventual skill on pilot tasks and compare effort, quality and token cost against the no-skill baseline.

## Risk-aware implementation rules

- Preserve current behavior outside agreed scope; identify compatibility and side effects.
- Minimize complexity rather than simply minimizing line count. Follow existing patterns unless design evidence warrants change.
- Choose test-first, test-after or iterative testing based on risk and feedback value; no universal TDD or coverage quota.
- Treat schema migrations, retries, idempotency, authorization, concurrency and observability as part of the affected change where relevant.
- Only claim a check ran if its results were actually observed. Mark missing tools/credentials as **Not run** or **Blocked**.
- Avoid secrets or company-confidential data in public repositories or unauthorized AI tools.
- Keep one issue/MR as the living record when sufficient; no mandatory duplicate Markdown plan.

## Cross-repository practice

Use one feature-level work item and scoped repository tasks. Record actual provider/consumer contracts from source evidence. An AI agent operating inside one repo **does not automatically have access to sibling repos**. Coordinate compatible changes and integration checkpoints with contract owners; do not assume a default deployment order.

## AI involvement (candidate guidance)

AI may choose appropriate task skills (hybrid routing), propose edits and execute focused, non-destructive developer checks inside the approved scope. **Scoped autonomy** allows local edits and commits on the agreed work branch, without per-file approval; additional architecture/scope changes and privileged/destructive operations require fresh human authorization. A **Fixed Human Implementation Gate** still approves the completed increment before authorized remote publishing. Push and MR creation have different permissions. No mandatory Claude/Codex split is approved. Keep AGENTS.md short; create reusable skills only after pilots.

## Approval granularity — feature decisions, increment execution (agreed for Track A)

The human approves **Solution Design and Delivery Planning once at Feature level** for the agreed direction, major contracts/dependencies, risk boundaries and full-scope progressive plan. **Implementation and Verification each receive separate human gates per agreed Increment**; a single Feature may therefore have several Implement → Verify loops and decisions.

Before implementing the next increment, refine its acceptance, repo/contracts, dependencies and checks **within the previously approved Feature plan**. This is progressive elaboration, not an automatic new Feature Design/Planning approval. Do not presume an increment is authorized when it changes the accepted scope or introduces unresolved critical risk.

If evidence **materially changes** the accepted Feature design, scope, public/data contract, security/data risks, or delivery constraints, stop the affected direction and reopen **only the prior Design/Planning gate(s) that are invalidated**, with their appropriate human owner. Scoped fixes and tests under the approved increment continue through Q7 without reapproving every edit; retest them and obtain that increment's Verification Gate. Keep Feature-level and Increment-level approval records linked in the existing Jira/MR, including the authorized owner and scope.

Multi-repo work can form one verifiable Increment with linked per-repo changes; **each Draft MR still needs its own human confirmation** of actual source/target Git Flow. This is a Track A AI-assisted policy candidate, not a blanket change to the already Accepted general lifecycle. See [Q1–Q9 and final gate decision](./ai-assisted-feature-delivery.md).

## Fixed Implementation Gate, scoped commits and rework (AI-assisted candidate)

For each agreed **Increment**, the authorized human reviews the changed scope, diff, completed developer checks, known gaps and relevant integration dependencies and **explicitly approves the Implementation Gate**. Preliminary Verification feedback is allowed before this gate; formal Verification completion remains a separate human decision.

Within the approved scope, AI may make **local commits** following repository conventions. **Normal pushes** to the specifically authorized, non-protected work branch require explicit **publish authorization at the Implementation Gate**; that authorization can cover later pushes of scoped fixes to the same branch. It never covers new destinations, protected branches, force-push, history rewrites, destructive Git actions, or unrelated changes. Company permissions override this candidate guidance.

**Scoped Rework (Q7=B):** After implementation approval, findings may be fixed and proportionately retested within the same agreed scope/design/risk envelope without asking for Implementation approval on each edit. The human still approves the **Verification Gate** after reviewing the final evidence. Material changes to scope, accepted design, contracts, security/data risk or planned delivery require re-approval of the **affected earlier gate(s)**.

**MR Completion (Q9=A):** AI can report status and suggest readiness. Only an authorized human may mark Draft MR as Ready or Merge; never auto-merge or deploy. See [AI-assisted design decisions](./ai-assisted-feature-delivery.md) for Q1–Q9 and the per-MR confirmation policy.

## GitLab Draft MR — mandatory per-MR human confirmation (v1.0 candidate)

After the **Implementation Gate**, AI must first investigate **the actual repository and work-item Git Flow**, including documented branching/contribution rules, Jira/work item, source branch, proposed target branch, release/integration strategy and dependent MRs. Never assume that `master` (or the default branch) is the right target.

**Before creating every single Draft MR**, show the human the repository, source → proposed target branch, target rationale, work-item link and relevant ordering constraints; **ask for and wait for explicit MR-specific approval**. Previous implementation approval or scoped Git permissions do not waive this question. Changing the MR target afterward also requires renewed human confirmation.

**Commit/push** follow the scoped rules above; **each Draft MR still needs separate explicit human approval**, even after a push was authorized. Humans alone mark Ready and Merge; deployment requires its own authorization.

## Handoff to Verification

Provide:
1. Linked acceptance cases and changed boundaries/repositories.
2. Reviewable diff and contract/schema changes.
3. Tests run, exact outcomes and environments; tests not run.
4. Key failure/permission scenarios and expected behavior.
5. Remaining uncertainties and rollback/release concerns.

**Exit options:** Ready for Verification; Needs More Implementation; Blocked — Discovery/Design; Stop/Defer. **Ready for Verification is not release authorization.**

## Release-candidate review criteria

1. Feature-level Design/Planning approvals are distinct from per-Increment Implementation/Verification gates, with only affected prior gates reopened on material change.
2. Separate ownership without mandatory handoff for each edit.
3. Evidence and findings shared through one existing issue/MR.
4. Risk-proportional review and actual test evidence.
5. Real repo access and Skill implementation remain separately authorized.

## Pilot / open decisions

Try a tiny regression, a medium feature and a simulated cross-repo contract change. Measure first useful feedback, rework, developer-test honesty and documentation overhead.

**Review focus:** Confirm that human gate owners and accepted-increment boundaries can be identified in each actual team, that publish authorization is recorded once in the existing issue/MR, and that early feedback does not bypass fixed gates. Git Flow, target branch and mandatory CI checks remain repository/task-specific.

**Release Candidate — awaiting explicit approval.** No AGENTS.md policy, automation, skills or repository permissions are changed.
