# My Engineer — Workflow Artifact Templates (ภาษาไทย)

**สถานะ:** Template สำหรับช่วยทำงานตาม **7 Workflows ที่ Accepted v1.0** แล้ว **ไม่ใช่ Human Approval, Execution Log หรือข้อบังคับให้สร้างไฟล์เพิ่ม** เอกสาร Workflow เดิมเป็นหลัก และต้องเคารพ Policy ของ Repo/บริษัทที่เข้มกว่าด้วย

**English index:** [README.md](./README.md) · **หน้ารวม Artifacts:** [../README.md](../README.md)

## วิธีใช้ — Jira/MR เป็นแหล่งบันทึกหลัก

1. **ระบุหน่วยงาน:** Requirement/Technical Discovery, Solution Design และ Delivery Planning ปกติอยู่ระดับ **Feature/Story** ส่วน Implementation และ Verification อยู่ระดับ **Jira Work Item ที่รับผิดชอบ** (Story, Sub-task, Bug หรือ Task) และ Release & Operations อยู่ระดับ **Release Bundle** ซึ่งอาจรวมหลาย Work Items/Repos
2. อ่านเฉพาะ **Accepted Workflow ที่เกี่ยวข้อง** และ **Artifact Template คู่กัน** ไม่ต้องยัดทุก Workflow เข้า Context ของ AI ทุกครั้ง
3. **คัดลอกเฉพาะส่วนที่มีประโยชน์** ไป Jira, MR หรือ Release/Incident Record เดิม งานเล็ก/ย้อนกลับง่ายใช้ Summary สั้น ๆ ก็พอ การแยก Markdown File เป็น Optional ไม่ใช่ Source of Truth ซ้ำ
4. เติม Placeholder ด้วย **Source Links, Human Owner, Commands, Environments และ Observed Results จริง** แยก `Confirmed / Inferred / Unknown`, `Not run / Blocked / Inconclusive`, `Pending` และ `N/A พร้อมเหตุผล` ตามจริง **ห้ามแต่ง Test Pass หรือหลักฐาน**
5. Acceptance ต้องอิง Requirement ไม่ใช่ Expected Behavior ที่ AI เดาจาก Code รักษา Working Tree และ Changes ที่ผู้ใช้อยู่เดิม ใช้เฉพาะสิทธิ์ Repo/Environment ที่ได้รับอนุญาต Artifact ไม่ได้ข้าม Human Gates และ MR/Release Permissions
6. เมื่อ Material Change ทำให้ Approved Feature Design/Planning หรือ Work Item Scope เดิมใช้ไม่ได้ เปิด **Human Gate เฉพาะ Decision ที่ได้รับผลกระทบ** และบันทึก Links กลับใน Record เดิม

## Artifact Map — เลือกใช้ตาม Workflow

| Stage / Accepted Workflow | Unit / Primary Record | English | ภาษาไทย | Human Decision / Boundary |
| --- | --- | --- | --- | --- |
| [Requirement Discovery](../../workflows/requirement-discovery.th.md) | Feature/Need; **Discovery Summary** | [EN](./requirement-discovery.md) | [TH](./requirement-discovery.th.md) | Validated **for exploration** ไม่ใช่อนุมัติเริ่ม Code |
| [Technical Discovery](../../workflows/technical-discovery.th.md) | Feature/Technical Question; **Technical Discovery Summary** | [EN](./technical-discovery.md) | [TH](./technical-discovery.th.md) | Evidence พอทำ Design / Targeted Spike |
| [Solution Design](../../workflows/solution-design.th.md) | Feature; **Solution Design Summary** | [EN](./solution-design.md) | [TH](./solution-design.th.md) | **Human Solution Design Gate ระดับ Feature** |
| [Delivery Planning](../../workflows/delivery-planning.th.md) | Feature + Next Work Item; **Delivery Plan / Shared Work View** | [EN](./delivery-planning.md) | [TH](./delivery-planning.th.md) | **Human Planning Gate ระดับ Feature** (Full Scope, Progressive Detail) |
| [Implementation](../../workflows/implementation.th.md) | **Assigned Work Item**; Developer Checks และ Code Handoff ใน Jira/MR | [EN](./implementation.md) | [TH](./implementation.th.md) | **Human Implementation Gate ราย Work Item**, Scoped Publish และ Human Confirm **ทุก Draft MR** |
| [Verification](../../workflows/verification.th.md) | **Assigned Work Item**; Independent Evidence ใน Jira/MR | [EN](./verification.md) | [TH](./verification.th.md) | **Human Verification Gate ราย Work Item** หลัง Developer-green และ Implementation Gate |
| [Release & Operations](../../workflows/release-operations.th.md) | **Release Bundle**; Bounded Release/Pipeline/Incident Record | [EN](./release-operations.md) | [TH](./release-operations.th.md) | **Human Go/No-Go ทุก Production Release; Human Confirm ใหม่ทุก Anomaly-driven Intervention** |

**ไม่สร้างเพิ่มเป็น Stage:** [Feature Delivery Lifecycle](../../workflows/feature-delivery-lifecycle.th.md) คือ Framework ภาพรวมที่ Accepted ไม่ใช่ Artifact ลำดับที่ 8 ส่วน [AI-assisted Feature Delivery Decision Record](../../workflows/ai-assisted-feature-delivery.md) ยังเป็น Draft และ [ADR Template](../../templates/adr.md) ใช้เฉพาะ Architecture Decision ที่สำคัญ/อยู่ยาว

## Prompt เริ่มต้นสำหรับ Codex / Claude Code

ใช้ Prompt นี้กับงานที่ได้รับมอบหมายและมีสิทธิ์ทำจริง **ไม่ใช่ Permission แบบเหมารวม**

```text
ทำงานตาม Accepted My Engineer Workflow สำหรับ Jira Work Item ที่ได้รับมอบหมาย
1. ระบุ Workflow Responsibility ที่ตรงกับ Task แล้วอ่านเฉพาะ Workflow
   และ artifacts/workflows/<matching-template>.th.md (หรือ .md หากต้องการ EN)
2. อ้าง Requirement, Source Code, Tests และ Repo ที่เข้าถึงจริง แยก Unknown
   และระบุ Repo/Environment ที่ยังไม่ได้ตรวจ ห้ามเดา Evidence
3. สร้าง/อัปเดต Artifact ที่จำเป็นให้น้อยที่สุดใน Jira/MR Record เดิมเมื่อทำได้
   ไม่สร้างไฟล์ซ้ำ ไม่ขยาย Scope โดยไม่ได้อนุญาต
4. ห้ามบอกว่า Test Pass หากไม่ได้รันจริง และห้ามถือ Placeholder เป็น Evidence
   หรืออ้าง Human Gate Approved โดยไม่มี Decision จริง
5. ขอ Human Approval ตาม Gate, ก่อนเปิด Draft MR ทุกอัน, ก่อน Publish ตามสิทธิ์,
   ทุก Production Release และทุก Anomaly-driven Operational Intervention
   ห้าม Auto-merge หรือ Deploy เอง
รายงาน Evidence ล่าสุด, Open Decisions, Next Safe Action และ Owner
```

**Privacy:** Repository Knowledge นี้เป็น Public ต้องใช้ Template ข้อมูลสมมติ ไม่ Commit ความลับบริษัท/ลูกค้า Password, Internal URLs, Real Production Logs หรือข้อมูล Jira ที่เป็นความลับลงที่นี่ Filled Artifact ของโปรเจกต์จริงต้องอยู่ใน Private System ที่ได้รับอนุญาต
