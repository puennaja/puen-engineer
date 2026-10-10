# กระบวนการออกแบบวิธีแก้ปัญหา (Solution Design) — v1.0

- **สถานะ:** Accepted Baseline — My Engineer
- **Track:** A — Build My Engineer
- **พื้นฐาน:** [FDL](./feature-delivery-lifecycle.th.md)
- **ก่อนหน้า:** [Requirement Discovery](./requirement-discovery.th.md), [Technical Discovery](./technical-discovery.th.md)
- **ถัดไป:** [Delivery Planning](./delivery-planning.th.md)
- **ขอบเขต:** Solo → Team → Product / Multi-team
- **ตัวอย่าง:** แอปการเงินส่วนบุคคลสมมติ ไม่มีข้อมูลบริษัท

## จุดประสงค์
เปลี่ยนปัญหาและ Technical Evidence ให้เป็น **แนวทางออกแบบที่อธิบายเหตุผล ตรวจสอบได้ และรับผิดชอบความเสี่ยง** หลีกเลี่ยงการข้ามไปเขียน Code ทันที หรือเลือก Architecture ตามความชอบส่วนตัว

Solution Design ไม่จำเป็นต้องหมายถึง Diagram ขนาดใหญ่ และไม่ใช่การเลือก Stack ใหม่ทุกครั้ง งานเล็กอาจเพียงสรุปการตัดสินใจสั้น ๆ ใน Ticket

## Inputs
- Problem, Outcome, Scope, Acceptance Examples และคำถามที่ยังเปิดอยู่
- Technical Discovery: Verified Behavior, Constraints, Contracts, Dependencies, Risks และ Evidence
- Quality Attributes ที่มีผลจริง เช่น Reliability, Privacy/Security, Performance, Maintainability, Operability
- Ownership/Decision Rights และเงื่อนไข Release/Migration

อย่านำสิ่งที่ยังเป็นสมมติฐานจาก Discovery ไปเขียนให้ดูเหมือนข้อเท็จจริง

## หลักการ
1. **แก้ปัญหาที่ตกลงกันแล้ว** ไม่เพิ่ม Scope เงียบ ๆ
2. **พิจารณาทางเลือกจริง** รวมถึงไม่ทำอะไรหรือใช้วิธีที่ง่ายกว่า
3. **ทำ Trade-off ให้ชัด** ทั้ง Complexity, Cost, Security, Data Correctness และ Operations
4. **วาง Contract/Invariant ก่อนรายละเอียด Code** เมื่อมี Boundary ข้าม Component
5. **ออกแบบเพื่อทดสอบและเปลี่ยนแปลงได้** พิจารณา Failure, Concurrency, Migration, Backward Compatibility
6. **หลักฐานตามความเสี่ยง** งานย้อนกลับง่ายใช้ Design Note; งานมีผลสูงต้องมี Decision/Evidence มากขึ้น
7. **ให้มนุษย์ตัดสินใจ** AI เสนอและวิจารณ์ได้ แต่ไม่เป็น Owner ของ Design

## กิจกรรมออกแบบ
| กิจกรรม | คำถาม | ผลลัพธ์ |
| --- | --- | --- |
| ยืนยันกรอบปัญหา | ต้องได้ Outcome ไหน Scope/Constraint ใดห้ามละเมิด? | Design Context |
| สร้าง Candidate Options | มีแนวทางง่ายกว่า ปลอดภัยกว่า หรือใช้ของเดิมได้หรือไม่? | Options ที่เป็นไปได้ |
| เปรียบเทียบ Trade-offs | แต่ละแนวทางกระทบ Quality, Complexity, Change Cost และ Risk อย่างไร? | Reasoned Comparison |
| ลงรายละเอียด Boundaries | API/Event/Schema, Data Invariants, Auth, Error/Retry และ Compatibility อะไรสำคัญ? | Contract/Boundary Decisions |
| วาง Verification & Rollout | Test อะไรพิสูจน์ได้ และจะ Deploy/Migrate/Rollback อย่างไร? | Testable & Operable Design |
| Review & Decide | ใครตัดสินใจ Unknown ที่มีผลสูงต้องทดลองก่อนหรือไม่? | Design Summary / Spike / Return to Discovery |

กิจกรรมเหล่านี้สามารถวนซ้ำ หรือลงมือ Spike ก่อนเลือก Design ได้

## ควรเปรียบเทียบอะไร
- **Correctness & Domain Semantics:** ข้อมูลซ้ำ Missing/Partial Data, State Transitions, Transfer/Refund
- **Contract & Compatibility:** API Schema, Consumer Impact, Backward Compatibility และ Version
- **Failure & Recovery:** Timeout, Retry, Idempotency, Partial Commit, Rollback
- **Security & Privacy:** Authorization, Sensitive Data, Retention และ Audit
- **Performance & Operations:** Load, Observability, Monitoring, Deployment/Migration
- **Maintainability & Cost:** Complexity, Coupling, Testability, Future Changes และ Ownership

ไม่ต้องติ๊กทุกหัวข้อเพื่อให้ดูครบ ให้ลงรายละเอียดเฉพาะความเสี่ยงที่มีผลจริง และอธิบายเรื่องสำคัญที่ตัดออก

## Artifact หลัก: Design Summary
~~~markdown
# Solution Design — <Feature>
Status: Draft | Accepted for planning | Spike needed | Blocked
Owner / Decision maker / Reviewers:
Related: Requirement Discovery, Technical Discovery
Evidence snapshot:

## Problem and constraints
Outcome, Scope, NFRs, Known/Unknown

## Options considered
Option | Benefits | Costs/Risks | Supporting evidence

## Proposed direction and rationale
แนวทางที่แนะนำ เหตุผล และทางเลือกที่ไม่เลือก

## Boundaries & invariants
Interfaces, API/Event/Schema, Data Flow,
Ownership, Auth, Consistency, Error/Retry, Compatibility

## Verification and release implications
Unit/Integration/Contract/Failure Tests,
Migration, Deployment, Monitoring, Rollback

## Risks and open decisions
Issue | Consequence | Next experiment / mitigation | Owner

## Decision / handoff
Accepted for planning | Design spike | Return to discovery | Defer
~~~

**ADR** ใช้เมื่อเป็น Decision สำคัญระยะยาวหรือมีผลข้าม Boundary ที่ควรบันทึก Context/Alternatives/Consequences ถาวร ไม่ใช่ทุกการแก้ Code ต้องมี ADR

## Quality Gate: ยอมรับเพื่อวางแผนได้
1. Outcome และ Constraint ที่สำคัญได้รับการเคารพ
2. มีการเปรียบเทียบทางเลือกในระดับที่เหมาะกับความเสี่ยง
3. Boundary, Contract และ Invariant ที่สำคัญชัดพอ
4. Failure, Privacy/Security, Compatibility, Data และ Operational Risk ถูกจัดการหรือมี Owner
5. รู้วิธีพิสูจน์ความถูกต้องและ Slice ปลอดภัยที่เล็กที่สุด
6. รู้ผู้ตัดสินใจและ Unknown ที่ยังต้องตามต่อ

**ผลลัพธ์:** Accepted for Planning, Design Spike, Return to Discovery, หรือ Defer/Reject โดยบันทึกเหตุผล Review ไม่จำเป็นต้องเป็น Ceremony สำหรับงานเล็ก แต่ High-risk Change อาจต้องผ่าน Policy จริง

## บทบาทตามขนาดงาน
| แบบ | วิธีทำ |
| --- | --- |
| **Solo** | เปรียบเทียบสั้น ๆ เช็ก Assumption/Testability เอง ขอ Review เพิ่มเมื่อเสี่ยง |
| **Team** | Implementer เสนอ Tech Lead/Peers ช่วย Review, Owner ของ Contract อนุมัติ Boundary Changes |
| **Product/Multi-team** | มี Design Owner/Decider และผู้เชี่ยวชาญ Platform/Security/Data/Ops ตามความเสี่ยง |

Tech Lead จัดการ Trade-off ทางเทคนิค ไม่ได้ตัดสินใจ Priority/Staffing หรือทุก Change เพียงลำพัง

## ตัวอย่างสมมติ: นำเข้า Statement การเงิน
**ปัญหาออกแบบ:** จะนำ Transaction มาเป็นข้อมูลที่เชื่อถือได้อย่างไรโดยไม่ Double-count Transfer หรือ Upload ซ้ำ?

| ตัวเลือก | ข้อดี | ความเสี่ยง/Trade-off |
| --- | --- | --- |
| **A. Upload แล้ว Parse ตรงเข้า Canonical Transactions** | Surface เริ่มต้นเล็ก Prototype เร็ว | Partial Failure/Mapping Error อาจทำให้ยอดรวมผิด และแก้ Duplicate ยาก |
| **B. Upload → Validate/Preview → Confirm Import** | ผู้ใช้ตรวจ Errors/Classification ก่อน Commit Audit/Rollback Boundary ชัด | UX และ State Complexity เพิ่ม |
| **C. ใช้ Managed Provider** | อาจรองรับหลาย Format/Normalization | Privacy, Cost, Availability, Integration, Data Ownership |

**สมมติฐานชั่วคราว:** B อาจช่วยเรื่อง Trust/Error Prevention แต่ **ยังไม่เลือก** จนพิสูจน์ Usability, Security และ File-format Feasibility

Invariants ที่ต้องตรวจ: Import ซ้ำไม่นับยอดซ้ำ, โอนบัญชีตัวเองไม่ใช่ Expense/Income อัตโนมัติ, Refund/Pending/Fee มีความหมายต่างกัน, Total ต้องบอกข้อจำกัดของข้อมูลเงินสด, Failed Import ไม่ทิ้ง Partial State แบบไร้คำอธิบาย, Statement อ่อนไหวต้องมี Access/Retention/Deletion Policy

**การทดลองเล็กถัดไป:** ใช้ CSV สมมติที่มี Duplicate, Transfer, Refund และ Malformed Row เปรียบเทียบวิธีรับข้อมูลและความยุ่งยากของ User Review ไม่อัปโหลด Statement จริงเข้า Repo สาธารณะ

## AI (ยังไม่สร้าง Skill ใหม่)
ช่วยเสนอ Option, ท้าทาย Assumption, ทำ Comparison, เชื่อม Technical Evidence, เสนอ Edge Cases และ Review Design Gap ได้ คนยังต้องยืนยัน Claim, Security/Privacy และ Approvals; AI ห้ามแต่ง Performance/Cost Data หรือรัน Mutation ที่ไม่อนุญาต

## Pilot และการปรับปรุง
ทดลองทั้ง Low-risk Local Change และ Medium/High-risk Fictional Import ดูว่าค้นพบ Data Integrity/Privacy/Migration/Testability/User Friction ก่อน Code หรือไม่ อย่าสรุป Productivity Improvement โดยไม่มีหลักฐาน

**คำถามที่เปิดอยู่:** Summary เดียวพอหรือไม่? Threshold ADR เหมาะสมไหม? Quality Gate เพียงพอสำหรับ High-risk หรือยัง? Accepted for Planning ต่างจาก Ready for Implementation ชัดไหม? ใช้ได้ทั้ง Brownfield/Greenfield/Multi-team หรือไม่?

**สถานะ:** Accepted Baseline; คำถามเปิดไว้เพื่อปรับจาก Pilot ตัวอย่างไม่ใช่ Requirement หรือ Architecture ที่ยืนยันแล้ว
