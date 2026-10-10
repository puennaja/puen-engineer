# Release & Operations — Artifact Template (EN)

> **Release-level record, not a single Work Item report.** One Release may include several verified Work Items, repos and dependent versions. Use the existing release ticket/pipeline/incident system where possible. This template **does not authorize any real production action**.
>
> Workflow: [Release & Operations v1.0 (Accepted)](../../workflows/release-operations.md) · [Thai version](./release-operations.th.md)

- **Release / change ID:** <existing release ticket / pipeline record>
- **Status:** Preparing | Readiness Blocked / No-Go | Approved (human) | Deploying | Observing | Healthy | Degraded | Inconclusive | Recovery in progress
- **Release owner / incident/on-call owner:** <authorized and reachable humans>
- **Included Work Items / MRs / verification evidence:** <links; do not confuse Work Item pass with integrated Story pass>
- **Last updated:** <actual timestamp/timezone>

## 1. Immutable release identity and bounded scope (Q2)

- **Pinned artifact digest/version or release manifest:** <exact build(s), commit(s), services/repos; no guessed versions>
- **Authorized production target(s):** <environment, cluster, region, tenant and destination as relevant>
- **Deployment / rollout steps:** <planned stages, traffic/users/feature exposure; include feature flag enablement **only if separately in approved scope**>
- **Dependencies / compatibility / rollout ordering:** <actual contract versions and cross-owner coordination>
- **Approval validity / stop criteria:** <time or trigger where applicable; material changes invalidate approval>
- **Release approver / executor permissions:** <who can decide, who can operate; distinguish from mere credential possession>

## 2. Risk-adaptive readiness and hard stops (Q3)

| Safety dimension | Verified evidence / actual link | Pass / Fail / Not run / Blocked / Inconclusive | Owner or action |
| --- | --- | --- | --- |
| Release artifact + destination match | <immutable manifest and target> | <status> | <owner> |
| Applicable CI, Build and Work Item Verification evidence | <actual test/reviewer/CI links> | <status> | <owner> |
| Cross-service/API/schema/config compatibility and integration | <actual proof, or N/A reason> | <status> | <owner> |
| Data migration, irreversibility and recovery strategy | <safe mitigation/rollback or roll-forward plan, trigger and owner> | <status> | <owner> |
| Permissions, incident ownership and emergency escalation | <authorized target/operator and reachable owner> | <status> | <owner> |
| Planned smoke checks, SLI/SLO or relevant health signals, observation and abort criteria | <baseline/threshold, safe checks, observer and time/signal window> | <status> | <owner> |

- **Risk level / reason and added checks:** <risk-adaptive; money/data/security/migration/cross-repo may require more>
- **Mandatory missing proof / critical blockers:** <No-Go unless resolved or valid policy-authorized alternative; NEVER invent Pass>
- **Human readiness recommendation:** Go | No-Go | Blocked; <reason, residual risk>

## 3. Fixed Human Release Gate and exact authorization (Q1 + Q2)

- **Human Release decision:** Go | No-Go | Deferred | Pending
- **Decider / timestamp / approval evidence link:** <real decision by authorized person>
- **Precisely approved envelope:** <manifest/version, target services/env, rollout/exposure, validations and validity bounds>
- **Exceptions / owned risks under applicable policy:** <documented or None>
- **Change-triggered reauthorization:** <if artifact, destination, exposure or material risk changes: Stop + new human approval>
- **Production privileges:** <separate authorized pipeline/operator permission; this document grants none>

## 4. Actual deployment and production validation (Q4)

| Observation | Expected / baseline / objective trigger | Actual result + time + evidence | Pass / Fail / Not run / Inconclusive |
| --- | --- | --- | --- |
| Deployment: what artifact/target/rollout actually executed | <Q2 approved envelope> | <pipeline result, version, partial/failed stages> | <status> |
| Service health: readiness/error rate/latency/dependencies | <real baseline/threshold or Unknown> | <monitor/log/trace and timestamp> | <status> |
| Changed critical behavior / safety signals | <source-backed acceptance/observable result> | <authorized non-destructive smoke/contract evidence> | <status> |
| Observation window / asynchronous effects | <risk-proportionate time or signal requirement> | <actual observation or outstanding follow-up> | <status> |

- **No unauthorized production writes/charges/events:** <safe procedure/fixture and permission, or Not run>
- **Separate dispositions:** Deployment: Completed/Partial/Failed/Unknown; Service: Healthy/Degraded/Inconclusive; Behavior: Pass/Fail/Not run/Inconclusive
- **Release disposition:** Healthy | Degraded | Inconclusive | Recovery in progress (not simply “pipeline green”)

## 5. Incident detection and human-confirmed interventions (Q5 B)

- **Symptoms / threshold / impacted users/data / first alert time:** <real evidence or N/A>
- **Automatic monitoring/paging:** <system and notified reachable human>
- **Every intervention requires fresh authorized human confirmation:** Pause / Abort / Remedial retry / Rollback / Roll Forward / Restart / Feature Exposure / Recovery
- **For each actual intervention:** <action, exact target/scope, human approver, timestamp, authorization link, executor/pipeline, result and post-action health evidence>
- **If no human reachable:** <escalate via incident policy; no autonomous new intervention>
- **Safety boundary:** <do not disable existing independent platform fail-safes; don't confuse ordinary preapproved pipeline stages with new anomaly response>

## 6. Release closeout and learning (Q6 A)

- **Final human release/outcome owner, decision and evidence:** <Healthy / Degraded / Inconclusive / Ongoing incident; timestamp and link>
- **Unresolved risks, long-tail monitoring and next owner/checkpoint:** <explicit responsible person/date>
- **Customer/product outcome still to measure:** <separate accountable owner and follow-up; technical health ≠ business value>
- **Incident review (only when impact warrants):** <timeline/evidence, contributing factors, follow-up links>
- **Actionable follow-up Work Items:** <links only where justified; do not force tickets/ceremonies for healthy small releases>

**Authority boundary:** This is an **optional record template**, not a production approval. Q1 requires explicit human approval for **each** release; Q5 requires new human confirmation before **every anomaly-driven intervention**. Production credentials, platform rules, protected environments, MR Ready/Merge and Skills/Automation are separately authorized.
