# FDE Learning Course — สาย Non-Software Developer

> คอร์สเรียนเพื่อไปเป็น **Forward Deployed Engineer (FDE) ระดับ expert**
> ออกแบบมาสำหรับคนที่**ไม่ได้จบสาย software engineering** หรือไม่ได้เป็น developer มาก่อน
> (เช่น BA, consultant, data analyst, PM, ops, finance, researcher, นักการตลาด ฯลฯ)

---

## ทำไมคอร์สนี้ต่างจากคอร์ส "เรียน AI" ทั่วไป

FDE ไม่ใช่ Data Scientist และไม่ใช่ ML Engineer — FDE คือคนที่**ถูกส่งไปหน้างานลูกค้า** แล้วรับผิดชอบ
ตั้งแต่ "หาว่าควรสร้างอะไร" ไปจนถึง "ระบบรันจริงและมีคนใช้งาน" คอร์สนี้จึงไม่เน้นทฤษฎีโมเดล
แต่เน้น 4 อย่างที่ตลาดต้องการจริง:

| เสาหลัก | น้ำหนัก | ทำไม |
|---|---|---|
| **1. Technical Floor** — พื้นฐานที่ต้องแน่นจนไม่กลัวของใหม่ | 20% | FDE ต้องอ่าน error ได้ ต่อ API เป็น และไม่ต้องพึ่งคนอื่นทุกครั้ง |
| **2. AI Engineering** — สร้างระบบ LLM ได้จริง ไม่ใช่แค่ prompt เล่น | 30% | เป็นหัวใจของงาน FDE ยุคนี้ (RAG, agent, eval) |
| **3. Deployment & Enterprise Reality** — เอาเข้าระบบองค์กรได้ | 20% | จุดที่โปรเจกต์ AI 95% ตาย — ไม่ใช่เพราะโมเดลห่วย |
| **4. FDE Craft** — discovery, delivery, adoption, exec comms | 30% | **จุดที่คน non-dev ได้เปรียบที่สุด** และเป็นตัวแยก "expert" ออกจาก "คนเขียนโค้ดเป็น" |

> ⚠️ **ความจริงที่ต้องรู้ก่อนเริ่ม:** ตำแหน่ง FDE มี coding interview เกือบทุกที่
> คน non-dev ไปถึงได้ แต่ต้องยอมรับว่า Phases 1–3 คือ "ค่าผ่านประตู" ที่ข้ามไม่ได้
> ส่วน Phase 4 คือที่ที่คุณจะชนะคนจบ CS — อย่าข้ามมันเพื่อไปรีบเขียนโค้ด

---

## เส้นทางภาพรวม (12 เดือน / ~470 ชั่วโมง)

```
PHASE 0  Orientation            6h    สัปดาห์ 0        ประเมินตัวเอง + เลือก vertical
─────────────────────────────────────────────────────────────────────────
PHASE 1  Technical Floor       80h    สัปดาห์ 1–8      software, data/SQL, Python
PHASE 2  AI Engineering       120h    สัปดาห์ 9–20     LLM, RAG, agent, EVAL
PHASE 3  Production           80h    สัปดาห์ 21–28    deploy, security, enterprise
─────────────────────────────────────────────────────────────────────────
PHASE 4  FDE Craft ★         100h    สัปดาห์ 29–40    discovery, delivery, adoption
PHASE 5  Expertise            80h    สัปดาห์ 41–52    capstone, specialization, ยื่นงาน
```

**ตารางเรียนที่แนะนำ**
- **แบบทำงานไปด้วย:** 12 ชม./สัปดาห์ → ~39 สัปดาห์ (~9 เดือน)
- **แบบเร่ง:** 25 ชม./สัปดาห์ → ~19 สัปดาห์ (~4.5 เดือน)
- **ขั้นต่ำที่ไม่แนะนำให้ต่ำกว่านี้:** 8 ชม./สัปดาห์ — ต่ำกว่านี้ความต่อเนื่องจะขาด

---

## สารบัญ

### เริ่มตรงนี้
- [`docs/00-roadmap.md`](docs/00-roadmap.md) — roadmap ละเอียด รายสัปดาห์ + เกณฑ์ผ่านแต่ละ phase
- [`docs/01-orientation.md`](docs/01-orientation.md) — Module 0: FDE คืออะไรจริง ๆ, ประเมินตัวเอง, เลือก vertical

### Phase 1 — Technical Floor
- [`docs/02-module-01-software-internet.md`](docs/02-module-01-software-internet.md) — Module 1: software, network, API, Git, cloud
- [`docs/03-module-02-data-sql.md`](docs/03-module-02-data-sql.md) — Module 2: data literacy + SQL จากศูนย์
- [`docs/04-module-03-python.md`](docs/04-module-03-python.md) — Module 3: Python สำหรับคนสร้างของ

### Phase 2 — AI Engineering
- [`docs/05-module-04-llm-engineering.md`](docs/05-module-04-llm-engineering.md) — Module 4: LLM ในฐานะวิศวกรรม
- [`docs/06-module-05-rag.md`](docs/06-module-05-rag.md) — Module 5: RAG และ grounding
- [`docs/07-module-06-agents.md`](docs/07-module-06-agents.md) — Module 6: tool use / agent / guardrails
- [`docs/08-module-07-evals-observability.md`](docs/08-module-07-evals-observability.md) — Module 7: **eval & observability** (ตัวแยก expert)

### Phase 3 — Production & Enterprise
- [`docs/09-module-08-deploy-production.md`](docs/09-module-08-deploy-production.md) — Module 8: เอา systems ขึ้น production
- [`docs/10-module-09-enterprise-security.md`](docs/10-module-09-enterprise-security.md) — Module 9: security, SSO, data residency, IT review

### Phase 4 — FDE Craft ★
- [`docs/11-module-10-discovery.md`](docs/11-module-10-discovery.md) — Module 10: discovery & problem framing
- [`docs/12-module-11-delivery-adoption.md`](docs/12-module-11-delivery-adoption.md) — Module 11: delivery, adoption, change management
- [`docs/13-module-12-commercial.md`](docs/13-module-12-commercial.md) — Module 12: ROI, pricing, working with sales & product

### Phase 5 — Expertise
- [`docs/14-module-13-capstone.md`](docs/14-module-13-capstone.md) — Module 13: capstone แบบ deploy จริงครบวงจร
- [`docs/15-module-14-positioning-interview.md`](docs/15-module-14-positioning-interview.md) — Module 14: portfolio, CV, interview loop
- [`docs/16-assessment-rubrics.md`](docs/16-assessment-rubrics.md) — rubric ให้คะแนนตัวเองทุก module
- [`docs/99-resources-glossary.md`](docs/99-resources-glossary.md) — แหล่งเรียนฟรี + glossary ไทย
- [`docs/20-study-resources.md`](docs/20-study-resources.md) — **แหล่งเรียนคัดสรร ~70 รายการพร้อมลิงก์** จัดกลุ่มตาม 5 Phase + แหล่งไทย + ตารางเรียนที่แนะนำ

### Templates (ใช้จริงในงาน)
- [`templates/my-fde-profile.md`](templates/my-fde-profile.md) — แบบฟอร์ม Module 0
- [`templates/opportunity-assessment.md`](templates/opportunity-assessment.md)
- [`templates/solution-design-doc.md`](templates/solution-design-doc.md)
- [`templates/eval-plan.md`](templates/eval-plan.md)
- [`templates/delivery-plan.md`](templates/delivery-plan.md)
- [`templates/weekly-ops-review.md`](templates/weekly-ops-review.md)

---

## กติกา 5 ข้อของคอร์สนี้

1. **ทุก module ต้องมี artifact** — ไฟล์ที่รันได้ หรือเอกสารที่ใช้กับลูกค้าจริงได้ ไม่มี artifact = ไม่ผ่าน
2. **ห้ามเรียนแบบดูวิดีโออย่างเดียว** — ทุก concept ต้องถูกนำไปใช้ใน lab ภายใน 7 วัน
3. **เขียนลง public** — GitHub repo, blog, หรือ demo ที่แชร์ได้ นี่คือ portfolio ของคุณ
4. **วัดผลด้วย eval ไม่ใช่ด้วยความรู้สึก** — ใช้ตั้งแต่ Module 4 เป็นต้นไป
5. **Phase 4 ห้ามข้าม** — เป็นส่วนที่ทำให้คุณเป็น FDE ไม่ใช่แค่ junior AI dev

---

## Checklist: เริ่มวันนี้

- [ ] ติดตั้ง Python 3.11+, VS Code (หรือ Cursor), Git
- [ ] สมัคร GitHub + สร้าง repo ชื่อ `fde-journey`
- [ ] ทำ self-assessment ใน `docs/01-orientation.md`
- [ ] เลือก **vertical** ที่ตัวเองมี domain knowledge (สำคัญมาก — ดู Module 0)
- [ ] ตั้ง calendar: block เวลาเรียน 3 slot/สัปดาห์
- [ ] เริ่ม Module 1 สัปดาห์นี้

---

*อัปเดตล่าสุด: พฤศจิกายน 2026*
