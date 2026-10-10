# Solution Design — Artifact Template (ภาษาไทย)

> **Artifact สำหรับ Feature-level Design Decision ไม่ใช่สิทธิ์เริ่มแก้โค้ด** งาน Risk ต่ำใช้ Jira Comment สั้น ๆ ได้ ADR แยกใช้เฉพาะ Decision สำคัญและอยู่ยาว ([ADR Template](../../templates/adr.md))
>
> Workflow: [Solution Design v1.0 (Accepted)](../../workflows/solution-design.th.md) · [English](./solution-design.md)

- **Feature / Parent Jira:** <Link>
- **สถานะ:** Proposed | In Review | Accepted for Planning | Needs Discovery / Spike | Deferred
- **Design Owner / Human Decider:** <ผู้มีอำนาจตัดสินจริง>
- **Requirement / Technical Discovery Evidence:** <Links>
- **Scope และ Risk Level:** <ครอบคลุมอะไร ทำไมเสี่ยงระดับนี้>

## 1. Problem, Outcome และ Constraints ที่ยืนยันแล้ว

- **Goal / Observable Acceptance:** <ผลลัพธ์อ้าง Source>
- **Confirmed Constraints:** <Business/API/Data/Security/Cost/Operation พร้อม Source>
- **Assumptions / Unknowns:** <สิ่งที่ยังต้องพิสูจน์ ไม่เดาตัวเลข NFR>
- **Non-goals / ระบบที่ไม่รวม:** <ขอบเขตอำนาจออกแบบ>

## 2. Decision Drivers และ Alternatives

| Option | ข้อดี | ต้นทุน / Failure Risks | Evidence หรือ Assumptions |
| --- | --- | --- | --- |
| <A> | <ประโยชน์> | <ความเสี่ยง> | <Source> |
| <B หรือเหตุผลที่ไม่จำเป็นต้องเทียบ> | <ประโยชน์> | <ความเสี่ยง> | <Source> |

**Recommended Option และเหตุผล:** <อธิบาย Trade-offs ไม่ใช่เลือกตาม AI Preference>

## 3. Proposed Solution และ Contracts

- **To-be Flow:** <Actor/Trigger → Service Owner → Data/Side Effects → Response>
- **Relevant API/Event/Data Contracts:** <Endpoint/Schema/Version พร้อม Links ที่มีจริง>
- **Domain Invariants และ Permissions:** <เงื่อนไขที่ห้ามผิด/ใครมีสิทธิ์/Privacy>
- **Failures, Retries, Concurrency และ Consistency:** <Scenario สำคัญ ไม่เดาผล>
- **Backward/Forward Compatibility และ Migration:** <Mixed-version Compatibility หรือ Unknown>
- **Diagram / ADR (Optional):** <เพิ่มเฉพาะมีประโยชน์>

## 4. Verification, Release และ Operations Implications

- **Acceptance / Negative / Boundary Cases:** <Expected Behavior ที่อ้าง Source>
- **Test & Integration Seams / Owner:** <BE/BFF/FE หรือ N/A พร้อมเหตุผล>
- **Migration/Rollout/Feature Flags/Monitoring/Recovery:** <ความเสี่ยงและ Owner>

## 5. Risks, Open Decisions และ Evidence ต่อไป

| Risk / Question | Impact | Mitigation / Spike / Evidence | Owner / Checkpoint |
| --- | --- | --- | --- |
| <คำถาม> | <Impact> | <สิ่งที่ต้องพิสูจน์> | <Owner> |

## 6. Human Solution Design Gate (ระดับ Feature)

- **Outcome:** Accepted for Planning | Needs more discovery/spike | Rework | Deferred
- **Human Decider / วันที่ / Approval Evidence:** <คนจริง เวลา และ Link หรือ Pending>
- **Chosen Option / Rationale / ความเห็นต่าง:** <Decision>
- **Accepted Boundaries / Exceptions:** <อนุมัติให้ทำอะไรและไม่ทำอะไร>
- **ส่งต่อ Delivery Planning:** <Work Item แรกที่ตรวจได้, Dependencies, Contracts, Test/Release Obligations>

**ขอบเขต:** Design Gate **ระดับ Feature** ไม่ได้อนุมัติโค้ดราย Work Item หรือข้าม Planning, Implementation, Verification และ Release Gates เมื่อมี Material Change ให้เปิดเฉพาะ Gate ที่ได้รับผลกระทบ
