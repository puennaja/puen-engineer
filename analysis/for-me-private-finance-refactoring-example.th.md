# Example: Private Finance — Refactor Logic รายจ่ายที่ซ้ำกัน

> ตัวอย่างสมมติใน Product `private-finance` (FE → BFF → BE → Database) ไม่ใช่ข้อเท็จจริงจาก Codebase จริง ใช้เฉพาะ Agent/เครื่องมือที่บริษัทอนุญาต

**วิธีใช้:** พิมพ์ทีละ Prompt ใน Repo ที่เกี่ยวข้อง ตรวจ Output ก่อนขั้นถัดไป และเก็บข้อมูลบริษัทไว้เฉพาะระบบที่ได้รับอนุญาต อย่า Commit/Push/Merge/Deploy อัตโนมัติ

**โจทย์:** สมมติว่า code สำหรับจัดรูปข้อมูล Transaction ใน BFF มี logic ซ้ำหลายจุด อยากลดความซับซ้อน **โดยไม่เปลี่ยน API behavior**; ต้องสำรวจว่าซ้ำจริงก่อน

### 1) เข้าใจของเดิม — pstack `/how`
```text
/how ช่วยอธิบาย flow แปลง transaction response ใน BFF นี้
รวมถึง callers และ tests ที่เกี่ยวข้อง ยังไม่แก้โค้ด
```
**คาดหวัง:** Map ของ module, callers และ contracts ที่มีอยู่จริง

### 2) ตรวจ Risk — pstack `/blast-radius`
```text
/blast-radius ถ้ารวม logic แปลง transaction จะกระทบ API หรือ feature ไหนบ้าง
และควร pin behavior ตรงไหนก่อนแก้
```
**คาดหวัง:** สัญญาที่ห้ามเปลี่ยน และ regression tests ที่ต้องมี

### 3) ออกแบบโดยไม่เพิ่ม Abstraction เกินจำเป็น — Matt `/codebase-design`
```text
/codebase-design ช่วยดูว่าควรย้าย logic ซ้ำไปอยู่ตรงไหน
เอาแบบ interface เรียบง่าย ไม่สร้าง layer เพิ่มถ้าไม่จำเป็น
```
**คาดหวัง:** ขอบเขต module/seam และ trade-offs; เป็น design reference ไม่ใช่ runner

### 4) ล็อก Behavior แล้วแก้ — Coding Agent + Matt `/tdd` ตามความเหมาะสม
```text
ช่วยเก็บ baseline test/output ของ transaction response ก่อน
แล้ว refactor ตาม design ที่ตกลงไว้ ห้ามเปลี่ยน payload, error และ ordering
รัน test ให้ดูด้วย ยังไม่ต้อง commit
```
**คาดหวัง:** Before/after ที่เท่ากัน, minimal diff; หากไม่มี tests ให้สร้าง characterization tests ที่ยืนยันพฤติกรรมเดิม ไม่เดาผลลัพธ์

### 5) ตรวจว่าไม่เกิด Regression — Matt `/code-review`
```text
/code-review <base-ref> รีวิว refactor นี้ว่า behavior ยังเหมือนเดิมไหม
มี abstraction ที่ไม่จำเป็น หรือขัด standard อะไรหรือเปล่า
```
**คาดหวัง:** Evidence จาก test + review (ตรวจ committed diff scope ก่อน) และ manual inspection กรณีจำเป็น

### 6) เตรียม MR — Matt `/pr`
```text
/pr เขียน Draft MR ของ refactor นี้ เน้นว่า behavior เดิมไม่เปลี่ยน
พร้อม test evidence และความเสี่ยง ไม่ต้องเปิด MR
```

**จำง่าย:** `how → blast-radius → codebase-design → baseline/refactor → code-review → pr`. หากยังไม่รู้ว่าควร refactor อะไร ค่อยใช้ Matt `/improve-codebase-architecture` สำรวจก่อนเป็นงานแยก
