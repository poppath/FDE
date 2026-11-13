# Module 12 — Commercial & Executive Communication

⏱️ **20 ชั่วโมง (สัปดาห์ 36–38)** · 🎯 ผลลัพธ์: business case + ROI model + พิตช์ 10 นาที

> **ความจริง:** งบประมาณไม่ได้มาจาก "เทคโนโลยีน่าสนใจ" แต่มาจาก "ตัวเลขที่ CFO เชื่อ"
> FDE ที่คำนวณ ROI เป็น = คนที่ได้ทำงานต่อเนื่อง ในขณะที่คนอื่นตกงานเมื่อโปรเจกต์แรกจบ

---

## 12.1 Business Case ที่ CFO ยอมรับ

```
┌─────────────────────────────────────────────────┐
│ 1. ปัญหา (ตัวเลข)        ← ต้นทุนของสถานะปัจจุบัน  │
│ 2. ทางแก้ (สั้น)          ← 1 ย่อหน้า              │
│ 3. ผลลัพธ์คาดหวัง (ตัวเลข) ← conservative / base / optimistic │
│ 4. ต้นทุน (ครบ)           ← build + run + people + change mgmt │
│ 5. ROI + payback period  ← ต่ำสุดที่แย่สุด           │
│ 6. ความเสี่ยง + มาตรการ    ← ต้องมี             │
│ 7. ทางเลือก               ← รวม "ไม่ทำอะไร"        │
└─────────────────────────────────────────────────┘
```

### ต้นทุนที่คนมักลืม (และ CFO ไม่ลืม)
| รายการ | ตัวอย่าง |
|---|---|
| Build | คน × เดือน × rate |
| Running | cloud + LLM + monitoring |
| **People** | คนดูแลหลังส่งมอบ (มักลืม! 20–30% ของต้นทุนทั้งหมด) |
| **Change management** | training, เวลาที่คนต้องเรียนรู้ |
| **Opportunity cost** | เวลาทีมที่ต้องมาร่วม UAT |
| **Compliance** | security review, audit, DPIA |
| **Contingency** | +15–20% |

---

## 12.2 คำนวณ ROI ให้ถูก (และซื่อสัตย์)

```python
# roi_model.py
def roi(
    volume_per_month: int,          # จำนวนงาน/เดือน
    minutes_before: float,          # เวลาก่อน (วัดจริง)
    minutes_after: float,           # เวลาหลัง (รวมเวลา review)
    error_rate_before: float,       # % ผิด
    error_rate_after: float,
    cost_per_error_thb: float,      # ต้นทุนแก้ 1 ครั้ง
    hourly_rate_thb: float,         # ค่าแรงต่อชั่วโมง
    adoption: float,                # % ที่ใช้จริง (สำคัญมาก!)
    build_cost_thb: float,
    monthly_run_cost_thb: float,
    months: int = 36,
):
    saved_min = volume_per_month * adoption * (minutes_before - minutes_after)
    error_saved = volume_per_month * adoption * (error_rate_before - error_rate_after) \
                  * cost_per_error_thb
    labor_saved = (saved_min / 60) * hourly_rate_thb

    monthly_benefit = labor_saved + error_saved
    monthly_cost = monthly_run_cost_thb + build_cost_thb / months
    monthly_net = monthly_benefit - monthly_cost

    payback_months = build_cost_thb / monthly_benefit if monthly_benefit > 0 else None
    return {
        "monthly_benefit_thb": round(monthly_benefit),
        "monthly_cost_thb": round(monthly_cost),
        "monthly_net_thb": round(monthly_net),
        "annual_net_thb": round(monthly_net * 12),
        "roi_pct_3y": round((monthly_net * months / (build_cost_thb + monthly_run_cost_thb*months)) * 100),
        "payback_months": round(payback_months, 1) if payback_months else None,
    }

print(roi(
    volume_per_month=420, minutes_before=12, minutes_after=4,
    error_rate_before=0.06, error_rate_after=0.02,
    cost_per_error_thb=25*400/60, hourly_rate_thb=400,
    adoption=0.7, build_cost_thb=450_000, monthly_run_cost_thb=18_000,
))
```

**ผลลัพธ์จริงจากการรัน (ตัวเลขในคอร์สนี้คำนวณจริง ไม่ใช่สมมติ):**
```
scenario                       benefit/เดือน   cost/เดือน   net/เดือน   payback
─────────────────────────────────────────────────────────────────────────────
base (adoption 70%, run 18k)       17,640       30,500     -12,860      ไม่คืนทุน
adoption 90%                       22,680       30,500      -7,820      19.8 เดือน
+ run 10k + build 300k             22,680       18,333      +4,347      13.2 เดือน ✅
```
> 🎯 **นี่คือคุณค่าของแบบฝึกหัดนี้:** โปรเจกต์ "ดูมีประโยชน์" กลับ**ติดลบ**เมื่อรวมต้นทุนครบ
> ต้องปรับ 3 อย่างพร้อมกัน (adoption, build cost, running cost) จึงจะคุ้ม
> — และการคุยเรื่องนี้กับลูกค้าคือบทสนทนาระดับ FDE expert

### ปรับจนคุ้ม: 4 ทางที่ใช้จริง
1. **เพิ่ม adoption** (0.7 → 0.9) — ถูกที่สุด ได้ผลมากที่สุด
2. **ลด build cost** — ใช้ component ที่มีอยู่
3. **ลด running cost** — caching, รุ่นเล็กลง, batch
4. **ขยาย scope** — ใช้ระบบเดียวกับงานที่คล้ายกัน (ถ้าคุณภาพผ่าน!)

---

## 12.3 ตีความตัวเลขให้เป็นภาษาธุรกิจ

| ตัวเลขเทคนิค | ภาษาที่ผู้บริหารเข้าใจ |
|---|---|
| "accuracy 94%" | "จาก 100 งาน มี 6 ที่ต้องแก้ด้วยคน (จากเดิม 25)" |
| "p95 latency 1.9s" | "ผู้ใช้รอไม่เกิน 2 วินาที ใน 95% ของครั้ง" |
| "ลด token ได้ 40%" | "ประหยัด 6,800 บาท/เดือน" |
| "adoption 72%" | "พนักงาน 216 จาก 300 คนใช้เป็นประจำ" |
| "recall@5 = 0.88" | "ระบบหาข้อมูลที่ต้องการเจอใน 88% ของคำถาม" |
| "eval score 0.84" | "คุณภาพผ่านเกณฑ์ภายใน 84% (เป้า 85%) — อีก 2 สัปดาห์ถึงเป้า" |

> 💡 **กฎเหล็ก:** ทุกตัวเลขเทคนิคต้องแปลงเป็น "งาน/เวลา/เงิน" ก่อนขึ้นสไลด์ผู้บริหาร

---

## 12.4 พิตช์ 10 นาที (โครงที่ใช้ได้ทุกที่)

```
นาที 0–1   ปัญหา 1 ประโยค + ตัวเลขที่ shocking
นาที 1–3   วันนี้เกิดอะไรขึ้น (เล่าเรื่องคนจริง 1 คน)
นาที 3–5   ทางแก้ + demo 30 วินาที (ของจริง ไม่ใช่สไลด์)
นาที 5–7   ผลลัพธ์ที่วัดได้ + หลักฐาน (eval, pilot data)
นาที 7–8   ต้นทุน + ROI + payback
นาที 8–9   ความเสี่ยง + มาตรการ (พูดเองก่อนถูกถาม)
นาที 9–10  สิ่งที่ต้องการตัดสินใจวันนี้ (ask ที่ชัดเจน)
```

**สิ่งที่ต้องมีในสไลด์:**
- 1 สไลด์ = 1 ข้อความ
- ตัวเลขตัวใหญ่
- ไม่มี bullet เกิน 5 บรรทัด
- ไม่มีศัพท์เทคนิคที่ไม่จำเป็น (หรืออธิบายในวงเล็บ)

**ask ที่ดี vs แย่:**
- ❌ "อยากให้สนับสนุนโปรเจกต์นี้"
- ✅ "ขออนุมัติ 450,000 บาท สำหรับ 12 สัปดาห์ และขอให้คุณสมชายเป็น sponsor — ตัดสินใจภายใน 20 พ.ย. เพื่อทันงบไตรมาส"

---

## 12.5 ทำงานกับ Sales และ Product

| บทบาท | FDE ช่วยอะไร | สิ่งที่ FDE ต้องการจากเขา |
|---|---|---|
| **Sales / AE** | technical credibility, demo จริง, ตอบ IT review | เข้าถึง decision maker, ข้อมูล budget, timeline |
| **Product** | field insights, feature gap, real use case | prioritize สิ่งที่ลูกค้าหลายรายต้องการ |
| **Customer Success** | handover, playbook, escalation path | feedback หลังส่งมอบ, renewal signal |
| **Legal/Security** | เอกสาร, threat model | ตีความสัญญา, อนุมัติ DPA |

### Feedback loop กลับ product (สิ่งที่ FDE ระดับ senior ทำ)
```
pattern ที่เจอ 3 ลูกค้าขึ้นไป → เขียน 1 หน้า:
- ปัญหาคืออะไร (มีหลักฐานจาก 3 ลูกค้า)
- workaround ที่เราทำอยู่ (ต้นทุนเท่าไร)
- ถ้า product รองรับ จะประหยัดเท่าไรต่อลูกค้า 1 ราย
- priority ที่แนะนำ
```
> นี่คือสิ่งที่ทำให้ FDE มีอิทธิพลต่อ roadmap และกลายเป็นคนที่บริษัทขาดไม่ได้

---

## 12.6 Lab (สัปดาห์ 36–38)

### Lab 12.1 (6 ชม.) — ROI model ★
- เขียน `roi_model.py` ที่ใช้ซ้ำได้
- ใส่ตัวเลขจริงจาก capstone ของคุณ
- ทำ 3 scenario: conservative / base / optimistic
- **ถ้า ROI ติดลบ ให้เขียนว่า "จะปรับอะไรเพื่อให้คุ้ม"**

### Lab 12.2 (4 ชม.) — Business case 1 หน้า
ตามโครง 12.1 ใช้ภาษาเดียวกับ CFO (ไม่ใช่ภาษา engineer)

### Lab 12.3 (4 ชม.) — พิตช์ 10 นาที
ทำสไลด์ 8–10 แผ่น + ซ้อม 3 ครั้ง (อัดวิดีโอ)
ให้คนที่ไม่ใช่สายเทคนิคฟังแล้วถาม: "เข้าใจไหม? ซื้อไหม? ไม่ซื้อเพราะอะไร"

### Lab 12.4 (3 ชม.) — แปลงตัวเลขเทคนิคเป็นภาษาธุรกิจ
เอาตัวเลข 10 ตัวจาก eval ของคุณ แปลงเป็นภาษาผู้บริหาร (ตาราง 2 คอลัมน์)

### Lab 12.5 (3 ชม.) — Product feedback memo
เขียน 1 หน้าเรื่อง pattern ที่ควรกลายเป็น feature ของ product

---

## ✅ เกณฑ์ผ่าน

- [ ] มี ROI model ที่ใช้ซ้ำได้และคำนวณจากตัวเลขจริง
- [ ] มี business case 1 หน้า
- [ ] พิตช์ 10 นาทีผ่านการทดสอบกับคน non-technical และได้ feedback
- [ ] แปลงตัวเลขเทคนิคเป็นภาษาธุรกิจได้ 10 ตัว
- [ ] มี product feedback memo
- [ ] **ตอบได้:** "ถ้า ROI ติดลบ คุณจะพูดกับลูกค้าว่าอะไร" (คำตอบต้องไม่ใช่ "ก็ไม่ทำ")

---

**ต่อไป →** [`14-module-13-capstone.md`](14-module-13-capstone.md)
