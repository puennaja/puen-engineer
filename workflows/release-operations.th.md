# Release & Operations Workflow — v1.0 Design Draft (ภาษาไทย)

- **สถานะ:** Design Draft — **ยังไม่ Accepted**; ตกลง Q1 และ Q2 แล้ว ส่วน Q3 รอตัดสิน
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

**ขอบเขต Decision:** Q1/Q2 เป็นการตกลง **หลักการออกแบบ Policy** ไม่ใช่อนุญาตให้เข้าถึง Production, ใช้ Credentials, Deploy จริง หรือสร้าง Automation ใหม่ Workflow ทั้งฉบับยังเป็น Draft

## Q3 — เรื่องที่ต้องตัดสินต่อ (ยังไม่ตกลง)

**ก่อน Human กดอนุมัติ Production Release ตาม Q1/Q2 ต้องมี Readiness Evidence ระดับไหน?**

- **A. Risk-adaptive Readiness พร้อม Safety Boundaries ที่ห้ามข้าม (แนะนำ):** มี Artifact/Scope ที่ตรึงแน่นอน, Verification/CI Evidence ที่เกี่ยวข้อง, ประเมิน Dependencies/Contracts และ Recovery, Release Owner, Target Permissions, Validation/Observation Plan และ Blockers ที่ระบุชัด เพิ่มความเข้มการตรวจ Migration, Security, Integration, Progressive Rollout และ Monitoring ตาม Risk หาก Required Check ขาดหรือ Critical Risk ยังไม่ยอมรับได้ → **No-Go** จนแก้หรือมี Alternative ตาม Policy ที่ได้รับอนุมัติ AI ยกเว้นเองไม่ได้
- **B. Uniform Heavyweight Checklist:** ทุก Release ต้องใช้ Checklist/Tests/Runbook ฉบับหนักเท่ากันไม่ว่างานเล็กใหญ่หรือ Risk ต่างกัน

**หลัง Q3:** ค่อยออกแบบ Post-deploy Observation, Stop/Recovery Authority และ Learning Loop ก่อนส่ง Workflow v1.0 ให้ Review/Approve
