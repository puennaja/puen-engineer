# Implementation Workflow — v1.0 Release Candidate (ภาษาไทย)

- **สถานะ:** Review Ready / Release Candidate — รอ Owner Approve; ยังไม่ Accepted
- **Decision:** แยก Implementation และ Verification เป็นสอง Workflow โดยใช้ Execution Loop ร่วมกัน (2026-10-10)
- **วันที่:** 2026-10-10
- **Track:** A — Build My Engineer
- **ก่อนหน้า:** [Delivery Planning v1.0](./delivery-planning.th.md), [Solution Design v1.0](./solution-design.th.md)
- **Workflow คู่กัน:** [Verification](./verification.th.md)
- **ต้นฉบับอังกฤษ:** [Implementation](./implementation.md)
- **รองรับ:** Solo ถึง Cross-team; AI เป็นตัวเลือก ไม่ใช่ข้อบังคับ

## เป้าหมายและขอบเขต

นำ Increment ที่ตกลงและเข้าใจพอแล้วมาสร้างเป็น **โค้ดที่ Review ได้** พร้อม Developer Checks และหลักฐานส่งต่อให้ Verification

**Implementation กับ Verification แยกความรับผิดชอบ แต่ไม่ใช่ Waterfall**: ทำวนสั้น ๆ ได้ เขียน Test หรือขอ Review ได้ตั้งแต่ระหว่าง Implement ไม่ต้องรอเขียนโค้ดทั้ง Feature จบ

**Inputs:** Acceptance Scenarios, Increment ใน Delivery Plan, Design Decisions, Repo/Contract ที่สำรวจแล้ว, Risks, Coding Conventions, สิทธิ์ที่ได้รับอนุญาต

**หยุดและกลับไป Discovery/Design/Planning** ถ้าข้อสมมติที่ยังไม่พิสูจน์อาจทำให้เกิด Data Loss, Authorization ผิด, Contract เข้ากันไม่ได้หรือ Business Behavior ผิด การทำ Spike แบบจำกัดขอบเขตยังนับเป็นงานที่เหมาะสม

## Execution Loop ร่วมกัน — ข้อตกลงการส่งต่องาน (เสนอสำหรับ v1.0)

Workflow สองตัว **แยกหน้าที่** แต่ใช้ **วงจรทำงานร่วมกันเดียว** ต่อ Increment:

```text
Delivery Planning (Increment ที่ตกลง)
   ↓
Implementation: สำรวจ → แก้โค้ด → Developer Checks
   ⇄ Verification: Acceptance / Tests / Contracts / Independent Review
   ↳ Findings → แก้โค้ด → ตรวจส่วนที่ได้รับผลกระทบซ้ำ
   ↓
Verification Decision → Release Review (Workflow อื่น)
```

**เริ่ม Verify ได้ทันทีที่มีสิ่งให้ตรวจ:** Acceptance Example, Test Strategy, API/Contract หรือ Partial Diff ก็เริ่มได้แล้ว ไม่ต้องรอ Code ทั้ง Feature จบ หรือรอป้าย Ready for Verification แบบบังคับ Developer Tests ยังคงอยู่ใน Implementation ส่วนการประเมินอย่างอิสระและการตัดสินคุณภาพหลักฐานเป็นหน้าที่ Verification

**ส่งต่อผ่าน Jira/MR เดิมเพียงชุดเดียว** ไม่บังคับสร้างเอกสารใหม่:
- **Identity / Scope:** Increment, Acceptance IDs, Repo/Diff Links, Exclusions
- **Changes / Risks:** Boundary ที่เปลี่ยน, Contracts/Data/Migration และ Failure Cases
- **Evidence:** Checks ที่รันจริงพร้อมผล/Environment, สิ่งที่ **Not run / Blocked**
- **Feedback:** Findings พร้อม Severity/หลักฐาน/Owner, ผลแก้และตรวจซ้ำ
- **Decision:** ทำต่อ / พร้อมตรวจเพิ่ม / Fix & Reverify / Blocked / Verified for Release Review โดยมนุษย์รับผิดชอบ Residual Risk

**กติกาวนซ้ำ:** แก้จุดไหนให้ Reverify จุดนั้นและ Dependencies ที่ได้รับผลกระทบ แต่ถ้าผลกระทบขยายต้องตรวจเพิ่ม ถ้าเจอ Unknown สำคัญย้อนกลับ Discovery/Design/Planning ทั้งสอง Workflow **ไม่มีอำนาจ Deploy** หรือแก้ Acceptance เงียบ ๆ

**Roles:** งาน Solo ทำได้ทั้งสองหน้าที่ แต่ควรแยก Self-check กับ Review Pass งาน Medium/High-risk ควรมี Reviewer ที่เป็นอิสระจริงเมื่อทำได้หรือ Local Policy กำหนด AI สองตัวเห็นตรงกันไม่ได้แปลว่า Test ผ่านหรือ Human อนุมัติ

**Scale:** งานเล็ก Reversible บันทึกใน MR สั้น ๆ ได้ งาน Cross-repo/Finance/Security ต้องมีหลักฐาน Contract, Tests, Review และ Release Risks มากขึ้น โดยไม่ต้องสร้าง Ticket ซ้ำ

## กิจกรรม (ทำซ้ำได้)

| กิจกรรม | ทำอะไร | หลักฐานขั้นต่ำ |
| --- | --- | --- |
| 1. Orient | อ่าน Codebase, AGENTS.md (ถ้ามี), Tests, Requirement/Design และ Git Working Tree | เข้าใจพฤติกรรมเดิมและ Files ที่มีหลักฐานว่ากระทบ |
| 2. Bound | ระบุ Increment, Acceptance, Non-goals และ Integration Boundary | Task/Plan Link และ Change Intent |
| 3. Plan local edits | เลือกการแก้ที่เล็กและสอดคล้อง Architecture ระบุ Test/Migration/Error | แนวทางแก้และ Unknown สำคัญ |
| 4. Implement | แก้โค้ดอย่างมีจุดประสงค์ แยก Refactor ที่ไม่เกี่ยวข้อง และรักษา Security/Compatibility | Diff ที่อ่าน Review ได้ |
| 5. Developer checks | รัน Test/Lint/Typecheck/Build ที่เกี่ยวข้องและเพิ่ม Regression Tests | คำสั่งที่รันจริง ผลและสิ่งที่ไม่ได้รัน |
| 6. Handoff / Iterate | แก้ Findings ภายใน สรุป Contract, Risks, Unknowns ให้ Verification | Diff/Acceptance/Test Evidence/Remaining Gaps |

**ไม่ต้องรอถึงขั้นท้ายค่อย Test** Developer Checks เป็น Feedback ภายใน Implementation แต่ Independent Verification อยู่ใน Workflow แยก และสามารถเรียกตรวจระหว่างทำได้

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

AI เสนอ Code, Tests, Explanation ได้ภายใต้สิทธิ์ที่อนุญาต Engineer รับผิดชอบการเปลี่ยน Architecture, Destructive Operations, การ Run Command ที่มีสิทธิ์พิเศษ, Commit/Push/MR ตาม Policy ท้องถิ่น ยังไม่ได้บังคับว่าต้องใช้ Claude หรือ Codex คู่กัน และไม่สร้าง Skill ก่อน Pilot

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

**รอ Review:** AI แก้/Commit ได้ภายใต้ขอบเขต Permission แบบไหน? Handoff ขั้นต่ำใน Jira/GitLab เท่าไร? งานใหญ่ควรเริ่ม Independent Review เมื่อไร?

**Release Candidate รอ Owner Approve** ยังไม่เปลี่ยน AGENTS.md, Skills, Automation หรือ Permissions
