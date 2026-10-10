# Delivery Plan / Shared Work View — Artifact Template (ภาษาไทย)

> **Artifact ระดับ Feature ไม่ใช่การสร้าง Backlog ซ้ำ** ใช้ Jira Feature/Story เดิมและลิงก์ Work Items เป็นหลัก Default คือ **Hybrid — Full Scope, Progressive Detail**: รู้ภาพรวม Risk/Contracts ก่อน แล้วลงรายละเอียดงานถัดไปเมื่อใกล้ทำ
>
> Workflow: [Delivery Planning v1.0 (Accepted)](../../workflows/delivery-planning.th.md) · [English](./delivery-planning.md)

- **Feature / Parent Jira:** <Link>
- **สถานะ:** Proposed | Agreed for first Work Item | Active | Replanning | Complete | Blocked
- **Sources:** <Requirement, Technical Discovery, Accepted Solution Design Links>
- **Product / Scope Decision Owner:** <Human>
- **Engineering Delivery Owner:** <Human>
- **วันที่ / Revision:** <YYYY-MM-DD / Revision>
- **Planning Depth:** Full Scope + Progressive Detail (Default) | Exception: <เหตุผล/ผู้อนุมัติ>

## 1. Whole-feature Map — เห็นภาพรวมก่อนแยกงาน

- **Outcome และ Acceptance Boundaries:** <ผลลัพธ์สุดท้ายรวม Out of Scope>
- **Systems / Owners ที่เกี่ยวข้อง:** <BE/BFF/FE/API/Data/External ตามจริง>
- **Feature-level Dependencies & Contracts:** <Producer/Consumer/Data, Owner, Evidence>
- **Critical Unknowns / Integration / Security / Migration / Release Risks:** <สิ่งที่อาจทำให้แผนผิดและ Owner>
- **Later Deliverables:** <Outline ระดับสูง พร้อม Assumptions ยังไม่ใช่ Commit>
- **Shared Integration / Story Acceptance Owner:** <Human หรือ Pending; Work Item ผ่านไม่เท่ากับ Story ผ่าน>

## 2. Next Work Item — ลงรายละเอียดเท่าที่จะทำจริง

- **Jira Story / Sub-task / Bug / Task ที่มีอยู่:** <Link ไม่สร้าง Issue Type ใหม่>
- **Assigned Engineer / Authorized Repos:** <ผู้รับผิดชอบและ Repo ที่ได้รับมอบหมาย>
- **Goal / Observable Work Item Acceptance:** <อ้าง Parent Acceptance และ Approved Design>
- **Included Changes / Exclusions:** <ขอบเขต ไม่เอางาน BE/BFF/FE ของคนอื่นมาทำเอง>
- **Inputs / Dependencies:** <Contract Versions, Upstream, External Decisions>
- **Required Checks / Independent Verification / Reviewer:** <ต้องพิสูจน์อะไร ใครเป็น Owner>
- **Risk / Complexity / Feasibility:** <เหตุผลกำหนด Depth หรือ Spike>
- **Ready-to-start:** Ready | Spike first | Blocked | Replan; <Evidence/Owner>

## 3. Remaining Work Items — Outline และ Refine ตามเวลา

| Work Item / Slice | Outcome / Acceptance เบื้องต้น | Owner (Confirmed/TBD) | Dependencies | Refinement Trigger / Checkpoint |
| --- | --- | --- | --- | --- |
| <Jira Link หรือ Proposed Slice> | <Goal> | <Owner> | <Dependency> | <เมื่อไรต้องลงรายละเอียด> |

## 4. Integration, QA และ Release Coordination

- **Shared Contracts / Integration Checkpoints:** <Versions ของ Provider/Consumer และ Owner>
- **Cross-repo Tests / Story-level Evidence Owner:** <พิสูจน์ Integration อย่างไร; ถ้ายังรันไม่ได้ให้ Not run>
- **Migration / Deploy / Recovery Ordering:** <แผนจาก Evidence หรือ Unknown อย่าเดาลำดับ>
- **Monitoring / Security / Non-feature Work:** <งานที่จำเป็นต่อ Safe Delivery ไม่ผลักไปทำท้ายงาน>

## 5. Risks, Trade-offs และ Forecast (Optional)

| Risk / Blocker / Assumption | Impact | Mitigation / Next Evidence | Owner / Trigger |
| --- | --- | --- | --- |
| <Risk> | <Impact> | <Action> | <Owner> |

**Estimate / Forecast (เมื่อจำเป็น):** <Method, Confidence/Range, Capacity Assumptions, วันที่; ห้ามเดาตัวเลข>

**Capacity / Scope Trade-offs:** <สิ่งที่ Human ตัดสินจริง>

## 6. Human Delivery Planning Gate (ระดับ Feature)

- **Decision:** Agreed for first Work Item | Spike first | Return to Discovery/Design | Defer/De-scope
- **Human Decider / วันที่ / Approval Evidence:** <Approval จริงหรือ Pending>
- **Feature-wide Scope / Risks ที่อนุมัติ:** <ขอบเขตและ Unknowns สำคัญ>
- **Approved Next Work Item / Owner:** <Jira Link และ Boundary>
- **Next Refinement Checkpoint / Trigger:** <เมื่อไรต้องลงรายละเอียด>
- **Changes จากแผนก่อนหน้า:** <เหตุผลและ Gate ที่กระทบหากเป็น Material Change>

**ขอบเขต:** Gate นี้อนุมัติ **ภาพรวม Feature + Work Item ถัดไป** ไม่ได้อนุมัติรายละเอียดทุก Task ในอนาคต Implementation/Verification ยังต้องมี **Human Gate ราย Work Item แยก** ส่วน MR/Production Permissions ก็แยกเช่นกัน
