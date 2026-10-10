# Implementation Workflow — v0.1 (ภาษาไทย)

- **สถานะ:** Proposed / Draft — ยังไม่อนุมัติ
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

## ทดลองและคำถามที่ยังเปิด

ลอง Tiny Regression, Medium Feature และ Simulated Cross-repo Contract Change; วัด First Feedback, Rework, ความตรงไปตรงมาของ Test Evidence และ Overhead

**รอ Review:** AI แก้/Commit ได้ภายใต้ขอบเขต Permission แบบไหน? Handoff ขั้นต่ำใน Jira/GitLab เท่าไร? งานใหญ่ควรเริ่ม Independent Review เมื่อไร?

**ยังเป็น Draft ไม่ได้เปลี่ยน AGENTS.md, Skills, Automation หรือ Permissions**
