# Requirement Discovery — Artifact Template (ภาษาไทย)

> **นี่คือ Template ไม่ใช่ Evidence หรือ Approval ที่เกิดขึ้นแล้ว** คัดลอกลง Jira/Product Record เดิม หรือ Markdown เมื่อจำเป็น งานเล็กและ Risk ต่ำใช้ Summary สั้น ๆ ได้ ห้ามแต่งคำพูดผู้ใช้ หลักฐาน หรือเป้าหมายตัวเลขเอง
>
> Workflow: [Requirement Discovery v1.0 (Accepted)](../../workflows/requirement-discovery.th.md) · [English](./requirement-discovery.md)

- **หัวข้อ / ลิงก์:** <ฟีเจอร์ ปัญหา หรือ Jira URL>
- **สถานะ:** Draft | Validated for exploration | More discovery required | Deferred | Declined
- **Requester / Decision Owner:** <ชื่อ/บทบาท หรือ TBD>
- **ผู้จัดทำ / วันที่:** <Owner / YYYY-MM-DD>
- **แหล่งข้อมูล:** <ลิงก์หรือหลักฐานที่ปกปิดข้อมูลอ่อนไหวแล้ว>

## 1. ปัญหาและผู้ได้รับผลกระทบ

**Problem Statement:** <ใครเจอปัญหาอะไร ภายใต้สถานการณ์ใด และทำไมต้องแก้>

**Users / Stakeholders:** <ผู้ได้รับผลกระทบที่ยืนยันได้ ไม่เดา>

**ความถี่ / ความรุนแรง / Workaround / ผลเสียหากไม่แก้:** <Fact พร้อม Source หรือ Unknown>

## 2. Current State และ Evidence

| ข้อเท็จจริงหรือข้อสังเกต | Source / Link / วันที่ | Confirmed / Assumed / Unknown | ต้องตรวจอะไรเพิ่ม |
| --- | --- | --- | --- |
| <พฤติกรรมเดิมหรือ Pain Point> | <ที่มา> | <สถานะ> | <วิธียืนยันหรือ N/A> |

**ความเห็นที่ขัดแย้ง:** <ระบุแยก ไม่สรุปแทนผู้เกี่ยวข้อง>

## 3. Desired Outcome

- **ผลลัพธ์ที่ผู้ใช้/ธุรกิจจะสังเกตได้:** <สิ่งที่ควรทำได้หรือดีขึ้น>
- **Success Signal ที่เสนอ:** <ตัวชี้วัดหรือสิ่งที่สังเกตได้ ตัวเลขต้องมีที่มา>
- **Non-goals:** <สิ่งที่ไม่แก้ในรอบนี้>

## 4. Needs, Scope และ Constraints

- **Needs / Behaviors:** <แยก Requirement ที่ยืนยันแล้วออกจาก Hypothesis>
- **In Scope:** <ขอบเขตที่เสนอ>
- **Out of Scope:** <สิ่งที่ไม่รวม>
- **To Be Tested:** <สิ่งที่ยังต้องทดลอง>
- **Constraints:** <Privacy, Security, Data, Legal, Integration, เวลา, Accessibility หรือ N/A>

## 5. Open Questions และ Risks

| คำถาม / Risk | ถ้าเข้าใจผิดกระทบอะไร | ต้องหา Evidence อย่างไร | Owner / Checkpoint |
| --- | --- | --- | --- |
| <คำถาม> | <ผลกระทบ> | <การยืนยัน> | <Owner/วันที่> |

## 6. Human Decision และ Next Step

- **Decision:** Explore technically | Prototype / experiment | Discover more | Defer / decline
- **Human Decider / วันที่ / หลักฐานอนุมัติ:** <ข้อมูลจริง หรือ Pending>
- **Facts กับ Assumptions:** <สรุปพร้อมที่มา>
- **Next Action และ Owner:** <คำถามทางเทคนิค/Experiment/Follow-up ที่จำกัดขอบเขต>
- **ส่งต่อ Technical Discovery:** <Problem, Outcome, Critical Unknowns และ Links>

**ขอบเขต:** `Validated for exploration` **ไม่ได้อนุมัติ Design, Delivery Plan, Implementation, MR หรือ Deploy** Artifact นี้คือ Summary หนึ่งชุดทางตรรกะ ไม่ได้บังคับให้สร้างไฟล์เพิ่ม
