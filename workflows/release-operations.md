# Release & Operations Workflow — v1.0 Design Draft

- **Status:** Design Draft — **not Accepted**; Q1, Q2 and Q3 agreed; Q4 pending
- **Date:** 2026-10-10
- **Track:** A — My Engineer, vendor/tool-neutral engineering practices
- **Upstream:** [Implementation (Accepted)](./implementation.md), [Verification (Accepted)](./verification.md), [Feature Delivery Lifecycle](./feature-delivery-lifecycle.md)
- **Thai companion:** [Release & Operations (TH)](./release-operations.th.md)
- **Design principle:** Work Item verification, merge, deployment, feature enablement and business outcome are distinct events and decisions.

## Q1 — Fixed Human Release Gate (agreed 2026-10-10)

**Owner decision: A — Fixed Human Release Gate for Release & Operations v1.0.**

Every **production release** must receive **explicit approval by an authorized human release decision owner** before execution. After approval, an authorized, policy-compliant deployment pipeline can automate the approved release operations **within its specifically approved scope**. This is **release-level approval**, separate from Feature-level Design/Planning gates, per-Work-Item Implementation/Verification gates, and human-controlled MR Ready/Merge decisions.

**Boundaries:**
- A Work Item marked `Verified for Release Review` is **not** authorization to merge, deploy or enable user-facing changes. One Release may contain multiple Work Items/repos.
- AI may **prepare, inspect and recommend** release readiness, evidence, compatibility, rollout, monitoring and recovery plans where access is authorized. AI may not infer approval from green CI or exercise production privileges without separate authorization.
- An approved pipeline is not the same thing as unrestricted agent action. Actual production credentials, deployment actions, release approver identity, protected-environment permissions, incident ownership and company policies remain project-specific and are **not granted by approving this design decision**.
- This decision does **not** require a human to click every pipeline step. It requires explicit human authorization before each production release, with automation allowed inside the approved bounds.
- Deployment, feature flag enablement and other changes to live user exposure are distinct operations; the **bounded approval envelope agreed in Q2** must state which of them is authorized.
- No deployment, production access, repo policy change, release automation or new AI Skill is authorized by this draft.

## Candidate responsibilities (not yet approved as a workflow)

1. **Release candidate and readiness:** identify immutable build/artifact, included changes, dependencies, CI evidence, migration and rollback/roll-forward limitations.
2. **Coordination and Go/No-Go:** identify release owner, affected services/environments, rollout plan, risk/stop criteria, communications and authorization.
3. **Controlled deployment and validation:** execute only through authorized pipeline/people, validate deployment completion and critical behavior with safe observability.
4. **Operations, recovery and learning:** monitor agreed signals, identify regressions, mitigate/rollback/roll-forward as safe, involve incident owners and record actual outcomes.

**Adaptive depth:** Low-risk and reversible releases should remain lightweight; high-risk, multi-service, data-changing or hard-to-reverse releases require stronger compatibility, recovery, monitoring and qualified human ownership. These are proposed practices, **not additional auto-approved gates**.

## Q2 — Bounded Release Authorization (agreed 2026-10-10)

**Owner decision: A — Bounded Release Authorization for Release & Operations v1.0.** A human authorization applies only to a **named, bounded Production Release**, never a blanket release window or an unrestricted selection of future builds.

**Minimum approval envelope, scaled to actual impact:**
- **Exact subject:** immutable artifact digest/version(s) or a pinned release manifest, originating build/commit, included repositories/services and relevant dependencies.
- **Destination and exposure:** authorized production environment/cluster/region/tenant as relevant, services, deployment strategy and user/traffic/feature-flag exposure scope. If enabling a flag is **not explicitly included**, deployment approval does not authorize enabling it.
- **Operational bounds:** authorized rollout steps, release timing/validity if relevant, required validation signals, pause/abort criteria, recovery/rollback or roll-forward owner and plan, and identified human release approver.
- **Traceability:** a linked release record in existing release ticket/pipeline tooling stating **who approved what, where, when and under which bounds**, not a compulsory new Markdown artifact.

**Automation inside the envelope:** a policy-compliant pipeline may execute ordinary approved stages, continue rollouts and perform retry actions **only when those actions are explicitly permitted, safe, do not expand scope/risk and preserve the same pinned release target**. Failed/repeated runs must not implicitly bypass stop conditions; risky/destructive operations and incident recovery follow their separately authorized processes.

**Approval invalidation / stop:** replace the artifact or manifest, change target service/environment or user exposure, expand rollout, introduce a new migration/contract risk, exceed validity or change other material release assumptions → **stop the affected release action and obtain renewed explicit human authorization**. Harmless reruns of approved checks do not automatically require new approval. Do not infer permission from CI-green, prior work-item verification, MR merge or an earlier release authorization.

**Boundary:** Q1–Q3 approve **policy design principles**, not real credentials, deployment rights, environment access or an actual production release. The overall workflow remains a Draft.

## Q3 — Risk-adaptive Release Readiness (agreed 2026-10-10)

**Owner decision: A — Risk-adaptive Release Readiness with non-waivable safety boundaries for Release & Operations v1.0.** Every Production Release must present credible minimum readiness evidence **before** the Q1 Human Release Gate; the depth of additional checks depends on its actual risk, not the number of files changed or the size of the team.

**Minimum readiness contract — all releases, in a proportionate record:**

| Required dimension | Minimum answer/evidence before the human Go / No-Go decision |
| --- | --- |
| **Release identity & bounds** | Which pinned artifact/manifest, source/build, services, approved production target and exposure scope are being proposed? Match Q2 exactly. |
| **Quality evidence** | What relevant per-Work-Item Verification results, applicable CI/build/test checks, critical findings and unresolved gaps apply to **this combination of changes**? Green status alone is not enough. |
| **Compatibility & dependencies** | What changed across services, API/data contracts, configuration and schema? Who owns any required cross-repo integration or migration ordering? Explicitly mark genuinely not-applicable concerns. |
| **Recovery readiness** | Can this release be stopped, mitigated, rolled back or safely rolled forward? What could be irreversible (notably data migration)? Identify a feasible action, trigger and owner. |
| **Execution & ownership** | Who is authorized to approve and execute, and which safe pipeline/environment permissions, rollout conditions and downstream effects are involved? |
| **Validation & observation** | What immediate smoke/critical-behavior checks, health signals, monitoring/observation window or equivalent evidence, and pause/abort criteria will be used? |
| **Open risks & decision** | Which checks are **Pass / Fail / Not run / Blocked / Inconclusive**? Which are policy-mandated or critical? State residual risks, owners, recommendation and a **human Go / No-Go decision**. |

**Risk-adaptive depth:** 
- **Low risk / reversible:** concise linked ticket/pipeline evidence, focused checks and an explicit recovery/validation owner; do not mandate a separate heavyweight checklist.
- **Moderate / multi-service:** stronger producer–consumer compatibility, configuration, integration and realistic recovery/operational validation.
- **High risk / authorization, sensitive data, money, schema migration, destructive or hard-to-reverse changes:** qualified human/domain review and targeted security/data integrity/migration, failure-mode, dependency and recovery proofs required by the actual risk and local policy. Small diffs can be high risk.

**Hard stop:** A missing **mandatory check**, unverified critical acceptance/contract, unresolved blocking defect, unsafe environment/authorization, or unbounded critical release risk is **No-Go / Blocked**. An authorized human may accept **only a policy-permitted equivalent form of proof or an explicitly permitted exception**, with rationale, limitations and accountable risk owner; mandatory policy cannot be waived by AI, and a check that did not run is **never recorded as Pass**. If no valid alternative exists, do not release. The release owner evaluates the whole proposed release, not merely individual Work Items.

**Recording:** Reuse the existing release ticket/pipeline, CI links and Work Item/MR evidence. Do not create a universal new Markdown report or assert release approval on the basis of this design agreement. Verification Gate success, merge and release are distinct events.

## Q4 — Proposed next decision (not yet agreed)

**What should happen after the authorized deployment pipeline reports “success,” before the release is considered complete?**

- **A. Risk-adaptive production validation and observation (recommended):** evaluate deployment completion **separately** from service health and changed business behavior. Run only policy-authorized safe post-deploy checks, inspect agreed metrics/logs/traces and dependency health, observe for a risk-appropriate period or signal threshold, and use predefined pause/abort/escalation criteria. Record **Healthy / Degraded / Inconclusive / Recovery in progress**, human owner and residual risks. Never infer success from green pipeline alone.
- **B. Pipeline-success closure by default:** close the release when the pipeline completes, with health and behavior checks as optional follow-up.

**Still pending after Q4:** recovery/incident decision authority and learning-loop details, followed by full v1.0 review and separate owner acceptance.
