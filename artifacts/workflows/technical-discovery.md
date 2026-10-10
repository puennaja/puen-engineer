# Technical Discovery — Artifact Template (EN)

> **Template, not completed investigation.** Cite actual inspected files, commits, test results and owner statements; do not claim to have inspected inaccessible repositories. Keep the summary in the existing Work Item/feature record when practical.
>
> Workflow: [Technical Discovery v1.0 (Accepted)](../../workflows/technical-discovery.md) · [Thai version](./technical-discovery.th.md)

- **Feature / work link:** <Requirement Discovery / Jira>
- **Status:** Draft | Evidence sufficient for design | More exploration needed | Blocked
- **Mode:** Brownfield | Greenfield | Hybrid
- **Technical owner / reviewer:** <actual person, TBD if not known>
- **Evidence snapshot:** <repos, branch/commit, relevant runtime/env, date; N/A if not inspected>
- **Bounded investigation question:** <what do we need to learn before design?>

## 1. System context and boundaries

- **Observed services/components and ownership:** <repo/service/path and owner; label unknown>
- **Relevant runtime, storage, access and deployment assumptions:** <confirmed facts vs unknowns>
- **Known technical constraints / NFRs:** <source-backed; no invented throughput/latency target>

## 2. Actual behavior or greenfield feasibility

- **Brownfield as-is flow:** <trigger → API/service → data/events → effects; link exact evidence>
- **Greenfield feasibility:** <proved constraints/capabilities; no fabricated as-is flow>
- **Inspection performed:** <Read/search/trace/authorized experiment and observed result>
- **Not inspected or unavailable:** <limitations; avoid guessed architectural claims>

## 3. Dependencies, interfaces and data contracts

| Producer / consumer | API / event / schema / invariant | Evidence location (commit/path/link) | Confirmed / Inferred / Unknown | Owner |
| --- | --- | --- | --- | --- |
| <boundary> | <contract> | <source> | <confidence> | <person/team> |

**Cross-repo limitation:** <repos not available to this investigator and who can validate them>

## 4. Impact and risks (only applicable dimensions)

- **Possible blast radius:** <users, services, workflows>
- **Security / authorization / privacy / data correctness:** <findings or N/A with reason>
- **Concurrency / performance / retries / observability:** <findings or N/A with reason>
- **Migration / compatibility / recovery implications:** <what is known, what is unverified>

## 5. Evidence ledger

| Finding | Actual evidence: path/commit/command/result/owner | Confirmed / Inferred / Unknown |
| --- | --- | --- |
| <finding> | <source> | <status> |

**Executed checks:** <commands/permissions/environment + actual Pass/Fail/Not run/Blocked; no invented output>

## 6. Critical unknowns and handoff

| Question | Impact if wrong | Next experiment / clarification | Owner / date |
| --- | --- | --- | --- |
| <question> | <impact> | <next action> | <owner> |

- **Recommended route:** Solution Design | Time-boxed spike | Requirement clarification | Blocked
- **Rationale and remaining risk:** <why design is / is not supportable yet>
- **Technical reviewer / decision / date:** <record or Pending>
- **Input to Solution Design:** <proven constraints, plausible alternatives, interfaces and open risks>

**Boundary:** Evidence sufficient **for design** is not authorization to implement. Separate ADRs/diagrams/POCs are optional, only when they materially improve a decision.
