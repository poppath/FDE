# Assessment Rubrics — ให้คะแนนตัวเองทุก Module

ใช้ 3 ระดับต่อข้อ: **0 = ยังไม่ทำ · 1 = ทำได้ถ้ามีคนช่วย/เปิดโน้ต · 2 = ทำได้เองโดยไม่เปิดอะไร**

> เป้าหมายไม่ใช่ "คะแนนสูง" แต่คือ **หาจุดอ่อนให้เจอเร็ว** เพื่อซ่อมก่อนเดินต่อ

---

## Module 0 — Orientation (เต็ม 8)
- [ ] อธิบายความต่าง FDE vs Solutions Engineer ได้ (2)
- [ ] มีผล self-assessment + ตีความถูก (2)
- [ ] เลือก vertical พร้อมเหตุผล 3 ข้อ (2)
- [ ] มี baseline project ที่มีตัวเลข (2)

**ผ่าน ≥6**

---

## Module 1 — Software & Internet (เต็ม 16)
- [ ] อธิบาย DNS → TLS → HTTP → render ได้ครบ (2)
- [ ] อ่าน status code แล้วรู้ว่าต้องทำอะไร (2)
- [ ] เรียก API ด้วย curl + จัดการ pagination (2)
- [ ] debug 401/403/429 ได้เอง (2)
- [ ] Git: branch, commit, PR, แก้ conflict (2)
- [ ] อธิบาย compute/storage/DB/secrets/region (2)
- [ ] เขียน Dockerfile และรันได้ (2)
- [ ] ประเมินต้นทุน cloud คร่าว ๆ ได้ (2)

**ผ่าน ≥12** — ถ้าข้อไหนได้ 0 ให้กลับไปทำ lab ของหัวข้อนั้น

---

## Module 2 — Data & SQL (เต็ม 16)
- [ ] INNER/LEFT join + anti-join (2)
- [ ] GROUP BY/HAVING + aggregate (2)
- [ ] window function (RANK, SUM OVER) (2)
- [ ] CTE อ่านง่าย 3 ชั้น (2)
- [ ] ระบุ NULL/timezone/duplicate trap (2)
- [ ] เขียน data contract (2)
- [ ] ระบุ data quality issue 3 อย่างได้ (2)
- [ ] ทำ report ผู้บริหาร 1 หน้า (2)

**ผ่าน ≥12**

---

## Module 3 — Python (เต็ม 20)
- [ ] เขียน function + type hints + docstring (2)
- [ ] จัดการ JSON/CSV/Path (2)
- [ ] API client + timeout + retry + backoff (3)
- [ ] error handling ที่ไม่กลืน error (2)
- [ ] logging แทน print (2)
- [ ] โครงโปรเจกต์ + module แยก (2)
- [ ] pytest ≥5 ตัว + boundary case (3)
- [ ] CLI ด้วย argparse + exit code (2)
- [ ] Dockerize (2)

**ผ่าน ≥15** — ถ้า <15 ห้ามไป Module 4 (จะเจ็บตัวหนัก)

---

## Module 4 — LLM Engineering (เต็ม 18)
- [ ] อธิบาย token/context/temperature (2)
- [ ] structured output ≥95% (3)
- [ ] prompt versioning + เหตุผล (2)
- [ ] wrapper ที่มี retry/timeout/log (3)
- [ ] คำนวณต้นทุนต่อเดือน (2)
- [ ] ระบุ failure mode 6 อย่าง (2)
- [ ] เปรียบเทียบ 2 โมเดลด้วย eval (4)

**ผ่าน ≥13** — ข้อสุดท้ายสำคัญที่สุด

---

## Module 5 — RAG (เต็ม 20)
- [ ] อธิบาย pipeline ครบ (2)
- [ ] เลือก chunking + เหตุผล (3)
- [ ] hybrid search + rerank (3)
- [ ] metadata ที่จำเป็น (2)
- [ ] golden set ≥30 ข้อ (3)
- [ ] วัด Recall@5 (3)
- [ ] citation ตรวจสอบได้ (2)
- [ ] ACL/สิทธิ์ (2)

**ผ่าน ≥15**

---

## Module 6 — Agents (เต็ม 18)
- [ ] tool ตาม checklist (3)
- [ ] สิทธิ์ผูกผู้ใช้ (3)
- [ ] guardrails ครบ 7 (3)
- [ ] audit log (2)
- [ ] ผ่านการโจมตี ≥13/15 (4)
- [ ] วัด task success rate (2)
- [ ] escalation table (1)

**ผ่าน ≥14**

---

## Module 7 — Eval & Observability ★ (เต็ม 20)
- [ ] eval set ≥50 ครบ 5 ประเภท (4)
- [ ] judge validated ด้วยคน ≥80% (3)
- [ ] harness รันด้วยคำสั่งเดียว (3)
- [ ] รายงานแยก tag (2)
- [ ] CI gate (3)
- [ ] trace + dashboard + alert (3)
- [ ] feedback loop → eval case ใหม่ (2)

**ผ่าน ≥16** — นี่คือ module ที่สำคัญที่สุดในคอร์ส

---

## Module 8 — Deploy (เต็ม 18)
- [ ] service มี URL (3)
- [ ] healthz + timeout + fallback + rate limit (3)
- [ ] CI/CD (3)
- [ ] eval gate ใน CI (2)
- [ ] dashboard + alert + runbook (3)
- [ ] rollback ทดสอบแล้ว (2)
- [ ] chaos test 5 สถานการณ์ (2)

**ผ่าน ≥14**

---

## Module 9 — Enterprise & Security (เต็ม 22)
- [ ] ตอบ 12 คำถาม CISO (5)
- [ ] security pack 14 ไฟล์ (4)
- [ ] threat model + indirect injection test (4)
- [ ] PDPA/DPIA (3)
- [ ] SSO + RBAC พิสูจน์ได้ (3)
- [ ] เปรียบเทียบ deploy 3 แบบด้วยตัวเลข (3)

**ผ่าน ≥17**

---

## Module 10 — Discovery ★ (เต็ม 18)
- [ ] shadow งานจริง (3)
- [ ] ตัวเลข baseline จากวัดจริง (4)
- [ ] problem statement มีตัวเลข ≥3 (3)
- [ ] opportunity assessment 5×5 (3)
- [ ] design doc + สมมติฐานที่ต้องพิสูจน์ (3)
- [ ] mock discovery + retrospective (2)

**ผ่าน ≥14**

---

## Module 11 — Delivery & Adoption ★ (เต็ม 20)
- [ ] delivery plan M1–M6 (3)
- [ ] vertical slice ครบ (4)
- [ ] ผู้ใช้จริง ≥5 คน (4)
- [ ] adoption metrics 4 สัปดาห์ (3)
- [ ] handover pack + วิดีโอ (4)
- [ ] change management ที่มีชื่อ champion (2)

**ผ่าน ≥16**

---

## Module 12 — Commercial (เต็ม 14)
- [ ] ROI model ใช้ซ้ำได้ (3)
- [ ] business case 1 หน้า (3)
- [ ] พิตช์ 10 นาทีผ่าน non-technical (3)
- [ ] แปลงตัวเลข 10 ตัว (3)
- [ ] product feedback memo (2)

**ผ่าน ≥11**

---

## Module 13 — Capstone (เต็ม 100) — ดูที่ [`14-module-13-capstone.md`](14-module-13-capstone.md) §13.6

---

## Module 14 — Positioning (เต็ม 14)
- [ ] portfolio 3 ชิ้น (3)
- [ ] README ตอบ 5 ข้อ (2)
- [ ] CV ตัวเลขทุกบรรทัด (2)
- [ ] "ทำไม FDE" 3 ระดับ (2)
- [ ] mock interview 3 รอบ (3)
- [ ] สมัคร ≥15 ที่ (2)

**ผ่าน ≥11**

---

## ตารางติดตามภาพรวม (กรอกเอง)

| Module | เต็ม | ได้ | วันที่ | หมายเหตุ / จุดที่ต้องซ่อม |
|---|---|---|---|---|
| 0 | 8 |  |  |  |
| 1 | 16 |  |  |  |
| 2 | 16 |  |  |  |
| 3 | 20 |  |  |  |
| 4 | 18 |  |  |  |
| 5 | 20 |  |  |  |
| 6 | 18 |  |  |  |
| 7 | 20 |  |  |  |
| 8 | 18 |  |  |  |
| 9 | 22 |  |  |  |
| 10 | 18 |  |  |  |
| 11 | 20 |  |  |  |
| 12 | 14 |  |  |  |
| 13 | 100 |  |  |  |
| 14 | 14 |  |  |  |
| **รวม** | **342** |  |  |  |

**เกณฑ์:** ≥80% = expert-ready · 65–79% = พร้อมสมัครตำแหน่ง adjacent · <65% = ทำเพิ่ม
