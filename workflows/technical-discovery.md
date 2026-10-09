# Technical Discovery Workflow — v0.1

- **Status:** Proposed / Draft — awaiting review
- **Date:** 2026-10-10
- **Track:** A — Build My Engineer
- **Foundation:** [Accepted FDL v1.0](./feature-delivery-lifecycle.md)
- **Upstream:** [Requirement Discovery v1.0](./requirement-discovery.md)
- **Next activity:** Solution Design (to be defined)
- **Scale:** Solo developer → feature team → product organization
- **Example:** Fictional personal-finance application (no confidential data)

## Purpose

**Problem solved:** Engineers often commit to a solution before understanding existing behavior, actual architectural boundaries, contracts, data semantics and operational constraints. This leads to missed dependencies, regressions, unrealistic plans and unjustified redesigns.

**Goal:** Establish evidence-backed technical understanding sufficient to identify constraints, feasibility risks, affected boundaries and design questions. Technical Discovery **does not choose the final solution** or mandate a particular stack.

**Success:** Someone unfamiliar with the task can explain what is known, what is uncertain, where evidence came from, what can break and which questions Solution Design must resolve.

## Inputs and entry paths

Use the [Discovery Summary](./requirement-discovery.md) or equivalent lean statement containing:
- Problem, affected users and desired outcome.
- Candidate scope, constraints and critical unknowns.
- Decision/next-step owner and any available acceptance examples.
- If requirements are incomplete, identify assumptions; technical exploration can help uncover more requirement questions.

### Choose the appropriate discovery mode

| Mode | When used | Key investigation |
| --- | --- | --- |
| **Existing system / brownfield** | A feature changes deployed or legacy software | Current behavior, call/data flows, ownership, contracts, coupling, regressions |
| **New product / greenfield** | No production codebase or major prior architecture exists | Available platforms, technical feasibility, critical capabilities, NFRs, build-vs-buy, constraints, proof-of-concept needs |
| **Hybrid / integration** | Existing services plus new components, imports or third parties | System boundaries, external contracts, permissions, data movement, failure/retry behavior |

Do not manufacture an as-is code map for greenfield work. Do not choose microservices versus monolith merely because a project is new.

## Principles

1. **Observe before assuming:** Code, schemas, tests, runtime behavior, docs and responsible humans are distinct evidence sources; label their freshness and reliability.
2. **Trace a question, not the whole repository:** Start from relevant user behavior/entry points; expand only when dependency evidence requires it.
3. **Evidence ≠ inference:** Cite relevant paths/symbols, version/revision, tests or measurements for consequential conclusions; mark hypotheses.
4. **Read-only first:** Explore without source mutations, migrations, secret exposure or production writes. Request authorization for experiments with side effects.
5. **Risk proportional:** Local reversible change can use a short note. Cross-repo, high-volume financial, privacy/security or migration work warrants deeper investigation.
6. **No premature solution lock-in:** Identify constraints, options to explore and trade-offs, but defer final architecture choice to Solution Design.
7. **Iterative:** Discovery may surface new product questions and return work to Requirement Discovery; prototype/spike when observation alone cannot answer a critical unknown.

## Five activities (not mandatory sequential gates)

| Activity | Key questions | Evidence/output | Failure prevented |
| --- | --- | --- | --- |
| **1. Orient & scope** | What capability changes? Brownfield/greenfield? Relevant systems, owners, version, environments? Which critical unknowns need answers? | Exploration boundary, target flows, available evidence | Unbounded repository archaeology |
| **2. Trace current behavior / capabilities** | For brownfield: where does the behavior enter, execute, read/write data and exit? For greenfield: what capabilities, constraints, integrations and feasibility assumptions exist? | As-is trace or greenfield capability/constraint map | Designing against imagined architecture |
| **3. Investigate dependencies & contracts** | What APIs, schemas, events, external services, identities/permissions, data sources and other repos are involved? Who owns them? | Dependency map, contract/data semantics, ownership, unknowns | Breaking integrations or data meaning |
| **4. Assess change impact & risks** | What could regress? Which failure modes, concurrency, performance, security/privacy, migration, observability and operational constraints matter? | Blast-radius assessment, test/release implications, risks | Hidden side effects and misleading estimates |
| **5. Synthesize & route** | Which findings are supported? Which assumptions need experiments? Are we ready to compare designs, or must we clarify requirements? | Technical Discovery Summary and next action | False certainty or premature implementation |

### Suggested brownfield investigation path

1. Choose one user journey/use case and find actual entry points.
2. Trace through UI/API, validation, authorization, business logic, persistence, asynchronous jobs and external calls **only where present**.
3. Check tests and deployment/runtime configuration for behavior not obvious in code.
4. Search additional repositories only when a contract/dependency requires it; explicitly list repositories **not inspected**.
5. Identify responsible owners and divergent documentation or behavior.
6. Record source references (repo/path/symbol plus revision or commit, where possible), rather than copying large code blocks.

### Suggested greenfield investigation path

1. List required capabilities, quality attributes and business/time/team constraints.
2. Identify external data sources and integration access/feasibility.
3. Clarify major domain and data correctness properties (for a finance product: transaction semantics, duplicate detection, privacy).
4. Evaluate technical unknowns via focused spikes (e.g., parse a synthetic statement sample), **not** full production architecture.
5. Document remaining options and constraints for Solution Design.

## Only required artifact: Technical Discovery Summary

**Consumers:** Engineers/Tech Lead use it to compare solutions and plan safe changes; QA uses risk areas to inform verification; product owner uses it to understand scope/feasibility trade-offs; operations/security join when affected.

Keep it in the same work item, a concise Markdown doc, or a shared feature record. Create diagrams/POCs only if a real decision needs them.

```markdown
# Technical Discovery — <Feature>
Status: Draft | Evidence sufficient for design | More exploration needed | Blocked
Mode: Brownfield | Greenfield | Hybrid
Owner / reviewed by:
Scope / linked problem:
Evidence snapshot: repositories, branches/commits, environment, date

## Technical context
Relevant boundaries, constraints, owners and environments.

## Observed behavior / feasibility
For brownfield: evidence-backed as-is flow.
For greenfield: proven capabilities and constraints; no fake as-is diagram.

## Dependencies & contracts
Interfaces, schemas, integrations, data source semantics; known vs assumed.

## Impact & risk
Potential blast radius, data correctness, security/privacy,
performance, concurrency, migration and operational considerations.
Record only applicable items and explain material exclusions.

## Evidence ledger
Finding | source/path/commit/test/owner | confirmed/inferred/unknown

## Open questions
Question | impact if wrong | next validation | owner

## Handoff decision
Enough evidence to compare solution options? Why?
Route: Solution Design | Spike | Requirement clarification | Blocked
```

### Avoid duplicated bureaucracy

- Small change: one short issue comment with current behavior, touched area, test impact and unknowns can be sufficient.
- Cross-component work: explicit dependency flow and interfaces.
- High-risk change: include data invariants, privacy/security review inputs, failure modes and release/migration risks.
- A diagram is **optional**; its consumer and the unanswered question must justify it.

## Quality gate — evidence sufficient for design

Not a sign-off on a final design. The responsible engineers can answer:

1. What behavior/capability are we changing and what was actually inspected?
2. Where does the relevant data come from, move, change and persist (or which greenfield data path must be proven)?
3. Which component/contract/owner dependencies and blast radius are plausible?
4. Which critical assumptions, security/privacy/data concerns and operational risks remain?
5. Is there enough evidence to compare viable approaches responsibly, or is a targeted experiment/clarification cheaper?

If a critical uncertainty could invalidate the proposed direction, **do not mark evidence sufficient** without a mitigation or a named next validation. It is still acceptable to proceed with an explicit *design spike*, not a fully committed implementation.

## Human roles and scalable application

| Scale | Ownership and approach |
| --- | --- |
| **Solo** | Developer conducts bounded investigation and records a concise evidence/unknowns note. Self-review key assumptions. |
| **Team** | Feature engineer traces, Tech Lead coordinates cross-boundary technical review, QA/operations review relevant risks, and owners confirm contracts. |
| **Product / multi-team** | Domain/service owners jointly map dependencies and interface changes; security/platform/data specialists contribute according to risk; a named technical decider resolves cross-team trade-offs. |

The Tech Lead does not need to read every file or personally approve every low-risk discovery.

## Fictional personal finance example

**User outcome from discovery (hypothesis):** Understand spending with little manual work and estimate spendable money after upcoming obligations.

### Brownfield hypothetical
An existing app has a transaction entry screen and monthly summary. Investigate:
- The actual request path from entry to calculation, with referenced code.
- Whether transfers between owned accounts are treated as expenses.
- How pending card charges and refunds affect aggregates.
- Date/timezone and currency semantics.
- Whether statement import will create duplicates, and what guarantees current storage provides.

**No claim is made about real code or production behavior.**

### Greenfield hypothetical
No app exists. Technical questions include:
- Can users obtain machine-readable statements and what permissions apply? Validate using synthetic or authorized redacted samples.
- Can a parser distinguish purchases, transfers, refunds and bank fees with reasonable reliability?
- What trust/privacy posture and storage minimization does the product require?
- What does incomplete cash transaction coverage mean for displayed confidence?
- What financial calculation invariants must Solution Design preserve?

**Possible spike:** Evaluate a small statement parser against fictional CSV samples and document observed accuracy/unsupported formats. This is a learning experiment, not a decision to ship CSV import.

## AI assistance (not a new Skill)

Allowed with appropriate access: identify likely entry points, summarize local code paths with file references, compare declared contracts, draft a risk checklist, identify test gaps and label unsupported claims.

Not allowed without explicit authorization: mutate repositories, pull sensitive production data, execute destructive commands, run arbitrary third-party scripts, submit confidential code to unapproved tools, or claim unrun tests succeeded.

Never infer a complete architecture from a single opened file. AI conclusions must name their evidence and inspection limits.

## Pilot & evaluation

Pilot a **small reversible brownfield change** and a **greenfield feasibility question** in permitted/synthetic environments. Compare:
- Time to answer the critical technical question.
- Consequential facts backed by code/test/observations vs unsupported inferences.
- Critical impacted components/dependencies discovered before design.
- Downstream redesign or rework due to missed dependencies.
- Burden of artifacts relative to risk.

Avoid extrapolating measurable productivity benefits from one pilot.

## Review questions for v0.1

1. Is a **single Technical Discovery Summary** enough for solo, team and multi-repo cases?
2. Should the evidence ledger be mandatory for all findings or only consequential ones?
3. What minimum dependency/impact detail does a moderately risky change require?
4. When should a focused spike replace further document review?
5. Is the greenfield mode distinct enough from Solution Design and Product Discovery?

**Pending approval:** This is a proposed Track A workflow. [FDL v1.0](./feature-delivery-lifecycle.md) and [Requirement Discovery v1.0](./requirement-discovery.md) remain accepted independently. No new `puen-stack` Skill is approved or created.
