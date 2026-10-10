# Release & Operations — Artifact Template (ภาษาไทย)

> **Artifact ระดับ Release ไม่ใช่รายงาน Work Item เดียว** Release หนึ่งอาจรวม Work Items, Repos และ Versions หลายชุด ใช้ Release Ticket/Pipeline/Incident Record ที่มีอยู่ Template นี้ **ไม่อนุญาตให้ทำ Production Action จริง**
>
> Workflow: [Release & Operations v1.0 (Accepted)](../../workflows/release-operations.th.md) · [English](./release-operations.md)

- **Release / Change ID:** <Release Ticket / Pipeline เดิม>
- **สถานะ:** Preparing | Readiness Blocked / No-Go | Approved (human) | Deploying | Observing | Healthy | Degraded | Inconclusive | Recovery in progress
- **Release Owner / Incident-On-call Owner:** <Human ที่มีอำนาจและติดต่อได้>
- **Work Items / MRs / Verification Evidence ที่รวม:** <Links; Work Item ผ่านไม่เท่ากับ Story Integration ผ่าน>
- **อัปเดตล่าสุด:** <วันที่เวลาและ Timezone จริง>

## 1. Immutable Release Identity และ Bounded Scope (Q2)

- **Pinned Artifact Digest/Version หรือ Release Manifest:** <Build(s), Commit(s), Services/Repos จริง ไม่เดา Version>
- **Authorized Production Targets:** <Environment/Cluster/Region/Tenant/Destination ตามบริบท>
- **Deployment / Rollout Steps:** <Stages, Traffic/Users/Feature Exposure; เปิด Feature Flag ได้เฉพาะอยู่ใน Approved Scope>
- **Dependencies / Compatibility / Rollout Order:** <Contract Versions ที่ตรวจจริงและ Cross-owner Coordination>
- **Approval Validity / Stop Criteria:** <ช่วงเวลา/Trigger ตามที่เกี่ยวข้อง; เปลี่ยน Scope/Risk สำคัญต้องอนุมัติใหม่>
- **Release Approver / Executor Permission:** <ใครตัดสิน ใครมีสิทธิ์ Execute; มี Credential ไม่เท่ากับได้ Approval>

## 2. Risk-adaptive Readiness และ Hard Stops (Q3)

| Safety Dimension | Verified Evidence / Link จริง | Pass / Fail / Not run / Blocked / Inconclusive | Owner / Action |
| --- | --- | --- | --- |
| Artifact + Destination ตรง Scope | <Manifest/Target> | <Status> | <Owner> |
| CI/Build + Work Item Verification Evidence | <Tests/Reviews/CI Links จริง> | <Status> | <Owner> |
| Cross-service/API/Schema/Config Compatibility + Integration | <Proof หรือ N/A พร้อมเหตุผล> | <Status> | <Owner> |
| Data Migration, Irreversible Changes และ Recovery | <Mitigation/Rollback/Roll-forward Trigger และ Owner> | <Status> | <Owner> |
| Permissions, Incident Owner และ Emergency Escalation | <Authorized Target/Operator และ On-call ที่ติดต่อได้> | <Status> | <Owner> |
| Smoke Checks, Health Signals, Observation & Abort Criteria | <Baseline/Threshold, Safe Checks, Observer และ Window> | <Status> | <Owner> |

- **Risk Level / เหตุผล / Checks เพิ่ม:** <Risk-adaptive; Money/Data/Security/Migration/Cross-repo ต้องเข้มตามความเสี่ยง>
- **Mandatory Evidence ที่ขาด / Critical Blockers:** <No-Go ถ้าไม่แก้หรือไม่มี Alternative ที่ Policy อนุญาต ห้ามอ้าง Pass เอง>
- **Human Readiness Recommendation:** Go | No-Go | Blocked; <เหตุผลและ Remaining Risks>

## 3. Fixed Human Release Gate และ Authorization Envelope (Q1 + Q2)

- **Human Release Decision:** Go | No-Go | Deferred | Pending
- **Human Decider / Timestamp / Approval Evidence:** <ผู้มีอำนาจจริงพร้อม Link>
- **Precisely Approved Scope:** <Manifest/Version, Targets, Services, Rollout/Exposure, Validation และ Validity>
- **Exceptions / Risks ตาม Policy:** <บันทึกหรือ None>
- **Reauthorization Triggers:** <Artifact, Target, Exposure หรือ Material Risk เปลี่ยน → Stop แล้วขอ Human Approval ใหม่>
- **Production Privileges:** <Pipeline/Operator ที่ได้สิทธิ์แยก ไม่ได้มาจาก Template>

## 4. Deployment จริงและ Production Validation (Q4)

| Observation | Expected / Baseline / Objective Trigger | Actual Result + เวลา + Evidence | Pass / Fail / Not run / Inconclusive |
| --- | --- | --- | --- |
| Deploy Artifact/Target/Rollout ที่เกิดจริง | <Q2 Approved Envelope> | <Pipeline Result, Version, Partial/Failed Stages> | <Status> |
| Service Health: Readiness/Error Rate/Latency/Dependencies | <Baseline/Threshold จริง หรือ Unknown> | <Monitoring/Logs/Traces พร้อมเวลา> | <Status> |
| Critical Changed Behavior / Safety Signals | <Acceptance / Expected Behavior ที่ยืนยัน> | <Authorized Non-destructive Smoke/Contract Checks> | <Status> |
| Observation Window / Async Effects | <ช่วงเวลา/Signal ตาม Risk> | <Observation จริง หรือ Follow-up ที่ยังค้าง> | <Status> |

- **ห้าม Prod Writes/Charges/Events ที่ไม่ได้รับอนุญาต:** <Safe Procedure/Fixture/Permission หรือ Not run>
- **แยก Dispositions:** Deployment: Completed/Partial/Failed/Unknown; Service: Healthy/Degraded/Inconclusive; Behavior: Pass/Fail/Not run/Inconclusive
- **Release Disposition:** Healthy | Degraded | Inconclusive | Recovery in progress (ไม่ใช่แค่ Pipeline เขียว)

## 5. Incident Detection และ Human-confirmed Interventions (Q5 B)

- **Symptoms / Threshold / Impact / เวลา Alert:** <Evidence จริง หรือ N/A>
- **Automatic Monitoring/Paging:** <ระบบตรวจและ Human ที่ได้รับแจ้ง>
- **Intervention ทุกครั้งต้องขอ Human Confirm ใหม่:** Pause / Abort / Remedial Retry / Rollback / Roll Forward / Restart / Feature Exposure / Recovery
- **Intervention แต่ละ Action ที่ Execute จริง:** <Action, Target/Scope, Human Approver, เวลา, Approval Link, Executor/Pipeline, Result และ Post-action Health Evidence>
- **ถ้า Human ติดต่อไม่ได้:** <Escalate ตาม Incident Policy ห้ามทำ Intervention ใหม่เอง>
- **Safety Boundary:** <ไม่ปิด Independent Platform Fail-safes; อย่าสับสน Routine Stages ที่อนุมัติแล้วกับ Anomaly Response ใหม่>

## 6. Release Closeout และ Learning (Q6 A)

- **Final Human Release/Outcome Owner, Decision & Evidence:** <Healthy/Degraded/Inconclusive/Ongoing Incident พร้อมเวลาและ Link>
- **Remaining Risks / Long-tail Monitoring / Next Owner & Checkpoint:** <ผู้รับผิดชอบและวันที่>
- **Customer/Product Outcome ที่ต้องรอพิสูจน์:** <แยก Owner/Follow-up; Technical Health ไม่เท่ากับ Business Value>
- **Incident Review (เมื่อ Impact สมควร):** <Timeline/Evidence/Contributing Factors/Follow-ups>
- **Actionable Follow-up Work Items:** <Links เมื่อมีเรื่องต้องแก้จริง ไม่บังคับ Ticket/Meeting ทุก Release เล็ก>

**Authority Boundary:** Template นี้เป็น Optional Record **ไม่ใช่ Production Approval** Q1 บังคับ Human Approve **ทุก Release** และ Q5 บังคับ Human Confirm ใหม่ก่อน **Anomaly-driven Intervention ทุก Action** Production Credentials/Platform Rules/Protected Environments/MR Ready-Merge/Skills/Automation ต้องมี Authorization แยก
