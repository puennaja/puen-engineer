# Verification Workflow — v1.0 Release Candidate (ภาษาไทย)

- **สถานะ:** Review Ready / Release Candidate — รอ Owner Approve; ยังไม่ Accepted
- **Decision:** แยก Implementation และ Verification เป็นสอง Workflow โดยใช้ Execution Loop ร่วมกัน (2026-10-10)
- **วันที่:** 2026-10-10
- **Track:** A — Build My Engineer
- **คู่กับ:** [Implementation](./implementation.th.md), [Delivery Planning v1.0](./delivery-planning.th.md)
- **พื้นฐาน:** [Feature Delivery Lifecycle v1.0](./feature-delivery-lifecycle.th.md)
- **ต้นฉบับอังกฤษ:** [Verification](./verification.md)
- **ถัดไป:** Release & Operations (ยังไม่ได้ออกแบบ)

## หน่วยที่ต้อง Verify: Work Item ตาม Jira (ข้อตกลง Track A)

**Work Item = Jira Story, Sub-task, Bug หรือ Task ที่ Engineer รับผิดชอบจริง** โดยต้องระบุ Acceptance และหลักฐานที่ตรวจได้ งานของ Backend Engineer อาจมีแค่ BE, BFF หรือ BE+BFF ใน Scope ที่ตกลง ส่วน FE อาจเป็น Developer อีกคน **Verification ต้องตรวจตาม Work Item ที่ได้รับมอบหมาย** ไม่เพิ่ม Scope งานข้าม Repo/FE เอง

**ก่อนเขียนโค้ด:** อ้าง Story Acceptance และ Contracts ที่ตกลงแล้วเพื่อกำหนด Expected Behavior, Negative Cases และ Test Seam เฉพาะส่วนที่รับผิดชอบ งาน BE-only ตรวจ Data/API Semantics, Authorization, Idempotency และ Contract ได้โดยไม่ต้องอ้างว่าระบบ FE ผ่าน Integration แล้ว ส่วน BFF ตรวจ Mapping, Error Semantics และ Contract กับ Consumer/Provider ด้วย Fixture หรือ Integration ที่เหมาะสม

**หลังมีโค้ด:** ตรวจ Behavior จริง, คุณภาพ Unit Tests, Contract/Integration Tests ภายใน Scope และ Independent Review ตาม Risk ส่วน **Story-level Integration Verification** ระหว่าง BE/BFF/FE เป็นงานประสานการตรวจตาม Acceptance ของ Story เมื่อจำเป็น โดยมี Owner ของทีมที่เหมาะสม ไม่ใช่ Human Gate ใหม่ที่ต้องมีทุก Story และไม่ใช่เหตุผลให้ AI ไปทำงาน FE ของคนอื่นเอง ถ้าเชื่อมทดสอบไม่ได้ ให้ระบุ **Not run / Blocked / External Owner** อย่างชัดเจน **Work Item Verify ผ่าน ไม่เท่ากับ Story ทั้งหมดผ่าน**

**Human Gates:** Design/Planning อนุมัติระดับ Feature/Story ส่วน Implementation/Verification อนุมัติ **แต่ละ Work Item ที่ตกลงแล้ว** ถ้า Work Item คือ Story เองก็ใช้ Jira รายการเดียวบันทึกการตัดสินใจหลาย Gate โดยไม่สร้างเอกสารซ้ำ เอกสาร [Delivery Planning ที่ Accepted](./delivery-planning.th.md) ยังใช้คำว่า Increment ในความหมายของ Delivery Slice ตาม Agile ได้ แต่หน่วยที่ AI รับงานคือ **Work Item** ดู [Track A Decisions](./ai-assisted-feature-delivery.md)

## เป้าหมายและเวลาเริ่ม

สร้าง **หลักฐานที่เชื่อถือได้ว่าโค้ดตรง Acceptance ไม่ทำลาย Contract สำคัญ และได้รับการ Review อย่างเหมาะสม**

Verification เป็น Workflow ที่มีความรับผิดชอบและ Exit Decision ของตัวเอง แต่ **เริ่มได้ระหว่าง Implementation** และส่ง Findings ย้อนให้แก้ได้หลายรอบ ไม่ใช่รอทำทุกอย่างจบแบบ Waterfall

Implementation รับผิดชอบการสร้าง Change และ Developer Checks; Verification รับผิดชอบการประเมิน Acceptance, Failure, Integration และ Review Findings งาน Solo คนเดียวอาจรับหลายบทบาท แต่ต้องแยกการ Self-check กับ Independent Assessment ให้ชัด AI Review ไม่ใช่การอนุมัติของมนุษย์

## Execution Loop ร่วมกัน — ข้อตกลงการส่งต่องาน (เสนอสำหรับ v1.0)

Workflow สองตัว **แยกหน้าที่** แต่ใช้ **วงจรทำงานร่วมกันเดียว** ต่อ Work Item:

```text
Feature Design/Plan ที่ Approved → Work Item
   ↓
Verification-first: Acceptance, Expected Outcomes, Test Seams
   ↓
Implementation: Vertical Slice → TDD เมื่อเหมาะ → Unit Tests PASS + Developer Checks
   ⇄ Verification: Test Quality, Behavior Evidence, Independent Review
   ↳ Findings → Scoped Fix → Reverify
   ↓
Human Verification Gate → Release Review (แยก)
```

**Verification เริ่มได้ก่อนเขียนโค้ด:** กำหนดพฤติกรรมและหลักฐานจาก Requirement/Contract ที่ยืนยันก่อน ไม่ใช่เพิ่ม Approval Gate ใหม่ **เริ่ม Verify ได้ทันทีที่มีสิ่งให้ตรวจ:** Acceptance Example, Test Strategy, API/Contract หรือ Partial Diff ก็เริ่มได้แล้ว ไม่ต้องรอ Code ทั้ง Feature จบ หรือรอป้าย Ready for Verification แบบบังคับ Developer Tests ยังคงอยู่ใน Implementation ส่วนการประเมินอย่างอิสระและการตัดสินคุณภาพหลักฐานเป็นหน้าที่ Verification

**ส่งต่อผ่าน Jira/MR เดิมเพียงชุดเดียว** ไม่บังคับสร้างเอกสารใหม่:
- **Identity / Scope:** Work Item, Acceptance IDs, Repo/Diff Links, Exclusions
- **Changes / Risks:** Boundary ที่เปลี่ยน, Contracts/Data/Migration และ Failure Cases
- **Evidence:** Checks ที่รันจริงพร้อมผล/Environment, สิ่งที่ **Not run / Blocked**
- **Feedback:** Findings พร้อม Severity/หลักฐาน/Owner, ผลแก้และตรวจซ้ำ
- **Decision:** ทำต่อ / พร้อมตรวจเพิ่ม / Fix & Reverify / Blocked / Verified for Release Review โดยมนุษย์รับผิดชอบ Residual Risk

**กติกาวนซ้ำ:** แก้จุดไหนให้ Reverify จุดนั้นและ Dependencies ที่ได้รับผลกระทบ แต่ถ้าผลกระทบขยายต้องตรวจเพิ่ม ถ้าเจอ Unknown สำคัญย้อนกลับ Discovery/Design/Planning ทั้งสอง Workflow **ไม่มีอำนาจ Deploy** หรือแก้ Acceptance เงียบ ๆ

**Roles:** งาน Solo ทำได้ทั้งสองหน้าที่ แต่ควรแยก Self-check กับ Review Pass งาน Medium/High-risk ควรมี Reviewer ที่เป็นอิสระจริงเมื่อทำได้หรือ Local Policy กำหนด AI สองตัวเห็นตรงกันไม่ได้แปลว่า Test ผ่านหรือ Human อนุมัติ

**Scale:** งานเล็ก Reversible บันทึกใน MR สั้น ๆ ได้ งาน Cross-repo/Finance/Security ต้องมีหลักฐาน Contract, Tests, Review และ Release Risks มากขึ้น โดยไม่ต้องสร้าง Ticket ซ้ำ

## Verification-first: เจ้าของ Acceptance และการพิสูจน์อย่างอิสระ (Design Direction)

**ก่อน Implement:** Verification อ้าง Requirement/Domain/Contract ที่ตกลงแล้ว กำหนด **Acceptance Scenarios, Expected Results, Negative Cases, Test Seams และ Proof Surface** ไม่เดา Expected Result จากโค้ดที่ AI เขียน ถ้า Requirement ไม่ชัดต้องถาม Decision Owner ก่อนเขียน Test ที่กลายเป็นข้อกำหนดผิด

หากมี Harness ที่เหมาะ Verification อาจสร้าง **Executable Acceptance/Contract Test เล็ก ๆ** ซึ่ง Fail เพราะ Behavior ที่ยังไม่มีจริง ถ้ายังไม่มี Harness หรือ Test แพง ใช้ Given–When–Then/Repro ที่ Review ได้แทน Test Fail เพราะ Environment พังนับเป็น **Blocked/Inconclusive** ไม่ใช่ Red ที่ใช้พิสูจน์ Requirement

**ระหว่าง Implement:** ฝั่ง Implementation เป็นเจ้าของ Code, Unit/Regression Tests และ TDD แบบ Red → Green → Refactor เป็น Vertical Slices Verification ช่วยท้าทาย Assertions, API Contract และ Partial Diff ได้โดยไม่เพิ่ม Approval Gate

**หลังมี Code:** Verification ตรวจว่า Developer Tests จับ Behavior ผิดได้จริงหรือไม่ และรัน Independent Behavior Checks บน API/UI/CLI/Data ตามความเสี่ยง พร้อม Review Diff เทียบ Spec และ Repo Standards Unit Tests/Build ผ่านอย่างเดียว **ยังไม่ใช่หลักฐานเพียงพอ** Findings ส่งกลับ Implement เพื่อ Fix/Retest ตาม Q7 และการเปลี่ยน Acceptance/Design สำคัญต้องกลับไปหา Human Gate ที่ได้รับผลกระทบ

Human Implementation/Verification Gates ราย Work Item ยังมีอยู่ ใช้ Jira/MR เดิมเก็บ Scenario/Expected Outcome/Check/Result ไม่บังคับเอกสารหรือ Skill เพิ่ม อ้างอิง [mattpocock TDD](https://github.com/mattpocock/skills/blob/main/skills/engineering/tdd/SKILL.md) และ [pstack Verification](https://github.com/cursor/plugins/blob/main/pstack/docs/guide/06-verify-and-ship.md)

## Formal Independent Verification เริ่มหลัง Unit Tests ผ่าน (ตกลง 2026-10-10)

**มีสองช่วงที่ต่างกัน:** Verification-first เริ่มก่อนเขียนโค้ดเพื่อกำหนด Acceptance, Expected Behavior และ Test Seams ส่วน Preliminary Feedback ตรวจ Partial Diff/Contract ระหว่าง Implementation ได้ แต่ **ยังไม่ถือว่าเข้า Formal Independent Verification**

**Formal Independent Verification ต้องมีหลักฐานว่า Unit/Regression Tests ที่เกี่ยวข้องและ Developer Checks ตาม Repo ผ่านจริงก่อน** พร้อม Command, Environment และ Result รวมถึงผ่าน Human Implementation Gate ตามที่ตกลงไว้ ถ้า Tests Fail ให้กลับไปแก้ที่ Implementation; ถ้ารันไม่ได้ให้รายงาน **Not run / Blocked** ห้ามสรุปว่าเขียว ถ้างานไม่มี Unit Test Seam ที่เหมาะจริง ให้มีเหตุผลและ Alternative Proof ซึ่ง Human รับทราบใน Implementation Gate เดิม ไม่ใช่ข้าม Tests เฉย ๆ

เมื่อเข้า Formal Independent Verification แล้วจึง **ท้าทาย Tests ที่ผ่าน** ว่า Assertions จับ Behavior ตาม Requirement ได้จริงหรือไม่ พร้อม Negative/Edge Cases, API/DB/BFF Contract Evidence และ Independent Diff Review **Unit Tests ผ่านไม่เท่ากับโค้ดถูกต้อง** หากพบ Finding ส่งกลับ Implementation → แก้ไข → รัน Unit Tests ที่เกี่ยวข้องใหม่ → Independent Reverify และผ่าน Human Verification Gate เดิม ไม่มี Gate ใหม่เพิ่ม

## Independent Verification v1.0 — Safety Net 4 ด้าน (ข้อตกลง 2026-10-10)

หลัง **Unit/Regression Tests ที่เกี่ยวข้องและ Developer Checks ผ่านจริง** และผ่าน Human Implementation Gate เดิมแล้ว Formal Independent Verification ประเมิน **4 ด้าน** ต่อ Work Item ที่รับผิดชอบ **ทุกงานต้องพิจารณาทั้ง 4 ด้าน แต่ความลึกของ Tests/Review แตกต่างตาม Risk และความเกี่ยวข้อง** Unit Tests เขียวเป็นเงื่อนไขเข้า ไม่ใช่หลักฐานว่าทุกอย่างถูกต้อง

| Safety Net | ต้องตอบอะไร / ผลลัพธ์ขั้นต่ำ | ตัวอย่าง BE / BFF |
| --- | --- | --- |
| **1. Requirement Verification** | โค้ดตรง Acceptance และ Parent Story Contract ไหม? ผูก Scenario กับ Evidence ระบุ Case ที่ตกหล่น หรือความเข้าใจที่ไม่ตรง Requirement | Activity Log เก็บ Actor/Action/Timestamp, Permission, Error Semantics |
| **2. Independent Code Review** | Diff ถูกต้อง ปลอดภัย และสอดคล้อง Codebase ไหม? Review เทียบ Spec และ Quality/Architecture **แยกสองแกน** ตรวจคุณภาพ Assertions ของ Developer Tests ด้วย | Transaction, Data Integrity, Error Handling, Security, Side Effects, Complex Code |
| **3. Risk-based Behavioral / Integration Proof** | รันแล้ว Behavior จริงบน Boundary ที่เปลี่ยนเป็นอย่างไร? เลือก API/DB/Contract/Integration Tests ตาม Risk และรายงานสิ่งที่ไม่ได้รัน | Update Status แล้วอ่าน Log จาก DB, Mapping ของ BFF↔BE, Negative Cases ตามความเสี่ยง |
| **4. Evidence & Risk Assessment** | ตรวจอะไรจริง ผ่าน/ไม่ผ่าน/ไม่ได้รันอะไร เหลือความเสี่ยงอะไรให้ใครรับผิดชอบ? | Command/Result/Environment, Finding/Resolution, Owner และ Recommendation |

**เลือกความลึกตาม Risk:** งาน Low-risk มี Acceptance Mapping, Test-quality/Diff Review และ Evidence Decision แบบกระชับ พร้อม Focused Behavior Check เมื่อมี Seam เหมาะสม งาน Medium-risk เพิ่มการพิสูจน์ Behavior/Contract/Integration และ Review ที่แยกจาก Implementer งาน High-risk เช่น Authorization, Sensitive Data, Concurrency, Migration, Money หรือการเปลี่ยนที่ย้อนกลับยาก ต้องมี Negative/Security/Data/Integration Checks และ Human Reviewer ที่เหมาะตาม Policy **จำนวนบรรทัดที่แก้ไม่ใช่ตัวกำหนด Risk**

**อย่าแค่ติ๊กว่าผ่าน:** Build/Lint/Unit Tests ที่เขียวเป็น Developer Evidence แต่ไม่ใช่ผล Independent Verification ถ้า Test Assertions ไม่สามารถจับ Behavior ที่ผิดได้ ต้องระบุ Finding ห้ามใช้คำอธิบายจาก AI แทนหลักฐานรันจริง ส่วน Mutation, Property-based, Fuzzing, Load และ Security Testing ขั้นสูงเป็น **ทางเลือกตาม Risk** ไม่บังคับทุก Work Item และการ Verify BE ผ่านไม่ได้แปลว่าทั้ง Story ที่มี FE ของคนอื่นผ่าน Integration แล้ว

**การตัดสิน:** Verification เสนอ Verified for Release Review / Fix & Reverify / Blocked / Stop-Defer จากหลักฐาน ส่วน Human ยังเป็นผู้ตัดสิน **Verification Gate ต่อ Work Item** การผ่าน Gate ไม่ได้อนุญาต Auto Ready/Merge/Deploy ใช้ Jira/MR เดิมเก็บผล ไม่สร้างไฟล์รายงานบังคับใหม่

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

## ระดับการอนุมัติและการวนกลับอย่างปลอดภัย (Track A ตกลงแล้ว)

**Feature-level Human Gates:** Solution Design อนุมัติแนวทางเทคนิคและ Contracts หลัก ส่วน Delivery Planning อนุมัติ Full Feature Scope, Critical Dependencies/Risks และแผนแบบ Progressive Detail สำหรับ Work Item ถัด ๆ ไป **Per-Work-Item Human Gates:** Implementation อนุมัติ Scope/Diff ของ Work Item ก่อน Publish อย่างเป็นทางการ และ Verification อนุมัติ **Acceptance Evidence, Review Findings และ Remaining Risks ของแต่ละ Work Item** แยกกัน

Work Item ถัดไปที่อยู่ใน **Feature Scope ที่อนุมัติแล้ว** สามารถแตก Acceptance และ Verification Checks ให้ละเอียดเพิ่มได้โดยไม่ต้องขอ Approve Design/Planning ซ้ำเพียงเพราะเริ่มงานรอบใหม่ Work Item เดียวอาจครอบคลุมหลาย Repos: ใช้ Feature-level Source of Truth เชื่อม Repo Diff/MRs และ Integration Evidence

**Q7=B — Loopback:** Finding ที่แก้ใน Scope/Design/Risk เดิมของ Work Item ส่งกลับ Implementation เพื่อ Fix และ Retest ตาม Impact ได้ **ไม่ต้อง Approve ทุก Edit หรือเริ่ม Feature Gate ใหม่** แต่ Human ยังคงต้องตัดสิน Verification Gate ของ Work Item นั้น ถ้า Finding ส่งผลให้ Requirement/Feature Scope, Architecture, API/Data Contract สำคัญ, Security/Data Risk หรือ Delivery Constraints เปลี่ยนอย่างมีนัยสำคัญ ให้ย้อนกลับไป Approve **เฉพาะ Gate ก่อนหน้าที่ได้รับผลกระทบ**

เก็บ Feature/Work Item Approval พร้อม Human Owner และ Risk ใน Jira/MR เดิม **ก่อนเปิด Draft MR แต่ละอัน** ต้องตรวจ Git Flow ของ Repo/งานและถาม Human แยกเสมอ แม้ Work Item มีหลาย Repos การ Verify ผ่านไม่ได้อนุญาตให้ Mark Ready, Merge หรือ Deploy แทนมนุษย์

## Fixed Verification Gate, Risk-based Review และ Scoped Rework (เสนอสำหรับ AI-assisted v1.0)

**Q4=B — Risk-based Independent Review:** งาน Low-risk ทำ Review Pass แยกด้วยตนเองได้เมื่อ Policy ทีมอนุญาต งาน Medium-risk ให้มี Independent Diff Review งาน High-risk ขอ Human Peer/Domain Reviewer ที่เหมาะสมตาม Risk และ Policy แต่ **Human Verification Gate ต้องมีทุก Work Item ที่ตกลงแล้ว** ไม่ว่า Review จะเข้มแค่ไหน

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

แต่ละ Work Item เข้า-ออก Workflow นี้หลายรอบได้ ไม่ต้องรอโค้ดทั้ง Feature เสร็จ

## Checklist ก่อนอนุมัติ v1.0

1. Design/Planning Approve ระดับ Feature และ Implementation/Verification Approve ราย Work Item ชัดเจน โดยไม่ขออนุมัติซ้ำหรือข้าม Gate
2. Verification แยกจาก Developer Checks แต่เริ่มตรวจระหว่าง Implementation ได้
3. Shared Handoff อยู่ใน Jira/MR โดยไม่เพิ่ม Artifact บังคับ
4. ครบ **Safety Net 4 ด้าน**: Requirement, Independent Code Review/Test Quality, Risk-based Behavior Proof และ Evidence/Risk Decision
5. Release Authorization และ Permission ของระบบจริงต้องแยกอนุมัติ

## Pilot และคำถามรอ Review

ทดลอง Tiny Change, Medium Integrated Feature และ Simulated Multi-repo Change วัด Defect Detection, Rework, Integration Surprises, False Positives, Time-to-feedback และ Overhead

**จุดตรวจสุดท้าย:** ระบุ Human Verification Gate Owner ให้ตรงกับทีมจริง ใช้ Jira/MR เดิมเก็บ Evidence และ Remaining Risks และเคารพ CI/Reviewer/Branch Policy ของ Repo ทดลอง Review Depth ตาม Risk ก่อนทำ Skill บังคับ

**Release Candidate รอ Owner Approve** ไม่อนุมัติ Release, Auto-merge, Repo Policy หรือ AI Skill ใหม่
