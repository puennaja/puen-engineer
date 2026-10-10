# กระบวนการสำรวจด้านเทคนิค (Technical Discovery) — v1.0

- **สถานะ:** Accepted — My Engineer Track A
- **อนุมัติ:** 2026-10-10 โดยเจ้าของ Repository
- **เวอร์ชัน:** 1.0
- **วันที่:** 2026-10-10
- **พื้นฐาน:** [FDL v1.0](./feature-delivery-lifecycle.th.md)
- **ก่อนหน้า:** [Requirement Discovery](./requirement-discovery.th.md)
- **ถัดไป:** Solution Design
- **ใช้ได้กับ:** คนเดียว ทีม และหลายทีม

## ขอบเขตการอนุมัติ
ข้ออ้างสำคัญเกี่ยวกับพฤติกรรมของระบบ Contract, Dependency และความเสี่ยง **ต้องตรวจสอบย้อนกลับถึงหลักฐานได้** เช่น Repo/Path/Symbol และ Revision, Test, Runtime Observation หรือคำยืนยันจากเจ้าของงาน ระบุสิ่งที่ยังไม่ได้ตรวจ และแยกข้อเท็จจริงออกจากการอนุมานและสิ่งที่ไม่รู้ ใช้หลักฐานให้เหมาะกับผลกระทบ ไม่ต้องอ้างทุกประโยค

## วัตถุประสงค์
**ปัญหา:** การเลือกวิธี Implement ก่อนเข้าใจพฤติกรรมเดิม ขอบเขต Architecture, Data Semantics และ Operational Constraints ทำให้พลาด Dependency เกิด Regression และวางแผนผิด

**เป้าหมาย:** สร้างความเข้าใจด้านเทคนิคที่มีหลักฐานเพียงพอต่อการระบุ Constraint, Feasibility Risk, Boundary ที่ได้รับผลกระทบ และคำถามสำหรับ Solution Design โดย **ยังไม่เลือกโซลูชันสุดท้าย**

## Input และวิธีเริ่ม
เริ่มจาก Discovery Summary หรือข้อความสั้น ๆ ที่มีปัญหา ผู้ใช้ Outcome, Scope, Constraints, Unknowns, Decision Owner และ Acceptance Examples ที่ทราบ

| Mode | ใช้เมื่อ | สำรวจอะไร |
| --- | --- | --- |
| **Brownfield** | เปลี่ยนระบบเก่าหรือระบบที่ใช้งานจริง | พฤติกรรมปัจจุบัน, Call/Data Flow, Ownership, Contract, Coupling, Regression |
| **Greenfield** | ยังไม่มีระบบ Production หรือ Architecture เดิม | Capability, Constraint, NFR, Feasibility, Build-vs-Buy, PoC |
| **Hybrid / Integration** | เชื่อมระบบเดิมกับ Component หรือ Third Party | Boundary, External Contract, Permission, Data Movement, Failure/Retry |

ห้ามสร้างภาพ As-is ปลอมสำหรับ Greenfield หรือสรุปว่าต้องเป็น Microservices เพียงเพราะเริ่มโปรเจกต์ใหม่

## หลักการ
1. **สังเกตก่อนสมมติ** — Code, Schema, Test, Runtime, เอกสารและคน ล้วนเป็นหลักฐานคนละประเภท ตรวจความสดและความน่าเชื่อถือ
2. **ตามคำถาม ไม่กวาดทั้ง Repo** — เริ่มจาก Entry Point ที่เกี่ยวข้องและขยายเมื่อพบ Dependency
3. **หลักฐานไม่เท่ากับข้ออนุมาน** — อ้าง Path/Symbol, Revision, Test หรือ Measurement สำหรับข้อสรุปสำคัญ
4. **Read-only ก่อน** — ไม่แก้ Source, Migration, Secret หรือ Production Write โดยพลการ
5. **สัดส่วนตามความเสี่ยง** — งานเล็กใช้ Note สั้นได้ งานข้าม Repo หรือข้อมูลอ่อนไหวต้องสำรวจลึกขึ้น
6. **อย่ารีบล็อก Solution** — ระบุ Constraint และ Option แต่เก็บการเลือกไว้ใน Solution Design
7. **วนซ้ำได้** — ผลสำรวจอาจย้อนกลับไปหา Requirement หรือสร้าง Spike

## 5 กิจกรรม
| กิจกรรม | คำถาม | หลักฐาน/ผลลัพธ์ |
| --- | --- | --- |
| 1. Orient & Scope | เปลี่ยน Capability ไหน? ระบบ/เจ้าของ/Environment ใดเกี่ยวข้อง? | ขอบเขตการสำรวจและ Critical Unknowns |
| 2. Trace Behavior / Capabilities | ข้อมูลเข้า ประมวลผล เก็บ และออกทางใด? หรือ Greenfield ต้องพิสูจน์อะไร? | As-is Trace หรือ Capability/Constraint Map |
| 3. Dependencies & Contracts | API, Event, Schema, Permission, Other Repo และ Owner ใดเกี่ยวข้อง? | Dependency/Contract/Data Semantics |
| 4. Impact & Risk | มี Regression, Concurrency, Security, Performance, Migration หรือ Operation Risk ไหม? | Blast Radius, Test/Release Implications |
| 5. Synthesize & Route | อะไรยืนยันแล้ว อะไรต้องทดลอง? | Technical Discovery Summary และ Next Action |

### แนวทาง Brownfield
1. เลือก User Journey หนึ่งกรณี แล้วหา Entry Point จริง
2. ไล่ UI/API → Validation → Authorization → Business Logic → Persistence → Async Job/External Call **เฉพาะส่วนที่มีจริง**
3. ตรวจ Test, Configuration และ Runtime เพื่อหา Behavior ที่ Code อย่างเดียวไม่แสดง
4. เปิด Repo อื่นเฉพาะเมื่อหลักฐานชี้ว่ามี Dependency และระบุ Repo ที่ **ยังไม่ได้ตรวจ**
5. หาผู้รับผิดชอบและความไม่ตรงกันระหว่างเอกสารกับพฤติกรรม
6. จด Repo/Path/Symbol พร้อม Commit/Revision แทนการ Copy Code ยาว ๆ

### แนวทาง Greenfield
1. ระบุ Capability, Quality Attributes, Business/Time/Team Constraints
2. ตรวจความเป็นไปได้ของ Data Source และ Integration Access
3. กำหนด Domain/Data Correctness Invariants เช่น Duplicate, Transfer, Privacy
4. ทำ Spike เฉพาะจุดด้วยข้อมูลสมมติ ไม่ออกแบบ Production Architecture ทั้งหมดก่อน
5. ส่ง Option และ Constraint ให้ Solution Design

## Artifact ที่จำเป็นเพียงชิ้นเดียว: Technical Discovery Summary
~~~markdown
# Technical Discovery — <Feature>
Status: Draft | Evidence sufficient for design | More exploration needed | Blocked
Mode: Brownfield | Greenfield | Hybrid
Owner / Reviewed by:
Scope / Linked problem:
Evidence snapshot: repos, branches/commits, environment, date

## Technical context
Boundary, Constraint, Owner และ Environment ที่เกี่ยวข้อง

## Observed behavior / feasibility
Brownfield: As-is Flow พร้อมหลักฐาน
Greenfield: Capability/Constraint ที่พิสูจน์แล้ว

## Dependencies & contracts
Interface, Schema, Integration, Data Semantics; Known vs Assumed

## Impact & risk
Blast Radius, Data Correctness, Privacy/Security,
Performance, Concurrency, Migration และ Operation ตามความจำเป็น

## Evidence ledger
Finding | Source/Path/Commit/Test/Owner | Confirmed/Inferred/Unknown

## Open questions
Question | Impact if wrong | Next validation | Owner

## Handoff decision
หลักฐานพอสำหรับเปรียบเทียบ Solution หรือไม่? เพราะอะไร?
Route: Solution Design | Spike | Requirement clarification | Blocked
~~~

งานเล็กอาจใช้ Comment สั้น ๆ ใน Issue; งานข้าม Component ต้องเห็น Interface; งานเสี่ยงสูงต้องครอบคลุม Invariants และ Failure Modes แผนภาพเป็นทางเลือก ไม่ใช่ข้อบังคับ

## เกณฑ์: มีหลักฐานเพียงพอสำหรับออกแบบ
1. รู้ว่าจะเปลี่ยน Behavior/Capability ไหน และตรวจอะไรจริงแล้ว
2. รู้เส้นทางข้อมูล หรือรู้ว่าเส้นทางใดของ Greenfield ต้องพิสูจน์
3. เห็น Dependency, Owner และ Blast Radius ที่เป็นไปได้
4. ระบุ Critical Unknowns ด้าน Security/Privacy/Data/Operation
5. มีข้อมูลพอเปรียบเทียบแนวทาง หรือควรทำ Spike/Clarification ก่อน

ถ้า Unknown สำคัญสามารถล้ม Design ได้ อย่าตีตราว่า Evidence Sufficient เว้นแต่มี Mitigation หรือ Next Validation พร้อม Owner

## บทบาท
- **Solo:** สำรวจในขอบเขตจำกัดและตรวจ Assumption เอง
- **Team:** Feature Engineer ไล่ Flow; Tech Lead ดูข้าม Boundary; QA/Ops ดูความเสี่ยง; Owner ยืนยัน Contract
- **Multi-team:** Service/Domain Owners ทำ Dependency Map ร่วมกัน และมีผู้ตัดสินใจด้านเทคนิคที่ระบุชื่อ

Tech Lead ไม่ต้องอ่านทุกไฟล์หรืออนุมัติ Discovery ความเสี่ยงต่ำทุกครั้ง

## ตัวอย่างสมมติ: แอปการเงินส่วนบุคคล
**เป้าหมายผู้ใช้ (สมมติฐาน):** เห็นการใช้เงินโดยทำงานมือให้น้อยลง และประเมินเงินที่ใช้ได้หลังหักภาระที่จะถึง

**Brownfield:** ตรวจ Request Path จริง, Transfer ที่ถูกนับเป็น Expense หรือไม่, Refund/Pending Transaction, Date/Timezone/Currency, Duplicate Import และ Storage Guarantees **ไม่ได้อ้างว่ามี Code จริง**

**Greenfield:** พิสูจน์ Format ของ Statement และ Permission, Parser แยก Purchase/Transfer/Refund/Fee ได้หรือไม่, Privacy/Storage Minimization และความหมายของข้อมูลเงินสดที่ไม่ครบ

**Spike:** ทดลอง Parser กับไฟล์ CSV สมมติ แล้วบันทึกความแม่นยำและ Format ที่ไม่รองรับ ไม่ใช่การอนุมัติให้ Ship CSV Import

## การใช้ AI (ยังไม่สร้าง Skill)
AI อาจช่วยหา Entry Point สรุป Code Path พร้อมอ้างไฟล์ เปรียบเทียบ Contract ทำ Risk Checklist และชี้ Test Gap ภายใต้สิทธิ์ที่เหมาะสม **ห้าม** แก้ Repo, ใช้ Production Data อ่อนไหว, รันคำสั่งทำลาย, ส่ง Code ลับเข้า Third Party หรืออ้างว่า Test ผ่านโดยไม่ได้รันจริงถ้าไม่ได้รับอนุญาต

## Pilot และการปรับปรุง
ทดลองงาน Brownfield ที่ย้อนกลับง่ายกับคำถาม Feasibility แบบ Greenfield ดูเวลาในการตอบคำถามสำคัญ สัดส่วน Claim ที่มีหลักฐาน Dependency ที่พบก่อน Design, Rework และภาระเอกสาร อย่าเหมารวมผลผลิตจากการทดลองครั้งเดียว

## คำถามหลังอนุมัติ
1. Summary เดียวเพียงพอสำหรับ Solo/Team/Multi-repo หรือไม่?
2. Evidence Ledger ควรบังคับทุก Finding หรือเฉพาะเรื่องสำคัญ?
3. งานเสี่ยงปานกลางต้องอธิบาย Dependency/Impact ขั้นต่ำแค่ไหน?
4. เมื่อใดควรทำ Spike แทนการอ่านเอกสารเพิ่ม?
5. Greenfield Mode แยกจาก Solution Design และ Product Discovery ชัดหรือยัง?

**สถานะ:** Accepted Baseline; ยังเปิดให้ปรับจาก Pilot และไม่มีการอนุมัติหรือสร้าง Skill ใหม่ใน `puen-stack`
