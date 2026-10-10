# Release & Operations Workflow — v1.0 Design Draft (ภาษาไทย)

- **สถานะ:** Design Draft — **ยังไม่ Accepted**; ตกลง Q1, Q2 และ Q3 แล้ว ส่วน Q4 รอตัดสิน
- **วันที่:** 2026-10-10
- **Track:** A — My Engineer, ออกแบบจาก Engineering Practices ไม่ผูกบริษัทหรือเครื่องมือ
- **ก่อนหน้า:** [Implementation (Accepted)](./implementation.th.md), [Verification (Accepted)](./verification.th.md), [Feature Delivery Lifecycle](./feature-delivery-lifecycle.th.md)
- **ภาษาอังกฤษ:** [Release & Operations (EN)](./release-operations.md)
- **หลักคิด:** Work Item Verify ผ่าน, Merge, Deploy, Enable Feature และ Business Outcome เป็นคนละเหตุการณ์/คนละ Decision

## Q1 — Fixed Human Release Gate (ตกลงเมื่อ 2026-10-10)

**Owner Decision: เลือก A — Fixed Human Release Gate สำหรับ Release & Operations v1.0**

**Production Release ทุกครั้งต้องได้รับ Explicit Human Authorization จาก Release Decision Owner ที่มีอำนาจก่อน Execute** หลังอนุมัติแล้ว Pipeline ที่ได้รับอนุญาตตาม Policy สามารถดำเนินขั้นตอนภายใน **Scope ของ Release ที่อนุมัติ** แบบอัตโนมัติได้ เป็นการอนุมัติ **ระดับ Release** แยกจาก Feature Design/Planning Gates, Work Item Implementation/Verification Gates และสิทธิ์ของมนุษย์ในการ Mark MR Ready/Merge

**ขอบเขต:**
- การ Verify Work Item ผ่านเป็น `Verified for Release Review` **ไม่ได้แปลว่าได้รับอนุญาตให้ Merge, Deploy หรือ Enable Feature** หนึ่ง Release อาจรวมหลาย Work Items/Repos
- AI ช่วยเตรียม/ประเมิน Release Readiness, Evidence, Compatibility, Rollout, Monitoring และ Recovery Plan ได้ **ภายในสิทธิ์ที่ได้รับอนุญาต** แต่ห้ามถือว่า CI เขียวคือ Human Approval หรือเข้าถึง Production เอง
- Pipeline ที่ได้รับอนุมัติไม่ได้ให้ AI มีสิทธิ์ Production อัตโนมัติ Credentials, Release Owner, Protected Environment, Incident Authority และนโยบายบริษัท/Repo ต้องกำหนดแยกใน Project จริง
- **ไม่ได้บังคับให้ Human กดทุกขั้นของ Pipeline** บังคับให้ Human อนุมัติ Production Release แต่ละครั้งก่อน ส่วนระบบ Automation ทำต่อภายในขอบเขตที่อนุมัติได้
- การ Deploy Code, เปิด Feature Flag หรือเปลี่ยน Exposure ของผู้ใช้ เป็นคนละการกระทำ **Q2 กำหนดแล้วว่า Approval ต้องระบุ Scope ที่อนุญาตไว้ชัด** ถ้าไม่รวมการเปิด Flag ห้ามตีความว่าเปิด Flag ได้
- การตกลง Q1 **ไม่อนุญาต** Deploy, เข้าถึง Production, เปลี่ยน Policy, สร้าง Automation หรือ AI Skill ใหม่

## Responsibilities ที่เสนอ (ยังไม่อนุมัติเป็น Workflow)

1. **Release Candidate & Readiness:** ระบุ Artifact/Build/Version ให้แน่นอน, Included Changes, Dependencies, CI Evidence, Migration และข้อจำกัด Rollback/Roll-forward
2. **Release Coordination & Go/No-Go:** กำหนด Release Owner, Services/Targets, Rollout/Risk/Stop Criteria, Communication และ Authorization
3. **Controlled Deployment & Validation:** Execute โดย Pipeline/บุคคลที่ได้รับสิทธิ์ ตรวจว่า Deploy สำเร็จจริงและ Critical Behavior ยังถูกต้อง
4. **Operations, Recovery & Learning:** ติดตาม Metrics/Logs/Signals, ตรวจ Regression, Mitigate/Rollback/Roll-forward เมื่อปลอดภัย ประสาน Incident Owner และบันทึกผลจริง

**ความลึกตาม Risk:** Low-risk/Reversible ทำแบบกระชับได้; High-risk, Multi-service, Data Changes หรือย้อนกลับยาก ต้องตรวจ Compatibility, Recovery, Monitoring และ Human Owner เข้มขึ้น เป็นแนวทางเสนอ **ไม่ใช่ Gate ใหม่ที่อนุมัติแล้ว**

## Q2 — Bounded Release Authorization (ตกลงเมื่อ 2026-10-10)

**Owner Decision: เลือก A — Bounded Release Authorization สำหรับ Release & Operations v1.0** Human อนุมัติ Production Release **ที่ระบุขอบเขตชัดเจนแต่ละครั้ง** ไม่ใช่อนุมัติช่วงเวลากว้าง ๆ ให้เลือก Build ใดมา Deploy ก็ได้

**ขอบเขตขั้นต่ำของ Approval ตามผลกระทบจริง:**
- **สิ่งที่จะปล่อย:** Artifact Digest/Version หรือ Release Manifest ที่ตรึงแน่นอน, Build/Commit ต้นทาง, Repos/Services ที่รวมอยู่ และ Dependencies สำคัญ
- **ปลายทางและ Exposure:** Production Environment/Cluster/Region/Tenant ที่ได้รับอนุญาตตามความเกี่ยวข้อง, Services, Deployment Strategy และขอบเขต Traffic/Users/Feature Flag หากไม่ได้อนุมัติ **Enable Feature Flag** ไว้ใน Scope ก็ห้ามถือว่าอนุมัติให้เปิด
- **ข้อจำกัดการทำงาน:** Rollout Steps ที่อนุญาต, เวลา/อายุ Approval ถ้าจำเป็น, Validation Signals, Pause/Abort Criteria, Recovery/Rollback/Roll-forward Plan และผู้รับผิดชอบ, Human Release Approver
- **Traceability:** ใช้ Release Ticket/Pipeline Record ที่มีอยู่บันทึก **ใครอนุมัติอะไร ที่ไหน เมื่อไร ภายใต้เงื่อนไขอะไร** โดยไม่สร้าง Markdown File ใหม่เป็นข้อบังคับ

**Automation ภายใน Scope:** Pipeline ที่ได้รับสิทธิ์สามารถทำ Stages ตาม Approval และ Retry/Continue เมื่อ **ได้รับอนุญาตไว้ตาม Policy, ปลอดภัย, ไม่เพิ่ม Risk/Scope และยังใช้ Artifact/Target เดิม** ห้ามใช้ Retry เพื่อข้าม Stop Criteria คำสั่งเสี่ยง/ทำลายและ Incident Recovery ยังอยู่ภายใต้ Process/Permissions ที่อนุมัติแยก

**เมื่อ Approval ใช้ต่อไม่ได้:** เปลี่ยน Artifact/Manifest, เปลี่ยน Production Target/Services/Exposure, ขยาย Rollout, เพิ่ม Migration/Contract Risk, Approval หมดอายุ หรือ Assumptions ที่สำคัญเปลี่ยน → **หยุด Action ที่กระทบและขอ Human Approval ใหม่** ส่วนการรัน Check เดิมที่อนุญาตแล้วแบบไม่มีผลเสี่ยงเพิ่มไม่จำเป็นต้องขอใหม่ทุกครั้ง ห้ามอ้าง CI เขียว, Work Item Verify ผ่าน, Merge แล้ว หรือ Approval จาก Release เก่าแทนการอนุมัติครั้งนี้

**ขอบเขต Decision:** Q1–Q3 เป็นการตกลง **หลักการออกแบบ Policy** ไม่ใช่อนุญาตให้เข้าถึง Production, ใช้ Credentials, Deploy จริง หรือสร้าง Automation ใหม่ Workflow ทั้งฉบับยังเป็น Draft

## Q3 — Risk-adaptive Release Readiness (ตกลงเมื่อ 2026-10-10)

**Owner Decision: เลือก A — Risk-adaptive Release Readiness พร้อม Safety Boundaries ที่ห้ามข้าม สำหรับ Release & Operations v1.0** ก่อนเข้าสู่ **Q1 Human Release Gate** ทุก Production Release ต้องมีหลักฐาน Readiness ที่เชื่อถือได้เป็นขั้นต่ำ แล้วเพิ่มความเข้มตาม **Risk จริง** ไม่ใช่จำนวนไฟล์หรือขนาดทีม

**Minimum Readiness Contract — ทุก Release บันทึกให้กระชับตามบริบท:**

| สิ่งที่ต้องประเมิน | คำถาม/หลักฐานขั้นต่ำก่อน Human Go / No-Go |
| --- | --- |
| **Release Identity & Bounds** | จะ Deploy Artifact/Manifest, Build/Commit ไหน, Services อะไร, Production Target และ Exposure Scope ใด? ต้องตรงกับ Q2 ที่ขออนุมัติ |
| **Quality Evidence** | มี Verification Evidence ของ Work Items, CI/Build/Tests ที่เกี่ยวข้อง, Blocking Findings และ Gaps อะไรบ้าง? ต้องประเมิน **ภาพรวมของ Changes ที่จะ Release ร่วมกัน** ไม่ใช่ดู CI เขียวอย่างเดียว |
| **Compatibility & Dependencies** | มีผลกระทบ API/Data Contracts, Config, Schema, Services ไหนบ้าง? Integration/Migration Order จำเป็นหรือไม่ ใครเป็น Owner? ถ้าไม่เกี่ยวข้องให้ระบุ N/A ตามข้อเท็จจริง |
| **Recovery Readiness** | หยุด, Mitigate, Rollback หรือ Roll Forward อย่างปลอดภัยได้อย่างไร? Data Migration ส่วนไหนย้อนกลับไม่ได้? มี Trigger, Action และ Owner ชัดหรือไม่ |
| **Execution & Ownership** | ใครมีอำนาจ Approve/Execute, Pipeline/Environment Permissions อะไรจำเป็น และมี Downstream Effects อะไร |
| **Validation & Observation** | จะตรวจ Smoke/Critical Behavior, Health, Metrics/Logs/Traces และเฝ้าดูตามระยะเวลา/เกณฑ์ใด? มี Pause/Abort Criteria ที่ระบุไว้หรือไม่ |
| **Risks & Decision** | Checks ไหน **Pass / Fail / Not run / Blocked / Inconclusive**? อันไหนเป็น Mandatory/Critical? เหลือ Risk อะไร ใครรับผิดชอบ และ Human ตัดสิน **Go / No-Go** อย่างไร |

**ปรับความลึกตาม Risk:**
- **Low-risk / Reversible:** ใช้ Jira/Release Ticket/Pipeline Links สั้น ๆ พร้อม Focused Checks, Recovery/Validation Owner ไม่ต้องสร้าง Heavyweight Checklist ใหม่
- **Moderate / Multi-service:** เพิ่ม Compatibility, Configuration/Integration Checks และ Recovery/Operational Validation ที่ใกล้ของจริง
- **High-risk / Authorization, Sensitive Data, Money, Schema Migration, Destructive หรือย้อนกลับยาก:** ต้องมี Human/Domain Review และ Targeted Security/Data/Migration/Failure/Dependency/Recovery Proof ตาม Risk และ Policy ของ Project จริง **Diff เล็กก็อาจเสี่ยงสูง**

**Hard Stop:** ถ้า **Mandatory Check ไม่ครบ**, Critical Acceptance/Contract ไม่ได้รับการพิสูจน์, มี Blocking Defect, Permission/Environment ไม่ปลอดภัย หรือ Critical Risk ไม่ถูกจำกัดให้รับได้ → **No-Go / Blocked** Human Owner ยอมรับได้เฉพาะ **Equivalent Evidence หรือ Exception ที่ Policy อนุญาตจริง** พร้อมเหตุผล ข้อจำกัด และ Residual Risk Owner; **AI ยกเว้นเองไม่ได้** และ Test ที่ไม่รันห้ามรายงานว่า Pass หากไม่มีทางเลือกที่ถูกต้องตาม Policy ก็ **ไม่ Release** ต้องประเมิน Release ทั้งชุด ไม่ใช่เพียง Work Item แยกส่วน

**การบันทึก:** ใช้ Release Ticket/Pipeline, CI Links และ Jira/MR Evidence ที่มีอยู่ ไม่สร้าง Markdown Report ใหม่เป็นข้อบังคับ และการตกลง Policy Design นี้ **ไม่ใช่การอนุญาต Deploy จริง** Verification Gate, Merge และ Release เป็นคนละ Decision

## Q4 — เรื่องที่ต้องตัดสินต่อ (ยังไม่ตกลง)

**หลัง Pipeline Deploy สำเร็จ ต้องตรวจอะไรอีกก่อนจะถือว่า Release จบสมบูรณ์?**

- **A. Risk-adaptive Production Validation & Observation (แนะนำ):** แยก **Deployment Completed** ออกจาก **Service Health** และ **Business Behavior** รันเฉพาะ Post-deploy Checks ที่ปลอดภัย/ได้รับอนุญาต, ตรวจ Health/Metrics/Logs/Traces กับ Dependencies, เฝ้าดูช่วงเวลาหรือ Signal Threshold ตาม Risk และใช้ Pause/Abort/Escalation Criteria ที่กำหนดก่อนหน้า บันทึกผล **Healthy / Degraded / Inconclusive / Recovery in progress**, Human Owner และ Residual Risks ไม่ตีความว่า Pipeline เขียวคือ Release ผ่าน
- **B. Pipeline-success Closure:** Pipeline สำเร็จแล้วปิด Release ได้เลย โดยให้ Health/Behavior Checks เป็น Optional Follow-up

**หลัง Q4:** ออกแบบ Recovery/Incident Decision Authority และ Learning Loop ต่อ ก่อน Full Review และรอ Owner Approve v1.0 แยกอีกครั้ง
