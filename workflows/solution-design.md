# Solution Design Workflow — v0.1

- **Status:** Proposed / Draft — awaiting review
- **Date:** 2026-10-10
- **Track:** A — Build My Engineer
- **Foundation:** [FDL v1.0 — Accepted](./feature-delivery-lifecycle.md)
- **Upstream:** [Requirement Discovery v1.0](./requirement-discovery.md) and [Technical Discovery v1.0](./technical-discovery.md)
- **Downstream:** Delivery Planning (not yet defined)
- **Scale:** Solo engineer → feature team → product organization
- **Example:** Fictional Personal Finance App only; this repository is public

## 1. Why this workflow exists

**Problem:** A team may jump from an apparently understood requirement to its first plausible technical solution. Consequences include unnecessary complexity, undocumented trade-offs, missed failure modes, broken contracts, untestable plans and designs that optimize for novelty instead of outcomes.

**Goal:** Choose and explain an appropriately simple, feasible, safe technical approach that satisfies the user outcome and verified constraints, while exposing important uncertainties for validation.

**Not the goal:** Producing exhaustive design documents, imposing specific architectural styles, estimating every task, or selecting technologies without evidence.

**Core rule:** Design is a set of decisions under constraints, not a contest to draw the largest architecture.

## 2. Input: evidence, not assumptions disguised as requirements

Use the upstream summaries or equivalent:
- Problem, outcome, actors, candidate scope and acceptance examples.
- Observed current behavior / greenfield feasibility, dependencies and source-backed technical constraints.
- Relevant non-functional needs: security/privacy, latency, consistency, reliability, accessibility, maintainability, operations.
- Critical unknowns, risk level, affected system boundaries, business/team/time constraints.
- Named technical decision owner and participants (or explicitly mark unknown).

If an unresolved assumption could overturn the design, route to a question, experiment or spike first. Technical Discovery and Solution Design may be revisited iteratively.

## 3. Seven lightweight design activities

Activities can be combined, skipped if inapplicable or revisited. They are **not waterfall gates**.

| Activity | Questions/actions | Minimum useful outcome | Prevents |
| --- | --- | --- | --- |
| **1. Reframe the decision** | What outcome are we targeting? What is in/out? Which constraints are verified? What is expensive to reverse? | Design goal, boundaries, decision drivers | Solving a different problem |
| **2. Generate viable options** | Can existing capability be reused? What is the simplest viable option? What alternative handles likely growth or failure? Is no build / smaller scope possible? | 1–3 realistic options, including reasons to reject obvious ones | First-idea bias and overengineering |
| **3. Compare trade-offs** | Compare correctness, complexity, cost, delivery speed, security/privacy, performance, maintainability, migration and operability **as relevant**; who bears each cost? | Reasoned comparison with assumptions/evidence | Architecture by preference or hype |
| **4. Specify chosen behavior** | Trace to-be user/data flow; define responsibility boundaries, inputs/outputs, API/event contracts, domain invariants, persistence, state transitions, auth and failure handling. | Reviewable to-be design at required fidelity | Ambiguous interfaces and incorrect behavior |
| **5. Design validation & change safety** | What tests prove invariants? How to migrate, integrate, observe, deploy, roll back/recover? How to handle concurrent, duplicate or partial operations? | Verification strategy and release/operational implications | Designs that cannot be safely shipped |
| **6. Review & decide** | Can implementers, affected service owners and relevant experts challenge assumptions? Are high-impact disagreements resolved? Who decides? | Agreed approach or documented unresolved issue; ADR only when warranted | Silent disagreement and hidden risk |
| **7. Handoff incrementally** | What thin vertical slice or experiment should be planned first? What contracts and decisions must be communicated? | Design Summary + planning-ready boundaries, tests, dependencies | Large upfront designs without usable next step |

### Option comparison guidance

- Require **at least a credible alternative** when the decision is consequential or difficult to reverse. For a trivial, reversible change, document why the obvious solution is sufficient rather than inventing alternatives.
- Prefer a short qualitative comparison based on actual requirements. Numerical scoring is optional and should not imply objectivity without data.
- Trade-offs are contextual; no rule that modular monolith, microservices, event-driven, serverless or any language is always best.
- Prefer reusing proven patterns when their constraints fit; be willing to deviate with explicit reasons.

## 4. Design concerns — review only what matters

| Concern | Questions to resolve when relevant |
| --- | --- |
| **Domain correctness** | What are the entities, business rules, invariants, rounding/time/currency rules and state transitions? |
| **Interfaces & dependencies** | API/event schema, error contracts, versioning, ownership, backward compatibility and cross-repo sequencing? |
| **Data** | Sources, consistency, idempotency, deduplication, retention, schema migration and auditability? |
| **Security & privacy** | Authentication, authorization, least privilege, sensitive data, secrets, threats, consent and data minimization? |
| **Reliability & concurrency** | Partial failures, retries, ordering, transaction boundaries, contention, recovery and degraded mode? |
| **Performance & scale** | Workload assumptions, service limits, latency budgets, bottlenecks and evidence for capacity? |
| **Operability** | Logging/metrics/traces, alerting, deployment, rollback/recovery, cost and service owner? |
| **Testing** | Unit/integration/contract/end-to-end strategy; meaningful edge cases; acceptance evidence? |
| **Maintainability** | Complexity, ownership, readability, extensibility, team familiarity and technical debt? |

Avoid treating the table as a mandatory checklist for every small change. Unknown numeric NFR targets are explicitly **unknown**, not made-up numbers.

## 5. One core artifact: Solution Design Summary

**Consumers:** Implementers use interfaces and boundaries; Tech Lead/service owners review trade-offs and safety; QA uses verification criteria; Product understands meaningful cost/scope constraints; operations/security contribute when impacted.

A short Jira issue/comment may be enough. Diagrams, API contracts, proof-of-concept results and ADRs are **optional linked artifacts**, added only if they answer an important question or outlive a single feature.

```markdown
# Solution Design — <Feature>
Status: Proposed | In Review | Accepted for Planning | Needs Discovery / Spike
Owner / technical decider:
Links: Problem / Requirement Discovery / Technical Discovery
Scope & risk level:

## Goal & constraints
Problem/outcome, boundaries, confirmed constraints, unresolved assumptions.

## Decision drivers
Most important correctness, security, performance, time, cost or maintenance criteria.

## Options & trade-offs
Option | benefits | costs/risks | evidence / assumptions
Why the recommended approach fits better than viable alternatives.

## Proposed solution
To-be flow and responsibilities; relevant API/event/data contracts,
business invariants, authorization, failure cases and compatibility.
Add diagram only when it makes a critical relationship clearer.

## Validation & rollout implications
Tests and acceptance coverage; migration, release, monitoring,
rollback/recovery or operational owner as applicable.

## Risks, unknowns & next evidence
Concern | impact | mitigation / experiment | owner

## Decision / review
Chosen approach, rationale, decision maker, reviewers, date,
dissent or deferred trade-offs.

## Handoff to planning
Smallest testable increment, cross-team dependencies,
relevant acceptance and test obligations.
```

**ADR rule:** Write a separate ADR only for significant, long-lived, cross-team, controversial, security-sensitive or expensive-to-reverse decisions. Reuse the [ADR template](../templates/adr.md). A routine local design decision can live in the Summary.

## 6. Quality gate: ready for planning, not automatically for implementation

A responsible human can answer:
1. Why does the chosen approach fit the confirmed problem and constraints?
2. What alternatives were considered (or why comparison is unnecessary)?
3. Are key domain/data invariants, boundaries and contracts explicit enough to plan work?
4. How are important failure, privacy/security, compatibility and operational risks addressed or owned?
5. What proves correctness, and what is the smallest safe increment?
6. Who owns the decision and unresolved questions?

Possible outcomes:
- **Accepted for planning:** enough design agreement to break down delivery work; residual risk is explicitly owned.
- **Design spike:** a targeted proof is cheaper than more discussion.
- **Return to discovery:** requirement or technical finding is materially incomplete.
- **Defer / reject:** risk, investment or constraints outweigh value; record why.

A design review is not an approval ceremony for all tiny changes. High-risk changes can require formal reviews according to their actual policies.

## 7. Scaling and decision ownership

| Profile | Typical design discipline |
| --- | --- |
| **Solo** | Quick alternatives and a short summary; self-check assumptions and testability; ask another reviewer when risk warrants. |
| **Team** | Implementer proposes; Tech Lead/peers review interfaces, alternatives and impact; relevant owners approve boundary/contract changes. |
| **Product / multi-team** | Explicit design owner and decision maker; platform/security/data/operations contribute where risks cross boundaries; record cross-team contracts and migration sequence. |

The Tech Lead leads technical trade-off resolution but does not unilaterally decide product priority, staffing or every low-risk change.

## 8. Worked fictional example — personal finance statement import

### Discovery context (unvalidated sample)

A fictional user wants low-effort spending visibility, uses bank accounts and cash, and may be willing to import bank statements monthly. Feasibility of supported statement formats and product acceptance are **not proven**.

### Design decision to explore

**How should imported transactions become trustworthy records without double-counting transfers or duplicate uploads?**

Candidate approaches (illustrative, not architecture approval):

| Option | Benefits | Risks / trade-offs |
| --- | --- | --- |
| **A. Upload and parse directly into canonical transactions** | Small initial surface, faster for a prototype | Partial failures and mapping mistakes may pollute financial summaries; dedupe harder to correct |
| **B. Upload → validate/preview → confirm import into canonical transactions** | User reviews classification/errors before commit; clearer audit and rollback boundary | Extra UX and state handling; may be too much friction |
| **C. Import through managed specialist provider** | May handle multiple formats and normalization | Privacy, cost, provider availability, integration and data ownership risks |

**Provisional design hypothesis:** B may be worth testing for trust and error prevention, but it is **not selected** until import effort, usability, security and file-format feasibility are validated.

Possible invariants/edge cases to investigate:
- Same source transaction imported twice should not silently double count.
- Transfers between owned accounts are not automatically expense/income.
- Refunds, pending card transactions and bank fees have distinct semantics.
- Displayed totals must make incomplete cash coverage and data confidence apparent.
- A failed import should not leave an unexplained partial committed state.
- Sensitive uploaded statements require authorized access, retention and deletion policy.

**Next thin experiment:** Synthetic CSV fixtures with duplicates, internal transfers, refunds and malformed rows; compare prototype ingestion and user review effort. Do not upload real bank statements to a public repo.

## 9. AI assistance — no new Skill at this stage

AI can draft options, challenge assumptions, structure comparisons, trace supported technical facts back to [Technical Discovery](./technical-discovery.md), suggest edge cases and review a design for missing tests or failure modes.

Human responsibility includes choosing design trade-offs, verifying claims, reviewing security/privacy decisions and approving changes. AI must not assert evidence it has not observed, execute unsafe mutations without authorization or invent performance/cost data.

Build a reusable skill **only after** observing repeated time-consuming design work and validating that the skill provides measurable value with reviewable results.

## 10. Pilot evaluation and pending review

Pilot with:
1. **Low-risk local change:** Does the workflow stay brief without forced diagrams/ADRs?
2. **Moderate/high-risk fictional statement import:** Does it expose data integrity, privacy, migration, testability and user-friction trade-offs before coding?

Look for fewer unsupported design assumptions, early discovery of critical risks, actionable implementation boundaries and proportional document effort; do not claim productivity improvements without evidence.

**Questions for approval:**
- Is the single Design Summary sufficient as default?
- Is the ADR threshold appropriate without creating decision-document overload?
- Are the quality gates strong enough for data-sensitive/high-risk work?
- Does the distinction between *Accepted for Planning* and *Ready for Implementation* remain clear?
- Is the workflow flexible enough for brownfield, greenfield and multiple teams?

**Status stays Draft until explicit approval.** This document does not change the accepted FDL, Requirement Discovery or Technical Discovery workflows.
