# Module 4 — LLM ในฐานะวิศวกรรม (ไม่ใช่เวทมนตร์)

⏱️ **30 ชั่วโมง (สัปดาห์ 9–11)** · 🎯 ผลลัพธ์: LLM feature ที่มี prompt versioning + cost model + eval เริ่มต้น

---

## 4.1 เข้าใจว่า LLM คืออะไรจริง ๆ

LLM = โมเดลที่ทำนาย **token ถัดไป** จาก token ก่อนหน้า ทำซ้ำ ๆ จนจบ
ทุกพฤติกรรมที่คุณเห็น (เก่ง, หลอน, ดื้อ, เก่งขึ้นเมื่อให้ตัวอย่าง) มาจากกลไกนี้

### แนวคิดที่ต้องแม่น

| แนวคิด | ความหมาย | ผลต่องานจริง |
|---|---|---|
| **Token** | ชิ้นส่วนคำ (~4 ตัวอักษรอังกฤษ, 1–2 ตัวอักษรไทย ≈ 1 token) | เป็นหน่วยคิดเงินและจำกัดขนาด |
| **Context window** | จำนวน token ที่โมเดลเห็นได้พร้อมกัน | เกินเมื่อไหร่ ข้อมูลต้น ๆ หาย |
| **Temperature / top_p** | ความสุ่ม | งานสกัดข้อมูล → 0, งาน brainstorming → 0.7–1.0 |
| **System / user / assistant** | บทบาทในบทสนทนา | system = นโยบาย, user = งาน |
| **Prompt caching** | เก็บ prefix ที่ซ้ำไว้ | ลดต้นทุนได้ 50–90% ในงาน RAG |
| **Structured output / JSON mode** | บังคับรูปแบบผลลัพธ์ | **จำเป็น** ถ้าผลลัพธ์จะเข้าระบบอื่น |
| **Tool / function calling** | ให้โมเดล "เรียกฟังก์ชัน" | รากฐานของ agent (Module 6) |
| **Multimodal** | รับภาพ/เสียงได้ | ใช้กับเอกสารสแกน ใบเสร็จ |
| **Streaming** | ค่อย ๆ ส่งคำตอบ | สำคัญมากต่อ perceived latency |

### เลือกโมเดลอย่างไร (กรอบคิดที่ใช้ได้ตลอด แม้ชื่อรุ่นจะเปลี่ยน)

```
1. งานต้องแม่นขนาดไหน?           → วัดด้วย eval ไม่ใช่ความรู้สึก
2. ข้อมูลออกนอกองค์กรได้ไหม?     → ถ้าไม่ได้ ต้องใช้ private deployment
3. ต้นทุนต่อ 1,000 ครั้งเท่าไร?   → คำนวณจาก token จริง
4. latency ยอมรับได้เท่าไร?        → <2s สำหรับ UI, ~10s สำหรับ batch
5. context ใหญ่พอไหม?             → เอกสารยาว/บทสนทนายาว
6. ภาษาไทยดีไหม?                  → ต้องทดสอบเอง ไม่เชื่อ benchmark อังกฤษ
```

> 💡 **FDE ที่ดีไม่พูดว่า "รุ่นไหนเก่งสุด"** แต่พูดว่า "รุ่นนี้ผ่าน eval 92% ที่ต้นทุน 0.4 บาท/ครั้ง"

---

## 4.2 เรียก LLM จากโค้ด (โครงสร้างที่ไม่ผูกกับเจ้าไหน)

```python
# src/llm/client.py
from dataclasses import dataclass
import os, time, logging

log = logging.getLogger(__name__)

@dataclass
class LLMResult:
    text: str
    input_tokens: int
    output_tokens: int
    latency_ms: int
    model: str

def complete(
    system: str,
    user: str,
    *,
    temperature: float = 0.0,
    max_tokens: int = 800,
    retries: int = 3,
) -> LLMResult:
    for attempt in range(retries):
        t0 = time.perf_counter()
        try:
            resp = _call_provider(system, user, temperature, max_tokens)
            return LLMResult(
                text=resp.text,
                input_tokens=resp.usage.input,
                output_tokens=resp.usage.output,
                latency_ms=int((time.perf_counter() - t0) * 1000),
                model=resp.model,
            )
        except RateLimitError:
            time.sleep(2 ** attempt)
        except TransientError:
            if attempt == retries - 1:
                raise
    raise RuntimeError("retries exhausted")
```

> 🔑 **ทุก LLM call ต้องบันทึก:** model, tokens, latency, cost, และ prompt version
> ถ้าไม่มีข้อมูลนี้ คุณจะ debug production ไม่ได้ และเถียงเรื่องต้นทุนกับ CFO ไม่ได้

---

## 4.3 Prompt engineering แบบวิศวกร

### โครง prompt ที่ทำงานได้จริง
```
[ROLE]        คุณคือผู้ช่วยวิเคราะห์สัญญาเช่า
[TASK]        สกัดเงื่อนไขสำคัญจากเอกสาร
[INPUT]       <document>...</document>
[RULES]       - ตอบเฉพาะจากเอกสาร ห้ามเดา
              - ถ้าไม่พบ ให้ตอบ "ไม่พบข้อมูล"
[FORMAT]      JSON ตาม schema ที่ให้
[EXAMPLES]    (2–3 ตัวอย่างครอบคลุมเคสปกติ + เคสขอบ)
```

### เทคนิคที่ได้ผลจริง (เรียงตามผลตอบแทน)

| เทคนิค | เมื่อไรใช้ | ผล |
|---|---|---|
| **Few-shot examples** | งานที่ต้องการรูปแบบเฉพาะ | สูงมาก |
| **Structured output schema** | ผลลัพธ์ต้องเข้าระบบ | สูงมาก |
| **Grounding + "ตอบเฉพาะจาก context"** | ลด hallucination | สูง |
| **Decomposition** (แยกงานใหญ่เป็นขั้น) | งานซับซ้อนหลายขั้น | สูง |
| **Self-check / verifier pass** | งานที่ผิดแล้วเสียหาย | กลาง–สูง (เพิ่มต้นทุน) |
| **Chain-of-thought** | งานให้เหตุผล | กลาง (โมเดลใหม่ ๆ ไม่ต้องขอ) |

> ⚠️ **กับดัก:** prompt ที่ดีขึ้น 5% ใน demo อาจไม่มีความหมายถ้า eval set ของคุณเล็ก
> — Module 7 คือสิ่งที่ทำให้คุณรู้ว่าดีขึ้นจริงหรือแค่รู้สึกดีขึ้น

### Prompt versioning (ทำตั้งแต่วันแรก)
```
prompts/
├── extract_lease.v1.md
├── extract_lease.v2.md
└── extract_lease.v3.md      # v3 = เพิ่ม few-shot สำหรับเคส "สัญญาแนบท้าย"
```
เก็บใน Git พร้อม commit message ที่บอกว่าเปลี่ยนเพราะอะไร และ eval score เปลี่ยนจากเท่าไรเป็นเท่าไร

---

## 4.4 ต้นทุน: คำนวณให้เป็นตั้งแต่วันแรก

```python
def cost_thb(input_tokens, output_tokens, price_in_per_mtok, price_out_per_mtok, fx=36.0):
    usd = (input_tokens / 1_000_000) * price_in_per_mtok \
        + (output_tokens / 1_000_000) * price_out_per_mtok
    return usd * fx
```

### แบบฝึกหัดคิดต้นทุน (ทำจริง — ใช้ตัวเลขจาก pricing จริง)
| สถานการณ์ | ปริมาณ | ต้องตอบ |
|---|---|---|
| ผู้ใช้ 500 คน/วัน, 4 ครั้ง/คน | 2,000 calls/วัน | ต้นทุน/เดือน? |
| RAG: context 6k token + output 400 | 2,000 calls/วัน | ต้นทุน/เดือน? |
| ลดต้นทุน 70% ได้ไหม | – | 3 วิธีที่เป็นไปได้ |

**วิธีลดต้นทุนที่ใช้จริง:** caching, ลด context ที่ไม่จำเป็น, ใช้รุ่นเล็กเป็น router ก่อน,
batch offline, และ **ตัดฟีเจอร์ที่ไม่มีคนใช้** (วิธีที่ได้ผลที่สุด)

---

## 4.5 Reliability & Failure modes

| Failure | อาการ | มาตรการ |
|---|---|---|
| **Rate limit (429)** | ใช้งานพร้อมกันเยอะ | queue + backoff + circuit breaker |
| **Timeout** | คำตอบช้า/ค้าง | timeout เข้มงวด + fallback response |
| **Hallucination** | ตอบมั่นใจแต่ผิด | grounding + "ไม่พบข้อมูล" + eval |
| **Prompt injection** | ผู้ใช้สั่งให้ลืมกฎ | แยก input ผู้ใช้, ตรวจ output, จำกัดสิทธิ์ tool |
| **Non-determinism** | รันซ้ำได้ผลต่าง | temperature 0 + eval ที่ยอมรับ variance |
| **Context overflow** | ตัดข้อมูลเงียบ ๆ | นับ token ก่อนส่ง + summarize |
| **PII leak** | ข้อมูลส่วนตัวหลุดเข้า log | redact ที่ logging layer |
| **Cost blowup** | บิลพุ่ง | budget alert + max_tokens + rate limit ต่อผู้ใช้ |

**Lab 4.4:** จงใจทำให้ระบบตัวเองล้ม 4 แบบข้างบน แล้วเขียนว่าแต่ละครั้งตรวจพบได้อย่างไร

---

## 4.6 Lab (สัปดาห์ 9–11)

### Lab 4.1 (4 ชม.) — Hello, structured output
สกัดข้อมูลจากอีเมลธุรกิจ (เช่น ชื่อผู้ส่ง, วันที่, รายการสินค้า, ยอดเงิน) เป็น JSON
ตาม schema ที่กำหนด และต้องได้ JSON ถูกต้อง ≥95% จาก 20 ตัวอย่าง

### Lab 4.2 (8 ชม.) — Prompt iteration ที่มีหลักฐาน
- เขียน 20 เคสทดสอบ (input + expected output)
- ทำ prompt v1 → วัดผล → v2 → วัดผล → v3
- บันทึกตาราง: version, accuracy, ต้นทุน, สิ่งที่เปลี่ยน
- **ห้าม** บอกว่า v3 ดีขึ้นถ้าไม่มีตัวเลข

### Lab 4.3 (8 ชม.) — Wrapper ที่ production-ready ★
- retry + backoff + timeout
- structured logging (model, tokens, latency, cost, prompt_version)
- config ผ่าน env
- fallback model เมื่อตัวหลักล่ม
- unit test ด้วย mock (ไม่เรียก API จริงทุกครั้ง)

### Lab 4.4 (6 ชม.) — Failure injection + cost model
ทำ failure 4 แบบ + เขียนตารางต้นทุน 3 สถานการณ์ตามหัวข้อ 4.4
**ส่งออกเป็น:** `docs/lab-4-cost-model.md` (ใช้ต่อใน Module 12)

### Lab 4.5 (4 ชม.) — เปรียบเทียบ 2 โมเดล
รัน eval set เดียวกันกับ 2 โมเดล (เช่น รุ่นเล็ก vs รุ่นใหญ่) แล้วสรุปเป็นตาราง
**คุณภาพ / ต้นทุน / latency** — พร้อมข้อเสนอแนะว่าควรใช้ตัวไหนกับงานแบบใด

---

## ✅ เกณฑ์ผ่าน

- [ ] อธิบาย token/context/temperature และผลต่อพฤติกรรมได้
- [ ] เรียก LLM API จากโค้ดได้ พร้อม retry/timeout
- [ ] บังคับ structured output ได้ ≥95%
- [ ] มี prompt versioning ใน Git พร้อมเหตุผลการเปลี่ยนแปลง
- [ ] คำนวณต้นทุนต่อเดือนได้จากตัวเลขจริง
- [ ] ระบุ failure mode ได้ 6 อย่างและมาตรการได้
- [ ] มีตารางเปรียบเทียบโมเดล 2 ตัวจาก eval ของตัวเอง

---

**ต่อไป →** [`06-module-05-rag.md`](06-module-05-rag.md)
