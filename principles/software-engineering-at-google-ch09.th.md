# Software Engineering at Google — บทที่ 09: Code Review

- **สถานะ:** Draft Study Notes v0.1 — Candidate ยังไม่ได้รับรอง
- **ขอบเขต:** อ่านจากตัวบทเผยแพร่ของ Google ครบทั้งบท (รวมบทสรุปและตัวอย่าง); สรุปใหม่ ไม่ใช่คำแปลแบบคัดลอก
- **ผู้เขียน:** Tom Manshreck and Caitlin Sadowski
- **อ้างอิงหลัก:** https://abseil.io/resources/swe-book/html/ch09.html
- **ฉบับอังกฤษหลัก:** [English](./software-engineering-at-google-ch09.md)
- **วันที่ตรวจสอบ:** 2026-10-10

**วิธีอ่าน:** เนื้อหาในส่วนสรุปอธิบายข้อเสนอและตัวอย่างของผู้เขียน ส่วน Candidate และ Mapping เป็นการสังเคราะห์ของ My Engineer ไม่ใช่กฎจาก Google โดยตรง ข้อมูลเชิงตัวเลขจากปี 2020 ไม่ใช่มาตรฐานปัจจุบัน

## สรุปและวิเคราะห์เนื้อหาทั้งบท

### 1. Review is more than bug finding

Review ตรวจ Correctness, Comprehension, Ownership และ Convention โดยหลายบทบาทอาจรวมในคนเดียว โค้ดใหม่สร้างภาระดูแลจึงควรดูว่าของเดิมใช้ซ้ำได้ไหม

**ข้อจำกัดและวิจารณญาณ:** Approval สามชั้นของ Google ไม่ควรยกมาบังคับทีมเล็ก

### 2. Different review goals require different evidence

Greenfield ต้องดู Design และ API, Behavior Change ต้องดู Test/Benchmark, Bug Fix ต้องเล็กและมี Regression, งาน Generated Change ต้องตรวจผลและวิธีสร้าง

**ข้อจำกัดและวิจารณญาณ:** อย่าถก Architecture ที่ตกลงแล้วใหม่ทุกคอมเมนต์ ให้ Escalate เมื่อมีข้อมูลสำคัญ

### 3. Respect, knowledge and ownership

Review เป็นพื้นที่แลกเปลี่ยนความรู้ ไม่ใช่ด่านอย่างเดียว คำถามช่วยเผยจุดที่ Code เข้าใจยาก ผู้เขียนควรมีสิทธิ์เลือกระหว่างทางที่ดีเท่ากัน และประวัติ Review มีค่า

**ข้อจำกัดและวิจารณญาณ:** ความไม่สุภาพ Style War และการรอคนอนุมัติอาจทำลายประโยชน์

### 4. Small, reviewable changes and meaningful descriptions

Change เล็กช่วยให้อ่าน แก้ปัญหา และ Rollback ง่าย คำอธิบายต้องบอก What/Why กรณีตัวเลข ~200 บรรทัดและตอบใน 24 ชั่วโมงเป็น Practice ของ Google

**ข้อจำกัดและวิจารณญาณ:** อย่ากำหนดจำนวนบรรทัดตายตัว งาน Generated หรือ Feature ที่ต้องมองเป็นก้อนอาจใหญ่กว่า

### 5. Automation and responsive review

Presubmit ช่วยตรวจ Format, Lint และ Test ให้คน Review Intent และ Design ลด Reviewer ที่ไม่จำเป็นแต่ยังต้องมี Owner ในงานข้าม Boundary

**ข้อจำกัดและวิจารณญาณ:** AI เป็นผู้ช่วย ไม่ใช่หลักฐานว่า Test ผ่านหรือมีคน Approve แล้ว

## Candidate Principles (ยังไม่ Approved)

### EF-G22 — Review ความถูกต้อง ความเข้าใจ และการดูแลต่อ

- **ชื่ออังกฤษ:** Review for Correctness, Comprehensibility, and Maintainability
- **ต้นฉบับที่เกี่ยวข้อง:** How Code Review Works at Google; Code Consistency; Code Comprehension
- **Layer A — ความหมาย / เมื่อไรควรใช้:** ปรับคำถาม Review ตามชนิดของ Change
- **Layer B — Trade-offs / ข้อควรระวัง:** Review แก้ Requirement/Design คลุมเครือทั้งหมดไม่ได้
- **Layer C — หลักฐานและสถานะ:** อ้างอิงเฉพาะหัวข้อข้างต้นจากบทเต็ม แต่ยังไม่ได้ตรวจหลักฐานอิสระหรือลองใช้จริง; Confidence: สูงสำหรับการระบุแนวคิดในบท ต่ำ/ยังไม่ประเมินสำหรับการยกเป็นกฎสากล; ทบทวนเมื่อมีหลักฐานใหม่หรือ Pilot ไม่สนับสนุน
- **Status:** Candidate

### EF-G23 — ทำ Change ให้ Review และย้อนกลับได้ง่าย

- **ชื่ออังกฤษ:** Make Change Sets Reviewable and Reversible
- **ต้นฉบับที่เกี่ยวข้อง:** Write Small Changes; Bug Fixes and Rollbacks; Change Descriptions
- **Layer A — ความหมาย / เมื่อไรควรใช้:** สร้าง Diff ที่เป็นเรื่องเดียวกัน บอกเหตุผลและ Test
- **Layer B — Trade-offs / ข้อควรระวัง:** บังคับจำนวนบรรทัดหรือแบ่งย่อยเกินไปทำให้ขาดภาพรวม
- **Layer C — หลักฐานและสถานะ:** อ้างอิงเฉพาะหัวข้อข้างต้นจากบทเต็ม แต่ยังไม่ได้ตรวจหลักฐานอิสระหรือลองใช้จริง; Confidence: สูงสำหรับการระบุแนวคิดในบท ต่ำ/ยังไม่ประเมินสำหรับการยกเป็นกฎสากล; ทบทวนเมื่อมีหลักฐานใหม่หรือ Pilot ไม่สนับสนุน
- **Status:** Candidate

### EF-G24 — มอง Review เป็นการแลกเปลี่ยนความรู้อย่างเคารพ

- **ชื่ออังกฤษ:** Treat Review as Respectful Knowledge Exchange
- **ต้นฉบับที่เกี่ยวข้อง:** Psychological and Cultural Benefits; Knowledge Sharing; Be Polite and Professional
- **Layer A — ความหมาย / เมื่อไรควรใช้:** ถามก่อนสรุปผิด ให้ผู้เขียนเลือกเมื่อทางเลือกดีพอกัน ตอบด้วยเหตุผล
- **Layer B — Trade-offs / ข้อควรระวัง:** รอนาน คอมเมนต์ดูถูก หรือใช้อำนาจกดทับทำลายการเรียนรู้
- **Layer C — หลักฐานและสถานะ:** อ้างอิงเฉพาะหัวข้อข้างต้นจากบทเต็ม แต่ยังไม่ได้ตรวจหลักฐานอิสระหรือลองใช้จริง; Confidence: สูงสำหรับการระบุแนวคิดในบท ต่ำ/ยังไม่ประเมินสำหรับการยกเป็นกฎสากล; ทบทวนเมื่อมีหลักฐานใหม่หรือ Pilot ไม่สนับสนุน
- **Status:** Candidate

## Mapping กลับ My Engineer (เสนอ)

Implementation & Verification: reviewer evidence and approvals; Solution Design: avoid reopening approved choices absent new evidence; Delivery Planning: review time and ownership are real capacity.

## ข้อควรระวังและขั้นถัดไป

แนวปฏิบัติและตัวเลขเป็นประสบการณ์ Google ภายใต้บริบทปี 2020 ไม่ใช่การรับรองว่าต้องใช้ในทุกองค์กร ก่อนเปลี่ยนเป็น Rule ให้ตรวจหลักฐานปัจจุบัน ข้อโต้แย้ง ผลกระทบ และทดลองกับงานที่ไม่เป็นความลับ ไม่เปลี่ยน AGENTS.md, Workflow ที่ Accepted หรือสร้าง Skill โดยอัตโนมัติ

**ต้นฉบับเต็ม:** https://abseil.io/resources/swe-book/html/ch09.html
