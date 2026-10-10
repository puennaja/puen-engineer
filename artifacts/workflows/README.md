# My Engineer — Workflow Artifact Templates (EN)

**Status:** Reusable supporting templates for the seven **Accepted v1.0** workflows. **These templates are not human approvals, execution logs or new mandatory files.** The workflow documents remain normative; always follow stricter local repo/company rules.

**Thai index:** [README.th.md](./README.th.md) · **Artifact home:** [../README.md](../README.md)

## How to use (Jira/MR first)

1. **Identify the unit:** Requirement/Technical Discovery, Solution Design and Delivery Planning generally operate at **Feature/Story level**. Implementation and Verification operate on the **assigned Jira Work Item** (Story, Sub-task, Bug or Task). Release & Operations operates on a **bounded Release bundle**, which may include several Work Items/repos.
2. Read only the **relevant Accepted workflow** and **its matching artifact**. Do not load every workflow into the AI's context for a single sub-task.
3. **Copy only useful sections** into existing Jira, MR or release/incident records; a small reversible change can use a short summary. A separate Markdown file is optional, not a duplicate source of truth.
4. Fill placeholders with **actual source links, confirmed people, commands, environments and observed outcomes**. Explicitly mark `Confirmed / Inferred / Unknown`, `Not run / Blocked / Inconclusive`, `Pending` and `N/A with reason` as applicable. **Never invent proof or claim an unexecuted check passed.**
5. Keep source-backed acceptance independent of generated code. Preserve existing working-tree/user changes; use only authorized repo/environment access. Human gates and MR/release permissions are **separate** from preparing an artifact.
6. When changes materially invalidate a previously approved Feature Design/Plan or Work Item scope, reopen only the **affected human decision**, keeping traceability in the existing record.

## Artifact map

| Stage / accepted workflow | Suggested unit and primary record | English template | Thai template | Key human decision / boundary |
| --- | --- | --- | --- | --- |
| [Requirement Discovery](../../workflows/requirement-discovery.md) | Feature/need; **Discovery Summary** | [EN](./requirement-discovery.md) | [TH](./requirement-discovery.th.md) | Validated **for exploration**, not implementation approval |
| [Technical Discovery](../../workflows/technical-discovery.md) | Feature/technical question; **Technical Discovery Summary** | [EN](./technical-discovery.md) | [TH](./technical-discovery.th.md) | Evidence sufficient for design / targeted spike |
| [Solution Design](../../workflows/solution-design.md) | Feature; **Solution Design Summary** | [EN](./solution-design.md) | [TH](./solution-design.th.md) | **Feature-level Human Solution Design Gate** |
| [Delivery Planning](../../workflows/delivery-planning.md) | Feature + next Work Item; **Delivery Plan / Shared Work View** | [EN](./delivery-planning.md) | [TH](./delivery-planning.th.md) | **Feature-level Human Planning Gate** (full scope, progressive detail) |
| [Implementation](../../workflows/implementation.md) | **Assigned Work Item**; developer-check evidence and code handoff in Jira/MR | [EN](./implementation.md) | [TH](./implementation.th.md) | **Per-Work-Item Human Implementation Gate**, scoped publish and **human approval before each Draft MR** |
| [Verification](../../workflows/verification.md) | **Assigned Work Item**; independent review/evidence in Jira/MR | [EN](./verification.md) | [TH](./verification.th.md) | **Per-Work-Item Human Verification Gate** after developer-green and Implementation Gate |
| [Release & Operations](../../workflows/release-operations.md) | **Release bundle**; bounded release/pipeline/incident record | [EN](./release-operations.md) | [TH](./release-operations.th.md) | **Human Release Go/No-Go for each bounded Production Release; fresh Human confirmation for every anomaly-driven operational intervention** |

**What is not included:** The [Feature Delivery Lifecycle](../../workflows/feature-delivery-lifecycle.md) is the overarching Accepted foundation, not an eighth work-item artifact. The broad [AI-assisted Feature Delivery decision record](../../workflows/ai-assisted-feature-delivery.md) is still Draft. A durable [ADR](../../templates/adr.md) is optional for consequential architecture decisions, not another automatic stage.

## Quick guidance for Codex / Claude Code

Use this as a **starting request** for a permitted, scoped engineering task, not a blanket command to change repositories:

```text
Follow the Accepted My Engineer workflow for my currently assigned Jira Work Item.
1. Identify which workflow responsibility applies and read only its workflow document
   and artifacts/workflows/<matching-template>.md (or .th.md for Thai).
2. Ground any acceptance, existing behavior, risks and proof in the actual Jira/source
   evidence and repo scope available. Clearly mark unknowns and inaccessible repos.
3. Prepare or update the smallest useful artifact IN the existing Jira/MR record
   where possible; do not invent evidence, create duplicate files or change scope.
4. Never claim tests ran without outputs, never treat template placeholders as real
   findings, and never claim a Human Gate was approved unless explicitly recorded.
5. Stop and request the appropriate human approval for the specified Gate,
   each Draft MR, publishing, production release and each anomaly-driven
   operational intervention. Do not auto-merge or deploy.
Report the updated evidence, open decisions, next safe action and owner.
```

**Privacy:** This knowledge repository is public. Keep templates generic. Never commit real company/customer information, passwords, internal URLs, production logs or confidential Jira details here. Project-specific filled records belong in their authorized private systems.
