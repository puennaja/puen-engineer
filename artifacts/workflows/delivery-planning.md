# Delivery Plan / Shared Work View — Artifact Template (EN)

> **Feature-level planning artifact, not a duplicate issue backlog.** Prefer existing Jira Feature/Story with linked Work Items. Default: **Hybrid — Full Scope, Progressive Detail**; plan enough feature-wide risks now, refine later tasks before work begins.
>
> Workflow: [Delivery Planning v1.0 (Accepted)](../../workflows/delivery-planning.md) · [Thai version](./delivery-planning.th.md)

- **Feature / parent Jira:** <link>
- **Status:** Proposed | Agreed for first Work Item | Active | Replanning | Complete | Blocked
- **Sources:** <Requirement, Technical Discovery, Accepted Solution Design links>
- **Product / scope decision owner:** <human>
- **Engineering delivery owner:** <human>
- **Date / revision:** <YYYY-MM-DD / revision>
- **Planning depth:** Full Scope + Progressive Detail (default) | Exception: <reason, approver>

## 1. Whole-feature map — understand before slicing

- **Outcome and acceptance boundary:** <what must eventually work, including excluded scope>
- **Known affected systems / owners:** <BE/BFF/FE/API/data/external parties as applicable>
- **Feature-wide dependencies and contracts:** <provider/consumer/data, owner, actual evidence link>
- **Critical unknowns and integration / security / migration / release risks:** <what could invalidate delivery; owner>
- **Future deliverables:** <high-level outline only, assumptions not implementation commitments>
- **Shared integration / Story acceptance owner:** <human or Pending; Work Item passing is not full Story acceptance>

## 2. Next Work Item (progressive detail)

- **Existing Jira Story / Sub-task / Bug / Task:** <link, not a new issue type>
- **Assigned engineering owner and authorized repo(s):** <who and where>
- **Goal / observable Work Item acceptance:** <derived from confirmed parent acceptance + approved design>
- **Included changes / exclusions:** <boundaries, no unassigned FE/BE/BFF scope>
- **Inputs and dependencies:** <contract versions, upstream deliverables, external decisions>
- **Required checks / independent verification / reviewer:** <what proves completion and who owns it>
- **Risk / complexity / feasibility:** <reason for depth, spike if warranted>
- **Ready-to-start status:** Ready | Spike first | Blocked | Replan; <evidence / owner>

## 3. Remaining Work Items (outlines; detail just-in-time)

| Work Item / proposed slice | Outcome / acceptance idea | Owner (confirmed/TBD) | Dependencies | Refinement trigger / checkpoint |
| --- | --- | --- | --- | --- |
| <Jira link / future slice> | <goal> | <owner> | <dependency> | <when to elaborate> |

## 4. Integration, QA and release coordination

- **Shared contracts / integration checkpoints:** <expected producer/consumer versions and owner>
- **Cross-repo test / Story-level evidence owner:** <how integration is verified; Not run if unavailable>
- **Migration / deployment / recovery sequencing:** <actual risk-informed plan or Unknown; do not invent order>
- **Observability / security / non-feature work:** <work needed for safe delivery, not invisible tail work>

## 5. Delivery risks, trade-offs and forecast (optional)

| Risk / blocker / assumption | Impact | Mitigation or next evidence | Owner / revisit trigger |
| --- | --- | --- | --- |
| <risk> | <impact> | <action> | <owner> |

**Estimate / forecast (only if relevant):** <method, confidence/range, capacity assumptions, date; never invent precision>

**Capacity / scope trade-offs:** <what a responsible human actually decided>

## 6. Human Delivery Planning Gate (Feature-level)

- **Decision:** Agreed for first Work Item | Spike first | Return to Discovery/Design | Defer/De-scope
- **Human decision maker / date / evidence:** <actual approval or Pending>
- **Feature-wide commitments and material unknowns accepted:** <explicit scope/risk>
- **Approved next Work Item / owner:** <Jira link and boundary>
- **Next refinement checkpoint / trigger:** <when details are needed>
- **Changes since prior plan:** <decision rationale, affected Design/Planning gate if material>

**Boundary:** This Gate agrees a credible **feature-wide plan plus next bounded Work Item**, not every future detailed ticket. Implementation and Verification still require separate **per-Work-Item Human Gates**; MR/production privileges are separate.
