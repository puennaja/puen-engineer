# การส่งมอบฟีเจอร์โดยใช้ AI ช่วย (AI-assisted feature delivery)

- **สถานะ:** Draft — รอการรีวิว
- **สร้างเมื่อ:** 2026-10-09
- **ขอบเขต:** Workflow วิศวกรรมส่วนบุคคล (ตัวอย่างทั่วไป ไม่มีข้อมูลลับของบริษัท)

## เป้าหมาย
พัฒนาฟีเจอร์อย่างมีประสิทธิภาพโดยใช้ AI ช่วย และยังคงให้ Developer เป็นเจ้าของงานและรับผิดชอบความถูกต้อง

## ข้อตกลงด้านการออกแบบ Track A — grill-me รอบ 1 (ตกลงเมื่อ 2026-10-10)

**สถานะการตัดสินใจ:** เจ้าของโปรเจกต์ยืนยันข้อเลือกเหล่านี้เพื่อ **ออกแบบต่อ** เท่านั้น เอกสารนี้รวมถึง Implementation/Verification v1.0 Release Candidates ยังคง **Draft / ยังไม่ Accepted** และไม่เปลี่ยน Feature Delivery Lifecycle ที่ Accepted แล้วแบบย้อนหลัง

| หัวข้อ | ตัวเลือก | ผลต่อ Design |
| --- | --- | --- |
| Q1 — เลือก Skill | **C: Hybrid routing** | AI แนะนำ/เลือก Skill หรือขั้นตอนตาม Goal, Done Check, Proof และ Risk ผู้พัฒนาสามารถ Override และต้องอนุมัติการตัดสินใจสำคัญตาม Gate ไม่บังคับ Skill Chain ตายตัว |
| Q2 — สิทธิ์ Coding | **B: Scoped autonomy** | AI แก้ Code เฉพาะจุดและรัน Non-destructive Checks ที่อนุญาตได้ใน Work Item/Scope ที่อนุมัติ โดยไม่ต้องถามทุกบรรทัด แต่ Scope ใหม่ การเปลี่ยน Architecture การกระทำใช้สิทธิ์สูง/ทำลายข้อมูล การเผยแพร่ภายนอก และขยายสิทธิ์ ต้องขออนุมัติ สิทธิ์ Git ดู Q8 ส่วน Command Allowlist/การบังคับใช้ต้องยึด Local Repo Policy |
| Q3 — Human Checkpoints | **A: Fixed gates** | ต้องให้คนอนุมัติการออกจาก **Solution Design → Delivery Planning → Implementation → Verification** แต่ละ Gate ก่อนเข้าสู่ความรับผิดชอบถัดไป นี่คือ Stage-boundary Approvals ไม่ใช่ Approvals รายไฟล์/ราย Test Implementation และ Verification ส่ง Preliminary Read-only/Provisional Feedback ระหว่างกันได้ แต่ห้ามข้าม Gate โดยปริยาย ใช้ Company Policy ที่เข้มกว่าเมื่อมี |

**การตีความ Loop และ Gate (สรุปใน Q7):** Preliminary Review และ Developer Checks วนปรับได้ก่อนการออกจาก Stage อย่างเป็นทางการ Implementation/Verification Gates ยังบังคับ การแก้ไขใน Approved Scope ไม่ต้องอนุมัติทุก Edit แต่ Material Change ต้องเปิด Gate ก่อนหน้าที่ได้รับผลกระทบใหม่ Fixed Gates ไม่ให้สิทธิ์ Merge/Deploy

## ข้อตกลงด้านการออกแบบ Track A — grill-me รอบ 2 (ตกลงเมื่อ 2026-10-10)

**สถานะ:** ยืนยันเพื่อออกแบบต่อ **ยังไม่ใช่อนุมัติ** AI-assisted Workflow หรือ Implementation/Verification Release Candidates และไม่ได้ยกเลิก Fixed Human Checkpoints ของรอบ 1

| หัวข้อ | ตัวเลือก | ผลต่อ Design |
| --- | --- | --- |
| Q4 — Independent Review | **B: Risk-based independent review** | งานเสี่ยงต่ำใช้ Deliberate Self-review ได้ถ้านโยบายอนุญาต งานเสี่ยงกลางต้อง Review Pass แยก งานเสี่ยงสูงต้อง Human Peer/Domain Review ตาม Local Policy AI Independent Review เป็นส่วนเสริม ไม่แทน Human Sign-off ที่บังคับ **Human Verification Gate ยังคงบังคับทุก Work Item** |
| Q5 — Verification Evidence | **B: Evidence by change type** | พิสูจน์ Changed Observable Behavior บน CLI, API, UI, Data หรือ Execution Surface ที่เกี่ยวข้อง ร่วมกับ Automated Checks และ Negative/Integration Scenarios ตามความเสี่ยง บันทึก Commands/Results จริง รวมช่องว่าง **Not run / Blocked / Inconclusive** ไม่ต้อง Full E2E/Video ทุก Change |
| Q6 — เวลาเปิด GitLab Draft MR | **A: หลัง Implementation Gate; ยืนยันโดยมนุษย์ราย MR (ชี้แจง 2026-10-10)** | ต้องผ่าน Human Implementation Gate ก่อน AI ตรวจ Branching/Merge Strategy ของ Repo/Work Item จริง จากนั้น **ถามคนก่อนสร้าง Draft MR ทุกอัน** ยืนยัน Source, Target และ Task/Release Context ต้องได้รับ Human Approval ราย MR และ Scoped Publish Authorization แยกตาม Q8 ห้ามสมมติว่า Target เป็น `master`, `main`, `develop` หรือถือ Approval ครั้งก่อนแทน Draft MR ไม่ใช่ Merge/Deploy Approval |

**ความสอดคล้อง:** Q3 และ Q4 เป็นคนละเรื่อง Human Gate **มีเสมอ** แต่ความลึกการ Review/Verification **ตาม Risk**; Self-review ไม่ยกเว้น Gate ส่วน Q6 ไม่ให้ Push แบบไร้ขอบเขต เพราะ Q8 จำกัดเฉพาะ Authorized Scope/Work Branch

### Mandatory Draft MR Preflight — การปรับ Q6 ที่ตกลงกันชัดเจน

1. **สำรวจจริง ไม่เดา:** ตรวจ Repo Contribution/Branching Rules, Work Item/Jira Context, Current Branch/Upstream, Destination Release/Integration Branch, Cross-repo Dependencies/Stacked MRs เมื่อเกี่ยวข้อง ใช้หลักฐาน **จาก Repo/Task จริง** และเปิดเผยความกำกวมเมื่อกฎไม่มีหรือขัดกัน
2. **เสนอ MR ให้ชัด:** Repo, Work Item, Source Branch, **Proposed Target Branch**, เหตุผลที่ตรงกับ Git Flow ของงาน, Draft Status และ Ordering/Dependent MR Constraints โดยมนุษย์ตัดสิน Target สุดท้าย
3. **ถามและรอ Explicit Confirmation ทุกครั้งก่อนสร้าง MR แต่ละอัน** แม้ Target ดูชัด Implementation Gate ผ่านแล้ว หรือ MR ก่อนใช้ Branch นี้แล้ว ห้ามใช้ Blanket Permission
4. **สร้างเฉพาะ Draft MR ที่ยืนยันแล้ว** หลังผ่าน Implementation Gate และมี Publish Permission เชื่อม Task รายงาน MR URL ถ้าคนปฏิเสธหรือเปลี่ยน Branch ต้องประเมินใหม่ การเปลี่ยน Target ภายหลังก็ต้องถามใหม่
5. **ห้าม Bypass:** ไม่เดา `master`/Default Branch ไม่สร้าง MR อื่นเพื่อเลี่ยงขั้นตอน ไม่ Auto-merge/Deploy

**ความต่างของ Permission:** สิทธิ์เปิด MR **แยกจาก** สิทธิ์ Local Edit, Commit และ Push ซึ่ง Q8 กำหนด การมี Scoped Git Autonomy ไม่ยกเว้น Human Confirmation ราย MR การตัดสินใจนี้เป็นข้อกำหนด Design ของ Flow ที่ Pending **ไม่ใช่การ Approve Implementation/Verification Workflow**

## ข้อตกลงด้านการออกแบบ Track A — grill-me รอบ 3 (ตกลงเมื่อ 2026-10-10)

**สถานะ:** เจ้าของโปรเจกต์ยืนยันเป็น Design Constraints **ไม่ใช่** อนุมัติ Implementation v1.0, Verification v1.0, Repo Automation Settings หรือ Release Authority

| หัวข้อ | ตัวเลือก | ผลต่อ Design |
| --- | --- | --- |
| Q7 — Rework และ Fixed Gates | **B: Scoped Rework** | หลัง Implementation Gate สามารถแก้ Findings **ภายใน Scope/Design/Risk Envelope เดิมที่ตกลง** ผ่าน Implementation ↔ Verification ได้โดยไม่อนุมัติทุก Edit ต้องรัน Impacted Checks ซ้ำ เก็บ Finding/Fix/Retest Evidence และผ่าน **Human Verification Gate** ถ้ามี Material Change ต่อ Scope, Accepted Design, Contract, Security/Data Risk หรือ Delivery Plan ให้ **เปิด Gate ก่อนหน้าที่ได้รับผลกระทบใหม่** |
| Q8 — Git Commit/Push | **B: Scoped Git Autonomy** | AI Local Commit บน Work Branch ที่ตกลงตาม Repo Conventions ได้ AI Push ไปยัง **Non-protected Work Branch ที่ระบุและอนุญาตเท่านั้น หลัง Explicit Publish Authorization ที่ Implementation Gate** Subsequent Normal Push สำหรับ Scoped Rework บน Branch เดิมอาจอยู่ในสิทธิ์เดิมตาม Company Policy ห้าม Unapproved Force-push, History Rewrite, Protected-branch Update, New Remote หรือ Destructive Git Operations **สิทธิ์ Push ไม่ใช่สิทธิ์เปิด MR** |
| Q9 — MR Completion | **A: Human Controlled** | **มนุษย์ที่มีสิทธิ์เท่านั้น** เปลี่ยน Draft MR เป็น Ready และ Merge AI รายงาน CI/Review Status เสนอ Fix และทำ Readiness Summary ได้ ห้าม Auto-ready/Merge/Deploy แม้ CI Green หรือ Verification Gate ผ่าน Local Branch Protection/Approver/Release Policy มีอำนาจสูงสุด |

### Harmonized Execution Contract (รอบ 1–3)

1. **AI เลือก Skill ตามงาน** ภายใต้ Approved Goal/Task Engineer Override ได้ ไม่บังคับ pstack/grill-me Chain
2. **Fixed Human Gates** ใช้ที่ทางออกของ Design, Planning, Implementation, Verification เป็น Approval ของ Deliverable ที่ตกลง ไม่ใช่ Permission Prompt ทุก Code Edit/Test Risk-based Review เปลี่ยนความลึก ไม่เปลี่ยนการมี Human Gate
3. **หลัง Implementation Approval** Authorized Publish Operation สามารถ Push Work Branch ที่กำหนด ก่อน **Draft MR ทุกอัน** AI ตรวจ **Git Flow จริงของ Repo/Work Item** เสนอ Source + Target + เหตุผล + Jira Link และ **รอ Human Confirmation เฉพาะ MR** ห้ามสมมติ `master`, `main`, `develop` สิทธิ์ Push ไม่แทน MR Confirmation
4. **ระหว่าง Verification** Provisional Review/Scoped Fix วนซ้ำได้ แต่ต้อง Retest ตามสัดส่วนและผ่าน Human Verification Gate หาก Material Change ให้เปิด Invalidated Upstream Gates
5. **หลัง Verification** AI แสดง Evidence/Readiness Summary **มนุษย์เท่านั้น** Mark Ready/Merge ส่วน Merge และ Production Release เป็นคนละ Decision

### Consistency Review — ประเด็นสำหรับ v1.0 ไม่ใช่ตัวเลือกที่ยังไม่ตกลง

- **ไม่มีข้อขัดแย้งโดยตรง** ใน Q1–Q9 เมื่อแยก Fixed Human Gate vs Risk-proportionate Evidence; Scoped Push vs Per-MR Permission; Preliminary Review vs Formal Verification Gate
- **Gate Granularity สำหรับ Jira Ownership:** Design/Planning อนุมัติระดับ Feature/Story; Implementation/Verification ระดับ Work Item (Story/Sub-task/Bug/Task ตามที่รับ) Material Change เปิดเฉพาะ Gates ที่ถูกทำให้ไม่ถูกต้อง Human Approver Identity และ Evidence Storage ตัดสินตาม Team/Work Item โดยใช้ Jira/MR ที่มี
- **เรื่องเฉพาะ Repo ไม่ใช่ Framework Global:** Actual Target Branch/Git Flow, Required Reviewers/CI, Commit Convention, Allowed Push Credentials และ Branch Protection ต้องค้นจาก Repo/Task จริง **ถามคนเมื่อขาดหรือกำกวม** ห้ามเดาใน Generic Public Workflow
- **งานอนาคตที่ไม่ Block การรีวิว Workflow v1.0 ทั้งสอง:** Detailed Release & Operations, GitLab Connector/Permission Enforcement และ `puen-stack` Skill Design/Evaluation ไม่มีการแก้ GitLab Settings จริงหรือมอบสิทธิ์

## Track A — ข้อตกลงสุดท้ายเรื่อง Gate Granularity (ตกลง 2026-10-10)

**Owner Decision:** ใน **AI-assisted Track A** ให้มนุษย์อนุมัติ **Solution Design และ Delivery Planning ระดับ Feature** แต่อนุมัติ **Implementation และ Verification สำหรับแต่ละ Work Item ที่ตกลงและพิสูจน์ Completion ได้แยกกัน** นี่คือข้อสรุปเรื่อง Granularity **ไม่ใช่** อนุมัติ Implementation/Verification v1.0 Release Candidates

| Gate | หน่วยอนุมัติ | สิ่งที่คนยอมรับ |
| --- | --- | --- |
| Solution Design | **Feature** | Overall Technical Direction, Boundaries/Contracts, Major Risks และ Decision Ownership |
| Delivery Planning | **Feature** | Whole-feature Scope, Critical Dependencies/Integration/Rollout Constraints, Initial Implementable Work Item และ **Progressive-detail Rule ของ Work Item ถัดไป** |
| Implementation | **แต่ละ Work Item** | Approved Scope, Actual Diff, Developer Checks, Known Gaps และความพร้อมสำหรับ Authorized Publishing/Formal Verification |
| Verification | **แต่ละ Work Item** | Acceptance/Evidence, Contract/Review Findings, Resolved/Blocking Issues และ Owned Residual Risks |

**Work Item ถัดไป:** ก่อน Implement ให้ Refinement รายละเอียดเทียบกับ **Feature Plan ที่อนุมัติแล้ว** บันทึก Acceptance, Impacted Contracts, Owner และ Verification Approach ใน Work Item ที่มี **ไม่ต้องผ่าน Feature Design/Planning Gates ซ้ำ** ถ้าเป็นเพียงการลงรายละเอียดภายใน Approved Boundaries หาก Readiness/Scope/Ownership กำกวมให้ถามเจ้าของ ไม่ถือว่า Approval เก่าครอบคลุม Requirement ใหม่โดยพลการ

**Material Change / Reopen Rule:** หากหลักฐานใหม่เปลี่ยน Approved Feature Scope, Outcome, Architecture, Critical API/Data Contract, Security/Privacy/Risk Assumptions, Dependencies หรือ Release Constraints **อย่างมีนัยสำคัญ** หยุด Change ที่กระทบและขอ Renewed Approval **เฉพาะ Upstream Gates ที่ Invalidated** ไม่ Restart ทุก Gate เพราะ Scoped Bug Fix/Test Addition/Planned Work Item Findings ภายใน Scope หลัง Implementation Gate แก้ตาม Q7 ได้ พร้อม Targeted Retest และ Normal Work Item Verification Gate

**Cross-repo / Multiple MR:** Work Item ครอบคลุมหลาย Repo ได้ โดยประสาน Evidence ระดับ Work Item พร้อม Repo-specific Links/Integration Outcomes แต่ **Draft MR แต่ละอันยังต้อง Human Confirmation ของตนเอง** หลังตรวจ Git Flow ของ Repo/Work Item นั้น Publish, Ready/Merge และ Release Permissions แยกจากกัน

**Recording Rule:** บันทึก Feature Approvals และ Work Item Gate Decisions ใน Jira/Issue/MR เดิม พร้อม Owner, Scope, Date และ Important Residual Risks ไม่บังคับ Duplicate Document หรือ Global Approver Identity/Branch Convention

## Track A — ข้อตกลงเรื่องศัพท์ Work Item (ตกลง 2026-10-10)

**Owner Decision:** ใช้ **Work Item** แทน *Increment* เป็นหน่วยหลักของ AI-assisted **Implementation/Verification** หมายถึง **งานวิศวกรรมที่ได้รับมอบหมาย มี Boundary ชัด และ Observable Completion/Verification Contract** ซึ่งแมปกับ **Jira Story, Sub-task, Bug หรือ Task เดิม** ตาม Assignment จริง **ไม่ใช่** Jira Issue Type ใหม่ เอกสารบังคับใหม่ หรือ End-to-end Deployable Feature Slice เสมอไป

**Typical Ownership:** Engineer อาจรับเฉพาะ Merchant Service (BE) Sub-task, Admin BFF Sub-task หรือทั้งคู่ ส่วน FE เป็นงานอีกคน ค่าเริ่มต้น Write/Edit/Test Scope ของ AI คือ **Assigned Work Item และ Authorized Repo(s)** ไม่ให้ Implement FE หรือไล่ทุก Repo อัตโนมัติ Work Item เดียวเกี่ยวหลาย Repo ได้ **ต่อเมื่อ Approved Scope ต้องการจริง** และต้องเห็น Shared Contracts, Acceptance และ Integration Dependencies ระดับ Story

**Execution และ Gates:**
- **Feature/Story level:** คนอนุมัติ Shared Solution Design และ Delivery Planning (เมื่อเกี่ยวข้อง) รวม Interface Contracts และ Ownership
- **Work Item level:** Verification-first นิยาม Source-backed Behavior/Expected Outcomes และ Proof; Implementation เขียน Code/Developer Tests (TDD เมื่อเหมาะ); Verification Challenge Actual Results แบบอิสระ Human **Implementation/Verification Gates** ใช้กับแต่ละ Work Item ตาม Fixed-gate Policy
- **Story integration:** ตรวจ Cross-repo/Cross-owner Acceptance/Contracts เมื่อจำเป็นโดยมอบหมาย Responsibility ให้ทีม **ไม่เพิ่ม Global Gate อัตโนมัติ** หรือบังคับ Engineer คนเดียวทำ FE/BFF/BE ที่ไม่ได้รับมอบหมาย ถ้า E2E ต้องอาศัย Repo/Service ที่เข้าไม่ถึง บันทึก **Not run / Blocked / Externally owned** พร้อม Integration Owner **ห้าม** บอกทั้ง Story Accepted เพราะ Backend Work Item ผ่าน
- **Work Item Boundary:** กำหนด Observable Outcome, Non-goals, Risk, Impacted Repos/Contracts, Evidence และ Jira Reference ถ้า Sub-task คลุมเครือเช่น “Implement BE” ให้หา Local Acceptance จาก Parent Story ร่วมกับ Human/Contract Owner ไม่ให้ AI เดาเอง

**ขอบเขตคำศัพท์:** [Delivery Planning v1.0](./delivery-planning.md) ที่ Accepted แล้วอาจยังใช้คำ Agile ว่า *Increment* สำหรับ Iterative Delivery Slice **ไม่แก้ Accepted Workflow ย้อนหลัง** แต่ใน Implementation/Verification Release Candidates ปัจจุบัน **Work Item** แทน *Increment* ในฐานะ Ownership/Approval Unit

**Guardrails เดิมยังมีทั้งหมด:** Hybrid Skill Choice, Scoped Edits/Rework/Git, Human Approval **ทุก Draft MR** หลังตรวจ Repo Git Flow, Humans-only Ready/Merge, No Automatic Deployment ข้อตกลงคำศัพท์นี้ **ไม่ทำ Workflow Accepted** หรือเปลี่ยน Repo Permissions จริง

## Track A — Verification-first และ TDD-enabled Loop (ตกลงทิศทาง 2026-10-10)

**Owner Design Decision:** Verification เป็นความรับผิดชอบแยกต่างหากที่ **เริ่มก่อนมี Code ได้** ไม่ใช่ QA หลัง Implementation เท่านั้น ภายใน Approved Feature Design/Plan ให้เริ่มแต่ละ Work Item ด้วย **Source-backed Acceptance Examples, Observable Expected Outcomes, Negative/Boundary Cases, Test Seams และ Evidence Strategy** จาก Verification

**ลำดับแบบวนซ้ำ (ไม่มี Gate ใหม่):** Verification กำหนด Test Intent → Implementation ทำ Small Vertical Slices พร้อม Developer-owned Unit/Regression Tests ใช้ **TDD Red → Green → Refactor เมื่อมี Reliable Test Seam และคุ้มค่า** → Verification ทดสอบ Actual Behavior อย่างอิสระ ประเมิน Test Quality/ท้าทาย Diff → ส่ง Findings ให้ Scoped Implementation/Retest การมี Executable Pre-code Acceptance/Contract Test ดีเมื่อทำได้แต่ไม่บังคับ Harness ที่เสียไม่ถือเป็น Valid Red Test ห้าม AI สร้าง Expected Output จาก Generated Implementation ของตัวเองเพียงอย่างเดียว

**แบ่งความรับผิดชอบ:** Verification รับผิดชอบ *ต้องพิสูจน์อะไร* และ *Evidence น่าเชื่อถือไหม*; Implementation รับผิดชอบ *เขียน Code อย่างไร* และ *Developer Tests* คน/AI เดียวอาจทำทั้งคู่ได้ แต่ Independent Assessment ต้อง Challenge Author Assumptions ความเห็นตรงกันของหลาย Model ไม่ใช่ Proof หลีกเลี่ยง Forced TDD เมื่อ Signal ต่ำหรือ Setup Cost สูง

**Gates/GitLab Rules ไม่เปลี่ยน:** Feature-level Human Design/Planning Approvals, Per-Work-Item Human Implementation/Verification Gates, Scoped Rework/Git Permissions, ตรวจ Git Flow/ถามก่อน **ทุก Draft MR**, Humans-only Ready/Merge การเริ่ม Verification-first ไม่เท่ากับผ่าน Gate **ห้าม** สร้าง Skills, แก้ AGENTS.md หรือทำ Candidate Workflow Accepted เพียงเพราะบันทึก Direction นี้

## AI-assisted Sequence ที่เสนอ — ตรงกับข้อตกลง

1. **Intake and Evidence:** ทำ Outcome, Done Checks, Constraints, Current Behavior ให้ชัด AI Route Procedures ที่เกี่ยวข้อง
2. **Feature-level Solution Design Gate:** คนอนุมัติ Major Technical/Contract Decisions ระดับ Feature
3. **Feature-level Delivery Planning Gate:** คนตกลง Whole-feature Scope/Risks, Work Item แรกโดยละเอียด และ Progressive Refinement สำหรับงานถัดไป
4. **Verification-first ของแต่ละ Work Item:** ก่อน Code กำหนด Source-backed Scenarios, Expected Results, Test Seam, Proof Strategy เป็น Preparation **ไม่ใช่ Gate ใหม่**
5. **Per-Work-Item Implementation:** AI ทำเฉพาะ Work Item ที่ตกลง รัน Developer Checks/Local Commits ได้; **Human Implementation Gate** ตรวจ Changed Scope/Diff/Evidence ของงานนั้น
6. **Scoped Publish + Draft MR:** Push ได้เมื่อมี Specific Work-branch Publish Authorization; **ตรวจ Git Flow จริง เสนอ Source/Target และถามมนุษย์ก่อนสร้าง Draft MR แต่ละอัน** ห้ามเดา Branch
7. **Per-Work-Item Verification ↔ Scoped Rework:** Actual Evidence, Risk-proportionate Independent Review, CI/MR Feedback, Fixes, Targeted Retests; **Human Verification Gate** ยืนยันผล; Material Changes เท่านั้นที่ Reopen Invalidated Upstream Gates
8. **Human MR Completion:** คนเท่านั้นเปลี่ยน Draft เป็น Ready และ Merge; Release/Operations Approval แยกต่างหาก

ลำดับนี้เป็นเพียง **AI-assisted Adapter ที่เสนอ** ไม่ใช่ Mandatory Waterfall ของ Engineering ทุกประเภท Tests/Provisional Reviewer Feedback เริ่มระหว่าง Implementation ได้ และวนกลับตามผลตรวจ

## กรณีหลาย Repository
เมื่อ Feature ครอบคลุมหลาย Repo ใช้ Shared Feature Design เดียว ระบุ Changes, Version/Contract Dependencies และ Test Responsibilities แยกราย Repo **อย่าสมมติว่า Tool เห็น Repos นอก Workspace อัตโนมัติ**

## Agent Instructions
ควรมี `AGENTS.md` ระดับ Project แบบสั้นที่ระบุ Local Conventions/Entry Points เก็บ Procedural Skills แยกออกไป โหลดตามจำเป็นจาก `puen-stack`

## Inputs เฉพาะตอนลงมือ และ Decisions ที่เลื่อนไว้

**Q1–Q9 ถูกบันทึกแล้ว** ก่อน Pilot ใน Team/Repo จริงต้องตกลงจากบริบทจริงดังนี้:

- **Human Gate Owner/Approval Record** ระดับ Feature/Work Item รวมการบันทึก Formal Approval ใน Jira/MR เดิม **ไม่สมมติ** Policy จาก Job Title
- **Work-item Git Flow**, Actual Source/Target Branch, MR Dependencies, Required CI, Peer-review Rules ตรวจราย Repo/Task และต้อง Human Confirm MR Target ทุกราย
- **Publish Authority/Credentials**, Permitted Work Branch และ Execution Environment ระบุ Authorized Scope ไม่ถือว่า Generic Bot Push ได้
- **Team Commit Convention/MR Content** ยึด Repo Rules ไม่บังคับ Global Template

เรื่องที่เลื่อนไว้และ **ไม่ใช่เงื่อนไขสำหรับอนุมัติเอกสาร Workflow สองฉบับ:** `puen-stack` Skill Authoring/Evaluation, GitLab Automation Integrations และ Release & Operations Workflow การอนุมัติเอกสาร Workflow **ไม่ใช่การมอบสิทธิ์ Repository จริง**

**สถานะ:** Draft / Design Decisions ตกลงเพื่อรีวิวต่อ; Implementation และ Verification v1.0 ยังรอ Explicit Owner Approval
