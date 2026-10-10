# Verification Workflow — v1.0 Release Candidate (ภาษาไทย)

- **สถานะ:** Review Ready / Release Candidate — รอ Owner Approve; ยังไม่ Accepted
- **Decision:** แยก Implementation และ Verification เป็นสอง Workflow โดยใช้ Execution Loop ร่วมกัน (2026-10-10)
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

## Prove It Works — ปรับจาก pstack และ mattpocock (เสนอสำหรับ v1.0)

นำแนวทาง **Evidence-based Verification** จาก [pstack Verify & Ship](https://github.com/cursor/plugins/blob/main/pstack/docs/guide/06-verify-and-ship.md), [pstack Interrogate](https://github.com/cursor/plugins/blob/main/pstack/skills/interrogate/SKILL.md), [pstack create-verification-skill](https://github.com/cursor/plugins/blob/main/pstack/skills/create-verification-skill/SKILL.md) และ [mattpocock Code Review](https://github.com/mattpocock/skills/blob/main/skills/engineering/code-review/SKILL.md) มาปรับใช้ โดย **ไม่บังคับใช้ Plugin หรือ Tool ตามต้นฉบับ**

### 1. พิสูจน์พฤติกรรมบน Surface ที่ตรงกับงานจริง

กำหนด Observable Done Check **ก่อน Implement** แล้วแยกว่า Build/Typecheck หรือ Unit Test ที่เขียวเป็นหลักฐานส่วนหนึ่ง แต่ยังไม่เท่ากับการใช้งานผ่าน Interface จริง

| งาน | หลักฐานที่ควรพิจารณา |
| --- | --- |
| **CLI / Job** | รันคำสั่งหรือ Job จริงกับ Input ตัวอย่างที่ปลอดภัย เทียบ Output, Exit Status และ Side Effects |
| **API / Data** | ตรวจ Contract, Permission, Error Semantics และ Read-back ค่าที่บันทึกเมื่อปลอดภัย |
| **Web / UI** | เดิน Flow ใน App ที่รันจริง ตรวจ State/Error และเก็บ Screenshot/Video เมื่อช่วยพิสูจน์ |
| **Refactor / Migration** | Replay Before/After Input และตรวจ Behavior/Compatibility |
| **Performance** | วัด Baseline และผลแบบเปรียบเทียบได้ ตรวจ Harness, Bottleneck, ความทำซ้ำได้และผล End-to-end |

เลือกความลึกตาม Risk **ไม่บังคับทุกงานต้องถ่ายวิดีโอหรือรัน E2E ทั้งหมด** ถ้าเข้าถึง Runtime ไม่ได้ ให้รายงาน **Inconclusive / Blocked / Not run** พร้อมเหตุผลและ Owner อย่าแต่งผลว่าผ่าน

### 2. Review สองแกนที่ไม่ควรปนกัน

**Spec / Acceptance:** โค้ดทำตาม Requirement/Acceptance ที่ตกลงไว้จริงหรือไม่? มีสิ่งที่ทำไม่ครบหรือ Scope Creep ไหม?

**Repository Standards / Quality:** Diff สอดคล้องกับ Coding Standards/Architecture/Patterns ของ Repo หรือไม่? แยก Violation ที่มีเอกสารอ้างอิงจาก Code Smell ที่เป็นเพียง Judgment และอย่าบังคับ Personal Style

เพิ่ม Lens เฉพาะที่เสี่ยง เช่น Auth, Privacy, Data Integrity, Concurrency, Retry, Errors, Observability, Migration และ Cross-service Contracts โดยระบุ **Diff Base และ Spec ที่มีแหล่งอ้างอิง** ถ้าไม่มี Spec ให้บอก Gap ไม่ใช่เดา

ใช้ Skeptical Independent Review หรืออีก Model ได้เมื่อคุ้มค่า แยก Findings เป็น **Act on / Consider / Noted / Dismissed** พร้อม Evidence/เหตุผล ห้าม Auto-apply เพียงเพราะ AI หลายตัวเห็นตรงกัน Engineer/Owner ยังต้องตัดสินและรับผิดชอบตาม Policy

### 3. สร้าง Verification Skill เฉพาะเมื่อคุ้มและตรวจสอบได้

ถ้าทีมต้องรัน UI/CLI/API แบบ Manual ซ้ำบ่อย อาจพิจารณาสร้าง **Project-local Verification Harness/Skill** จาก Test Tool ที่มีอยู่ก่อน โดยมี Contract:

**Launch → Doctor/Health Check → Drive Real Behavior → Capture Evidence → Cleanup ทรัพยากรที่สร้างเอง**

ใช้ Safe Fixtures, แยก Parallel Runs และต้องทดลองรันคำแนะนำที่ Skill สร้าง **End-to-end อย่างน้อยหนึ่ง Flow** ก่อนเชื่อถือ Feature Map ควรมีเมื่อช่วยการดูแลจริง

**ไม่สร้าง `.cursor/skills` หรือบังคับ Maintenance ทุกวันตาม pstack ทันที** หากจะย้ายแนวทางไปเป็น Skill ใน `puen-stack` ต้องมี Approval, Pilot, Owner/Maintenance และวัดเทียบ Baseline แบบไม่ใช้ Skill ก่อน

## Fixed Verification Gate, Risk-based Review และ Scoped Rework (เสนอสำหรับ AI-assisted v1.0)

**Q4=B — Risk-based Independent Review:** งาน Low-risk ทำ Review Pass แยกด้วยตนเองได้เมื่อ Policy ทีมอนุญาต งาน Medium-risk ให้มี Independent Diff Review งาน High-risk ขอ Human Peer/Domain Reviewer ที่เหมาะสมตาม Risk และ Policy แต่ **Human Verification Gate ต้องมีทุก Increment ที่ตกลงแล้ว** ไม่ว่า Review จะเข้มแค่ไหน

**Q5=B — Evidence by Change Type:** ต้องพิสูจน์ Acceptance บน Surface ที่ตรงกับงาน เช่น API/UI/CLI/Data พร้อม Checks และ Negative/Integration Cases ตาม Risk สิ่งที่ไม่ได้รันต้องระบุ **Not run / Blocked / Inconclusive** แยก Review ตาม Spec/Acceptance กับ Repo Standards/Quality ให้ชัด

**Q7=B — Scoped Rework:** Finding ที่อยู่ใน Scope/Design/Risk เดิมกลับไปให้ Implementation แก้และ Retest ตาม Impact ได้โดยไม่ต้อง Approve ทุก Edit แต่ Human ต้องดู Evidence และยืนยัน **Verification Gate** สุดท้าย ถ้ามีการเปลี่ยน Requirement, Scope, Contract, Design, Security/Data Risk อย่างมีนัยสำคัญต้องกลับไป Approve **Fixed Gate ก่อนหน้าที่ได้รับผลกระทบ** ใหม่

**Q6=A / Q8=B / Q9=A — MR Flow:** Formal Verification และ MR/CI Review อยู่หลัง Implementation Gate ที่ Human อนุมัติ AI อาจ Push Scoped Fix ไป Non-protected Work Branch ที่ได้รับ Publish Authorization แล้ว แต่ **ก่อนเปิด Draft MR ทุกครั้งต้องตรวจ Git Flow/Source/Target Branch ของ Repo และถาม Human ยืนยันเฉพาะ MR นั้น** ห้ามเดาว่า Target เป็น `master` และ **Human เท่านั้น** ที่เปลี่ยน Draft MR เป็น Ready หรือ Merge การ Deploy ต้องมี Approval แยก

**ใช้ Jira/MR เดิมบันทึก Evidence/Findings/Resolution/Gate Approver** ไม่บังคับสร้างเอกสารซ้ำ ดู Q1–Q9 ที่ [AI-assisted delivery decisions](./ai-assisted-feature-delivery.md)

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

## Checklist ก่อนอนุมัติ v1.0

1. Verification แยกจาก Developer Checks แต่เริ่มตรวจระหว่าง Implementation ได้
2. Shared Handoff อยู่ใน Jira/MR โดยไม่เพิ่ม Artifact บังคับ
3. Evidence, Review, Retest และ Human Risk Decision ชัดตาม Risk
4. Release Authorization และ AI Permissions รออนุมัติแยก

## Pilot และคำถามรอ Review

ทดลอง Tiny Change, Medium Integrated Feature และ Simulated Multi-repo Change วัด Defect Detection, Rework, Integration Surprises, False Positives, Time-to-feedback และ Overhead

**จุดตรวจสุดท้าย:** ระบุ Human Verification Gate Owner ให้ตรงกับทีมจริง ใช้ Jira/MR เดิมเก็บ Evidence และ Remaining Risks และเคารพ CI/Reviewer/Branch Policy ของ Repo ทดลอง Review Depth ตาม Risk ก่อนทำ Skill บังคับ

**Release Candidate รอ Owner Approve** ไม่อนุมัติ Release, Auto-merge, Repo Policy หรือ AI Skill ใหม่
