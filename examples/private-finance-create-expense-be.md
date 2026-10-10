# Example — Private Finance: Create Expense Transaction (Backend)

- **Status:** Hypothetical Reference Example — **Not Implemented / Not Tested / Not a Pilot**
- **Track:** My Engineer — Independent Verification Safety Nets
- **Example Work Item ID:** `BE-001` *(รหัสสมมติ ไม่ใช่ Jira จริง)*
- **Scope:** Backend (BE) เท่านั้น; ไม่มีการมอบหมาย BFF/FE หรือ Cross-repo Implementation
- **Implementation technology:** Go + PostgreSQL เป็นเพียงตัวเลือกสำหรับการยกตัวอย่าง **ไม่ใช่ข้อกำหนดของ Project จริง**
- **Purpose:** ใช้คุยและ Review Workflow; ผู้ใช้จะเลือก Project สำหรับ Pilot/Implementation จริงภายหลังแยกต่างหาก

## 1. Story / Work Item (สมมติ)

**Story:** ผู้ใช้บันทึกรายรับ–รายจ่ายส่วนตัวเพื่อดูรายการและยอดใช้จ่ายรายเดือน

**Work Item:** Implement Backend API สำหรับสร้าง Expense Transaction หนึ่งรายการของผู้ใช้ที่ผ่านการยืนยันตัวตน

**Example input:** จำนวน `199.50 THB`, หมวดหมู่ `Food`, วันที่ `2026-10-10`.

**Non-goals:** ไม่ Implement หน้า UI, BFF, Monthly Dashboard, การโอนเงิน, การเชื่อมบัญชีธนาคาร หรือระบบเงินจริง

> ตัวอย่างนี้ **ไม่มี Spec, Codebase, Jira หรือ API Contract จริงที่ผ่านการรับรอง** ทุก Scenario ต่อไปนี้เป็น *Proposed Acceptance* สำหรับสาธิตเท่านั้น ไม่ใช่ Approved Requirements

## 2. Verification-first — proposed Acceptance / Expected Behavior

| Scenario | Expected behavior ที่เสนอให้ตกลงกับ Owner |
| --- | --- |
| Valid expense | บันทึกรายจ่าย `199.50 THB` ตามผู้ใช้ที่ Authenticated อย่างถูกต้องและอ่านค่ากลับได้ |
| Zero / negative amount | ปฏิเสธ Input และไม่เกิดการบันทึกข้อมูล |
| Other-user access | Request ไม่สามารถระบุ Owner เพื่อสร้างหรือเข้าถึงรายการแทนผู้อื่นโดยไม่ได้รับอนุญาต |
| Persistence failure | ไม่แสดงว่า Success เมื่อ Database ไม่บันทึกข้อมูลสำเร็จ |
| Duplicate request / retry | **Open Decision** — ต้องตกลง Idempotency และ Duplicate Policy ก่อนระบุ Expected Result |
| Currency precision | **Open Decision** — ต้องตกลง Money Representation (เช่น integer minor units หรือ exact decimal) ก่อนออกแบบ Storage/Assertions |
| Date / category / auth semantics | **Open Decision** — ตรวจ Timezone, Category Ownership, Validation, Error Contract ตาม Domain ก่อนเขียน Tests |

Verification รับผิดชอบการตั้งคำถามและหา Expected Results จาก Requirement/Contract ที่ Owner ยืนยัน ไม่เดาผลลัพธ์จากโค้ดของ AI เอง

## 3. Implementation — Developer Safety Net

**ลำดับทดลองที่ตั้งใจใช้ในอนาคต** เมื่อมี Project จริงและได้รับอนุญาต:

1. รับ Acceptance/Test Intent ที่ตกลงแล้ว และสำรวจ Codebase/Repo Conventions
2. ทำ Vertical Slice; ใช้ TDD Red → Green → Refactor เมื่อ Test Seam เหมาะสม
3. เขียนและรัน Unit / Regression Tests ที่เกี่ยวข้องจริง รวม Build/Lint/Typecheck ตาม Repo
4. **Unit Tests ต้องผ่านก่อน Formal Independent Verification**; ถ้า Fail → กลับไปแก้, ถ้ารันไม่ได้ → `Blocked / Not run` พร้อมเหตุผล
5. ขอ Human Implementation Gate ตามข้อตกลง Work Item; ไม่สร้าง MR หรือ Push โดยพลการ

**ตัวอย่าง Unit Test Categories (ไม่ใช่ผลทดสอบจริง):** Amount Validation, Owner Binding/Authorization, Service Error Handling, Repository Interface Behavior

## 4. Independent Verification — four safety nets

| Safety Net | สิ่งที่ควรท้าทาย | Example proof เมื่อมี Test Environment จริง |
| --- | --- | --- |
| **1. Requirement Verification** | API ตรง Acceptance ที่ยืนยันหรือไม่? มี Scenario สำคัญหายไปไหม? | Scenario-to-check map พร้อม Expected / Observed Outcome |
| **2. Independent Code Review** | Logic/Architecture ปลอดภัยไหม? Assertions ใน Unit Tests จับผลผิดจริงไหม? | Diff Review ฝั่ง Spec และ Codebase Quality แยกกัน: Money Precision, Auth, Error, Transaction, Unintended Effects |
| **3. Risk-based Behavioral / Integration Verification** | ผ่าน Mock Unit Tests แล้ว Behavior กับ Boundary จริงเป็นอย่างไร? | ยิง API กับ Isolated Test DB → read-back Amount/Owner; ลอง Unauthorized, Invalid Amount และ DB Failure ตาม Risk |
| **4. Evidence & Risk Assessment** | อะไรรันจริง, ไม่รัน, ล้มเหลว, ยังเสี่ยงอยู่? | Commands, Environment, Outcomes, Findings, Evidence Links และ Human Decision Recommendation |

### ตัวอย่าง Defects ที่ Reviewer อาจจับได้ (สมมติ)

- ใช้ `float64` สำหรับการคำนวณเงินโดยไม่มีข้อตกลงเรื่อง Precision/Representation ทำให้เกิดความเสี่ยง Round-off
- Unit Test Mock ยืนยันแค่ว่า Repository ถูกเรียก แต่ไม่ได้พิสูจน์ว่า Expense ถูก Persist หรือ Read-back ถูกต้อง
- ผูก Owner ID จาก Request โดยไม่ยืนยันสิทธิ์ ทำให้บันทึกรายการแทนผู้ใช้อื่นได้
- ตอบ Success ก่อนแน่ใจว่า Commit สำเร็จ หรือจัดการ Retry ผิดจาก Policy ที่ตกลง

ทั้งหมดเป็น **Risk Hypotheses / Test Ideas** ไม่ใช่ Bug ที่ตรวจพบจริง

## 5. Example Verification Evidence Report (blank; not executed)

| Check | Status | Evidence |
| --- | --- | --- |
| Requirement/Acceptance mapping | **Not run** | No approved spec |
| Unit/Regression Tests | **Not run** | No implementation / runner |
| Independent Code Review | **Not run** | No diff |
| Actual API + DB behavior | **Not run** | No application / test DB |
| Negative/Authorization cases | **Not run** | No executable environment |
| Cross-repo / Story Integration | **Not run / Out of assigned BE scope** | External dependency owner not established |

**Decision:** `Not evaluated — reference example only`. ไม่มีการกล่าวอ้างว่า Tests ผ่าน, Independent Verification ผ่าน, หรือ Work Item Ready for Release

## 6. Future actual pilot (intentionally separate)

- ผู้ใช้จะเลือก **Project จริงในภายหลัง**; อย่านำตัวอย่าง Private Finance นี้ไปตั้งต้นสร้าง Repo/Code/Tasks โดยไม่ได้รับคำสั่ง
- เมื่อเลือก Project แล้ว ต้องเริ่มจาก Requirement, Repo, Work Item, Scope, Permissions และ Acceptance ของ **Project นั้น**; อาจไม่เกี่ยวกับ Private Finance เลย
- วัด Verification ว่าเจอ Defects ที่ Unit Tests ไม่พบหรือไม่, False Positives, Human Decisions, Rework, Time และ Token Cost
- Human Gates และ GitLab Policy คงเดิม: ตรวจ Git Flow ตาม Repo, ขออนุมัติแยกก่อนทุก Draft MR, Human เท่านั้นที่ Mark Ready/Merge; ไม่มี Auto Deploy

**Related workflows:** [Implementation (TH)](../workflows/implementation.th.md) · [Verification (TH)](../workflows/verification.th.md) · [Track A decisions](../workflows/ai-assisted-feature-delivery.md)
