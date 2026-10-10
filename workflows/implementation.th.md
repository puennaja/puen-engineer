# Implementation Workflow — v1.0 Release Candidate (ภาษาไทย)

- **สถานะ:** Review Ready / Release Candidate — รอ Owner Approve; ยังไม่ Accepted
- **Decision:** แยก Implementation และ Verification เป็นสอง Workflow โดยใช้ Execution Loop ร่วมกัน (2026-10-10)
- **วันที่:** 2026-10-10
- **Track:** A — Build My Engineer
- **ก่อนหน้า:** [Delivery Planning v1.0](./delivery-planning.th.md), [Solution Design v1.0](./solution-design.th.md)
- **Workflow คู่กัน:** [Verification](./verification.th.md)
- **ต้นฉบับอังกฤษ:** [Implementation](./implementation.md)
- **รองรับ:** Solo ถึง Cross-team; AI เป็นตัวเลือก ไม่ใช่ข้อบังคับ

## หน่วยงานหลัก: Work Item ตาม Jira (ข้อตกลง Track A)

**Work Item** หมายถึง **Jira Story, Sub-task, Bug หรือ Task** ที่ Engineer ได้รับมอบหมายและกำหนดขอบเขตให้ AI ช่วยทำ ไม่ใช่ Jira Issue Type ใหม่ และไม่จำเป็นต้องครอบคลุม BE, BFF, FE พร้อมกัน งานจริงอาจเป็น **BE อย่างเดียว**, **BFF อย่างเดียว** หรือ BE+BFF ตามที่ได้รับมอบหมาย ส่วน FE เป็นความรับผิดชอบของ Engineer คนอื่นได้

ใช้ **Story เป็น Context หลัก** สำหรับ Business Acceptance, Architecture, API/Contract และ Dependencies ส่วน Work Item ต้องระบุ **ผลลัพธ์ที่ตรวจได้, Non-goals, Test Intent, Evidence และ Repo ที่เกี่ยวข้อง** จาก Requirement จริงก่อนลงมือ

**ค่าเริ่มต้น:** AI ทำงานเฉพาะ Repo ที่ได้รับอนุญาตใน Work Item ไม่สลับไป Implement Repo อื่นหรือรับงาน FE เอง Cross-repo Implementation ทำเฉพาะเมื่อ Scope/Permission ระบุ ส่วน Contract/Integration Verification ระดับ Story ให้ประสานผู้รับผิดชอบของทีมตามจริง ถ้า Backend Work Item Verify ผ่าน **ยังไม่ถือว่า Story ทั้งหมดผ่าน**; ถ้าทดสอบเชื่อม FE ไม่ได้ ให้รายงาน Not run/Blocked และผู้รับผิดชอบ

**Human Gates:** Solution Design/Delivery Planning อยู่ระดับ Feature/Story และ Implementation/Verification อยู่ระดับ **Work Item ที่ตกลงแล้วแต่ละรายการ** ถ้า Work Item เป็น Story เองให้บันทึก Gate ใน Jira เดิมโดยไม่สร้าง Artifact ซ้ำ ไม่เพิ่ม Story-level Gate บังคับใหม่ ทั้งนี้ [Delivery Planning ที่ Accepted](./delivery-planning.th.md) อาจยังใช้ศัพท์ Agile ว่า *Increment* ในความหมายของ Delivery Slice แต่ AI Execution ใช้ **Work Item** ตามข้อตกลงใหม่ ดู [Track A Decisions](./ai-assisted-feature-delivery.md)

## เป้าหมายและขอบเขต

นำ Work Item ที่ตกลงและเข้าใจพอแล้วมาสร้างเป็น **โค้ดที่ Review ได้** พร้อม Developer Checks และหลักฐานส่งต่อให้ Verification

**Implementation กับ Verification แยกความรับผิดชอบ แต่ไม่ใช่ Waterfall**: ทำวนสั้น ๆ ได้ เขียน Test หรือขอ Review ได้ตั้งแต่ระหว่าง Implement ไม่ต้องรอเขียนโค้ดทั้ง Feature จบ

**Inputs:** Acceptance Scenarios, Work Item ใน Delivery Plan, Design Decisions, Repo/Contract ที่สำรวจแล้ว, Risks, Coding Conventions, สิทธิ์ที่ได้รับอนุญาต

**หยุดและกลับไป Discovery/Design/Planning** ถ้าข้อสมมติที่ยังไม่พิสูจน์อาจทำให้เกิด Data Loss, Authorization ผิด, Contract เข้ากันไม่ได้หรือ Business Behavior ผิด การทำ Spike แบบจำกัดขอบเขตยังนับเป็นงานที่เหมาะสม

## Execution Loop ร่วมกัน — ข้อตกลงการส่งต่องาน (เสนอสำหรับ v1.0)

Workflow สองตัว **แยกหน้าที่** แต่ใช้ **วงจรทำงานร่วมกันเดียว** ต่อ Work Item:

```text
Feature Design / Plan ที่ Approved → Work Item
   ↓
Verification-first: Acceptance, Expected Outcomes, Test Seams
   ↓
Implementation: Vertical Slice → TDD เมื่อเหมาะสม → Developer Checks
   ⇄ Verification: Test Quality, Behavior Evidence, Independent Review
   ↳ Findings → Scoped Fix → Reverify
   ↓
Human Verification Gate → Release Review (แยก)
```

**Verification เริ่มก่อนเขียนโค้ดได้** เพื่อกำหนด Expected Behavior จาก Requirement/Contract; นี่เป็นการเตรียมและตรวจสอบระหว่างทาง ไม่ใช่ Approval Gate ใหม่ **เริ่ม Verify ได้ทันทีที่มีสิ่งให้ตรวจ:** Acceptance Example, Test Strategy, API/Contract หรือ Partial Diff ก็เริ่มได้แล้ว ไม่ต้องรอ Code ทั้ง Feature จบ หรือรอป้าย Ready for Verification แบบบังคับ Developer Tests ยังคงอยู่ใน Implementation ส่วนการประเมินอย่างอิสระและการตัดสินคุณภาพหลักฐานเป็นหน้าที่ Verification

**ส่งต่อผ่าน Jira/MR เดิมเพียงชุดเดียว** ไม่บังคับสร้างเอกสารใหม่:
- **Identity / Scope:** Work Item, Acceptance IDs, Repo/Diff Links, Exclusions
- **Changes / Risks:** Boundary ที่เปลี่ยน, Contracts/Data/Migration และ Failure Cases
- **Evidence:** Checks ที่รันจริงพร้อมผล/Environment, สิ่งที่ **Not run / Blocked**
- **Feedback:** Findings พร้อม Severity/หลักฐาน/Owner, ผลแก้และตรวจซ้ำ
- **Decision:** ทำต่อ / พร้อมตรวจเพิ่ม / Fix & Reverify / Blocked / Verified for Release Review โดยมนุษย์รับผิดชอบ Residual Risk

**กติกาวนซ้ำ:** แก้จุดไหนให้ Reverify จุดนั้นและ Dependencies ที่ได้รับผลกระทบ แต่ถ้าผลกระทบขยายต้องตรวจเพิ่ม ถ้าเจอ Unknown สำคัญย้อนกลับ Discovery/Design/Planning ทั้งสอง Workflow **ไม่มีอำนาจ Deploy** หรือแก้ Acceptance เงียบ ๆ

**Roles:** งาน Solo ทำได้ทั้งสองหน้าที่ แต่ควรแยก Self-check กับ Review Pass งาน Medium/High-risk ควรมี Reviewer ที่เป็นอิสระจริงเมื่อทำได้หรือ Local Policy กำหนด AI สองตัวเห็นตรงกันไม่ได้แปลว่า Test ผ่านหรือ Human อนุมัติ

**Scale:** งานเล็ก Reversible บันทึกใน MR สั้น ๆ ได้ งาน Cross-repo/Finance/Security ต้องมีหลักฐาน Contract, Tests, Review และ Release Risks มากขึ้น โดยไม่ต้องสร้าง Ticket ซ้ำ

## Verification-first และ TDD (Design Direction สำหรับ v1.0)

Verification กำหนด **Acceptance Scenarios, Expected Outcomes และ Test Seams ก่อน Implement** โดยอ้าง Requirement/Domain/Contract ที่ยืนยันแล้ว อาจเขียน Acceptance Test ที่ Fail ด้วยเหตุผลตรงตาม Behavior ที่ยังขาด หากมี Harness เหมาะสม มิฉะนั้นใช้ Given–When–Then หรือ Repro ที่ Review ได้ ห้ามใช้ Test Failure จาก Environment เสียเป็นหลักฐานว่าโค้ดผิด

Implementation เป็นเจ้าของ Code และ Unit/Regression Tests ทำทีละ Vertical Slice และใช้ **Red → Green → Refactor → Rerun** เมื่อ TDD มี Feedback ที่เชื่อถือได้ ไม่บังคับทุกงาน ห้ามเปลี่ยน Expected Result เพียงเพื่อให้ Test ผ่าน

Verification กลับมาตรวจคุณภาพ Tests และรัน Independent Behavior Evidence บน Surface จริงตาม Risk เพราะ Build/Unit Tests ผ่านไม่ได้พิสูจน์ทุกอย่าง หากต้องแก้ Acceptance/Design/Scope สำคัญต้องขอ Human Decision ใช้ Jira/MR เดิม ไม่มี Gate หรือ Skill บังคับเพิ่ม

## กิจกรรม (ทำซ้ำได้)

| กิจกรรม | ทำอะไร | หลักฐานขั้นต่ำ |
| --- | --- | --- |
| 1. Orient | อ่าน Codebase, AGENTS.md (ถ้ามี), Tests, Requirement/Design และ Git Working Tree | เข้าใจพฤติกรรมเดิมและ Files ที่มีหลักฐานว่ากระทบ |
| 2. Bound | ระบุ Work Item, Acceptance, Non-goals และ Integration Boundary | Task/Plan Link และ Change Intent |
| 3. Plan local edits | เลือกการแก้ที่เล็กและสอดคล้อง Architecture ระบุ Test/Migration/Error | แนวทางแก้และ Unknown สำคัญ |
| 4. Implement | แก้โค้ดอย่างมีจุดประสงค์ แยก Refactor ที่ไม่เกี่ยวข้อง และรักษา Security/Compatibility | Diff ที่อ่าน Review ได้ |
| 5. Developer checks | รัน Test/Lint/Typecheck/Build ที่เกี่ยวข้องและเพิ่ม Regression Tests | คำสั่งที่รันจริง ผลและสิ่งที่ไม่ได้รัน |
| 6. Handoff / Iterate | แก้ Findings ภายใน สรุป Contract, Risks, Unknowns ให้ Verification | Diff/Acceptance/Test Evidence/Remaining Gaps |

**ไม่ต้องรอถึงขั้นท้ายค่อย Test** Developer Checks เป็น Feedback ภายใน Implementation แต่ Independent Verification อยู่ใน Workflow แยก และสามารถเรียกตรวจระหว่างทำได้

## ปรับแนวทาง Implementation จาก pstack + grill-me (เสนอสำหรับ v1.0)

ส่วนนี้ **ยืมหลักคิด ไม่ได้บังคับให้ใช้ Plugin หรือ Slash Command** อ้างอิง [pstack Guide](https://github.com/cursor/plugins/blob/main/pstack/docs/guide/README.md), [Understand](https://github.com/cursor/plugins/blob/main/pstack/docs/guide/03-understand.md), [Design](https://github.com/cursor/plugins/blob/main/pstack/docs/guide/04-design.md), [Build](https://github.com/cursor/plugins/blob/main/pstack/docs/guide/05-build-and-clean.md) และ [grilling ของ mattpocock](https://github.com/mattpocock/skills/blob/main/skills/productivity/grilling/SKILL.md) โดยถือว่า **Workflow เป็นหลัก ส่วน Skill เป็นเครื่องมือช่วยทำงาน**

### 1. ก่อน AI แก้โค้ด ต้องมี Goal Contract

ข้อมูลที่ต้องมีหรืออ้างจาก Jira/Plan เดิมคือ **Goal**, **Done Check ที่ผ่าน/ไม่ผ่านได้**, **Proof ที่ต้องเห็น**, **Known Facts/Code References** และ **Constraints** เช่น Read-only หรือหยุดรอ Decision Owner ถ้าข้อมูลอยู่ใน Issue แล้วไม่ต้องเขียน Spec ซ้ำทุก Prompt ถ้า Requirement คลุมเครือให้ AI สรุปความเข้าใจก่อนแก้โค้ด

### 2. เข้าใจโค้ดจาก Evidence และถามเฉพาะ Decision สำคัญ

เริ่มจาก Read-only Investigation ว่าโค้ดทำงานอย่างไร (**how**) แล้วดูเหตุผลที่ออกแบบไว้ (**why**) เมื่อกำลังเปลี่ยน Boundary, Ownership หรือ Contract แยก Fact, Inference, Assumption และ Unknown ห้ามถือว่าทฤษฎีเรื่อง Root Cause เป็นข้อเท็จจริงโดยไม่มี Reproduction

นำแนวคิด `grill-me` มาใช้เฉพาะ **การตัดสินใจที่คลุมเครือหรือเปลี่ยนยาก**: ไล่ Decision Tree ตาม Dependencies, เสนอตัวเลือกพร้อมเหตุผลให้ Human ตัดสิน แต่ข้อเท็จจริงที่หาได้จาก Codebase ให้ AI ไปอ่านเอง ไม่ถาม Engineer เกินจำเป็น **Solution Design ที่ Approved อยู่แล้วเป็นเจ้าของ Design Decision**; การ Grill ไม่ใช่การ Approve Design โดยอัตโนมัติ

### 3. เลือก Execution Path ให้เหมาะกับชนิดงาน

| ประเภท | สิ่งที่ควรทำ | เมื่อไรต้องกลับไปค้นหาเพิ่ม |
| --- | --- | --- |
| **Bug** | Reproduce ด้วย Surface ที่ใกล้ผู้ใช้จริงที่สุด → หา Root Cause → แก้เล็กเท่าที่ Evidence รองรับ → รัน Repro ซ้ำ | ถ้ายัง Reproduce ไม่ได้ให้แจ้ง **Inconclusive** ไม่เดา Fix |
| **Feature** | เริ่มจาก Acceptance/Contracts/Data Shape → Implement ทีละ Vertical Slice ที่ Verify ได้ | ถ้า Boundary หรือ Data Shape เสี่ยง ให้กลับ Solution Design และพิจารณา Options/Prototypes |
| **Refactor** | เก็บผล Behavior เดิม → เปลี่ยนโครงสร้าง → พิสูจน์ Behavior เท่าเดิม | ถ้าผลลัพธ์หรือ Contract เปลี่ยนโดยไม่ตั้งใจ ให้หยุดทบทวน |
| **Performance** | วัด Baseline และ Bottleneck ก่อนแก้ แล้ว Compare โดยเงื่อนไขใกล้กัน | ถ้าตัวเลขวัดไม่น่าเชื่อถือ ให้ตรวจ Harness ก่อนอ้างว่าเร็วขึ้น |

ทำ **Change → Developer Check → Inspect → Adjust** แบบสั้น ๆ จะเลือก Test-first, Test-after, Real CLI/API/UI หรือ Experiment ก็ได้ตาม Risk ไม่บังคับ TDD ทุกกรณี

### 4. เตรียม Diff ให้ Review ได้

ตัด Unrelated Edits, Dead Compatibility Code และ Defensive Code ที่ไม่มีหลักฐานรองรับเมื่อปลอดภัย แต่ **ไม่เอากฎลบ Comments ทั้งหมดของ pstack มาใช้ตรง ๆ**: Comments ที่อธิบาย Invariant, External Constraints และ API Contract ที่จำเป็นยังมีคุณค่า เก็บ Decisions/Evidence ใน Jira/MR เดิม

**Candidate Skills ในอนาคต (ยังไม่สร้าง):** Task Router (Goal/Done/Evidence → Procedure), Codebase Grounding, Implement by Slice และ Decision Interview ไม่ Copy `/poteto-mode`, `/architect`, `/grill-me` ไปเป็น Chain บังคับใน `AGENTS.md` ต้อง Pilot เทียบ No-skill Baseline ทั้งด้าน Quality, Overhead และ Token Cost ก่อน

## แนวทางตาม Risk

- รักษาพฤติกรรมนอก Scope; ระวัง Compatibility และ Side Effects
- ลด Complexity ไม่ใช่เพียงลดจำนวนบรรทัด ใช้ Pattern เดิมถ้าไม่มีหลักฐานให้เปลี่ยน
- เลือก Test-first / Test-after / แบบผสมตาม Risk ไม่บังคับ TDD หรือ Coverage %
- พิจารณา Migration, Retry, Idempotency, Permission, Concurrency, Observability เมื่อเกี่ยวข้อง
- บอกว่า Test ผ่านต่อเมื่อรันเห็นผลจริง ถ้ารันไม่ได้ให้ระบุ **Not run / Blocked**
- ห้ามเผย Secrets และข้อมูลลับบริษัทใน Public Repo หรือ AI Tool ที่ไม่ได้รับอนุญาต
- ใช้ Jira/MR เป็น Source of Truth โดยไม่สร้าง Document ซ้ำถ้าไม่จำเป็น

## การทำงานหลาย Repos

หนึ่ง Feature-level Work Item เชื่อม Task แยก Repo กำหนด Contract Owner และ Integration Checkpoint จากข้อมูลจริง **Agent ที่เปิด Repo เดียวไม่ได้แปลว่าเห็น Repo อื่น** ต้องยืนยัน Producer/Consumer Contract และลำดับ Deploy ตาม Compatibility ไม่เดา

## AI ช่วยได้อย่างไร (ยังเป็น Candidate)

AI เลือก Skill ตามงานแบบ Hybrid และแก้โค้ด/รัน Developer Checks ที่ปลอดภัยภายใน Scope ที่อนุมัติได้ (**Scoped Autonomy**) รวมทั้ง Local Commit บน Work Branch ที่ตกลง โดยไม่ต้องขออนุมัติทุกไฟล์ แต่การเปลี่ยน Scope/Architecture หรือใช้คำสั่งอันตราย/สิทธิ์พิเศษต้องได้รับอนุญาตเพิ่ม **Human Implementation Gate** ยังคงบังคับก่อน Publish ขึ้น Remote โดย Commit, Push และการเปิด Draft MR มี Permission แยกกัน ไม่บังคับใช้ Claude/Codex คู่กันหรือสร้าง Skill ก่อน Pilot

## ระดับการอนุมัติ — Feature Design/Planning และ Work Item Execution (Track A ตกลงแล้ว)

**Solution Design และ Delivery Planning อนุมัติระดับ Feature** สำหรับแนวทางหลัก, Contracts/Dependencies, Scope/Risk และแผนแบบ Full Scope, Progressive Detail ส่วน **Implementation และ Verification มี Human Gate แยกสำหรับแต่ละ Work Item** ที่ตกลงและตรวจสอบได้ หนึ่ง Feature จึงมีหลาย Implement ↔ Verify Loops ได้โดยไม่ต้องกลับไป Approve Feature ทุกครั้ง

ก่อนเริ่ม Work Item ถัดไป ให้แตก Acceptance, Repos/Contracts, Dependencies และ Checks ให้ละเอียดพอภายใน **Feature Plan ที่อนุมัติอยู่แล้ว** ไม่ต้องขอ Design/Planning Approval ซ้ำถ้าเป็นเพียงการลงรายละเอียดที่ไม่เปลี่ยนขอบเขตเดิม แต่ถ้ามี Critical Unknown หรือ Work Item ไม่อยู่ใน Scope เดิม ห้ามอ้าง Approval เก่ามาใช้โดยไม่ถาม Human

ถ้ามีการเปลี่ยน **Feature Scope, Design, API/Data Contract, Security/Data Risk หรือ Delivery Constraints อย่างมีนัยสำคัญ** ให้หยุดส่วนที่ได้รับผลกระทบและกลับไปขออนุมัติ **เฉพาะ Gate ก่อนหน้าที่ Decision ถูกกระทบ** ตาม Human Owner ของทีมนั้น แต่ Scoped Bug Fix/Test ที่ยังอยู่ใน Scope เดิมทำซ้ำได้ตาม Q7 และต้องผ่าน Verification Gate ของ Work Item นั้น ใช้ Jira/MR เดิม Link Approval ของ Feature และ Work Item โดยระบุ Owner, Scope และ Evidence

Work Item หนึ่งอาจกระทบหลาย Repos ได้ โดยใช้ Evidence ที่เชื่อมโยงกัน แต่ **การเปิด Draft MR แต่ละครั้งยังต้องถาม Human เพื่อยืนยัน Git Flow และ Source/Target Branch แยกเสมอ** นี่คือ Design Decision ของ Track A AI-assisted ที่ยังรอ Approve Workflow ไม่ได้เปลี่ยน General Lifecycle ที่ Accepted แล้ว ดู [Decision Q1–Q9 และ Gate Granularity](./ai-assisted-feature-delivery.md)

## Fixed Implementation Gate, Scoped Git และ Rework (สำหรับ AI-assisted v1.0)

ต่อ **Work Item ที่ตกลงแล้วแต่ละงาน** Human Owner ต้องดู Scope, Diff, ผล Developer Checks ที่รันจริง, Tests ที่ไม่ได้รัน, Contract/Integration Risk และอนุมัติ **Implementation Gate** อย่างชัดเจนก่อนให้ AI Publish งาน ส่วน Preliminary Verification Feedback ทำระหว่าง Implement ได้ แต่ไม่ได้แปลว่าผ่าน Formal Verification Gate

**Q8=B — Scoped Git Autonomy:** AI ทำ Local Commit ใน Work Branch ที่ตกลงได้ตาม Convention ของ Repo หลังผ่าน Implementation Gate ต้องมี **Publish Authorization** ก่อนจึงจะ Push แบบปกติไป **Non-protected Work Branch ที่ระบุ** ได้ และสามารถ Push Scoped Fix รอบถัดไปบน Branch เดิมภายใต้สิทธิ์นั้นตาม Policy ห้าม Force Push, Rewrite History, Push Protected Branch, เปลี่ยน Remote/ปลายทาง หรือลบข้อมูลโดยไม่ได้อนุญาต

**Q7=B — Scoped Rework:** Finding ที่แก้ใน Scope/Design/Risk เดิมสามารถย้อนกลับไป Implement และ Retest ตาม Impact ได้ โดยไม่ต้อง Approve ทุก Edit แต่ **Human ต้อง Approve Verification Gate** หลังดู Evidence สุดท้าย ถ้า Scope, Design, Contract, Security/Data Risk หรือ Plan เปลี่ยนอย่างมีนัยสำคัญ ต้องกลับไปขออนุมัติ Gate ก่อนหน้าที่ได้รับผลกระทบใหม่

**Q9=A — Human Controlled MR:** AI ช่วยรายงาน CI/Review Status ได้ แต่ **มนุษย์เท่านั้น** ที่เปลี่ยน Draft MR เป็น Ready และ Merge; ห้าม Auto-merge/Deploy ดู Q1–Q9 ใน [AI-assisted Delivery Decisions](./ai-assisted-feature-delivery.md)

## GitLab Draft MR — Human Confirmation ทุกครั้ง (ข้อตกลงสำหรับ Review v1.0)

หลังผ่าน **Implementation Gate** แล้ว AI ต้อง **ตรวจ Git Flow ของ Repo และงานนั้นก่อน** ทั้ง Contribution/Branching Rules, Jira/Work Item, Source Branch, Proposed Target Branch, Release/Integration Flow และ Dependency ของ MR ที่เกี่ยวข้อง ห้ามสมมติว่าต้อง Target `master` เสมอ

**ก่อนสร้าง Draft MR ทุกครั้ง AI ต้องแสดง Repo + Source → Target Branch + เหตุผล + Jira Link แล้วถาม Human เพื่อยืนยัน MR นั้นโดยเฉพาะ** แม้เคยอนุมัติ Implementation Gate หรือมี Scoped Git Permission แล้วก็ตาม ห้ามเปิดก่อนมีคำตอบ การแก้ Target Branch ภายหลังก็ต้องยืนยันใหม่

Local Commit/Push ใช้กติกา Scoped Git ด้านบน ส่วน **Draft MR ต้องมี Human Confirmation แยกทุกครั้ง** แม้ได้รับสิทธิ์ Push แล้ว การเปิด MR ไม่ใช่การอนุมัติ Ready/Merge/Deploy

## ส่งอะไรให้ Verification

1. Acceptance Cases และ Repo/Boundary ที่เปลี่ยน
2. Diff และ Contract/Schema Changes
3. Tests ที่รันจริง ผลและ Environment รวมทั้ง Tests ที่ไม่ได้รัน
4. Failure/Permission Scenarios
5. Remaining Unknowns และ Release/Rollback Concerns

**ผลลัพธ์:** Ready for Verification / Needs More Implementation / Blocked — Discovery/Design / Stop-Defer

**Ready for Verification ไม่ใช่สิทธิ์ Deploy**

## Checklist ก่อนอนุมัติ — v1.0 Release Candidate

1. Implementation กับ Verification แยก Responsibility ชัด และไม่สร้าง Waterfall หรือบังคับ Handoff ทุกครั้งหรือไม่?
2. Shared Loop และหลักฐาน Jira/MR เพียงชุดเดียวเหมาะกับ Solo และ Multi-repo หรือไม่?
3. เกณฑ์ Independent Review และผล Verify เพียงพอตาม Risk หรือไม่?
4. ขอบเขต AI Permissions ยังรอออกแบบแยกจาก Workflow อย่างเหมาะสมหรือไม่?

## ทดลองและคำถามที่ยังเปิด

ลอง Tiny Regression, Medium Feature และ Simulated Cross-repo Contract Change; วัด First Feedback, Rework, ความตรงไปตรงมาของ Test Evidence และ Overhead

**จุดตรวจสุดท้าย:** ระบุ Human Gate Owner และ Work Item Boundary ให้ชัดตามทีมจริง ตรวจว่า Publish Authorization เก็บใน Jira/MR เดิมและไม่มีขั้นตอน Review ที่ข้าม Fixed Gates ส่วน Target Branch, CI และ Reviewer Requirements ต้องตรวจจาก Git Flow/Policy ของ Repo จริง

**Release Candidate รอ Owner Approve** ยังไม่เปลี่ยน AGENTS.md, Skills, Automation หรือ Permissions
