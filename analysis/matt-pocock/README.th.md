# วิเคราะห์ Matt Pocock Engineering Skills สำหรับ My Engineer (ภาษาไทย)

> วิเคราะห์: 12 ตุลาคม 2026 | Source: [mattpocock/skills/docs/engineering](https://github.com/mattpocock/skills/tree/main/docs/engineering) | สถานะ: Source analysis, ยังไม่ใช่ผลพิสูจน์การรัน Skills

## 1. คำตัดสิน

**ควรใช้ Matt Pocock Skills เป็นชั้น Requirement → Spec → Work Item → Engineering Review ระหว่างรอ Skills ของเรา** และใช้ pstack ช่วย Codebase Investigation, Architecture, Verification และ Adversarial Review เฉพาะจุดที่เหมาะสม โดยให้ **7 Accepted My Engineer Workflows / Human Gates / Provider Permissions** เป็นเจ้าของ Lifecycle เสมอ

หลักการของ Matt คือ skills เล็ก ๆ ที่ composable ได้; ไม่ใช่ agent orchestration framework แบบ pstack. ต้องแยก **user-invoked orchestrator** กับ **model-invoked reference** และอย่าเข้าใจว่า skill ทุกตัวรัน flow จนจบเอง.

## 2. Entry points และ chain หลัก

```
IDEA / REQUIREMENT
  → /grill-with-docs [เมื่อความต้องการยังไม่ชัด; glossary + ADR]
  → /to-spec [สรุปข้อตกลงเป็น spec; เหมาะงานหลาย sessions]
  → /to-tickets [vertical-slice/tracer-bullet tickets + blockers]
  → /implement [หนึ่ง ticket ต่อหนึ่ง session; red-green, typecheck, tests, review, commit]
  → /code-review [Standards vs Spec เป็นคนละแกน]
  → /pr [เขียน PR body: Summary, Evidence, Merge Danger]
  → /retro [เสนอการปรับ environment, ไม่มี automatic fixes]
```

**ทางแยกสำคัญ**
- Idea/architecture ที่ยังตัดสินไม่ได้และกินหลาย sessions: `/wayfinder` สร้าง **decision tickets** จนคำถามหมด แล้วค่อย `/to-spec`. ห้ามส่ง decision tickets ให้ `/implement` ตรง ๆ
- งานเล็กซึ่งตัดสินใจแล้ว: ข้าม spec/tickets ไป `/implement` ได้ แต่สำหรับ My Engineer ยังต้องมี approved scope, acceptance และ gate ตามความเสี่ยง
- Incoming unconfirmed bug: `/triage` คัดกรอง → `/diagnosing-bugs` ตั้ง failing reproducible loop → `/tdd` → review
- งานทั้ง spec ที่ต้องขนาน: `/implement-spec` ทำ orchestration ตาม DAG ของ tickets, subagents + worktrees + integration branch; **เสี่ยงต่อ bypass gates** และเปลือง resource จึงยังไม่เปิดใช้ใน pilot เริ่มต้น
- ไม่รู้จะเลือก skill ไหน: `/ask-matt` แนะนำแล้วหยุด ไม่ได้ execute skill ต่อ

## 3. Mapping กับ My Engineer Accepted Workflows

| My Engineer workflow | Skill Matt ที่ควรหยิบ | Artifact/Output ใกล้เคียง | Gap / ข้อกำหนดของเรา |
|---|---|---|---|
| Requirement Discovery | `grill-with-docs`, `domain-modeling`, `wayfinder` (งานใหญ่), `triage` (incoming) | Clarified decisions, `GLOSSARY.md`, ADRs, decision map | ต้องยืนยัน Problem/Outcome/Scope/Acceptance/Human Owner และ Plane record; ห้ามให้ agent ตอบคำถามแทน PO |
| Technical Discovery | `research`, `codebase-design`, `improve-codebase-architecture` | Fact finding, module/seam/depth vocabulary, candidate architecture improvements | ไม่ได้แทน trace-as-is แบบ `pstack /how` และ impact analysis ข้าม repos; เก็บ path/line/evidence เพิ่ม |
| Solution Design | `grill-with-docs`, `codebase-design`, `prototype`, `domain-modeling`, `to-spec` | Module design, agreed seam, alternatives from prototypes, spec | `to-spec` **บันทึก decision ที่ตกลงแล้ว** ไม่ใช่ design workshop; ต้องมี approved design gate |
| Delivery Planning | `to-spec`, `to-tickets`, `wayfinder` ก่อนหน้า (ถ้าต้องใช้) | Tracker spec + tracer-bullet implementation tickets พร้อม blockers | แตก tickets หลัง Human Planning Gate; Plane write ต้องได้รับ authorization; progressive-detail ต่อ work item |
| Implementation | `implement`, `tdd`, `diagnosing-bugs`, `implement-spec` (advanced) | Scoped code, red-green evidence, checks, commit | `implement` ปกติ **commit อัตโนมัติ**; ห้ามใช้ตรง ๆ ก่อน scoped commit authorization หรือปรับ invocation/custom wrapper ให้หยุดก่อน commit |
| Verification | `code-review`, `tdd`, `diagnosing-bugs` | Standards + Spec axes, test behavior | review ไม่เท่ากับ runtime/e2e acceptance; fixed ref diff ต้องมี commit; formal independent verification + Human Verification Gate ของเรายังคงจำเป็น |
| Release Operations | `pr`, `wizard` (manual setup), `retro` (post-mortem) | PR body: summary/evidence/merge danger; ops wizard; improvements | `pr` **ไม่ push และไม่เปิด PR เอง**; ต้องใช้ Git integration/skills ของเรา; ห้าม auto merge/deploy; release monitoring/outcome อยู่ใน workflow เรา |
| Cross-workflow Outcome & Learning | `retro` | Ranked environment improvement proposals | pilot recorder ต้องบันทึก experiment, failures, actual outcome, tokens/cost ถ้าวัดได้ ลง `pilot-projects/docs` พร้อมหลักฐาน |

## 4. Catalog: ทุกหัวข้อใน docs/engineering ที่ตรวจพบ

| Documentation / Skill | ใช้ตอนไหนและได้อะไร | หมายเหตุ |
|---|---|---|
| `ask-matt` | Router ช่วยเลือก skill/chain แล้วหยุด | ไม่ trigger งานจริง |
| `setup-matt-pocock-skills` | ตั้งค่า tracker, labels, domain docs ใน `docs/agents/` | ทำต่อ repo; เลือกให้เขียน `AGENTS.md` เดิม ไม่สร้าง CLAUDE.md |
| `grill-with-docs` | Interview requirement ที่ยังคลุมเครือ; เขียน Glossary/ADR ระหว่างถาม | ต้องมี `grilling` และ `domain-modeling`; หยุดให้คนตัดสินใจ |
| `domain-modeling` | ภาษาร่วมและโดเมนโมเดลที่ใช้ตั้งชื่อโค้ด/เอกสาร | เป็น foundational/reference ไม่ใช่ product approval |
| `wayfinder` | ระบุ destination แล้วจัดการ decision tickets ระยะหลาย sessions | map != implementation tickets; tracker support matters |
| `research` | ค้น evidence ที่ยังขาดก่อน decision | ประเมินหลักฐาน/ความไม่แน่นอน |
| `prototype` | ทดลองไอเดีย/flow/design ด้วยสิ่งที่รันและสังเกตได้ | prototype ไม่ใช่ production implementation |
| `to-spec` | สังเคราะห์ข้อตกลงที่ได้แล้วเป็น spec ใน issue tracker | ไม่ควรใช้แทน clarification; ได้ spec หนึ่งชิ้น |
| `to-tickets` | แตก spec เป็น tracer-bullet tickets และ blocking edges | ข้าม layer ได้ แต่ยังคง demo ได้แต่ละ slice |
| `implement` | Implement 1 ticket, TDD/typecheck/test/review แล้ว commit | review fixed point อาจว่างถ้า diff ยัง uncommitted; ควบคุม commit gate |
| `implement-spec` | Orchestrate ticket DAG parallel worktrees และ integration branch | advanced; cost/conflict/gate risks; skip ใน pilot รอบแรก |
| `tdd` | Red-green-refactor บน behavioral seams ที่ตกลงก่อนเขียน test | reference skill; ไม่คุ้มสำหรับ glue/CRUD ที่ไม่มี independent assertion |
| `diagnosing-bugs` | Repro → minimize → hypotheses → instrumentation → fix/test → cleanup | ต้องพิสูจน์ failing loop ก่อนเดาสาเหตุ |
| `codebase-design` | นิยาม module/interface/depth/seam/adapter/leverage เพื่อประเมิน design | reference skill ไม่ใช่ task runner |
| `improve-codebase-architecture` | Read-only survey deep modules, HTML report, แล้ว grill candidate | ไม่แก้ product code และไม่ควรแทรกทุก task |
| `code-review` | Two-axis independent subagent review: Standards และ Spec | ต่างจาก security/bug-hunting review; ต้องระบุ fixed point และ spec |
| `pr` | **เขียน body อย่างเดียว**: Summary, Evidence, Merge Danger | ไม่เปิด PR, ไม่ push; อาจชน repo template |
| `retro` | วิเคราะห์ friction ใน session เสนอ tests/lint/pointers/standards | ไม่แก้อะไรจน human เลือก candidate |
| `triage` | Intake bug/feature request ที่มาจากคนอื่น แล้วให้สถานะ/label | ไม่ใช่ diagnosis ลึก; ตรวจ tracker configuration |
| `wizard` | สร้าง interactive Bash wizard สำหรับ manual provisioning | ไม่รันเอง; ระวัง secrets และ policy |
| `resolving-merge-conflicts` | **Archived** ตั้งแต่ v1.3.0 ไม่ maintained | อย่าใส่ใน recommended installed pack |

## 5. ตัวอย่าง Practical Workflow: Feature จาก Idea ถึง Draft PR/MR

### A. Requirement Discovery
```text
/grill-with-docs
Idea: <idea>
ถามทีละรอบเพื่อสรุป user/problem/outcome/acceptance/non-goals.
แยก Confirmed / Assumed / Unknown. อธิบาย domain terms และ ADR ที่ยืนยันจริง.
อย่าเขียน production code หรือ create tracker work items โดยไม่ได้รับอนุญาต.
```
Output: Requirement Summary + glossary/ADR เท่าที่มี **และ Human Requirement Gate**. ถ้างานยาวหลาย sessions ใช้ `/wayfinder` ก่อน spec.

### B. Technical Discovery + Solution Design
ใช้ pstack `/how` และ `/blast-radius` สำรวจ As-Is; Matt `/codebase-design` แยก module/interface/seam; `/prototype` เมื่อต้องทดลองความเป็นไปได้; `/to-spec` สรุป decision หลัง Human Design Gate. ถ้า design ยังไม่อนุมัติ อย่าให้ `/to-spec` เสก decision เอง

### C. Delivery Planning
```text
/to-tickets <approved spec URL>
Break into independently demoable tracer-bullet vertical slices; list blockers,
acceptance, impacted contracts, owner and verification approach.
Propose tickets first; don't write tracker items until Human Planning Gate.
```
Output: Work Items + Dependencies (ต้องตรวจจริงใน Plane Cloud; generic tracker adapter อาจรองรับไม่ครบ)

### D. Implement Work Item
```text
/implement <exact ticket URL>
Implement only approved scope; use TDD at agreed behavioral seams.
Show tests and changed diff. DO NOT commit, push, open PR, merge or deploy.
Stop for My Engineer Human Implementation Gate.
```
**สำคัญ:** นี่เป็น **policy override ที่ต้องทดสอบจริง** เนื่องจากต้นฉบับ skill ชอบ commit อัตโนมัติ; สำหรับ pilot ที่เคร่ง gates ให้ใช้ custom wrapper / restricted git permissions จนยืนยันได้ ไม่ใช่เชื่อ prompt อย่างเดียว

### E. Formal Independent Verification
```text
/code-review <fixed base ref>
Review Standards and Spec independently against <approved ticket/spec URL>.
Return concrete findings, severity, evidence and gaps. Do not modify code.
```
ต้องตรวจว่ามี committed diff ระหว่าง fixed point กับ HEAD จริง; ระหว่างยังไม่ commit อาจต้องสร้าง ephemeral review snapshot (หลังได้รับอนุญาต) หรือใช้ review skill อื่นที่รองรับ working tree. พ่วง runtime/API/UI verification แยก; Human Verification Gate ก่อน publish

### F. Commit, Draft PR/MR, Release, Learn
- `git-commit` ของเรารองรับ `[WORK-123] feat: ...`, และ commit permission แยก push
- `/pr` **ช่วยเขียน description** เท่านั้น; ใช้ git-host connector เปิด Draft PR/MR ภายใต้ authorization แยก
- ไม่ auto Ready/Merge/Deploy; Manual Production Go/No-Go + Observe
- `/retro` เสนอ environment improvements แล้ว human เลือก + `pilot-recorder` เก็บ actual outcome

## 6. Matt vs pstack: ใช้เสริมกันอย่างไร

| งาน | Matt Pocock | pstack | ชั่วคราวเลือก |
|---|---|---|---|
| Requirement clarification | `grill-with-docs` (glossary/ADR) | Investigation / Prototype | **Matt** |
| Code flow and history | Research/Codebase-design | `how`, `why`, `blast-radius` | **pstack** |
| Design alternatives | codebase-design/prototype | `architect`, `arena` | ตามระดับ design risk; high-risk ใช้ pstack architecture |
| Ticket breakdown | `to-spec`, `to-tickets` | Multi-phase plan | **Matt** (ต้องปรับ Plane + gates) |
| Scoped implementation | `implement` (auto commit) | Feature playbook (auto PR) | **เลือกอย่างใดอย่างหนึ่ง**, บังคับหยุดก่อน publish/commit ตาม gate |
| Test discipline | `tdd` | `tdd`, prove-it-works | Matt `tdd` + pstack actual-app verification เมื่อจำเป็น |
| Independent review | `code-review` Standards/Spec | `interrogate` adversarial diff | **ใช้คู่กันเมื่อ high-risk**; ไม่จำเป็นสองชุดทุก task |
| Release handoff | `pr` body only | Opening PR, Babysit, Shipping | **Matt /pr** + `git-commit`/`git-draft-pr` ของเรา; งด Shipping |
| Learning | `retro` environment proposals | `reflect`, `correct`, `show-me-your-work` | **Matt retro** + pilot evidence |

## 7. Risks/Trade-offs ที่ห้ามมองข้าม

1. Skills README เน้น composability; **ไม่ใช่ guaranteed end-to-end autonomous workflow**. ให้ coordinator ของเราคุม stage/gate
2. `/grill-with-docs` เขียน `GLOSSARY.md` และ ADR จริงขณะสนทนา → ตรวจ changes ก่อนยอมรับ
3. `/to-spec`, `/to-tickets`, `/wayfinder` เขียน tracker → Plane Cloud ต้องมี adapter and confirmed write authorization, ไม่เดาความสามารถ native blockers
4. `/implement` ทำ commit เอง ส่วน `/implement-spec` สามารถ resolve tickets และ draft PR: ตรงข้าม verify-before-publish policy ใน `playbooks/idea-to-outcome.th.md`
5. `/code-review` เทียบ fixed ref...HEAD; uncommitted working tree อาจไม่อยู่ใน diff → ต้องตรวจ review scope จริง
6. `/code-review` ช่วย Standards/Spec ไม่รับประกันความถูกต้อง runtime/integration/security; verify พฤติกรรมจริงยังต้องแยก
7. `/tdd` ต้องตกลง seams ก่อน; ข้ามสำหรับ glue/wiring เมื่อไม่มี independent behavioral expectation
8. `/implement-spec` ต้องมี background subagents/worktrees และตั๋วที่ dependency ถูกต้อง; parallelism ไม่ฟรี
9. `AGENTS.md` คือแหล่ง steering เดียวใน My Engineer; หาก setup เสนอ `CLAUDE.md` ให้กำหนดตรง `AGENTS.md` ที่มีอยู่และตรวจว่าไม่ duplicate
10. Docs/engineering มีรายการ archived เช่น `resolving-merge-conflicts`; อย่าสรุปว่าทุกหน้าเป็น skill active

## 8. Recommended Pilot Pack และเกณฑ์ประเมิน

**Starter pack:** `setup-matt-pocock-skills` (config), `grill-with-docs`, `to-spec`, `to-tickets`, `tdd`, `code-review`, `pr`, `retro`; เสริม `diagnosing-bugs` สำหรับ bug, `domain-modeling`/`codebase-design` เป็น dependencies.

**Pstack complementary:** `how`, `why`, `blast-radius`, `architect with checkpoint`, `create-verification-skill`; อย่าเริ่มจาก multi-agent arena/swarm/implement-spec ทุกครั้ง

**Pilot evidence:** stage/gate, actual artifacts/URLs, before-after reproduction, test coverage by behavior, reviewer findings, corrections, token/cost/runtime ที่วัดได้, regression, permission violations, Worked/Partial/Failed/Not Verified. เก็บใน `pilot-projects/docs`; source-of-truth policy อยู่ `puen-engineer`.

**ความสำเร็จ:** Requirement alignment ดีขึ้น, scope drift ลดลง, reviewer ตอบได้ว่าทำ *สิ่งที่ถูก* และ *ทำถูกวิธี*, evidence reproduce ได้, ไม่มี human gate bypass. อย่าสร้าง custom skill จนกว่าจะมีผล pilot ที่ชี้ gap.

## 9. References

- [Engineering docs (21 pages)](https://github.com/mattpocock/skills/tree/main/docs/engineering)
- [README / installation / philosophy](https://github.com/mattpocock/skills/blob/main/README.md)
- [Engineering SKILL.md sources](https://github.com/mattpocock/skills/tree/main/skills/engineering)
- [Our 7 workflows](../../workflows/feature-delivery-lifecycle.th.md)
- [Our Pilot playbook](../../playbooks/idea-to-outcome.th.md)
- [Pstack comparison](../pstack/README.th.md)

> หมายเหตุ: ตรวจหน้าคู่มือ engineering และ README ของต้นฉบับแล้ว แต่ **ไม่ได้ทดสอบทุก skill ใน agent runtime จริง**. รายงานพฤติกรรมที่ยังไม่ทดลองเป็น Not Verified ไม่ใช่ Pass.
