# Codex Model Routing — My Engineer Pilot (TH)

**สถานะ:** Pilot hypothesis ไม่ใช่การรับประกันราคาหรือคุณภาพ; ให้เลือกโมเดลจาก `/model` ในบัญชีจริง และบันทึกผลไว้ใน `pilot-projects/docs`.

## หลักการ
- **เลือกโมเดลที่เบาที่สุดที่ผ่าน Acceptance Criteria และ Safety Gate ได้**; ห้ามลดระดับเพียงเพราะ Task มีจำนวนบรรทัดน้อยแต่มีผลกระทบสูง
- ใช้ Codex เป็นตัวเดียวสำหรับ Pilot #001; Qwen/Ollama เป็นการทดลองภายหลัง
- เปลี่ยน Model/Effort โดยผู้ใช้เลือก Profile ก่อนแต่ละ Stage; `AGENTS.md` ไม่สามารถสลับ Model ให้โดยอัตโนมัติ
- Independent Review ต้องอยู่ **คนละ Session** กับ Implementer และตรวจ Diff, Tests, Spec จริง ไม่พึ่งสรุปจาก Session เดิม
- โมเดลทุกตัวอาจผิดพลาด; Unit/Integration Tests และ Human Approval ไม่สามารถแทนกันได้

## Initial routing

| ประเภทงาน | Model (ตามตัวเลือก Codex CLI ที่ตรวจพบ 2026-10-11) | Effort เริ่มต้น |
| --- | --- | --- |
| Formatting, commit message, simple docs/log | `gpt-6-luna` | low |
| Small, low-risk, isolated code change | `gpt-6-luna` | low (เพิ่มถ้าจำเป็น) |
| Requirements, task planning, typical implementation, tests | `gpt-6.1-sol` | medium |
| Independent code review | `gpt-6.1-sol` | high |
| Critical architecture, high-risk security, difficult debugging | `gpt-6-astra` | high; human decides |

ชื่อโมเดลและ Effort อาจเปลี่ยนหรือไม่รองรับในบางเวอร์ชัน; ยืนยันใน `/model` ก่อนใช้งานจริง

## Escalation & budget
1. กำหนด Acceptance Criteria / Risk / Context ของ Task ก่อนเลือก Profile
2. ถ้างาน Low-risk ลอง Luna; ถ้าผิด Acceptance, ติดขัด, หรือวนซ้ำโดยไม่มีหลักฐานความคืบหน้า 1–2 รอบ ให้หยุดและ Escalate เป็น Sol
3. ถ้า Sol พบ Risk สูงหรือปัญหา Architectural ให้ขอ Human Approval ก่อนใช้ Astra
4. จำกัดการอ่าน Repo เฉพาะไฟล์ที่จำเป็น ไม่ Dump ทุกไฟล์ ไม่เรียก Review ซ้ำโดยไม่มี Diff ใหม่
5. การใช้ ChatGPT Plus อยู่ภายใต้ Usage Limits; **อย่าคิดราคา API/token เป็นบิล Codex โดยอัตโนมัติ** วัดสถานะจาก `/status` ถ้าระบบแสดงและบันทึก Usage เท่าที่ตรวจได้

## Metrics & Evidence
ทุก Stage บันทึก: วันที่, Stage, Task/Plane ID, Model, Effort, elapsed time (ถ้าวัดได้), Retry, Tests, Human Intervention, Review Findings, Usage Snapshot (ถ้ามี), หลักฐาน Git/Plane/MR, Result: Work / Partial / Failed / Not Verified.
เก็บที่ `pilot-projects/docs` พร้อมปัญหาและ Solution ทั้งที่ Work/Not Work; ห้ามแต่งข้อมูลที่ไม่ได้วัด.

## Related
- [Canonical Pilot Playbook](../playbooks/idea-to-outcome.th.md)
- [Codex Setup in puen-stack](https://github.com/puennaja/puen-stack/blob/master/setup/codex-model-routing.th.md)
