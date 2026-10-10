# Implementation Workflow — v1.0 Release Candidate

- **Status:** Review Ready / Release Candidate — not Accepted
- **Decision (2026-10-10):** separate Implementation and Verification workflows; one shared iterative execution loop.
- **Date:** 2026-10-10
- **Track:** A — Build My Engineer
- **Upstream:** [Delivery Planning v1.0](./delivery-planning.md), [Solution Design v1.0](./solution-design.md)
- **Companion:** [Verification Workflow](./verification.md)
- **Thai:** [Implementation (Thai)](./implementation.th.md)
- **Scope:** Solo to cross-team; tool-neutral and AI-optional

## Unit of execution: assigned Work Item (Track A decision)

**Work Item** is the Jira **Story, Sub-task, Bug or Task** assigned and approved for AI assistance. It is not a new Jira issue type and need not cover BE, BFF and FE together. A backend engineer may own **BE only**, **BFF only**, or BE+BFF under an explicitly agreed scope; FE and other teams remain separate owners. Use the parent Story for shared acceptance, architecture and contracts. Define **work-item-specific observable outcomes, non-goals, tests and proof** before editing.

**Default:** AI edits and checks only the repo(s) authorized for the assigned Work Item. Read other repo contracts only with appropriate access; cross-repo implementation is opt-in, not the default. Coordinate Story-level integration/contract checks with the relevant team owners; **passing one backend Work Item does not establish full Story acceptance**. Record missing external integration evidence and its owner, rather than silently taking on FE work.

**Gates:** Feature/Story Design and Planning approval set the broader boundaries; separate Implementation and Verification Human Gates apply **per agreed Work Item**, even when it is a standalone Jira Story. Use existing Jira/MR links and do not introduce an extra universal Story-integration gate. Accepted Delivery Planning may still use *increment* as the Agile delivery-slice term; it is not the unit used for AI execution here. See [Track A terminology decision](./ai-assisted-feature-delivery.md).

## Purpose and boundary

Turn one agreed, sufficiently understood work item into **reviewable code** with its developer-owned checks and clear handoff to independent verification. Implementation and Verification are distinct **responsibilities**, not sequential waterfall phases. They run in short feedback loops; tests and reviews can begin before code is complete.

**Inputs:** acceptance scenarios, current slice from the Delivery Plan, approved design decisions, verified repo boundaries, key contracts, risks, local coding conventions and access approvals.

**Stop and revisit discovery/design/planning** when unverified assumptions could cause unsafe authorization, data loss, incompatible contracts, or incorrect domain behavior. A bounded spike is valid implementation work if it has a question and evidence output.

## Shared execution loop

Implementation and [Verification](./verification.md) are distinct responsibilities working in a shared iterative loop, **not sequential waterfall gates**:

```text
Agreed Work Item within approved Feature/Story decisions
    ↓
Verification-first: expected behavior + acceptance/test intent (before code)
    ↓
Implementation: small slices + optional TDD → unit/regression tests PASS
                + required developer checks, with observed results
    ↓
HUMAN IMPLEMENTATION GATE: review scope, diff, checks and publish authority
    ↓
Scoped publish / Draft MR (if needed; explicit approval for EACH MR)
    ↓
FORMAL INDEPENDENT VERIFICATION: test quality + behavior + code review
    ↳ findings → scoped Implementation fix → unit tests PASS → reverify
    ↓
HUMAN VERIFICATION GATE → separate MR completion / release decisions
```

**Verification-first starts before code** by defining expected behavior and a meaningful proof strategy. Provisional review of scenarios, contracts and partial diffs is allowed during Implementation; **formal** Independent Verification starts only after observed passing developer checks and the **Human Implementation Gate**. The diagram shows the fixed approval order, **not** a mandatory waterfall for every edit or early reviewer comment. A small standalone bug can use a lean, human-approved Design/Planning path (defined below), not silently omit those approvals. Handoff information belongs in the **same issue/MR**, not mandatory duplicate documents:

- Scope and links: work item, acceptance, repository diffs.
- Change boundaries: contracts, schema, critical invariants and operational risks.
- Actual evidence: commands executed, environment, observed results, not-run checks.
- Feedback: findings, severity, evidence, owner, resolution and relevant retest.
- Human decision: continue, fix-and-reverify, blocked or ready for release review.

Scale the checks and independent review to risk. Solo work may use separate self-check and review passes; AI agreement alone is not independent human approval. After a fix, reverify affected behavior and dependencies, widening checks if impact expands. Neither workflow authorizes deployment.

## Verification-first, TDD-enabled implementation contract (agreed design direction; v1.0 candidate)

**Default for a sufficiently understood feature/bug work item:** begin with the [Verification workflow](./verification.md) to express **what counts as correct** before selecting the implementation. Use confirmed requirements, representative examples, domain invariants and known codebase contracts as the source of expected outcomes, not guesses from generated code.

**Verification owns test intent and independent judgment:** select acceptance/negative/boundary scenarios, externally observable expected outcomes, relevant **test seams** and proof surfaces. When a reliable harness exists, it may produce a small **executable acceptance/contract test** that initially fails for the intended missing behavior; otherwise a reviewable Given–When–Then scenario or repro is sufficient. A test that fails because the service is unbuilt, dependencies are unavailable or the harness is broken is **not evidence** of a behavior regression.

**Implementation owns developer tests and code:** receive the agreed scenario/seam and proceed in small vertical slices. When the seam is stable and the feedback is cheap, prefer **TDD: meaningful red (expected failure) → minimal green → refactor with behavior preserved → rerun**. Implementers own unit/regression tests, suitable integration tests and local developer checks. Do **not** write a whole suite of speculative tests up front, test private implementation details, or rewrite acceptance expectations merely to make a failing build green. For UI/manual/high-setup work, use the nearest credible executable check; give a reason when TDD is unsuitable.

**Feedback ownership:** Verification can revise *proposed* scenarios when new **source-backed** requirement evidence emerges, but material changes to agreed acceptance, design, contracts or scope return to the relevant human decision owner / affected gate. Implementation can challenge test feasibility; neither side unilaterally changes the success oracle. After code changes, Verification checks the quality of developer tests and **independently probes the actual behavior**, including meaningful failure cases. Build + unit tests are necessary feedback where applicable, **not alone sufficient safety evidence**.

**AI use:** Separate agent context/models when useful to reduce shared assumptions, not as a mandatory multi-agent tax or substitute for executed evidence and human review. Save approved scenarios, checked results and failures in existing Jira/MR links; no extra mandatory test-plan file and no new skill is created by this workflow.

**Related references:** [mattpocock TDD](https://github.com/mattpocock/skills/blob/main/skills/engineering/tdd/SKILL.md), [pstack Build & Clean](https://github.com/cursor/plugins/blob/main/pstack/docs/guide/05-build-and-clean.md) and [pstack Prove It Works](https://github.com/cursor/plugins/blob/main/pstack/docs/guide/06-verify-and-ship.md).

## Developer-check precondition for formal Independent Verification (agreed 2026-10-10)

**Implementation must first reach a credible Unit Test green state** for the assigned Work Item: run the relevant existing and new unit/regression tests plus required repo developer checks (e.g. build, lint, typecheck) and capture their **real results and environment**. Do not present a test that was not run as passing; no claimed success without actual runner output. If a required check fails, fix the cause inside Implementation and rerun it **before starting formal Independent Verification**. If tests cannot run, record **Blocked / Not run**, the reason and owner; do not silently advance as green.

**When unit tests genuinely do not apply**, e.g. a documentation-only change or a work type without a viable unit-test seam, explicitly justify that and agree on appropriate alternative developer checks with the authorized human at the existing Implementation Gate. This is an exception, not a license to skip tests for ordinary BE/BFF code.

**Timing distinction:** Verification-first **acceptance/test-intent design** and provisional review/feedback can happen before or during coding. The **formal Independent Verification assessment** (test quality, external behavior, contracts, independent diff review) happens only after the Implementation developer-check precondition and the already agreed Human Implementation Gate. Green unit tests **are an entry condition, not a correctness verdict**. Any findings return to scoped Implementation: rerun impacted unit tests first, then independently reverify and complete the Human Verification Gate. No additional approval gate is introduced.

## Working-tree and existing-change safety (v1.0 review amendment)

**Before the first write:** inspect the **actual repository, current branch/upstream and Git status**, including staged, unstaged and untracked files; inspect relevant diffs without changing them. Identify a baseline so the agent can distinguish **pre-existing human/other-agent work** from changes it is authorized to make for this Work Item.

- **Preserve existing work:** never overwrite, reset, clean, stash, discard, rebase, switch branches over, or delete pre-existing changes (including untracked files) merely to get a clean workspace. Do not modify an overlapping work-in-progress file without resolving ownership and safe isolation with the human first. Never use destructive Git commands without the separate authorization required by the scoped-Git policy.
- **Keep work isolated:** prefer a dedicated work branch/worktree **when available and authorized by local Git Flow**, or coordinate a clearly agreed safe editing scope. Creating isolation must not secretly move, stash, or remove someone else's files. If existing changes make isolation unsafe or ownership unclear, **stop and ask** rather than guessing.
- **Before committing:** recheck Git status and the actual staged/unstaged diff, attribute each change to the Work Item, and stage/commit **only authorized changes**. Avoid indiscriminate staging (for example `git add -A`) where unrelated or pre-existing work exists. Do not claim a clean baseline or a focused diff without checking.

This safety check does **not** require asking approval for every ordinary file edit inside an already approved, uncontested Work Item scope. It protects work outside that scope and does not grant new branch, push or MR permissions.

## Lean path for a small, low-risk bug or standalone Work Item (v1.0 review amendment)

The accepted general lifecycle and Delivery Planning support **risk-proportional depth**, not a new full Feature document for every tiny, reversible fix. For a small bug inside **already approved Feature/Story boundaries**, **reuse existing Design/Planning approvals**, and record the specific repro or expected behavior, affected scope, risk, owner, relevant tests and change intention in its Jira Work Item. No repeat Feature gates solely because the next Work Item starts.

For a genuinely standalone small bug **without a prior applicable approved Feature plan**, have the authorized human make **concise, explicit Design and Planning decisions** in the existing Jira/issue (they may be reviewed together in one conversation/checkpoint), confirming the expected behavior, safe approach/scope, risk/rollback and owner. **Do not treat "small" as implicit approval or skip the Track A Design/Planning decisions.** Higher-risk bugs, cross-repo contract/data changes or unresolved requirements require deeper upstream work and the appropriate human approvals.

In **both** paths, the assigned Work Item still uses Verification-first intent, scoped Implementation, **applicable unit/regression tests and developer checks passing**, a **Human Implementation Gate**, then formal Independent Verification and the **Human Verification Gate**. Repo-specific Git permissions and **per-Draft-MR human confirmation** remain unchanged. No mandatory new artifact, extra gate or general exemption is introduced.

## Activities (repeat as necessary)

| Activity | Action | Minimal evidence |
| --- | --- | --- |
| 1. Orient + protect baseline | Read code/AGENTS.md/tests/requirements and inspect branch/upstream, staged, unstaged and untracked work **before edits** | Existing behavior, scope/ownership of pre-existing changes and evidence-backed affected paths |
| 2. Agree test intent | Align with Verification's source-backed acceptance examples, expected outcomes, safe observable seams and non-goals | Linked scenarios / meaningful check, with unknowns owned |
| 3. Plan local edits | Choose a small vertical slice, its test-first seam when appropriate, minimal design-consistent code and relevant migrations/errors | Next verifiable slice, intended developer test and key unknowns |
| 4. Implement + developer tests | Prefer red → green → refactor on viable seams; otherwise use the nearest credible check; edit only authorized files, preserving other work | Meaningful test failure/pass when feasible, reviewable diff and rationale |
| 5. Developer checks | Run suitable local tests, lints, typechecks and builds; write or update behavioral/regression tests | Actual commands/results, not AI-claimed outcomes |
| 6. Handoff or iterate | Recheck own diff/staging, fix local findings, report contracts, unrun checks, migrations and dependencies | Scoped diff/commit evidence, acceptance mapping, actual test results and known gaps |

**Do not wait until the end to test.** Developer checks are part of Implementation for fast feedback; **provisional Verification feedback** is available at any point, whereas **formal Independent Verification** follows passing applicable developer checks and the Human Implementation Gate.

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

Before a review pass, remove **only unrelated edits attributable to this Work Item that the agent is authorized to change**, dead compatibility scaffolding and unsupported defensive code where safe; never remove pre-existing user changes. **Do not adopt pstack's blanket comment-removal preference**: retain comments that explain non-obvious invariants, externally imposed constraints and genuinely useful API contracts. Keep human-reviewed decisions and proof in the existing Jira/MR record.

**Candidate future skill boundaries (not yet created):** a task router (goal/done/evidence → selected procedure), codebase-grounding, implementation-by-slice and a decision-interview helper. Do not copy `/poteto-mode`, `/architect` or `/grill-me` into a mandatory `AGENTS.md` chain. Validate any eventual skill on pilot tasks and compare effort, quality and token cost against the no-skill baseline.

## Risk-aware implementation rules

- Preserve current behavior outside agreed scope; identify compatibility and side effects.
- Minimize complexity rather than simply minimizing line count. Follow existing patterns unless design evidence warrants change.
- **Consider TDD first** for changed feature/bug behavior with a stable seam and inexpensive feedback; use test-after or an alternative proof when TDD adds brittle setup or low signal. Do not mandate TDD or a coverage percentage universally.
- Treat schema migrations, retries, idempotency, authorization, concurrency and observability as part of the affected change where relevant.
- Only claim a check ran if its results were actually observed. Mark missing tools/credentials as **Not run** or **Blocked**.
- Inspect and preserve the existing Git worktree; never stage, overwrite or discard unrelated human/agent changes.
- Avoid secrets or company-confidential data in public repositories or unauthorized AI tools.
- Keep one issue/MR as the living record when sufficient; no mandatory duplicate Markdown plan.

## Cross-repository practice

Use the parent Jira Story (or standalone Work Item) as feature-level source of truth and keep assigned repository Work Items scoped. Record actual provider/consumer contracts from source evidence. An AI agent operating inside one repo **does not automatically have access to sibling repos**. Coordinate compatible changes and integration checkpoints with contract owners; do not assume a default deployment order.

## AI involvement (candidate guidance)

AI may choose appropriate task skills (hybrid routing), propose edits and execute focused, non-destructive developer checks inside the approved scope. **Scoped autonomy** allows local edits and commits on the agreed work branch, without per-file approval; additional architecture/scope changes and privileged/destructive operations require fresh human authorization. A **Fixed Human Implementation Gate** still approves the completed work item before authorized remote publishing. Push and MR creation have different permissions. No mandatory Claude/Codex split is approved. Keep AGENTS.md short; create reusable skills only after pilots.

## Approval granularity — feature decisions, work item execution (agreed for Track A)

The human approves **Solution Design and Delivery Planning once at Feature level** for the agreed direction, major contracts/dependencies, risk boundaries and full-scope progressive plan. **Implementation and Verification each receive separate human gates per agreed Work Item**; a single Feature may therefore have several Implement → Verify loops and decisions.

Before implementing the next work item, refine its acceptance, repo/contracts, dependencies and checks **within the previously approved Feature plan**. This is progressive elaboration, not an automatic new Feature Design/Planning approval. Do not presume a work item is authorized when it changes the accepted scope or introduces unresolved critical risk.

If evidence **materially changes** the accepted Feature design, scope, public/data contract, security/data risks, or delivery constraints, stop the affected direction and reopen **only the prior Design/Planning gate(s) that are invalidated**, with their appropriate human owner. Scoped fixes and tests under the approved work item continue through Q7 without reapproving every edit; retest them and obtain that work item's Verification Gate. Keep Feature-level and Work Item-level approval records linked in the existing Jira/MR, including the authorized owner and scope.

Multi-repo work can form one verifiable Work Item with linked per-repo changes; **each Draft MR still needs its own human confirmation** of actual source/target Git Flow. This is a Track A AI-assisted policy candidate, not a blanket change to the already Accepted general lifecycle. See [Q1–Q9 and final gate decision](./ai-assisted-feature-delivery.md).

## Fixed Implementation Gate, scoped commits and rework (AI-assisted candidate)

For each agreed **Work Item**, the authorized human reviews the changed scope, diff, completed developer checks, known gaps and relevant integration dependencies and **explicitly approves the Implementation Gate**. Preliminary Verification feedback is allowed before this gate; formal Verification completion remains a separate human decision.

Within the approved scope, AI may make **local commits** following repository conventions. **Normal pushes** to the specifically authorized, non-protected work branch require explicit **publish authorization at the Implementation Gate**; that authorization can cover later pushes of scoped fixes to the same branch. It never covers new destinations, protected branches, force-push, history rewrites, destructive Git actions, or unrelated changes. Company permissions override this candidate guidance.

**Scoped Rework (Q7=B):** After implementation approval, findings may be fixed and proportionately retested within the same agreed scope/design/risk envelope without asking for Implementation approval on each edit. The human still approves the **Verification Gate** after reviewing the final evidence. Material changes to scope, accepted design, contracts, security/data risk or planned delivery require re-approval of the **affected earlier gate(s)**.

**MR Completion (Q9=A):** AI can report status and suggest readiness. Only an authorized human may mark Draft MR as Ready or Merge; never auto-merge or deploy. See [AI-assisted design decisions](./ai-assisted-feature-delivery.md) for Q1–Q9 and the per-MR confirmation policy.

## GitLab Draft MR — mandatory per-MR human confirmation (v1.0 candidate)

After the **Implementation Gate**, AI must first investigate **the actual repository and work-item Git Flow**, including documented branching/contribution rules, Jira/work item, source branch, proposed target branch, release/integration strategy and dependent MRs. Never assume that `master` (or the default branch) is the right target.

**Before creating every single Draft MR**, show the human the repository, source → proposed target branch, target rationale, work-item link and relevant ordering constraints; **ask for and wait for explicit MR-specific approval**. Previous implementation approval or scoped Git permissions do not waive this question. Changing the MR target afterward also requires renewed human confirmation.

**Commit/push** follow the scoped rules above; **each Draft MR still needs separate explicit human approval**, even after a push was authorized. Humans alone mark Ready and Merge; deployment requires its own authorization.

## Handoff to Verification

Provide:
1. Pre-agreed acceptance examples, test intent/seams and changed boundaries/repositories.
2. Reviewable diff and contract/schema changes.
3. Tests run, exact outcomes and environments; tests not run.
4. Key failure/permission scenarios and expected behavior.
5. Remaining uncertainties and rollback/release concerns.

**Exit options:** Ready for Verification; Needs More Implementation; Blocked — Discovery/Design; Stop/Defer. **Ready for Verification is not release authorization.**

## Release-candidate review criteria

1. Feature-level Design/Planning approvals are distinct from per-Work-Item Implementation/Verification gates, with only affected prior gates reopened on material change.
2. Separate ownership without mandatory handoff for each edit.
3. Evidence and findings shared through one existing issue/MR.
4. Verification-first acceptance/test intent, TDD where viable, and risk-proportional independent behavior evidence.
5. Existing working-tree/user changes are protected, including before edits and commits.
6. Small bug/standalone Work Item uses risk-proportionate upstream depth **without bypassing applicable human Design/Planning and Work Item gates**.
7. Real repo access and Skill implementation remain separately authorized.

## Pilot / open decisions

Try a tiny regression, a medium feature and a simulated cross-repo contract change. Measure first useful feedback, rework, developer-test honesty and documentation overhead.

**Review focus:** Confirm that human gate owners and accepted-work item boundaries can be identified in each actual team, that publish authorization is recorded once in the existing issue/MR, and that early feedback does not bypass fixed gates. Git Flow, target branch and mandatory CI checks remain repository/task-specific.

**Release Candidate — awaiting explicit approval.** No AGENTS.md policy, automation, skills or repository permissions are changed.
