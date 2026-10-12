# วิเคราะห์ pstack สำหรับ My Engineer — คู่มือใช้งานชั่วคราว (ภาษาไทย)

> วันที่วิเคราะห์: 2026-10-12 · สถานะ: Analysis / Interim Adoption · ไม่ใช่การรับรองว่าทุก Skill ทำงานกับ Codex/Claude ได้เท่ากัน

## 1. วัตถุประสงค์

อ่าน [Cursor pstack](https://github.com/cursor/plugins/tree/main/pstack) โดยยึด [Guide 10 บท](https://github.com/cursor/plugins/tree/main/pstack/docs/guide), [poteto-mode](https://github.com/cursor/plugins/blob/main/pstack/skills/poteto-mode/SKILL.md) และ Playbooks เพื่อระบุว่า Skill ไหนใช้แทนหรือช่วยให้เกิดผลลัพธ์ของ [My Engineer Workflows](../../workflows/feature-delivery-lifecycle.th.md) ระหว่างที่ Skills ใน puen-stack ยังไม่เสร็จ

**คำตัดสิน:** ใช้ pstack เป็น engineering execution/verification layer ได้ แต่ **ห้ามนำ pstack มาแทน lifecycle coordinator, tracker, human gates หรือ permissions**. pstack เลือก playbook และทำโค้ดเก่ง แต่ไม่ใช่ระบบ requirement/product governance ที่มีหลักฐานอนุมัติครบตาม workflow ของเรา

## 2. วิธีอ่าน pstack ให้ถูก

- เริ่มจาก `/setup-pstack` เพื่อกำหนดโมเดลตาม role; ค่าเริ่มต้น/คำสั่งเฉพาะ Cursor เปลี่ยนได้ตามเวอร์ชัน
- `/poteto-mode` เป็น entry point: เลือกหนึ่งใน 23 playbooks จากประเภทงาน, ตั้ง todo, ส่งต่อไปยัง skills เฉพาะทาง
- `/how` อธิบาย behavior จากโค้ด; `/why` ค้นเหตุผลจาก git/issue/docs/MCP; `/teach` ทำให้มนุษย์เข้าใจ; `/recall` คืน context
- `/architect` จัดการ data shape, contracts และ boundaries; `/arena` ให้หลาย candidate แข่งในโจทย์เดียว; `/swarm` แบ่ง independent slices; ทั้งสองอย่างไม่เหมือนกัน
- `/interrogate` คือ adversarial multi-model review ของ diff ไม่ใช่การรับรอง acceptance หรือเป็น independent verification ทั้งหมด
- `/create-verification-skill` สร้าง harness สำหรับรันแอปจริง (ต้นฉบับเขียนลง .cursor/skills); ใช้ได้ต่อเมื่อ generator สาธิต run จริงสำเร็จ
- `/show-me-your-work` decision trail; `/reflect` บทเรียนสู่ skill; `/correct` แก้ repeated agent mistakes โดยตั้งใจ encode เป็น lint/types/tests ก่อน docs

## 3. Mapping ระหว่าง My Engineer 7 Workflows กับ pstack

| Workflow | เป้าหมาย/Output ของเรา | Skills / Playbook ของ pstack | ความใกล้เคียง / Gap |
|---|---|---|---|
| Requirement Discovery | Problem, user outcome, scope, acceptance, confirmed/assumed/unknown, tracker record | Investigation, `/why`, `/teach`, Prototype | **บางส่วน**: ช่วยสืบและทดลอง แต่ไม่สร้าง/อนุมัติ Product Requirement หรือ Plane work item ตาม policy |
| Technical Discovery | current flow, affected repos, dependencies, risk, references | `/how`, `/why`, `/blast-radius`, Investigation | **สูง**: ระบุ trace/หลักฐาน; cross-repo ต้องเปิด context/repo ให้ถูกและรวบรวมเอง |
| Solution Design | options, data/API contracts, edge cases, rationale, approved design | `/architect`, `/arena`, Prototype, `/technical-writing` | **สูงเชิงเทคนิค**: แต่ `/architect` default ไป implement ต่อ ต้องสั่ง **with checkpoint / stop before editing** เพื่อคง Human Design Gate |
| Delivery Planning | whole-feature plan, dependencies, sequenced work items, approval | Multi-phase plan, `principle-sequence-verifiable-units`, `/show-me-your-work` | **ปานกลาง**: pstack ไม่มี dedicated planning skill; multi-phase plan เน้น PR chunks ไม่เท่ากับ Plane items/human approval |
| Implementation | bounded diff, checks, no extra scope, actual evidence | Feature/Bug fix/Refactoring/Perf playbooks, `/tdd`, `/architect`, `/unslop` | **สูง**: Feature playbook มัก delegate และเปิด PR เอง ต้อง **หยุดก่อน commit/push/open PR** จนได้รับอนุญาต |
| Verification | behavior evidence, negative tests, independent diff review, risks, human verdict | `/create-verification-skill`, `/blast-radius`, `/interrogate`, `/benchmark-checklist`, `principle-prove-it-works` | **สูงด้านการพิสูจน์** แต่ต้องแยก developer checks, independent reviewer, human verification gate; Not run != Pass |
| Release Operations | scoped commit, authorized draft PR/MR, CI, release go/no-go, observe outcome | Opening a PR, Babysit, Shipping, `/reflect` | **บางส่วน**: pstack เน้น GitHub/Origin + merge; เราใช้ GitLab ได้ด้วยและ **ห้าม auto ready/merge/deploy** |

**Outcome & Learning** เป็นส่วนต่อท้าย cross-workflow ของ Pilot: `/show-me-your-work` + `/reflect` + `/correct` ช่วยได้ แต่ต้องบันทึก actual outcome, experiment, evidence และ worked/failed/not-verified ลง `pilot-projects/docs` ตามนโยบายของเรา

## 4. เส้นทางลงมือทำจริง (Interim Happy Path)

**เริ่มจาก Idea / Requirement ใหม่:**

1. **Bootstrap:** เปิด repo ที่ถูก, ตรวจ `AGENTS.md`, tree/branch และ existing changes; setup pstack ใน Cursor เฉพาะที่ใช้ได้. สำหรับ Codex-first ต้องยืนยัน compatibility ของ agent, subagent, model routing และ skill paths ก่อน; ห้ามถือว่า slash commands ของ Cursor ทำงานใน Codex โดยตรง
2. **Requirement Gate:** ใช้ `work-item-runner` ของเรา/Plane หรือกระบวนการ manual ที่ได้รับอนุญาต; ยืนยัน problem, acceptance, non-goals, risk และ work item link. pstack Investigation/Prototype ใช้ช่วยหาคำตอบได้ **แต่ห้ามแทนการอนุมัติ**
3. **Technical Discovery:** `/how` สำรวจ execution/data flow; `/why` สืบความเป็นมาถ้ามีคำถาม; `/blast-radius` ตรวจ callers/impacts. Output: file paths, actual contract, assumptions และสิ่งยังไม่ยืนยัน
4. **Solution Design:** `/architect ... with checkpoint; stop before implementing`; `/arena` เมื่อ design ambiguity สูง/ย้อนกลับยาก; Prototype ถ้าคำตอบวัดได้. Output: 2–3 options เมื่อจำเป็น, chosen data shape, contracts, edge cases, verification approach. **Human Solution Design Gate**
5. **Delivery Planning:** `/poteto-mode plan ... small verifiable work items; do not implement` ผ่าน Multi-phase plan. แตกทั้ง Feature เป็น workstreams/dependencies; ลงรายละเอียดแค่ next work item; สร้าง child Plane items หลัง **Human Planning Gate**
6. **Acceptance-first:** `verify-work-item` ของเราเตรียม expected outcomes; ใช้ `/tdd` สำหรับ bug ที่มี cheap repeatable tests. แยก behavior assertions จาก implementation details
7. **Implementation:** `/poteto-mode feature ...` หรือ Bug fix/Refactoring; ระบุ **authorized work item only**; ห้าม push, PR, merge, deploy และอย่าข้าม Human Gates แม้ Feature playbook ของ pstack จะมี opening PR เป็นขั้นท้าย. Run unit/lint/build และ attach exact results. **Human Implementation Gate**
8. **Independent Verification:** หลัง checks ผ่านและ gate อนุญาต ให้ independent reviewer ใช้ `/interrogate` + `/blast-radius`; ใช้ `/create-verification-skill` เฉพาะเมื่อมี UI/API harness ให้ verify app behavior จริง. สรุป Pass/Fail/Blocked/Not run/Inconclusive และ evidence. **Human Verification Gate**
9. **Local Commit & Publish:** ใช้ `git-commit` ของเราตาม `[WORK-123] feat: ...` ด้วย ID จริง (commit != push). ขออนุญาต Push แยก และอนุญาต Draft PR/MR เป็นรายครั้ง. GitHub ใช้ PR; GitLab ใช้ MR. ตั้ง target branch จาก repo จริง ไม่เดาว่า master. **ไม่เรียก Shipping แบบ auto-merge**
10. **Release/Observe:** Human go/no-go ก่อน prod, smoke/monitoring/rollback, outcome evidence. pstack Babysit อาจใช้ดู PR/CI ได้แบบ status-only ภายใต้สิทธิ์; merge/deploy เป็นของ human
11. **Learn:** `/show-me-your-work` เก็บ decision trail, `/reflect` สกัดบทเรียน, `/correct` เมื่อผิดซ้ำ; บันทึกใน pilot docs และทำ follow-up โดยอ้างหลักฐาน

### Prompt เริ่มต้น (ปรับชื่อ repo / Work Item)

```text
/poteto-mode Investigate this approved work item in READ-ONLY mode.
Work item: <ID/link>
Requirement and acceptance: <text>
Start with /how for the current flow; use /why for design history and /blast-radius for impacted callers.
Return files/contracts, confirmed evidence, unknowns, risks, and recommended next decision.
Do not edit, commit, push, open PR/MR, merge, or deploy. Stop for our My Engineer Human Gate.
```

```text
/architect Design <approved scope> with checkpoint.
Show caller usage first, named data shapes, API contracts, edge cases, alternatives and test strategy.
STOP before implementation and wait for Human Solution Design Gate.
```

```text
/poteto-mode Implement ONLY approved work item <ID> and follow acceptance <...>.
Run local developer checks and demonstrate changed behavior with reproducible evidence.
STOP before commit, push, PR/MR, merge, or deployment. Report diff, checks, failures and open risks.
```

```text
/interrogate Independently review the fixed diff against work item <ID> and acceptance <...>.
Focus on correctness, regressions, boundaries, test quality, edge cases and evidence.
Do not change the code. Classify actionable findings with path/line and severity.
```

## 5. เลือก Skills แบบประหยัด Token/Cost

| Task | Default | ใช้เพิ่มเฉพาะเมื่อ |
|---|---|---|
| คำถาม Codebase ธรรมดา | `/how` | `/why` ถาม rationale/history |
| Bug เล็ก | Bug fix playbook + repro/test | `/blast-radius` เมื่อเสี่ยงกระทบ consumers |
| Feature ทั่วไป | `/how` → `/architect` (เมื่อข้าม boundary) → Feature → verify | `/arena` เมื่อทางเลือกแพง/ย้อนยาก |
| Design ยาก | `/architect`, prototype | `/interrogate` บน diff จริง ไม่ใช่ speculative plan |
| Multi-repo impact | Read-only investigation แยก repo, consolidate contract | `/swarm` เฉพาะ independent investigation slices และ agent supports |
| Quality review | `/interrogate` (ถ้ารองรับ model panel) | Independent behavior verification เพิ่มเอง |
| Performance | Perf playbook + `/benchmark-checklist` | Hillclimb เมื่อมี metric/target/measuring harness |
| Learning | `/show-me-your-work` → `/reflect` | `/correct` เมื่อพบ pattern ผิดซ้ำ |

**อย่าเปิด `/arena`/`/swarm` ทุก task**: เพิ่มค่าใช้จ่ายและอาจเพิ่ม conflicts. เลือก minimum process ที่ยังพิสูจน์คุณภาพได้ และ log model, tokens/cost (ถ้าเข้าถึงได้จริง), time, evidence, result.

## 6. ตัวอย่าง 3 Repos: GP Request Operation Log

- **Admin Web:** `/how` trace detail page and render components; design UI data contract; verify display ordering, pagination/empty/error
- **Admin BFF:** `/how` existing handler / mapper, `/blast-radius` affected consumers; `/architect` response DTO/API contract
- **Merchant Service:** `/how` status transition and log persistence; `/why` existing behavior/history; verify log ordering, actor identity and concurrent transitions
- รวม cross-repo contract และ dependencies ใน Solution Design artifact; อย่าคิดว่าการเปิด VS Code ทีละ repo ทำให้ agent มองเห็นอีกสอง repo อัตโนมัติ
- Implement ตาม dependency sequence; independent verify แต่ละ repo และ integration ด้วย evidence; อย่าเรียก `/swarm` ให้หลาย agent แก้ schema เดียวกันโดยไม่มี owner

## 7. ช่องว่าง/ข้อจำกัดที่ต้องระวัง

1. **Cursor-specific:** plugin install `/add-plugin pstack`, `/setup-pstack`, subagents, `.cursor/skills`, `/loop` และ model routing อาจไม่รองรับใน Codex/Claude ตรง ๆ; ห้ามแปลว่า npx install อะไรก็เทียบเท่ากันโดยไม่มีผลทดสอบ
2. **Missing bundled dependencies:** README ระบุ `deslop`, `control-cli`, `control-ui` จาก `cursor-team-kit`; การทำ UI verification อาจต้องใช้เครื่องมือเสริม
3. **Default behavior:** `/architect` อาจ implement ต่อ; Feature playbook เปิด PR; Shipping สามารถ merge — ต้องใส่ explicit stop boundaries และ enforce ผ่าน tool permissions
4. **No native Plane contract:** pstack ไม่ใช่ `work-item-runner` และไม่มี Human Gate verification ตามเรา
5. **Independent review:** multi-model reviewer อาจไม่ available; หากไม่ available ให้ report degraded mode ห้ามกล่าวอ้าง independent panel
6. **Verification evidence:** lint/build/agent self-report ไม่เท่ากับ actual runtime behavior; ถ้าไม่รันต้องรายงาน Not run/Inconclusive
7. **Security:** review permissions, external MCP scopes, secrets, protected branches และ sandbox ก่อนปล่อย agent ทำงาน. Prompt ห้ามไม่ใช่ security boundary
8. **Governance:** decisions ใน `/show-me-your-work` ไม่แทน pilot experiment outcome + evidence + human approval record
9. **Principles เป็น heuristics:** `migrate-callers-then-delete-legacy-apis` และ `never-block-on-the-human` อาจขัด production compatibility / human gates ของเรา; policy ของเราต้องมาก่อน

## 8. Adoption Recommendation / Pilot Evaluation

**Phase A:** ใช้ `/how`, `/why`, `/blast-radius`, `/architect with checkpoint`, `/interrogate` แบบ manual; ทดสอบ compatibility ใน Cursor ก่อน

**Phase B:** ทดลอง `/poteto-mode` Feature/Bug fix แบบห้าม publish, เสริม behavior verification, เก็บ evidence

**Phase C:** เทียบ pstack กับ skills เราใน `puen-stack` สำหรับ task เดียวกัน: requirement correctness, regression escapes, human corrections, review findings, runtime proof, token/cost/time, gate violations

**Exit criteria:** เปลี่ยน/สร้าง custom skill เฉพาะ gap ที่ pilot พิสูจน์ว่าจำเป็น ไม่ fork pstack ทั้งชุดแบบไม่มีเหตุผล

## 9. เอกสารต้นฉบับที่อ่าน / ทางไปอ่านต่อ

- [Guide Index](https://github.com/cursor/plugins/blob/main/pstack/docs/guide/README.md)
- [Understand](https://github.com/cursor/plugins/blob/main/pstack/docs/guide/03-understand.md)
- [Design](https://github.com/cursor/plugins/blob/main/pstack/docs/guide/04-design.md)
- [Build](https://github.com/cursor/plugins/blob/main/pstack/docs/guide/05-build-and-clean.md)
- [Verify and Ship](https://github.com/cursor/plugins/blob/main/pstack/docs/guide/06-verify-and-ship.md)
- [Poteto Mode](https://github.com/cursor/plugins/blob/main/pstack/skills/poteto-mode/SKILL.md)
- [Feature Playbook](https://github.com/cursor/plugins/blob/main/pstack/skills/poteto-mode/playbooks/feature.md)
- [All 23 Playbooks](https://github.com/cursor/plugins/tree/main/pstack/skills/poteto-mode/playbooks)
- [All Skills](https://github.com/cursor/plugins/tree/main/pstack/skills)
- [Our Pilot Playbook](../../playbooks/idea-to-outcome.th.md)

หมายเหตุ: เป็นการวิเคราะห์จาก Source Guides/README/Mode/Feature Playbook และเอกสาร Workflow ของเรา ไม่ใช่รายงานผลรันทุก skill จริง. สิ่งที่ยังไม่ทดลองต้องอยู่ในสถานะ **Not Verified**.
