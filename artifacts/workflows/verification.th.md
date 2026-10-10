# Verification / Independent Review — Artifact Template (ภาษาไทย)

> **ต่อ Jira Work Item ที่รับผิดชอบจริง ไม่ใช่ Story ทั้งหมดผ่าน** ใช้เป็นส่วนใน Jira/MR ได้ ไม่บังคับไฟล์รายงานใหม่ Verification-first/Preliminary Feedback เริ่มก่อนโค้ดได้ แต่ **Formal Independent Verification ต้องรอ Developer Checks ที่เกี่ยวข้องผ่านจริงและ Human Implementation Gate** (หรือ Alternative Evidence กรณีไม่มี Unit-test Seam ตามข้อยกเว้นที่ Human อนุมัติ)
>
> Workflow: [Verification v1.0 (Accepted)](../../workflows/verification.th.md) · [English](./verification.md)

- **Jira Work Item / Parent Story Acceptance:** <Source Links>
- **สถานะ:** Verification-first prep | Preliminary review | Formal review | Fix & reverify | Blocked | Verified for release review (human-gated)
- **Implementation Human Gate + Developer Checks ที่ผ่านจริง:** <Links, วันที่, ผลที่สังเกตได้>
- **Fixed Diff Base → Head / Repos:** <Commit SHAs และ Scope ไม่เชื่อ Implementer Summary อย่างเดียว>
- **Independent Reviewer / Risk / Policy:** <ผู้ตรวจ/Separate Skeptical Pass; High-risk ต้องมี Human Reviewer ตาม Policy>

## 1. Source-backed Acceptance และ Independent Oracle

| Scenario / Requirement | Expected Behavior จาก Requirement/Contract ที่ยืนยัน | Scope (Work Item หรือ Story ของผู้อื่น) | ต้องใช้ Evidence อะไร |
| --- | --- | --- | --- |
| <Scenario> | <Expected Result พร้อม Link> | <Owner> | <Observable Proof> |

- **Negative / Boundary / Permission Cases:** <Expected Outcomes>
- **Non-goals / External Integration Owner:** <ส่วนที่ไม่รวมและใครตรวจ>
- **Preliminary Feedback ก่อน Formal:** <ข้อเสนอเรื่อง Scenario/Contract/Partial Diff ไม่ใช่ Formal Verification Result>

## 2. Execution-safety Preflight — ก่อน Checks ที่มี Side Effects

- **Authorized Test Target:** <Environment, Service, DB, Endpoint และ Access Owner>
- **Permission:** <Approval ที่ชัดเจน; มี Credential ไม่เท่ากับมีสิทธิ์>
- **Data/Fixtures:** <Synthetic/Sanitized Identity, Read-only หรือ Isolated Resource>
- **Potential Side Effects:** <Writes, Events, Messages, Payments, Downstream Integrations ห้ามยิง Prod โดยไร้อนุญาต>
- **Cleanup / Rollback Boundary:** <ลบเฉพาะ Resource ที่ Test ครั้งนี้สร้างเอง ไม่ลบข้อมูลเดิม/ไม่รู้เจ้าของ>
- **Unsafe/Unavailable Check:** <ห้ามรัน ให้ Not run/Blocked พร้อม Owner แต่ Review แบบ Read-only ที่มีสิทธิ์ต่อได้>

## 3. Independent Verification — Four Safety Nets

| Safety Net | Reviewer ตรวจอะไร / Evidence | Pass / Fail / Not run / Blocked / Inconclusive |
| --- | --- | --- |
| **1. Requirement Correctness** | <เทียบ Code/Behavior กับ Acceptance จริง ไม่เอา Expected จาก AI เดา> | <สถานะ> |
| **2. Independent Diff + Test Quality** | <Review Fixed Diff ทั้ง Spec/Codebase Quality และ Assertion ที่ควรจับ Bug ได้> | <สถานะ> |
| **3. Risk-scaled Behavior / Integration Proof** | <ผลจริง API/DB/Contract/CLI/UI ตาม Scope; ระบุ Owner ของ External Integration> | <สถานะ> |
| **4. Evidence / Residual Risk Judgment** | <Reproducible Results, Gaps, Findings, Links และ Decision Owner> | <สถานะ> |

**Reviewer Separation:** <อ่าน Requirement ต้นทาง + Fixed Diff + Evidence ไม่เชื่อ Implementer Summary; Separate Skeptical Pass และข้อจำกัด; High-risk ตาม Human Review Policy>

## 4. Executed Checks และ Evidence เท่านั้น

| Required / Optional + Risk | Command / Check | Authorized Environment + Fixture | Expected vs Observed | Pass / Fail / Not run / Blocked / Inconclusive | Raw Evidence |
| --- | --- | --- | --- | --- | --- |
| <Required?> | <คำสั่งจริง> | <Env> | <ผล> | <Status> | <Link> |

- **Required Checks ที่ยังไม่รัน / Equivalent Evidence:** <Gap, เหตุผล, Human Owner ที่ Policy ให้อนุมัติ Alternative ได้ หรือ Blocked>
- **Cross-repo Contracts:** <Producer/Consumer/Version, อะไรที่ทดสอบจริง, Integration Owner>
- **Real-surface Proof กับ Proxy:** <Build/Unit Green เป็น Entry Condition ไม่ใช่ Proof Behavior ทั้งหมด>
- **Evidence Limitations:** <เวลา, Logs ที่ขาด, Environment เข้าไม่ได้, Inconclusive>

## 5. Findings และ Scoped Rework

| Severity / Blocking? | Spec / Codebase Quality Axis | Diff Location / Scenario / Evidence | Impact | Fix / Disposition | Owner |
| --- | --- | --- | --- | --- | --- |
| <Finding หรือ None observed> | <Axis> | <Path/Line/Proof> | <Risk> | <Action> | <Owner> |

**Reverification หลัง Fix:** <Scope ที่เปลี่ยน, Developer Tests ที่กระทบและรันซ้ำจริง, Independent Checks ที่รันซ้ำ, ปรับ Fixed Diff เมื่อจำเป็น>

**Material Risk / Requirement ใหม่:** <ย้อน Human Gate ก่อนหน้าที่ใช้ไม่ได้จริง พร้อม Owner>

## 6. Human Verification Gate — ต่อ Work Item

- **Reviewer Recommendation:** Verified for release review | Fix & reverify | Blocked | Stop/defer
- **Blocking Checks/Findings:** <None พร้อม Evidence หรือรายการที่ยังค้าง; Mandatory Proof ที่ขาดห้าม Verified>
- **Remaining Non-blocking Risks / Owner / Checkpoint:** <ระบุจริง>
- **Human Gate Decision / Owner / Date / Link:** <Approved / Rejected / Pending พร้อมหลักฐาน>
- **ส่งต่อ Release Coordination:** <Work Item Proof, Integration Gaps, Contract/Schema/Release Risks และ MR Links>

**Authority Boundary:** ผ่าน Human Verification Gate ไม่เท่ากับ Story ทั้งหมดผ่าน ไม่ได้อนุญาต Mark Draft MR Ready/Merge, Deploy Production, Enable Feature หรือเปลี่ยน Repo Policy ห้ามแต่งผลทดสอบ
