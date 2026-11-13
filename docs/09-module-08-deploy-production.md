# Module 8 — เอา Systems ขึ้น Production

⏱️ **35 ชั่วโมง (สัปดาห์ 21–23)** · 🎯 ผลลัพธ์: service ที่มี CI/CD, monitoring, rollback, และ runbook

> **FDE ต่างจาก dev ทั่วไปตรงนี้:** คุณต้อง deploy เข้า**สภาพแวดล้อมของลูกค้า** ได้
> ซึ่งบางครั้งไม่มี CI/CD, ไม่มี Docker, และมี firewall ที่คุณควบคุมไม่ได้

---

## 8.1 สถาปัตยกรรมขั้นต่ำของ AI service

```
                    ┌──────────────────────┐
   ผู้ใช้ ──────────▶│  API (FastAPI)       │
                    │  - auth              │
                    │  - rate limit        │
                    │  - validate input    │
                    └──────────┬───────────┘
                               │
              ┌────────────────┼────────────────┐
              ▼                ▼                ▼
      ┌──────────────┐ ┌──────────────┐ ┌──────────────┐
      │ Retrieval    │ │ LLM provider │ │ DB / cache   │
      │ (pgvector)   │ │ (fallback x2)│ │ (Postgres)   │
      └──────────────┘ └──────────────┘ └──────────────┘
                               │
                    ┌──────────▼──────────┐
                    │ Observability       │
                    │ logs/traces/metrics │
                    └─────────────────────┘
```

**ขั้นต่ำที่ขาดไม่ได้:**
1. **Health endpoint** (`/healthz`) — ให้ load balancer/monitoring รู้ว่ายังมีชีวิต
2. **Timeouts ทุกที่** — DB, HTTP, LLM (ไม่มี timeout = ค้างทั้งระบบ)
3. **Retry + fallback model** — LLM เจ้าหนึ่งล่ม ต้องสลับได้
4. **Config ผ่าน env** — ห้าม hardcode URL/key
5. **Graceful shutdown** — รับ request ที่ค้างให้จบก่อน

---

## 8.2 FastAPI — โครงที่ใช้ได้ทันที

```python
# app/main.py
from fastapi import FastAPI, HTTPException, Depends
from pydantic import BaseModel, Field
import logging, time, uuid

app = FastAPI(title="Policy QA")
log = logging.getLogger("app")

class AskRequest(BaseModel):
    question: str = Field(min_length=1, max_length=2000)
    lang: str = Field(default="th", pattern="^(th|en)$")

class AskResponse(BaseModel):
    answer: str
    citations: list[str]
    trace_id: str

@app.get("/healthz")
def healthz():
    return {"ok": True}

@app.post("/ask", response_model=AskResponse)
def ask(req: AskRequest, user=Depends(get_current_user)):
    trace_id = f"t_{uuid.uuid4().hex[:8]}"
    t0 = time.perf_counter()
    try:
        result = pipeline.answer(req.question, user=user, trace_id=trace_id)
        log.info("ask_ok", extra={"trace_id": trace_id, "ms": int((time.perf_counter()-t0)*1000)})
        return AskResponse(answer=result.text, citations=result.citations, trace_id=trace_id)
    except UpstreamTimeout:
        log.warning("upstream_timeout", extra={"trace_id": trace_id})
        raise HTTPException(504, "ระบบ AI ตอบช้า กรุณาลองใหม่")
    except NoPermission:
        raise HTTPException(403, "ไม่มีสิทธิ์เข้าถึงข้อมูลนี้")
```

**สิ่งที่ต้องมีเพิ่มก่อนใช้จริง:**
- Rate limit ต่อผู้ใช้ (กันคนเดียวยิงจนบิลระเบิด)
- Request ID ในทุก log
- Redact PII ก่อน log
- `max_tokens` และ budget ต่อ request

---

## 8.3 Deploy: 3 ระดับที่ FDE เจอจริง

| ระดับ | สภาพแวดล้อม | วิธี deploy | ความยาก |
|---|---|---|---|
| **A. Cloud ของเรา** | AWS/GCP/Azure | Docker + Cloud Run/ECS + IaC | ง่าย |
| **B. Cloud ของลูกค้า** | subscription ลูกค้า | Terraform ในบัญชีเขา, CI จาก repo เขา | กลาง |
| **C. On-prem / air-gapped** | ไม่มีอินเทอร์เน็ต | container registry ภายใน, ถ่าย image ด้วยไฟล์, offline license | ยากมาก |

### เช็กลิสต์ก่อน deploy ที่ไหนก็ตาม
- [ ] Secrets มาจาก secret manager (ไม่ใช่ .env ใน image)
- [ ] Image ถูก tag ด้วย git SHA (ไม่ใช้ `latest`)
- [ ] มี rollback plan ที่ทดสอบแล้ว
- [ ] มี backup ของ DB + วิธี restore ที่ทดสอบแล้ว
- [ ] Health check + readiness ทำงาน
- [ ] Log ส่งออกที่ที่ทีมดูได้
- [ ] มี runbook 1 หน้า
- [ ] ใครเป็นคนกด deploy? ใครอนุมัติ?

### Rollback ที่ใช้จริง
```bash
# Cloud Run ตัวอย่าง
gcloud run services describe policy-qa --format='value(status.traffic)'
gcloud run services update-traffic policy-qa --to-revisions=policy-qa-00042-abc=100
```
> 🔑 **กฎ:** ถ้าคุณไม่เคยทดสอบ rollback แปลว่าคุณยังไม่มี rollback
> และควรทดสอบในวันแรกที่ deploy ไม่ใช่ตอนไฟไหม้

---

## 8.4 CI/CD ขั้นต่ำ

```yaml
name: ci
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with: { python-version: "3.11" }
      - run: pip install -r requirements-dev.txt
      - run: ruff check . && mypy src --ignore-missing-imports
      - run: pytest -q --cov=src --cov-fail-under=60
      - run: python evals/run.py --system prompt_v3   # eval gate
  build:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: docker build -t app:${{ github.sha }} .
```

**Quality gate ขั้นต่ำ:** lint ผ่าน, unit test ผ่าน, coverage ≥60%, eval gate ผ่าน

---

## 8.5 Monitoring, Alert, และการนอนหลับตอนกลางคืน

| Alert | เงื่อนไข | ทำอะไร |
|---|---|---|
| Service down | healthz ล้ม 2 นาที | page on-call |
| Error rate | >2% ใน 5 นาที | page |
| p95 latency | >5s ต่อเนื่อง 10 นาที | notify |
| Cost spike | >120% ของ daily avg | notify + auto-limit |
| Eval drift | quality proxy ตก 10% | notify + เก็บ sample |
| Queue backlog | >100 งานค้าง | notify |
| Dependency down | provider ตอบ error >50% | สลับ fallback อัตโนมัติ |

### Runbook 1 หน้า (ต้องมีทุก deploy)
```markdown
# Runbook: Policy QA

## ข้อมูลระบบ
- URL: ...
- Repo: ...
- Dashboard: ...
- Log: ...

## อาการที่พบบ่อย
| อาการ | สาเหตุ probable | วิธีแก้ |
|---|---|---|
| 504 ทั้งหมด | LLM provider ล่ม | ตรวจ status page, สลับ fallback ตามข้อ 3 |
| 429 จาก provider | traffic เกิน | ลด rate limit, เปิด queue |
| คำตอบไม่มี citation | retrieval คืน 0 chunk | ตรวจ index, ตรวจ metadata filter |

## วิธี rollback
...

## ใครติดต่อได้
- FDE owner: ...
- ลูกค้า IT: ...
```

---

## 8.6 ต้นทุน running (คำนวณจริง)

| รายการ | ตัวอย่าง | ต่อเดือน (USD) |
|---|---|---|
| Compute (small service 24/7) | 0.5 vCPU / 1 GB | 10–25 |
| DB (managed small) | 20 GB | 30–60 |
| Vector storage | 500k chunks | 10–40 |
| LLM calls | 60k/เดือน (RAG 6k in, 300 out) | 150–600 |
| Logging/monitoring | – | 10–30 |
| **รวม** | – | **~210–755** |

> ตัวเลขนี้ต้องอยู่ใน business case (Module 12) — และต้องระบุต้นทุน "ต่อผู้ใช้ 1 คน" ด้วย
> เพราะนั่นคือตัวเลขที่ผู้บริหารถาม

---

## 8.7 Lab (สัปดาห์ 21–23)

### Lab 8.1 (8 ชม.) — Service จริง ★
ห่อระบบจาก Module 5/6 เป็น FastAPI + Docker + healthz + timeout + fallback + rate limit

### Lab 8.2 (6 ชม.) — CI/CD
lint + test + coverage + eval gate + build image tag ด้วย SHA

### Lab 8.3 (6 ชม.) — Deploy จริง
deploy ขึ้น cloud ฟรี/low-cost 1 ที่ (Cloud Run, Fly.io, Railway, หรือ VPS)
**ต้องมี URL ที่คนอื่นเข้าถึงได้** — จะได้เอาใส่ portfolio

### Lab 8.4 (6 ชม.) — Observability + runbook
logging ตาม schema + dashboard + alert 3 ตัว + runbook 1 หน้า

### Lab 8.5 (5 ชม.) — Chaos test
จงใจทำ 5 อย่าง: ปิด DB, ทำให้ LLM timeout, ยิง request เกิน rate limit, ลบ API key, deploy image เสีย
แล้วเขียนว่า: ระบบตรวจพบอย่างไร → runbook ใช้ได้ไหม → แก้กี่นาที
**นี่คือ lab ที่จะทำให้คุณพร้อมทำงานจริงกว่าคนอื่นมาก**

### Lab 8.6 (4 ชม.) — Cost report
เขียนตารางต้นทุนต่อเดือน + ต่อผู้ใช้ + 3 วิธีลดต้นทุน 30%

---

## ✅ เกณฑ์ผ่าน

- [ ] มี service ที่มี URL สาธารณะและรันได้
- [ ] CI/CD ทำงาน (ทดสอบด้วยการแก้โค้ด 1 บรรทัดแล้วเห็น pipeline ทำงาน)
- [ ] มี eval gate ใน CI
- [ ] มี dashboard + alert ≥3 + runbook
- [ ] ทดสอบ rollback จริง ≥1 ครั้ง
- [ ] ผ่าน chaos test 5 สถานการณ์ และกู้คืนได้
- [ ] คำนวณต้นทุนต่อเดือนและต่อผู้ใช้ได้

---

**ต่อไป →** [`10-module-09-enterprise-security.md`](10-module-09-enterprise-security.md)
