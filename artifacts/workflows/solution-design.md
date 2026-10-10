# Solution Design — Artifact Template (EN)

> **Feature-level design decision, not an implementation permit.** Keep concise; Jira/comment is enough for low-risk work. Add a separate [ADR](../../templates/adr.md) only for consequential, long-lived choices.
>
> Workflow: [Solution Design v1.0 (Accepted)](../../workflows/solution-design.md) · [Thai version](./solution-design.th.md)

- **Feature / parent Jira:** <link>
- **Status:** Proposed | In Review | Accepted for Planning | Needs Discovery / Spike | Deferred
- **Design owner / decision maker:** <named authorized human>
- **Requirement / Technical Discovery evidence:** <links>
- **Scope and risk level:** <what is covered; why low/moderate/high>

## 1. Problem, outcomes and verified constraints

- **Goal / observable acceptance:** <source-backed outcome>
- **Confirmed constraints:** <business, API, data, security, cost, operability; sources>
- **Assumptions needing evidence:** <explicit Unknowns; no invented NFR targets>
- **Non-goals / excluded systems:** <limits to design authority>

## 2. Decision drivers and alternatives

| Option | Benefits | Costs / failure risks | Evidence or assumptions |
| --- | --- | --- | --- |
| <A> | <value> | <risk> | <source> |
| <B, or reason comparison is unnecessary> | <value> | <risk> | <source> |

**Recommended option and why:** <trade-offs tied to drivers, not AI preference>

## 3. Proposed solution and contracts

- **To-be flow:** <actor/trigger → ownership boundary → data state/side effects → response>
- **Relevant API/event/data contracts:** <actual endpoints, schema, version expectations; link only if known>
- **Domain invariants and permissions:** <what must always hold / authorized actor / privacy>
- **Failures, retries, concurrency and consistency:** <important scenarios, no invented outcomes>
- **Backward/forward compatibility and migration:** <mixed-version expectation; safe sequencing or Unknown>
- **Diagram / ADR link (optional):** <only where useful>

## 4. Verification, release and operations implications

- **Acceptance and negative/boundary cases:** <independent observable oracle>
- **Test and integration seams / owner:** <BE/BFF/FE etc; N/A with reason>
- **Migration/rollout/feature flag/observability/recovery:** <identified concerns and responsible owner>

## 5. Risks, open decisions and follow-up evidence

| Risk / question | Impact | Mitigation, spike or evidence | Owner / checkpoint |
| --- | --- | --- | --- |
| <question> | <impact> | <next action> | <owner> |

## 6. Human Solution Design Gate (Feature-level)

- **Outcome:** Accepted for Planning | Needs more discovery/spike | Rework | Deferred
- **Authorized decision maker, date, approval evidence:** <real person/time/link or Pending>
- **Chosen option / rationale / recorded dissent:** <decision>
- **Accepted boundaries and exceptions:** <what the gate actually covers>
- **Handoff to Delivery Planning:** <initial testable Work Item, dependencies, contracts, verification/release obligations>

**Boundary:** This **Feature-level** decision does not approve each Work Item's code or bypass Delivery Planning, Implementation, Verification or Release gates. Material changes reopen affected upstream decisions only.
