# Verification Workflow — v0.1 (ภาษาไทย)

- **สถานะ:** Proposed / Draft — ยังไม่อนุมัติ
- **วันที่:** 2026-10-10
- **Track:** A — Build My Engineer
- **คู่กับ:** [Implementation](./implementation.th.md), [Delivery Planning v1.0](./delivery-planning.th.md)
- **พื้นฐาน:** [Feature Delivery Lifecycle v1.0](./feature-delivery-lifecycle.th.md)
- **ต้นฉบับอังกฤษ:** [Verification](./verification.md)
- **ถัดไป:** Release & Operations (ยังไม่ได้ออกแบบ)

## เป้าหมายและเวลาเริ่ม

สร้าง **หลักฐานที่เชื่อถือได้ว่าโค้ดตรง Acceptance ไม่ทำลาย Contract สำคัญ และได้รับการ Review อย่างเหมาะสม**

Verification เป็น Workflow ที่มีความรับผิดชอบและ Exit Decision ของตัวเอง แต่ **เริ่มได้ระหว่าง Implementation** และส่ง Findings ย้อนให้แก้ได้หลายรอบ ไม่ใช่รอทำทุกอย่างจบแบบ Waterfall

Implementation รับผิดชอบการสร้าง Change และ Developer Checks; Verification รับผิดชอบการประเมิน Acceptance, Failure, Integration และ Review Findings งาน Solo คนเดียวอาจรับหลายบทบาท แต่ต้องแยกการ Self-check กับ Independent Assessment ให้ชัด AI Review ไม่ใช่การอนุมัติของมนุษย์

## เกณฑ์เริ่มและเลือกความลึก

เริ่มเมื่อมี Slice, Test Plan หรือ Contract ให้ตรวจได้ อ่าน Requirement, Diff, ผลทดสอบที่รันจริง, Design Constraints และ Risks

เลือกความลึกตาม Impact, Reversibility, Privacy, Concurrency, External Contract และ Operational Risk

- **Low Risk:** อ่าน Diff, Focused Tests, Acceptance Behavior; บันทึกสั้นใน MR
- **Medium Risk:** เพิ่ม Negative Cases, Integration Verification และ Independent Review
- **High Risk:** ระบุ Security/Data/Contract/Migration Scenarios และ Owner/Reviewer ที่มีอำนาจ; เก็บหลักฐานตามข้อบังคับถ้ามี

ข้อจำกัดเรื่อง Repo Access, Credential หรือ Environment ให้บันทึกเป็น Blocker ห้ามแต่งผลให้เหมือนตรวจแล้ว

## กิจกรรมที่ทำซ้ำได้

| กิจกรรม | ตรวจอะไร | หลักฐาน |
| --- | --- | --- |
| 1. Trace Acceptance | ผูก Acceptance/Exclusions กับ Behavior และ Tests | Scenario → Check → Observed Result |
| 2. Validate Tests | ตรวจ Assertions และรัน Check ใน Environment ที่อนุญาต | Command, Environment, Pass/Fail/Not run |
| 3. Probe Failures | Permissions, Invalid Input, Race, Retry, Idempotency, Timeout ตาม Risk | Negative-case Evidence |
| 4. Validate Integration | API/Schema, Consumer/Producer, Migration, Version Skew | Integration/Contract Tests หรือ Known Gap |
| 5. Independent Review | ดู Diff, Correctness, Maintainability, Side Effects, Safety และ Test Gaps | Finding พร้อม Severity/หลักฐาน |
| 6. Reconcile | ส่ง Findings กลับ Implementation แล้วตรวจซ้ำหลังแก้ | Finding Resolved/Accepted/Blocked |
| 7. Handoff Decision | ระบุ Verification State, Residual Risks และ Release Readiness | Decision/Owner ที่ชัดเจน |

## หลักฐานต้องไม่ปนกัน

- **Proposed:** Test หรือ Review ที่เสนอ แต่ยังไม่รัน
- **Executed:** มีผลรัน/สังเกตจริง
- **Reviewed:** คนประเมินคุณภาพ Evidence แล้ว
- **Accepted:** ผู้มีอำนาจยอมรับ Residual Risk

ต้องตอบได้ว่าเช็ก Behavior อะไร ด้วยอะไร Environment ไหน ผลเป็นอย่างไร มี Tests ที่ไม่รันหรือไม่ Contract ข้าม Service ถูกตรวจหรือยัง และใครรับผิดชอบ Findings

งานเล็ก Reversible ใช้ MR Note สั้น ๆ ก็พอ; งาน Finance, Authorization, Data Migration ต้องละเอียดขึ้น

## Independent Review

ดู Requirement, Compatibility, Invariants, Permissions, Privacy, Retry/Timeout/Errors, Concurrency, Data, Tests, Observability, Recovery, Performance และ Release Risks เท่าที่เหมาะ

Finding ที่ดีประกอบด้วย **Severity | ตำแหน่ง/หลักฐาน | Scenario/Impact | Recommendation | Resolution/Owner** ไม่ใช่บังคับ Personal Style และการไม่มี Finding ไม่ได้แปลว่าไม่มี Bug

**AI Review:** ใช้ Prompt/Context แยกหรืออีก Model ได้เป็นแนวทางทดลอง แต่ไม่ได้รับประกันว่า Independent จริง Engineer ต้องตัดสินและรับผิดชอบความเสี่ยงเอง

## Cross-repo Verification

รักษา Feature-level Source of Truth หนึ่งชุดพร้อม Link MR ของแต่ละ Repo ตรวจ Contract Provider/Consumer และความเข้ากันได้ของ Versions จากหลักฐานจริง อย่าสมมติว่า Agent ใน Repo เดียวเข้าถึงทุก Repo หรือเดาลำดับ Deploy ว่าปลอดภัย

## บันทึกใน Jira/MR ที่มีอยู่

```markdown
## Acceptance coverage
Scenario | Evidence / Result | Gap

## Checks
Command / Manual procedure | Environment | Pass/Fail/Not run | Link

## Findings
Severity | Location/Evidence | Impact | Resolution | Owner

## Contract / Rollout
Compatibility, Integration, Migration, Remaining Verification

## Decision
Verified for release review | Fix & reverify | Blocked | Stop/defer
Human decision owner / known limitations:
```

## Exit Decision และวนกลับ

- **Verified for Release Review:** Evidence เพียงพอกับ Risk, Critical Findings เคลียร์, Remaining Risks มี Owner แต่ **ไม่ได้อนุญาต Deploy**
- **Fix & Reverify:** กลับไป [Implementation](./implementation.th.md) แล้วตรวจส่วนที่เปลี่ยนซ้ำ
- **Blocked — Discovery/Design:** Requirement/Contract ขัดกันหรือ Unknown ไม่ปลอดภัย
- **Stop/Defer:** Risk รับไม่ได้หรือข้อมูลยังไม่พอให้รับรอง

แต่ละ Increment เข้า-ออก Workflow นี้หลายรอบได้ ไม่ต้องรอโค้ดทั้ง Feature เสร็จ

## Pilot และคำถามรอ Review

ทดลอง Tiny Change, Medium Integrated Feature และ Simulated Multi-repo Change วัด Defect Detection, Rework, Integration Surprises, False Positives, Time-to-feedback และ Overhead

**รอ Review:** Solo ทำ Independent Review อย่างไร? งาน Medium/High-risk อะไรต้อง Sign-off มนุษย์? Evidence ขั้นต่ำใน GitLab/Jira คืออะไร? AI-assisted Diff Review ควรเป็น Default เมื่อไร?

**ยังเป็น Draft ไม่อนุมัติ Release, Auto-merge, Repo Policy หรือ AI Skill ใหม่**
