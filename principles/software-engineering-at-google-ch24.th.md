# Software Engineering at Google — บทที่ 24: Continuous Delivery

- **Approval:** 🟢 Accepted — สรุปบทและ Principles ได้รับรองเป็นแนวทางประกอบการตัดสินใจ ไม่ใช่กฎบังคับ (ตรวจสอบหลักฐานอิสระเพิ่มเติมได้)
- **สถานะ:** Draft Study Notes v0.1; Candidate Principles **ยังไม่ Approved**
- **ผู้เขียนบท:** Radha Narayan, Bobbi Jones, Sheri Shipe, David Owens
- **แหล่งต้นฉบับ:** https://abseil.io/resources/swe-book/html/ch24.html
- **ฉบับหลักภาษาอังกฤษ:** [English](./software-engineering-at-google-ch24.md)
- **วันที่ตรวจสอบ:** 2026-10-10
- **วิธีสรุป:** อ่านบทเผยแพร่จากต้นฉบับ Google และเรียบเรียงใหม่โดยไม่คัดลอกหนังสือ; บทวิเคราะห์และ Candidate คือข้อสังเคราะห์ My Engineer

**ข้อควรระวัง:** กรณีศึกษาองค์กร Google (ปี 2020) ไม่ใช่ข้อบังคับทั่วไป ต้องตรวจสอบการใช้งานในบริบทปัจจุบันอีกครั้ง

## วิเคราะห์ประเด็นสำคัญของบท

### 1. Code value arrives with users

Code ที่เสร็จแต่ยังไม่ถึงผู้ใช้ยังไม่ได้สร้างคุณค่าที่ตั้งใจ การ Release ครั้งละน้อยแต่ถี่ช่วยลดความเสี่ยงสะสมและหาต้นเหตุ Bug ง่ายขึ้น

**ข้อจำกัด / Trade-off:** บางระบบต้อง Release ไม่ถี่จากข้อกำกับหรือ App Store

### 2. Readiness before frequency

บทนี้เน้น Agility, Automation, Isolation, Reliability, Data-driven Decisions และ Phased Rollout ทีมอาจทำระบบให้ Deploy เมื่อไรก็ได้ โดยไม่ต้องปล่อยทุก Commit ทันที

**ข้อจำกัด / Trade-off:** Release ถี่ไม่ปลอดภัยเองถ้าไม่มี CI/Test/Rollback/Monitoring

### 3. Keep release trains predictable

Release Batch ใหญ่และ Cherry-pick ตอนท้ายทำให้ทีม Release เป็นคอขวด Release Train ถี่ที่มี Cutoff ชัดทำให้งานตกขบวนไม่เจ็บมาก

**ข้อจำกัด / Trade-off:** Deadline ไม่ควรอยู่เหนือความถูกต้องที่กระทบผู้ใช้สำคัญ

### 4. Progressive exposure with guardrails

ใช้ Feature Flags แยกการ Deploy จากเปิดให้ผู้ใช้เห็น ค่อยเพิ่ม Traffic และดู Health Metrics ใช้ Representative Testing แทนไล่ทุก Device แต่ต้องระวังปัญหาที่กระทบผู้ใช้กลุ่มเล็กหรือ Accessibility

**ข้อจำกัด / Trade-off:** A/B ต้องมีตัวอย่างพอ Flag มีต้นทุนดูแล และ Test บน Production ไม่อาจลบผลกระทบทั้งหมด

### 5. Release only worthwhile functionality

Feature มีต้นทุนต่อผู้ใช้ทั้งพื้นที่ Bandwidth และความซับซ้อน ต้องดู Value เทียบ Cost ผ่าน Metrics และผู้รับผิดชอบตัดสิน Go/No-go

**ข้อจำกัด / Trade-off:** ยอดใช้งานไม่ใช่ตัววัดคุณค่าด้าน Accessibility หรือสังคมทั้งหมด

## Candidate Principles — ยังไม่ใช่กฎกลาง

### EF-G37 — สร้างความสามารถ Release อย่างปลอดภัยเมื่อจำเป็น

- **ชื่ออังกฤษ:** Build the Capability to Release Safely on Demand
- **ที่มาในบท:** Idioms of Continuous Delivery; Conclusion
- **Layer A — กลไก/เมื่อควรใช้:** ลงทุนกับ CI, Deploy ซ้ำได้, Rollback และ Monitoring
- **Layer B — ข้อแลกเปลี่ยนและ Failure Mode:** ไม่บังคับความถี่ Release โดยไม่ดูบริบทผู้ใช้
- **Layer C — Evidence / Evolution:** รองรับว่าเป็นแนวคิดในบทจากหัวข้อข้างต้น แต่ยังไม่มีการตรวจสอบแหล่งอิสระและ Pilot ใน My Engineer; ทบทวนเมื่อมีหลักฐาน/ข้อจำกัดใหม่
- **Status:** Candidate

### EF-G38 — ปล่อย Change เล็กพร้อมค่อย ๆ เปิดใช้งาน

- **ชื่ออังกฤษ:** Release Small, Isolated Changes with Progressive Exposure
- **ที่มาในบท:** Velocity Is a Team Sport; Shifting Left; Staged Rollout
- **Layer A — กลไก/เมื่อควรใช้:** ใช้ Change เล็ก, Feature Flags และ Canary ตามความเสี่ยง
- **Layer B — ข้อแลกเปลี่ยนและ Failure Mode:** Flag เยอะเกินไปและ Auto Promote ผิดอาจยิ่งเสี่ยง
- **Layer C — Evidence / Evolution:** รองรับว่าเป็นแนวคิดในบทจากหัวข้อข้างต้น แต่ยังไม่มีการตรวจสอบแหล่งอิสระและ Pilot ใน My Engineer; ทบทวนเมื่อมีหลักฐาน/ข้อจำกัดใหม่
- **Status:** Candidate

### EF-G39 — ตัดสิน Release จากผลต่อผู้ใช้และสุขภาพระบบ

- **ชื่ออังกฤษ:** Use User Impact and Health Guardrails for Release Decisions
- **ที่มาในบท:** Quality and User-Focus; Meet Your Release Deadline; Ship Only What Gets Used
- **Layer A — กลไก/เมื่อควรใช้:** กำหนด Health Indicators และผู้ตัดสินใจ Go/No-go
- **Layer B — ข้อแลกเปลี่ยนและ Failure Mode:** ตัวเลขเดียวอาจซ่อนปัญหาคนกลุ่มเล็กหรือ Accessibility
- **Layer C — Evidence / Evolution:** รองรับว่าเป็นแนวคิดในบทจากหัวข้อข้างต้น แต่ยังไม่มีการตรวจสอบแหล่งอิสระและ Pilot ใน My Engineer; ทบทวนเมื่อมีหลักฐาน/ข้อจำกัดใหม่
- **Status:** Candidate

## เชื่อมกับ Workflow ของเรา (ยังเป็นข้อเสนอ)

FDL: outcome feedback loop; Solution Design: feature isolation; Delivery Planning: small release increments; Release & Operations: deployment, rollback and progressive rollout.

## สิ่งที่ต้องตรวจสอบก่อน Approve

1. เนื้อหาจาก Google สอดคล้องกับบริบท Solo/Team/Multi-repo ของเราแค่ไหน?
2. มีข้อโต้แย้งจากงานวิจัย/แนวปฏิบัติล่าสุดหรือไม่?
3. หลักการซ้ำกับ EF-G01–24 หรือไม่ และควรรวมกันแทนเพิ่มกฎใหม่ไหม?
4. มีงานตัวอย่างที่ไม่เป็นความลับให้ทดลองและวัดผลได้หรือไม่?

**ยังไม่เปลี่ยน Accepted Workflows, Delivery Planning Draft, AGENTS.md หรือสร้าง Skills**
