# Module 7 — Evaluation & Observability: ทักษะที่แยก "expert" ออกจาก "คนทำเป็น"

⏱️ **30 ชั่วโมง (สัปดาห์ 19–20)** · 🎯 ผลลัพธ์: eval suite ที่รันใน CI + dashboard + regression gate

> ถ้าคุณจำได้อย่างเดียวจากคอร์สนี้ ให้จำสิ่งนี้:
> **งาน AI ที่ไม่มี eval = งานที่ไม่มีวันจบ เพราะไม่มีใครรู้ว่าดีพอหรือยัง**
> นี่คือทักษะที่ทำให้ FDE ได้ค่าแรงสูง และเป็นทักษะที่คนส่วนใหญ่ข้าม

---

## 7.1 สามชั้นของการวัดผล

```
┌──────────────────────────────────────────────────────────┐
│ ชั้น 1: EVAL SET (offline)                               │
│ golden set + LLM-as-judge + human review                 │
│ → ตอบคำถาม: "เวอร์ชันนี้ดีกว่าเดิมไหม"                     │
├──────────────────────────────────────────────────────────┤
│ ชั้น 2: OBSERVABILITY (online)                           │
│ traces, logs, tokens, cost, latency, user feedback       │
│ → ตอบคำถาม: "ตอนนี้ production เป็นอย่างไร"               │
├──────────────────────────────────────────────────────────┤
│ ชั้น 3: BUSINESS METRIC                                  │
│ เวลาที่ประหยัด, tickets ที่ลดลง, conversion                 │
│ → ตอบคำถาม: "มันคุ้มค่าไหม"                                │
└──────────────────────────────────────────────────────────┘
```
ถ้าขาดชั้น 1 → เปลี่ยน prompt แล้วเดา; ขาดชั้น 2 → พังแล้วไม่รู้; ขาดชั้น 3 → ไม่มีใครอนุมัติงบ

---

## 7.2 สร้าง Eval Set ที่ใช้ได้จริง

### โครงสร้างไฟล์ (JSONL — ง่ายต่อ version control)
```jsonl
{"id": "e001", "input": "...", "expected": "...", "tags": ["happy_path", "th"]}
{"id": "e002", "input": "...", "expected": "...", "tags": ["edge_case", "mixed_lang"]}
{"id": "e003", "input": "...", "expected": "NOT_FOUND", "tags": ["abstention"]}
{"id": "e004", "input": "...", "expected": "...", "tags": ["adversarial", "injection"]}
```

### สัดส่วนที่แนะนำ (ขั้นต่ำ 50 เคส)
| ประเภท | สัดส่วน | ตัวอย่าง |
|---|---|---|
| Happy path | 40% | คำถามปกติที่ควรตอบได้ |
| Edge case | 20% | ข้อมูลผิดรูป, คำถามสั้นมาก, ภาษาไทยผสมอังกฤษ |
| Abstention | 15% | ข้อมูลไม่มีในระบบ → ต้องตอบ "ไม่พบ" |
| Adversarial | 15% | prompt injection, ขอข้อมูลคนอื่น, ขอให้ทำสิ่งต้องห้าม |
| Ambiguous | 10% | คำถามกำกวม → ควรถามกลับหรือขอความชัดเจน |

### แหล่งเคสจริง (สำคัญมาก)
1. **ประวัติ ticket/แชทของลูกค้า** ← แหล่งที่ดีที่สุด
2. คำถามที่ทีม support ตอบซ้ำ ๆ
3. คำถามที่ระบบเดิมตอบผิด
4. ให้ผู้ใช้จริงช่วยเขียน 20 ข้อ
5. สร้างเอง + ให้ LLM ช่วยสร้างแล้ว**คนคัด**

> ⚠️ **ห้ามใช้ LLM สร้าง eval set แล้วไม่ตรวจ** — คุณจะวัดความเห็นของ LLM ไม่ใช่คุณภาพจริง

---

## 7.3 วิธีให้คะแนน (จากง่าย → แม่น)

| วิธี | เหมาะกับ | ข้อควรระวัง |
|---|---|---|
| **Exact match / regex** | ข้อมูลสกัด (ตัวเลข, วันที่, enum) | เข้มเกินไปสำหรับข้อความ |
| **Contains / F1 ของ token** | งานสรุปสั้น | ลงโทษคำพ้องความหมาย |
| **Semantic similarity (embedding)** | งานตอบคำถาม | คล้าย ≠ ถูก |
| **LLM-as-judge + rubric** | งานเปิด ความยาวมาก | ต้อง validate judge ด้วยคน |
| **Pairwise comparison** | เทียบ 2 เวอร์ชัน | ลำเอียงตามตำแหน่ง (สลับที่ทุกครั้ง) |
| **Human review (สุ่ม)** | ชั้นสุดท้ายยืนยัน | แพง → สุ่ม 5–10% |

### Rubric สำหรับ LLM-as-judge (ใช้ได้เลย)
```
ให้คะแนนคำตอบ 1–5 ตามเกณฑ์:
5 = ถูกต้องครบ อ้างอิงถูก ไม่มีข้อมูลเกิน
4 = ถูกต้อง แต่ขาดรายละเอียดเล็กน้อย
3 = ถูกบางส่วน มีข้อบกพร่องชัดเจน 1 จุด
2 = ผิดประเด็นหลัก แต่ยังเกี่ยวข้อง
1 = ผิดอย่างมีนัยสำคัญ หรือ hallucinate
0 = ไม่ตอบ / ตอบนอกขอบเขต

ตอบเป็น JSON: {"score": int, "reason": str, "issues": [str]}
```

### Validate judge ของคุณ (ห้ามข้าม!)
สุ่ม 30 เคส → ให้คนให้คะแนน → เทียบกับ judge
- ถ้าเห็นตรงกัน <80% → rubric ไม่ชัด หรือ judge อ่อน → แก้ก่อนใช้
- บันทึก agreement rate ไว้ในรายงาน (นี่คือสิ่งที่ทำให้คุณน่าเชื่อถือ)

---

## 7.4 Eval Harness ที่รันซ้ำได้

```python
# evals/run.py
import json, argparse
from pathlib import Path
from concurrent.futures import ThreadPoolExecutor

def load_cases(path): return [json.loads(l) for l in Path(path).read_text().splitlines() if l.strip()]

def run_case(case, system_under_test):
    out = system_under_test(case["input"])
    score = grade(case, out)          # exact / f1 / judge
    return {"id": case["id"], "tags": case["tags"], "output": out, "score": score}

def main():
    ap = argparse.ArgumentParser()
    ap.add_argument("--system", choices=["prompt_v2", "prompt_v3", "prod"])
    ap.add_argument("--concurrency", type=int, default=4)
    args = ap.parse_args()

    cases = load_cases("evals/golden.jsonl")
    sut = load_system(args.system)

    with ThreadPoolExecutor(args.concurrency) as ex:
        results = list(ex.map(lambda c: run_case(c, sut), cases))

    report = summarize(results, by_tag=True)
    Path(f"evals/reports/{args.system}.json").parent.mkdir(parents=True, exist_ok=True)
    Path(f"evals/reports/{args.system}.json").write_text(json.dumps(report, ensure_ascii=False, indent=2))
    print(report["table"])
```

**ผลลัพธ์ที่ควรเห็น:**
```
system       overall  happy  edge  abstain  adversarial   cost   p95_ms
prompt_v2     0.72    0.85  0.61   0.70      0.45        0.008   1840
prompt_v3     0.84    0.90  0.78   0.88      0.62        0.011   1920
```

> 🔑 **นี่คือเอกสารที่ทำให้คุณเป็น expert:** ตารางนี้ตอบได้ทุกคำถามในที่ประชุม
> "ดีขึ้นไหม?" "เท่าไร?" "แลกกับอะไร?" "ยังมีจุดอ่อนตรงไหน?"

---

## 7.5 Regression gate ใน CI

```yaml
# .github/workflows/eval.yml
name: eval-gate
on: [pull_request]
jobs:
  eval:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: pip install -r requirements.txt
      - name: Run eval
        run: python evals/run.py --system prompt_v3
        env: { LLM_API_KEY: ${{ secrets.LLM_API_KEY }} }
      - name: Gate
        run: |
          python - <<'PY'
          import json,sys
          r=json.load(open("evals/reports/prompt_v3.json"))
          baseline=json.load(open("evals/baseline.json"))
          overall, base = r["overall"], baseline["overall"]
          print(f"overall {overall:.3f} vs baseline {base:.3f}")
          if overall < base - 0.03: sys.exit("❌ regression > 3%")
          if r["by_tag"]["adversarial"] < 0.60: sys.exit("❌ adversarial ต่ำเกิน")
          PY
```

**เกณฑ์ gate ที่แนะนำ:**
- overall ลด >3% → block
- adversarial <0.60 → block
- abstention <0.75 → block (สำคัญกับงานองค์กร)
- cost เพิ่ม >30% → เตือน (ไม่บล็อก)

---

## 7.6 Observability — มองเห็น production

### Trace ที่ต้องมีต่อ 1 request
```json
{
  "trace_id": "t_abc",
  "user_id": "u_123",
  "feature": "policy_qa",
  "prompt_version": "policy_qa.v4",
  "model": "xxx",
  "retrieval": {"k": 5, "top_score": 0.82, "chunks": ["doc#1", "doc#2"]},
  "tokens": {"in": 3200, "out": 280},
  "cost_usd": 0.0071,
  "latency_ms": {"total": 2100, "retrieval": 120, "llm": 1900},
  "status": "ok",
  "feedback": null,
  "guardrail_hits": []
}
```

### Dashboard ขั้นต่ำ (ทำได้ใน 1 วัน)
| Panel | Metric | Alert |
|---|---|---|
| Volume | requests/hour | – |
| Quality proxy | % คำตอบที่มี citation | <90% |
| Abstention rate | % ตอบ "ไม่พบ" | เปลี่ยน >50% แบบฉับพลัน |
| Errors | % 5xx / timeout | >2% |
| Latency | p50 / p95 | p95 >5s |
| Cost | USD/day | >budget 120% |
| Feedback | 👍/👎 ratio | 👎 >25% |
| Guardrail | จำนวนครั้งที่ trip | พุ่งขึ้น |

### User feedback loop (ง่ายและทรงพลัง)
```
[👍 ถูกต้อง]  [👎 ไม่ถูกต้อง]  → ถ้า 👎 ให้เลือกเหตุผล:
  ○ ข้อมูลผิด   ○ ไม่ตอบคำถาม   ○ ล้าสมัย   ○ ไม่มีสิทธิ์เข้าถึง
```
เอาเคส 👎 มาทำเป็น eval case ใหม่ทุกสัปดาห์ → **ระบบจะดีขึ้นเรื่อย ๆ เอง**
นี่คือ **data flywheel** ที่ FDE ควรติดตั้งให้ลูกค้าทุกคน

---

## 7.7 Lab (สัปดาห์ 19–20)

### Lab 7.1 (6 ชม.) — Golden set 50 เคส ★
ใช้ระบบจาก Module 5 หรือ 6 ทำ eval set ครบสัดส่วนในหัวข้อ 7.2 พร้อม tag

### Lab 7.2 (8 ชม.) — Harness + Judge
- เขียน `evals/run.py` ให้รันได้ด้วยคำสั่งเดียว
- มี exact match สำหรับงานสกัด + LLM judge สำหรับงานเปิด
- validate judge ด้วยคน 30 เคส และรายงาน agreement rate

### Lab 7.3 (6 ชม.) — Regression report
รัน 3 เวอร์ชัน prompt (หรือ 2 prompts × 2 models) แล้วทำตารางเปรียบเทียบ
เขียนสรุป 1 หน้า: "ควรใช้เวอร์ชันไหน เพราะอะไร และยังมีจุดอ่อนตรงไหน"

### Lab 7.4 (4 ชม.) — CI gate
ใส่ workflow ที่ block PR เมื่อ eval ตกตามเกณฑ์ 7.5

### Lab 7.5 (4 ชม.) — Observability
- ใส่ structured logging ตาม trace schema
- ทำ dashboard ง่าย ๆ (Streamlit/Grafana/แม้แต่ Google Sheet ที่ป้อนอัตโนมัติ)
- ตั้ง alert 3 ตัว

### Lab 7.6 (2 ชม.) — Feedback loop
ทำ UI feedback ง่าย ๆ และสคริปต์ที่แปลง "👎" เป็น eval case อัตโนมัติ

---

## ✅ เกณฑ์ผ่าน

- [ ] eval set ≥50 เคส ครบ 5 ประเภท พร้อม tag
- [ ] judge ผ่านการ validate ด้วยคน (agreement ≥80%)
- [ ] รัน eval ด้วยคำสั่งเดียวได้ และได้รายงานแยกตาม tag
- [ ] มี CI gate ที่ block regression
- [ ] มี trace log + dashboard + 3 alerts
- [ ] มี feedback loop ที่ป้อน eval set ใหม่ได้
- [ ] **อธิบายได้ใน 3 นาที:** ระบบคุณภาพแค่ไหน วัดอย่างไร จุดอ่อนคืออะไร

> 🎓 **ถ้าทำ Module นี้ได้ครบ คุณจะอยู่เหนือผู้สมัคร FDE ส่วนใหญ่แล้ว**
> เพราะคนส่วนใหญ่ทำ demo ได้ แต่ตอบไม่ได้ว่า "คุณภาพเท่าไร และวัดอย่างไร"

---

**ต่อไป →** [`09-module-08-deploy-production.md`](09-module-08-deploy-production.md)
