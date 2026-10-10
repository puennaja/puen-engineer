# Release & Operations Workflow — v1.0 Design Draft

- **Status:** Design Draft — **not Accepted**; Q1–Q5 agreed; Q6 pending
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

**Automation inside the envelope:** an authorized pipeline may execute ordinary approved stages and routine **non-intervention** retries only within its pinned artifact, environment and scope. Under Q5, if an anomaly requires an intentional response—pause, abort, recovery retry, rollback, roll forward or restart—the system must **alert and wait for fresh human confirmation of that particular action**. Never use retries to bypass stop conditions. No autonomous recovery or destructive action is authorized.

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

## Q4 — Risk-adaptive Production Validation & Observation (agreed 2026-10-11)

**Owner decision: A — Risk-adaptive Production Validation & Observation for Release & Operations v1.0.** **Deployment completed** is only a pipeline/execution event, not proof of **service health**, **changed business behavior**, or **successful release outcome**. After authorized deployment, assess the released version on safe, authorized production observation surfaces before recommending closure.

**Minimum post-deployment contract, risk-adaptive in depth:**
1. **Confirm what actually deployed:** pipeline/deployment outcome, immutable artifact/version, target environment/region/services and enabled traffic/feature exposure **versus the Q2 approved envelope**. Report partial rollouts, drift and unexecuted stages.
2. **Check operational health:** service readiness/availability, relevant error rate and latency signals, saturation where relevant, application logs/traces, downstream dependency and queue/job health. Compare against useful pre-release baseline or explicitly agreed thresholds; never invent baselines or declare zero observed errors when data is unavailable.
3. **Check changed behavior:** run **only policy-authorized, non-destructive** smoke/critical-flow checks that match acceptance, and include relevant negative/permission/data-integrity signals. If a live check could write real data, charge money, publish messages or affect users, follow the separate approved safe procedure or use an authorized synthetic/isolated alternative; missing evidence remains visible.
4. **Observe and decide:** choose a **risk-appropriate observation period or objective signal threshold** defined for the release, with named observer/on-call owner, accessible evidence and agreed stop/abort/escalation triggers. Short focused observation may suffice for low risk; high risk needs deeper signals/progressive exposure and potentially longer monitoring. Some effects manifest asynchronously; unknown long-tail effects remain owned follow-ups, not invented success.
5. **Record distinct outcomes:** deployment state (**completed / partial / failed / unknown**), service state (**healthy / degraded / inconclusive**), behavioral evidence (**pass / fail / not run / inconclusive**), and overall release disposition (**Healthy / Degraded / Inconclusive / Recovery in progress**), including owner, timestamps, observed evidence and remaining risk.

**Guardrails and escalation:** A breached critical threshold, material drift from the release envelope, harmful user/data impact, or inconclusive mandatory evidence triggers **detection, immediate alert and escalation to an authorized human for a pause/abort/recovery decision under Q5**. Even a protective pause requires fresh human confirmation under the chosen Q5 policy. Never stamp the release Healthy merely from pipeline, dashboard or pod status; no autonomous AI intervention privilege is granted.

**Closure boundary:** A human release/outcome owner (or authorized release process under an explicit human decision) considers the collected evidence and residual risks. Mark a release Healthy only when its risk-relevant health/behavior criteria are actually met; if mandatory evidence is unavailable, record **Inconclusive/Blocked**, not success. Reuse the existing release/pipeline/incident record; no mandatory standalone report and no new per-test Human Gate.

## Q5 — Human Confirmation Before Every Intervention (agreed 2026-10-11)

**Owner decision: B — Human Confirmation Before Every Intervention for Release & Operations v1.0.** Monitoring, AI and pipelines may **detect, alert, document and recommend**, but **each intentional operational intervention in response to an anomaly** requires **fresh, action-specific confirmation from an authorized human incident/release owner before execution**. This includes **Pause, Abort, Rollback, Roll Forward, restart, remedial retry, feature-flag exposure changes and recovery**, even when the proposed action appears protective or low risk.

**Intervention contract:**

1. **Detect and escalate:** notify the reachable human owner through the defined incident channel, attaching observed symptoms, affected release/version, blast radius, deployment state, logs/metrics, and proposed options. Detection and paging may be automatic; recommendation is not execution permission.
2. **Authorize each intervention:** the human reviews target/scope, data and schema safety, dependencies, expected side effects, reversibility and policy. Confirm the **specific action**. Approval for one action never authorizes another intervention by implication.
3. **Execute only with actual operational permission:** the designated human or policy-authorized pipeline performs only the confirmed action. Material changes to the Q2 release envelope also require the applicable new release authorization. An incident does not grant AI production access.
4. **Revalidate and record:** capture actual intervention execution, service/behavior checks and outcome (**Recovered / Degraded / Inconclusive / Still mitigating**) with human decision owner, executor, timestamp and continuing observer; successful rollback commands do not prove service recovery.
5. **If the human is unavailable:** continue alerts and escalation under the actual incident policy, but **do not automatically execute an intervention or invent approval**. Q3 readiness should include reachable incident ownership, escalation route and realistic response expectations, especially for high-risk releases.

**Reconcile with Q1–Q4:** The fixed human Release Gate and bounded envelope still allow **ordinary planned pipeline stages** to execute without a human click at each stage. Q5 controls **new, responsive operational interventions**; routine execution is not the same as a new incident response. A failed command or platform-managed shutdown is observed as a failure, not permission to initiate an autonomous recovery. **No new auto-pause or auto-rollback incident logic is approved by this design.** More stringent platform, safety and legal requirements remain authoritative; the generic draft does not reconfigure or bypass existing independent fail-safe mechanisms.

**Trade-off acknowledged:** Requiring human confirmation for even protective pauses can increase response time and customer/data impact while waiting. Compensate with actionable alerts, staffed escalation and credible safe recovery procedures. This is a deliberate human-control choice, **not a claim that it is always the fastest or safest emergency response model**.

## Q6 — Proposed next decision (not yet agreed)

**How should a release be closed and how should operational findings flow back into engineering work?**

- **A. Lightweight Outcome & Learning Loop (recommended):** after Q4 validation and any Q5 incident, the human release/outcome owner records **actual health/behavior outcome, supporting evidence, residual risks, follow-up owner and any later customer/business outcome check** in existing release/Jira/incident records. Create new Work Items only for actionable defects or improvements. Use a focused incident review when impact warrants it, without mandating a ceremony or new Markdown report for every release.
- **B. Close at Technical Health:** record operational health and outstanding critical incidents, while leaving business outcome tracking and release learning to separate processes.

**After Q6:** reconcile the complete lifecycle with the Accepted upstream workflows, review the v1.0 candidate and seek explicit separate owner approval. This Draft does not grant deployment, production or AI automation rights.
