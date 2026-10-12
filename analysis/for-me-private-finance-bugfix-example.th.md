# Example: Private Finance — Bug Fix ยอดสรุปรายจ่ายผิด

> ตัวอย่างสมมติใน Product `private-finance` (FE → BFF → BE → Database) ไม่ใช่ข้อเท็จจริงจาก Codebase จริง ใช้เฉพาะ Agent/เครื่องมือที่บริษัทอนุญาต

**วิธีใช้:** พิมพ์ทีละ Prompt ใน Repo ที่เกี่ยวข้อง ตรวจ Output ก่อนขั้นถัดไป และเก็บข้อมูลบริษัทไว้เฉพาะระบบที่ได้รับอนุญาต อย่า Commit/Push/Merge/Deploy อัตโนมัติ

**โจทย์:** ผู้ใช้เลือกเดือนตุลาคม แต่ยอดสรุปบน Dashboard ไม่ตรงกับ Transaction List โดยเฉพาะรายการ refund (เป็นสมมติฐาน ต้องพิสูจน์ ไม่ใช่สาเหตุที่รู้แล้ว)

### 1) เข้าใจ Flow — pstack `/how`
```text
/how ช่วยไล่ว่ายอดสรุปรายจ่ายเดือนนี้มาจากไหน ตั้งแต่หน้าจอไปจนถึง query
ขอเฉพาะ flow ใน repo นี้ ยังไม่ต้องแก้
```
**คาดหวัง:** จุดคำนวณ/กรองวันที่, caller, contract, test และสิ่งที่ยังไม่รู้; เปิดตรวจ FE/BFF/BE แยก Repo

### 2) Reproduce ก่อนเดาสาเหตุ — Matt `/diagnosing-bugs`
```text
/diagnosing-bugs หน้า Dashboard รวมยอดเดือนตุลาคมไม่ตรงกับรายการ
ช่วยทำให้ reproduce ได้ก่อน แล้วค่อยหาสาเหตุ อย่าเพิ่งแก้จากที่เดา
```
**คาดหวัง:** Input fixture, actual/expected, คำสั่งที่รันซ้ำแล้ว fail ได้ และสมมติฐานที่จัดลำดับ; ถ้า reproduce ไม่ได้ให้รายงาน Blocked

### 3) เช็กผลกระทบ — pstack `/blast-radius`
```text
/blast-radius ถ้าต้องแก้ logic รวมยอด จะมีหน้าจอหรือ API ไหนใช้ร่วมกันบ้าง
ขอผลจากโค้ดและ test จริง
```
**คาดหวัง:** Consumers/risks ที่พิสูจน์ได้

### 4) Regression Test และ Fix — Matt `/tdd` + Coding Agent
```text
/tdd เอาเคสที่ reproduce ได้มาเขียน regression test ก่อน
แล้วแก้เฉพาะสาเหตุจริงให้ผ่าน test โดยไม่เปลี่ยน behavior อื่น
```
**คาดหวัง:** Red → Green พร้อมผล test และ diff; ตรวจ timezone, refund, currency ตาม business rule ที่ยืนยันแล้ว ไม่สร้างกฎเอง

### 5) Verify + Review — Tests และ Matt `/code-review`
```text
/code-review <base-ref> ช่วยดูว่า fix นี้ตรง bug ที่รายงานไหม
และมี regression หรือผิด convention อะไรหรือเปล่า
```
**คาดหวัง:** Standards/Spec findings แยกกัน พร้อมผล runtime/API test จริง; `code-review` อาจไม่อ่าน uncommitted diff จึงต้องตรวจ scope ของ diff

### 6) เตรียม MR — Matt `/pr`
```text
/pr ช่วยเขียน Draft MR สั้นๆ ว่าบั๊กคืออะไร แก้ยังไง
มีหลักฐาน test อะไร และเสี่ยงกระทบตรงไหน ไม่ต้องเปิด MR
```
**คาดหวัง:** MR description อย่างเดียว. การ Commit/Push/MR ให้ทำตามสิทธิ์และ Review ของทีม

**จำง่าย:** `how → diagnosing-bugs → blast-radius → tdd/fix → code-review → pr` (งานเล็กตัดขั้นที่ไม่จำเป็นได้)
