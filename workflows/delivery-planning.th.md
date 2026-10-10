# กระบวนการวางแผนส่งมอบ (Delivery Planning Workflow) — v1.0

- **สถานะ:** Accepted — เจ้าของ Repository อนุมัติแล้วเมื่อ 2026-10-10
- **แนวทางที่ตกลง:** Hybrid — Full Scope, Progressive Detail เป็น Default (2026-10-10)
- **ฉบับอังกฤษหลัก:** [Delivery Planning (English)](./delivery-planning.md)
- **วันที่:** 2026-10-10
- **Track:** A — Build My Engineer
- **พื้นฐาน:** [Feature Delivery Lifecycle v1.0](./feature-delivery-lifecycle.th.md)
- **ก่อนหน้า:** [Requirement Discovery](./requirement-discovery.th.md), [Technical Discovery](./technical-discovery.th.md), [Solution Design](./solution-design.th.md)
- **ถัดไป:** [Implementation v0.1 Draft](./implementation.th.md) → [Verification v0.1 Draft](./verification.th.md) (ทำวนซ้ำได้)
- **ขนาดงาน:** Solo → Team → Product / Cross-team
- **ตัวอย่าง:** แอปการเงินส่วนบุคคลสมมติ ไม่มีข้อมูลลับบริษัท

## 1. ทำไมต้องมี Workflow นี้
**ปัญหา:** แม้เข้าใจ Solution แล้ว งานก็อาจล้มเหลวเพราะ Task ใหญ่เกิน Dependency ซ่อนอยู่ ไม่มีเจ้าของการตัดสินใจ Estimate ไม่สมจริง รับงานเกินกำลัง Verify ไม่ชัด หรือ Blocker ไม่ถูกเปิดเผย บางทีมเข้าใจผิดว่าการเติม Sprint ให้เต็มเท่ากับมีแผนที่ดี

**เป้าหมาย:** เปลี่ยน Solution Direction ให้เป็นแผนที่ทำได้จริงและปรับตัวได้ ผ่าน Increment เล็ก ๆ มี Owner, Sequence, Capacity Assumptions, Verification และ Risk Response

**ไม่ใช่เป้าหมาย:** บังคับ Scrum, Sprint ระยะตายตัว, Story Points, Schedule ละเอียดยิบ หรือ Commitment ที่ AI สร้างขึ้น Product Leadership ดูคุณค่า/Priority; Delivery Team ดู Feasibility/Execution

**ข้อแตกต่างสำคัญ:** Planning เกิดต่อเนื่อง แผนคือการตัดสินใจที่ดีที่สุด ณ ปัจจุบันจาก Assumptions ที่รู้ ไม่ใช่สัญญาว่าความไม่แน่นอนหมดไปแล้ว

## 2. Inputs และขอบเขตการวางแผน
Input อ้าง Link ได้ ไม่ต้อง Copy:
- Outcome, Scope, Acceptance Criteria หรือช่องว่างที่ระบุชัด
- Solution Direction, Design Constraints, Contracts, Data Invariants และ Tests
- Technical Dependencies, Risks, Unknowns และ Spike Results
- Priority/Desired Timing จาก Requester/Product Owner และ Capacity/Availability หากเกี่ยวข้อง
- บริบท: Solo/Team, Release Restrictions, Cross-team Ownership และ WIP

ถ้า Unknown สำคัญทำให้ประเมิน Effort ไม่ได้ หรือเปลี่ยน Scope อย่างมีนัยสำคัญ ให้กำหนด **Discovery/Spike Task ที่มีขอบเขต** ก่อน Commit งานทั้งหมด ห้ามแปลง Discovery ที่อ่อนแอเป็น Estimate ที่ดูแม่นยำเกินจริง

## 2A. แนวทางระดับความละเอียดของแผน — Hybrid: Full Scope, Progressive Detail

**ค่าเริ่มต้น:** รู้ภาพรวมของ Feature ทั้งหมดในระดับที่มีประโยชน์ ได้แก่ Outcome, ขอบเขต, Dependencies สำคัญ, Contract ข้าม Repo, ความเสี่ยง Integration/Release และผู้ตัดสินใจ แต่แตกงาน **ละเอียดเพิ่มตามความเสี่ยง ความไม่แน่นอน และความใกล้ที่จะลงมือทำ** ไม่ต้องแตกทุก Subtask ของ Increment ไกล ๆ ก่อนเริ่มงาน Increment แรกที่พร้อมแล้ว

แบ่งเป็นสามระดับ:

| ระดับ | สิ่งขั้นต่ำที่ควรรู้ | เมื่อไรต้องลงรายละเอียดเพิ่ม |
| --- | --- | --- |
| **Feature / End-to-end** | Outcome, Acceptance Boundary, Component ที่รู้/ยังไม่รู้, Cross-repo Contract, Dependency, Owner, Risk และแผน Integration/Release | มีหลายทีม กำหนด Cutover, Compliance หรือการเปลี่ยนย้อนกลับยาก |
| **Increment ที่จะส่งมอบ** | Acceptance/Learning Goal, Owner, Interface, Dependency, Verification และผลต่อ Rollback/Recovery | งานใกล้เริ่มหรืองานที่มีความเสี่ยงสูง |
| **Task ที่ใช้ทำจริง** | ขั้นตอนและ Done Evidence เท่าที่ช่วยลงมือหรือประสานงาน; ไม่เดาชื่อไฟล์ที่ยังไม่ตรวจโค้ด | งานที่เริ่มแล้ว Hand-off ระหว่างคน หรือ Migration ที่มีเงื่อนไข |

**Full Breakdown ทำได้และอาจเหมาะกว่า** กับงานเล็ก ชัดเจน หรืองานที่มีข้อจำกัดภายนอกสูง แต่ไม่บังคับแตก Task ละเอียดสำหรับงานไกลที่ยังเปลี่ยนได้ ขณะเดียวกันการเห็น Feature ครบก็ไม่ใช่คำรับรองว่ารู้ทุกอย่าง

### ตารางเลือกระดับความละเอียด

| Risk / Uncertainty | วิธีวางแผน | สิ่งที่ต้องรักษา |
| --- | --- | --- |
| ต่ำ Requirement นิ่ง ย้อนกลับง่าย | Full Breakdown ได้ทันที อาจใช้เพียง Issue เดียว | Acceptance และ Verification ชัด |
| ปานกลาง / รายละเอียดเปลี่ยนได้ | เห็น Scope ทั้ง Feature; แตก Increment แรกละเอียด; งานหลังเป็น Outline | ระบุ Unknowns และจุดทบทวน |
| สูง / หลายทีม / Migration / Cutover | วาง Dependency, Contracts, Release/Rollback และ Ownership **ทั้ง Feature ให้ละเอียดพอ**; ค่อยแตก Subtask ทีละช่วง | ห้ามข้าม Critical Safety/Coordination Dependency ที่ยังไม่เคลียร์ |
| ไม่แน่นอนสูงจนกระทบ Solution/Scope | ทำ Spike/Discovery แบบจำกัดขอบเขตก่อน | ไม่สร้าง Estimate หรือ Task Count ปลอม |

**เกณฑ์พิจารณา Risk:** ผลกระทบเมื่อผิดพลาด, Reversibility, Privacy/Security/Compliance, Data Correctness, จำนวน Integration, Dependencies ภายนอก, วันที่ผูกมัด และคุณภาพ Tests/Monitoring งานที่ดูเล็กอาจมี Risk สูงได้

**เมื่อไรต้อง Replan:** พบ Dependency ใหม่ที่ยืนยันแล้ว, Contract เปลี่ยน, ผล Spike, Test/Acceptance ไม่ผ่าน, Priority/Capacity เปลี่ยนมาก หรือพบ Migration/Production Risk ให้ปรับใน Shared Work View เดิมและแจ้ง Decision Owner

**Anti-patterns:** แตกทุก Task จากการเดา; ใช้ Rolling-wave โดยไม่เห็น Dependency/Release ทั้งระบบ; ไม่ยอมเริ่ม Spike ปลอดภัยจนกว่าจะแตก Ticket ครบ; ผลัก Test/Operational Work ไปทำตอนท้ายโดยไม่วางแผน

## 3. 7 กิจกรรมที่ทำซ้ำเมื่อสถานการณ์เปลี่ยน
| กิจกรรม | คำถาม | ผลลัพธ์ที่พอดี | ความเสี่ยงที่ลด |
| --- | --- | --- | --- |
| **1. ยืนยันเป้าหมายส่งมอบ** | Outcome, Acceptance, Slice ที่มีคุณค่าที่สุดคืออะไร? Scope/Date/Capacity/Quality ส่วนไหนต่อรองได้? | Goal, First Slice, Constraints | ทำสิ่งที่ไม่มีใคร Accept |
| **2. แบ่งงานเป็น Slice** | Increment แบบ End-to-end ที่ตรวจได้คืออะไร? ต้องรวม Test/Monitoring/Migration อะไร? | Small Vertical Slices | งานใหญ่ที่ Integrate ช้า |
| **3. วาง Dependency/Sequence** | อะไรต้องรออะไร Contract/Approval ไหนจำเป็น? อะไรทำขนานได้? | Dependency/Critical Path, Integration Milestones | Blocker และ Cross-repo Surprise |
| **4. ประเมิน Unknown/Effort** | อะไรยืนยันแล้ว อะไรเดา? ควร Estimate ไหม Confidence/Assumption เป็นอย่างไร? | Relative Size/Range หรือเหตุผล No-estimate | False Precision |
| **5. เทียบ Capacity/Owner/Priority** | ใครทำ ตัดสินใจ รีวิว? งานค้าง วันลา Interrupt และ Bottleneck มีอะไร? | Plan ที่ทำได้จริงและ WIP เหมาะสม | Overcommitment |
| **6. ตกลง Delivery/Verification** | Merge, Test, Integrate, Deploy, Recover อย่างไร? Done คืออะไร? | Definition of Done, Test/Release Checkpoints | งาน Done แต่ใช้งานไม่ปลอดภัย |
| **7. Execute/Inspect/Replan** | อะไรเสร็จ Blocked หรือเปลี่ยน? Forecast ยังเชื่อถือได้หรือไม่? | Updated Work, Risks, Forecast, Decisions | แผนเก่าและ Status Theater |

กิจกรรม **ไม่ใช่ Waterfall** อาจ Slice → Test Assumptions → Implement → Learn → Refine สำหรับงานเล็กหนึ่ง Issue อาจครอบคลุมทุกกิจกรรมที่เกี่ยวข้องได้

## 4. แบ่งงานจากคุณค่าก่อน Component
ใช้ **Vertical Slicing** เป็นค่าเริ่มต้น: Increment เล็กที่รวมกันจริงและพิสูจน์ Outcome ของผู้ใช้หรือ Assumption ด้านเทคนิคได้ ไม่จำเป็นต้องปล่อยให้ลูกค้าทุกคนทันที แต่ต้อง Integrated และ Testable

อย่าใช้ “Backend Done”, “Frontend Done”, “QA Later” เป็นหน่วยความคืบหน้าหลักอย่างเดียว สิ่งเหล่านั้นเป็น Subtasks ใน Slice ได้ แต่ยังไม่รับประกันคุณค่าที่ผู้ใช้ได้รับ

Work Item ต้องตอบ:
- **Why/Outcome:** ส่งเสริม Acceptance หรือ Learning Goal ใด?
- **What/Boundary:** Behavior และ Exclusions อะไร เชื่อม Design ไหน?
- **Done Evidence:** Tests, Review, Integration/Demo ใดจำเป็น?
- **Owner:** ผู้รับผิดชอบหลักหนึ่งคน และ Collaborator/Reviewer ตามจำเป็น
- **Dependencies/Unknowns:** Blocker หรือ Decision อะไรต้องมาก่อน?

**Spike:** มีคำถามจำกัด มี Time/Effort Limit และต้องได้ Evidence/Decision ไม่ได้แปลว่าจะ Implement Prototype Code ต่อ

อย่าซ่อนงานด้าน Security, Testing, Migration, Observability, Accessibility, Documentation, Rollout และ Support จนหลัง Commit Capacity ไปแล้ว

## 5. Dependency และงานขนาน
เมื่อมีหลาย Repo/Team:
1. หา Feature-level Source of Truth และ Contract Owners
2. ระบุ Repo/Service/Contract เฉพาะที่มี Evidence สนับสนุน
3. ตกลง API/Event/Schema Compatible Changes, Producer/Consumer Ordering และ Integration Tests
4. วาง Deploy/Migration/Rollback เช่น Expand → Migrate → Contract
5. เปิดเผยการประสานงาน/Permission จากภายนอกเป็น Dependency ที่มี Owner
6. บอก Boundary ที่ยังไม่รู้แทนการแต่ง System Map ให้ครบ

ทำงาน Parallel อย่างปลอดภัยเมื่อ Interface Assumptions และ Integration Checkpoints ชัด อย่า Optimize ให้ทุกคนยุ่ง 100% จน Flow แย่

## 6. Estimation, Forecasting และ Capacity
**Estimate สื่อถึงความไม่แน่นอน ไม่ใช่คะแนนผลงานหรือการรับประกัน**

- **Solo/Small:** เรียง Priority, ใช้ Rough Size หรือ Timebox ไม่จำเป็นต้องมี Story Points
- **Team:** คุย Range อิงงานคล้ายกันที่เสร็จจริงและจด Assumptions Story Points เป็นทางเลือกเฉพาะทีม ไม่ใช่มาตรวัด Productivity
- **Product ขนาดใหญ่:** ใช้ Throughput/Cycle-time History, Probability Range, Capacity และ Cross-team Dependencies เพื่อ Forecast; แยก Forecast ออกจาก External Commitment

ถ้าไม่มี History ให้บอกตรง ๆ ใช้ Scenario/Range และ Early Checkpoint ห้ามแต่ง Estimate ตัวเลขหรือสมมติว่าชั่วโมงทำงานทั้งหมดเป็น Delivery Capacity

นับเวลา Review, Testing, Coordination, Interrupts, On-call, Leave และ Learning เก็บ Slack สำหรับ Unknown โดยเฉพาะ High-risk

**เมื่อมี Change:** ให้ Product Owner/Team เห็น Trade-off ของ Scope/Timing/Resources/Risk ชัด อย่าตัด Essential Quality แบบเงียบ ๆ เพื่อรักษาวันส่ง

## 7. ตัวปรับให้เข้ากับ Personal Flow, Scrum และ Kanban
| รูปแบบ | Planning/Inspection | สิ่งที่ควรเลี่ยง |
| --- | --- | --- |
| **Personal** | คิวเล็กตาม Priority, WIP ต่ำ, Self-review เมื่อ Assumption เปลี่ยน | เอกสาร Sprint ยาวสำหรับงานคนเดียว |
| **Scrum** | Sprint Goal, Backlog ที่พอดี, Daily Adaptation, Review, Retro; Scope ต่อรองร่วมกัน | นับ Story Points เป็น Commitment หรือหลักฐานคุณภาพ |
| **Kanban** | Visualize Flow, WIP Limit, Manage Blockers, Replenishment | ทำหลายงานพร้อมกันไม่จำกัด หรือคิดว่า Board คือ Process ทั้งหมด |
| **Multi-team/Product** | เชื่อม Team Execution กับ Roadmap, Shared Contracts, Owners, Dependency/Risk Reviews | Micromanagement กลางและ Precision ปลอม |

Ceremony เป็นเครื่องมือประสานงาน ไม่ใช่กิจกรรม Engineering สากล ไม่มี Sprint Length บังคับ

## 8. Artifact หลัก: Delivery Plan / Shared Work View
**ผู้ใช้งาน:** Engineer รู้ Next Action/Acceptance; Reviewer/QA รู้ Verification; Tech Lead เห็น Risk/Dependency; Product Lead เห็น Slice Outcome/Trade-off; Owner ที่เกี่ยวข้องรู้ Decision ที่ต้องทำ

เลือก Issue Tracker ที่ทีมใช้อยู่และ Link ไป Summary ก่อนหน้า ไม่จำเป็นต้องมี Markdown Plan ยักษ์ที่ซ้ำข้อมูลกัน งาน Solo อาจใช้ Issue และ Subtasks ไม่กี่รายการ

~~~markdown
# Delivery Plan — <Feature>
Status: Proposed | Agreed | Active | Replanning | Complete
Links: requirement, technical discovery, solution design
Delivery goal / current smallest valuable increment:
Product decision owner / engineering delivery owner:
Time/capacity assumptions (if applicable):

## Planning Depth และ Checkpoint ถัดไป
Mode: Hybrid — Full Scope, Progressive Detail (Default); ข้อยกเว้นต้องมีเหตุผล
Scope ทั้ง Feature, Unknowns สำคัญ, Cross-team Integration/Release Dependencies:
Increment ที่แตกละเอียดและพร้อมเริ่ม / จุดทบทวน:
Increment ภายหลัง (Outline, Assumptions, Trigger สำหรับ Refine):

## Work slices
Slice | Outcome / Acceptance | Owner | Dependencies | Verification | Status

## Integration and release
Contracts, Integration Checkpoints, Migration/Rollout Ordering

## Estimates / forecast (optional)
Method, Range/Confidence, Assumptions, Next Review Point
อย่าสร้างความแม่นยำของตัวเลขขึ้นมาเอง

## Risks & active blockers
Risk / Consequence | Mitigation / Next Experiment | Owner | Review Trigger

## Decisions and changes
Scope/Time/Capacity Trade-offs, Decider, Date, Rationale

## Progress / outcome
อะไร Integrated/Verified แล้ว? Next Deliverable? อะไรเปลี่ยน?
~~~

แยก Ticket เฉพาะเมื่อต้องช่วย Ownership, Execution หรือ Traceability และรักษา Source of Truth เดียวที่เชื่อมถึงกัน

## 9. เกณฑ์แผนพร้อมเริ่มและ Checkpoints
เริ่ม Increment ปลอดภัยถัดไปได้เมื่อ **สำรวจภาพรวมและ Critical Risks ของ Feature แล้ว** และ:
1. Increment ที่มีคุณค่าหรือทำให้เรียนรู้ พร้อม Acceptance/Learning Goal ชัด
2. Owner, Reviewer, Dependencies และ Major Unknowns มองเห็น
3. Solution/Contract Boundary เข้าใจพอสำหรับ Increment นี้
4. มี Verification, Integration และ Release Implications ตามเหมาะสม
5. Capacity/Timing Claim (ถ้ามี) มี Assumption และ Uncertainty
6. Critical Scope/Technical Conflict มีผู้ตัดสินใจหรือ Bounded Follow-up
7. ข้อจำกัดสำคัญด้าน Integration, Migration, Security และ Rollout ของงานทั้ง Feature ปรากฏชัด แม้ Task ภายหลังยังเป็น Outline
8. ระบุ Planning Mode และ Trigger สำหรับแตก Increment ถัดไป

**ผลลัพธ์:** Agreed for First Increment / Start with Spike / Return to Solution-Technical-Requirement Discovery / Defer or De-scope

Checkpoint นี้ไม่รับรองว่าทั้ง Product พร้อม Implement; Code Review, Tests, Safety และ Authorized Deployment ยังมี Gates ของตัวเอง

## 10. ผู้คนและขอบเขตการตัดสินใจ
- **Product Owner/Lead:** Business Priority, Value, Scope Trade-offs และ Outcomes
- **Delivery Team/Engineers:** Decomposition, Technical Estimates, Implementation, Quality
- **Tech Lead:** Cross-component Sequence, Risk, Design Coherence, Coaching/Escalation ไม่เป็น Sole Approver
- **Engineering/Delivery Manager:** Staffing, Availability, Competing Obligations และ People Support
- **QA/Security/Ops/Service Owners:** Verification, Policy, Release Readiness ตามเรื่องที่เกี่ยวข้อง

หนึ่ง Action ต้องมี Accountable Owner แต่ไม่ใช่ว่าคนนั้นทำทุกอย่างเอง เปิดเผย Dependency Conflict และกำหนด Decider ตั้งแต่เนิ่น ๆ ใช้ Progress บอก Flow/Outcome/Blocked Work ไม่จัดอันดับคนด้วย Tickets, Hours, Points หรือ Commits

## 11. ตัวอย่างสมมติ: แอปการเงินส่วนบุคคล
**Candidate Change:** นำเข้า Bank Statement เพื่อให้เห็น Monthly Spending ตัวอย่างนี้ไม่ใช่ MVP Scope หรือ Architecture ที่ตรวจยืนยันแล้ว

**Assumption สำหรับฝึกวางแผน:** Design Review ก่อนหน้าเลือก CSV Import พร้อม Preview/Confirmation ชั่วคราวสำหรับทดลองแรก แต่ Format ที่รองรับและความต้องการจริงยังไม่พิสูจน์

Vertical Slices ตัวอย่าง:
1. **Feasibility Spike:** ใช้ Synthetic CSV ตรวจ Date, Amount, Malformed Rows และ Format Variants; ได้ Decision Evidence
2. **Preview Increment:** Parse Format สมมติที่รองรับ แสดง Normalized Transactions/Errors โดย **ยังไม่ Persist Canonical Transactions**
3. **Confirmed Import:** หลัง User Confirm ค่อย Persist พร้อม Deduplication และ Retry/Partial Failure Tests
4. **Financial-summary Verification:** ตรวจ Transfer, Refund, Internal Movement ไม่เพิ่ม Income/Expense ผิด และแสดง Missing Cash Data
5. **Operational Readiness:** Access Controls, Retention, Monitoring, Recovery และ Release Checks ตาม Privacy/Data Risk

อาจเรียงใหม่หลังทดลอง ไม่รับประกันว่าจะเป็น Independent Releases ทั้งหมด

งานอาจแบ่ง Engineer ฝั่ง Ingest และ Preview UI โดยตกลง API Contract ร่วมกัน Peer/QA ตรวจ Critical Scenarios และ Technical Owner ตรวจ Data Correctness; Solo อาจเป็นคนเดียวหลายบทบาท

**ห้ามอ้างโดยไม่มีหลักฐาน:** รองรับธนาคารจริง, CSV Formats, Story Points/Person-days ตายตัว หรือยอด Balance ถูกต้องแน่นอน

## 12. AI Opportunities (ยังไม่สร้าง Skill)
AI **เสนอ** Slices, Dependency Questions, Risk Checklists, Test Cases และสรุป Work Board ที่ได้รับอนุญาตได้ ช่วยเทียบ Scope กับ Design และ Flag Missing Owners

AI **ห้าม** แต่ง Estimates, Commit Sprint, Assign คนโดยไม่ยินยอม, เปลี่ยน Priority เงียบ ๆ หรืออ้างว่างานที่ยังไม่ Test เสร็จแล้ว Human Owners ยืนยัน Allocation, Capacity, Business Priority, Release และ Forecast

สร้าง `puen-stack` Skill เมื่อ Pilot แสดงว่ามีงานซ้ำที่ Procedure ที่ Reusable ช่วยลดได้จริงเท่านั้น

## 13. ประเมินและปรับปรุง
Pilot:
- **Tiny Reversible Task:** Planning เบาพอหรือไม่?
- **Medium-risk Integrated Feature:** Slice เปิดเผยงาน Integration/Verification เร็วหรือไม่?
- **Cross-repo/Cross-team:** Dependency, Owner และ Contract Ordering ชัดหรือไม่?

วัด Blocked Time, WIP, Unplanned Scope, Integration Surprises, Forecast Reliability เท่าที่มีความหมาย, Review/Rework และ Plan Maintenance Overhead ในระดับทีม/ระบบ ไม่จัดอันดับบุคคล

## Approval review checklist — v1.0 Release Candidate
1. Hybrid แสดงภาพรวม Risk/Contract ทั้ง Feature โดยไม่บังคับแตก Ticket จากการเดาหรือไม่?
2. Full Breakdown สำหรับงานง่าย และข้อยกเว้นงาน Cutover/Risk สูง ชัดเจนพอหรือไม่?
3. Gate **Agreed for First Increment** ป้องกัน Critical Dependency ค้างโดยไม่ต้องแตกทุก Task ไกล ๆ ได้หรือไม่?
4. Jira-first Source of Truth, Estimate ที่บอก Unknowns และ Decision Owners ใช้ได้ทั้ง Solo/Team หรือไม่?
5. งาน Cross-repo ใช้ Feature-level View หลักและ Link ไป Issue ราย Repo โดยไม่ดูแลข้อมูลซ้ำได้หรือไม่?

**Release Candidate pending explicit owner approval.** No automated staffing, sprint commitments, Jira integration, or new Skill is approved by this document.
