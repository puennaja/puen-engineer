# Release & Operations Workflow — v1.0 Design Draft (ภาษาไทย)

- **สถานะ:** Design Draft — **ยังไม่ Accepted**; ตกลง Q1–Q5 แล้ว ส่วน Q6 รอตัดสิน
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

**Automation ภายใน Scope:** Pipeline ที่ได้รับสิทธิ์ทำ **Routine Stages ที่อนุมัติไว้** และ Retry ตามปกติที่ **ไม่ใช่การแก้ Incident** ได้ภายใน Artifact/Target/Scope เดิมเท่านั้น แต่ตาม Q5 เมื่อมีเหตุผิดปกติที่ต้องสั่ง **Intervention ใหม่** เช่น Pause, Abort, Remedial Retry, Rollback, Roll Forward, Restart หรือ Recovery ต้อง **แจ้งเตือนและรอ Human ยืนยัน Action นั้นใหม่ก่อน** ห้ามใช้ Retry ข้าม Stop Criteria และไม่ได้อนุมัติ Auto Recovery/Destructive Action

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

## Q4 — Risk-adaptive Production Validation & Observation (ตกลงเมื่อ 2026-10-11)

**Owner Decision: เลือก A — Risk-adaptive Production Validation & Observation สำหรับ Release & Operations v1.0** ต้องแยก **Deployment Completed**, **Service Health**, **Changed Business Behavior** และ **Release Outcome** ออกจากกัน Pipeline สำเร็จ **ยังไม่ใช่หลักฐานว่า Release Healthy** หลัง Deploy ที่อนุญาตแล้ว ต้องตรวจผลบน Production Observation Surface ที่ปลอดภัยและมีสิทธิ์ใช้งานก่อนเสนอปิด Release

**Minimum Post-deployment Contract — เพิ่มความเข้มตาม Risk:**
1. **ตรวจ Version/Scope ที่ Deploy จริง:** Pipeline/Deployment Result, Artifact/Version, Target Environment/Region/Services และ Traffic/Feature Exposure **เทียบกับ Q2 Approval Envelope** ถ้า Partial Rollout, มี Drift หรือ Stage ไม่ได้รัน ต้องรายงานตรง ๆ
2. **ตรวจ Operational Health:** Readiness/Availability, Error Rate, Latency, Saturation เมื่อเกี่ยวข้อง, Logs/Traces, Downstream Dependency และ Queue/Job Health เทียบกับ Baseline ที่มีจริงหรือ Threshold ที่ตกลงล่วงหน้า **ห้ามแต่ง Baseline หรือถือว่าไม่มี Error เมื่อไม่มีข้อมูล**
3. **ตรวจ Changed Business Behavior:** รันเฉพาะ Smoke/Critical-flow Checks ที่ **ได้รับอนุญาตและไม่ทำลายข้อมูล** โดยอิง Acceptance รวมถึง Permission/Negative/Data-integrity Signals เมื่อเกี่ยวข้อง ถ้า Check มีโอกาสเขียนข้อมูลจริง, ตัดเงินจริง, ส่ง Event หรือกระทบผู้ใช้ ต้องใช้ Safe Procedure ที่อนุมัติแยกหรือ Synthetic/Isolated Alternative ที่ได้รับสิทธิ์ ถ้ายังตรวจไม่ได้ให้บันทึก Gap
4. **Observe & Decide:** ใช้ **Observation Window หรือ Objective Signal Threshold ตาม Risk** ที่ตกลงไว้ก่อน Release มี Observer/On-call Owner, Evidence และ Stop/Abort/Escalation Trigger งาน Low-risk อาจดูช่วงสั้น ๆ ด้วย Focused Checks ส่วน High-risk เพิ่ม Depth/Progressive Exposure/Monitoring ผลกระทบที่เกิดช้าต้องมี Owner ติดตาม ไม่อ้างว่าผ่านครบเมื่อยังไม่เห็น
5. **แยกผลลัพธ์:** Deployment (**Completed / Partial / Failed / Unknown**), Service (**Healthy / Degraded / Inconclusive**), Business Behavior (**Pass / Fail / Not run / Inconclusive**) และ Release Disposition (**Healthy / Degraded / Inconclusive / Recovery in progress**) พร้อม Owner, เวลาที่ตรวจ, Evidence และ Remaining Risks

**Safety & Escalation:** หาก Critical Threshold ผิด, Release Scope เบี่ยงจาก Approval, ผู้ใช้/ข้อมูลได้รับผลเสีย หรือ Mandatory Evidence ยังไม่ชัด ให้ **ตรวจจับ/แจ้งเตือนอัตโนมัติและ Escalate หา Human ทันทีเพื่อตัดสิน Pause/Abort/Recovery ตาม Q5** แม้เป็น Protective Pause ก็ต้องขอ Human Confirmation ใหม่ตาม Q5 ไม่สรุป Healthy จาก Pipeline/Dashboard/Pod ที่เขียว และไม่ให้ AI Intervene เอง

**เงื่อนไขปิด Release:** Human Release/Outcome Owner (หรือ Process ที่มี Human Decision ถูกต้อง) ประเมิน Evidence และ Residual Risks จะสรุป Healthy ได้เมื่อ Health/Behavior Criteria ที่เกี่ยวข้องตาม Risk ผ่านจริง ถ้า Mandatory Evidence ไม่มีให้บันทึก **Inconclusive/Blocked** ไม่ใช่ Success ใช้ Release/Pipeline/Incident Record เดิม ไม่เพิ่ม Markdown Report หรือ Human Gate ต่อ Test

## Q5 — Human Confirmation Before Every Intervention (ตกลงเมื่อ 2026-10-11)

**Owner Decision: เลือก B — Human Confirmation Before Every Intervention สำหรับ Release & Operations v1.0** Monitoring, AI และ Pipeline สามารถ **ตรวจจับ แจ้งเตือน บันทึก Evidence และเสนอทางเลือก** ได้ แต่ **ทุก Intentional Operational Intervention ที่ตอบสนองเหตุผิดปกติ ต้องได้รับ Human Confirmation ใหม่เฉพาะ Action ก่อน Execute** ได้แก่ **Pause, Abort, Rollback, Roll Forward, Restart, Remedial Retry, เปลี่ยน Feature-flag Exposure และ Recovery** แม้จะเป็น Action ป้องกันผลเสียหรือดูมี Risk ต่ำก็ตาม

**Intervention Contract:**

1. **Detect & Escalate:** แจ้ง Human Incident/Release Owner ที่ติดต่อได้ผ่านช่องทางที่ตกลง พร้อม Symptoms, Impact, Release/Version, Deployment State, Metrics/Logs และตัวเลือกที่เสนอ ระบบ Detection/Paging ทำอัตโนมัติได้ แต่ AI Recommendation ไม่ใช่ Execution Permission
2. **Human Confirm ทุก Intervention:** Owner ประเมิน Target/Scope, Schema/Data Safety, Dependencies, Side Effects, Reversibility และ Policy แล้วอนุมัติ **Action ที่ระบุชัดเจน** การอนุมัติหนึ่ง Action ไม่ได้เหมารวม Action ถัดไป
3. **Execute ตาม Operational Permission จริง:** Human หรือ Pipeline ที่มีสิทธิ์ดำเนินการเฉพาะ Action ที่ยืนยันแล้ว ถ้าการกระทำเปลี่ยน Q2 Release Envelope อย่างมีนัยสำคัญต้องได้รับ Release Authorization ใหม่ตามเกณฑ์ด้วย การเปิด Incident ไม่ได้ให้สิทธิ์ Production เพิ่มแก่ AI
4. **Revalidate & Record:** เก็บผล Execute และตรวจ Service Health/Business Behavior ใหม่ พร้อม Outcome (**Recovered / Degraded / Inconclusive / Still mitigating**), Human Decision Owner, ผู้ Execute, เวลา และ Observer/Owner ที่รับช่วง ห้ามถือว่า Rollback Command ผ่านเท่ากับ Recovery สำเร็จ
5. **ถ้า Human ติดต่อไม่ได้:** ทำ Detection, Paging, Escalation ต่อภายใต้ Incident Policy จริง แต่ **ห้าม Auto-execute Intervention หรือสมมติว่าได้รับ Approval** ดังนั้น Q3 Readiness ต้องพิจารณา On-call Owner, Escalation Route และ Response Expectations โดยเฉพาะ Release ที่มี Risk สูง

**ไม่ขัดกับ Q1–Q4:** Fixed Human Release Gate และ Bounded Authorization ยังอนุญาตให้ Pipeline ทำ **Routine Stages ที่ตกลงไว้** โดย Human ไม่ต้องกดทุก Step ส่วน Q5 คุม **Intervention ใหม่หลังเกิดเหตุผิดปกติ** ไม่ใช่งานปกติใน Pipeline การรันคำสั่งล้มเหลวหรือระบบ Platform หยุดด้วยกลไกของมันเองเป็น Observed Failure ไม่ใช่สิทธิ์ให้ AI Initiate Recovery ต่อเอง **เราไม่ได้อนุมัติให้เพิ่ม Auto-pause หรือ Auto-rollback เพื่อจัดการ Incident ใน v1.0** หากมีข้อบังคับด้าน Platform/Safety/Legal ที่เข้มงวดกว่า ต้องเคารพข้อบังคับและ Independent Fail-safe Mechanism เดิม; Draft นี้ไม่เปลี่ยน Config หรือข้าม Policy เหล่านั้น

**ข้อแลกเปลี่ยน:** การรอ Human แม้แต่ก่อน Protective Pause อาจทำให้ Response ช้าและเพิ่มผลกระทบกับผู้ใช้/ข้อมูล จึงควรมี Actionable Alerts, Escalation ที่มีคนรับผิดชอบ และ Recovery Procedure ที่น่าเชื่อถือ การเลือก B **ไม่ได้แปลว่าเป็น Incident Response ที่เร็วหรือปลอดภัยที่สุดเสมอ** แต่เป็น Choice เรื่อง Human Control ที่ตกลงสำหรับ v1.0

## Q6 — เรื่องที่ต้องตัดสินต่อ (ยังไม่ตกลง)

**หลัง Q4 Validation และ Q5 Incident Response (ถ้ามี) เราควรปิด Release และนำสิ่งที่เรียนรู้กลับไปสู่ Engineering อย่างไร?**

- **A. Lightweight Outcome & Learning Loop (กูแนะนำ):** Human Release/Outcome Owner บันทึก **ผล Health/Behavior จริง, Evidence, Remaining Risks, Follow-up Owner และ Business/Customer Outcome ที่ยังต้องติดตาม** ใน Release/Jira/Incident Record เดิม สร้าง Work Item ใหม่เฉพาะ Defect หรือ Improvement ที่ลงมือทำได้จริง ทำ Incident Review แบบกระชับเมื่อ Impact/Risk สมควร ไม่บังคับ Meeting หรือ Markdown Report ทุก Release เล็ก
- **B. Close at Technical Health:** บันทึก Technical Health และ Critical Incidents ที่ยังค้าง โดยปล่อยให้ Process อื่นติดตาม Business Outcome และ Learning แยก

**หลัง Q6:** ตรวจ Consistency ของ Lifecycle กับ Workflow ที่ Accepted แล้ว Review v1.0 ฉบับสมบูรณ์เพื่อขอ Owner Approve แยกอีกครั้ง Draft นี้ไม่ได้ให้สิทธิ์ Production Deployment หรือ AI Automation
