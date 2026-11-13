# Module 2 — Data Literacy & SQL จากศูนย์

⏱️ **20 ชั่วโมง (สัปดาห์ 3–4)** · 🎯 ผลลัพธ์: ชุด query 20 ข้อบน dataset จริง + data contract 1 ชุด

> **ทำไมสำคัญที่สุดรองจาก AI:** FDE ทุกคนเจอคำถาม "ข้อมูลอยู่ไหน แล้วมันเชื่อถือได้ไหม"
> ระบบ AI ที่ดี 80% มาจากข้อมูลที่ดี — และการเข้าใจ data model ของลูกค้าคือ superpower ของ FDE

---

## 2.1 ภาษาของข้อมูล

| คำ | ความหมาย | ตัวอย่างในโลกจริง |
|---|---|---|
| **Table** | ตาราง = กลุ่มระเบียนชนิดเดียวกัน | `customers`, `orders` |
| **Row / record** | 1 ระเบียน | ลูกค้า 1 คน |
| **Column / field** | คุณสมบัติ | `email`, `created_at` |
| **Primary key** | ตัวระบุไม่ซ้ำ | `customer_id` |
| **Foreign key** | ชี้ไปยังตารางอื่น | `orders.customer_id → customers.id` |
| **Schema** | โครงสร้างทั้งหมด | ชื่อ+ชนิดของทุกคอลัมน์ |
| **Index** | ดัชนีทำให้ query เร็ว | เหมือนสารบัญหนังสือ |
| **Normalization** | แยกข้อมูลซ้ำออก | เก็บที่อยู่ลูกค้าที่เดียว |
| **Cardinality** | จำนวนค่าที่ไม่ซ้ำ | `status` มี 4 ค่า = low cardinality |

---

## 2.2 SQL — เริ่มจาก SELECT

```sql
SELECT customer_id, name, created_at
FROM customers
WHERE status = 'active'
ORDER BY created_at DESC
LIMIT 20;
```

### เรียงลำดับการทำงานจริง (สำคัญกว่าที่คิด)
```
FROM → JOIN → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT
```
รู้ลำดับนี้แล้วจะตอบได้ว่าทำไม alias จาก SELECT ใช้ใน WHERE ไม่ได้

### Aggregate — หาคำตอบทางธุรกิจ
```sql
SELECT
  DATE_TRUNC('month', created_at) AS month,
  COUNT(*)                        AS orders,
  SUM(amount)                     AS revenue,
  AVG(amount)                     AS avg_order_value
FROM orders
WHERE created_at >= '2026-01-01'
GROUP BY 1
HAVING SUM(amount) > 100000
ORDER BY 1;
```

### JOIN — หัวใจของงานจริง
```sql
-- ลูกค้าที่ไม่เคยสั่งซื้อ (anti-join pattern)
SELECT c.customer_id, c.name
FROM customers c
LEFT JOIN orders o ON o.customer_id = c.customer_id
WHERE o.customer_id IS NULL;
```

| Join | ได้อะไร |
|---|---|
| `INNER JOIN` | เฉพาะที่ตรงกันทั้งสองฝั่ง |
| `LEFT JOIN` | ทั้งหมดฝั่งซ้าย + ที่ตรงฝั่งขวา (ถ้าไม่มี = NULL) |
| `FULL OUTER JOIN` | ทั้งหมดทั้งสองฝั่ง |
| `CROSS JOIN` | ทุกคู่ (ระวัง! แถวระเบิด) |

### Window function — ใช้บ่อยในงาน enterprise
```sql
-- อันดับลูกค้าตามยอดใช้จ่ายภายในแต่ละไตรมาส
SELECT
  customer_id, quarter, revenue,
  RANK() OVER (PARTITION BY quarter ORDER BY revenue DESC) AS rnk,
  SUM(revenue) OVER (PARTITION BY customer_id)             AS lifetime_value
FROM quarterly_revenue;
```

### CTE — เขียน query ยาวให้อ่านได้
```sql
WITH active_customers AS (
  SELECT customer_id FROM customers WHERE status = 'active'
),
monthly AS (
  SELECT customer_id, DATE_TRUNC('month', created_at) m, SUM(amount) rev
  FROM orders GROUP BY 1, 2
)
SELECT * FROM monthly JOIN active_customers USING (customer_id);
```

---

## 2.3 กับดักของข้อมูลที่ทำให้โปรเจกต์พัง

| กับดัก | อาการ | วิธีตรวจ |
|---|---|---|
| **NULL** | `SUM` หายไป, `WHERE x != 'a'` ไม่รวมแถว NULL | ใช้ `COALESCE`, `IS DISTINCT FROM` |
| **Timezone** | ยอดขายต่างกัน 1 วัน | เก็บเป็น UTC, แปลงตอนแสดง |
| **Duplicate** | ยอดพอง | `COUNT(*)` vs `COUNT(DISTINCT id)` |
| **Soft delete** | นับข้อมูลที่ถูกลบแล้ว | ต้องมี `WHERE deleted_at IS NULL` |
| **Late arriving data** | รายงานย้อนหลังเปลี่ยน | ถามว่า "ตัวเลขเมื่อวานอาจเปลี่ยนไหม" |
| **Surrogate vs natural key** | join ผิดได้หลายแถว | ตรวจ uniqueness ก่อน join |

> 💡 **FDE ต้องถามเสมอ:** "ตัวเลขนี้ใครเป็นเจ้าของ? อัปเดตกี่โมง? มีคนแก้มือไหม?"
> (คำตอบมักเป็น "มีคนแก้มือใน Excel" — นั่นคือ requirement ที่แท้จริง)

---

## 2.4 Data Quality: ทำให้ได้คะแนน trust

สร้าง **data contract** หนึ่งหน้าให้ทุก dataset ที่ใช้:

```markdown
## Data Contract: orders

- Owner: ทีม Finance
- Refresh: ทุกวัน 03:00 UTC
- Grain: 1 แถว = 1 คำสั่งซื้อ (ไม่รวมที่ถูกยกเลิก)
- Primary key: order_id
- Nullable: discount_code, shipped_at
- Known issues: ยอดก่อน 2025-06 มาจาก legacy system (ปัดเศษต่างกัน)
- Quality checks:
  - order_id ไม่ซ้ำ
  - amount >= 0
  - created_at <= now()
  - จำนวนแถววันนี้ต่างจากเมื่อวานไม่เกิน ±20%
```

**Lab 2.3:** เขียน data contract ให้ dataset ที่คุณใช้บ่อยที่สุด

---

## 2.5 ในโลก LLM: vector & embedding

เมื่อทำ RAG (Module 5) ข้อมูลจะถูกแปลงเป็น **embedding** — vector จำนวนจริงที่แทน "ความหมาย"

```
"วิธีขอคืนสินค้า"  → [0.021, -0.118, 0.442, ...]  (เช่น 1536 มิติ)
```

- ข้อความที่ความหมายใกล้กัน → vector ใกล้กัน (วัดด้วย cosine similarity)
- **Vector DB** (pgvector, Qdrant, Pinecone) เก็บและค้น vector ได้เร็ว
- **Metadata filter** สำคัญมากในงานจริง (เช่น "เฉพาะเอกสารปี 2026 แผนก HR")

> 🔑 **ความเข้าใจที่คนพลาดบ่อย:** RAG ที่แย่ไม่ใช่เพราะ embedding แย่ แต่เพราะ
> **chunking แย่ + ไม่มี metadata + ไม่มีการวัดผล** (Module 5–7)

---

## 2.6 Lab ทั้งหมดของ Module นี้

### Lab 2.1 (6 ชม.) — SQL 20 ข้อบน dataset จริง
ใช้ dataset สาธารณะ (เช่น Chinook, NYC taxi, หรือข้อมูลเปิดของไทย) ทำ query ต่อไปนี้ เขียนคำตอบเป็นไฟล์ `.sql` พร้อม comment:

1. นับจำนวนแถวทั้งหมดของแต่ละตาราง
2. หา top 10 ลูกค้าตามยอดใช้จ่าย
3. หายอดขายรายเดือน 12 เดือนล่าสุด
4. หาลูกค้าที่ไม่มีการสั่งซื้อใน 90 วัน
5. หาค่าเฉลี่ย/มัธยฐานของมูลค่าคำสั่งซื้อ
6. หาวันที่ยอดขายสูงสุด
7. นับผู้ใช้ใหม่รายเดือน (cohort)
8. หาอัตราการกลับมาซื้อซ้ำ (retention)
9. หาสินค้าที่ขายดีแต่กำไรต่ำ
10. join 3 ตารางขึ้นไป
11. ใช้ window function หาอันดับภายในกลุ่ม
12. ใช้ `LAG`/`LEAD` หาความต่างจากแถวก่อน
13. หา duplicate ที่ไม่ควรมี
14. หาแถวที่ละเมิด business rule
15. เขียน CTE ที่อ่านง่าย 3 ชั้น
16. ทำ report สรุป 1 หน้าให้ผู้บริหาร (ภาษาไทย)
17. หา data quality issue 3 อย่างใน dataset
18. อธิบายว่าถ้ามีข้อมูล 100 ล้านแถว query ที่ช้าที่สุดของคุณจะเป็นอะไร และจะแก้อย่างไร (index? partition?)
19. เขียน query ที่ให้ผลเหมือนกันแต่อ่านง่ายกว่าเดิม
20. เขียน view หรือ materialized table ที่ทีมอื่นใช้ซ้ำได้

### Lab 2.2 (2 ชม.) — ตั้งฐานข้อมูลเอง
```bash
docker run --rm -d --name pg -e POSTGRES_PASSWORD=dev -p 5432:5432 postgres:16
docker exec -it pg psql -U postgres
```
โหลดข้อมูลเข้าไป แล้วรัน query เดิมซ้ำ — เพื่อให้เข้าใจว่า "production database" ไม่ใช่ Excel

### Lab 2.3 (2 ชม.) — Data contract ตามหัวข้อ 2.4

---

## ✅ เกณฑ์ผ่าน

- [ ] เขียน JOIN (inner/left/anti-join) ได้โดยไม่ต้องเปิดเฉลย
- [ ] ใช้ `GROUP BY`, `HAVING`, window function ได้
- [ ] ระบุ data quality issue ได้ 3 อย่างใน dataset จริง
- [ ] เขียน data contract ได้
- [ ] ส่ง query ครบ 20 ข้อ + report ผู้บริหาร 1 หน้า
- [ ] **ตอบได้:** "ถ้าตัวเลขนี้ผิด คุณจะรู้ได้อย่างไร" (คำตอบต้องไม่ใช่ "ก็ดูแล้วมันถูก")

---

**ต่อไป →** [`04-module-03-python.md`](04-module-03-python.md)
