# Module 3 — Python สำหรับคนที่ "ไม่เคยเขียนโปรแกรม"

⏱️ **40 ชั่วโมง (สัปดาห์ 5–8)** · 🎯 ผลลัพธ์: CLI tool ที่มี test ≥5 ตัว, อ่าน/เขียน API, จัดการ error

> **เป้าหมายไม่ใช่ "เขียน Python เก่ง"** แต่คือ **"สร้างเครื่องมือที่แก้ปัญหาของตัวเองได้"**
> FDE ไม่ได้เขียนระบบใหญ่ — FDE เขียนสคริปต์ 20 ตัวที่แต่ละตัวประหยัดเวลาคน 10 ชั่วโมง/สัปดาห์

---

## 3.1 พื้นฐานที่ต้องแน่น (สัปดาห์ 5)

### ตัวแปรและชนิดข้อมูล
```python
name: str = "สมชาย"
age: int = 34
score: float = 8.75
is_active: bool = True
tags: list[str] = ["fintech", "thai"]          # ลิสต์ (แก้ได้)
config: dict[str, int] = {"timeout": 30}       # dict (key-value)
coords: tuple[float, float] = (13.75, 100.50)  # tuple (แก้ไม่ได้)
unique: set[int] = {1, 2, 3}                   # set (ไม่ซ้ำ)
```

### Control flow
```python
if score >= 8:
    print("ดีมาก")
elif score >= 5:
    print("ผ่าน")
else:
    print("ต้องปรับ")

for i, tag in enumerate(tags, start=1):
    print(i, tag)

while config["timeout"] > 0:
    config["timeout"] -= 10
```

### Function + type hints
```python
def summarize_orders(orders: list[dict], min_amount: float = 0.0) -> dict:
    """สรุปยอดคำสั่งซื้อที่มากกว่า min_amount"""
    filtered = [o for o in orders if o["amount"] >= min_amount]
    return {
        "count": len(filtered),
        "total": sum(o["amount"] for o in filtered),
        "avg": (sum(o["amount"] for o in filtered) / len(filtered)) if filtered else 0.0,
    }
```

> **Type hints ไม่ใช่ของเล่น** — มันคือเอกสารที่ editor ใช้เตือนคุณก่อนรัน และช่วยให้ AI ช่วยคุณได้ตรงขึ้น

### Comprehension (เขียนสั้นแต่ยังอ่านรู้เรื่อง)
```python
names = [c["name"] for c in customers if c["active"]]
by_city = {c["city"]: c for c in customers}
```

---

## 3.2 จัดการข้อมูลจริง (สัปดาห์ 6)

```python
import json, csv
from pathlib import Path

# อ่าน JSON
data = json.loads(Path("input.json").read_text(encoding="utf-8"))

# เขียน JSON สวย ๆ
Path("out.json").write_text(
    json.dumps(data, ensure_ascii=False, indent=2), encoding="utf-8"
)

# CSV
with open("data.csv", newline="", encoding="utf-8") as f:
    for row in csv.DictReader(f):
        print(row["customer_id"], row["amount"])
```

### Requests — ต่อ API
```python
import requests, time

def fetch_with_retry(url: str, retries: int = 3) -> dict:
    for attempt in range(retries):
        try:
            r = requests.get(url, timeout=10)
            if r.status_code == 429:                 # rate limited
                time.sleep(2 ** attempt)             # exponential backoff
                continue
            r.raise_for_status()
            return r.json()
        except requests.RequestException as e:
            if attempt == retries - 1:
                raise
            time.sleep(2 ** attempt)
    raise RuntimeError("unreachable")
```

> 🔑 **ทักษะนี้คือ FDE จริง ๆ:** timeout + retry + backoff + ยอมแพ้อย่างสุภาพ
> ระบบที่ไม่มีสี่อย่างนี้จะพังใน production แน่นอน

### จัดการ environment / secrets
```python
import os
from dotenv import load_dotenv

load_dotenv()                     # อ่านจาก .env
API_KEY = os.environ["API_KEY"]   # ถ้าไม่มี = crash ทันที (ดี! ดีกว่าใช้ค่าว่างเงียบ ๆ)
```
**กฎเหล็ก:** `.env` ต้องอยู่ใน `.gitignore` เสมอ — เคยมี FDE ทำ key รั่วใน repo ลูกค้าแล้วจบอาชีพ

---

## 3.3 เขียนโค้ดที่คนอื่น (และอนาคตของคุณ) อ่านรู้เรื่อง

### โครงสร้างโปรเจกต์ที่ควรใช้
```
my-tool/
├── README.md
├── pyproject.toml
├── .gitignore
├── .env.example          # ตัวอย่าง ไม่มีค่าจริง
├── src/
│   └── my_tool/
│       ├── __init__.py
│       ├── cli.py
│       ├── client.py     # คุยกับ API
│       ├── transform.py  # แปลงข้อมูล
│       └── settings.py   # config
└── tests/
    ├── test_transform.py
    └── test_client.py
```

### Error handling ที่ดี
```python
class UpstreamError(Exception):
    """ระบบปลายทางตอบผิดปกติ"""

try:
    data = fetch_with_retry(url)
except requests.Timeout:
    log.error("timeout", extra={"url": url})
    raise UpstreamError(f"timeout calling {url}") from None
```
กฎ: **จับเฉพาะ error ที่คุณรู้ว่าจะทำอะไรกับมัน** — `except Exception: pass` คืออาชญากรรม

### Logging แทน print
```python
import logging
logging.basicConfig(level=logging.INFO, format="%(asctime)s %(levelname)s %(message)s")
log = logging.getLogger(__name__)
log.info("processed", extra={"rows": len(rows), "elapsed_s": 1.2})
```
เหตุผล: ใน production คุณต้อง**ค้นย้อนหลังได้**ว่าเกิดอะไรขึ้นเวลา 14:03

---

## 3.4 Test — ทักษะที่แยกมืออาชีพออกจากมือใหม่

```python
# tests/test_transform.py
from my_tool.transform import summarize_orders

def test_empty():
    assert summarize_orders([])["count"] == 0

def test_filters_by_min_amount():
    orders = [{"amount": 10}, {"amount": 100}]
    assert summarize_orders(orders, min_amount=50)["count"] == 1

def test_avg_is_zero_when_empty():
    assert summarize_orders([])["avg"] == 0.0

def test_boundary_included():
    assert summarize_orders([{"amount": 50}], min_amount=50)["count"] == 1
```

```bash
pip install pytest
pytest -q
```

> **เขียน test ก่อนหรือหลังก็ได้ — แต่ต้องมี** และควรมี **boundary case** อย่างน้อย 1 ตัวทุกฟังก์ชัน
> เพราะ FDE ต้องส่งมอบโค้ดให้ทีมลูกค้าดูแลต่อ — โค้ดที่ไม่มี test คือหนี้สินที่คุณทิ้งไว้ให้เขา

---

## 3.5 CLI ที่คนอื่นใช้ได้จริง

```python
# src/my_tool/cli.py
import argparse, sys

def main() -> int:
    p = argparse.ArgumentParser(description="สรุปยอดคำสั่งซื้อ")
    p.add_argument("input", help="path ของ CSV")
    p.add_argument("--min-amount", type=float, default=0.0)
    p.add_argument("--out", default="-", help="ไฟล์ปลายทาง (- = stdout)")
    args = p.parse_args()

    result = run(args.input, args.min_amount)
    if args.out == "-":
        print(result)
    else:
        open(args.out, "w", encoding="utf-8").write(result)
    return 0

if __name__ == "__main__":
    sys.exit(main())
```
แล้วเพิ่มใน `pyproject.toml`:
```toml
[project.scripts]
my-tool = "my_tool.cli:main"
```

**ทำไมต้องใส่ใจ CLI:** เครื่องมือที่รันด้วยคำสั่งเดียว = สิ่งที่ทีมลูกค้าดูแลต่อได้เอง
= adoption ที่ยั่งยืน (Module 11)

---

## 3.6 AI ช่วยเขียนโค้ด — ใช้ให้ถูก

| ใช้ AI ทำ | ✅ เหมาะ | ❌ อันตราย |
|---|---|---|
| อธิบาย error | ✅ ดีมาก | – |
| เขียน boilerplate | ✅ | ถ้าไม่เข้าใจแล้วส่งต่อ |
| ออกแบบ test case | ✅ | – |
| เขียน logic ธุรกิจทั้งก้อน | – | ❌ คุณจะตอบคำถามลูกค้าไม่ได้ |
| แก้โค้ดที่คุณไม่เข้าใจเลย | – | ❌ หนี้สินเงียบ |

**กฎ 3 บรรทัด:** ทุกครั้งที่ AI เขียนโค้ดให้ ให้เขียน 3 bullet ทันที
1. โค้ดนี้ทำอะไร (ภาษาคน)
2. ทำไมต้องทำแบบนี้ (ทางเลือกอื่นคืออะไร)
3. ถ้ามันพัง จะพังแบบไหน

ถ้าเขียนข้อ 3 ไม่ได้ = **ยังไม่เข้าใจ** → อย่า commit

---

## 3.7 Lab (สัปดาห์ 5–8)

### Lab 3.1 (8 ชม.) — เกมฝึกพื้นฐาน
ทำ 20 โจทย์จาก Exercism (Python track) หรือ Advent of Code ปีล่าสุด 10 ข้อแรก
**กติกา:** ทำเองก่อน 30 นาที ต่อโจทย์ แล้วค่อยดูเฉลย → เขียนว่าตัวเองพลาดตรงไหน

### Lab 3.2 (12 ชม.) — Data wrangling tool ★
สร้าง CLI ที่:
- อ่านข้อมูลจาก API หรือ CSV จริง
- ทำความสะอาด (จัดการ NULL, duplicate, format วันที่)
- คำนวณสรุป 3 ตัวชี้วัด
- ส่งออกเป็น JSON + ตาราง markdown
- มี `--dry-run`, `--verbose`, `--out`
- มี test ≥5 ตัว (รวม boundary + error case)
- มี README ที่คนอื่นรันตามได้ใน 3 นาที

### Lab 3.3 (4 ชม.) — Refactor
เลือกโค้ด Lab 3.2 มาแยกเป็น module, ใส่ type hints, เพิ่ม logging, ลบ print ออก

### Lab 3.4 (4 ชม.) — Code review
หา repo โอเพนซอร์สเล็ก ๆ 1 อัน อ่านโค้ด แล้วเขียน review 5 ข้อ (อะไรดี อะไรเสี่ยง)
— ทักษะนี้จะใช้ทุกวันในงาน FDE

### Lab 3.5 (4 ชม.) — Dockerize
ห่อ tool เป็น image แล้วให้เพื่อน/เพื่อนร่วมงานรันโดยไม่ต้องติดตั้ง Python

---

## ✅ เกณฑ์ผ่าน

- [ ] เขียนฟังก์ชันที่มี type hints + docstring ได้เอง
- [ ] อ่าน stack trace แล้วชี้บรรทัดที่พังได้
- [ ] เขียน API client ที่มี timeout/retry/backoff
- [ ] แยก module ได้ และใช้ `logging` เป็น
- [ ] เขียน pytest ≥5 ตัวและรันผ่าน
- [ ] ห่อเป็น Docker image ได้
- [ ] **ส่งงาน:** CLI tool ตาม Lab 3.2 + README + test ครบ

> ⏳ **ถ้าใช้เวลาเกิน 6 สัปดาห์** ไม่เป็นไร — แต่อย่าข้าม Lab 3.2
> เพราะทุก module หลังจากนี้จะต่อยอดจากมัน

---

**ต่อไป →** [`05-module-04-llm-engineering.md`](05-module-04-llm-engineering.md)
