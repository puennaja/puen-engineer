# My Engineer — Universal Engineering System

- **Status:** Proposed architecture v0.1 (direction accepted; implementation details under review)
- **Date:** 2026-10-09
- **Decision:** [ADR-0001](../decisions/0001-universal-engineering-system.md)
- **Applies to:** Solo engineering, delivery teams, and product organizations
- **Security:** This repository is public; examples must be synthetic.

## 1. Purpose and boundaries

My Engineer is a scalable **engineering operating system**, not a mandatory methodology, autonomous coding bot, or replacement for product leadership.

**Outcome:** turn uncertain needs into software and products with measurable value, repeatable quality, sustainable operations, and increasing team capability.

A solo developer should be able to use the minimum viable practices. A team should add explicit ownership and coordination. A product organization should additionally manage portfolio, people, finance, reliability and governance.

**Non-goals for v0.1**
- Mandating Scrum or a specific sprint length.
- Requiring every activity or document for every change.
- Building an orchestration platform or MCP server before demonstrating value.
- Automating human approvals or placing confidential company information in public repositories.

## 2. Architecture: four independent, composable layers

| Layer | Core questions | Includes | Outputs |
| --- | --- | --- | --- |
| **Engineering practices** | What must we learn, design, build, verify and operate? | Discovery, requirements, technical exploration, design, implementation, validation, delivery, operations, learning | Evidence and engineering artifacts |
| **Delivery operating model** | How do we organize work and receive feedback? | Agile principles, vertical slices, flow, Scrum or Kanban chosen by team, WIP, review and retrospectives | Prioritized work, working increments, flow measures |
| **Product & organization** | Why build, who decides, who does the work and how is it sustained? | Product discovery, outcomes, roadmaps, roles/ownership, staffing, coaching, budget, dependencies, risk, governance | Strategy, team topology, accountable decisions, investment priorities |
| **AI & automation** | Which repetitive tasks can be assisted safely? | On-demand skills, templates, code exploration, planning, verification, integration | Reviewed suggestions and automation evidence |

All layers use the same common primitives:
1. **Work item** — problem, goal, priority, affected users.
2. **Decision** — owner, context, options, rationale, status and date.
3. **Artifact** — minimal durable evidence and links.
4. **Quality evidence** — acceptance criteria, tests, risk review and operational checks.
5. **Feedback** — user outcome, defects, throughput and team learning.

## 3. Recursive learning and delivery loop

There is no one-way pipeline. For each manageable increment:

`Frame the problem → Explore & validate → Design enough → Build → Verify → Deliver/observe → Learn → Reprioritize`

- Iterate or revisit earlier activities as new evidence emerges.
- Plan at different horizons: immediate task, near-term iteration, product roadmap.
- Define **working increment** by usable, testable value (or explicit learning from a spike), not by repository boundaries.
- Continuous integration and testing happen throughout, not only after building.
- Deploy when ready under the release policy, not necessarily on the final day of a sprint.
- Prefer early cross-functional review when a decision is expensive to reverse.
- Documentation grows with uncertainty, risk and number of collaborating people.

## 4. Scale profiles: same practices, different formality

These are configuration profiles, **not maturity rankings**.

| Dimension | Solo / Personal | Team / Feature Delivery | Product / Organization |
| --- | --- | --- | --- |
| Planning | Small ranked queue or lightweight board | Shared prioritized backlog and capacity discussion | Strategy, outcome roadmap, portfolio and investment trade-offs |
| Scope | Small problem statement and acceptance examples | Stakeholder-confirmed acceptance, dependencies and scope | Product discovery, research, market/usage signals and business outcomes |
| Design | Lightweight notes for meaningful trade-offs | Design review for cross-cutting or risky changes | Architecture governance on irreversible or cross-team decisions |
| Ownership | One explicit implementer/decision maker | Product and engineering ownership, service owners | Product lead, Tech Lead, EM, designated operations owners and escalation |
| Delivery | Short iterations and automated checks | Scrum/Kanban/hybrid; manageable vertical slices | Cross-team dependency management, releases and value-stream metrics |
| Quality | Relevant tests, review (peer or disciplined self-review), recovery plan | Peer review, QA strategy, observability and incident response | SLOs, security/compliance, reliability investment and audits where appropriate |
| People | Personal learning and focus | Delegation, mentoring, conflict resolution | Staffing, career support, performance processes and succession |
| Documentation | One concise work item may be enough | Linked spec/design/decisions for nontrivial features | Durable service ownership, operational and cross-team records |

**Upgrade triggers**: multiple contributors, shared dependencies, high data/security impact, external customers, stringent reliability, long-lived services or repeated incidents. **Downgrade** ceremonies and artifacts when they cost more than the risk they mitigate.

## 5. Risk-based quality and decision gates

Use a practical classification before implementing:

- **Low:** localized reversible change, limited blast radius, no sensitive data or breaking contract. Use compact acceptance + tests + review.
- **Moderate:** multi-component changes, schema/API impact or uncertain behavior. Add explicit impact analysis, design and integration verification.
- **High:** payments, auth, personal data, destructive migrations, critical availability or irreversible changes. Require accountable design/security review, rollout/rollback and deeper verification under local policy.

A gate means **evidence sufficient for the risk** and a named decision owner, not a compulsory meeting or sign-off from the Tech Lead.

Minimum checkpoints:
1. **Problem understood** — outcome and requester/decision owner identified.
2. **Risk and approach understood** — critical assumptions/dependencies surfaced.
3. **Change verified** — acceptance and appropriate tests reviewed.
4. **Safe to release** — operational and release owner approves where applicable.
5. **Outcome assessed** — measure benefit and record follow-ups.

## 6. Decisions, roles and people

Ownership is contextual and may be combined in a small team:
- **Product/Business:** problem priority, outcomes, scope and acceptance.
- **Tech Lead/Engineering:** technical approach, trade-offs, code quality and technical coordination.
- **Engineers:** implementation decisions within agreed boundaries; testing and maintenance.
- **QA/Quality:** test strategy and evidence; raises quality concerns.
- **Engineering Manager:** people, hiring/staffing, career development and capacity in organizations with this role.
- **Service/Release Owner:** production readiness, rollback, on-call and incident ownership.

For contentious issues, explicitly name **decider, contributors, informed parties and escalation path**. Do not assign one person unilateral ownership of every product, people and architecture decision.

People management grows from self-management → collaboration/coaching/delegation → staffing, organization design, capacity and performance support. Build feedback loops and team safety; do not use story points or PR counts to rank engineers.

## 7. Agile and delivery model

**Default principles:** customer collaboration, small batches, frequent feedback, transparency, sustainable pace, adaptation.

Scrum, Kanban or hybrid are **adapters** chosen to fit constraints; no framework is a dependency of core engineering activities. Maintain visibility of work-in-progress, blocked work, lead/cycle time and outcomes without turning metrics into individual scorecards.

Product roadmap and engineering lifecycle are not sprint artifacts. A sprint is a planning/inspection cadence, not the unit of architectural validity or release safety.

## 8. Repository architecture

### puen-engineer: policy, context and reasoning
- `architecture/`: system model and diagrams (this document)
- `workflows/`: feature, product and team operating workflows
- `decisions/`: dated ADRs and explicit status
- `principles/`: technical/leadership principles
- `templates/`: risk-based intake, review, ADR, outcome notes
- `artifacts/`: **sanitized** durable outputs

### puen-stack: reusable execution helpers
- `skills/`: small, discoverable `SKILL.md` units called only when applicable
- `agents/`: optional roles only when their separation adds value
- `scripts/`: audited automation
- `templates/`: portable execution/output formats
- `AGENTS.md`: concise repository-level instructions, not a giant knowledge dump

**Integration contract:** a skill references the activity it supports; states input, expected output, verification, boundaries, and when to ask a person. Providers (Claude Code, Codex, later others) should share concepts, not be assumed to share identical skill-loading behavior.

**Separation:** `puen-engineer` describes **what/why/ownership**. `puen-stack` implements reusable **how-to assistance**. Actual project code and confidential documents stay in authorized project environments.

## 9. Build order: prove the smallest useful slice

1. **Foundation (current):** accept universal direction and propose architecture; distinguish engineering lifecycle from delivery model.
2. **Pilot workflow:** one intake template → acceptance cases → codebase/impact exploration → compact design → delivery/checks/outcome, using a synthetic or permitted project.
3. **First skill:** identify repeated manual steps observed during the pilot; implement one small provider-neutral skill with tests and human checkpoints.
4. **Team adapter:** explicit delegation, owner/decision model, backlog/WIP and cross-repo work plan.
5. **Product adapter:** discovery/outcomes, roadmap/finance/risk, ownership, operations, people and coordination.
6. **Automation:** integrate Jira/Git provider or orchestration only where it removes measured toil and permissions allow.

**Pilot evidence:** time from request to reviewable design, missed requirement/edge cases, review/test findings, rework, user outcome, and AI usage cost. Compare to an appropriate baseline; no unverified productivity claims.

## 10. Unresolved architecture choices

- How many concrete templates are needed before a small task feels bureaucratic?
- How does an AI skill receive public policy versus private project context?
- Which gates should be hard stops for high-risk changes?
- How should the system handle shared cross-repository contracts and versioning?
- What is the smallest team/product adapter that adds real value?

**Next document:** `workflows/requirement-discovery.md` after reviewing this architecture. No large skill catalog or platform yet.
