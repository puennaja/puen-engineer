# Release & Operations Workflow — v1.0 Design Draft

- **Status:** Review Ready / Release Candidate — **not Accepted**; Q1–Q6 agreed; final cross-document review completed 2026-10-11; explicit owner approval pending
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

## Q6 — Lightweight Outcome & Learning Loop (agreed 2026-10-11)

**Owner decision: A — Lightweight Outcome & Learning Loop for Release & Operations v1.0.** A release closes with honest operational evidence and accountable next steps; it should not become a mandatory retrospective meeting or report for every small deployment. **Technical release closure and longer-term business outcome confirmation are distinct.**

**Minimum closeout, recorded in existing release ticket/pipeline/Jira/incident tooling:**

1. **Actual release disposition:** record the released artifact/targets/exposure, actual deployment result and Q4 health/critical-behavior observations, distinguishing **Healthy / Degraded / Inconclusive / Recovery in progress** with timestamps and source-linked evidence. Never replace unrun or asynchronous checks with a success claim.
2. **Human outcome decision and risk:** an authorized human release/outcome owner decides whether to close as **Healthy**, keep open/escalated for degradation, or hand off outstanding follow-up. Required Q3/Q4 checks remain mandatory; closure cannot waive missing critical evidence or ongoing unsafe conditions.
3. **Owner for every meaningful gap:** note unresolved defects, dependency/compatibility issues, incident links, operational risks and any long-tail monitoring with a concrete **owner and next checkpoint**. Make pending business/customer acceptance visible without falsely keeping the deployment in an endless “pending” state.
4. **Feed work back proportionately:** create or link **new Work Items only for actionable defects, reliability improvements or missing behavior**, tied to actual evidence. Material scope/contract changes re-enter the relevant upstream Design/Planning gates; routine follow-ups do not reopen past approvals without cause.
5. **Learn when the impact warrants it:** a consequential outage, regression, rollback, security/data event or recurring failure should receive a **focused incident review**, using an evidence-backed timeline, contributing factors and prevention actions. For tiny healthy releases, the linked release record is enough; no mandatory meeting, new Markdown artifact or universal ceremony.

**Business outcome boundary:** technical health means the shipped service meets its measured technical/critical-flow criteria; it does **not prove** users adopted the feature or that product outcomes improved. If those signals mature later, assign their appropriate product/feature owner and a future observation checkpoint. Reassess poor outcomes as fresh evidence, not automatically as a failed deploy.

**AI assistance:** AI may prepare release summaries, identify unowned gaps, propose actionable Work Items and synthesize authorized incident evidence. AI **cannot silently close a degraded release, accept residual risk, change product priorities, create production privileges or declare human approval**. The original human-controlled release and intervention rules remain in force.

## Proposed cohesive Release & Operations lifecycle — review candidate (not yet Accepted)

The six owner decisions form one **iterative, risk-adaptive release responsibility**, not a mandatory waterfall and not a new set of approvals for every test:

1. **Select release scope:** collect Work Items that have applicable **Human Verification Gate** decisions and compatible release dependencies. Pin immutable artifact(s)/manifest, included source/build versions and production exposure; a Work Item may be verified while a cross-team Story still lacks integration evidence.
2. **Assess readiness (Q3):** verify the combined release against CI/verification evidence, contract/configuration/migration compatibility, environment access, release/incident owners, realistic stop/recovery plan and risk-appropriate production validation plan. **Critical or policy-mandated gaps yield No-Go**, never a manufactured Pass.
3. **Request bounded approval (Q1 + Q2):** the authorized human Release Owner gives an explicit **Go / No-Go** for the specific artifact, targets, rollout/exposure and constraints, recorded in existing release tooling. This is independent of MR Ready/Merge, Work Item Implementation/Verification and protected-environment execution rights.
4. **Execute approved ordinary stages:** an authorized pipeline/human deploys only inside that exact envelope. Record artifact, target, step and execution outcome. An approved routine pipeline stage need not prompt the human again; **a new anomaly-driven operational intervention does (Q5)**.
5. **Validate & observe (Q4):** separately assess deployment completion, service/dependency health, changed business/critical-flow evidence, objective stop triggers and risk-appropriate observation. **Automatic detection/alerts** may run; pause/abort/rollback/roll-forward or other remedial intervention needs **fresh human confirmation under Q5** and existing stronger safety rules.
6. **Close or escalate and learn (Q6):** the authorized human records an evidence-backed status; unresolved incidents/critical missing evidence are not stamped Healthy. Assign residual risks, long-tail/customer outcome follow-ups and actionable engineering improvements. Re-enter upstream decisions only for material change.

**Minimum shared record:** release identity / manifest and affected Work Items + MRs; CI/verification and dependency evidence; risk and recovery plan; release human decision (who/when/scope); deployed artifact and safe validation/observation results; Q5 intervention decisions if any; release disposition and remaining owners. These can be short links/fields in existing tooling, with no required new document.

**Authority boundary:** approval of this *workflow design* authorizes **no actual Production deployment**, GitLab Ready/Merge, new production access, recovery command, AI skill or automation. Protected environments and emergency/incident policy remain authoritative. If the stated Human-confirmation policy conflicts with a mandatory independent safety mechanism, **do not disable that mechanism**; reconcile under the system's responsible owner.

## Candidate final-review checklist — before explicit v1.0 approval

1. **Boundary & unit:** Work Items and release bundles remain distinct; Verification Gate passing is not permission to merge, deploy or enable a feature; no guessed target or artifact.
2. **Human authority:** Q1 explicit release approval, Q2 bounded scope, and Q5 fresh human authorization before *each anomaly-driven intervention* coexist with ordinary authorized pipeline automation and mandatory independent fail-safes.
3. **Readiness & proof:** Q3 covers full-release compatibility and policy-required checks; Q4 proves observed post-deploy technical and critical-flow behavior rather than pipeline status; missing critical evidence blocks a Healthy claim.
4. **Recovery safety:** realistic rollback/roll-forward or mitigation owner, migration irreversibility and data/user impacts are examined before deployment; AI has no autonomous production privileges.
5. **Learning & proportionality:** Q6 differentiates health from product outcomes and assigns concrete owners to actionable residual gaps without a report/ceremony mandate.
6. **No accidental rollout authorization:** all Q1–Q6 decisions are approved **design inputs only**. This overall workflow is **still a Design Draft** until separate explicit owner acceptance; actual pilot, skills, CI/CD configuration and production permissions are separate.

## Final review — 2026-10-11

**Result: Ready for explicit owner review/approval (Release Candidate, not Accepted).** Compared the cohesive lifecycle with the Accepted Feature Delivery Lifecycle, Implementation and Verification workflows, and both language variants. No blocking decision conflict found:

- **Upstream handoff:** Work Item Verification is evidence for, not authorization of, a potentially multi-Work-Item Production Release; MR Ready/Merge and production release remain distinct human-controlled decisions.
- **Human and automation boundaries:** Q1 Human Release Gate and Q2 pinned authorization allow only planned pipeline execution; Q5 requires separate human confirmation for every **new anomaly-response intervention**. Detection/paging are not interventions.
- **Safety & evidence:** Q3 missing mandatory evidence yields No-Go, while Q4 never infers Production Health/Business Behavior from a successful pipeline. Q6 leaves factual unresolved outcomes with accountable owners.
- **Cross-scale fit:** existing linked records and risk-based depth avoid a mandatory new Jira type, runbook, report or ceremony; project-specific CI/CD, approvers and incident policy must still be resolved during adoption.

**Explicit adoption caveat (known Q5 trade-off):** Requiring human confirmation even before a protective pause can enlarge the blast radius when responders are unreachable. High-risk projects must **validate on-call response and mandatory independent safety mechanisms** before using this workflow. Never disable platform-enforced fail-safes to implement generic workflow text. This choice remains deliberate and was not silently changed to automated pausing.

**Remaining work outside v1.0 document acceptance:** pilot in a safe, non-sensitive environment; identify actual human owners, required company/platform controls, release and recovery permissions, CI/CD implementation and evaluation metrics. No production access, skills or automation approved.

**Approval status:** Release Candidate — **awaiting explicit owner approval**.
