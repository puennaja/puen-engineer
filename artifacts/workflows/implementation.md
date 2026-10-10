# Implementation / Developer Handoff — Artifact Template (EN)

> **Per assigned Jira Work Item.** This is a reusable section for Jira/MR, **not** a required extra file. Source-backed acceptance and Verification-first preparation may precede coding. Record actual execution; never auto-mark a gate approved.
>
> Workflow: [Implementation v1.0 (Accepted)](../../workflows/implementation.md) · [Thai version](./implementation.th.md)

- **Work Item (existing Jira Story / Sub-task / Bug / Task):** <link>
- **Parent Feature / approved Design + Plan:** <links and Human Gate records>
- **Status:** Planned | Implementing | Developer checks passed | Implementation Gate pending | Ready for formal verification | Blocked
- **Assigned engineer / authorized repository scope:** <owner, repos, exclusions>
- **Risk level / reason:** <low/medium/high, data/security/contract impact>
- **Branch / fixed diff base:** <actual branch, base commit(s); never assume target branch>

## 1. Goal, acceptance and boundaries

- **Work Item outcome / source-backed acceptance:** <scenarios and expected behavior independent of implementation>
- **In scope / out of scope:** <specific services and excluded FE/BE/BFF work>
- **Affected API/event/data contracts & migrations:** <known changes with owner and compatibility plan>
- **Important negative cases / invariants:** <permissions, idempotency, concurrency, validation as applicable>
- **Verification-first input:** <early acceptance examples/test plan/link; provisional feedback is not formal verification>

## 2. Working-tree and edit safety

- **Before editing:** <git status, active branch, dirty/untracked user-owned files, allowed paths>
- **Existing changes to preserve:** <not overwritten; if overlap, human clarification>
- **Editing approach / boundaries:** <scoped changes, no unrelated refactor/destructive command>
- **After editing:** <diff/status reviewed, own vs pre-existing changes differentiated>
- **Files / commits in scope:** <actual paths and commit SHAs; N/A until available>

## 3. Implementation changes and important reasoning

| Change / file | Why needed for acceptance | Contract / data / risk effect | Evidence |
| --- | --- | --- | --- |
| <file or diff> | <reason> | <effect or None verified> | <diff link> |

**New assumptions / material scope changes:** <return to affected Design/Planning Human Gate(s) if necessary>

## 4. Developer-owned checks — actual results only

| Required? / reason | Command or manual check | Safe environment / fixtures | Expected vs observed | Pass / Fail / Not run / Blocked / Inconclusive | Evidence link |
| --- | --- | --- | --- | --- | --- |
| <required/optional> | <actual command> | <env> | <result> | <status> | <CI/log> |

- **Applicable unit / regression tests:** <what ran and exact outcome>
- **Repo-required build / lint / typecheck / CI:** <what ran>
- **No suitable unit-test seam (exception only):** <why, risk-appropriate alternative proof and explicit Human Implementation Gate agreement; never pretend Pass>
- **Unrun/failed checks and blocker owner:** <specific gap; required failures prevent formal verification entry>

## 5. Human Implementation Gate — per Work Item

- **Proposed disposition:** Ready for verification | More Implementation | Blocked (Discovery/Design) | Stop/Defer
- **Review packet:** <agreed scope, diff/base, real checks, contract risks, known gaps>
- **Authorized Human Gate owner / decision / date / link:** <explicit approved/rejected/pending record>
- **Publishing authorization (separate):** <approved work branch + scoped push rights, or None/Pending>
- **Formal Independent Verification entry:** <only after applicable developer checks actually pass and Human Gate approves>

## 6. MR publication and Verification handoff

- **Draft MR proposal, if requested:** <repo, source branch, proposed target, target rationale, Jira link, dependency/stacking>
- **Explicit human confirmation for each Draft MR:** <person/date/link or Pending — no automatic creation>
- **Actual MR / commit / pipeline links:** <only after real action and permission>
- **Verification handoff:** <fixed diff base, acceptance oracle, executed test evidence, regression/negative cases, gaps, dependencies and relevant release risk>
- **Rework:** <finding → scoped fix → affected developer tests rerun → independent reverification>

**Authority boundary:** Only a **human** may mark Draft MR Ready or Merge; neither code/test success nor this template permits deployment. Do not bypass local GitLab branch, CI, reviewer and security policies.
