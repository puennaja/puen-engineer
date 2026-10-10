# Release & Operations Workflow — v1.0 Design Draft (ภาษาไทย)

- **สถานะ:** Design Draft — **ยังไม่ Accepted**; ตอนนี้ตกลงเฉพาะ Q1
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
- Deploy Code, เปิด Feature Flag หรือเปลี่ยน Exposure ของผู้ใช้ อาจมี Risk/Authorization ต่างกัน; **ขอบเขตที่ Approval หนึ่งครั้งครอบคลุมเป็น Q2 ที่ยังไม่ตัดสิน**
- การตกลง Q1 **ไม่อนุญาต** Deploy, เข้าถึง Production, เปลี่ยน Policy, สร้าง Automation หรือ AI Skill ใหม่

## Responsibilities ที่เสนอ (ยังไม่อนุมัติเป็น Workflow)

1. **Release Candidate & Readiness:** ระบุ Artifact/Build/Version ให้แน่นอน, Included Changes, Dependencies, CI Evidence, Migration และข้อจำกัด Rollback/Roll-forward
2. **Release Coordination & Go/No-Go:** กำหนด Release Owner, Services/Targets, Rollout/Risk/Stop Criteria, Communication และ Authorization
3. **Controlled Deployment & Validation:** Execute โดย Pipeline/บุคคลที่ได้รับสิทธิ์ ตรวจว่า Deploy สำเร็จจริงและ Critical Behavior ยังถูกต้อง
4. **Operations, Recovery & Learning:** ติดตาม Metrics/Logs/Signals, ตรวจ Regression, Mitigate/Rollback/Roll-forward เมื่อปลอดภัย ประสาน Incident Owner และบันทึกผลจริง

**ความลึกตาม Risk:** Low-risk/Reversible ทำแบบกระชับได้; High-risk, Multi-service, Data Changes หรือย้อนกลับยาก ต้องตรวจ Compatibility, Recovery, Monitoring และ Human Owner เข้มขึ้น เป็นแนวทางเสนอ **ไม่ใช่ Gate ใหม่ที่อนุมัติแล้ว**

## Q2 — เรื่องที่ต้องตัดสินต่อ (ยังไม่ตกลง)

**หนึ่งครั้งที่ Human Approve Production Release ควรอนุญาตอะไรบ้าง?**

- **A. Bounded Release Authorization (แนะนำ):** ระบุ Artifact/Version หรือ Release Manifest ที่แน่นอน, Production Target, Rollout/Exposure Scope และข้อจำกัดสำคัญให้ครบ Pipeline ทำขั้นตอนภายใน Scope นี้ต่ออัตโนมัติได้ แต่ถ้าสาระสำคัญหรือ Risk เปลี่ยนต้อง Approve ใหม่ การ Recovery จาก Incident อยู่ภายใต้ Runbook/Policy และ Permission ที่อนุญาตแยก
- **B. Broad Release-window Authorization:** Human อนุมัติ Window หรือ Batch กว้าง ๆ แล้ว Pipeline เลือก Build ที่เข้าเกณฑ์ภายในช่วงนั้นตาม Policy

**รอตัดสิน:** Q2 แล้วค่อยออกแบบ Release Readiness, Recovery, Observation, Exception และ Release Decision Record จนพร้อม Review v1.0
