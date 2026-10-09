# Feature Delivery Lifecycle (FDL) — v1.0

> **Architecture note (2026-10-09):** This foundation describes engineering activities, **not mandatory sequential phases**. Apply an iterative, risk-proportional delivery model. See [Universal Engineering System](../architecture/universal-engineering-system.md) and [ADR-0001](../decisions/0001-universal-engineering-system.md).

- **Status:** Accepted — My Engineer foundation (not automatically a company-wide policy)
- **Approved:** 2026-10-09 by repository owner
- **Version:** 1.0
- **Created:** 2026-10-09
- **Owner:** My Engineer
- **Scope:** One feature or change, from an informal request to verified production outcome
- **Companion:** [AI-assisted feature delivery](./ai-assisted-feature-delivery.md)
- **Repository:** Public — use fictional examples only; never publish internal tickets, credentials, code, or architecture

## Approval scope

This lifecycle is the accepted **baseline for My Engineer**. Its activities, role boundaries, traceability, and risk-proportional evidence define what should be considered across feature delivery. The numbered stages are a reference model, **not a mandatory sequential pipeline or fixed set of ceremonies**. Teams and individuals may combine, revisit, or skip inapplicable activities when justified by risk and context. Templates, specific automation, enforcement rules, and integrations require separate evaluation and agreement.

**Improvement principle:** Every workflow must solve an identifiable problem; every artifact must have an intended consumer; every skill must be validated in real work before scaling. Prefer understanding and practice before automation.

## Purpose

Create a repeatable, lightweight method to turn unclear requests into delivered, maintainable software. This is **not** a strict waterfall process. Revisit discovery and design when evidence changes. Scale documentation and ceremonies to the risk/size of work.

**Core principles**
1. Understand the **problem and success condition** before accepting a solution.
2. Separate **confirmed facts, assumptions, decisions, and open questions**.
3. Assign decision ownership; the Tech Lead is accountable for technical direction, not every product or people decision.
4. Design for change and testability; do not introduce unnecessary architecture.
5. Evidence over assertions: link critical choices to code, experiments, tests, or stakeholder confirmation.
6. AI may propose and prepare; humans retain accountability for product decisions, code changes, and release authorization.

## Roles and decision ownership

Roles may be combined in a small team; their **responsibilities** must still be explicit.

| Role | Core responsibility | Decisions |
| --- | --- | --- |
| Product Owner / Requester | Problem, users, business outcomes, priority, scope acceptance | What and why; acceptance of scope |
| Tech Lead | Technical feasibility, design quality, risks, technical coordination | How, with team input; escalates trade-offs |
| Engineers | Discover, design, implement, test, document | Local implementation within agreed boundaries |
| QA / Quality owner | Test strategy, exploratory/acceptance verification | Quality evidence; raises release blockers |
| Engineering Manager / Delivery lead | Capacity, staffing, coaching, delivery constraints where the role exists | People management, staffing and escalation |
| Service / Release owner | Operational readiness, deployment and incident ownership | Release approval per team policy |

When role ownership conflicts, explicitly record who is the **decision maker**, consulted parties and escalation route. No role is presumed to approve all gates alone.

## Lifecycle and quality gates

| Stage | Main activities | Minimum artifact / evidence | Exit gate (human owner) | Candidate AI assistance |
| --- | --- | --- | --- | --- |
| 0. Intake & triage | Capture request; clarify urgency, affected users, priority and risk | Short intake / problem statement | Requester agrees on the problem to investigate | Summarize request, enumerate unknowns |
| 1. Discovery | Verify current behavior, actors, pain points, goal and success measure; consider *not building* | Problem + success criteria, assumptions | Product owner validates purpose | Interview prompts, discovery checklist |
| 2. Requirements | Define in/out scope, functional/nonfunctional requirements, acceptance cases, permissions and edge cases | Lean requirement spec; testable acceptance criteria | Product/QA/Tech agree that the work is understandable enough to explore | Draft scenarios; find ambiguity and contradictions |
| 3. Technical exploration | Trace code and data, dependencies, existing patterns, contracts, migration and blast radius | As-is flow, impacted components, technical risks | Tech Lead/implementers can explain current behavior with evidence | Read-only codebase explorer with file references |
| 4. Solution design | Compare viable options; define to-be flow, interfaces, data, failure modes, testing, release approach | Design note; ADR for significant decisions | Team design review; unresolved critical decisions escalated | Alternatives, review questions, risk checklist |
| 5. Delivery planning | Break down vertical slices; identify dependencies, estimates/ranges, capacity, owners and checkpoints | Task list, sequencing, definition of done, expected risks | Product and delivery team align on scope and feasible plan | Task breakdown, dependency detection |
| 6. Implementation | Small changes, pair/review, maintain contracts, capture deviations | Code and tests; links to plan/decisions | Implementers confirm requirements addressed and checks executed | Focused code assistance, regression-test suggestions |
| 7. Verification & review | Automated tests, security checks as needed, QA/UAT, independent code review | Test evidence, review decisions, known limitations | Quality owner + responsible engineer assess readiness | Diff review, test-gap analysis |
| 8. Release & observe | Rollout, migrations, monitoring, rollback, operational communication | Release checklist, rollback plan, dashboard / support owner | Authorized release owner approves per local policy | Draft runbooks, change summary |
| 9. Outcome & learning | Validate success metrics, capture defects/incidents, update docs and technical debt | Outcome note and follow-up items | Product/Tech confirm outcome or next iteration | Summarize observations, propose process improvements |

**Gate rule:** No automatic progression when a critical assumption, ownership question, security concern, or release risk remains unresolved. For low-risk changes, stages can be merged and artifacts kept to a single concise issue; record why.

## Stage 0: intake minimum

Ask before creating a ticket or touching code:
- Who experiences the problem? What happened, and what is the desired outcome?
- Why now? What happens if we do nothing?
- Which behavior is observed today, and how is it measured?
- Which constraints are real (time, compliance, dependencies, budget)?
- Who can confirm acceptance and prioritize the request?

Do not treat the requester's proposed UI/API as a proven solution.

### Intake mini-template

```markdown
## Problem
## Users / stakeholders
## Current vs desired outcome
## Evidence or examples (sanitized)
## Impact and urgency
## Success measure
## Constraints
## Confirmed / Assumed / Unknown
## Requester and decision owner
```

## Cross-cutting controls

- **Security and privacy:** Identify personal data, authorization, auditability and retention requirements early.
- **Reliability:** Consider consistency, concurrency, error handling, recovery, observability, retries and idempotency.
- **Change safety:** Consider existing integrations, backward compatibility, schema migrations, rollout and rollback.
- **Traceability:** Keep links from problem → acceptance criteria → design → tasks → tests → release/outcome.
- **Communication:** Maintain a decision log and make blockers visible early.
- **People:** Assign one accountable owner per deliverable but encourage shared reviews and coaching.
- **Multi-repository:** Use one feature-level source of truth plus per-repository impact, contract and test responsibilities.
- **AI:** Do not provide secrets or confidential company data to public repositories or unapproved tools. Review all AI output; use read-only exploration as a safe starting point.

## Definition of Ready (proposed)

A feature is ready for implementation **when sufficiently true for its risk level**:
- Problem and intended value are stated.
- Acceptance criteria and major edge cases are reviewable.
- Dependencies and affected areas are understood well enough.
- Major design/security/data questions are resolved or explicitly tracked.
- Scope, decision owner, implementation owner and verification approach are agreed.

This is a conversation aid, **not** a bureaucratic block on learning or doing a spike.

## Definition of Done (proposed)

- Agreed acceptance criteria are met or deviations are accepted and documented.
- Appropriate automated/manual test evidence exists; high-risk paths receive deeper verification.
- Code review completed; security/privacy concerns addressed.
- Operational needs (migration, monitoring, rollback, owner) handled where relevant.
- Released or explicitly marked ready-to-release according to team policy.
- Outcome and deferred follow-ups are visible to stakeholders.

## Example (fictional): approval operation history

**Request:** "Show who changed an approval status and when."

Before implementation, validate actor types (admin/system), audit correctness, data access, older records, simultaneous updates, failure consistency and retention.

Possible artifacts:
- Intake: inability to investigate status disputes.
- Requirements: actor/time/old/new values and permission policy; scenarios for automated status changes.
- Exploration: where transitions currently occur, how writes and transactions are handled.
- Design: compare transactional audit table with an existing event/audit mechanism; record choice and trade-offs.
- Plan: storage/migration → API contracts → UI → tests → rollout.
- Verification: success/failure paths, race conditions, audit immutability and access control.
- Outcome: operators can answer status-history questions with reliable evidence.

## Next decisions to resolve

1. Which stages should be mandatory versus combined for tiny changes?
2. What are the exact approval responsibilities in the user's real team?
3. What is the agreed format for Jira intake/spec, design notes, and ADR IDs?
4. Where should feature-level artifacts live when work spans several private repositories?
5. Which first skill in `puen-stack` removes the most repeated work without compromising review quality?

## Suggested first pilot

Pick one **non-sensitive** feature. Record baseline time and pain points; run intake → reviewable requirement → codebase exploration → design → plan → verification; compare clarity, time, review findings and token cost. Update this document only after evidence and agreement.
