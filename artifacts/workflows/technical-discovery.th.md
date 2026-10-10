# Technical Discovery — Artifact Template (ภาษาไทย)

> **Template นี้ไม่ใช่หลักฐานว่าได้สำรวจจริงแล้ว** ระบุไฟล์ Commit ผลตรวจและ Owner ที่ตรวจจริง ห้ามอ้างว่าอ่าน Repository ที่ไม่มีสิทธิ์/ไม่ได้เปิดดู ใช้ Jira/Feature Record เดิมแทนไฟล์ใหม่ได้
>
> Workflow: [Technical Discovery v1.0 (Accepted)](../../workflows/technical-discovery.th.md) · [English](./technical-discovery.md)

- **Feature / Jira Link:** <Requirement Discovery / Jira>
- **สถานะ:** Draft | Evidence sufficient for design | More exploration needed | Blocked
- **ประเภท:** Brownfield | Greenfield | Hybrid
- **Technical Owner / Reviewer:** <ผู้รับผิดชอบจริง หรือ TBD>
- **Evidence Snapshot:** <Repos, Branch/Commit, Runtime/Environment, วันที่; N/A หากยังไม่ได้ตรวจ>
- **คำถามที่ต้องสำรวจ:** <อะไรที่ต้องรู้ก่อน Design?>

## 1. System Context และ Boundaries

- **Components/Services และ Owner ที่ตรวจพบจริง:** <Service/Repo/Path/Owner หรือ Unknown>
- **Runtime, Storage, Access และ Deploy Assumptions:** <แยก Confirmed กับ Unknown>
- **Technical Constraints / NFRs:** <มี Source; ห้ามสร้างค่า Latency/Throughput เอง>

## 2. Behavior จริง หรือ Greenfield Feasibility

- **Brownfield As-is Flow:** <Trigger → API/Service → Data/Event → ผลที่เกิดจริง พร้อม Evidence>
- **Greenfield Feasibility:** <ความสามารถ/ข้อจำกัดที่พิสูจน์ได้ ไม่แต่ง As-is Flow>
- **สิ่งที่ตรวจแล้ว:** <อ่าน/ค้น/Trace/Experiment ที่ได้รับอนุญาต พร้อมผลจริง>
- **สิ่งที่ยังไม่ได้ตรวจ:** <ข้อจำกัดหรือส่วนที่เข้าไม่ถึง อย่าเดาสถาปัตยกรรม>

## 3. Dependencies, Interfaces และ Contracts

| Producer / Consumer | API / Event / Schema / Invariant | Evidence (Commit/Path/Link) | Confirmed / Inferred / Unknown | Owner |
| --- | --- | --- | --- | --- |
| <Boundary> | <Contract> | <Source> | <สถานะ> | <คน/ทีม> |

**ข้อจำกัด Cross-repo:** <Repo ที่ AI/Engineer ยังไม่ได้ตรวจ และใครเป็นคนยืนยัน>

## 4. Impact และ Risks ที่เกี่ยวข้อง

- **Blast Radius:** <Users, Services, Workflows ที่อาจได้รับผล>
- **Security / Authorization / Privacy / Data Correctness:** <Finding หรือ N/A พร้อมเหตุผล>
- **Concurrency / Performance / Retries / Observability:** <Finding หรือ N/A พร้อมเหตุผล>
- **Migration / Compatibility / Recovery:** <สิ่งที่รู้และยังไม่พิสูจน์>

## 5. Evidence Ledger

| Finding | Evidence จริง: Path/Commit/Command/Result/Owner | Confirmed / Inferred / Unknown |
| --- | --- | --- |
| <สิ่งที่พบ> | <หลักฐาน> | <สถานะ> |

**Executed Checks:** <Commands/Permissions/Environment พร้อมผล Pass/Fail/Not run/Blocked จริง>

## 6. Critical Unknowns และ Handoff

| คำถาม | ถ้าเข้าใจผิดกระทบอะไร | Next Experiment / Clarification | Owner / วันที่ |
| --- | --- | --- | --- |
| <คำถาม> | <Impact> | <การตรวจต่อ> | <Owner> |

- **Recommended Route:** Solution Design | Time-boxed spike | Requirement clarification | Blocked
- **เหตุผล / Remaining Risks:** <ทำไม Evidence เพียงพอหรือยังไม่พอ>
- **Technical Reviewer / Decision / Date:** <ผลจริงหรือ Pending>
- **ส่งต่อ Solution Design:** <ข้อจำกัดที่พิสูจน์แล้ว, Alternatives, Interfaces และ Open Risks>

**ขอบเขต:** Evidence พอทำ Design **ไม่ได้อนุมัติ Implementation** ADR/Diagram/POC เป็น Optional Artifact ที่สร้างเฉพาะเมื่อช่วยตัดสินใจจริง
