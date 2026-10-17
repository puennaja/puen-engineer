# Structural Design Patterns — สรุปภาษาไทย

อ้างอิง: https://refactoring.guru/design-patterns/structural-patterns | สรุปและตัวอย่างเรียบเรียงใหม่สำหรับ My Engineer (2026-10-17)

เอกสารนี้เป็นโน้ตศึกษา ไม่ใช่งานแปลต้นฉบับหรือกฎบังคับใช้ pattern; ตัวอย่าง Backend/BFF เป็นกรณีประยุกต์ที่จัดทำขึ้นใหม่

## ภาพรวม

มุ่งจัดโครงสร้างและความสัมพันธ์ระหว่าง objects/components เพื่อให้ต่อยอดโดยไม่ต้องแก้โค้ดทุกจุด

## 1. Adapter

**ปัญหาที่แก้:** dependency เดิมให้ interface ไม่ตรงที่ caller ต้องใช้

**หลักการและวิธีทำงาน:** สร้าง Target interface ของฝั่งเรา; Adapter implement Target และแปลง input/output/error ก่อนส่งต่อ Adaptee

**ตัวอย่าง BE/BFF:** MerchantApiAdapter แปลง external status codes ไป domain status สำหรับ BFF

**เมื่อไรควรใช้ / ทางเลือก:** เหมาะกับ legacy หรือ third-party boundary และ anti-corruption layer

**ข้อเสียและจุดทดสอบ:** mapping ผิดอาจสูญเสียข้อมูล; ต้องจัดการ backward compatibility และ error semantics

**อ่านต้นฉบับ:** https://refactoring.guru/design-patterns/adapter

## 2. Bridge

**ปัญหาที่แก้:** มีสองแกนที่เปลี่ยนอิสระ ทำให้ใช้ inheritance แล้ว combinations โตแบบคูณ

**หลักการและวิธีทำงาน:** Abstraction ถือ reference ไป Implementor interface; extended abstractions และ concrete implementors วิวัฒน์แยกกัน

**ตัวอย่าง BE/BFF:** Alert type เช่น BillingAlert และ SecurityAlert ใช้ Delivery channel เช่น Email และ Push

**เมื่อไรควรใช้ / ทางเลือก:** เมื่อมี variation สองแกนที่เป็นจริงและต้องจับคู่หลายแบบ

**ข้อเสียและจุดทดสอบ:** ถ้ามีแกนเดียวการเพิ่ม interface สองชั้นจะซับซ้อนโดยไม่คุ้ม

**อ่านต้นฉบับ:** https://refactoring.guru/design-patterns/bridge

## 3. Composite

**ปัญหาที่แก้:** ต้องจัดการ object เดี่ยวและกลุ่มที่ซ้อนเป็นต้นไม้ด้วย operations เดียวกัน

**หลักการและวิธีทำงาน:** Component interface เป็น contract; Leaf ทำงานจริง; Composite เก็บ children แล้วส่งต่อ/aggregate operations recursively

**ตัวอย่าง BE/BFF:** role permission tree: PermissionLeaf กับ PermissionGroup ประเมินสิทธิ์ด้วย evaluate()

**เมื่อไรควรใช้ / ทางเลือก:** เหมาะเมื่อ recursive structure เป็นส่วนของ domain

**ข้อเสียและจุดทดสอบ:** ไม่ใช่ทุก operation ที่เหมาะกับ leaf และ group; ตรวจ cycle และ recursion depth

**อ่านต้นฉบับ:** https://refactoring.guru/design-patterns/composite

## 4. Decorator

**ปัญหาที่แก้:** อยากเพิ่มความสามารถให้ object โดยไม่แก้ core และไม่สร้าง subclass ตามทุก combination

**หลักการและวิธีทำงาน:** Decorator implement interface เดียวกับ wrapped component, delegate พร้อม behavior ก่อนหรือหลัง call; ซ้อน wrapper ได้

**ตัวอย่าง BE/BFF:** Service → tracing decorator → metrics decorator → cache decorator

**เมื่อไรควรใช้ / ทางเลือก:** เหมาะกับ cross-cutting concerns ที่ต้องเลือกและ compose ได้

**ข้อเสียและจุดทดสอบ:** ลำดับมีผลต่อ cache hit/metrics/error; tracing context และการ debug chain ต้องชัด

**อ่านต้นฉบับ:** https://refactoring.guru/design-patterns/decorator

## 5. Facade

**ปัญหาที่แก้:** caller ต้องรู้จักหลาย subsystem และประสานขั้นตอนซ้ำ

**หลักการและวิธีทำงาน:** Facade มี API ระดับ use case และรับผิดชอบ orchestrate subsystem ที่เกี่ยวข้อง โดย subsystem ยังเข้าถึงได้ตามจำเป็น

**ตัวอย่าง BE/BFF:** AdminOperationFacade รวมโหลดรายละเอียด merchant และ operation history สำหรับหน้า detail

**เมื่อไรควรใช้ / ทางเลือก:** เหมาะกับ API boundary ที่ต้องลดความซับซ้อนให้ clients

**ข้อเสียและจุดทดสอบ:** ระวัง god object, timeout budget และ partial failure โดยเฉพาะ cross-service

**อ่านต้นฉบับ:** https://refactoring.guru/design-patterns/facade

## 6. Flyweight

**ปัญหาที่แก้:** มี object จำนวนมากที่เก็บข้อมูลซ้ำจนเปลือง memory

**หลักการและวิธีทำงาน:** แยก intrinsic immutable shared state จาก extrinsic per-object state; factory/cache คืน shared flyweight; caller ส่ง extrinsic state เมื่อใช้

**ตัวอย่าง BE/BFF:** rule engine แชร์ compiled static rule definitions ระหว่าง evaluations

**เมื่อไรควรใช้ / ทางเลือก:** ใช้หลังวัด profile ว่า memory เป็น bottleneck จริง

**ข้อเสียและจุดทดสอบ:** cache lifecycle, concurrency และ accidental mutation ซับซ้อน; อย่า optimize ก่อนวัด

**อ่านต้นฉบับ:** https://refactoring.guru/design-patterns/flyweight

## 7. Proxy

**ปัญหาที่แก้:** ต้องควบคุมการเข้าถึง object จริงโดยคง interface เดิม

**หลักการและวิธีทำงาน:** Proxy implement interface เดียวกันและถือ reference ถึง RealSubject; อาจ lazy-init, check access, cache หรือส่งต่อ remote call

**ตัวอย่าง BE/BFF:** Remote merchant client proxy ใส่ authentication และ timeout; การ retry ต้องพิจารณา idempotency

**เมื่อไรควรใช้ / ทางเลือก:** เหมาะกับ access control, virtualization, lazy initialization

**ข้อเสียและจุดทดสอบ:** side effects แอบแฝง เช่น network, retry, cache stale; latency และ errors ต้องมองเห็น

**อ่านต้นฉบับ:** https://refactoring.guru/design-patterns/proxy

## แนวทางนำไปใช้ใน My Engineer

ก่อนเลือก pattern ให้ระบุ requirement, change pressure, ทางเลือกที่เรียบง่ายกว่า, ค่าใช้จ่ายด้าน maintenance และ test plan; ใช้ pattern เท่าที่ evidence รองรับเท่านั้น บันทึก ADR หากการตัดสินใจมีนัยสำคัญ และบันทึกผล pilot/ปัญหาจริงใน pilot-projects/docs แยกจาก reusable knowledge นี้
