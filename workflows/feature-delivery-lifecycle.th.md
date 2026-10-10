# วงจรการส่งมอบฟีเจอร์ (Feature Delivery Lifecycle: FDL) — v1.0

> **หมายเหตุด้าน Architecture (2026-10-09):** เอกสารนี้อธิบายกิจกรรมทางวิศวกรรม ไม่ใช่ขั้นตอนที่ต้องทำตามลำดับแบบบังคับ ใช้แนวทางวนซ้ำตามสัดส่วนความเสี่ยง ดู [Universal Engineering System](../architecture/universal-engineering-system.md) และ [ADR-0001](../decisions/0001-universal-engineering-system.md)

- **สถานะ:** Accepted — พื้นฐานของ My Engineer (ไม่ใช่นโยบายบริษัทโดยอัตโนมัติ)
- **อนุมัติ:** 2026-10-09 โดยเจ้าของ Repository
- **เวอร์ชัน:** 1.0
- **สร้างเมื่อ:** 2026-10-09
- **เจ้าของ:** My Engineer
- **ขอบเขต:** ฟีเจอร์หรือ Change หนึ่งงาน ตั้งแต่คำขอเบื้องต้นจนถึงผลลัพธ์ Production ที่ตรวจสอบแล้ว
- **เอกสารคู่:** [AI-assisted feature delivery](./ai-assisted-feature-delivery.th.md)
- **ฝึกเพิ่มเติม (ไม่บังคับ):** [Track B — Engineering Practice](../practice/README.md)
- **Repository สาธารณะ:** ใช้แต่ตัวอย่างสมมติ ห้ามเผย Ticket, Credentials, Code หรือ Architecture ภายใน

## ขอบเขตการอนุมัติ
FDL เป็น Baseline ที่ยอมรับสำหรับ **My Engineer** ครอบคลุมกิจกรรม ขอบเขตบทบาท Traceability และหลักฐานตามความเสี่ยง หมายเลขขั้นตอนเป็น **Reference Model ไม่ใช่ Pipeline บังคับ** สามารถรวม ย้อนกลับ หรือข้ามกิจกรรมที่ไม่เกี่ยวข้องอย่างมีเหตุผลได้ Template, Automation, Enforcement และ Integration ต้องผ่านการประเมินแยกต่างหาก

## แนวคิดหลัก
**การส่งมอบฟีเจอร์ที่ดีไม่จบที่ Code ถูก Merge** แต่ต้องรู้ว่าปัญหาคืออะไร โซลูชันเหมาะสมหรือไม่ ทำงานและปล่อยอย่างปลอดภัยหรือไม่ และผลลัพธ์ตรงความต้องการหรือไม่

กิจกรรมสามารถซ้อนทับและวนกลับได้ ไม่จำเป็นต้องทำเอกสารทุกชิ้นสำหรับทุกงาน ระดับหลักฐานควรสัมพันธ์กับ Complexity, Reversibility, Change Impact และ Risk

## กิจกรรมอ้างอิงตลอดวงจร
| กิจกรรม | คำถามสำคัญ | ผลลัพธ์ที่คาดหวัง |
| --- | --- | --- |
| **1. Requirement Discovery** | ใครมีปัญหาอะไร Outcome คืออะไร และจะยอมรับอย่างไร? | Problem, Scope, Acceptance Examples, Unknowns |
| **2. Technical Discovery** | ระบบจริงทำงานอย่างไร มี Constraint/Dependency/Risk อะไร? | Evidence-backed System Understanding |
| **3. Solution Design** | ทางเลือกไหนเหมาะกับ Constraint/Trade-off และตรวจสอบได้? | Design Decision, Interface/Invariants, Risks |
| **4. Delivery Planning** | จะแบ่งงาน เรียง Dependency และส่งมอบ Slice อย่างไร? | Work Slices, Owner, Verification, Risk |
| **5. Implementation** | แก้ Code อย่างมีวินัย รักษา Behavior เดิมและ Contract อย่างไร? | Focused Change พร้อม Traceability |
| **6. Verification** | อะไรพิสูจน์ความถูกต้อง Integration, Security และ Failure Cases? | Test/Evidence ตามความเสี่ยง |
| **7. Review & Integration** | Peer/Developer เห็นปัญหาอะไร Diff รวมกับระบบได้หรือไม่? | Reviewed, Integrated Change |
| **8. Release & Operational Readiness** | Deploy, Rollback, Migration, Monitoring และ Approval พร้อมหรือไม่? | Controlled Release |
| **9. Outcome & Learning** | ส่งมอบคุณค่าจริงหรือไม่ และจะปรับปรุงอย่างไร? | Observed Outcome, Feedback, Learning |

นี่คือแผนที่กิจกรรม ไม่ใช่ประตูอนุมัติทั้งหมดที่ต้องบังคับทุกงาน

## หลักการดำเนินงาน
1. **Outcome มาก่อน Output** — ทำ Code เสร็จไม่ได้แปลว่าปัญหาผู้ใช้ถูกแก้
2. **Evidence มาก่อนความมั่นใจ** — ระบุสิ่งที่เห็นจริง สมมติ และยังไม่รู้
3. **Risk-proportional** — งานเล็กไม่ควรแบกพิธีการของงานใหญ่ แต่งานเสี่ยงต้องมีหลักฐานเข้มขึ้น
4. **Iterative Delivery** — ทดลอง เรียนรู้ และวางแผนใหม่ได้โดยไม่ต้องเริ่มเอกสารทุกชิ้นตั้งแต่ต้น
5. **Human Accountability** — AI ช่วยได้ แต่เจ้าของการตัดสินใจยังเป็นคน
6. **Traceability** — เชื่อม Requirement → Design → Work → Tests → Release/Outcome เท่าที่มีประโยชน์
7. **Safe Changes** — คิดถึง Regression, Data Integrity, Security, Operations และ Rollback
8. **One Source of Truth** — หลีกเลี่ยงการทำสำเนาข้อมูล Jira/Design หลายชุดจนไม่ตรงกัน

## บทบาทและขอบเขตการตัดสินใจ
- **Product Owner/Lead:** ปัญหา ผู้ใช้ Outcome, Priority และ Scope Trade-off
- **Engineers:** Discovery เชิงเทคนิค Design/Implementation, Tests และหลักฐานคุณภาพ
- **Tech Lead:** ความสอดคล้องด้านเทคนิค Risk/Dependency ข้าม Component, Coaching และ Escalation ไม่จำเป็นต้องอนุมัติทุกอย่าง
- **QA/Reviewers:** การตรวจสอบ Scenario, Evidence และ Failure Mode ที่สำคัญ
- **Engineering/Delivery Manager:** Capacity, Staffing และความขัดแย้งด้านทรัพยากรเมื่อเกี่ยวข้อง
- **Security/Operations/Service Owners:** Policy, Production Readiness และ Approval ที่จำเป็น

บทบาทหนึ่งคนสามารถรับหลายหน้าที่ใน Solo Project ได้ แต่ไม่ควรซ่อน Conflict ของการตัดสินใจ

## จุดตรวจที่ใช้อย่างยืดหยุ่น
- **เริ่ม Design:** เข้าใจปัญหาและ Unknowns สำคัญพอ พร้อมข้อมูล Technical Discovery ที่เชื่อถือได้
- **เริ่ม Slice แรก:** Design/Contract มีความชัดพอสำหรับส่วนเล็กที่ตรวจสอบได้
- **Merge:** Code, Tests, Review และการจัดการความเสี่ยงตามมาตรฐานที่ตกลง
- **Release:** Deployment/Recovery/Monitoring/Authorization พร้อมตามผลกระทบ
- **Close:** ตรวจผลลัพธ์หรือระบุวิธีติดตามผลหลังปล่อย

ไม่ควรใช้สถานะ Ready เพื่อแกล้งบอกว่าความไม่แน่นอนทั้งหมดหายไป

## รูปแบบตามขนาดงาน
| ขนาดงาน | วิธีใช้ |
| --- | --- |
| **Solo / Low risk** | Issue เดียวพร้อม Acceptance, การตรวจ Code/Test และสรุปสั้น |
| **Team / Moderate risk** | Shared Summary, Design Review ตามความจำเป็น, Work Slices และ Integration Checkpoints |
| **High risk / Multi-repo / Multi-team** | Evidence, Contract Owners, Migration/Rollback, Security/Data Invariants และ Cross-team Decisions ที่ชัดเจน |

## ใช้ AI อย่างรับผิดชอบ
AI ช่วยอ่านเอกสาร ตั้งคำถาม สำรวจ Code เสนอ Alternative, Implementation Plan, Tests และ Review ได้โดยต้อง **อ้างหลักฐานจริงและระบุข้อจำกัดของการตรวจ** ไม่ควรอ้างผล Test ที่ไม่ได้รัน แก้ระบบ Production หรือเผยข้อมูลลับโดยไม่ได้รับอนุญาต ไม่ให้ AI ตัดสินใจแทนเจ้าของธุรกิจหรืออนุมัติ Release

## ตัวอย่างสมมติ: ฟีเจอร์นำเข้า Statement
Discovery: พิสูจน์ว่าผู้ใช้ต้องการนำเข้าข้อมูลจริงหรือไม่ → Technical Discovery: ตรวจ Data Semantics และ Source → Solution Design: เปรียบเทียบ Import ตรง กับ Preview/Confirm หรือ Provider → Planning: แบ่ง Slice และ Validation → Implementation/Verification: ตรวจ Duplicate, Transfer, Refund, Partial Failure → Release: สิทธิ์ Data Retention, Monitoring และ Rollback → Outcome: วัดว่าผู้ใช้เข้าใจค่าใช้จ่ายดีขึ้นจริงหรือไม่

การยกตัวอย่างไม่ใช่การรับรอง Scope, Architecture หรือ Security ของ Product จริง

## การประเมินและปรับปรุง
ดู Lead/Cycle Time, Blockers, Defects/Rework, Critical Risks ที่พบก่อน Implement, คุณภาพของผลลัพธ์ และต้นทุนของพิธีการ อย่าใช้ Commit Count, Story Points หรือจำนวน Ticket จัดอันดับนักพัฒนา ระบุจุดที่ Workflow ทำให้งานช้าลง แล้วลดขั้นตอนที่ไม่เพิ่มคุณค่า

**สถานะ:** FDL v1.0 เป็น Accepted Baseline สำหรับ My Engineer โดยไม่มีผลอนุมัติ Skill, Automation หรือ Corporate Policy อื่นโดยอัตโนมัติ
