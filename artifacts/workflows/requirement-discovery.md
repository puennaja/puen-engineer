# Requirement Discovery — Artifact Template (EN)

> **Template, not evidence or approval.** Copy into the existing Jira issue/product record or a Markdown file only when useful. A small, low-risk request may use a short summary. Never fabricate user quotes, evidence or success targets.
>
> Workflow: [Requirement Discovery v1.0 (Accepted)](../../workflows/requirement-discovery.md) · [Thai version](./requirement-discovery.th.md)

- **Subject / link:** <feature, problem or Jira URL>
- **Status:** Draft | Validated for exploration | More discovery required | Deferred | Declined
- **Requester / decision owner:** <role or person, TBD if unknown>
- **Prepared by / date:** <owner / YYYY-MM-DD>
- **Source evidence:** <sanitized references, interviews or observations>

## 1. Problem and affected users

**Problem statement:** <who experiences what problem, in which context, and why it matters>

**Affected users / stakeholders:** <who is impacted; do not guess>

**Frequency / severity / workaround / cost of inaction:** <facts, estimates with sources, or Unknown>

## 2. Current state and evidence

| Claim or observation | Source / link / date | Confidence: Confirmed / Assumed / Unknown | What would validate it? |
| --- | --- | --- | --- |
| <existing behavior or pain> | <source> | <status> | <next step or N/A> |

**Conflicts or disagreements:** <include competing accounts; do not silently reconcile>

## 3. Desired outcome

- **Observable user / business change:** <what should become possible or improve>
- **Candidate success signal:** <measurable or observable indicator; numeric target only if sourced>
- **Non-goals:** <what is explicitly not being solved>

## 4. Candidate needs, boundaries and constraints

- **Needs / behaviors:** <confirmed requirement vs hypothesis, labelled individually>
- **In scope:** <candidate area>
- **Out of scope:** <explicitly excluded area>
- **To be tested:** <open hypothesis>
- **Constraints:** <privacy, security, legal, data, integration, timing, accessibility or N/A>

## 5. Open questions and risks

| Question / risk | Impact if wrong | Evidence or experiment needed | Owner / checkpoint |
| --- | --- | --- | --- |
| <question> | <impact> | <validation> | <owner/date> |

## 6. Human decision and next step

- **Decision:** Explore technically | Prototype / experiment | Discover more | Defer / decline
- **Decider / date / source:** <explicit human record, or Pending>
- **Agreed facts vs assumptions:** <summary and source>
- **Next action and owner:** <bounded technical question, experiment or follow-up>
- **Handoff to Technical Discovery:** <problem, outcome, critical unknowns and links>

**Boundary:** `Validated for exploration` is **not** approval of a design, delivery plan, implementation, MR or deployment. This is one logical discovery summary, **not** a mandatory extra file.
