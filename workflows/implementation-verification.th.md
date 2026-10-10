# Implementation & Verification Workflow — v0.1 (ฉบับภาษาไทย)

- **สถานะ:** Proposed / Draft — รอเจ้าของ Review; **ยังไม่ Accepted**
- **วันที่:** 2026-10-10
- **Track:** A — Build My Engineer v1.0
- **พื้นฐาน:** [Feature Delivery Lifecycle v1.0](./feature-delivery-lifecycle.th.md)
- **ก่อนหน้า:** [Delivery Planning v1.0](./delivery-planning.th.md), [Solution Design v1.0](./solution-design.th.md)
- **AI Flow ประกอบ:** [AI-Assisted Feature Delivery — Draft](./ai-assisted-feature-delivery.md)
- **ต้นฉบับอังกฤษหลัก:** [English](./implementation-verification.md)
- **ถัดไป:** Release & Operations (ยังไม่ได้ออกแบบ)
- **รองรับ:** Solo, Team, Multi-repo; ไม่บังคับใช้ AI

## 1. ทำไมต้องมี Workflow นี้

เปลี่ยน Increment ที่ตกลงและเข้าใจมากพอแล้วให้เป็น **Software ที่ทำงานจริง มีหลักฐานทดสอบ และผ่านการ Review อย่างอิสระ** โดย Engineer ยังรับผิดชอบผลลัพธ์เสมอ

ครอบคลุมการแก้โค้ด การทดสอบของผู้พัฒนา Integration Verification และ Independent Review เพื่อส่งต่อการตัดสินใจ Release **Test เขียวไม่ได้แปลว่าได้รับอนุญาตให้ Deploy** และ AI สองตัวเห็นตรงกันก็ไม่ได้ยืนยันว่าโค้ดถูกต้อง

ไม่บังคับ TDD, Coverage เปอร์เซ็นต์, IDE, Model, Agent ตัวที่สอง, CI หรือ Commit Format แบบเดียวทุกทีม ไม่สร้างเอกสารซ้ำ Jira โดยไม่จำเป็น

## 2. Input และเกณฑ์หยุดก่อนเริ่ม

สำหรับ **Increment ถัดไป** (ไม่ใช่ทุก Task ในอนาคต) ควรรู้:
- Acceptance Cases และสิ่งที่ไม่อยู่ใน Scope พร้อม Product Decision Owner
- Solution Direction, ขอบเขต Code/Service ที่มีหลักฐานจาก Technical Discovery
- Critical Contracts, Invariants, Permissions และผลด้าน Data/Migration
- วิธี Verify ตาม Risk: Unit, Component, Integration, Contract, E2E, Manual หรือตามความเหมาะสม
- Git Branch/MR และ Convention ปัจจุบันของ Repo; ผู้ Review
- Unknowns ที่ยังเหลือและผลกระทบหาก Assumption ผิด

**กลับไป Discovery/Design/Planning หรือเริ่ม Spike แบบจำกัด** เมื่อ Security, Data Integrity, Contract หรือ Ownership ยังคลุมเครือจนทำงานต่ออย่างปลอดภัยไม่ได้

## 3. วงจรการทำงานที่ปรับได้

| กิจกรรม | ทำอะไร | หลักฐาน/Checkpoint |
| --- | --- | --- |
| **1. Orient** | อ่านคำแนะนำของ Repo, โค้ดใกล้จุดที่จะเปลี่ยน, แนว Test, Requirement/Design; ตรวจ Git Working Tree | รู้พฤติกรรมเดิมและ Scope จริง ไม่แต่งรายชื่อไฟล์ |
| **2. Bound Increment** | เลือกงานเล็กที่มี Outcome ตรวจสอบได้ แยก Refactor อื่นออกถ้าเป็นไปได้ | Change Intent, Dependencies, Test Plan |
| **3. Implement** | ใช้ Pattern เดิมถ้าไม่มี Evidence ให้เปลี่ยน, ทำ Diff ที่ Review ได้, ดู Failure/Compatibility/Migration | โค้ดและเหตุผลสำคัญ |
| **4. Local Verify** | รัน Format/Lint/Type/Build/Test ตามสมควร เพิ่ม Regression Tests และ Negative Cases | คำสั่งที่รันจริง ผล Scope และสิ่งที่ไม่ได้รัน ห้ามแต่งผล Test |
| **5. Integrate** | ตรวจ API/Events/Schema, Consumer/Producer, Version Skew, Migration/Flags ข้าม Boundary | Integration/Contract Evidence หรือ Blocker มี Owner |
| **6. Independent Review** | Engineer คนอื่นหรือ Review Pass อิสระตรวจ Diff เทียบ Requirement, Design, Tests, Security, Risks | Findings พร้อม Severity, Evidence และ Resolution |
| **7. Reconcile & Handoff** | แก้ Review, รัน Check ซ้ำเท่าที่ได้รับผลกระทบ, อัปเดต Issue, Commit/Draft MR ตามเหมาะสม | Traceability และ Remaining Risk |

**ไม่ใช่ Waterfall** งานเล็กทำรอบเดียวได้ งานเสี่ยงสูงต้อง Verify/Review ลึกกว่า

## 4. แนวทางลงมือแก้โค้ด

- แก้ให้น้อยเท่าที่พอดีกับ Acceptance และ Domain Invariants; อย่าแถม Cleanup ที่ทำให้ Review ยาก
- เคารพ Toolchain, Style และ Compatibility เดิมของ Repo เว้นแต่มีหลักฐานและเหตุผลที่ต้องเปลี่ยน
- Test ต้องพิสูจน์ Behavior ที่ต้องการ โดยเฉพาะ Regression, Boundary, Negative Cases; ไม่ตั้งเป้า Coverage เดียวทุก Repo
- Prototype/Experiment ที่ยังไม่แน่ใจต้องแยกจาก Production Code และอย่าเชื่อการคาดเดา Codebase ของ AI โดยไม่ตรวจ
- ห้ามนำ Secret, Customer Data หรือข้อมูลลับบริษัทเข้า Public Repo/AI Tool ที่ไม่ได้รับอนุญาต
- ถ้า Unknown เปลี่ยน Design ต้องย้อนกลับไป Solution/Technical Discovery ไม่ฝืนแผน
- จะ Test-first, Test-after หรือแบบผสมก็ได้ ถ้าสุดท้ายมี Verification เพียงพอกับ Risk

## 5. Verification Evidence ที่นับว่าเชื่อถือได้

1. Acceptance Examples ที่ตรวจแล้วพร้อม Observed Behavior
2. Test/Check ที่ **รันจริง** มี Command, ผล และ Scope; ถ้าไม่ได้รันให้บอก **Not run / Blocked**
3. Failure, Edge, Concurrency, Permission หรือ Data Correctness Cases ตามระดับ Risk
4. Integration/Contract Evidence เมื่อกระทบหลาย Repo หรือหลาย Service
5. Independent Review และสถานะ Findings รวมถึงผู้ยอมรับ Residual Risk
6. งาน Release, Monitoring, Migration และ Rollback ที่ต้องส่งต่อ

ลำดับสถานะ Evidence: **Proposed → Executed → Reviewed → Accepted** แต่ละระดับแทนกันไม่ได้

**ห้ามเหมารวม:** Unit Test ผ่าน ≠ Feature Accepted, AI Review แล้ว ≠ Human Approved, Merge ได้ ≠ Deploy ปลอดภัย

## 6. Checklist สำหรับ Reviewer

ตรวจ: Acceptance, Compatibility, Domain Invariants, Authorization/Privacy, Error/Timeout/Retry/Idempotency, Observability, Test Quality, Design Consistency, Migration/Rollback, Performance/Concurrency ตาม Risk

Finding ควรมี **Severity · หลักฐาน/ตำแหน่งโค้ด · Scenario/Impact · Recommendation · Resolution** เน้น Risk จริง ไม่ยึด Personal Style การไม่พบปัญหาไม่ได้พิสูจน์ว่าโค้ดถูก

## 7. หลาย Repositories และ Workspaces

ใช้ Feature-level Issue/Design **หนึ่งชุดเป็น Source of Truth** และ Link งานแยกตาม Repo ให้เห็นว่าใครเป็นเจ้าของ Contract พร้อม Compatibility และ Release Sequence

**Agent ที่เปิดใน Repo เดียวไม่สามารถถือว่าอ่านอีก Repo ไปแล้ว** โดยอัตโนมัติ

| Boundary | ข้อมูลที่ต้องยืนยัน | วิธี Verify | Deployment |
| --- | --- | --- | --- |
| Service ↔ BFF | Provider/Consumer, Response Schema, Permission | Contract หรือ Integration Test | ตัดสินตาม Backward Compatibility |
| BFF ↔ Web | Mapping, Errors, UI Behavior | Component + Integrated Scenario | ตัดสินตาม Backward Compatibility |

อย่าสรุปว่าต้อง Deploy Repo ไหนก่อนโดยยังไม่ตรวจ Contract จริง

## 8. ใช้ AI อย่างมีขอบเขต

AI สามารถ **เสนอ** Code Changes, Tests, Edge Cases, Review Findings, Summary และ Draft Commit/MR ได้ ภายใต้ Access ที่ได้รับอนุญาต

Engineer ต้องยืนยันการเปลี่ยน Acceptance, Architecture, Ownership, การลบข้อมูล, การใช้ Secret, Push/Merge, Privileged Commands และ Release โดยยังไม่มี Approval Policy สำหรับ Automation ที่แน่นอน

แนวทางให้ทดลองก่อนบังคับ: Agent หนึ่งช่วย Implement แล้วทำ Independent Review Pass โดยใช้ Prompt/Context ที่เตรียมแยกจากรอบ Implement; จะเป็นอีก Model หรือ Model เดิมก็ได้ แต่ไม่ได้รับประกันความเป็นอิสระ ต้องมี Human Review และ Test ที่รันจริง

**AGENTS.md:** เก็บคำแนะนำ Repo ที่สั้นและจำเป็น Link ไป Workflow กลางแทน Copy ซ้ำ; สร้าง Skill ใน `puen-stack` ก็ต่อเมื่อ Pilot พบงานซ้ำที่แก้ได้จริง

## 9. Artifact เดียว: Verification & Review Record แบบเล็ก

แนะนำให้ใส่ใน Jira/MR เดิม ไม่ต้องสร้าง Markdown เพิ่มถ้าไม่มีเหตุผล

```markdown
## Increment and acceptance
Work item / target behavior / scope exclusions:

## Changes
Repositories / contracts / links to diffs:

## Evidence
Check | executed? | result | environment | link
Acceptance examples / negative tests / integration evidence:

## Independent review
Reviewer or separate review pass / findings / resolution:

## Gaps and risks
Not-run checks / waived findings / blockers / owner:

## Handoff
Ready for release review? Migration / monitoring / rollback pending:
```

## 10. Exit Decision

- **Verified — Ready for Release Review:** มี Acceptance Evidence, Tests และ Review ตาม Risk พร้อม Remaining Risk ชัด; ยังต้องผ่าน Release Workflow
- **Needs Fixes/Re-verification:** พบ Defect, Failed Checks หรือ Review Finding
- **Blocked — Discovery/Design:** ข้อมูลใหม่ทำให้ Assumptions ใช้ไม่ได้
- **Stop/Defer:** Risk รับไม่ได้หรือประโยชน์ไม่คุ้มลงทุน

งานเล็กอาจใช้เพียง MR Description กับ Test Output; งาน Payment, Security, Migration ต้องตรวจและบันทึกมากขึ้น

## 11. ทดลองก่อนบังคับ

เลือกงานสมมติหรือ Non-sensitive ที่ได้รับอนุญาต: (1) Tiny Regression, (2) Medium Feature มี Tests/Peer Review, (3) Simulated Cross-repo Contract Change. ดู Time-to-feedback, Review Findings, Integration Surprises, การรัน Test จริง, Process Overhead และ AI Cost

ห้ามใช้จำนวน Lines/Commits/Tickets ที่ AI สร้างจัดอันดับ Engineer

## คำถามที่รอ Review

1. ให้ Implementation & Verification อยู่ในไฟล์เดียวแต่แยก Checkpoints หรือแยกสอง Workflow?
2. Independent AI Review ควรแนะนำเป็น Default เฉพาะงาน Medium/High-risk หรือทุก Change?
3. หลักฐานขั้นต่ำอะไรอยู่ใน GitLab MR/Jira ได้โดยไม่ซ้ำ Documentation?
4. AI Run/Edit/Commit/Open MR/Push ได้แค่ไหน และสิ่งไหนต้อง Confirm?
5. จะใช้ Pilot แบบไหนก่อน Extract Skill ไป `puen-stack`?

**ยังเป็น Draft เท่านั้น ไม่อนุมัติการเปลี่ยน AGENTS.md, Permission, Company Process หรือ Skill ใหม่**
